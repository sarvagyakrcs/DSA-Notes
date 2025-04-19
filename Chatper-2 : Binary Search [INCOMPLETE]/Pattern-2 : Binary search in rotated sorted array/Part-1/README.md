# Binary Search in Rotated Sorted Arrays

## Overview
This chapter explores an important variation of the binary search algorithm: searching in a rotated sorted array. This problem frequently appears in technical interviews and demonstrates how the classic binary search algorithm can be adapted to handle partially ordered data. We'll cover the approach for arrays with unique elements, understand what makes this problem challenging, and implement an efficient O(log n) solution.

## Concept Map

```
┌───────────────────────────────────────────────────────────────────────┐
│                  BINARY SEARCH IN ROTATED SORTED ARRAY                 │
└───────────────────────────────────────────────────────────────────────┘
                 │
    ┌────────────┴────────────┐
    ▼                         ▼
┌─────────────────┐    ┌─────────────────────┐
│ UNDERSTANDING   │    │ ALGORITHM           │
│ ROTATED ARRAYS  │    │ IMPLEMENTATION      │
└─────────────────┘    └─────────────────────┘
    │                         │
    ▼                         ▼
┌─────────────────┐    ┌─────────────────────┐
│ - Definition    │    │ 1. Find mid element │
│ - Examples      │    │ 2. Check if mid     │
│   [7,8,9,1,2,3] │    │    equals target    │
│   (Rotated at 7)│    │ 3. Identify the     │
└─────────────────┘    │    sorted half      │
                       │ 4. Check if target  │
                       │    is in sorted half│
                       │ 5. Eliminate half   │
                       │    & repeat         │
                       └─────────────────────┘
                                │
                    ┌───────────┴───────────┐
                    ▼                       ▼
        ┌─────────────────────┐   ┌─────────────────────┐
        │ TIME COMPLEXITY     │   │ SPACE COMPLEXITY    │
        │ O(log n)            │   │ O(1)                │
        └─────────────────────┘   └─────────────────────┘
```

## Detailed Sections

### 1. Understanding Rotated Sorted Arrays

#### What is a Rotated Sorted Array?
- **Definition**: A rotated sorted array is created when a sorted array is split at some pivot point and the second part is moved to the front
- **Example**: Original sorted array: `[1,2,3,4,5,6,7,8,9]`
- **Rotation**: If we rotate at position 7, we get: `[7,8,9,1,2,3,4,5,6]`
- **Key property**: After rotation, the array has two sorted subarrays, with all elements in the first subarray being greater than elements in the second subarray

#### Problem Statement
- **Task**: Given a rotated sorted array and a target value, return the index of the target if it exists in the array; otherwise, return -1
- **Constraints**: 
  - The array contains only unique elements
  - The solution should have O(log n) time complexity

### 2. Solution Approaches

#### Linear Search Approach
- **Method**: Check each element in the array until target is found
- **Time Complexity**: O(n) - inefficient for large arrays
- **Space Complexity**: O(1)
- **Drawback**: Doesn't leverage the sorted property of the array

#### Binary Search Approach
- **Key Insight**: Although the array is rotated, we can still use binary search by identifying the sorted half at each step
- **Challenge**: Standard binary search elimination logic doesn't work directly
- **Solution**: Identify which half is sorted, then check if target is in that sorted range

### 3. Binary Search Algorithm for Rotated Sorted Arrays

#### Algorithm Steps:
1. Initialize `low = 0` and `high = n-1`
2. While `low <= high`:
   - Calculate `mid = (low + high) / 2`
   - If `arr[mid] == target`, return `mid`
   - Identify which half is sorted:
     - If `arr[low] <= arr[mid]`, left half is sorted
     - Otherwise, right half is sorted
   - Based on which half is sorted:
     - Check if target lies within the sorted half's range
     - Adjust `low` or `high` accordingly to eliminate half of the array

#### Critical Insight:
- In a rotated sorted array, at least one half (either left or right) must always be properly sorted
- We can determine if a half is sorted by comparing its boundary elements
- Once we know which half is sorted, we can check if the target lies within that sorted range
- If the target is in the sorted range, eliminate the other half; otherwise, eliminate the sorted half

### 4. Implementation

```cpp
int search(vector<int>& nums, int target) {
    int n = nums.size();
    int low = 0;
    int high = n - 1;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        // Found target
        if (nums[mid] == target) {
            return mid;
        }
        
        // Check if left half is sorted
        if (nums[low] <= nums[mid]) {
            // Check if target is in left sorted half
            if (nums[low] <= target && target < nums[mid]) {
                high = mid - 1; // Eliminate right half
            } else {
                low = mid + 1;  // Eliminate left half
            }
        } 
        // Right half is sorted
        else {
            // Check if target is in right sorted half
            if (nums[mid] < target && target <= nums[high]) {
                low = mid + 1;  // Eliminate left half
            } else {
                high = mid - 1; // Eliminate right half
            }
        }
    }
    
    return -1; // Target not found
}
```

### 5. Walkthrough with Example

**Example**: `nums = [7,8,9,1,2,3]`, `target = 1`

Step-by-step:
1. Initialize `low = 0`, `high = 5`
2. Calculate `mid = 2` (value = 9)
3. 9 != 1, so check which half is sorted
4. Left half [7,8,9] is sorted because `nums[low] <= nums[mid]` (7 <= 9)
5. Check if target is in left half: `nums[low] <= target && target < nums[mid]` (7 <= 1 && 1 < 9) is false
6. So target must be in right half, update `low = mid + 1 = 3`
7. Now search range is [1,2,3]
8. Calculate `mid = 4` (value = 2)
9. 2 != 1, so check which half is sorted
10. Left half [1,2] is sorted because `nums[low] <= nums[mid]` (1 <= 2)
11. Check if target is in left half: `nums[low] <= target && target < nums[mid]` (1 <= 1 && 1 < 2) is true
12. Update `high = mid - 1 = 3`
13. Now search range is just [1]
14. Calculate `mid = 3` (value = 1)
15. 1 == 1, return `mid = 3`

## Flash Cards for Quick Spaced Repetition Revision

1. **Front**: What is a rotated sorted array?  
   **Back**: A sorted array that has been split at some pivot point, with the second part moved to the front. Example: [1,2,3,4,5] rotated becomes [4,5,1,2,3].

2. **Front**: What is the time complexity of searching in a rotated sorted array using binary search?  
   **Back**: O(log n)

3. **Front**: How do you identify which half of a rotated sorted array is sorted?  
   **Back**: Check if the leftmost element is less than or equal to the middle element. If true, the left half is sorted; otherwise, the right half is sorted.

4. **Front**: What is the key insight for applying binary search to a rotated sorted array?  
   **Back**: At least one half of the array will always be sorted, and we can check if the target is within the range of the sorted half.

5. **Front**: If the target is not in the sorted half of a rotated array, what should you do?  
   **Back**: Eliminate the sorted half and continue the search in the unsorted half.

6. **Front**: How do you check if a target is within a sorted range?  
   **Back**: Check if the target is greater than or equal to the start value AND less than or equal to the end value of the range.

7. **Front**: What's the base condition to terminate the binary search?  
   **Back**: When `low > high` (target not found) or when `nums[mid] == target` (target found).

8. **Front**: Why can't we use standard binary search directly on rotated sorted arrays?  
   **Back**: Because the standard elimination logic assumes the entire array is sorted, which isn't true for rotated arrays.

## Practical Examples

### Problem: Search in Rotated Sorted Array (LeetCode 33)

```cpp
// Example 1:
// Input: nums = [4,5,6,7,0,1,2], target = 0
// Output: 4

// Example 2:
// Input: nums = [4,5,6,7,0,1,2], target = 3
// Output: -1

// Example 3:
// Input: nums = [1], target = 0
// Output: -1

class Solution {
public:
    int search(vector<int>& nums, int target) {
        int low = 0;
        int high = nums.size() - 1;
        
        while (low <= high) {
            int mid = low + (high - low) / 2;
            
            // Found target
            if (nums[mid] == target) {
                return mid;
            }
            
            // Check if left half is sorted
            if (nums[low] <= nums[mid]) {
                // Check if target is in left sorted half
                if (nums[low] <= target && target < nums[mid]) {
                    high = mid - 1; // Eliminate right half
                } else {
                    low = mid + 1;  // Eliminate left half
                }
            } 
            // Right half is sorted
            else {
                // Check if target is in right sorted half
                if (nums[mid] < target && target <= nums[high]) {
                    low = mid + 1;  // Eliminate left half
                } else {
                    high = mid - 1; // Eliminate right half
                }
            }
        }
        
        return -1; // Target not found
    }
};
```

### Extension: Search in Rotated Sorted Array with Duplicates

For arrays with duplicates, additional considerations are needed:

```cpp
// When duplicates are allowed, the condition nums[low] <= nums[mid]
// isn't sufficient to determine which half is sorted.
// We need to handle the case when nums[low] == nums[mid]

int search(vector<int>& nums, int target) {
    int low = 0;
    int high = nums.size() - 1;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        if (nums[mid] == target) {
            return mid;
        }
        
        // Handle the case when left and mid elements are same
        if (nums[low] == nums[mid]) {
            low++;
            continue;
        }
        
        // Check if left half is sorted
        if (nums[low] < nums[mid]) {
            // Check if target is in left sorted half
            if (nums[low] <= target && target < nums[mid]) {
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        } 
        // Right half is sorted
        else {
            // Check if target is in right sorted half
            if (nums[mid] < target && target <= nums[high]) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
    }
    
    return -1;
}
```

## Interview Preparation Tips

1. **Common Question**: "How would you search for an element in a rotated sorted array?"
   - Emphasize the O(log n) time complexity requirement
   - Explain how to identify the sorted portion
   - Show how to determine where the target might be

2. **Follow-up Question**: "What if the array contains duplicates?"
   - Mention the edge case where `nums[low] == nums[mid]`
   - Explain how this can break the logic for identifying the sorted half
   - Show how to handle this by incrementing `low`
   - Note that with duplicates, worst-case time complexity becomes O(n)

3. **Conceptual Question**: "How do you know which half of a rotated sorted array is sorted?"
   - Explain that comparing boundary elements (`low` and `mid`) can determine this
   - Demonstrate with examples

4. **Edge Cases to Consider**:
   - Array with just one element
   - Target not present in the array
   - Array not rotated (regular sorted array)
   - Array fully rotated (original sorted array)

5. **Optimization Question**: "Can we optimize the implementation further?"
   - Discuss using `low + (high - low) / 2` instead of `(low + high) / 2` to prevent integer overflow
   - Mention handling special cases upfront for very small arrays

6. **Variation Question**: "How would you find the minimum element in a rotated sorted array?"
   - Explain that this is similar but focuses on finding the pivot point
   - The element immediately after the pivot is the minimum

7. **Extension Question**: "Can you find the number of rotations in a rotated sorted array?"
   - The number of rotations equals the index of the minimum element

8. **Time/Space Analysis Question**: "What is the time and space complexity of your solution?"
   - Time: O(log n) for arrays without duplicates, potentially O(n) for arrays with duplicates
   - Space: O(1) as we're using only constant extra space

## Common Mistakes or Misunderstandings

1. **Directly applying standard binary search**
   - The standard binary search elimination logic doesn't work because the entire array isn't sorted.
   - Solution: Identify the sorted half before deciding which half to eliminate.

2. **Incorrect condition for identifying sorted halves**
   - Using only `nums[low] < nums[mid]` without considering equality.
   - Solution: Use `nums[low] <= nums[mid]` to correctly identify the sorted left half.

3. **Not handling duplicates properly**
   - When duplicates exist, `nums[low] == nums[mid]` doesn't tell us which half is sorted.
   - Solution: Handle this case separately by incrementing `low` and continuing.

4. **Off-by-one errors in range checks**
   - Incorrectly specifying range boundaries when checking if target is in sorted half.
   - Solution: Be precise about inclusive/exclusive inequalities (≤ vs <).

5. **Integer overflow in mid calculation**
   - Using `(low + high) / 2` can cause integer overflow for large arrays.
   - Solution: Use `low + (high - low) / 2` instead.

6. **Not returning -1 when target is not found**
   - Forgetting to handle the case when the target doesn't exist in the array.
   - Solution: Return -1 after the while loop completes.

## Advanced Notes / Behind the Scenes

### Why Binary Search Works on Rotated Sorted Arrays

Binary search fundamentally relies on the ability to eliminate portions of the search space. In a rotated sorted array, even though the entire array isn't sorted, we can always identify at least one sorted half at each step. This property guarantees that we can make an informed decision about which half to eliminate, maintaining the logarithmic time complexity.

### Mathematical Proof of Correctness

The algorithm works because:
1. Every rotated sorted array consists of exactly two sorted subarrays
2. The pivot point (where rotation occurred) separates these two subarrays
3. At any division point (mid), at least one half must be entirely within one of these sorted subarrays
4. We can identify which half is sorted by comparing boundary elements
5. If the target is not in the sorted half (which we can determine by range checking), it must be in the other half

### Extension to Find Rotation Count

The number of rotations equals the index of the minimum element. This can be found using a modified binary search:

```cpp
int findRotationCount(vector<int>& nums) {
    int n = nums.size();
    int low = 0;
    int high = n - 1;
    
    // Handle edge cases
    if (n == 1 || nums[low] < nums[high]) {
        return 0; // Array is not rotated
    }
    
    while (low <= high) {
        // If array is already sorted
        if (nums[low] <= nums[high]) {
            return low;
        }
        
        int mid = low + (high - low) / 2;
        int next = (mid + 1) % n;
        int prev = (mid + n - 1) % n;
        
        // Check if mid is the minimum element
        if (nums[mid] <= nums[next] && nums[mid] <= nums[prev]) {
            return mid;
        }
        
        // If right half is sorted, minimum must be in left half
        if (nums[mid] <= nums[high]) {
            high = mid - 1;
        }
        // If left half is sorted, minimum must be in right half
        else if (nums[low] <= nums[mid]) {
            low = mid + 1;
        }
    }
    
    return 0; // Default case
}
```

## Glossary / Technical Terms

- **Rotated Sorted Array**: A sorted array that has been divided at a pivot point, with the second part moved to the front
- **Pivot Point**: The index where rotation occurs in a rotated sorted array
- **Binary Search**: A search algorithm that finds the position of a target value within a sorted array by repeatedly dividing the search space in half
- **Elimination Strategy**: The approach of systematically discarding portions of the array that cannot contain the target
- **Time Complexity**: A measure of the amount of time an algorithm takes to run as a function of the length of the input
- **Space Complexity**: A measure of the amount of memory an algorithm requires as a function of the length of the input
- **In-place Algorithm**: An algorithm that doesn't require extra space and transforms input using no auxiliary data structure
- **Target**: The value being searched for in the array
