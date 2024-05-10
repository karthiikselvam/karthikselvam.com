---
title: "Recursion & Backtracking"
description: "2024"
date: "2024-04-22"
tags:
- datastructures
---

In this article, we will solve recursion and backtracking related problems that are commonly encountered in interviews.

**1. Given an array of integers, print all combinations of size X.
```java
public static void printCombos(int[] a, int x) {
    if (a == null || a.length == 0 || x > a.length)
        return;
    int[] buffer = new int[x];
    printCombosHelper(a, buffer, 0, 0);
}

public static void printCombosHelper(int[] a, int[] buffer, int startIndex, int bufferIndex) {
    // termination cases - buffer full
    if (bufferIndex == buffer.length) {
        printArray(buffer);
        return;
    }
    if (startIndex == a.length) {
        return;
    }
    // find candidates that go into current buffer index
    for (int i = startIndex; i < a.length; i++) {
        // place item into buffer
        buffer[bufferIndex] = a[i];
        // recurse to next buffer index
        printCombosHelper(a, buffer, i + 1, bufferIndex + 1);
    }
}

```
Time complexity: O(n^x), where n is the length of array 'a' and 'x' is the size of combinations.

Space complexity: O(x), for the buffer array.

---

**2. Given an array of integers A, print all its subsets.
```java
public static void printSubsets(int[] a, int x) {
    if (a == null || a.length == 0 || x > a.length)
        return;
    int[] buffer = new int[x];
    printCombosHelper(a, buffer, 0, 0);
}

public static void printSubsetsHelper(int[] a, int[] buffer, int startIndex, int bufferIndex) {
    printArray(buffer);
    // termination cases - buffer full
    if (bufferIndex == buffer.length) {
        return;
    }
    if (startIndex == a.length) {
        return;
    }
    // find candidates that go into current buffer index
    for (int i = startIndex; i < a.length; i++) {
        // place item into buffer
        buffer[bufferIndex] = a[i];
        // recurse to next buffer index
        printCombosHelper(a, buffer, i + 1, bufferIndex + 1);
    }
}

```
Time complexity: O(n^x), where n is the length of array 'a' and 'x' is the size of combinations.

Space complexity: O(x), for the buffer array.

---

**3. [Subsets](https://leetcode.com/problems/subsets/).**
```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> templist  = new  ArrayList<>();
        getSubsets(nums,result,templist,0);
        return result;
    }
    
    public void getSubsets(int[] nums, List<List<Integer>> result, List<Integer> tempList , int start){
            result.add(new ArrayList(tempList));
            if(start >= nums.length){
                return;
            }
            for(int i = start ; i < nums.length ; i++){
                tempList.add(nums[i]);
                getSubsets(nums,result,tempList,i+1);
                tempList.remove(tempList.size()-1);
            }
    }
}
```
---

**3. [Subsets II](https://leetcode.com/problems/subsets-ii/).**
```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums) ;
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> tempList = new ArrayList<>();
        getSubsets(nums,result,tempList,0);
        return result;
    }
    
    public void getSubsets(int[] nums, List<List<Integer>> result, List<Integer> tempList , int start){
       result.add(new ArrayList(tempList));
       if(start >= nums.length){
           return;
       }
       for(int i =start ; i < nums.length ; i++){
           if(i > start && nums[i-1] == nums[i]){
                continue; 
           }
           tempList.add(nums[i]);
           getSubsets(nums,result,tempList,i+1);
           tempList.remove(tempList.size()-1);
       }
    }
}
```
Time complexity: O(n), where n is the number of nodes in the linked list.The algorithm iterates through each node of the linked list once. The time complexity is directly proportional to the number of nodes in the list

Space complexity: O(1) The iterative approach uses a constant amount of extra space regardless of the size of the input linked list. We only use a few extra pointers (prev, current, nextTemp) to perform the reversal in place, hence the space complexity is constant.

---

**4. Given an array A, print all permutations of size X.    
```java
public static void printPerms(int[] a, int x) {
    if (a == null || a.length == 0 || x > a.length)
        return;
    int[] buffer = new int[x];
    boolean[] isInBuffer = new boolean[a.length];
    printPermsHelper(a, buffer, 0, isInBuffer);
}

public static void printPermsHelper(int[] a, int[] buffer, int bufferIndex, boolean[] isInBuffer) {
    // termination cases - buffer full
    if (bufferIndex == buffer.length) {
        printArray(buffer);
        return;
    }
    // find candidates that go into current buffer index
    for (int i = startIndex; i < a.length; i++) {
        // place item into buffer
        buffer[bufferIndex] = a[i];
        isInBuffer[i] = true;
        // recurse to next buffer index
        printCombosHelper(a, buffer, bufferIndex + 1, isInBuffer);
        isInBuffer[i] = false;
    }
}
```
Time complexity: O(n^x), where n is the length of array 'a' and 'x' is the size of combinations.

Space complexity: O(x), for the buffer array.

---

**6. [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/).**
```java
class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> partitions = new ArrayList<>();
        backtrack(s, 0, new ArrayList<>(), partitions);
        return partitions;
    }
    
    private void backtrack(String s, int start, List<String> path, List<List<String>> partitions) {
        if (start == s.length()) {
            partitions.add(new ArrayList<>(path));
            return;
        }
        
        for (int end = start + 1; end <= s.length(); end++) {
            if (isPalindrome(s, start, end - 1)) {
                path.add(s.substring(start, end));
                backtrack(s, end, path, partitions);
                path.remove(path.size() - 1);
            }
        }
    }
    
    private boolean isPalindrome(String s, int left, int right) {
        while (left < right) {
            if (s.charAt(left++) != s.charAt(right--)) {
                return false;
            }
        }
        return true;
    }
}

```
---

**7. [Combination Sum](hhttps://leetcode.com/problems/combination-sum/).**
```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(candidates, target, 0, new ArrayList<>(), result);
        return result;
    }
    
    private void backtrack(int[] candidates, int target, int start, List<Integer> combination, List<List<Integer>> result) {
        if (target == 0) {
            result.add(new ArrayList<>(combination));
            return;
        }
        
        for (int i = start; i < candidates.length; i++) {
            if (candidates[i] <= target) {
                combination.add(candidates[i]);
                backtrack(candidates, target - candidates[i], i, combination, result);
                combination.remove(combination.size() - 1);
            }
        }
    }
}
```
---


**8. [Permutations](https://leetcode.com/problems/permutations/).**
```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(result, new ArrayList<>(), nums);
        return result;
    }

    private void backtrack(List<List<Integer>> result, List<Integer> tempList, int[] nums) {
        if (tempList.size() == nums.length) {
            result.add(new ArrayList<>(tempList));
        } else {
            for (int i = 0; i < nums.length; i++) {
                if (tempList.contains(nums[i])) continue;
                tempList.add(nums[i]);
                backtrack(result, tempList, nums);
                tempList.remove(tempList.size() - 1);
            }
        }
    }
}

```
---

**8. [Combination Sum II](https://leetcode.com/problems/combination-sum-ii).**
```java
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(candidates);
        backtrack(result, new ArrayList<>(), candidates, target, 0);
        return result;
    }

    private void backtrack(List<List<Integer>> result, List<Integer> tempList, int[] candidates, int remain, int start) {
        if (remain < 0) return;
        else if (remain == 0) result.add(new ArrayList<>(tempList));
        else {
            for (int i = start; i < candidates.length; i++) {
                if (i > start && candidates[i] == candidates[i - 1]) continue; // skip duplicates
                tempList.add(candidates[i]);
                backtrack(result, tempList, candidates, remain - candidates[i], i + 1);
                tempList.remove(tempList.size() - 1);
            }
        }
    }
}

```
---

**9. [Word Search](https://leetcode.com/problems/word-search/).**
```java
class Solution {
    public boolean exist(char[][] board, String word) {
        int rows = board.length ; 
        int cols = board[0].length;
        
        for(int r = 0 ; r < rows; r++ ){
            for(int c = 0 ; c < cols; c++){
                if(dfs(board,word,r,c,rows,cols,0)){
                    return true;
                }
            }
        }
        return false;
    }
    
   public boolean dfs(char[][] board, String word, int r, int c , int rows, int cols, int index) {
       if(index == word.length()){
           return true;
       }
       
       if(oob(r,c,rows,cols) || board[r][c] != word.charAt(index) || board[r][c] == '#' ){
           return false;
       }
       
       char ch = board[r][c];
       board[r][c] = '#';
       boolean result = dfs(board,word,r+1,c,rows,cols,index+1) ||  dfs(board,word,r-1,c,rows,cols,index+1) || 
                dfs(board,word,r,c-1,rows,cols,index+1) ||  dfs(board,word,r,c+1,rows,cols,index+1) ;
       board[r][c] = ch;
       return result;
   }
    
    public boolean oob(int r, int c , int rows , int cols) {
       if(r > rows-1 || r < 0 || c> cols-1 || c < 0) {
           return true;
       } else{
           return false;
       }
    }
}
```
---


