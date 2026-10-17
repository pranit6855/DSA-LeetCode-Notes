# Greedy Pattern — Question 9
# LC 1005 — Maximize Sum of Array After K Negations

## 1. Problem Details

- **LeetCode Number:** 1005
- **Pattern:** Greedy
- **Difficulty:** Easy
- **Topics:** Greedy, Sorting, Array

## 2. Problem Statement

We are given an integer array `nums` and an integer `k`.

In one operation, we can choose any index and multiply
the element at that index by -1.

For example:

    5  -> -5
    -4 -> 4

We must perform exactly `k` operations.

Our task is to maximize the sum of the array after
performing these operations.

Return the maximum possible sum.

Important:
- The same element can be selected multiple times.
- An element can be flipped from positive to negative
  and then back to positive.
- Exactly `k` operations must be performed.

## 3. Example

Input:

    nums = [4, 2, 3]
    k = 1

Output:

    5

Explanation:

Initially:

    [4, 2, 3]

The sum is:

    4 + 2 + 3 = 9

We must perform one operation.

Flip the smallest element, 2:

    [4, -2, 3]

New sum:

    4 + (-2) + 3 = 5

Therefore, the maximum possible sum is 5.

## 4. Greedy Concept

Greedy means making a suitable choice at each step
to maximize the overall result.

In this problem, the main idea is:

**First flip negative numbers to positive. If operations
remain, handle them using the smallest element.**

Why?

A negative number reduces the sum.

For example:

    -8 -> 8

The sum increases by 16.

Therefore, converting negative numbers into positive
numbers is beneficial whenever operations are available.

After flipping negative numbers, some operations may
still remain.

If the remaining number of operations is odd, we must
flip one number an odd number of times.

To minimize the decrease in the sum, we flip the
smallest absolute-value element.

## 5. Approach — Step by Step

### Step 1: Sort the array

    sort(nums.begin(), nums.end());

Ascending order puts the smallest values first.

Negative numbers appear before positive numbers.

Example:

    [-4, -2, 3]

### Step 2: Flip negative numbers

Traverse the array.

If the current number is negative and `k > 0`:

    nums[i] = -nums[i];
    k--;

This changes the sign and uses one operation.

Continue until:
- All negative numbers have been processed, or
- All operations have been used.

### Step 3: Sort the array again

    sort(nums.begin(), nums.end());

If operations remain after the first loop, all negative
numbers have already been flipped.

Sorting again puts the smallest non-negative value at
index 0.

### Step 4: Check whether k is odd

    if (k % 2 == 1) {
        nums[0] = -nums[0];
    }

If `k` is odd, flip the smallest element once.

If `k` is even, no additional change is required.

Why?

Flipping the same element twice restores its original
value.

Therefore:
- An even number of remaining flips has no net effect.
- An odd number of remaining flips has the same net
  effect as one flip.

### Step 5: Calculate the sum

Add all the elements and return the total.

## 6. C++ Code

```cpp
class Solution {
public:
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());

        for (int i = 0; i < nums.size() && k > 0; i++) {
            if (nums[i] < 0) {
                nums[i] = -nums[i];
                k--;
            }
        }

        sort(nums.begin(), nums.end());

        if (k % 2 == 1) {
            nums[0] = -nums[0];
        }

        int sum = 0;

        for (int x : nums) {
            sum += x;
        }

        return sum;
    }
};
```

## 7. Code Explanation

### A. First Sorting

```cpp
sort(nums.begin(), nums.end());
```

Arranges the array in ascending order.

Example:

    [3, -4, 2, -1]

After sorting:

    [-4, -1, 2, 3]

Negative numbers are now at the beginning, so we
can process them first.

### B. The for Loop

```cpp
for (int i = 0; i < nums.size() && k > 0; i++)
```

The loop continues while:
- The current index is within the array.
- At least one operation remains.

If `k` becomes zero, the loop stops because there
are no operations left.

### C. Check for Negative Numbers

```cpp
if (nums[i] < 0) {
    nums[i] = -nums[i];
    k--;
}
```

If the current element is negative, change its sign.

For example:

    nums[i] = -4

After:

    nums[i] = -(-4)
    nums[i] = 4

One operation has been used, so:

    k--;

Important: `k--` happens only when a negative number
is actually flipped in this loop.

If no negative numbers remain, `k` can still be
greater than zero.

### D. Sort Again

```cpp
sort(nums.begin(), nums.end());
```

After the first loop, if operations remain, all negative
numbers have been flipped.

The second sorting puts the smallest value at `nums[0]`.

Example:

    Before sorting: [4, 2, 3]
    After sorting:  [2, 3, 4]

We need the smallest value because flipping it causes
the smallest possible decrease in the sum.

### E. Odd Operations

```cpp
if (k % 2 == 1) {
    nums[0] = -nums[0];
}
```

The `%` operator gives the remainder.

Examples:

    3 % 2 = 1
    2 % 2 = 0
    4 % 2 = 0

If `k` is odd, flip the smallest element once.

If `k` is even, do nothing.

Example with two remaining operations:

    2 -> -2 -> 2

Both flips cancel each other.

Example with three remaining operations:

    2 -> -2 -> 2 -> -2

The net result is equivalent to one flip.

### F. Calculate the Sum

```cpp
int sum = 0;

for (int x : nums) {
    sum += x;
}
```

Traverse the final array and add every element.

Finally:

```cpp
return sum;
```

Return the maximum possible sum.

## 8. Detailed Dry Run

Input:

    nums = [-4, -2, 3]
    k = 3

### Step 1: Initial Sorting

The array is already sorted.

    nums = [-4, -2, 3]
    k = 3

### Step 2: First Negative Number

At index 0:

    nums[0] = -4

Since it is negative and `k > 0`, flip it.

    nums[0] = 4
    k = 3 - 1 = 2

Array:

    [4, -2, 3]

### Step 3: Second Negative Number

At index 1:

    nums[1] = -2

It is still negative and `k > 0`.

Flip it.

    nums[1] = 2
    k = 2 - 1 = 1

Array:

    [4, 2, 3]

### Step 4: Third Element

At index 2:

    nums[2] = 3

It is not negative, so no operation is performed.

The remaining operation count stays:

    k = 1

At this point, all negative numbers have been
processed, but one operation remains.

### Step 5: Second Sorting

Sort the array again:

    [4, 2, 3]

Becomes:

    [2, 3, 4]

The smallest element is now:

    nums[0] = 2

### Step 6: Check Odd or Even

The remaining operation count is:

    k = 1

Check:

    k % 2 == 1
    1 % 2 == 1

True.

Flip the smallest element:

    nums[0] = -nums[0]

Final array:

    [-2, 3, 4]

### Step 7: Calculate Sum

    sum = -2 + 3 + 4
    sum = 5

Output:

    5

## 9. Important Case — Many Negative Numbers but k Remains

Input:

    nums = [-20, -18, -15, -12, -10, -8, -6,
            -4, -2, -1, 5, 7, 9]

    k = 13

There are 10 negative numbers.

After flipping all 10 negative numbers:

    k = 13 - 10
    k = 3

Array after these flips:

    [20, 18, 15, 12, 10, 8, 6, 4, 2, 1, 5, 7, 9]

Sort again:

    [1, 2, 4, 5, 6, 7, 8, 9, 10, 12, 15, 18, 20]

Now:

    k = 3

Three operations remain.

We can apply all three to the smallest element:

    1 -> -1 -> 1 -> -1

The final result is equivalent to flipping `1` once.

Therefore, the final array becomes:

    [-1, 2, 4, 5, 6, 7, 8, 9, 10, 12, 15, 18, 20]

Final sum:

    115

Output:

    115

## 10. Why Do We Check Odd and Even?

Suppose all negative numbers have been flipped and
the remaining array is:

    [2, 3, 4]

### Case 1: k = 3

Three operations remain.

    2 -> -2 -> 2 -> -2

The final result is equivalent to one flip.

So the smallest element becomes negative.

### Case 2: k = 2

Two operations remain.

    2 -> -2 -> 2

The final result is the same as the starting array.

No additional flip is needed.

### General Rule

    If k is odd:
        Flip the smallest element once.

    If k is even:
        Do not flip anything further.

This works because performing two flips on the same
element cancels their overall effect.

## 11. Dry Run Table

Input:

    nums = [-4, -2, 3]
    k = 3

| Step | Action | Array | k |
|---|---|---|---:|
| Initial | Start | [-4, -2, 3] | 3 |
| 1 | Flip -4 | [4, -2, 3] | 2 |
| 2 | Flip -2 | [4, 2, 3] | 1 |
| 3 | Positive number, skip | [4, 2, 3] | 1 |
| 4 | Sort again | [2, 3, 4] | 1 |
| 5 | Odd k, flip smallest | [-2, 3, 4] | 1 |
| Final | Calculate sum | [-2, 3, 4] | — |

Final sum:

    -2 + 3 + 4 = 5

Output:

    5

## 12. Complexity Analysis

Let `n` be the size of the array.

### Time Complexity: O(n log n)

We sort the array twice.

Each sorting operation takes O(n log n).

The traversal and sum calculation take O(n).

Overall:

    O(n log n)

### Extra Space Complexity: O(1)

Apart from the input array, we use only a constant
number of variables.

The auxiliary space used by the sorting implementation
may vary.

## 13. Important Interview Points

1. Flip negative numbers first because this increases
   the sum.
2. Decrease `k` only when an operation is performed.
3. The remaining `k` can be greater than zero even
   after every negative number has been flipped.
4. Sort again to find the smallest element.
5. If `k` is odd, flip the smallest element once.
6. If `k` is even, no additional flip is needed.
7. Two flips on the same element cancel each other.
8. Finally
