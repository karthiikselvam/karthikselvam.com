---
title: "Interview Preparation"
description: "2023"
date: "2023-01-20"
tags:
- fundamentals
---

# PostgreSQL WAL, Replication, and Replication Slots: A Deep Technical Dive

PostgreSQL’s reliability and advanced replication features are built on its Write-Ahead Log (WAL) and the mechanisms that manage it. In this post, we’ll go deep into how WAL works, how physical and logical replication use it, and how replication slots ensure data safety and consistency. We’ll also look at the actual C structures and functions that implement these features.

---

## 1. Write-Ahead Log (WAL): The Foundation

### What is WAL?

WAL is a sequential log of all changes made to the database. The core rule:  
**No change is made to the data files until the change is safely written to the WAL.**

### How WAL is Written

When a transaction modifies data:

1. **WAL Record Creation:**  
   Each change (insert, update, delete) generates a WAL record.
2. **WAL Buffering:**  
   WAL records are first written to an in-memory buffer.
3. **WAL Flush:**  
   On commit, WAL records are flushed to disk (`pg_wal/` directory) using `XLogFlush()`.

**Relevant code (simplified):**
```c
// filepath: src/backend/access/transam/xlog.c
void
XLogFlush(XLogRecPtr record)
{
    // Ensures all WAL up to 'record' is written and fsync'd to disk
}
```

### WAL Segments and LSN

- WAL is stored in segment files (default 16MB each).
- Each WAL record has a Log Sequence Number (LSN), a unique pointer to its position in the log.

---

## 2. WAL Replay: Crash Recovery and Standby Sync

### On the Primary

- WAL is both written and replayed as part of normal operation.
- Crash recovery uses WAL to bring data files up to date.

### On the Standby

- Standby receives WAL from the primary (via streaming or file shipping).
- Standby writes WAL to its own `pg_wal/`.
- Standby **replays** WAL to apply changes to its data files.

**Key functions:**
```c
// filepath: src/backend/access/transam/xlog.c
XLogRecPtr GetXLogReplayRecPtr(TimeLineID *replayTLI);
```

---

## 3. Physical Replication: Streaming WAL

### How It Works

- Standby connects to primary using a replication protocol.
- Primary sends WAL records as soon as they are written to disk.
- Standby writes and replays WAL.

**Key function for sending WAL:**
```c
// filepath: src/backend/replication/walsender.c
void XLogSendPhysical(void)
{
    // Reads WAL from disk and sends to standby
}
```

### WAL Positions

- **Received:** WAL written to disk on standby.
- **Replayed:** WAL applied to data files on standby.

**Code to get these positions:**
```c
// filepath: src/backend/replication/walreceiver.c
XLogRecPtr GetWalRcvFlushRecPtr(TimeLineID *receiveTLI);
```

---

## 4. Logical Replication: Decoding WAL

### How It Works

- WAL is written and replayed on the source.
- Logical decoding reads WAL and interprets it as logical changes (row-level).
- Changes are sent to subscribers, which apply them at the SQL level.

**Logical decoding context:**
```c
// filepath: src/include/replication/logical.h
typedef struct LogicalDecodingContext LogicalDecodingContext;
```

**Sending logical changes:**
```c
// filepath: src/backend/replication/walsender.c
void XLogSendLogical(void)
{
    // Decodes and sends logical changes to subscriber
}
```

### WAL Position for Logical Replication

- Logical decoding can only process WAL that has been **replayed** (applied).
- This ensures the database state is consistent for decoding.

---

## 5. Replication Slots: WAL Retention and Safety

### What is a Replication Slot?

A replication slot is a persistent object that tells PostgreSQL to retain WAL files until a replica (physical or logical) has processed them.

### Slot Types

- **Physical slots:** Used for physical replication.
- **Logical slots:** Used for logical replication.

### Slot Structures

**On-disk and in-memory representation:**
```c
// filepath: src/include/replication/slot.h
typedef struct ReplicationSlotPersistentData
{
    NameData    name;
    Oid         database;
    ReplicationSlotPersistency persistency;
    TransactionId xmin;
    TransactionId catalog_xmin;
    XLogRecPtr   restart_lsn;
    ReplicationSlotInvalidationCause invalidated;
    XLogRecPtr   confirmed_flush;
    // ... more fields
} ReplicationSlotPersistentData;

typedef struct ReplicationSlot
{
    slock_t     mutex;
    bool        in_use;
    pid_t       active_pid;
    bool        just_dirtied;
    bool        dirty;
    TransactionId effective_xmin;
    TransactionId effective_catalog_xmin;
    ReplicationSlotPersistentData data;
    LWLock      io_in_progress_lock;
    ConditionVariable active_cv;
    // ... more fields for logical slots
} ReplicationSlot;
```

### Slot Creation and Management

**Creating a physical slot:**
```sql
SELECT * FROM pg_create_physical_replication_slot('myslot');
```

**Creating a logical slot:**
```sql
SELECT * FROM pg_create_logical_replication_slot('myslot', 'pgoutput');
```

**Dropping a slot:**
```sql
SELECT pg_drop_replication_slot('myslot');
```

**C functions:**
```c
// filepath: src/backend/replication/slot.c
void ReplicationSlotCreate(const char *name, bool db_specific,
                          ReplicationSlotPersistency persistency,
                          bool two_phase, bool failover, bool synced);

void ReplicationSlotDrop(const char *name, bool nowait);
```

### How Slots Work

- Each slot tracks the oldest WAL LSN needed by the replica.
- The server will **not remove** WAL segments older than this LSN.
- As the replica processes WAL, it reports progress, and the slot’s LSN advances.
- Slots are stored in `pg_replslot/` and in shared memory.

**Advancing slot LSN:**
```c
// filepath: src/backend/replication/slot.c
void ReplicationSlotSave(void)
{
    // Saves slot state to disk
}
```

---

## 6. Monitoring and Troubleshooting

**View all slots:**
```sql
SELECT * FROM pg_replication_slots;
```

**Check WAL retention:**
```sql
SELECT slot_name, restart_lsn, confirmed_flush_lsn FROM pg_replication_slots;
```

**Potential issues:**
- If a slot is not advancing (replica is offline or lagging), WAL files will accumulate, possibly filling up disk space.
- Always drop unused slots to allow WAL cleanup.

---

## 7. Example: Creating and Using a Logical Replication Slot

**Step 1: Create a publication on the primary**
```sql
CREATE PUBLICATION mypub FOR TABLE mytable;
```

**Step 2: Create a logical slot**
```sql
SELECT * FROM pg_create_logical_replication_slot('myslot', 'pgoutput');
```

**Step 3: On the subscriber, create a subscription**
```sql
CREATE SUBSCRIPTION mysub
  CONNECTION 'host=primary ...'
  PUBLICATION mypub;
```

**Step 4: Monitor slot progress**
```sql
SELECT * FROM pg_replication_slots WHERE slot_name = 'myslot';
```

---

## 8. Key Macros and Utilities

**Check slot type:**
```c
#define SlotIsPhysical(slot) ((slot)->data.database == InvalidOid)
#define SlotIsLogical(slot) ((slot)->data.database != InvalidOid)
```

**Compute required LSN for all slots:**
```c
XLogRecPtr ReplicationSlotsComputeRequiredLSN(void);
```

**Compute logical restart LSN:**
```c
XLogRecPtr ReplicationSlotsComputeLogicalRestartLSN(void);
```

---

## 9. Summary Table

| Feature                | Physical Replication         | Logical Replication           |
|------------------------|-----------------------------|-------------------------------|
| Data sent              | Raw WAL records             | Decoded logical changes       |
| When can send          | As soon as WAL is received  | Only after WAL is replayed    |
| Slot type              | Physical                    | Logical                       |
| Subscriber applies     | At storage level            | At SQL/table level            |
| Key functions          | `XLogSendPhysical`, `GetWalRcvFlushRecPtr` | `XLogSendLogical`, `LogicalDecodingContext` |
| Slot struct            | `ReplicationSlot`           | `ReplicationSlot`             |

---

## 10. Conclusion

PostgreSQL’s WAL and replication system is both powerful and complex. WAL ensures durability and crash safety, while replication slots guarantee that no replica misses any changes. Understanding the difference between physical and logical replication, and how slots manage WAL retention, is crucial for database reliability and scaling.

If you want to go even deeper, explore the PostgreSQL source code in the files and functions referenced above. This will give you a true understanding of how these features are implemented.

---

**Further Reading:**
- [PostgreSQL WAL Internals](https://www.postgresql.org/docs/current/wal-intro.html)
- [Streaming Replication](https://www.postgresql.org/docs/current/warm-standby.html)
- [Logical Replication](https://www.postgresql.org/docs/current/logical-replication.html)
- [Replication Slots](https://www.postgresql.org/docs/current/warm-standby.html#STREAMING-REPLICATION-SLOTS)

---


