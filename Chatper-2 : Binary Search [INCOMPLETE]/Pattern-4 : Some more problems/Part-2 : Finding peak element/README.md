# Binary Search: Finding the Peak Element

## Overview
This chapter covers the "Find Peak Element" problem, a classic application of binary search. The problem involves finding an element in an array that is greater than its adjacent elements. This concept is important because it demonstrates how binary search can be applied to problems beyond simple sorted array lookups, particularly when we have a pattern of increasing and decreasing values. The solution showcases the power of binary search in reducing time complexity from O(n) to O(log n).

## Concept Map: Binary Search for Peak Element

```
                             Peak Element Problem
                                     |
                  +------------------+------------------+
                  |                                     |
            Definition                           Solution Approaches
                  |                                     |
  +---------------+---------------+           +---------+---------+
  |               |               |           |                   |
Greater than      Greater than    Edge cases  Linear Search       Binary Search
left neighbor     right neighbor     |        O(n)                O(log n)
                                     |           |                   |
                          +---------+---------+  |      +-----------+-----------+
                          |                   |  |      |                       |
                 First element      Last element |   Single Peak          Multiple Peaks
                 (compare only     (compare only |      |                       |
                  with right)       with left)   |      |                       |
                                                 |      |                       |
                                        +--------+------+                       |
                                        |               |                       |
                             Check edge cases    Main algorithm            Check mid point
                             before BS           (Binary Search)         +------+-------+
                                                        |                |              |
                                                        |          Increasing?     Decreasing?
                                                        |                |              |
                                                        |          Search right     Search left
                                                        |                |              |
                                                        +----------------+--------------+
                                                                         |
                                                                 Eventually converges
                                                                   to a peak element
```

## Detailed Sections

### 1. Problem Definition

#### What is a Peak Element?
- A peak element in an array is an element that is **greater than** (not greater than or equal to) both its left and right neighbors
- For edge cases:
  - The first element is a peak if it's greater than the element to its right
  - The last element is a peak if it's greater than the element to its left
  - The problem assumes there's a "-∞" (minus infinity) on both sides of the array
- The problem requires finding any one peak element (not necessarily all peaks)

#### Example Cases
- **Example 1:** [1, 2, 3, 4, 5, 6, 7, 8, 5]
  - Peak element is 8 (at index 7)
  - 8 > 7 (left neighbor) and 8 > 5 (right neighbor)

- **Example 2:** [1, 2, 1, 3, 5, 6, 4]
  - Two peak elements: 2 (at index 1) and 6 (at index 5)
  - 2 > 1 (left) and 2 > 1 (right)
  - 6 > 5 (left) and 6 > 4 (right)
  - You can return either one of them

- **Example 3:** [1, 2, 3, 4, 5] (Increasing sequence)
  - Peak element is 5 (at index 4)
  - 5 > 4 (left) and 5 > -∞ (conceptual right)

- **Example 4:** [5, 4, 3, 2, 1] (Decreasing sequence)
  - Peak element is 5 (at index 0)
  - 5 > -∞ (conceptual left) and 5 > 4 (right)

### 2. Linear Search Approach

#### Algorithm
- Scan the array linearly from left to right
- For each element, check if it's greater than both its neighbors
- Handle edge cases separately (first and last elements)
- Return the index of the first peak element found

#### Implementation
```cpp
int findPeakElement(vector<int>& nums) {
    int n = nums.size();
    
    // Edge case: single element array
    if (n == 1) return 0;
    
    // Edge case: first element
    if (nums[0] > nums[1]) return 0;
    
    // Edge case: last element
    if (nums[n-1] > nums[n-2]) return n-1;
    
    // Check remaining elements
    for (int i = 1; i < n-1; i++) {
        if (nums[i] > nums[i-1] && nums[i] > nums[i+1]) {
            return i;
        }
    }
    
    return -1;  // This line should never execute if the problem guarantees a peak
}
```

#### Time and Space Complexity
- Time Complexity: O(n) - we might need to check every element
- Space Complexity: O(1) - we only use a constant amount of extra space

### 3. Binary Search Approach

#### Key Insight
- Even though the array isn't sorted, binary search can be applied by using the "slope" of the array
- If the mid element is in an increasing slope (mid > mid-1), a peak must exist on the right side
- If the mid element is in a decreasing slope (mid > mid+1), a peak must exist on the left side
- If both conditions are true, the mid element itself is a peak

#### Algorithm for Single Peak Case
1. Handle edge cases (size 1 array, first element, last element)
2. Initialize low = 1 and high = n-2 (skipping the already checked edges)
3. While low ≤ high:
   - Calculate mid = low + (high - low)/2
   - If mid element is a peak, return mid
   - If mid element is in increasing slope, search right half
   - If mid element is in decreasing slope, search left half

#### Multiple Peaks Case
- Same approach works for multiple peaks
- When eliminating half the array, we are guaranteed that the remaining half contains at least one peak
- Additional case: if the mid element is neither on an increasing nor decreasing slope, we can choose either side

#### Implementation
```cpp
int findPeakElement(vector<int>& nums) {
    int n = nums.size();
    
    // Edge case: single element array
    if (n == 1) return 0;
    
    // Edge case: first element
    if (nums[0] > nums[1]) return 0;
    
    // Edge case: last element
    if (nums[n-1] > nums[n-2]) return n-1;
    
    // Binary search for peak
    int low = 1;
    int high = n-2;
    
    while (low <= high) {
        int mid = low + (high - low)/2;
        
        // Check if mid is a peak
        if (nums[mid] > nums[mid-1] && nums[mid] > nums[mid+1]) {
            return mid;
        }
        
        // If on increasing slope, peak is on right
        if (nums[mid] > nums[mid-1]) {
            low = mid + 1;
        }
        // Otherwise, peak is on left or we're at a "valley"
        else {
            high = mid - 1;
        }
    }
    
    return -1;  // This line should never execute if the problem guarantees a peak
}
```

#### Optimized Implementation
```cpp
int findPeakElement(vector<int>& nums) {
    int n = nums.size();
    
    // Edge case: single element array
    if (n == 1) return 0;
    
    // Edge case: first element
    if (nums[0] > nums[1]) return 0;
    
    // Edge case: last element
    if (nums[n-1] > nums[n-2]) return n-1;
    
    // Binary search for peak
    int low = 1;
    int high = n-2;
    
    while (low <= high) {
        int mid = low + (high - low)/2;
        
        // Check if mid is a peak
        if (nums[mid] > nums[mid-1] && nums[mid] > nums[mid+1]) {
            return mid;
        }
        
        // If on increasing slope, peak is on right
        if (nums[mid] > nums[mid-1]) {
            low = mid + 1;
        }
        // If on decreasing slope, peak is on left
        else if (nums[mid] > nums[mid+1]) {
            high = mid - 1;
        }
        // If in a "valley", can go either way
        else {
            // Choose left arbitrarily
            high = mid - 1;
        }
    }
    
    return -1;  // This line should never execute if the problem guarantees a peak
}
```

#### Time and Space Complexity
- Time Complexity: O(log n) - we eliminate half the search space in each iteration
- Space Complexity: O(1) - we only use a constant amount of extra space

## Flash Cards for Quick Spaced Repetition Revision

1. **Front:** What is a peak element in an array?  
   **Back:** An element that is greater than both its left and right neighbors. Special cases: first element only needs to be greater than right neighbor; last element only needs to be greater than left neighbor.

2. **Front:** What is the time complexity of finding a peak element using binary search?  
   **Back:** O(log n)

3. **Front:** If the mid element in binary search is greater than its left neighbor, what does this tell us?  
   **Back:** We are on an increasing curve/slope, so a peak must exist on the right side.

4. **Front:** Can an array have multiple peak elements?  
   **Back:** Yes, an array can have multiple peaks. The problem usually asks to find any one of them.

5. **Front:** What is the key insight that allows binary search to work for the peak element problem?  
   **Back:** By examining the "slope" at the mid element, we can determine which half of the array must contain a peak, allowing us to eliminate half the search space each time.

6. **Front:** What is the first step you should take when solving the peak element problem?  
   **Back:** Handle the edge cases: single element array, first element, and last element.

7. **Front:** If the mid element is in a "valley" (smaller than both neighbors), what should you do?  
   **Back:** You can choose either side (left or right) as both sides will eventually lead to a peak.

8. **Front:** Why do we set low=1 and high=n-2 in the binary search implementation?  
   **Back:** Because we've already checked the edge cases (index 0 and index n-1) and can eliminate them from the search space.

## Practical Examples

### Example 1: Finding the Peak in a Monotonically Increasing Array
```cpp
vector<int> nums = {1, 2, 3, 4, 5};
int peakIndex = findPeakElement(nums);
// Expected output: peakIndex = 4 (value 5)
```

### Example 2: Finding the Peak in a Monotonically Decreasing Array
```cpp
vector<int> nums = {5, 4, 3, 2, 1};
int peakIndex = findPeakElement(nums);
// Expected output: peakIndex = 0 (value 5)
```

### Example 3: Multiple Peaks
```cpp
vector<int> nums = {1, 3, 2, 4, 1, 5, 2};
int peakIndex = findPeakElement(nums);
// Expected output: peakIndex = 3 (value 4) or peakIndex = 5 (value 5)
```

### Example 4: Single Element Array
```cpp
vector<int> nums = {42};
int peakIndex = findPeakElement(nums);
// Expected output: peakIndex = 0 (value 42)
```

## Interview Preparation Tips

### Key Questions to Expect
1. What is the definition of a peak element? How do you handle edge cases?
2. Can you solve the peak element problem using O(n) time complexity? Now, optimize it to O(log n).
3. Why does binary search work for the peak element problem even though the array isn't sorted?
4. How would you modify your algorithm if the problem asked for all peak elements instead of just one?
5. What if the array can have equal adjacent elements? How would that affect your solution?
6. Can you explain the intuition behind why, when you're on an increasing slope, a peak must exist on the right?
7. Implement the binary search solution step by step. Walk me through the execution of your algorithm with an example.
8. What if the array is cyclical (circular)? How would you find a peak element then?

### Preparation Strategies
- Practice tracing through the binary search algorithm on paper for different examples
- Clearly understand the concept of "slope" in the array and how it guides the binary search
- Be prepared to explain the edge cases and how they're handled
- Practice explaining why binary search works for this problem even though the array isn't sorted
- Review the proof of correctness for why there must be a peak on either the left or right side

## Common Mistakes or Misunderstandings

1. **Forgetting Edge Cases**: Not handling the first or last element as possible peaks.
2. **Incorrect Slope Determination**: Misinterpreting whether we're on an increasing or decreasing slope.
3. **Using `mid = (low + high) / 2`**: This can cause integer overflow for large arrays; use `mid = low + (high - low) / 2` instead.
4. **"Valley" Handling**: Not accounting for when the mid element is smaller than both neighbors (a "valley").
5. **Peak Definition**: Confusing "greater than" with "greater than or equal to" in the peak definition.
6. **Search Space Reduction**: Not setting the low and high pointers correctly after determining which half to search.
7. **Loop Termination**: Using wrong loop condition (e.g., using `<` instead of `<=`).
8. **Return Value**: Returning -1 when the problem guarantees a peak element exists.

## Tooling & IDE Integration

### Debugging Tips
- Use visualizers to trace the binary search execution:
  - Visual Studio: Use the "Watch" window to monitor `low`, `high`, and `mid` values
  - CLion: Use inline debugging to track changes in binary search variables
  - Online platforms: Use sites like [VisuAlgo](https://visualgo.net/en/binarysearch) to visualize binary search

### Automated Testing
```cpp
// Example unit test (using Google Test)
TEST(PeakElementTest, MultipleExamples) {
    vector<int> test1 = {1, 2, 3, 1};
    EXPECT_EQ(findPeakElement(test1), 2);
    
    vector<int> test2 = {1, 2, 1, 3, 5, 6, 4};
    int result2 = findPeakElement(test2);
    EXPECT_TRUE(result2 == 1 || result2 == 5);
    
    vector<int> test3 = {1};
    EXPECT_EQ(findPeakElement(test3), 0);
}
```

## Advanced Notes / Behind the Scenes

### Mathematical Proof of Correctness
The binary search approach works because:
1. If we're at position `mid` and `nums[mid] > nums[mid-1]`, then we're on an increasing slope.
2. If we continue to move right and the array keeps increasing, the rightmost element will be a peak (since it's followed by a conceptual -∞).
3. If the array starts decreasing at some point, then the point just before it starts decreasing will be a peak.
4. Similar logic applies when `nums[mid] > nums[mid+1]` and we search the left half.

### Extension to 2D Arrays
The peak element problem can be extended to 2D arrays where a peak is an element greater than all its 4 adjacent neighbors. This can be solved using a modified binary search approach with a time complexity of O(m log n) or O(n log m), where m and n are the dimensions of the array.

### Relation to Bitonic Arrays
A bitonic array is an array that increases and then decreases. The peak element problem is related to finding the maximum in a bitonic array, which is a special case where there's only one peak.

## Glossary

- **Peak Element**: An element that is greater than both its left and right neighbors.
- **Binary Search**: A search algorithm that repeatedly divides the search interval in half.
- **Slope**: In the context of this problem, refers to whether the array is increasing or decreasing at a given position.
- **Monotonic Array**: An array that is either entirely non-increasing or entirely non-decreasing.
- **Bitonic Array**: An array that is first increasing and then decreasing.
- **O(log n)**: Logarithmic time complexity, characteristic of divide-and-conquer algorithms like binary search.
- **Edge Case**: Special cases that need to be handled separately, like first or last elements or arrays with a single element.
- **Valley**: An element that is smaller than both its adjacent neighbors.
