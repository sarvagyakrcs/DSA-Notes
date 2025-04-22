# Chapter Title: Finding Single Element in Sorted Array - Binary Search Application

## Overview
This chapter explores the problem of finding a single element in a sorted array where every other element appears exactly twice. This problem demonstrates how binary search can be applied to search problems beyond just finding a specific value, showing how to identify patterns that allow for efficient elimination in search spaces. The solution showcases both a linear and optimized logarithmic approach using binary search principles.

## Concept Map
```
┌─────────────────────────────┐
│ Finding Single Element in   │
│ Sorted Array                │
└───────────────┬─────────────┘
                │
    ┌───────────┴─────────────┐
    │                         │
┌───▼───────────┐      ┌──────▼─────────┐
│ O(n) Approach │      │ O(log n)       │
│ Linear Scan   │      │ Binary Search  │
└───────────────┘      └──────┬─────────┘
                              │
         ┌──────────────────┬─┴────────────┐
         │                  │              │
    ┌────▼────┐        ┌────▼────┐    ┌────▼────┐
    │ Search  │        │ Pattern │    │ Search  │
    │ Element │        │ Finding │    │ Space   │
    └─────────┘        └─────────┘    │ Handling│
                                      └─────────┘
```

## Detailed Sections

### 1. Problem Definition
- Given a sorted array where all elements appear exactly twice except one element which appears only once
- All elements that appear twice are adjacent to each other
- Task: Find the single element that appears only once
- Example: `[1,1,2,2,3,3,4,5,5,6,6]` → The answer is `4`

### 2. Brute Force Approach - O(n)

#### Key Strategy
- Linearly scan the array and check the neighbors of each element

#### Implementation Logic
- For any element at index i, check if it matches with either its left or right neighbor
- If neither matches, it's the single element
- Handle edge cases (first and last elements) separately
- For first element: Check if it's different from the second element
- For last element: Check if it's different from the second-to-last element

```cpp
int singleNonDuplicate(vector<int>& nums) {
    int n = nums.size();
    
    // If array has only one element
    if (n == 1) return nums[0];
    
    // Check first element
    if (nums[0] != nums[1]) return nums[0];
    
    // Check last element
    if (nums[n-1] != nums[n-2]) return nums[n-1];
    
    // Check all elements in the middle
    for (int i = 1; i < n-1; i++) {
        if (nums[i] != nums[i-1] && nums[i] != nums[i+1]) {
            return nums[i];
        }
    }
    
    return -1; // This should never execute if problem constraints are met
}
```

- Time Complexity: O(n) - linear scan of the array
- Space Complexity: O(1) - constant extra space

### 3. Optimized Approach - Binary Search O(log n)

#### Key Pattern Observation
- Before the single element, pairs start at even indices (0-indexed)
- After the single element, pairs start at odd indices
- This pattern shift is the key to applying binary search

| Index | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|-------|---|---|---|---|---|---|---|---|---|---|
| Value | 1 | 1 | 2 | 2 | 3 | 3 | 4 | 5 | 5 | 6 | 6 |
| Pair  | Even-Odd | Even-Odd | Even-Odd | Single | Odd-Even | Odd-Even |

#### Binary Search Implementation Logic
- **Eliminate Edge Cases First**:
  - Check if first or last element is the single element to simplify the main search logic
- **Search Space**: Limit to indices 1 to n-2 to avoid excessive edge case handling
- **Partition Property**:
  - If mid is on even index:
    - If nums[mid] == nums[mid+1]: Single element is on the right half
    - If nums[mid] != nums[mid+1]: Single element is on the left half or is the mid itself
  - If mid is on odd index:
    - If nums[mid] == nums[mid-1]: Single element is on the right half
    - If nums[mid] != nums[mid-1]: Single element is on the left half
- **Element Check**: At each step, check if mid element is the single element directly

```cpp
int singleNonDuplicate(vector<int>& nums) {
    int n = nums.size();
    
    // Handle edge cases
    if (n == 1) return nums[0];
    if (nums[0] != nums[1]) return nums[0];
    if (nums[n-1] != nums[n-2]) return nums[n-1];
    
    // Binary search on remaining elements
    int low = 1, high = n-2;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        // Check if mid is the single element
        if (nums[mid] != nums[mid+1] && nums[mid] != nums[mid-1]) {
            return nums[mid];
        }
        
        // Check which half to eliminate
        if ((mid % 2 == 1 && nums[mid] == nums[mid-1]) || 
            (mid % 2 == 0 && nums[mid] == nums[mid+1])) {
            // We are on the left half, single element is on right
            low = mid + 1;
        } else {
            // We are on the right half, single element is on left
            high = mid - 1;
        }
    }
    
    return -1; // This should never execute if problem constraints are met
}
```

- Time Complexity: O(log n) - binary search halves the search space each iteration
- Space Complexity: O(1) - constant extra space

### 4. Alternative Binary Search Implementation
Another way to think about the problem is that all pairs are in their "correct positions" before the single element, and shifted after it.

```cpp
int singleNonDuplicate(vector<int>& nums) {
    int n = nums.size();
    int left = 0, right = n-1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        // Make sure mid is even
        if (mid % 2 == 1) mid--;
        
        // If mid and next element are same, single element is on the right
        if (nums[mid] == nums[mid+1]) {
            left = mid + 2;
        } else {
            // Single element is on the left or is mid itself
            right = mid;
        }
    }
    
    return nums[left];
}
```

## Flash cards for Quick Spaced Repetition Revision

1. **Front**: What's the key pattern in the "single element in sorted array" problem?
   **Back**: Before the single element, pairs start at even indices; after it, pairs start at odd indices.

2. **Front**: Why trim the search space to [1, n-2] in the binary search approach?
   **Back**: To avoid extra edge case handling when checking neighbors of elements at the boundaries.

3. **Front**: If at an even index mid, nums[mid] equals nums[mid+1], what does it tell us?
   **Back**: We are on the left half of the array (before the single element), so we should search in the right half.

4. **Front**: If at an odd index mid, nums[mid] equals nums[mid-1], what does it tell us?
   **Back**: We are on the left half of the array (before the single element), so we should search in the right half.

5. **Front**: How do we check if the current mid element is the single element?
   **Back**: If nums[mid] is not equal to both its left neighbor (nums[mid-1]) and its right neighbor (nums[mid+1]).

6. **Front**: What's the time complexity of the binary search approach to this problem?
   **Back**: O(log n)

7. **Front**: What's the key insight that allows binary search to work for this problem?
   **Back**: The shift in pair alignment patterns at the single element creates a searchable property.

8. **Front**: Why do we check the first and last elements separately?
   **Back**: To simplify the main search logic and avoid boundary checks during binary search.

## Practical Examples

### Example 1: Basic Example
Input array: [1,1,2,2,3,4,4,5,5]
Output: 3

Tracing through the binary search:
1. low=1, high=7, mid=4 (value 4)
2. Check: 4!=3 and 4==4 → not single element
3. mid is even, nums[mid]==nums[mid+1] → we're on left half
4. low=5, high=7, mid=6 (value 5)
5. Check: 5==5 and 5==5 → not single element
6. mid is even, nums[mid]==nums[mid+1] → we're on left half
7. low=7, high=7, mid=7 (value 5)
8. Check: 5!=4 and 5==5 → not single element
9. mid is odd, nums[mid]==nums[mid-1] → we're on left half
10. low=8, high=7 → loop ends
11. Return single element previously found at index 2: 3

### Example 2: Single Element at the Start
Input array: [1,2,2,3,3,4,4,5,5]
Output: 1

This is caught by the edge case check: nums[0] != nums[1]

### Example 3: Single Element at the End
Input array: [1,1,2,2,3,3,4,4,5]
Output: 5

This is caught by the edge case check: nums[n-1] != nums[n-2]

## Interview Preparation Tips

### Key Questions to Expect
1. **Brute Force vs. Optimized**: Can you explain both the O(n) and O(log n) approaches to this problem?
2. **Pattern Recognition**: How did you identify the pattern that allows binary search to work?
3. **Edge Cases**: What edge cases need to be handled and why?
4. **Invariant Property**: What property remains consistent throughout the binary search that guides the elimination process?
5. **Time and Space Complexity**: Can you analyze the time and space complexity of both approaches?
6. **Code Cleanliness**: How can you make the solution more readable without sacrificing efficiency?
7. **Related Problems**: How would you adapt this approach if elements appeared three times except for one?
8. **Alternative Solutions**: Is there a bitwise XOR solution to this problem? How does it compare?

### Key Concepts to Master
- Pattern recognition in sorted arrays
- Adapting binary search for non-traditional search problems
- Handle edge cases cleanly
- Understanding how to establish and maintain search invariants
- Recognizing when to trim the search space for cleaner code

## Common Mistakes or Misunderstandings

1. **Forgetting Edge Cases**: Not handling the single element at the start or end of the array.
2. **Index Confusion**: Getting confused between even and odd indices in the pattern analysis.
3. **Boundary Conditions**: Incorrect handling of boundary conditions in binary search (e.g., using < vs <=).
4. **Over-complicated Checks**: Writing too many conditional checks instead of finding the elegant pattern.
5. **Missing the Pattern**: Failing to recognize the even-odd pattern shift at the single element.
6. **Inconsistent Binary Search Implementation**: Switching between different binary search styles (e.g., low < high vs low <= high) leading to off-by-one errors.
7. **Using Extra Space**: Attempting to use additional data structures when O(1) space is sufficient.

## Advanced Notes / Behind the Scenes

1. **Generalization**: This problem can be generalized to find an element that appears K times when all others appear L times in a sorted array.
2. **Bit Manipulation Alternative**: For this specific problem, XORing all elements would also give the single element in O(n) time, but doesn't utilize the sorted property.
3. **Eliminating Edge Cases**: The technique of trimming the search space to avoid boundary checks is a common pattern in interview-style binary search problems.
4. **Search Space Partitioning**: The core binary search principle is finding a condition that cleanly partitions the search space into "can contain answer" and "cannot contain answer" sections.
5. **Binary Search Templates**: This problem fits into a class of binary search problems where we're not searching for a specific value, but rather a position where some property changes.

## Glossary

- **Binary Search**: A divide-and-conquer algorithm that finds the position of a target value within a sorted array by repeatedly dividing the search space in half.
- **Search Space**: The range of indices within which we are searching for our target.
- **Elimination**: The process of discarding a portion of the search space that cannot contain the target element.
- **Invariant**: A property that remains true throughout the execution of an algorithm.
- **Edge Case**: A problem or situation that occurs only at an extreme (maximum or minimum) parameter value.
- **Linear Search**: An algorithm that sequentially checks each element of a list until the target is found or the whole list has been searched.
