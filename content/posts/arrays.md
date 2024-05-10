---
title: "Arrays"
description: "2023"
date: "2023-01-22"
tags:
- datastructures
---

In this article, we will solve array-related problems that are commonly encountered in interviews.

**1. [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/).**
```java

class Solution {
    public boolean containsDuplicate(int[] nums) {
        // Use a hash set to keep track of seen elements
        HashSet<Integer> seen = new HashSet<>();
        
        for (int num : nums) {
            // If we've already seen this element, then we have a duplicate
            if (seen.contains(num)) {
                return true;
            }
            
            // Otherwise, add it to the set
            seen.add(num);
        }
        
        // If we make it through the loop without finding a duplicate, then there isn't one
        return false;
    }
}
```
Time complexity: O(n) We loop through the array of integers once, which takes O(n) time.
The hash set's average time complexity for insertion and lookup is O(1), so the total time complexity of the loop is also O(n).

Space complexity: O(n) In the worst case, all elements in the input array are distinct and we must store them all in the hash set. The hash set will therefore have a size of n, so the space complexity is O(n).

---
**2. [Valid Anagram](https://leetcode.com/problems/valid-anagram/).**
```java
public boolean isAnagram(String s, String t) {
    // Check if the lengths of the two strings are the same
    if (s.length() != t.length()) {
        return false;
    }
    
    // Create an array to count the occurrences of characters
    int[] count = new int[26];
    
    // Iterate over the two strings and update the count array
    for (int i = 0; i < s.length(); i++) {
        // Increment the count of the character in the first string
        count[s.charAt(i) - 'a']++;
        // Decrement the count of the character in the second string
        count[t.charAt(i) - 'a']--;
    }
    
    // Iterate over the count array and check if all the counts are 0
    for (int c : count) {
        if (c != 0) {
            // If any count is non-zero, the two strings are not anagrams
            return false;
        }
    }
    
    // If all counts are 0, the two strings are anagrams
    return true;
}
```
Time complexity: O(n), where n is the length of the strings, since we iterate over the two strings once.

Space complexity: O(1), since we use a fixed-size array of size 26 to count the occurrences of characters.

---
**3. [Two Sum](https://leetcode.com/problems/two-sum/).**
```java
public int[] twoSum(int[] nums, int target) {
    // Create a hash table to store the indices of each element
    Map<Integer, Integer> indexMap = new HashMap<>();
    
    // Iterate over the array
    for (int i = 0; i < nums.length; i++) {
        // Calculate the complement of the current element
        int complement = target - nums[i];
        // Check if the complement is in the hash table
        if (indexMap.containsKey(complement)) {
            // If it is, return the indices of the two numbers
            return new int[] { indexMap.get(complement), i };
        }
        // If the complement is not in the hash table, add the current element and its index
        indexMap.put(nums[i], i);
    }
    
    // If no two numbers add up to the target, return null
    return null;
}
```
Time complexity: O(n), where n is the length of the array, since we iterate over the array once.

Space complexity: O(n), since we may store all n elements of the array in the hash table.

---
**4. [Group Anagrams](https://leetcode.com/problems/group-anagrams/).**
```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        // create a map to store the anagram groups
        Map<String, List<String>> map = new HashMap<>();
        
        // iterate through each string in the input array
        for (String str : strs) {
            // convert the string to a character array
            char[] arr = str.toCharArray();
            // sort the characters in the array
            Arrays.sort(arr);
            // create a new string from the sorted characters
            String sorted = new String(arr);
            
            // check if the sorted string is already a key in the map
            if (!map.containsKey(sorted)) {
                // if not, create a new list for this group
                map.put(sorted, new ArrayList<>());
            }
            
            // add the current string to the appropriate group
            map.get(sorted).add(str);
        }
        
        // create a list to hold the anagram groups
        List<List<String>> groups = new ArrayList<>();
        
        // add each group to the list
        for (List<String> group : map.values()) {
            groups.add(group);
        }
        
        // return the list of anagram groups
        return groups;
    }
}

```
Time complexity: O(n * k log k), where n is the number of strings in the input array and k is the maximum length of a string in the array. The main loop iterates through each string in the array and sorts its characters, which takes O(k log k) time per string.

Space complexity: O(n * k), where n is the number of strings in the input array and k is the maximum length of a string in the array. The map can potentially store all n strings, and each string may have up to k characters. Additionally, the list of anagram groups also takes O(n * k) space.

---

**5. [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/).**
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] res = new int[n];
        
        // Initialize the result array with 1
        for (int i = 0; i < n; i++) {
            res[i] = 1;
        }
        
        // Calculate the left products
        int left = 1;
        for (int i = 1; i < n; i++) {
            left *= nums[i - 1];
            res[i] *= left;
        }
        
        // Calculate the right products
        int right = 1;
        for (int i = n - 2; i >= 0; i--) {
            right *= nums[i + 1];
            res[i] *= right;
        }
        
        return res;
    }
}

```
Suppose we have the input array nums = [1, 2, 3, 4]. The goal is to calculate the product of all elements in the array except the current element. In other words, for each element nums[i], we need to calculate the product of all elements except nums[i].

To solve this problem, we can first initialize the result array res with all ones, because the product of any number with one is the number itself. So, res = [1, 1, 1, 1].

Next, we traverse the array from left to right and calculate the product of all elements to the left of each element. For the first element, there are no elements to the left, so we skip it. For the second element, the product of all elements to the left is simply the first element, so we set res[1] to 1 * 1 = 1. For the third element, the product of all elements to the left is 1 * 2 = 2, so we set res[2] to 1 * 2 = 2. For the fourth element, the product of all elements to the left is 1 * 2 * 3 = 6, so we set res[3] to 1 * 2 * 3 = 6. After this step, res = [1, 1, 2, 6].

Next, we traverse the array from right to left and calculate the product of all elements to the right of each element. For the last element, there are no elements to the right, so we skip it. For the third element, the product of all elements to the right is simply the fourth element, so we set res[2] to 2 * 4 = 8. For the second element, the product of all elements to the right is 4 * 3 = 12, so we set res[1] to 1 * 12 = 12. For the first element, the product of all elements to the right is 4 * 3 * 2 = 24, so we set res[0] to 1 * 24 = 24. After this step, res = [24, 12, 8, 6].

Thus, the output for the input array nums = [1, 2, 3, 4] is [24, 12, 8, 6], which is the product of all elements in the array except the current element.

Time Complexity: O(n) - The solution traverses the array three times, each taking O(n) time. Thus, the overall time complexity is O(n).

Space Complexity: O(1) - The solution uses only constant space for storing the variables, and the output array is not considered in the space complexity calculation.If we consider the output array in the space complexity calculation, the space complexity would be O(n).

--- 
**5. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/description/).**
```java
    public int[] topKFrequent(int[] nums, int k) {
        HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
        for(int num : nums){
            map.put(num,map.getOrDefault(num,0)+1);
        }
        // Sorting in ascending order
        //PriorityQueue<Map.Entry<Integer,Integer>> minHeap = new PriorityQueue<Map.Entry<Integer,Integer>>((e1,e2) -> e1.getValue() - e2.getValue());
        PriorityQueue<Map.Entry<Integer,Integer>> maxHeap = new PriorityQueue<Map.Entry<Integer,Integer>>((e1,e2) -> e2.getValue() - e1.getValue());
        for(Map.Entry<Integer,Integer> entry : map.entrySet()) {
           maxHeap.add(entry) ;
        }
        int i = 0; 
        int[] result = new int[k];
        while(i < k){
           Map.Entry<Integer,Integer> currEntry = maxHeap.poll() ;
           result[i] = currEntry.getKey();
            i++;
        }
          return result;  
    }
```
Time Complexity: Building the HashMap takes O(n) time, as we need to iterate through the entire input array.
Building the PriorityQueue takes O(n log k) time. For each element in the HashMap, we perform an operation that takes O(log k) time to add the entry to the priority queue. We do this operation n times, so the total time complexity of building the PriorityQueue is O(n log k). Extracting the top k frequent elements from the PriorityQueue takes O(k log n) time. We perform the poll operation k times, which takes O(log n) time each time. Therefore, the overall time complexity of the topKFrequent method is O(n log k).

Space Complexity: O(n) because we need to store the frequency of each element in the input array in the HashMap. The size of the HashMap is bounded by the number of distinct elements in the input array, which is at most n. The PriorityQueue can also have at most k elements, so its space complexity is O(k). Combining these two space requirements, the overall space complexity of the topKFrequent method is O(n).

---

**6. [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence).**
```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums == null || nums.length == 0)
            return 0;

        HashSet<Integer> set = new HashSet<>();
        for (int num : nums) {
            set.add(num);
        }

        int longestStreak = 0;

        for (int num : nums) {
            if (!set.contains(num - 1)) { // Only process if it's the start of a sequence
                int currentNum = num;
                int currentStreak = 1;

                while (set.contains(currentNum + 1)) {
                    currentNum++;
                    currentStreak++;
                }

                longestStreak = Math.max(longestStreak, currentStreak);
            }
        }

        return longestStreak;
    }
}

```
Time complexity: Building the HashSet initially takes O(n) time, as we need to add each element of the array to the HashSet.The subsequent iteration over the array takes O(n) time as well, as we examine each element exactly once. Within the iteration, the while loop that checks for consecutive elements also takes O(n) time in the worst case, but it does not run for each element. It runs only when we encounter the start of a consecutive sequence, which happens relatively infrequently in practice.

Space complexity: O(n), This is because we use a HashSet to store the elements of the array, which can take up to O(n) space in the worst case if all elements are unique.

---
