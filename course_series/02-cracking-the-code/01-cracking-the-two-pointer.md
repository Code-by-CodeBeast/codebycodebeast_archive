# 1. Cracking the Two Pointer Technique

**Status:** **IN PROGRESS**

**File:** course_series/02-cracking-the-code/01-cracking-the-two-pointer.md

**Title:**
Cracking the Two Pointer Technique

**Description:**
In this comprehensive guide to the Two Pointer Technique, we'll explore one of the most fundamental algorithmic patterns in coding interviews. The Two Pointer approach is an elegant way to solve array and string problems with improved time complexity. We'll cover various scenarios where this technique shines, from solving simple problems like finding pairs in a sorted array to more complex challenges like three-sum problems and palindrome verification. By the end of this article, you'll have a solid understanding of when and how to apply the Two Pointer Technique effectively in your coding interviews.

**Tags:**
two pointer technique, coding interviews, algorithm patterns, array algorithms, string algorithms, data structures and algorithms, coding problems, leetcode solutions, interview preparation, time complexity optimization, space complexity, algorithmic problem solving, coding patterns, three sum problem, palindrome algorithms, sorted array algorithms, sliding window technique, pointer manipulation, coding interview techniques, algorithm efficiency, programming interviews, technical interviews, DSA problems, algorithm strategies, coding optimization, interview coding patterns, algorithm fundamentals, coding best practices, problem solving techniques, algorithm design patterns, coding interview preparation, competitive programming

### Learning Approach

To master the Two Pointer Technique, we'll follow a structured learning path:

1. **Understanding the Basics**
   We'll start by introducing what two pointers technique is and how it works.

2. **Problem Walkthrough**
   Let's solve a classic problem with the technique:

   **Brute Force Solution:**

   - Use nested loops to check every possible pair
   - Time Complexity: O(n²)
   - Space Complexity: O(1)

   **Two Pointer Solution:**

   - Use left and right pointers to efficiently navigate the sorted array
   - Time Complexity: O(n)
   - Space Complexity: O(1)

3. **Pattern Recognition**
   After solving this initial problem, we'll break down:

   - Key characteristics that suggest using two pointers
   - Common variations of the technique
   - How to adapt the pattern for different problem types

4. **Practice Problems**
   We'll solve a variety of problems that use the two pointer technique:
   Each problem will reinforce the pattern while introducing new twists and considerations.

### Why This Approach Works

This learning structure helps you:

- Build intuition by comparing solutions
- Understand the efficiency gains
- Recognize pattern applicability in new problems
- Develop a systematic approach to problem-solving

In the following sections, we'll dive deep into each part of this approach, starting with our example problem...

### Problem Walkthrough

A common interview question that comes on is the [two sum](https://leetcode.com/problems/two-sum/) / [two sum II](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) and it's variation [three sum](https://leetcode.com/problems/3sum/).

#### Two Sum

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

<br>

> Note that: The second variation two sum II is the same problem but the array is sorted in ascending order and returns the indices of the two numbers that add up to the target.

##### Questions to ask:

- What does my input look like?
  - Is the array sorted in ascending order?
  - Is the array sorted in descending order?
  - Is the array sorted in random order?
  - Can there be duplicates?
- Will the sum cause integer overflow?
- Can there be negative numbers?
- What if there are no solutions?
- What if there is only one solution?

> The question from leetcode clearly answers some of these questions with assumptions but in the case of an interview, you should ask these questions to the interviewer to make sure you are on the right track.

##### Brute Force Solution

In interviews, it's important to show your thought process and not just the solution. You can start off with a brute force solution and then optimize it.

The most straightforward approach is to use nested loops to check every possible pair of numbers:

```js
   // iteration 1
         ___________________
         |                 |
   1  -  2  -  3  -  4  -  5
   ^
   |
   point

   * Start with the first point and iterate through the remaining elements (in this case from 2 to 5) to find the second point that sums up to the target.

   // iteration 2
               _____________
               |           |
   1  -  2  -  3  -  4  -  5
         ^
         |
         point

   * Start with the second point and iterate through the remaining elements (in this case from 3 to 5) to find the second point that sums up to the target. In the case of an array sorted in ascending order, you can ignore the thge nested iteration on the first element since it will not matter when finding the sum.

   ...Keep iterating until you find the sum or the end of the array.
```

This results in a nested loop which is `O(N^2)` time complexity.

##### Finding the better solution

- Find insights from the brute force solution or the problem statement.
- Ask for a hint from the interviewer.
- Find algorithm patterns that can be applied to the problem based off the runtimes.

An improvement from the brute force solution which results in `O(N^2)` time complexity is to find a solution that reduces that to a `O(N log N)`.

**Note that: `O(N log N) > O(N^2)`**

---

###### Typically, an `O(N log N)` time complexity can arise from:

1. **Sorting Operations**

   - Most comparison-based sorting algorithms (QuickSort, MergeSort, TimSort) run in `O(N log N)`
   - Example: Arrays.sort(), .sort(), sorted()

2. **Divide and Conquer Algorithms**

   - Repeatedly dividing the problem into halves
   - Example: MergeSort, QuickSort

3. **Combined Operations**
   - Using sorting followed by linear scanning: `O(N log N) + O(N) = O(N log N)`
   - Using binary search (`O(log N)`) for each element in array (N times): `O(N log N)`

**We can make use of the binary search to find the solution for this problem.**

> Binary search in itself is `O(log N)` but since we are scanning through the array, it becomes `O(N log N)`. So whenever you know for sure the input array is always sorted, you can use a binary search to if the problem involves a search.

We are going to talk more about binary search in other articles.

##### The best solution (Two Pointer Technique)

One solution that results in a linear solution (`O(N)` time complexity) is to use the two pointer technique.
`O(N) > O(N log N)`

The two pointer technique involves:

- Sorting the array (works better when the array is already sorted)
- Using two pointers to scan through the array.

> The two pointer technique is a great way to solve problems that involve finding pairs in a sorted array.

```js
   // iteration 1
   target = 9
   sum = 2 + 9 = 11
   2  -  4  -  5  -  9
   ^                 ^
   |                 |
   front             back

   * The sum for this iteration is greater than the target, but we dont have more information on the difference between the elements of the input array. So what we can do it to move the front pointer to the right to find a smaller sum as a test.

   // iteration 2
   target = 9
   sum = 4 + 9 = 13
   2  -  4  -  5  -  9
         ^           ^
         |           |
         front       back

   * Since the array is sorted and we now know that the sum of the pointers second iteration is greater than the sum of the pointers of the first iteration, we can move the back pointer to the left to find a smaller sum.

   // iteration 3
   target = 9
   sum = 4 + 5 = 9
   2  -  4  -  5  -  9
         ^     ^
         |     |
         front back

   * Now we arrive at a solution
```

###### The solution in python:

```python
def two_sum(nums, target):
    # Sort the array first - this is crucial for the two pointer approach to work
    # Note that this step is not necessary for the two sum II problem since the array is already sorted.
    nums.sort()

    # Initialize two pointers at opposite ends of the array
    left, right = 0, len(nums) - 1

    # Continue until pointers meet
    while left < right:
        # Calculate current sum of elements at both pointers
        current_sum = nums[left] + nums[right]

        # If we found the target sum, return the indices
        if current_sum == target:
            return [left, right]

        # If sum is too small, we need a larger number
        # Move left pointer to the right to get a bigger number
        elif current_sum < target:
            left += 1

        # If sum is too large, we need a smaller number
        # Move right pointer to the left to get a smaller number
        else:  # current_sum > target
            right -= 1

    # If no solution is found, the while loop will exit
    # You might want to return None or raise an exception here
    return None
```

###### The solution in js:

```js
const twoSum = (nums, target) => {
  // Sort the array first - this is crucial for the two pointer approach to work.
  // Note that this step is not necessary for the two sum II problem since the array is already sorted.
  nums.sort((a, b) => a - b);

  // Initialize two pointers at opposite ends of the array
  let left = 0;
  let right = nums.length - 1;

  // Continue until pointers meet
  while (left < right) {
    // Calculate current sum of elements at both pointers
    const currentSum = nums[left] + nums[right];

    // If we found the target sum, return the indices
    if (currentSum === target) return [left, right];

    // If sum is too small, move left pointer right to get bigger numbers
    if (currentSum < target) left++;
    // If sum is too large, move right pointer left to get smaller numbers
    else right--;
  }

  // If no solution is found
  return [];
};
```

> The time complexity of the two pointer technique is `O(N)` which is better than the `O(N log N)` time complexity of the binary search if and only if the input array is already sorted.

### Understanding the Basics

The Two Pointer Technique is a fundamental problem-solving pattern that can transform brute force solutions into elegant, efficient algorithms. At its core, this technique involves using two pointers to traverse through data (usually an array or string) in a coordinated way, often reducing the time complexity from O(n²) to O(n).

#### Types of Two Pointer Patterns

There are two main variations of the two pointer technique:

1. **Opposite Ends Pattern**

   - Pointers start from both ends and move toward each other
   - Perfect for sorted arrays or palindrome checks
   - Examples: [Two Sum in sorted array](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/) , [Container With Most Water](https://leetcode.com/problems/container-with-most-water/description/).

```js
   1  -  2  -  3  -  4  -  5  -  6  -  7
   ^                                   ^
   |                                   |
   front                               back

   // iteration 1
   sum = 8
   1  -  2  -  3  -  4  -  5  -  6  -  7
   ^                                   ^
   |                                   |
   front                               back

   // iteration 2
   sum = 9
   1  -  2  -  3  -  4  -  5  -  6  -  7
         ^                             ^
         |                             |
         front                         back

   // iteration 3
   sum = 8
   1  -  2  -  3  -  4  -  5  -  6  -  7
         ^                       ^
         |                       |
         front                   back

   // iteration 4
   sum = 8
   1  -  2  -  3  -  4  -  5  -  6  -  7
               ^           ^
               |           |
               front       back
```

2. **Same Direction Pattern**
   - Both pointers move in the same direction at different paces
   - Also called the Tortoise and Hare or fast and slow pointer pattern
   - Often used for linked list problems or finding patterns in sequences
   - Examples: Fast and slow pointers for cycle detection, sliding window

```js
   1  -  2  -  3  -  4  -  5  -  6  -  7
   ^     ^
   |     |
   slow  fast

   // iteration 1
   sum = 3
   1  -  2  -  3  -  4  -  5  -  6  -  7
   ^     ^
   |     |
   slow  fast

   // iteration 2
   sum = 4
   1  -  2  -  3  -  4  -  5  -  6  -  7
   ^           ^
   |           |
   slow        fast
```

> The two pointer technique is like having two fingers moving through a book - they can work together to find what you're looking for much faster than checking every page one by one.

### Pattern Breakdown and Recognition

#### When to Use Two Pointers?

Look for these characteristics in a problem:

1. **Sequential Data Structure**

   - Arrays, strings, or linked lists
   - Need to traverse or compare elements

2. **Searching Pairs or Subarrays**

   - Finding two elements that satisfy a condition
   - Looking for subarrays with specific properties

3. **Sorted Data**

   - Input is sorted or sorting wouldn't affect the result
   - Need to find elements that sum to a target

4. **Space Optimization Required**
   - Need O(1) space complexity
   - In-place array modifications

#### Key Implementation Patterns

1. **Initialization**

```python
# Opposite ends
left, right = 0, len(array) - 1

# Same direction
slow = 0
fast = 0  # or fast = 1 depending on the problem
```

2. **Movement Conditions**

```python
# Opposite ends - move based on target comparison
while left < right:
    if condition < target:
        left += 1
    elif condition > target:
        right -= 1
    else:
        return result

# Same direction - move at different speeds
while fast < len(array):
    if some_condition:
        slow += 1
    fast += 1
```

3. **Common Optimization Techniques**

```python
# Pre-sort if needed
nums.sort()

# Handle duplicates in sorted array
if i > 0 and nums[i] == nums[i-1]:
    continue

# Skip processed elements (common in k-sum problems)
while left < right and nums[left] == nums[left - 1]:
    left += 1
while left < right and nums[right] == nums[right + 1]:
    right -= 1
```

#### Common Pitfalls to Avoid

1. **Pointer Movement**

   - Ensure pointers don't cross inappropriately
   - Be careful with increment/decrement timing
   - Handle equal values correctly

2. **Edge Cases**

   - Empty input
   - Single element
   - Duplicate elements
   - Input size less than window size (for cases where we are looking for a subarray)

3. **Infinite Loops**
   - Always ensure pointer movement
   - Verify loop termination conditions
   - Check boundary conditions

#### Problem-Solving Framework

When approaching a two-pointer problem:

1. **Analyze the Input**

   - Is it sorted?
   - Are there duplicates?
   - What are the constraints?

2. **Choose the Pattern**

   - Opposite ends for range-based problems
   - Same direction for sequential processing

3. **Initialize Correctly**

   - Set starting positions
   - Consider pre-processing (like sorting)

4. **Define Movement Logic**

   - When to move each pointer
   - How to move them
   - When to stop

5. **Handle Edge Cases**
   - Test with minimal input
   - Consider boundary conditions
   - Verify termination cases

### Common Applications and Two Pointer Problems on LeetCode

The technique is particularly effective for:

1. Finding Pairs in Sorted Arrays

   - [Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
   - [Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/)
   - [Sum of Three Values](https://leetcode.com/problems/3sum/)

2. Detecting Cycles in Linked Lists

   - [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
   - [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
   - [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)

3. String Palindrome Verification

   - [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
   - [Valid Palindrome II](https://leetcode.com/problems/valid-palindrome-ii/)
   - [Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)

4. Three Sum Problems\*\*

   - [3Sum](https://leetcode.com/problems/3sum/)
   - [3Sum Closest](https://leetcode.com/problems/3sum-closest/)

5. Removing Duplicates\*\*

   - [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
   - [Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)
   - [Remove Element](https://leetcode.com/problems/remove-element/)

6. Finding Subarrays\*\*

   - [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)
   - [Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)
   - [Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/)

7. Array Partitioning\*\*

   - [Sort Colors](https://leetcode.com/problems/sort-colors/)
   - [Partition Labels](https://leetcode.com/problems/partition-labels/)
   - [Move Zeroes](https://leetcode.com/problems/move-zeroes/)

8. **Area/Range Problems** (New Category)
   - [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
   - [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
   - [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

### Solving More Problems

##### [Container With Most Water](https://leetcode.com/problems/container-with-most-water/description/)

**Problem Statement:**
You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Notice that you may not slant the container.

```py
# Example 1
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

# Example 2
Input: height = [1,1]
Output: 1

# Constraints:
n == height.length
2 <= n <= 105
0 <= height[i] <= 104
```

**Solution:**
Let's solve this step by step using the two pointer technique.

1. **Key Insights:**

   - Area = width × height
   - Width = right_index - left_index
   - Height = min(height[left], height[right])
   - Need to maximize: (right - left) × min(height[left], height[right])

2. **Why Two Pointers?**
   - We need to find two lines to form the container
   - Moving inward from the ends lets us try all possible widths
   - We can make intelligent decisions about which pointer to move

Here's the solution in Python:

```python
def maxArea(height: List[int]) -> int:
    # Initialize two pointers at the ends
    left, right = 0, len(height) - 1

    # Variable to store maximum area
    max_area = 0

    # Continue until pointers meet
    while left < right:
        # Calculate width between lines
        width = right - left

        # Calculate height (limited by shorter line)
        container_height = min(height[left], height[right])

        # Calculate and update maximum area
        current_area = width * container_height
        max_area = max(max_area, current_area)

        # Move the pointer at the shorter line inward
        # (because keeping the shorter line can't give us a larger area)
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1

    return max_area
```

##### [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)

**Problem Statement:**
Given an array of integers nums containing n + 1 integers where each integer is in the range [1, n] inclusive.

There is only one repeated number in nums, return this repeated number.

You must solve the problem without modifying the array nums and uses only constant extra space.

```python
# Example 1:
Input: nums = [1,3,4,2,2]
Output: 2

# Example 2:
Input: nums = [3,1,3,4,2]
Output: 3

# Constraints:
1 <= n <= 10^5
nums.length == n + 1
1 <= nums[i] <= n

All the integers in nums appear only once except for precisely one integer which appears two or more times.
```

**Solution:**
Let's solve this using Fast and Slow Pointer (Floyd's Cycle Detection).

1. **Key Insights:**

   - Array can be viewed as a linked list where nums[i] points to nums[nums[i]]
   - Due to the duplicate, there must be a cycle
   - We can use fast and slow pointers to detect the cycle
   - The start of the cycle is our duplicate number

2. **Why Two Pointers?**
   - Fast pointer moves twice as fast as slow pointer
   - They will eventually meet inside the cycle
   - Distance calculations help find cycle start
   - Perfect for constant space requirement

Here's the solution in Python:

```python
def findDuplicate(nums: List[int]) -> int:
    # Phase 1: Find the intersection point inside the cycle
    slow = fast = nums[0]

    # Move until they meet
    while True:
        slow = nums[slow]          # Move one step
        fast = nums[nums[fast]]    # Move two steps
        if slow == fast:
            break

    # Phase 2: Find the entrance to the cycle
    slow = nums[0]
    while slow != fast:
        slow = nums[slow]
        fast = nums[fast]

    return slow
```

3. **Why This Works:**

   - If there's a duplicate number, it creates a cycle in our "linked list"
   - When fast and slow pointers meet:
     1. Slow has traveled distance k
     2. Fast has traveled distance 2k
     3. The difference (k) is a multiple of cycle length
   - Moving one pointer to start and moving both at same speed:
     - They will meet at cycle entrance (our duplicate)

4. **Time and Space Complexity:**

   - Time Complexity: O(n)
   - Space Complexity: O(1)

5. **Example Walkthrough:**

```python
nums = [1,3,4,2,2]

# Visualize as linked list:
# index: 0->1->3->2->4
#           ^     |
#           |_____|

# Phase 1:
slow = 1->3->2->4->2->4->2...
fast = 1->4->2->2->2->2...
# Meet at 2

# Phase 2:
slow = 1->3->2
fast = 2->4->2
# Meet at 2 (our answer)
```

6. **Important Notes:**
   - This solution satisfies all constraints:
     - Doesn't modify the array
     - Uses constant extra space
     - Works for multiple occurrences of duplicate
   - The solution is optimal in both time and space

##### [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

**Problem Statement:**
A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward.

Given a string s, return true if it is a palindrome, or false otherwise.

```python
# Example 1:
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.

# Example 2:
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.

# Example 3:
Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.

# Constraints:
1 <= s.length <= 2 * 105
s consists only of printable ASCII characters.
```

**Solution:**
Let's solve this using the opposite ends two-pointer technique.

1. **Key Insights:**

   - Only consider alphanumeric characters
   - Case-insensitive comparison
   - Empty string or single character is palindrome
   - Two pointers moving from outside to inside

2. **Why Two Pointers?**
   - Need to compare characters from both ends
   - Can skip non-alphanumeric characters efficiently
   - No need to create new string/array
   - Achieves O(n) time with O(1) space

Here's the solution in Python:

```python
def isPalindrome(s: str) -> bool:
    # Initialize two pointers at the ends
    left, right = 0, len(s) - 1

    while left < right:
        # Skip non-alphanumeric characters from left
        while left < right and not s[left].isalnum():
            left += 1

        # Skip non-alphanumeric characters from right
        while left < right and not s[right].isalnum():
            right -= 1

        # Compare characters (case-insensitive)
        if s[left].lower() != s[right].lower():
            return False

        left += 1
        right -= 1

    return True
```

3. **Why This Works:**

   - Skips all non-alphanumeric characters
   - Compares characters in case-insensitive manner
   - If all comparisons match, it's a palindrome
   - If any comparison fails, it's not a palindrome

4. **Time and Space Complexity:**

   - Time Complexity: O(n) - each character is visited at most twice
   - Space Complexity: O(1) - only using two pointers

5. **Example Walkthrough:**

```python
s = "A man, a plan, a canal: Panama"

# Step by step:
left = 0, right = 30  # 'A' and 'a'
left = 1, right = 29  # 'm' and 'm'
left = 2, right = 28  # 'a' and 'a'
left = 3, right = 27  # 'n' and 'n'
# ... continue comparing
# All matches = palindrome
```

##### [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

**Problem Statement:**
Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place. The relative order of the elements should be kept the same. Return k after placing the final result in the first k slots of nums.

```python
# Example 1:
Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]

# Example 2:
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
```

**Simple Solution:**

```python
def removeDuplicates(nums: List[int]) -> int:
    # Edge case: empty array
    if not nums:
        return 0

    # Position to place next unique element
    k = 1

    # Iterate through array starting from second element
    for i in range(1, len(nums)):
        # If current number is different from previous
        if nums[i] != nums[i-1]:
            nums[k] = nums[i]
            k += 1

    return k
```

**Why it works:**

- k keeps track of where to place next unique element
- Only moves forward when we find a new unique number
- Array being sorted means duplicates are always adjacent

##### [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

**Problem Statement:**
Given an array of positive integers nums and a positive integer target, return the minimal length of a subarray whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.

```python
# Example 1:
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.

# Example 2:
Input: target = 4, nums = [1,4,4]
Output: 1
```

**Simple Solution (Sliding Window):**

```python
def minSubArrayLen(target: int, nums: List[int]) -> int:
    min_length = float('inf')
    window_sum = 0
    start = 0

    for end in range(len(nums)):
        # Add number to current window
        window_sum += nums[end]

        # Shrink window while sum >= target
        while window_sum >= target:
            # Update minimum length
            min_length = min(min_length, end - start + 1)
            # Remove from window start
            window_sum -= nums[start]
            start += 1

    return min_length if min_length != float('inf') else 0
```

**Why it works:**

- Uses sliding window technique (variation of two pointers)
- Expands window until sum >= target
- Shrinks window while maintaining sum >= target
- Keeps track of minimum window size seen

### Conclusion

The Two Pointer Technique is a powerful algorithmic pattern that can significantly improve the efficiency of your solutions. Through this guide, we've explored:

- The fundamental concept and mechanics of the two pointer approach
- How it improves time complexity from O(n²) to O(n) in many cases
- Various applications including sliding window variations
- Practical problem-solving strategies using this technique

By mastering the Two Pointer Technique, you'll be better equipped to tackle a wide range of coding interview problems efficiently. Remember that recognizing when to apply this pattern is just as important as knowing how to implement it. Keep practicing with different variations of problems to build your pattern recognition skills and problem-solving intuition.
