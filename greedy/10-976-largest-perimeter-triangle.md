# Greedy Pattern — Question 10
# LC 976 — Largest Perimeter Triangle

## 1. Problem Details

- **LeetCode Number:** 976
- **Pattern:** Greedy
- **Difficulty:** Easy
- **Topics:** Greedy, Sorting, Array

## 2. Problem Statement

We are given an integer array `nums`, where each element
represents the length of a possible side of a triangle.

Our task is to select three sides that form a valid triangle
and have the largest possible perimeter.

Return the maximum possible perimeter.

If no valid triangle can be formed, return 0.

## 3. Example 1

Input:

    nums = [2, 1, 2]

Output:

    5

Explanation:

The three side lengths are:

    2, 1, 2

These sides can form a valid triangle because:

    1 + 2 > 2

Perimeter:

    2 + 1 + 2 = 5

Therefore, the answer is 5.

## 4. Example 2

Input:

    nums = [1, 2, 1, 10]

Output:

    0

Explanation:

After sorting:

    [1, 1, 2, 10]

Try the three largest sides:

    1, 2, 10

Triangle condition:

    1 + 2 > 10

This is false.

Try the next three largest sides:

    1, 1, 2

Triangle condition:

    1 + 1 > 2

This is also false because 2 is equal to 2.

Therefore, no valid triangle can be formed.

Output:

    0

## 5. Important Triangle Condition

Suppose the three sides are sorted in ascending order:

    a <= b <= c

A valid triangle must satisfy:

    a + b > c

The sum of the two smaller sides must be strictly
greater than the largest side.

Important:
- If a + b > c, a valid triangle can be formed.
- If a + b == c, a valid triangle cannot be formed.
- If a + b < c, a valid triangle cannot be formed.

This condition is the main logic of the problem.

## 6. Greedy Concept

Greedy means making a suitable choice at each step to
obtain the best possible result.

In this problem, we want the largest perimeter.

The main idea is:

**Sort the array and check the largest three sides first.**

Why?

The perimeter is the sum of the three side lengths.

Larger sides can give a larger perimeter, so we start
with the largest available sides.

If those three sides form a valid triangle, their
perimeter is the maximum possible.

If they do not form a valid triangle, we try the next
largest combination.

We continue until we find a valid triangle or run out
of possible combinations.

## 7. Approach — Step by Step

### Step 1: Sort the array

    sort(nums.begin(), nums.end());

The array is arranged in ascending order.

Example:

    Before: [2, 1, 2]
    After:  [1, 2, 2]

The largest sides are now at the end of the array.

### Step 2: Start from the end

    for (int i = nums.size() - 1; i >= 2; i--)

We start from the largest element.

At every index `i`, the three candidate sides are:

    nums[i - 2]
    nums[i - 1]
    nums[i]

Because the array is sorted, these three sides are in
ascending order.

### Step 3: Check the triangle condition

    if (nums[i - 1] + nums[i - 2] > nums[i])

The two smaller sides must have a sum greater than
the largest side.

If the condition is true, these sides form a valid triangle.

### Step 4: Return the perimeter

    return nums[i] + nums[i - 1] + nums[i - 2];

We immediately return the sum of the three sides.

Since we check candidate triplets from largest to smaller,
the first valid triangle gives the maximum perimeter.

### Step 5: If no valid triangle exists

    return 0;

If the loop finishes without finding a valid triangle,
return 0.

## 8. Beginner-Friendly C++ Code

```cpp
class Solution {
public:
    int largestPerimeter(vector<int>& nums) {
        sort(nums.begin(), nums.end());

        for (int i = nums.size() - 1; i >= 2; i--) {
            if (nums[i - 1] + nums[i - 2] > nums[i]) {
                return nums[i] + nums[i - 1] + nums[i - 2];
            }
        }

        return 0;
    }
};
```

## 9. Code Explanation

### A. Sort the array

```cpp
sort(nums.begin(), nums.end());
```

This arranges the side lengths in ascending order.

Example:

    [2, 1, 2]

Becomes:

    [1, 2, 2]

Now the largest side is at the last index.

### B. Traverse from right to left

```cpp
for (int i = nums.size() - 1; i >= 2; i--)
```

We start from the largest side and move toward smaller sides.

We stop at index 2 because we need three elements:

    nums[i - 2]
    nums[i - 1]
    nums[i]

If `i` becomes smaller than 2, three elements are
no longer available.

### C. Check whether the sides form a triangle

```cpp
if (nums[i - 1] + nums[i - 2] > nums[i])
```

Because the array is sorted:

- `nums[i - 2]` is the smallest candidate side.
- `nums[i - 1]` is the middle candidate side.
- `nums[i]` is the largest candidate side.

We check whether the sum of the two smaller sides
is strictly greater than the largest side.

### D. Return the perimeter

```cpp
return nums[i] + nums[i - 1] + nums[i - 2];
```

If the triangle condition is satisfied, return the
sum of its three sides.

We can return immediately because we are checking the
largest candidate triplets first.

### E. Return zero

```cpp
return 0;
```

If no valid triangle is found, return 0.

## 10. Detailed Dry Run — Example 1

Input:

    nums = [2, 1, 2]

### Step 1: Sort the array

After sorting:

    nums = [1, 2, 2]

Array size:

    n = 3

The loop starts at:

    i = n - 1
    i = 2

### Step 2: First iteration

Current index:

    i = 2

Candidate sides:

    nums[i - 2] = nums[0] = 1
    nums[i - 1] = nums[1] = 2
    nums[i]     = nums[2] = 2

So the sides are:

    1, 2, 2

Check the triangle condition:

    nums[i - 1] + nums[i - 2] > nums[i]

Substitute the values:

    2 + 1 > 2
    3 > 2

This is true.

Therefore, these sides form a valid triangle.

Calculate the perimeter:

    2 + 2 + 1 = 5

The code returns:

    5

The loop stops because the answer has been found.

Final output:

    5

## 11. Dry Run — Example 2

Input:

    nums = [1, 2, 1, 10]

### Step 1: Sort the array

After sorting:

    nums = [1, 1, 2, 10]

Array size:

    n = 4

The loop starts at:

    i = 3

### Step 2: First iteration

Current index:

    i = 3

Candidate sides:

    nums[1] = 1
    nums[2] = 2
    nums[3] = 10

Check:

    1 + 2 > 10
    3 > 10

This is false.

The three largest sides cannot form a valid triangle.

Move to the next index:

    i = 2

### Step 3: Second iteration

Current index:

    i = 2

Candidate sides:

    nums[0] = 1
    nums[1] = 1
    nums[2] = 2

Check:

    1 + 1 > 2
    2 > 2

This is false.

Equality is not enough. The sum of the two smaller sides
must be strictly greater than the largest side.

The loop finishes.

No valid triangle was found.

The code returns:

    0

Final output:

    0

## 12. Dry Run Table

Example 1:

    nums = [2, 1, 2]

After sorting:

    [1, 2, 2]

| i | First Side | Second Side | Third Side | Triangle Condition | Result |
|---:|---:|---:|---:|---|---|
| 2 | 1 | 2 | 2 | 1 + 2 > 2 | Valid |

Perimeter:

    1 + 2 + 2 = 5

Output:

    5

Example 2:

    nums = [1, 2, 1, 10]

After sorting:

    [1, 1, 2, 10]

| i | First Side | Second Side | Third Side | Triangle Condition | Result |
|---:|---:|---:|---:|---|---|
| 3 | 1 | 2 | 10 | 1 + 2 > 10 | Invalid |
| 2 | 1 | 1 | 2 | 1 + 1 > 2 | Invalid |

Output:

    0

## 13. Why Does the Greedy Approach Work?

We want to maximize:

    a + b + c

Therefore, we first consider the three largest available
side lengths.

If they satisfy the triangle condition, no other triplet
can have a larger perimeter because no other triplet can
have a larger sum of three sides.

If they do not satisfy the condition, we move to the
next largest consecutive triplet.

Because the array is sorted, checking consecutive triplets
from right to left lets us find the first valid triangle
with the largest possible perimeter.

## 14. Complexity Analysis

Let `n` be the number of elements in `nums`.

### Time Complexity: O(n log n)

Sorting takes:

    O(n log n)

The loop checks at most `n - 2` candidate triplets:

    O(n)

Overall time complexity:

    O(n log n)

### Extra Space Complexity

The algorithm uses O(1) additional variables apart from
the input array.

The auxiliary space used by `sort()` depends on the
sorting implementation.

## 15. Important Interview Points

1. Sort the array in ascending order.
2. Start checking from the largest side.
3. Use three consecutive elements as candidate sides.
4. The triangle condition is `a + b > c`.
5. Equality is not enough to form a valid triangle.
6. Return the perimeter as soon as a valid triangle
   is found.
7. If no valid triangle exists, return 0.
8. Time complexity is O(n log n).

## 16. Key Takeaway

**Sort the side lengths, check the largest three first,
and move toward smaller sides until a valid triangle
is found.**

The main condition is:

    nums[i - 1] + nums[i - 2] > nums[i]

Return their sum when the condition is true.

## Filename

10-976-largest-perimeter-triangle.md
