# Greedy Pattern — Question 3
# LC 605 — Can Place Flowers

## 1. Problem Details

- **LeetCode Number:** 605
- **Pattern:** Greedy
- **Difficulty:** Easy
- **Topics:** Greedy, Array

## 2. Problem Statement

We are given an integer array `flowerbed`, where:

- `0` means the position is empty.
- `1` means a flower is already planted.

We are also given an integer `n`, representing the number of
new flowers we want to plant.

We cannot plant flowers in adjacent positions.

Our task is to determine whether we can plant all `n` flowers
without violating the rule.

Return `true` if possible; otherwise, return `false`.

## 3. Example

Input:

    flowerbed = [1, 0, 0, 0, 1]
    n = 1

Output:

    true

Explanation:

The first and last positions already contain flowers.

We can plant a new flower in the middle position:

    Before: [1, 0, 0, 0, 1]
    After:  [1, 0, 1, 0, 1]

No two flowers are adjacent.

Therefore, we can plant one flower and return `true`.

## 4. Greedy Concept

Greedy means making a valid choice at each step to reach
the overall goal.

In this problem, we scan the flowerbed from left to right.

Whenever we find a position where planting a flower is valid,
we plant it immediately.

Why?

Planting at a valid position does not prevent us from achieving
the maximum possible number of flowers in this problem.

The main idea is:

**If the current position is empty and both neighbours are
empty or do not exist, plant a flower there.**

## 5. Approach — Step by Step

### Step 1: Initialize variables

    int count = 0;
    int size = flowerbed.size();

- `count` stores the number of flowers planted by us.
- `size` stores the length of the array.

If `n == 0`, we do not need to plant any flowers, so return
`true` immediately.

### Step 2: Traverse the array

Use a loop to visit every index from `0` to `size - 1`.

### Step 3: Check whether the current position is empty

    if (flowerbed[i] == 0)

We can only plant a flower if the current position is empty.

If the current position already contains a flower (`1`),
skip that position.

### Step 4: Handle the first position

When `i == 0`, there is no left neighbour.

We only need to check the right neighbour.

If the array contains only one position, we can plant there
if it is empty.

Condition:

    size == 1 || flowerbed[i + 1] == 0

If valid:
- Mark the position as `1`.
- Increase `count`.

### Step 5: Handle the last position

When `i == size - 1`, there is no right neighbour.

We only need to check the left neighbour.

Condition:

    flowerbed[i - 1] == 0

If valid:
- Mark the position as `1`.
- Increase `count`.

### Step 6: Handle middle positions

For all other positions, both neighbours exist.

We can plant only when:

    flowerbed[i - 1] == 0 &&
    flowerbed[i + 1] == 0

If both conditions are true:
- Mark the current position as `1`.
- Increase `count`.

### Step 7: Check the required count

After processing a position, check:

    if (count >= n)

If we have planted enough flowers, return `true`.

### Step 8: Finish the traversal

If we finish the array without planting enough flowers,
return `false`.

## 6. C++ Code — Beginner-Friendly

```cpp
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        int count = 0;
        int size = flowerbed.size();

        if (n == 0) return true;

        for (int i = 0; i < size; i++) {

            // Current position must be empty
            if (flowerbed[i] == 0) {

                // First position
                if (i == 0) {
                    if (size == 1 || flowerbed[i + 1] == 0) {
                        flowerbed[i] = 1;
                        count++;
                    }
                }

                // Last position
                else if (i == size - 1) {
                    if (flowerbed[i - 1] == 0) {
                        flowerbed[i] = 1;
                        count++;
                    }
                }

                // Middle positions
                else {
                    if (flowerbed[i - 1] == 0 &&
                        flowerbed[i + 1] == 0) {
                        flowerbed[i] = 1;
                        count++;
                    }
                }
            }

            if (count >= n) {
                return true;
            }
        }

        return false;
    }
};
```

## 7. Code Explanation

### A. Initialize variables

```cpp
int count = 0;
int size = flowerbed.size();
```

`count` keeps track of the new flowers we have planted.

`size` stores the total number of positions.

### B. Handle n = 0

```cpp
if (n == 0) return true;
```

If we need to plant zero flowers, the task is already complete.

### C. Traverse the array

```cpp
for (int i = 0; i < size; i++)
```

We check every position from left to right.

### D. Check whether the current position is empty

```cpp
if (flowerbed[i] == 0)
```

We must check this condition first.

If the current position already contains a flower, we cannot
plant another flower there.

Without this condition, the code might count a flower at a
position where a flower already exists.

### E. Check the first position

```cpp
if (i == 0) {
    if (size == 1 || flowerbed[i + 1] == 0) {
        flowerbed[i] = 1;
        count++;
    }
}
```

The first position has no left neighbour.

If the array has only one position, `size == 1` is true.
Otherwise, the right neighbour must be empty.

The `||` operator means OR.

It also uses short-circuit evaluation: if `size == 1` is
true, the right side does not need to be evaluated.

### F. Check the last position

```cpp
else if (i == size - 1) {
    if (flowerbed[i - 1] == 0) {
        flowerbed[i] = 1;
        count++;
    }
}
```

The last position has no right neighbour.

We only check the left neighbour.

### G. Check middle positions

```cpp
else {
    if (flowerbed[i - 1] == 0 &&
        flowerbed[i + 1] == 0) {
        flowerbed[i] = 1;
        count++;
    }
}
```

A middle position has both left and right neighbours.

Both must be empty before we plant a flower.

The `&&` operator means AND, so both conditions must be true.

### H. Mark the position and increase count

```cpp
flowerbed[i] = 1;
count++;
```

We mark the position as occupied immediately.

This is important because the next position must consider
the newly planted flower.

### I. Check the answer

```cpp
if (count >= n) {
    return true;
}
```

If we have planted enough flowers, return `true` immediately.

### J. Return false

```cpp
return false;
```

If the loop finishes and we still have not planted enough
flowers, return `false`.

## 8. Detailed Dry Run

Input:

    flowerbed = [1, 0, 0, 0, 1]
    n = 1

Initially:

    count = 0
    size = 5

### Step 1: i = 0

Current array:

    [1, 0, 0, 0, 1]

Current value:

    flowerbed[0] = 1

The current position already contains a flower.

The condition `flowerbed[i] == 0` is false.

We skip this position.

    count = 0

### Step 2: i = 1

Current value:

    flowerbed[1] = 0

The position is empty.

It is not the first or last position, so we check both
neighbours.

Left neighbour:

    flowerbed[0] = 1

Right neighbour:

    flowerbed[2] = 0

The left neighbour contains a flower, so we cannot plant here.

    count = 0

Array remains:

    [1, 0, 0, 0, 1]

### Step 3: i = 2

Current value:

    flowerbed[2] = 0

The position is empty and is a middle position.

Check the left neighbour:

    flowerbed[1] = 0

Check the right neighbour:

    flowerbed[3] = 0

Both neighbours are empty.

Therefore, we can plant a flower.

Update:

    flowerbed[2] = 1
    count = 1

New array:

    [1, 0, 1, 0, 1]

Now check:

    count >= n
    1 >= 1

This is true, so the function returns `true`.

Final output:

    true

## 9. Another Example

Input:

    flowerbed = [1, 0, 0, 0, 1]
    n = 2

We can plant a flower at index `2`.

The array becomes:

    [1, 0, 1, 0, 1]

Now:

    count = 1

But we need two new flowers.

The remaining empty positions cannot be used because each
has a neighbouring flower.

The loop finishes with:

    count = 1

Since:

    1 < 2

The function returns:

    false

## 10. Complexity Analysis

Let `N` be the length of the flowerbed array.

### Time Complexity: O(N)

We traverse the array from left to right.

At each position, we perform a constant number of checks.

Therefore, the time complexity is:

    O(N)

### Extra Space Complexity: O(1)

We only use two integer variables:

    count
    size

Therefore, the extra space complexity is:

    O(1)

Note: The code modifies the input array itself.

## 11. Important Interview Points

1. The current position must be `0`.
2. The first position has no left neighbour.
3. The last position has no right neighbour.
4. Middle positions require both neighbours to be empty.
5. Mark a newly planted flower as `1` immediately.
6. Increment `count` only when a new flower is actually planted.
7. If `n == 0`, return `true`.
8. Return `true` as soon as `
