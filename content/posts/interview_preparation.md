---
title: "Understanding PostgreSQL’s Write-Ahead Logging (WAL)"
description: "2023"
date: "2025-05-22"
tags:
- postgres
---

PostgreSQL’s Write-Ahead Logging (WAL) is at the heart of its durability and crash recovery. If you’ve ever wondered how PostgreSQL ensures your data is safe—even in the event of a crash—this post will walk you through the architecture, flow, and the actual source code that makes it all work.

---

## High-Level Architecture & Flow of WAL

### What is WAL?

WAL is a mechanism that ensures all changes to the database are first recorded in a log before being applied to the data files. This guarantees that, even if the system crashes, PostgreSQL can recover to a consistent state.

### Key Components

- **WAL Buffers:**  
  In-memory buffers that temporarily hold WAL records before they’re written to disk.
- **WAL Files:**  
  On-disk files (in `pg_wal/`), typically 16MB each, storing the WAL records.
- **WAL Writer Process:**  
  A background process that flushes WAL buffers to disk.
- **Checkpointer:**  
  Ensures data files are consistent with WAL.
- **Archiver:**  
  Optionally archives completed WAL segments for point-in-time recovery (PITR).

### Sequence of Events

1. **Change Initiation:**  
   A transaction modifies data (e.g., an `INSERT`).
2. **WAL Record Creation:**  
   The change is encoded as a WAL record in memory.
3. **WAL Buffering:**  
   The WAL record is placed in the WAL buffers.
4. **WAL Flush:**  
   Before a transaction commits, its WAL records are flushed to disk.
5. **WAL File Management:**  
   WAL files are rotated, archived, and recycled as needed.
6. **Crash Recovery:**  
   On restart after a crash, WAL is replayed to bring the database to a consistent state.

---

## Mapping WAL to the PostgreSQL Source Code

Let’s walk through the main code files and functions for each component.

### 1. WAL Record Creation

- **File:** xlog.c
- **Key Struct:** `XLogRecord`
- **Key Functions:**
    - `XLogInsert()`: Called whenever a change is made (e.g., tuple insert/update/delete). Constructs a WAL record and appends it to the WAL buffers.
    - `XLogRegisterData()`, `XLogRegisterBuffer()`: Used by lower-level code to register data and buffers that should be included in the WAL record.

### 2. WAL Buffering and Flushing

- **File:** xlog.c
- **Key Struct:** `XLogCtlData` (shared memory control structure for WAL)
- **Key Functions:**
    - `XLogWrite()`: Flushes WAL buffers to disk.
    - `XLogFlush()`: Ensures that WAL up to a certain point is safely on disk (called before commit).
    - `XLogBackgroundFlush()`: Used by the WAL writer background process.

### 3. WAL Writer Process

- **File:** walwriter.c
- **Key Function:**  
    - `WalWriterMain()`: Main loop for the WAL writer background process, periodically flushing WAL buffers.

### 4. WAL File Management

- **File:** xlog.c
- **Key Functions:**
    - `XLogFileInit()`, `XLogFileOpen()`, `XLogFileClose()`: Manage creation, opening, and closing of WAL segment files.
    - `XLogFileName()`: Generates the filename for a given WAL segment.

### 5. Crash Recovery

- **File:** xlog.c
- **Key Function:**  
    - `StartupXLOG()`: Main function for crash recovery; reads and replays WAL records.

### 6. Archiving

- **File:** xlogarchive.c
- **Key Functions:**  
    - `XLogArchiveNotify()`, `XLogArchiveCheckDone()`

---

## Important Structs, Macros, and Configurations

- **`XLogRecPtr`**: 64-bit pointer to a WAL location.
- **`XLogRecord`**: Struct representing a single WAL record.
- **`XLogCtlData`**: Shared memory structure for WAL state.

**Configuration Parameters (in `postgresql.conf`):**
- `wal_level`
- `wal_buffers`
- `wal_writer_delay`
- `archive_mode`, `archive_command`
- `max_wal_size`, `min_wal_size`

---

## Step-by-Step Exploration

1. **WAL Record Creation:**  
   Start in `xlog.c` with `XLogInsert()`. See how a WAL record is constructed and added to the buffer. Explore how `XLogRegisterData()` and `XLogRegisterBuffer()` are used to build the record.

2. **WAL Buffering and Flushing:**  
   Follow `XLogWrite()` and `XLogFlush()`. See how WAL buffers are managed and flushed to disk. Understand the role of `XLogCtlData`.

3. **WAL Writer Process:**  
   Look at `WalWriterMain()` in `walwriter.c`. See how the background process periodically flushes WAL.

4. **WAL File Management:**  
   Explore `XLogFileInit()`, `XLogFileOpen()`, etc. See how WAL files are created, opened, and rotated.

5. **Crash Recovery:**  
   Study `StartupXLOG()`. See how WAL is replayed after a crash.

---

## Conclusion

PostgreSQL’s WAL system is a robust, well-architected mechanism that ensures your data is safe and recoverable. By understanding both the high-level flow and the underlying source code, you gain insight into one of the most critical parts of PostgreSQL’s architecture.

---

