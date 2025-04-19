# Chapter: Find the Minimum in Rotated Sorted Array

## Overview
This chapter explores an important binary search application: finding the minimum element in a rotated sorted array. This problem is a common interview question that tests understanding of binary search and sorted array properties. The chapter builds upon previous knowledge of binary search and rotated sorted arrays, offering both brute force and optimized approaches with a focus on time complexity analysis.

## Concept Map
```
                           ┌───────────────────┐
                           │ Rotated Sorted    │
                           │ Array Problem     │
                           └─────────┬─────────┘
                                     │
                 ┌───────────────────┴───────────────────┐
                 │                                       │
        ┌────────▼─────────┐                   ┌─────────▼────────┐
        │  Brute Force     │                   │  Binary Search   │
        │  O(n) approach   │                   │  O(log n)        │
        └──────────────────┘                   └─────────┬────────┘
                                                         │
                          ┌────────────────────────────┬─┴───┬─────────────────────────────┐
                          │                            │     │                             │
                 ┌────────▼───────┐        ┌───────────▼───┐ │                  ┌─────────▼─────────┐
                 │ Find which half│        │ Find minimum  │ │                  │ Optimization:     │
                 │ is sorted      │        │ in sorted half│ │                  │ Detect fully      │
                 └────────────────┘        └───────────────┘ │                  │ sorted subarray   │
                                                             │                  └───────────────────┘
                                                    ┌────────▼─────────┐
                                                    │ Eliminate half   │
                                                    │ & continue search │
                                                    └──────────────────┘
```

## Detailed Sections

### 1. Problem Definition
- **Input**: An array that was originally sorted but has been rotated around a pivot point
  - Example: `[4,5,6,7,0,1,2]` was originally `[0,1,2,4,5,6,7]` rotated at index 4
- **Output**: The minimum value in the array
  - In the example above, the answer would be `0`
- **Constraint**: Must achieve better than O(n) time complexity

### 2. Understanding Rotated Sorted Arrays
- **Definition**: A sorted array that has been rotated around some pivot point
  - Before rotation: `[0,1,2,4,5,6,7]`
  - After rotation: `[4,5,6,7,0,1,2]`
- **Properties**:
  - After rotation, the array consists of two sorted subarrays
  - The minimum element is the first element of the second subarray
  - The point of rotation is where the minimum element is located

### 3. Approaches to Solve the Problem

#### 3.1 Brute Force Approach
- Simply traverse the entire array once and find the minimum value
- Time Complexity: O(n)
- Space Complexity: O(1)
- Not optimal for interviews where optimized solutions are expected

#### 3.2 Binary Search Approach
- We can leverage the fact that parts of the array are sorted
- The key insight: we need to determine which half to eliminate in each step
- Time Complexity: O(log n)
- Space Complexity: O(1)

### 4. Binary Search Implementation Strategy

#### 4.1 Key Insights
- In a rotated sorted array, at least one half will always be properly sorted
- We can identify which half is sorted by comparing `array[mid]` with the endpoints
- The minimum element will be either:
  - The first element of the sorted half (if the rotation point isn't in that half)
  - Or somewhere in the unsorted half

#### 4.2 Algorithm Steps
1. Initialize `low = 0`, `high = n-1`, and `answer = INT_MAX`
2. While `low <= high`:
   - Find `mid = (low + high) / 2`
   - Check if the left half is sorted (`array[low] <= array[mid]`)
     - If yes, the minimum in this half is `array[low]`
     - Update `answer = min(answer, array[low])`
     - Eliminate left half by setting `low = mid + 1`
   - Otherwise, the right half must be sorted
     - The minimum in this half is `array[mid]`
     - Update `answer = min(answer, array[mid])`
     - Eliminate right half by setting `high = mid - 1`
3. Return `answer`

#### 4.3 Optimization
- If the entire search space is already sorted (`array[low] <= array[high]`), then:
  - The minimum element is `array[low]`
  - We can update `answer` and break out of the loop
- This optimization handles the case when we've eliminated enough elements that the remaining subarray is fully sorted

### 5. Code Implementation

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        int low = 0;
        int high = nums.size() - 1;
        int ans = INT_MAX;
        
        while (low <= high) {
            // If array is already sorted
            if (nums[low] <= nums[high]) {
                ans = min(ans, nums[low]);
                break;
            }
            
            int mid = low + (high - low) / 2;
            
            // Check if left half is sorted
            if (nums[low] <= nums[mid]) {
                // Minimum in left half would be at low
                ans = min(ans, nums[low]);
                // Eliminate left half
                low = mid + 1;
            } else {
                // Right half must be sorted, minimum could be at mid
                ans = min(ans, nums[mid]);
                // Eliminate right half
                high = mid - 1;
            }
        }
        
        return ans;
    }
};
```

## Flash cards for Quick Spaced Repetition Revision

1. **Front**: What is a rotated sorted array?  
   **Back**: A sorted array that has been rotated around a pivot point. Example: `[1,2,3,4,5]` becomes `[3,4,5,1,2]`.

2. **Front**: What is the time complexity of finding the minimum in a rotated sorted array using binary search?  
   **Back**: O(log n)

3. **Front**: How do you identify which half of a rotated sorted array is sorted?  
   **Back**: By comparing array[low] and array[mid]. If array[low] <= array[mid], then the left half is sorted.

4. **Front**: Where is the minimum element located in a rotated sorted array?  
   **Back**: At the pivot point of rotation, which is the first element of the second sorted subarray.

5. **Front**: In the binary search approach for finding the minimum, after identifying the sorted half, what do we do?  
   **Back**: We store the minimum of that sorted half (which is the first element) and eliminate that half from our search.

6. **Front**: What optimization can we apply to the binary search algorithm for this problem?  
   **Back**: If the current search space is already sorted (array[low] <= array[high]), we can immediately return array[low] as the minimum for that segment.

7. **Front**: Why does binary search work for finding the minimum in a rotated sorted array?  
   **Back**: Because in each step we can identify which half is sorted, find its minimum, and eliminate one half of the array.

8. **Front**: How do we handle duplicate elements in a rotated sorted array?  
   **Back**: The base algorithm assumes unique elements. For duplicates, additional handling is required, typically resulting in O(n) worst-case time complexity.

## Practical Examples

### Example 1: Basic Rotated Array
```cpp
vector<int> nums = {4, 5, 6, 7, 0, 1, 2};
// Expected output: 0

// Trace:
// Initial: low=0, high=6, ans=INT_MAX
// Iteration 1: mid=3, nums[mid]=7
//   left half (4,5,6,7) is sorted, min=4
//   ans=4, low=4
// Iteration 2: mid=5, nums[mid]=1
//   left half (0,1) is sorted, min=0
//   ans=0, low=6
// Iteration 3: mid=6, nums[mid]=2
//   left half (2) is sorted, min=2
//   ans=0, low=7
// low > high, loop ends
// Return ans=0
```

### Example 2: Almost Sorted Array
```cpp
vector<int> nums = {3, 4, 5, 1, 2};
// Expected output: 1

// Trace:
// Initial: low=0, high=4, ans=INT_MAX
// Iteration 1: mid=2, nums[mid]=5
//   left half (3,4,5) is sorted, min=3
//   ans=3, low=3
// Iteration 2: mid=3, nums[mid]=1
//   left half (1) is not sorted, right half must be sorted
//   min=1, ans=1, high=2
// Iteration 3: array is sorted (empty), loop ends
// Return ans=1
```

### Example 3: Fully Sorted Array
```cpp
vector<int> nums = {1, 2, 3, 4, 5};
// Expected output: 1

// Trace:
// Initial: low=0, high=4, ans=INT_MAX
// Iteration 1: mid=2, nums[mid]=3
//   array is sorted (nums[low] <= nums[high])
//   ans=1, break
// Return ans=1
```

### Example 4: One Element Array
```cpp
vector<int> nums = {1};
// Expected output: 1

// Trace:
// Initial: low=0, high=0, ans=INT_MAX
// Iteration 1: mid=0, nums[mid]=1
//   array is sorted (nums[low] <= nums[high])
//   ans=1, break
// Return ans=1
```

## Interview Preparation Tips

1. **Question**: "Find the minimum element in a rotated sorted array."
   - **Key points**: Use binary search to achieve O(log n), identify sorted halves, track minimum value.

2. **Question**: "How would you handle duplicate elements in a rotated sorted array?"
   - **Key points**: Duplicates can break the binary search approach in worst case. May need linear time algorithm.

3. **Question**: "Can you optimize your solution if you know the array is fully sorted?"
   - **Key points**: Check if array[low] <= array[high], in which case array[low] is the minimum.

4. **Question**: "What's the time complexity of your approach and can it be improved?"
   - **Key points**: Binary search is O(log n), which is optimal for this problem.

5. **Question**: "How would you find the rotation point (pivot) in a rotated sorted array?"
   - **Key points**: The pivot is where the minimum element is located. Same approach can be used.

6. **Question**: "Compare and contrast finding minimum in a rotated sorted array versus searching for a specific element."
   - **Key points**: Both use binary search but with different decision criteria. Searching for a specific element requires additional checks.

7. **Question**: "Why do we update answer with array[low] for the sorted half and array[mid] for the unsorted half?"
   - **Key points**: In the sorted half, the minimum is always at low. In the unsorted half, mid could be the minimum.

## Common Mistakes or Misunderstandings

1. **Using standard binary search directly**
   - Standard binary search doesn't work because the array isn't fully sorted.
   - You need to adapt the approach to handle the rotation.

2. **Incorrect half elimination logic**
   - Eliminating the wrong half will miss the minimum element.
   - Always ensure you're keeping the half that might contain the minimum.

3. **Not handling edge cases**
   - Single element arrays
   - Already sorted arrays (no rotation)
   - Arrays where the rotation happens at the beginning or end

4. **Overlooking the optimization**
   - Not checking if the current subarray is already sorted
   - This optimization can significantly improve average-case performance

5. **Using low < high instead of low <= high**
   - This could miss checking the last element
   - The instructor emphasizes consistent use of low <= high across binary search implementations

6. **Applying incorrect logic for duplicates**
   - The basic algorithm assumes unique elements
   - With duplicates, you might need to handle cases like [3,3,1,3]

## Advanced Notes / Behind the Scenes

1. **Binary Search Template Consistency**
   - The instructor emphasizes using a consistent template for binary search problems:
     - `low <= high` as the loop condition
     - `low = mid + 1` and `high = mid - 1` for updating pointers
     - This consistency helps avoid off-by-one errors and builds stronger mental models

2. **Time Complexity Analysis**
   - For arrays with unique elements: O(log n)
   - For arrays with duplicates: worst case can degrade to O(n)
     - Example: [1,1,1,1,1,1,0,1,1,1,1]

3. **Space Complexity Analysis**
   - O(1) as we're only using a few variables regardless of input size

4. **Handling Duplicates (Advanced)**
   - When nums[low] == nums[mid] == nums[high], you cannot determine which half is sorted
   - A common approach is to increment low and decrement high
   - This can lead to O(n) worst-case time complexity

5. **Connection to Other Problems**
   - Finding the minimum is closely related to:
     - Finding the rotation point
     - Searching in a rotated sorted array
     - Determining if an array is rotated

## Glossary

- **Rotated Sorted Array**: A sorted array that has been rotated around some pivot point.
- **Pivot Point**: The index where rotation occurs in a rotated sorted array.
- **Binary Search**: A search algorithm that finds the position of a target value by dividing the search space in half in each step.
- **Time Complexity**: A measure of the amount of time an algorithm takes to run as a function of the length of the input.
- **Space Complexity**: A measure of the amount of memory an algorithm uses as a function of the length of the input.
- **Brute Force**: A straightforward approach that tries all possibilities without any optimization.
