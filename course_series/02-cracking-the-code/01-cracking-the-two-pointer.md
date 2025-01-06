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

This results in a nested loop which is `O(n^2)` time complexity.

##### Finding the better solution
- Find insights from the brute force solution or the problem statement.
- Ask for a hint from the interviewer.
- Find algorithm patterns that can be applied to the problem based off the runtimes.

An improvement from the brute force solution which results in O(n^2) time complexity is to find a solution that reduces that to a O(N log N).

`O(N log N) > O(N^2)`

Typically an `O(N log N)` Algorithm involves:
- Sorting the array
- Scanning through the array with a binary search. 

> Binary search in itself is `O(log N)` but since we are scanning through the array, it becomes O(N log N). So whenever you know for sure the input array is always sorted, you can use a binary search to if the problem involves a search.

We are going to talk more about binary search in other articles.

##### The best solution (Two Pointer Technique)
One solution that results in a linear solution (`O(N)` time complexity) is to use the two pointer technique.
`O(N) > O(N log N)`

The two pointer technique involves:
- Sorting the array (works better when the array is already sorted)
- Using two pointers to scan through the array.

```js
   // iteration 1
   target = 9
   sum = 2 + 9 = 11
   2  -  4  -  5  -  9
   ^                 ^
   |                 |
   front             back

   * The sum for this iteration is greater than the target, but we dont have more information on the difference between the elements of the input array. So what we can do it to move thje front pointer to the right to find a smaller sum as a test.

   // iteration 2
   target = 9
   sum = 4 + 9 = 13  
   2  -  4  -  5  -  9
         ^           ^
         |           |
         front       back

   * Since tne array is sorted and we now know that the sum of the pointers second iteration is greater than the sum of the pointers of the first iteration, we can move the back pointer to the left to find a smaller sum.

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


### Understanding the Basics

The Two Pointer Technique is a fundamental problem-solving pattern that can transform brute force solutions into elegant, efficient algorithms. At its core, this technique involves using two pointers to traverse through data (usually an array or string) in a coordinated way, often reducing the time complexity from O(n²) to O(n).

#### Types of Two Pointer Patterns

There are two main variations of the two pointer technique:

1. **Opposite Ends Pattern**

   - Pointers start from both ends and move toward each other
   - Perfect for sorted arrays or palindrome checks
   - Examples: Two Sum in sorted array, Container With Most Water

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













// Pattern Breakdown and Recognition






#### Common Applications

The technique is particularly effective for:

- Finding pairs in sorted arrays
- Detecting cycles in linked lists
- String palindrome verification
- Three sum problems
- Removing duplicates from sorted arrays
- Finding subarrays with specific properties
- Partitioning arrays (like Dutch National Flag problem)

### Conclusion

Now that you have both apps running, you’ll realize that VGV did a good job of adding splash screens and app icons but that’s not what we want. I’m going to walk you through how to have custom app icons and splash screens.

#### Useful Links

- Very Good CLI's official documentation [here](https://cli.vgv.dev/docs/overview)
- Learn more about [Very Good Start](https://verygood.ventures/solution/very-good-start)
