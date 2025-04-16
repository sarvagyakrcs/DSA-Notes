# Binary Search Algorithm: Efficient Searching in Sorted Collections

## Overview
Binary search is an efficient searching algorithm that works on sorted collections. Unlike linear search which examines every element, binary search divides the search space in half with each comparison, allowing it to find elements in logarithmic time (O(log n)). This chapter covers the fundamentals of binary search, its implementation (both iterative and recursive), time complexity analysis, and considerations for edge cases like integer overflow.

## Concept Map
```
                                   BINARY SEARCH
                                         |
          +-----------------------------+-----------------------------+
          |                             |                             |
    PREREQUISITES                   ALGORITHM                     ANALYSIS
          |                             |                             |
    Sorted collection            +------+------+                +-----+-----+
                                 |             |                |           |
                           APPROACHES    SEARCH SPACE    Time complexity  Edge cases
                                 |         reduction         O(log n)    Integer overflow
                        +--------+---------+
                        |                  |
                   ITERATIVE          RECURSIVE
                        |                  |
                    while loop         Base case
                                      Function calls
```

## Detailed Sections

### 1. Understanding Binary Search

- **Definition**: Binary search is a searching algorithm that works on sorted collections by repeatedly dividing the search space in half.
- **Prerequisites**:
  - The collection must be sorted (in ascending or descending order)
  - Random access to elements is required (e.g., arrays, not linked lists)
- **Real-world Analogy**: Finding a word in a dictionary
  - Opening the dictionary in the middle
  - Checking if the word comes before or after the page
  - Eliminating half of the remaining pages
  - Repeating until the word is found

### 2. Binary Search Algorithm

#### Search Space Concept
- **Search space**: The portion of the collection where the target might be located
- **Pointers**: Low (`low`) and High (`high`) pointers define the current search space
- Initially: `low = 0, high = array.length - 1`

#### Core Algorithm Steps
1. Calculate the middle index: `mid = low + (high - low) / 2`
2. Compare the middle element with the target
3. If found, return the index
4. If target < middle element, search in the left half: `high = mid - 1`
5. If target > middle element, search in the right half: `low = mid + 1`
6. Repeat until the element is found or the search space is exhausted

#### Termination Conditions
- **Success**: Element found at `mid` index
- **Failure**: `low > high` (search space exhausted)

### 3. Iterative Implementation

```
function binarySearch(array, size, target):
    low = 0
    high = size - 1
    
    while low <= high:
        mid = low + (high - low) / 2
        
        if array[mid] == target:
            return mid  // Element found
        else if target > array[mid]:
            low = mid + 1  // Look in right half
        else:
            high = mid - 1  // Look in left half
    
    return -1  // Element not found
```

### 4. Recursive Implementation

```
function binarySearchRecursive(array, low, high, target):
    // Base case: search space exhausted
    if low > high:
        return -1
    
    mid = low + (high - low) / 2
    
    // Check if found
    if array[mid] == target:
        return mid
    // Check if target is on right side
    else if target > array[mid]:
        return binarySearchRecursive(array, mid + 1, high, target)
    // Otherwise, target is on left side
    else:
        return binarySearchRecursive(array, low, mid - 1, target)
```

### 5. Time Complexity Analysis

- **Time Complexity**: O(log n), where n is the size of the array
- **Proof**:
  - Each iteration reduces the search space by half
  - For an array of size n, the worst-case number of steps is log₂(n)
  - Example: For an array of size 32 (2⁵), at most 5 steps are needed

- **Space Complexity**:
  - Iterative: O(1) - uses constant extra space
  - Recursive: O(log n) - due to the call stack

### 6. Edge Cases and Optimizations

#### Integer Overflow Consideration
- When the array size is large (close to INT_MAX), calculating `mid` as `(low + high) / 2` may cause integer overflow
- **Solution**: Use `low + (high - low) / 2` to prevent overflow
- **Reason**: Mathematically equivalent but prevents `low + high` from exceeding the integer limits

#### Other Edge Cases
- Empty array (`size = 0`)
- Single element array
- Target not present in the array
- Duplicate elements in the array

## Flash Cards for Quick Revision

1. **Front**: What is the prerequisite for applying binary search?
   **Back**: The collection must be sorted.

2. **Front**: What is the time complexity of binary search?
   **Back**: O(log n), where n is the size of the collection.

3. **Front**: How do you prevent integer overflow when calculating mid in binary search?
   **Back**: Use `mid = low + (high - low) / 2` instead of `mid = (low + high) / 2`.

4. **Front**: What termination condition indicates the target is not found?
   **Back**: When `low > high`, indicating the search space is exhausted.

5. **Front**: How does binary search reduce the search space in each iteration?
   **Back**: It eliminates half of the remaining search space based on the comparison with the middle element.

6. **Front**: When are you NOT able to use binary search?
   **Back**: When the collection is not sorted or when you don't have random access to elements.

7. **Front**: What are the initial values of `low` and `high` pointers?
   **Back**: `low = 0` and `high = collection.length - 1`.

8. **Front**: What should binary search return if the target is not present?
   **Back**: -1 or a special value indicating the target was not found.

## Practical Examples

### Example 1: Finding an Element in a Sorted Array

```
// Given a sorted array [3, 4, 6, 7, 9, 12, 16, 17]
// Find the index of element 6

function binarySearch(array, target):
    low = 0
    high = array.length - 1
    
    while low <= high:
        mid = low + (high - low) / 2
        
        if array[mid] == target:
            return mid
        else if target > array[mid]:
            low = mid + 1
        else:
            high = mid - 1
    
    return -1

// For target = 6:
// Initial: low = 0, high = 7, mid = 3, array[mid] = 7
// Compare: 6 < 7, so high = mid - 1 = 2
// Next: low = 0, high = 2, mid = 1, array[mid] = 4
// Compare: 6 > 4, so low = mid + 1 = 2
// Next: low = 2, high = 2, mid = 2, array[mid] = 6
// Compare: 6 == 6, so return mid = 2
```

### Example 2: Searching for a Non-existent Element

```
// Given a sorted array [3, 4, 6, 7, 9, 12, 16, 17]
// Find the index of element 13

// For target = 13:
// Initial: low = 0, high = 7, mid = 3, array[mid] = 7
// Compare: 13 > 7, so low = mid + 1 = 4
// Next: low = 4, high = 7, mid = 5, array[mid] = 12
// Compare: 13 > 12, so low = mid + 1 = 6
// Next: low = 6, high = 7, mid = 6, array[mid] = 16
// Compare: 13 < 16, so high = mid - 1 = 5
// Next: low = 6, high = 5 (low > high)
// Search space exhausted, return -1
```

## Interview Preparation Tips

### Potential Interview Questions:

1. **Implementation**: "Implement binary search algorithm (iterative and recursive)"
2. **Analysis**: "What is the time and space complexity of binary search?"
3. **Variations**: "How would you modify binary search to find the first or last occurrence of an element in a sorted array with duplicates?"
4. **Edge Cases**: "How would you handle integer overflow in binary search?"
5. **Application**: "Given a sorted array that has been rotated k times, how would you find an element in it?"
6. **Understanding**: "Why is binary search better than linear search for sorted arrays?"
7. **Problem Solving**: "How would you use binary search to find the square root of a number?"

### Key Concepts Interviewers Test:

- **Understanding of time complexity**: Recognizing when and why binary search is O(log n)
- **Edge case handling**: Empty arrays, single element arrays, target not present
- **Correctness**: Getting the termination conditions right (`low <= high`)
- **Integer overflow**: Using the correct technique to calculate mid
- **Problem recognition**: Identifying when binary search can be applied beyond simple array searching

## Common Mistakes or Misunderstandings

1. **Applying binary search to unsorted arrays**: The array must be sorted for binary search to work correctly.
2. **Incorrect loop condition**: Using `low < high` instead of `low <= high` can miss the last element.
3. **Incorrect mid calculation**: Using `(low + high) / 2` can cause integer overflow.
4. **Forgetting to update the search space**: Not updating `low` or `high` creates an infinite loop.
5. **Incorrect boundary updates**: Using `low = mid` or `high = mid` instead of `low = mid + 1` or `high = mid - 1` can lead to infinite loops.
6. **Assuming binary search only works for arrays**: Binary search can be applied to any collection that allows random access and is sorted.
7. **Not handling duplicates properly**: Standard binary search only finds an occurrence, not necessarily the first or last occurrence.

## Advanced Notes / Behind the Scenes

### Binary Search Variations

1. **Finding the first occurrence**:
   - When the target is found, don't return immediately
   - Continue searching in the left half by setting `high = mid - 1`
   - Keep track of the last position where the target was found

2. **Finding the last occurrence**:
   - When the target is found, don't return immediately
   - Continue searching in the right half by setting `low = mid + 1`
   - Keep track of the last position where the target was found

3. **Finding the closest element**:
   - Run binary search until search space is exhausted
   - Compare the elements at `low-1`, `low`, and `low+1` to find the closest

### Beyond Array Searching

Binary search can be applied to:
- Finding roots of monotonic functions
- Searching in binary search trees
- Optimization problems with monotonic properties
- Solving equations where the search space can be divided efficiently

### Optimizations

- **Early termination**: If the collection is large but the target is expected to be near the beginning or end, start with linear search and switch to binary search after a few checks.
- **Interpolation search**: An improvement over binary search for uniformly distributed arrays by estimating the position of the target based on its value.

## Footnotes / Glossary

### Glossary

- **Search Space**: The portion of the collection where the target might be located
- **Logarithmic Time**: Time complexity that grows logarithmically with input size
- **Mid Index**: The middle index of the current search space
- **Monotonic Function**: A function that is either entirely non-decreasing or entirely non-increasing
- **Integer Overflow**: When an arithmetic operation produces a result that exceeds the maximum value a variable can hold

### Platform Specific Notes

- Different programming languages handle integer overflow differently:
  - C/C++: Integer overflow is undefined behavior
  - Java: Integer overflow wraps around
  - Python: Integers have unlimited precision, so overflow is not an issue

- Language-specific implementations:
  - Java: `Arrays.binarySearch(array, target)`
  - C++: `std::binary_search()` and `std::lower_bound()`
  - Python: `bisect` module provides binary search functions
