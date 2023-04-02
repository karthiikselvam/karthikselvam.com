---
title: "Intervals"
description: "2023"
date: "2023-01-22"
tags:
- datastructures
---


In this article, we will solve intervals-related problems that are commonly encountered in interviews.

**1. [Merge Intervals](https://leetcode.com/problems/merge-intervals/).**
```java
public int[][] merge(int[][] intervals) {
    List<int[]> result = new ArrayList<>();
    if (intervals.length < 2) {
        return intervals;
    }
    Arrays.sort(intervals, Comparator.comparingInt(a -> a[0]));
    int start = intervals[0][0];
    int end = intervals[0][1];
    for (int i = 1; i < intervals.length; i++) {
        if (end >= intervals[i][0]) {
            end = Math.max(end, intervals[i][1]);
        } else {
            result.add(new int[]{start, end});
            start = intervals[i][0];
            end = intervals[i][1];
        }
    }
    result.add(new int[]{start, end});
    return result.toArray(new int[result.size()][]);
}
```
Time complexity:  O(n log n) for the initial sorting of the intervals array, plus O(n) for the loop that iterates over the sorted intervals. Within the loop, the operations Math.max and adding to the result list take constant time. Therefore, the overall time complexity is O(n log n + n) = O(n log n).

Space complexity: O(n) for the result list, which could potentially store all n intervals if none of them overlap. The intervals array is modified in place, so it does not contribute to the space complexity. Therefore, the overall space complexity is O(n).

---

**2. [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/).**
One approach to solve this problem is to use the greedy algorithm. We can first sort the intervals based on their end time. Then, you can iterate through the intervals and keep track of the current end time. If the start time of the current interval is less than the current end time, then it overlaps with the previous interval and needs to be removed. Otherwise, update the current end time to be the end time of the current interval.
```java
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals,Comparator.comparingInt(a -> a[1]));
        int count = 0;
        int end = Integer.MIN_VALUE;
        for(int[] interval : intervals){
            int start = interval[0];
            int currEnd = interval[1];
            if(start < end){
                count++;
            } else{
                end = currEnd;
            }
        }
        return count;
    }
```
Time complexity:  O(n log n) for the initial sorting of the intervals array, plus O(n) for the loop that iterates over the sorted intervals. Within the loop, the operations Math.max and adding to the result list take constant time. Therefore, the overall time complexity is O(n log n + n) = O(n log n).

Space complexity: O(1) 

---

**3. [Insert Interval](https://leetcode.com/problems/insert-interval/).**
```java
    class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        List<int[]> result = new ArrayList<>();
        int i = 0;
        
        // add all intervals before newInterval
        while (i < intervals.length && intervals[i][1] < newInterval[0]) {
            result.add(intervals[i]);
            i++;
        }
        
        // merge intervals that overlap with newInterval
        while (i < intervals.length && intervals[i][0] <= newInterval[1]) {
            newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
            newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
            i++;
        }
        
        result.add(newInterval);
        
        // add all remaining intervals
        while (i < intervals.length) {
            result.add(intervals[i]);
            i++;
        }
        
        return result.toArray(new int[result.size()][2]);
    }
}

```
Time complexity: O(n), The method iterates through each interval once, which takes O(n) time complexity.

Space complexity: O(n), The method uses an ArrayList to store the resulting intervals, which can take up to O(n) space complexity in the worst case.

---
