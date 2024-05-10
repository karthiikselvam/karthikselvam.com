---
title: "Linked List"
description: "2024"
date: "2024-04-22"
tags:
- datastructures
---

In this article, we will solve linked list related problems that are commonly encountered in interviews.

**1. [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/).**
```java
public class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode current = head;
        
        while (current != null) {
            ListNode nextTemp = current.next;
            current.next = prev;
            prev = current;
            current = nextTemp;
        }
        
        return prev; // prev now points to the new head of the reversed list
    }
}
```
Time complexity: O(n), where n is the number of nodes in the linked list.The algorithm iterates through each node of the linked list once. The time complexity is directly proportional to the number of nodes in the list

Space complexity: O(1) The iterative approach uses a constant amount of extra space regardless of the size of the input linked list. We only use a few extra pointers (prev, current, nextTemp) to perform the reversal in place, hence the space complexity is constant.

---


**2. [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/).**
```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        final ListNode root = new ListNode();
        ListNode prev = root;
        while (list1 != null && list2 != null) {
            if (list1.val < list2.val) {
                prev.next = list1;
                list1 = list1.next;
            } else {
                prev.next = list2;
                list2 = list2.next;
            }
            prev = prev.next;
        }
        prev.next = list1 != null ? list1 : list2;
        return root.next;
    }
}
```
Time complexity: O(n), where n is the number of nodes in the linked list.The algorithm iterates through each node of the linked list once. The time complexity is directly proportional to the number of nodes in the list

Space complexity: O(1) The iterative approach uses a constant amount of extra space regardless of the size of the input linked list. We only use a few extra pointers (prev, root) to perform the reversal in place, hence the space complexity is constant.

---

**3. [Reorder List](https://leetcode.com/problems/reorder-list/).**
```java
class Solution {
    public void reorderList(ListNode head) {
        //Find middle of list using a slow and fast pointer approach
        ListNode slow = head;
        ListNode fast = head.next;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        //Reverse the second half of the list using a tmp variable
        ListNode second = slow.next;
        ListNode prev = slow.next = null;
        while (second != null) {
            ListNode tmp = second.next;
            second.next = prev;
            prev = second;
            second = tmp;
        }
        //Re-assign the pointers to match the pattern
        ListNode first = head;
        second = prev;
        while (second != null) {
            ListNode tmp1 = first.next;
            ListNode tmp2 = second.next;
            first.next = second;
            second.next = tmp1;
            first = tmp1;
            second = tmp2;
        }
    }
}
```
Time complexity: O(n), where n is the number of nodes in the linked list.The algorithm iterates through each node of the linked list once. The time complexity is directly proportional to the number of nodes in the list

Space complexity: O(1) The iterative approach uses a constant amount of extra space regardless of the size of the input linked list. We only use a few extra pointers (prev, root) to perform the reversal in place, hence the space complexity is constant.

---

**4. [Remove nth node from the end of list](https://leetcode.com/problems/remove-nth-node-from-end-of-list/).**
```java
public class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode fast = dummy;
        ListNode slow = dummy;
        
        // Move fast pointer n steps ahead
        for (int i = 0; i <= n; i++) {
            fast = fast.next;
        }
        
        // Move both fast and slow pointers until fast reaches the end
        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }
        
        // Remove the nth node from the end
        slow.next = slow.next.next;
        
        return dummy.next;
    }
}
```
Time complexity: O(n), where n is the number of nodes in the linked list.The algorithm iterates through each node of the linked list once. The time complexity is directly proportional to the number of nodes in the list

Space complexity: O(1) The iterative approach uses a constant amount of extra space regardless of the size of the input linked list.

---

**5. [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/).**
```java
public class Solution {

    public boolean hasCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) return true;
        }
        return false;
    }
}
```
Time complexity: O(n), where n is the number of nodes in the linked list.The algorithm iterates through each node of the linked list once. The time complexity is directly proportional to the number of nodes in the list

Space complexity: O(1) The iterative approach uses a constant amount of extra space regardless of the size of the input linked list.

---

**6. [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/).**
```java
    public int findDuplicate(int[] nums) {
        // Initialize slow and fast pointers
        int slow = nums[0];
        int fast = nums[0];
        
        // Move slow pointer one step and fast pointer two steps
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);
        
        // Move slow pointer to the start
        slow = nums[0];
        
        // Move both pointers at the same speed until they meet
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }
        
        // Return the duplicate number
        return slow;
    }


```
Time complexity: The first phase, where we detect the cycle, takes O(n) time, where n is the length of the array.The second phase, where we find the entrance to the cycle, also takes O(n) time in the worst case.

Space complexity: O(1) We are only using a few extra integer variables to store indices (slow and fast pointers), so the space complexity is O(1), constant space.

---

**7. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/).**
```java
    //  Solution using Min Heap
    //  Time Complexity: O(n*log(k))
    //  Extra Space Complexity: O(k)
class Solution1 {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) {
            return null;
        }

        PriorityQueue<ListNode> queue = new PriorityQueue<>((a, b) -> a.val - b.val);
        for (ListNode node : lists) {
            if (node != null) {
                queue.offer(node);
            }
        }

        ListNode dummy = new ListNode(0);
        ListNode current = dummy;

        while (!queue.isEmpty()) {
            ListNode node = queue.poll();
            current.next = node;
            current = current.next;

            if (node.next != null) {
                queue.offer(node.next);
            }
        }

        return dummy.next;
    }
}

class Solution2 {
    public ListNode mergeKLists(ListNode[] lists) {
        int size = lists.length;
        int interval = 1;

        while (interval < size) {
            for (int i = 0; i < size - interval; i += 2 * interval) {
                lists[i] = mergeTwoLists(lists[i], lists[i + interval]);
            }

            interval *= 2;
        }

        return size > 0 ? lists[0] : null;
    }
}

```
Time complexity: The first phase, where we detect the cycle, takes O(n) time, where n is the length of the array.The second phase, where we find the entrance to the cycle, also takes O(n) time in the worst case.

Space complexity: O(1) We are only using a few extra integer variables to store indices (slow and fast pointers), so the space complexity is O(1), constant space.

---

**5. [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer).**
```java
class Solution {

    public Node copyRandomList(Node head) {
        Node cur = head;
        HashMap<Node, Node> map = new HashMap<>();
        while (cur != null) {
            map.put(cur, new Node(cur.val));
            cur = cur.next;
        }
        cur = head;
        while (cur != null) {
            map.get(cur).next = map.get(cur.next);
            map.get(cur).random = map.get(cur.random);
            cur = cur.next;
        }
        return map.get(head);
    }
}

```
Time complexity: In the first pass, we iterate through the original list once to create a copy of each node. This operation takes O(n), where n is the number of nodes in the original list. In the second pass, we again iterate through the original list once to link the copied nodes and assign random pointers. This operation also takes O(n).

Space complexity: O(n) due to the HashMap and the additional nodes created for the copied list.

---
