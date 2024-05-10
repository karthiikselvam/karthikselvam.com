---
title: "Two Pointers"
description: "2023"
date: "2023-01-22"
tags:
- datastructures
---

In this article, we will solve Two Pointers related problems that are commonly encountered in interviews.

**1. [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/).**
```java
class Solution {
    public boolean isPalindrome(String s) {
        if (s == null || s.isEmpty()) {
            return true; // Empty string is considered a palindrome
        }
        // Preprocess the string: remove non-alphanumeric characters and convert to lowercase
        StringBuilder sb = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (Character.isLetterOrDigit(c)) {
                sb.append(Character.toLowerCase(c));
            }
        }
        String processedString = sb.toString();
        // Use two pointers approach to check if the string is a palindrome
        int left = 0, right = processedString.length() - 1;
        while (left < right) {
            if (processedString.charAt(left) != processedString.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}

```
Time complexity:  O(n), where n is the length of the input string.

Space complexity: O(n), The space required by the StringBuilder can be at most the same as the input string, which is O(n) in the worst case. The processed string may require additional space, but it is still bounded by the length of the input string, which is O(n). Therefore, the overall space complexity is also O(n).

---

**2. [Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/).**
```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0;
        int right = numbers.length - 1;
        
        while (left < right) {
            int sum = numbers[left] + numbers[right];
            if (sum == target) {
                return new int[]{left + 1, right + 1}; // Indices are 1-based
            } else if (sum < target) {
                left++;
            } else {
                right--;
            }
        }
        
        // If no such pair is found
        return new int[]{-1, -1};
    }
}

```
Time complexity:  O(n), where n is the number of elements in the array.

Space complexity: O(1), meaning it uses constant extra space.

---

**3. [3Sum](https://leetcode.com/problems/3sum/).**
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums); // Sort the array
        for (int i = 0; i < nums.length - 2; i++) {
            // Avoid duplicates
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int left = i + 1;
            int right = nums.length - 1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                
                if (sum == 0) {
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    // Avoid duplicates
                    while (left < right && nums[left] == nums[left + 1]) {
                        left++;
                    }
                    while (left < right && nums[right] == nums[right - 1]) {
                        right--;
                    }
                    // Move pointers
                    left++;
                    right--;
                } else if (sum < 0) {
                    left++;
                } else {
                    right--;
                }
            }
        }
        return result;
    }
}

```
Time complexity:  O(n log n), Sorting the array takes O(n log n) time complexity.

Space complexity: The space complexity of the result list is O(m), where m is the number of unique triplets that sum up to zero.

---

**4. [Container With Most Water](https://leetcode.com/problems/container-with-most-water).**
```java
class Solution {
    public int maxArea(int[] height) {
        int maxArea = 0;
        int left = 0;
        int right = height.length - 1;
        while (left < right) {
            int minHeight = Math.min(height[left], height[right]);
            int width = right - left;
            int area = width * minHeight;
            maxArea = Math.max(maxArea, area);
            
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }
        return maxArea;
    }
}

```
Time complexity:  O(n), where n is the number of elements in the array, the time complexity is O(n).

Space complexity: O(1) 

---

**5. [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/).**
```java
public class TrappingRainWater {
    public int trap(int[] height) {
        if (height == null || height.length == 0) {
            return 0;
        }
        int n = height.length;
        int[] leftMax = new int[n];
        int[] rightMax = new int[n];
        // Calculate the maximum height of bars to the left of each position
        leftMax[0] = height[0];
        for (int i = 1; i < n; i++) {
            leftMax[i] = Math.max(leftMax[i - 1], height[i]);
        }
        // Calculate the maximum height of bars to the right of each position
        rightMax[n - 1] = height[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            rightMax[i] = Math.max(rightMax[i + 1], height[i]);
        }
        // Calculate the trapped water at each position
        int totalWater = 0;
        for (int i = 0; i < n; i++) {
            int minHeight = Math.min(leftMax[i], rightMax[i]);
            totalWater += minHeight - height[i];
        }
        return totalWater;
    }
}

```
Time complexity:  O(n), Each iteration through the array takes O(n) time, where n is the number of elements in the array.

Space complexity: O(n), where n is the number of elements in the array. 

---
