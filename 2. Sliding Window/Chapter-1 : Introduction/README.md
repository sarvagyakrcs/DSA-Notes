# Two Pointer and Sliding Window Techniques

## Overview
This chapter covers two important algorithmic patterns in data structures and algorithms: the Two Pointer technique and the Sliding Window approach. These are not formal algorithms but rather conceptual frameworks that help solve a variety of array and string problems efficiently. The chapter explains four common patterns seen with these techniques, provides templates for implementation, and demonstrates how to approach different problem types systematically. These techniques are particularly valuable for optimizing brute force solutions and are frequently tested in coding interviews.

## Concept Map

```
TWO POINTER & SLIDING WINDOW TECHNIQUES
│
├── PATTERN 1: Constant Window
│   └── Fixed size window moves through array
│       ├── Calculate initial window sum/result
│       ├── Slide window by adding new element and removing first element
│       └── Track maximum/minimum value
│
├── PATTERN 2: Longest Subarray/Substring with Condition
│   ├── Brute Force: Generate all subarrays O(n²)
│   ├── Better: Two Pointer + Sliding Window O(2n)
│   │   ├── Expand window (move right pointer)
│   │   └── Shrink window (move left pointer) when condition violated
│   └── Optimal: Modified shrinking O(n)
│       └── Only shrink once instead of complete shrink
│
├── PATTERN 3: Count Subarrays with Condition
│   └── Typically solved by breaking into subproblems:
│       ├── Count subarrays with sum <= K
│       ├── Count subarrays with sum <= (K-1)
│       └── Answer = First count - Second count
│
└── PATTERN 4: Shortest Subarray/Substring with Condition
    ├── Find valid window first
    └── Then shrink to find minimum valid window
```

## Detailed Sections

### 1. Understanding Two Pointer and Sliding Window

- These are **constructive algorithm techniques** rather than specific algorithms
- You need to understand the concept and adapt it to specific problem requirements
- Four common patterns appear in interview questions:
  - Constant window problems
  - Finding longest subarray/substring with a condition
  - Counting subarrays with a condition
  - Finding shortest subarray/substring with a condition

### 2. Pattern 1: Constant Window Problems

- **Description**: Problems where window size is fixed and specified in the problem
- **Example Problem**: Find maximum sum of K consecutive elements in an array
- **Approach**:
  1. Initialize two pointers: `L = 0` and `R = K-1` (defining window boundaries)
  2. Calculate sum of elements in initial window (indexes 0 to K-1)
  3. Slide window by moving both pointers:
     - Remove element at `L` from sum
     - Increment `L`
     - Increment `R`
     - Add element at new `R` position to sum
  4. Compare with maximum sum found so far
  5. Repeat until `R` reaches end of array

```python
# Pseudocode for constant window sum
def max_sum_k_consecutive(arr, k):
    n = len(arr)
    
    # Calculate initial window sum
    window_sum = 0
    for i in range(k):
        window_sum += arr[i]
    
    max_sum = window_sum
    
    # Slide window
    for i in range(k, n):
        window_sum = window_sum - arr[i-k] + arr[i]  # Remove leftmost, add rightmost
        max_sum = max(max_sum, window_sum)
    
    return max_sum
```

### 3. Pattern 2: Longest Subarray/Substring with Condition

#### 3.1 Brute Force Approach
- **Time Complexity**: O(n²)
- **Space Complexity**: O(1)
- **Approach**:
  1. Generate all possible subarrays
  2. Check each against the given condition
  3. Keep track of the longest valid subarray

```python
# Pseudocode for brute force approach
def longest_subarray_with_condition(arr, k):
    n = len(arr)
    max_length = 0
    
    # Generate all subarrays
    for i in range(n):
        sum = 0
        for j in range(i, n):
            sum += arr[j]
            
            # Check condition (e.g., sum <= k)
            if sum <= k:
                max_length = max(max_length, j - i + 1)
            else:
                # Optional optimization: break if condition violated
                break
                
    return max_length
```

#### 3.2 Better Approach (Two Pointer + Sliding Window)
- **Time Complexity**: O(2n) → O(n)
- **Space Complexity**: O(1)
- **Approach**:
  1. Start with window of size 1 (L=0, R=0)
  2. Two operations:
     - **Expand**: Increase window size by moving right pointer
     - **Shrink**: Decrease window size by moving left pointer
  3. Key template components:
     - Expand window when condition is met
     - Shrink window when condition is violated
     - Update maximum length when condition is met

```python
# Pseudocode for better approach
def longest_subarray_with_condition(arr, k):
    n = len(arr)
    max_length = 0
    L = 0
    sum = 0
    
    for R in range(n):
        # Expand window
        sum += arr[R]
        
        # Shrink window until condition is satisfied
        while sum > k:
            sum -= arr[L]
            L += 1
            
        # Update maximum length
        max_length = max(max_length, R - L + 1)
        
    return max_length
```

#### 3.3 Optimal Approach (Improved Shrinking)
- **Time Complexity**: O(n)
- **Space Complexity**: O(1)
- **Approach**:
  - Only shrink window once instead of completely shrinking it
  - Useful when only the length (not the actual subarray) is needed

```python
# Pseudocode for optimal approach
def longest_subarray_with_condition(arr, k):
    n = len(arr)
    max_length = 0
    L = 0
    sum = 0
    
    for R in range(n):
        # Expand window
        sum += arr[R]
        
        # If condition violated, shrink once
        if sum > k:
            sum -= arr[L]
            L += 1
            
        # Update maximum length
        max_length = max(max_length, R - L + 1)
        
    return max_length
```

- **Important note**: This optimization only works for length calculation. If the actual subarray is needed, use the better approach with complete shrinking.

### 4. Pattern 3: Count Subarrays with Condition

- **Approach**: Break into subproblems when dealing with exact conditions
- For problems like "count subarrays with sum = K":
  1. Find count of subarrays with sum ≤ K
  2. Find count of subarrays with sum ≤ (K-1)
  3. Answer = First count - Second count
- Uses Pattern 2 technique as the underlying mechanism
- Especially useful when condition is an exact value rather than a range

### 5. Pattern 4: Shortest Subarray/Substring with Condition

- **Approach**:
  1. Find first valid window that satisfies the condition
  2. Try to shrink from left while maintaining validity
  3. Update minimum length when a valid smaller window is found
  4. Continue expanding from right
- Similar to Pattern 2 but focus is on minimum rather than maximum length

```python
# Pseudocode for shortest subarray with condition
def shortest_subarray_with_condition(arr, condition):
    n = len(arr)
    min_length = float('inf')
    L = 0
    
    for R in range(n):
        # Expand window
        # Update window state
        
        # While condition is satisfied, try to shrink
        while condition_is_met():
            # Update minimum length
            min_length = min(min_length, R - L + 1)
            
            # Try to shrink further
            # Update window state
            L += 1
            
    return min_length if min_length != float('inf') else 0
```

## Flash Cards for Quick Revision

1. **Q**: What is the time complexity of sliding window approach for Pattern 2?  
   **A**: O(n) because each element is processed at most twice (once during expansion, once during shrinking)

2. **Q**: When do you expand the window in sliding window technique?  
   **A**: When moving the right pointer forward to include more elements in the window

3. **Q**: When do you shrink the window in sliding window technique?  
   **A**: When the window violates the required condition and needs to exclude elements from the left

4. **Q**: What's the difference between Pattern 2 and Pattern 4?  
   **A**: Pattern 2 finds the longest subarray meeting a condition, Pattern 4 finds the shortest

5. **Q**: How is Pattern 3 (count subarrays) typically solved?  
   **A**: By breaking into two subproblems: count subarrays with sum ≤ K and count subarrays with sum ≤ (K-1), then subtracting

6. **Q**: What is a subarray?  
   **A**: Any consecutive portion of an array; single elements and the entire array are also subarrays

7. **Q**: When should you use the optimization in Pattern 2?  
   **A**: When only the length is needed, not the actual subarray itself

8. **Q**: Why do we avoid completely shrinking the window in the optimal approach?  
   **A**: To avoid redundant work - we only need to shrink enough to make the window valid again

## Practical Examples

### Example 1: Maximum Sum of K Consecutive Elements (Pattern 1)

```java
public int maxSumKConsecutive(int[] arr, int k) {
    int n = arr.length;
    
    // Calculate sum of first window
    int windowSum = 0;
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    
    int maxSum = windowSum;
    
    // Slide window
    for (int i = k; i < n; i++) {
        windowSum = windowSum - arr[i - k] + arr[i];
        maxSum = Math.max(maxSum, windowSum);
    }
    
    return maxSum;
}
```

### Example 2: Longest Subarray with Sum ≤ K (Pattern 2)

```java
public int longestSubarrayWithSumLessThanK(int[] arr, int k) {
    int n = arr.length;
    int L = 0;
    int sum = 0;
    int maxLength = 0;
    
    for (int R = 0; R < n; R++) {
        // Expand window
        sum += arr[R];
        
        // Shrink window if condition violated
        while (sum > k && L <= R) {
            sum -= arr[L];
            L++;
        }
        
        // Update max length
        maxLength = Math.max(maxLength, R - L + 1);
    }
    
    return maxLength;
}
```

### Example 3: Optimized Version for Length Only (Pattern 2)

```java
public int longestSubarrayWithSumLessThanK(int[] arr, int k) {
    int n = arr.length;
    int L = 0;
    int sum = 0;
    int maxLength = 0;
    
    for (int R = 0; R < n; R++) {
        // Expand window
        sum += arr[R];
        
        // Shrink window once if condition violated
        if (sum > k && L <= R) {
            sum -= arr[L];
            L++;
        }
        
        // Update max length
        maxLength = Math.max(maxLength, R - L + 1);
    }
    
    return maxLength;
}
```

## Interview Preparation Tips

1. **Pattern Recognition**: Be able to quickly identify which pattern (1-4) applies to the problem.

2. **Template Mastery**: Memorize the basic template for sliding window, especially the expand/shrink operations.

3. **Window State**: Be clear about what state you need to track within the window (sum, frequency of elements, etc.).

4. **Edge Cases**: Pay attention to empty arrays, negative numbers, and when pointers could cross each other.

5. **Optimization Questions**: Interviewers may ask "Can we do better?" after you present the O(2n) solution - be ready with the O(n) optimization.

6. **Time/Space Analysis**: Be prepared to analyze the time and space complexity of your solution.

7. **Problem Variations**: Practice with variations like:
   - Find all subarrays with sum exactly K
   - Find longest substring with at most K distinct characters
   - Find shortest subarray with sum at least K

8. **Follow-up Questions**: "What if the array contains negative numbers?" is a common follow-up that may change your approach.

## Common Mistakes or Misunderstandings

1. **Improper Window Update**: Forgetting to update the window state when expanding or shrinking.

2. **Boundary Conditions**: Off-by-one errors in window size calculation (R - L + 1).

3. **Overcomplicated Implementation**: Using complicated data structures when a simple two-pointer approach would suffice.

4. **Complete Shrinking vs. Single Shrink**: Using complete shrinking when only length is needed or single shrinking when the actual subarray is required.

5. **Optimizing Too Early**: Implementing the optimized version without understanding the core sliding window mechanics.

6. **Inconsistent Indices**: Mixing 0-indexed and 1-indexed thinking.

7. **Improper Termination**: Not handling cases where valid windows extend to the end of the array.

8. **Unnecessary Checks**: Adding redundant condition checks that slow down the solution.

## Advanced Notes / Behind the Scenes

1. **Amortized Analysis**: While the sliding window appears to have nested loops (suggesting O(n²)), each element is processed at most twice, giving amortized O(n) time complexity.

2. **Monotonic Queues/Stacks**: For more complex problems, sliding windows can be combined with monotonic queues to track minimums/maximums in the window efficiently.

3. **Multi-dimensional Sliding Window**: The concept can be extended to 2D arrays for problems like maximum sum submatrix.

4. **Binary Search with Sliding Window**: For certain problems, binary search can be combined with sliding window to find optimal window sizes.

5. **Caterpillar Method**: Another name for the two-pointer technique used in competitive programming.

## Glossary

- **Subarray**: A contiguous sequence of elements within an array.
- **Two Pointer Technique**: An algorithmic approach using two pointers to solve array-related problems.
- **Sliding Window**: A variation of two pointer where a "window" of elements is maintained.
- **Window Size**: The number of elements currently in the window (R - L + 1).
- **Expand Operation**: Increasing window size by moving the right pointer.
- **Shrink Operation**: Decreasing window size by moving the left pointer.
- **Window State**: Information maintained about the current window (e.g., sum, frequency map).
- **Amortized Analysis**: Method for analyzing a sequence of operations to understand the average performance.
