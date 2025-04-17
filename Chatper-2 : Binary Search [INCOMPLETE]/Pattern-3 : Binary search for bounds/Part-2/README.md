# Binary Search Applications: Finding First & Last Occurrences in Sorted Arrays

## Overview
This chapter explores specialized applications of binary search in sorted arrays, specifically addressing two common problems:
1. Finding the first and last occurrence of a given element
2. Counting the total occurrences of an element in a sorted array

These techniques are essential for efficient searching in arrays with duplicate elements and are frequently asked during technical interviews. The methods presented utilize both standard binary search implementations and specialized functions like `lower_bound` and `upper_bound`.

## Concept Map

```
                       ┌───────────────────────┐
                       │    Binary Search      │
                       │  in Sorted Arrays     │
                       └─────────┬─────────────┘
                               ▼
           ┌─────────────────────────────────────────┐
           │                                         │
┌──────────▼──────────┐                  ┌───────────▼──────────────┐
│  Finding First and  │                  │ Counting Occurrences     │
│  Last Occurrences   │                  │ of Element X             │
└──────────┬──────────┘                  └───────────┬──────────────┘
           │                                         │
           ▼                                         │
┌─────────────────────┐                              │
│    Approaches:      │                              │
└─────────┬───────────┘                              │
          │                                          │
    ┌─────▼─────┬────────────────┐                   │
    │           │                │                   │
┌───▼───┐  ┌────▼────┐   ┌───────▼────────┐   ┌──────▼───────┐
│Linear │  │Standard │   │lower_bound &   │   │Last - First  │
│Search │  │Binary   │   │upper_bound     │──►│+ 1           │
│O(n)   │  │Search   │   │Functions       │   │              │
└───────┘  │O(log n) │   │O(log n)        │   └──────────────┘
           └─────────┘   └────────────────┘
```

## Detailed Sections

### 1. Problem Definition: Finding First and Last Occurrences

#### Problem Statement:
- Given a sorted array and an element X, find:
  - The index of the first occurrence of X
  - The index of the last occurrence of X
- Return -1 for both values if X is not present in the array

#### Example:
```
Array: [1, 2, 3, 8, 8, 8, 11]
X = 8
First occurrence: index 3
Last occurrence: index 5

X = 10
First occurrence: -1 (not found)
Last occurrence: -1 (not found)

X = 11
First occurrence: index 6
Last occurrence: index 6 (only one occurrence)
```

### 2. Approach 1: Linear Search (Naive Solution)

This is the brute force approach with O(n) time complexity.

#### Algorithm:
1. Initialize `first = -1` and `last = -1`
2. Traverse the array from left to right
3. When element X is found:
   - If `first` is still -1, update it to current index
   - Always update `last` to current index

#### Pseudocode:
```
function findFirstAndLastOccurrence(array, n, x):
    first = -1
    last = -1
    
    for i from 0 to n-1:
        if array[i] == x:
            if first == -1:
                first = i
            last = i
    
    return [first, last]
```

#### Time Complexity: O(n)
#### Space Complexity: O(1)

### 3. Approach 2: Using Lower and Upper Bound

This approach leverages binary search through the existing functions `lower_bound` and `upper_bound`.

#### Definitions:
- `lower_bound(x)`: Returns index of first element that is greater than or equal to x
- `upper_bound(x)`: Returns index of first element that is greater than x

#### Algorithm:
1. Find `lower_bound` of X
2. If `lower_bound` points to end of array or element at `lower_bound` is not X, return [-1, -1]
3. Otherwise, `first_occurrence = lower_bound`
4. Find `upper_bound` of X
5. `last_occurrence = upper_bound - 1`

#### Pseudocode:
```
function findFirstAndLastOccurrence(array, n, x):
    // Find lower_bound
    lb = lower_bound(array, n, x)
    
    // Check if element exists
    if lb == n or array[lb] != x:
        return [-1, -1]
    
    // Find upper_bound and subtract 1 to get last occurrence
    ub = upper_bound(array, n, x)
    
    return [lb, ub-1]
```

#### Implementations of lower_bound and upper_bound:

```
function lower_bound(array, n, x):
    low = 0
    high = n-1
    ans = n
    
    while (low <= high):
        mid = low + (high - low) / 2
        
        if array[mid] >= x:
            ans = mid
            high = mid - 1
        else:
            low = mid + 1
    
    return ans

function upper_bound(array, n, x):
    low = 0
    high = n-1
    ans = n
    
    while (low <= high):
        mid = low + (high - low) / 2
        
        if array[mid] > x:
            ans = mid
            high = mid - 1
        else:
            low = mid + 1
    
    return ans
```

#### Time Complexity: O(log n)
#### Space Complexity: O(1)

### 4. Approach 3: Custom Binary Search Implementation

This approach implements two separate binary search algorithms - one to find the first occurrence and another to find the last occurrence.

#### Algorithm for Finding First Occurrence:
1. Initialize `first = -1`, `low = 0`, `high = n-1`
2. Perform binary search:
   - If `array[mid] == x`, update `first = mid` and search in left half
   - If `array[mid] < x`, search in right half
   - If `array[mid] > x`, search in left half

#### Algorithm for Finding Last Occurrence:
1. Initialize `last = -1`, `low = 0`, `high = n-1`
2. Perform binary search:
   - If `array[mid] == x`, update `last = mid` and search in right half
   - If `array[mid] < x`, search in right half
   - If `array[mid] > x`, search in left half

#### Pseudocode for Finding First Occurrence:
```
function findFirstOccurrence(array, n, x):
    low = 0
    high = n-1
    first = -1
    
    while (low <= high):
        mid = low + (high - low) / 2
        
        if array[mid] == x:
            first = mid
            high = mid - 1
        else if array[mid] < x:
            low = mid + 1
        else:
            high = mid - 1
    
    return first
```

#### Pseudocode for Finding Last Occurrence:
```
function findLastOccurrence(array, n, x):
    low = 0
    high = n-1
    last = -1
    
    while (low <= high):
        mid = low + (high - low) / 2
        
        if array[mid] == x:
            last = mid
            low = mid + 1
        else if array[mid] < x:
            low = mid + 1
        else:
            high = mid - 1
    
    return last
```

#### Combined Function:
```
function findFirstAndLastOccurrence(array, n, x):
    first = findFirstOccurrence(array, n, x)
    
    // Optimization: If first occurrence not found, don't search for last
    if first == -1:
        return [-1, -1]
    
    last = findLastOccurrence(array, n, x)
    return [first, last]
```

#### Time Complexity: O(log n)
#### Space Complexity: O(1)

### 5. Problem: Counting Occurrences in Sorted Array

#### Problem Statement:
- Given a sorted array and an element X, find the total number of occurrences of X in the array

#### Solution:
Using the first and last occurrence approach, the number of occurrences can be calculated as:
```
occurrences = last_occurrence - first_occurrence + 1
```

#### Algorithm:
1. Find first and last occurrence using one of the approaches above
2. If element not found (first == -1), return 0
3. Otherwise, return (last - first + 1)

#### Pseudocode:
```
function countOccurrences(array, n, x):
    result = findFirstAndLastOccurrence(array, n, x)
    
    if result[0] == -1:
        return 0
    
    return result[1] - result[0] + 1
```

#### Time Complexity: O(log n)
#### Space Complexity: O(1)

## Flash Cards for Quick Revision

1. **Front**: What is the time complexity of finding first and last occurrences in a sorted array?
   **Back**: O(log n) using binary search, O(n) using linear search

2. **Front**: How can you find the total number of occurrences of an element in a sorted array?
   **Back**: lastOccurrence - firstOccurrence + 1

3. **Front**: What is the key difference between finding first occurrence vs last occurrence using binary search?
   **Back**: For first occurrence, after finding an element, search in left half; for last occurrence, search in right half

4. **Front**: What is the lower_bound function?
   **Back**: Returns index of first element greater than or equal to x

5. **Front**: What is the upper_bound function?
   **Back**: Returns index of first element greater than x

6. **Front**: How do you handle when an element is not present in the array?
   **Back**: Return [-1, -1] for first and last occurrence

7. **Front**: How do you optimize finding first and last occurrences?
   **Back**: If first occurrence is -1, don't search for last occurrence

8. **Front**: What's the difference between searching for elements in sorted vs unsorted arrays?
   **Back**: Sorted arrays allow binary search (O(log n)), unsorted arrays require linear search (O(n))

## Practical Examples

### Example 1: Finding First and Last Occurrence using Binary Search

```cpp
// Function to find the first occurrence
int findFirstOccurrence(vector<int>& nums, int target) {
    int low = 0, high = nums.size() - 1;
    int first = -1;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        if (nums[mid] == target) {
            first = mid;
            high = mid - 1;  // Search in left half
        } else if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    
    return first;
}

// Function to find the last occurrence
int findLastOccurrence(vector<int>& nums, int target) {
    int low = 0, high = nums.size() - 1;
    int last = -1;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        if (nums[mid] == target) {
            last = mid;
            low = mid + 1;  // Search in right half
        } else if (nums[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    
    return last;
}

// Combined function
vector<int> searchRange(vector<int>& nums, int target) {
    int first = findFirstOccurrence(nums, target);
    
    // Optimization
    if (first == -1) {
        return {-1, -1};
    }
    
    int last = findLastOccurrence(nums, target);
    return {first, last};
}
```

### Example 2: Using lower_bound and upper_bound

```cpp
// Using C++ STL functions
vector<int> searchRange(vector<int>& nums, int target) {
    if (nums.empty()) return {-1, -1};
    
    // Find lower_bound
    auto lb = lower_bound(nums.begin(), nums.end(), target);
    
    // Check if element exists
    if (lb == nums.end() || *lb != target) {
        return {-1, -1};
    }
    
    // Find upper_bound
    auto ub = upper_bound(nums.begin(), nums.end(), target);
    
    // Convert to indices
    int first = lb - nums.begin();
    int last = ub - nums.begin() - 1;
    
    return {first, last};
}

// Custom implementation
vector<int> searchRange(vector<int>& nums, int target) {
    int n = nums.size();
    
    // Find lower_bound
    int low = 0, high = n-1, lb = n;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (nums[mid] >= target) {
            lb = mid;
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }
    
    // Check if element exists
    if (lb == n || nums[lb] != target) {
        return {-1, -1};
    }
    
    // Find upper_bound
    low = 0, high = n-1;
    int ub = n;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (nums[mid] > target) {
            ub = mid;
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }
    
    return {lb, ub-1};
}
```

### Example 3: Counting Occurrences

```cpp
int countOccurrences(vector<int>& nums, int target) {
    vector<int> range = searchRange(nums, target);
    
    if (range[0] == -1) {
        return 0;
    }
    
    return range[1] - range[0] + 1;
}
```

## Interview Preparation Tips

1. **Know the variations**: Be familiar with all three approaches - linear search, custom binary search, and using lower_bound/upper_bound functions.

2. **Edge cases**: Always consider:
   - Element not present in array
   - Element present only once
   - Element present at beginning or end of array
   - Empty array

3. **Optimization tricks**: Mention that you're checking if first occurrence exists before searching for last occurrence to save time.

4. **Time and space complexity**: Be ready to analyze:
   - Time complexity: O(log n) for binary search approaches
   - Space complexity: O(1) for all approaches

5. **Follow-up questions you might encounter**:
   - "How would you modify if array is not sorted?"
   - "What if array is circular sorted?"
   - "How would you adapt this to find first and last occurrences of a number in a given range?"
   - "Can you optimize further if you know there are many duplicates?"

6. **Implementation details**: When coding, pay attention to:
   - Integer overflow when calculating mid
   - Proper updating of search boundaries
   - Returning correct values when element not found

7. **Binary search variants**: Be prepared to explain when to use `<=` vs `<` in the while loop condition.

## Common Mistakes or Misunderstandings

1. **Forgetting to check if element exists**: When using lower_bound, always check if the returned index contains the target element.

2. **Off-by-one errors**: Be careful when converting upper_bound to last occurrence (need to subtract 1).

3. **Incorrect search space updates**: When finding first occurrence, search left after finding target; for last occurrence, search right.

4. **Inefficient implementation**: Don't perform two separate binary searches when one fails. Check if first occurrence exists before searching for last.

5. **Assuming unique elements**: Remember that the problem involves duplicates, which is why finding first and last occurrences is necessary.

6. **Using linear search**: This is inefficient - always utilize the fact that the array is sorted.

7. **Incorrect boundary conditions**: Being careless with while loop conditions (`<` vs `<=`) can cause missed elements.

## Advanced Notes / Behind the Scenes

1. **Template method for binary search**: With slight modifications, you can create a generalized binary search template that handles various scenarios like:
   - Finding first occurrence
   - Finding last occurrence
   - Finding floor/ceiling values

2. **STL implementations**: C++ STL's `lower_bound` and `upper_bound` are optimized implementations that might use additional techniques like:
   - Fast path for small ranges
   - Unrolled loops for performance
   - Platform-specific optimizations

3. **Advanced variations**:
   - Finding first and last occurrences in a rotated sorted array
   - Finding in a nearly sorted array (elements might be k positions away from sorted position)
   - Applying to 2D sorted matrices

4. **Why binary search is used**: Besides O(log n) time complexity, binary search also offers good locality of reference, which can improve cache performance compared to other logarithmic algorithms.

## Footnotes / Glossary

### Glossary:
- **Binary Search**: A search algorithm that finds the position of a target value within a sorted array by repeatedly dividing the search space in half.
- **Lower Bound**: The smallest index i where array[i] >= target.
- **Upper Bound**: The smallest index i where array[i] > target.
- **Time Complexity**: A measure of the amount of time an algorithm takes to run as a function of the length of the input.
- **Space Complexity**: A measure of the amount of memory an algorithm uses as a function of the length of the input.

### Language-Specific Notes:
- **C++**: Use `std::lower_bound` and `std::upper_bound` from the STL
- **Java**: Use `Collections.binarySearch` with custom comparators
- **Python**: Use `bisect.bisect_left` and `bisect.bisect_right`
- **JavaScript**: No built-in binary search functions, implement custom solutions