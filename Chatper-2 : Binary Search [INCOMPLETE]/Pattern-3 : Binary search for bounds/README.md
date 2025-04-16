# Title: Bounds in Binary Search

## Overview
This chapter covers the concepts of lower bound and upper bound algorithms in binary search, which are essential extensions of the standard binary search technique. These algorithms are particularly useful for working with sorted arrays when you need to find specific positions or elements with particular relationships to a target value. Understanding lower and upper bound operations is crucial for solving many algorithm problems efficiently, especially in competitive programming and technical interviews. The chapter explains their implementations, usage scenarios, time complexity, and practical applications with detailed examples.

## Concept Map

```
                   BINARY SEARCH VARIANTS
                           |
           +---------------+---------------+
           |                               |
      LOWER BOUND                     UPPER BOUND
           |                               |
Find smallest index i where         Find smallest index i where
    arr[i] >= target                   arr[i] > target
           |                               |
           +---------------+---------------+
                           |
                  APPLICATION SCENARIOS
                           |
     +----------+----------+----------+----------+
     |          |          |          |          |
First larger  Insert     Find floor  Find ceil  Find count
or equal     position                            of occurrences
```

## Detailed Sections

### 1. Definition of Lower Bound

- The **lower bound** of a number X in a sorted array is the **smallest index i** where the value at that index is **greater than or equal to X**.
- Key characteristics:
  - Returns the first occurrence of X if X exists in the array
  - Returns the first element greater than X if X doesn't exist
  - Returns the array size if all elements are smaller than X

#### Example of Lower Bound:
For array `[1, 3, 5, 7, 9, 9, 10]`:
- Lower bound of 5 → index 2 (first occurrence of 5)
- Lower bound of 6 → index 3 (first element ≥ 6 is 7 at index 3)
- Lower bound of 11 → index 7 (hypothetical index after the array)

### 2. Definition of Upper Bound

- The **upper bound** of a number X in a sorted array is the **smallest index i** where the value at that index is **strictly greater than X**.
- Key characteristics:
  - Returns the index of the first element greater than X
  - Returns the index after the last occurrence of X if X exists
  - Returns the array size if all elements are less than or equal to X

#### Example of Upper Bound:
For array `[1, 3, 5, 7, 9, 9, 10]`:
- Upper bound of 5 → index 3 (first element > 5 is 7 at index 3)
- Upper bound of 6 → index 3 (first element > 6 is 7 at index 3)
- Upper bound of 9 → index 6 (first element > 9 is 10 at index 6)
- Upper bound of 11 → index 7 (hypothetical index after the array)

### 3. Lower Bound Implementation

#### Linear Search Approach (O(n)):
- Iterate through the array from left to right
- Return the first index where the element is greater than or equal to the target

#### Binary Search Approach (O(log n)):
- Use binary search to efficiently find the lower bound
- Keep track of a potential answer while searching
- When an element ≥ target is found, store its index and continue searching left
- When an element < target is found, search right

```cpp
int lowerBound(vector<int>& arr, int target) {
    int low = 0;
    int high = arr.size() - 1;
    int answer = arr.size(); // Default answer if no element is found
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        if (arr[mid] >= target) {
            // This could be the answer, but we need to look for smaller indices
            answer = mid;
            high = mid - 1; // Search in the left half
        } else {
            // Element is smaller, look in the right half
            low = mid + 1;
        }
    }
    
    return answer;
}
```

#### Using C++ STL:
```cpp
// For arrays:
int idx = lower_bound(arr, arr + n, target) - arr;

// For vectors:
int idx = lower_bound(vec.begin(), vec.end(), target) - vec.begin();
```

### 4. Upper Bound Implementation

#### Binary Search Approach (O(log n)):

```cpp
int upperBound(vector<int>& arr, int target) {
    int low = 0;
    int high = arr.size() - 1;
    int answer = arr.size(); // Default answer if no element is found
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        if (arr[mid] > target) {
            // This could be the answer, but we need to look for smaller indices
            answer = mid;
            high = mid - 1; // Search in the left half
        } else {
            // Element is smaller or equal, look in the right half
            low = mid + 1;
        }
    }
    
    return answer;
}
```

#### Using C++ STL:
```cpp
// For arrays:
int idx = upper_bound(arr, arr + n, target) - arr;

// For vectors:
int idx = upper_bound(vec.begin(), vec.end(), target) - vec.begin();
```

### 5. Time Complexity Analysis

- **Lower Bound** using binary search: O(log n)
- **Upper Bound** using binary search: O(log n)
- Using linear search: O(n)

### 6. Common Applications

#### Search Insert Position
- Problem: Find the index where a target would be inserted to maintain the sorted order
- Solution: This is exactly what lower bound does!

```cpp
int searchInsertPosition(vector<int>& nums, int target) {
    return lowerBound(nums, target);
}
```

#### Floor and Ceiling in Sorted Array
- **Floor**: Largest element in array ≤ target (reverse of upper bound)
- **Ceiling**: Smallest element in array ≥ target (same as lower bound)

```cpp
int findFloor(vector<int>& arr, int target) {
    int low = 0;
    int high = arr.size() - 1;
    int answer = -1; // -1 if no floor exists
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        if (arr[mid] <= target) {
            // This is a potential answer
            answer = arr[mid];
            low = mid + 1; // Search in right half for a larger floor
        } else {
            high = mid - 1; // Search in left half
        }
    }
    
    return answer;
}

int findCeiling(vector<int>& arr, int target) {
    int idx = lowerBound(arr, target);
    
    if (idx == arr.size())
        return -1; // No ceiling exists
    
    return arr[idx];
}
```

#### Count of Elements
- Problem: Count occurrences of an element in a sorted array
- Solution: `upperBound(arr, target) - lowerBound(arr, target)`

```cpp
int countOccurrences(vector<int>& arr, int target) {
    int first = lowerBound(arr, target);
    int last = upperBound(arr, target);
    
    // If element doesn't exist, first will point to an element > target
    if (first == arr.size() || arr[first] != target)
        return 0;
        
    return last - first;
}
```

## Flash Cards for Quick Spaced Repetition

1. **Q**: What is the lower bound of a number X in a sorted array?  
   **A**: The smallest index i where the value at that index is greater than or equal to X.

2. **Q**: What is the upper bound of a number X in a sorted array?  
   **A**: The smallest index i where the value at that index is strictly greater than X.

3. **Q**: What does lower_bound return if the target doesn't exist in the array?  
   **A**: The index of the first element greater than the target, or the array size if no such element exists.

4. **Q**: What does upper_bound return if the target exists multiple times in the array?  
   **A**: The index immediately after the last occurrence of the target.

5. **Q**: What is the time complexity of lower_bound and upper_bound using binary search?  
   **A**: O(log n), where n is the size of the array.

6. **Q**: How do you find the count of occurrences of an element in a sorted array?  
   **A**: Upper bound - Lower bound

7. **Q**: In C++, how do you get the index from lower_bound when using STL?  
   **A**: By subtracting the beginning iterator: `lower_bound(arr, arr+n, x) - arr`

8. **Q**: How do you find the floor of a number in a sorted array?  
   **A**: The largest element in the array that is less than or equal to the target.

## Practical Examples

### Example 1: Finding First and Last Position

Problem: Find the first and last occurrence of a target element in a sorted array.

```cpp
vector<int> searchRange(vector<int>& nums, int target) {
    int first = lower_bound(nums.begin(), nums.end(), target) - nums.begin();
    
    // Check if the element exists
    if (first == nums.size() || nums[first] != target)
        return {-1, -1};
    
    int last = upper_bound(nums.begin(), nums.end(), target) - nums.begin() - 1;
    return {first, last};
}
```

### Example 2: Count of Elements Less Than or Equal To

Problem: Count elements less than or equal to a target in a sorted array.

```cpp
int countLessThanOrEqual(vector<int>& nums, int target) {
    return upper_bound(nums.begin(), nums.end(), target) - nums.begin();
}
```

### Example 3: Find Median in Two Sorted Arrays (Hard)

This is a partial solution demonstrating binary search usage:

```cpp
double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
    // Ensure nums1 is the smaller array
    if (nums1.size() > nums2.size())
        return findMedianSortedArrays(nums2, nums1);
    
    int x = nums1.size();
    int y = nums2.size();
    int low = 0, high = x;
    
    while (low <= high) {
        int partitionX = (low + high) / 2;
        int partitionY = (x + y + 1) / 2 - partitionX;
        
        // Handle edge cases
        int maxX = (partitionX == 0) ? INT_MIN : nums1[partitionX - 1];
        int maxY = (partitionY == 0) ? INT_MIN : nums2[partitionY - 1];
        
        int minX = (partitionX == x) ? INT_MAX : nums1[partitionX];
        int minY = (partitionY == y) ? INT_MAX : nums2[partitionY];
        
        if (maxX <= minY && maxY <= minX) {
            // Found the correct partition
            if ((x + y) % 2 == 0) {
                return (max(maxX, maxY) + min(minX, minY)) / 2.0;
            } else {
                return max(maxX, maxY);
            }
        } else if (maxX > minY) {
            high = partitionX - 1;
        } else {
            low = partitionX + 1;
        }
    }
    
    return 0.0; // Will not reach here if arrays are valid
}
```

## Interview Preparation Tips

1. **Know the difference**: Be very clear about the difference between lower_bound and upper_bound - one includes equality, the other doesn't.

2. **Edge cases**: Always consider:
   - Target smaller than all array elements
   - Target larger than all array elements
   - Target appears multiple times in the array
   - Empty array

3. **Time complexity**: Be prepared to explain why binary search gives O(log n) complexity.

4. **Without STL**: Practice implementing lower_bound and upper_bound without using built-in functions.

5. **Common bugs**: Watch out for integer overflow in the calculation of mid index. Use `mid = low + (high - low) / 2` instead of `mid = (low + high) / 2`.

6. **Application questions**:
   - How would you use lower_bound/upper_bound to find:
     - The largest element smaller than X
     - The count of elements in a range [X, Y]
     - Whether an element exists exactly X times in a sorted array

7. **Common optimization**: When returning the low or high pointer as the answer, you often don't need an extra 'answer' variable.

8. **Problem translation**: Practice recognizing when a problem can be solved using these functions.

## Common Mistakes or Misunderstandings

1. **Confusing lower_bound and upper_bound**: Always remember lower_bound is ≥, upper_bound is >.

2. **Default return value**: Forgetting to handle the case when no suitable element exists.

3. **Incorrect indexing**: When using STL, forgetting to subtract the beginning iterator to get the index.

4. **Updating answer variable**: Not updating the answer variable correctly in the binary search implementation.

5. **Infinite loops**: Incorrect updating of low and high pointers leading to infinite loops.

6. **Hard-coding return values**: Returning specific indices without generalizing the solution.

7. **Off-by-one errors**: Especially common when dealing with boundaries and ranges.

8. **Incorrect assumptions**: Assuming the array doesn't contain duplicates when it might.

## Tooling & IDE Integration

### In Visual Studio:
- Use the STL algorithms `std::lower_bound` and `std::upper_bound` from the `<algorithm>` header.
- Utilize the debugger to visualize the binary search process step by step.

### In CLion/Visual Studio Code:
- Set up code templates for common binary search patterns.
- Use breakpoints to inspect the values of low, mid, and high pointers during execution.

### Competitive Programming Tools:
- Use online judges like LeetCode, Codeforces, HackerRank to practice problems involving these concepts.
- For C++ users: Create shorthand versions of these functions for quick implementation during contests.

## Advanced Notes / Behind the Scenes

### Eliminating the Answer Variable:
In many implementations, you can eliminate the separate answer variable:

```cpp
int lowerBound(vector<int>& arr, int target) {
    int low = 0;
    int high = arr.size() - 1;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        if (arr[mid] >= target) {
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }
    
    return low; // Low will point to the lower bound
}
```

### Binary Search Direction Analysis:
- In lower_bound, when we find a candidate (arr[mid] >= target), we move left to find a potentially smaller valid index.
- This ensures that when the algorithm terminates, low points to the smallest index satisfying the condition.
- Similarly, for upper_bound, low will always end up pointing to the smallest index where arr[mid] > target.

### Generalized Binary Search Template:
Binary searches can be generalized as finding the smallest index where a condition becomes true:

```cpp
int binarySearch(vector<int>& arr, function<bool(int)> condition) {
    int low = 0;
    int high = arr.size() - 1;
    int result = arr.size(); // Default if condition is never satisfied
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        if (condition(mid)) {
            result = mid;
            high = mid - 1; // Look for an earlier occurrence
        } else {
            low = mid + 1;
        }
    }
    
    return result;
}

// Usage examples:
// lowerBound: condition = [](int i) { return arr[i] >= target; }
// upperBound: condition = [](int i) { return arr[i] > target; }
```

## Footnotes / Glossary

### Glossary:
- **Binary Search**: A divide-and-conquer search algorithm that works on sorted arrays with O(log n) time complexity.
- **Lower Bound**: Smallest index i where arr[i] ≥ target.
- **Upper Bound**: Smallest index i where arr[i] > target.
- **Floor**: Largest element ≤ target.
- **Ceiling**: Smallest element ≥ target.
- **STL**: Standard Template Library, a set of C++ template classes to provide common programming data structures and functions.

### Platform-Specific Notes:
- In Java: Use `Collections.binarySearch()` or `Arrays.binarySearch()`.
- In Python: No direct equivalents, but can use `bisect.bisect_left` (similar to lower_bound) and `bisect.bisect_right` (similar to upper_bound).
- In JavaScript: No built-in functions; must implement manually.

### References:
- C++ STL documentation: [lower_bound](https://en.cppreference.com/w/cpp/algorithm/lower_bound) and [upper_bound](https://en.cppreference.com/w/cpp/algorithm/upper_bound)
- Competitive Programming Handbooks and Guides
- Common coding interview problem collections
