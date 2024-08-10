---
title: "Binary Search"
description: "2024"
date: "2023-08-08"
tags:
- datastructures
---

In this article, we will solve binary search related problems that are commonly encountered in interviews.

**1. Implement a binary search on a sorted array to find the index of a target element.**

```java
public class BinarySearchExample {
    public static int binarySearch(int[] array, int target) {
        int left = 0;
        int right = array.length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (array[mid] == target) {
                return mid; // Target found
            } else if (array[mid] < target) {
                left = mid + 1; // Search right half
            } else {
                right = mid - 1; // Search left half
            }
        }

        return -1; // Target not found
    }

    public static void main(String[] args) {
        int[] sortedArray = {1, 3, 5, 7, 9, 11};
        int target = 7;

        int result = binarySearch(sortedArray, target);

        if (result != -1) {
            System.out.println("Target found at index: " + result);
        } else {
            System.out.println("Target not found.");
        }
    }
}

```

**2. Modify binary search to return the index of the first occurrence of a target element (handling duplicates).**

```java
public class BinarySearchFirstOccurrence {
    public static int binarySearchFirstOccurrence(int[] array, int target) {
        int left = 0;
        int right = array.length - 1;
        int result = -1; // Variable to store the index of the first occurrence

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (array[mid] == target) {
                result = mid; // Update result to current index
                right = mid - 1; // Continue searching in the left half
            } else if (array[mid] < target) {
                left = mid + 1; // Search right half
            } else {
                right = mid - 1; // Search left half
            }
        }

        return result; // Return the index of the first occurrence or -1 if not found
    }

    public static void main(String[] args) {
        int[] sortedArray = {1, 2, 2, 2, 3, 4, 5};
        int target = 2;

        int result = binarySearchFirstOccurrence(sortedArray, target);

        if (result != -1) {
            System.out.println("First occurrence of target found at index: " + result);
        } else {
            System.out.println("Target not found.");
        }
    }
}

```

**3. Given a rotated sorted array, search for a target value. Modify the binary search algorithm to account for rotation.**

```java
public class SearchInRotatedSortedArray {
    public static int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            // Check if mid is the target
            if (nums[mid] == target) {
                return mid;
            }

            // Determine which side is sorted
            if (nums[left] <= nums[mid]) { // Left half is sorted
                if (target >= nums[left] && target < nums[mid]) {
                    right = mid - 1; // Target is in the left half
                } else {
                    left = mid + 1; // Target is in the right half
                }
            } else { // Right half is sorted
                if (target > nums[mid] && target <= nums[right]) {
                    left = mid + 1; // Target is in the right half
                } else {
                    right = mid - 1; // Target is in the left half
                }
            }
        }

        return -1; // Target not found
    }

    public static void main(String[] args) {
        int[] nums = {4, 5, 6, 7, 0, 1, 2};
        int target = 0;

        int result = search(nums, target);

        if (result != -1) {
            System.out.println("Target found at index: " + result);
        } else {
            System.out.println("Target not found.");
        }
    }
}

```

**4. Search in a rotated sorted array with duplicates.**

```java
public class SearchInRotatedSortedArrayWithDuplicates {
    public static int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            // Check if mid is the target
            if (nums[mid] == target) {
                return mid; // Return the index if target is found
            }

            // If duplicates are present, we cannot be sure of the sorted part
            if (nums[left] == nums[mid] && nums[mid] == nums[right]) {
                left++;
                right--;
            } else if (nums[left] <= nums[mid]) { // Left half is sorted
                if (target >= nums[left] && target < nums[mid]) {
                    right = mid - 1; // Target is in the left half
                } else {
                    left = mid + 1; // Target is in the right half
                }
            } else { // Right half is sorted
                if (target > nums[mid] && target <= nums[right]) {
                    left = mid + 1; // Target is in the right half
                } else {
                    right = mid - 1; // Target is in the left half
                }
            }
        }

        return -1; // Target not found
    }

    public static void main(String[] args) {
        int[] nums = {2, 5, 6, 0, 0, 1, 2};
        int target = 0;

        int result = search(nums, target);

        if (result != -1) {
            System.out.println("Target found at index: " + result);
        } else {
            System.out.println("Target not found.");
        }
    }
}

```

**5. Given a rotated sorted array, find the minimum element.**

By comparing the middle element (nums[mid]) with the rightmost element (nums[right]), we can determine whether the minimum element is to the right or left of mid. If nums[mid] > nums[right], it means the minimum is in the right half. If nums[mid] <= nums[right], it means the minimum is in the left half or could be mid itself.

```java
public class FindMinimumInRotatedSortedArrayWithDuplicates {
    public static int findMin(int[] nums) {
        int left = 0;
        int right = nums.length - 1;

        while (left < right) {
            int mid = left + (right - left) / 2;

            if (nums[mid] > nums[right]) {
                // Minimum is in the right part
                left = mid + 1;
            } else if (nums[mid] < nums[right]) {
                // Minimum is in the left part including mid
                right = mid;
            } else {
                // nums[mid] == nums[right], we cannot determine the side, reduce the search space (since duplicates at the end do not help in locating the minimum).
                right--;
            }
        }

        // When left == right, the minimum element is found
        return nums[left];
    }

    public static void main(String[] args) {
        int[] nums = {2, 2, 2, 0, 1};
        int minElement = findMin(nums);
        System.out.println("The minimum element is: " + minElement);
    }
}

```

**6. Given an array where adjacent elements are not equal, find a peak element (an element that is greater than its neighbors).**

In a peak-finding problem, we are interested in the region where elements increase and then decrease, creating a peak. If nums[mid] < nums[mid - 1], it suggests that we are on a "downhill slope" from nums[mid - 1] to nums[mid]. Since we're looking for a peak, and since nums[mid - 1] is greater than nums[mid], we know that a peak must exist either at nums[mid - 1] or further to the left. Therefore, we move our search to the left half by setting right = mid - 1.

```java
public class FindPeakElement {
    public static int findPeakElement(int[] nums) {
        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            // Check if mid is a peak element
            boolean isPeak = (mid == 0 || nums[mid] > nums[mid - 1]) &&
                             (mid == nums.length - 1 || nums[mid] > nums[mid + 1]);

            if (isPeak) {
                return mid; // Return the index of the peak element
            } else if (mid > 0 && nums[mid] < nums[mid - 1]) {
                right = mid - 1; // Peak is in the left half
            } else {
                left = mid + 1; // Peak is in the right half
            }
        }

        return -1; // This should never be reached if input constraints are met
    }

    public static void main(String[] args) {
        int[] nums = {1, 3, 20, 4, 1};
        int peakIndex = findPeakElement(nums);
        if (peakIndex != -1) {
            System.out.println("Peak element found at index: " + peakIndex + " with value: " + nums[peakIndex]);
        } else {
            System.out.println("No peak element found.");
        }
    }
}

```

**7. Given a sorted array of integers, find the starting and ending position of a given target value.**

The idea is to perform two binary searches: one to find the first occurrence of the target and another to find the last occurrence.
```java
public class FindFirstAndLastPosition {
    public static int[] searchRange(int[] nums, int target) {
        int[] result = new int[2];
        result[0] = findFirstPosition(nums, target);
        result[1] = findLastPosition(nums, target);
        return result;
    }

    private static int findFirstPosition(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        int firstPosition = -1;

        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] == target) {
                firstPosition = mid;
                right = mid - 1;  // Move left to find the first occurrence
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return firstPosition;
    }

    private static int findLastPosition(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        int lastPosition = -1;

        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] == target) {
                lastPosition = mid;
                left = mid + 1;  // Move right to find the last occurrence
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return lastPosition;
    }

    public static void main(String[] args) {
        int[] nums = {5, 7, 7, 8, 8, 10};
        int target = 8;
        int[] positions = searchRange(nums, target);
        System.out.println("Starting position: " + positions[0] + ", Ending position: " + positions[1]);
    }
}

```

**8. Implement integer square root calculation using binary search.**

The idea is to find the largest integer x such that x * x is less than or equal to the given number n. Check if mid * mid is less than or equal to n. Since mid * mid could overflow for large mid values, use mid <= n / mid instead of mid * mid <= n. If mid * mid is less than or equal to n, update result to mid and move to the right half (left = mid + 1) to see if there's a larger mid that also satisfies the condition. If mid * mid is greater than n, move to the left half (right = mid - 1).
```java
public class IntegerSquareRoot {
    public static int sqrt(int n) {
        if (n < 0) {
            throw new IllegalArgumentException("Square root of a negative number is not defined.");
        }
        if (n == 0 || n == 1) {
            return n;
        }

        int left = 1, right = n;
        int result = 0;

        while (left <= right) {
            int mid = left + (right - left) / 2;
            // Check if mid*mid is equal to n
            if (mid <= n / mid) {
                // If mid*mid <= n, move to the right half
                result = mid;  // Update the result to mid as a potential answer
                left = mid + 1;
            } else {
                // If mid*mid > n, move to the left half
                right = mid - 1;
            }
        }

        return result;
    }

    public static void main(String[] args) {
        int number = 16;
        int sqrtValue = sqrt(number);
        System.out.println("The integer square root of " + number + " is: " + sqrtValue);

        number = 27;
        sqrtValue = sqrt(number);
        System.out.println("The integer square root of " + number + " is: " + sqrtValue);
    }
}

```

**8. Given a sorted array where every element appears exactly twice except for one element, find that single one using binary search.**

Before the Single Element: If the mid-point falls before the single element, the array behaves normally: pairs start at even indices. If mid is even and nums[mid] == nums[mid + 1], the single element is to the right, so you move low to mid + 2. At or After the Single Element: Once mid passes the single element, the pattern is broken. Now, if mid is even and nums[mid] != nums[mid + 1], it indicates that the single element is either at mid or to the left, so you move high to mid.
```java
public class SingleElementInSortedArray {
    public static int findSingleElement(int[] nums) {
        int low = 0;
        int high = nums.length - 1;

        while (low < high) {
            int mid = low + (high - low) / 2;

            // Ensure mid is even, to check pairs correctly
            if (mid % 2 == 1) {
                mid--;
            }

            // Check if the pair starts at mid
            if (nums[mid] == nums[mid + 1]) {
                // Single element is after mid
                low = mid + 2;
            } else {
                // Single element is before mid
                high = mid;
            }
        }

        // Low should point to the single element
        return nums[low];
    }

    public static void main(String[] args) {
        int[] nums = {1, 1, 2, 2, 3, 4, 4, 5, 5};
        int singleElement = findSingleElement(nums);
        System.out.println("The single element is: " + singleElement); // Output should be 3
    }
}

```

**9. Given a 2D matrix, search for a target value using a modified binary search.**

When performing binary search on a 1D array, the index mid directly points to an element in that array. However, since the matrix is 2D, we need to access elements using row and column indices.Given a mid index in this flattened array, you need to determine which row and column it corresponds to in the original 2D matrix. The first n elements (where n is the number of columns) fill up the first row. The next n elements fill up the second row, and so on. Given that, by dividing mid by the number of columns (n), you're effectively counting how many complete rows fit into the first mid elements, and the remainder gives you the exact position within that row.
```java
public class Search2DMatrix {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }

        int rows = matrix.length;
        int cols = matrix[0].length;
        int low = 0;
        int high = rows * cols - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;
            int midElement = matrix[mid / cols][mid % cols];

            if (midElement == target) {
                return true;
            } else if (midElement < target) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }

        return false;
    }

    public static void main(String[] args) {
        Search2DMatrix solution = new Search2DMatrix();
        int[][] matrix = {
            {1, 3, 5, 7},
            {10, 11, 16, 20},
            {23, 30, 34, 60}
        };
        int target = 3;
        System.out.println(solution.searchMatrix(matrix, target)); // Output: true

        target = 13;
        System.out.println(solution.searchMatrix(matrix, target)); // Output: false
    }
}

```

**10. Find the k-th smallest element in a sorted matrix (matrix is row-wise and column-wise sorted).**

Use a min-heap (priority queue) to store the smallest elements. Start by adding the first element of each row into the heap. Extract the smallest element from the heap, and add the next element from the same row to the heap. After performing this k-1 times, the root of the heap will be the k-th smallest element.
```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        PriorityQueue<int[]> minHeap = new PriorityQueue<>((a, b) -> a[0] - b[0]);

        // Initialize the heap with the first element of each row
        for (int i = 0; i < Math.min(n, k); i++) {
            minHeap.offer(new int[]{matrix[i][0], i, 0});
        }

        // Extract the min element from the heap k-1 times
        for (int i = 0; i < k - 1; i++) {
            int[] entry = minHeap.poll();
            int row = entry[1], col = entry[2];
            if (col + 1 < n) {
                minHeap.offer(new int[]{matrix[row][col + 1], row, col + 1});
            }
        }

        return minHeap.poll()[0];
    }
}

```
Binary Search : 

Perform binary search on the range of possible values in the matrix, from the smallest element to the largest element. For each middle value, count how many elements are less than or equal to it by traversing the matrix. If the count is less than k, adjust the search range to the higher half; otherwise, adjust to the lower half. When the search range converges, the value is the k-th smallest.
```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        int low = matrix[0][0];
        int high = matrix[n - 1][n - 1];

        while (low < high) {
            int mid = low + (high - low) / 2;
            int count = countLessEqual(matrix, mid);

            if (count < k) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }

        return low;
    }

    private int countLessEqual(int[][] matrix, int target) {
        int n = matrix.length;
        int count = 0;
        int row = n - 1;
        int col = 0;

        while (row >= 0 && col < n) {
            if (matrix[row][col] <= target) {
                count += (row + 1);
                col++;
            } else {
                row--;
            }
        }

        return count;
    }
}

```

**9. Searching in a Sorted Matrix.**

Top-Right Approach : In this approach, you start from the top-right corner of the matrix at position (row = 0, col = n-1) where n is the number of columns. From this position: If the current element is equal to the target, you've found the target. If the current element is greater than the target, you move left (decrease col). If the current element is smaller than the target, you move down (increase row).
```java
matrix = [
  [1, 4, 7, 11],
  [2, 5, 8, 12],
  [3, 6, 9, 16],
  [10, 13, 14, 17]
]
int row = 0; // Start at the first row
int col = matrix[0].length - 1; // Start at the last column

while (row < matrix.length && col >= 0) {
    if (matrix[row][col] == target) {
        // Target found
        return true;
    } else if (matrix[row][col] > target) {
        // Current element is greater than target, move left
        col--;
    } else {
        // Current element is smaller than target, move down
        row++;
    }
}

// Target not found
return false;

```
Bottom-Left Approach : Starting from the bottom-left corner (n-1, 0): If the target is smaller than the current element, move up (decrease row). If the target is larger than the current element, move right (increase col).
```java
int row = matrix.length - 1; // Start at the last row
int col = 0; // Start at the first column

while (row >= 0 && col < matrix[0].length) {
    if (matrix[row][col] == target) {
        // Target found
        return true;
    } else if (matrix[row][col] > target) {
        // Target is smaller, move up
        row--;
    } else {
        // Target is larger, move right
        col++;
    }
}

```

**11. Find the median of two sorted arrays. This problem requires an efficient solution using binary search..**

Ensure nums1 is the smaller array: Since the binary search will be applied on the smaller array, we ensure nums1 has fewer or equal elements compared to nums2. Binary Search on nums1:Define two pointers, low and high, representing the search range within nums1. Perform binary search by selecting a partition index i for nums1 and a corresponding partition index j for nums2 such that i + j = (m + n + 1) / 2. To find the median, we need to know how many elements should be on the left side of the partition. If the total number of elements is m + n, the left half should contain (m + n) / 2 elements if m + n is even, or (m + n + 1) / 2 elements if m + n is odd (we use (m + n + 1) / 2 to handle both cases with a single expression). We are using binary search on nums1, so we choose a partition index i for nums1. The remaining elements that should be in the left half must then come from nums2, which means we want totalLeft - i elements from nums2. Thus, the partition index j in nums2 should be j = totalLeft - i.
```java
public double findMedianSortedArrays(int[] nums1, int[] nums2) {
    if (nums1.length > nums2.length) {
        return findMedianSortedArrays(nums2, nums1);
    }

    int m = nums1.length;
    int n = nums2.length;
    int low = 0, high = m;

    while (low <= high) {
        int i = (low + high) / 2;
        int j = (m + n + 1) / 2 - i;

        int maxLeft1 = (i == 0) ? Integer.MIN_VALUE : nums1[i - 1];
        int minRight1 = (i == m) ? Integer.MAX_VALUE : nums1[i];

        int maxLeft2 = (j == 0) ? Integer.MIN_VALUE : nums2[j - 1];
        int minRight2 = (j == n) ? Integer.MAX_VALUE : nums2[j];

        if (maxLeft1 <= minRight2 && maxLeft2 <= minRight1) {
            if ((m + n) % 2 == 0) {
                return (Math.max(maxLeft1, maxLeft2) + Math.min(minRight1, minRight2)) / 2.0;
            } else {
                return Math.max(maxLeft1, maxLeft2);
            }
        } else if (maxLeft1 > minRight2) {
            high = i - 1;
        } else {
            low = i + 1;
        }
    }

    throw new IllegalArgumentException("Input arrays are not sorted");
}
```



