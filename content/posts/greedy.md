---
title: "Greedy Problems"
description: "2023"
date: "2023-01-22"
tags:
- datastructures
---

In this article, we will solve greedy problems that are commonly encountered in interviews.

**1. [Jump Game](https://leetcode.com/problems/jump-game/).**
```java
public boolean canJump(int[] nums) {
    int lastIndex = nums.length - 1; // set last index as the end of the array
    for (int i = nums.length - 2; i >= 0; i--) { // iterate backwards through the array
        if (i + nums[i] >= lastIndex) { // check if we can reach the current last index from current index
            lastIndex = i; // update last index to current index
        }
    }
    return lastIndex == 0; // check if we can reach the first index
}

```

```
if (i + nums[i] >= lastIndex)
```
This line checks if we can reach the current lastIndex from the current index i. Here's how it works:
- i is the current index we're looking at.
- nums[i] is the maximum number of steps we can take from index i.
- i + nums[i] is the index we would end up at if we were to make a jump of size nums[i] from index i.
- lastIndex is the last index we need to be able to reach.

Time complexity: O(n) where n is the length of the input array. This is because we only iterate through the input array once. 

Space complexity: O(1) because we only use a constant amount of extra space to store the lastIndex variable.

---
