# Greedy Pattern — Question 8
# LC 45 — Jump Game II

## 1. Problem Details

- **LeetCode Number:** 45
- **Pattern:** Greedy
- **Difficulty:** Medium
- **Topics:** Array, Greedy, Reachability

## 2. Problem Statement

We are given an integer array `nums`.

Each element `nums[i]` represents the maximum number of
steps we can jump forward from index `i`.

We start at index 0 and need to reach the last index.

Our task is to return the minimum number of jumps required
to reach the last index.

Important:
- We can jump fewer steps than `nums[i]`.
- We cannot jump backward.
- The problem guarantees that the last index is reachable.
- We want the minimum number of jumps, not just any path.

## 3. Example

Input:

    nums = [2, 3, 1, 1, 4]

Output:

    2

Explanation:

We can reach the destination in two jumps:

    Index 0 -> Index 1 -> Index 4

First jump:
- Start at index 0.
- Since nums[0] = 2, we can jump to index 1 or 2.
- We choose index 1 because it provides a better next reach.

Second jump:
- At index 1, nums[1] = 3.
- We can reach index 4 directly.

Therefore, the minimum number of jumps is 2.

## 4. Difference Between LC 55 and LC 45

### LC 55 — Jump Game

We check whether reaching the last index is possible.

Return type:

    true or false

### LC 45 — Jump Game II

We calculate the minimum number of jumps needed to reach
the last index.

Return type:

    Integer

Example:

    nums = [2, 3, 1, 1, 4]

LC 55 output: true
LC 45 output: 2

## 5. Greedy Concept

Greedy means making a suitable choice at each step to
obtain the best overall result.

In this problem, we divide the reachable indexes into
ranges.

We explore all indexes within the current range and find
the farthest index reachable from any of them.

When we reach the end of the current range, we count
another jump and extend the range to the farthest index.

The main idea is:

**Explore the current range completely, find the farthest
reach, and then count the next jump.**

This allows us to find the minimum number of jumps without
trying every possible path.

## 6. Three Important Variables

We use three variables:

    jumps
    currentEnd
    farthest

### A. jumps

Stores the number of jumps counted so far.

Initially:

    jumps = 0

### B. currentEnd

Represents the last index of the current jump range.

Initially:

    currentEnd = 0

When we reach this boundary, we have finished exploring
the current range.

We then count another jump and extend the boundary.

### C. farthest

Stores the farthest index we can reach using the indexes
processed so far.

Initially:

    farthest = 0

For every index, we update it using:

    farthest = max(farthest, i + nums[i])

Here:
- `i` is the current index.
- `nums[i]` is the maximum jump length from that index.
- `i + nums[i]` is the farthest index reachable from it.

We keep the maximum of the old and new reach.

## 7. Approach — Step by Step

### Step 1: Initialize variables

    int jumps = 0;
    int currentEnd = 0;
    int farthest = 0;

Initially, we are at index 0 and have not counted any jumps.

### Step 2: Traverse the array

    for (int i = 0; i < nums.size() - 1; i++)

We start from index 0 and stop before the last index.

We do not need to process the destination because reaching
its range already tells us the minimum jump count.

### Step 3: Find the farthest reach

    farthest = max(farthest, i + nums[i]);

At each index, calculate how far we could reach from
that index.

Update `farthest` if the new reach is greater.

### Step 4: Check the current range boundary

    if (i == currentEnd)

When the current index reaches the end of the current
range, we have finished exploring that range.

We must count another jump to move into the next range.

### Step 5: Count the jump and extend the boundary

    jumps++;
    currentEnd = farthest;

- Increase `jumps` by one.
- Set `currentEnd` to the farthest reachable index.

Now the next range is established.

### Step 6: Return the answer

    return jumps;

After the loop, return the minimum jump count.

## 8. Beginner-Friendly C++ Code

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int jumps = 0;
        int currentEnd = 0;
        int farthest = 0;

        for (int i = 0; i < nums.size() - 1; i++) {
            farthest = max(farthest, i + nums[i]);

            if (i == currentEnd) {
                jumps++;
                currentEnd = farthest;
            }
        }

        return jumps;
    }
};
```

## 9. Code Explanation

### A. Initialize the variables

```cpp
int jumps = 0;
int currentEnd = 0;
int farthest = 0;
```

Initially:
- No jumps have been counted.
- The current range ends at index 0.
- The farthest reachable index is 0.

### B. Loop condition

```cpp
for (int i = 0; i < nums.size() - 1; i++)
```

The loop stops before the last index.

For an array of size 5, the loop processes:

    i = 0, 1, 2, 3

It does not process index 4.

This avoids counting an unnecessary jump after reaching
the destination range.

### C. Update farthest

```cpp
farthest = max(farthest, i + nums[i]);
```

Suppose:

    i = 1
    nums[i] = 3
    farthest = 2

The new possible reach is:

    i + nums[i] = 1 + 3 = 4

Therefore:

    farthest = max(2, 4)
    farthest = 4

This means index 4 is reachable from index 1.

### D. Check the range boundary

```cpp
if (i == currentEnd)
```

This checks whether we have reached the end of the
current range.

Suppose:

    i = 2
    currentEnd = 2

Then:

    i == currentEnd

is true.

This means we have processed all indexes in the current
range. It is time to count the next jump.

### E. Count the jump

```cpp
jumps++;
```

We increase the jump count by one because the current
range has ended and we need another jump to progress.

### F. Update the current boundary

```cpp
currentEnd = farthest;
```

The next range ends at the farthest index we discovered
while exploring the current range.

For example:

    currentEnd = 2
    farthest = 4

After updating:

    currentEnd = 4

We have established that the destination can be reached
within the next jump range.

### G. Return the number of jumps

```cpp
return jumps;
```

The greedy range-expansion process counts the minimum
jumps required to reach the last index.

## 10. Detailed Dry Run

Input:

    nums = [2, 3, 1, 1, 4]

Initial values:

    jumps = 0
    currentEnd = 0
    farthest = 0

### Step 1: i = 0

Current value:

    nums[0] = 2

Calculate the farthest reach:

    farthest = max(0, 0 + 2)
    farthest = 2

Now:

    jumps = 0
    currentEnd = 0
    farthest = 2

Check:

    i == currentEnd
    0 == 0

True.

Execute:

    jumps++;
    currentEnd = farthest;

Updated values:

    jumps = 1
    currentEnd = 2
    farthest = 2

Meaning:

With one jump, we can reach index 1 or index 2.
The current range ends at index 2.

### Step 2: i = 1

Current value:

    nums[1] = 3

Calculate:

    farthest = max(2, 1 + 3)
    farthest = 4

Updated state before the condition:

    jumps = 1
    currentEnd = 2
    farthest = 4

Check:

    i == currentEnd
    1 == 2

False.

Therefore, we do not increase `jumps` yet.

Why?

Because the current range ends at index 2. We still
need to process index 2 before counting the next jump.

Current state:

    jumps = 1
    currentEnd = 2
    farthest = 4

### Step 3: i = 2

Current value:

    nums[2] = 1

Calculate:

    farthest = max(4, 2 + 1)
    farthest = 4

Current state:

    jumps = 1
    currentEnd = 2
    farthest = 4

Check:

    i == currentEnd
    2 == 2

True.

We have now processed the entire current range:
indexes 0, 1, and 2.

The farthest reachable index discovered within this
range is index 4.

Execute:

    jumps++;
    currentEnd = farthest;

Updated values:

    jumps = 2
    currentEnd = 4
    farthest = 4

Meaning:

The minimum jump count has increased to 2, and the
destination is within the new boundary.

### Step 4: i = 3

Current value:

    nums[3] = 1

Calculate:

    farthest = max(4, 3 + 1)
    farthest = 4

Check:

    i == currentEnd
    3 == 4

False.

The jump count remains unchanged.

Current state:

    jumps = 2
    currentEnd = 4
    farthest = 4

The loop ends because index 4 is the last index and
the loop stops before processing it.

Return:

    jumps

Final output:

    2

## 11. Dry Run Table

Input:

    nums = [2, 3, 1, 1, 4]

| i | nums[i] | i + nums[i] | farthest | currentEnd After Condition | jumps |
|---:|---:|---:|---:|---:|---:|
| Start | - | - | 0 | 0 | 0 |
| 0 | 2 | 2 | 2 | 2 | 1 |
| 1 | 3 | 4 | 4 | 2 | 1 |
| 2 | 1 | 3 | 4 | 4 | 2 |
| 3 | 1 | 4 | 4 | 4 | 2 |

Final output:

    2

One optimal path is:

    Index 0 -> Index 1 -> Index 4

Number of jumps:

    2

## 12. Another Example

Input:

    nums = [2, 3, 0, 1, 4]

Initial values:

    jumps = 0
    currentEnd = 0
    farthest = 0

At index 0:

    farthest = max(0, 0 + 2)
    farthest = 2

Since `i == currentEnd`:

    jumps = 1
    currentEnd = 2

At index 1:

    farthest = max(2, 1 + 3)
    farthest = 4

Since index 1 is not the current boundary, the jump
count remains 1.

At index 2:

    farthest = max(4, 2 + 0)
    farthest = 4

Now `i == currentEnd`, because both equal 2.

Update:

    jumps = 2
    currentEnd = 4

At index 3, the farthest reach remains 4. No additional
jump is counted because index 3 is not the boundary.

The loop finishes and returns:

    2

Output:

    2

## 13. Why Does the Greedy Approach Work?

We do not need to decide the exact landing index of
every jump in advance.

Instead, we consider all indexes reachable within the
current number of jumps.

We calculate the farthest index reachable from this
range.

When the current range ends, we know that any valid
continuation must use another jump.

Choosing the farthest reachable boundary ensures that
the next range includes every position we can reach with
one additional jump.

This is why the range-expansion method finds the
minimum number of jumps.

## 14. Complexity Analysis

Let `n` be the size of the array.

### Time
