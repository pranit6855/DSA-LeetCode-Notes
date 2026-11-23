
# 09. Triangle — LeetCode 120

## Problem

Given a triangle array, return the minimum path sum from top to bottom.

From each position, we can move only in two directions:

1. Down: `(i + 1, j)`
2. Diagonal Down-Right: `(i + 1, j + 1)`

We have to find the minimum sum from the top to the bottom row.

### Example

```text
        2
      3   4
    6   5   7
  4   1   8   3
```

**Output:** `11`

**Explanation:**

Minimum path:

```text
        2
      3
    5
  1
```

Minimum sum = `2 + 3 + 5 + 1 = 11`

---

## Approach: Recursion + Memoization (Top-Down DP)

We start from the top position `(0, 0)` and try both possible movements.

At every position:

1. Calculate the minimum path sum by moving Down.
2. Calculate the minimum path sum by moving Diagonal.
3. Choose the minimum of both.
4. Add the current triangle element.
5. Store the calculated answer in `dp`.

### Recurrence Relation

```text
solve(i, j) = triangle[i][j] +
              min(solve(i + 1, j),
                  solve(i + 1, j + 1))
```

### Base Case

When we reach the last row, there is no further movement.

Therefore, return the current element:

```cpp
if (i == n - 1) {
    return triangle[i][j];
}
```

### Memoization

We use two 2D arrays:

- `dp`: Stores the minimum path sum for each position.
- `visited`: Tracks whether a position has already been calculated.

```cpp
if (visited[i][j]) {
    return dp[i][j];
}
```

If the answer is already calculated, return it directly instead of repeating recursion.

We use a separate `visited` array because a valid minimum path sum can also be `-1`. So using `-1` as an uncomputed marker can create confusion.

---

## C++ Code

```cpp
class Solution {
public:
    int solve(int i, int j, vector<vector<int>>& triangle,
              vector<vector<int>>& dp,
              vector<vector<bool>>& visited) {

        int n = triangle.size();

        // Base case
        if (i == n - 1) {
            return triangle[i][j];
        }

        // Already calculated
        if (visited[i][j]) {
            return dp[i][j];
        }

        // Down movement
        int down = solve(i + 1, j, triangle, dp, visited);

        // Diagonal movement
        int diagonal = solve(i + 1, j + 1, triangle, dp, visited);

        // Store answer
        visited[i][j] = true;

        return dp[i][j] = triangle[i][j] + min(down, diagonal);
    }

    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();

        vector<vector<int>> dp(n, vector<int>(n, 0));
        vector<vector<bool>> visited(n, vector<bool>(n, false));

        return solve(0, 0, triangle, dp, visited);
    }
};
```

---

## Code Explanation

### 1. Function Parameters

```cpp
int solve(int i, int j, vector<vector<int>>& triangle,
          vector<vector<int>>& dp,
          vector<vector<bool>>& visited)
```

- `i`: Current row.
- `j`: Current column.
- `triangle`: Original input triangle.
- `dp`: Stores calculated minimum path sums.
- `visited`: Tracks whether a position is already calculated.

The function returns the minimum path sum from `(i, j)` to the bottom row.

### 2. Base Case

```cpp
if (i == n - 1) {
    return triangle[i][j];
}
```

When we reach the last row, return the current element because no further movement is possible.

### 3. Memoization Check

```cpp
if (visited[i][j]) {
    return dp[i][j];
}
```

If the current position has already been calculated, return its stored answer.

This prevents repeated calculations.

### 4. Down Movement

```cpp
int down = solve(i + 1, j, triangle, dp, visited);
```

Move one row down while staying in the same column.

### 5. Diagonal Movement

```cpp
int diagonal = solve(i + 1, j + 1, triangle, dp, visited);
```

Move one row down and one column to the right.

### 6. Calculate and Store Answer

```cpp
visited[i][j] = true;

return dp[i][j] = triangle[i][j] + min(down, diagonal);
```

- Find the minimum between Down and Diagonal.
- Add the current element.
- Store the result in `dp`.
- Mark the position as visited.

### 7. Main Function

```cpp
int minimumTotal(vector<vector<int>>& triangle) {
    int n = triangle.size();

    vector<vector<int>> dp(n, vector<int>(n, 0));
    vector<vector<bool>> visited(n, vector<bool>(n, false));

    return solve(0, 0, triangle, dp, visited);
}
```

- Initialize `dp` with `0`.
- Initialize `visited` with `false`.
- Start recursion from the top position `(0, 0)`.

---

## Dry Run

Input:

```text
        2
      3   4
    6   5   7
  4   1   8   3
```

We start from `(0, 0)`.

### Step 1: Position (0, 0)

Current value = `2`

Two recursive calls:

```text
Down     = solve(1, 0)
Diagonal = solve(1, 1)
```

### Step 2: Position (1, 0)

Current value = `3`

```text
Down     = solve(2, 0)  -> 6
Diagonal = solve(2, 1)  -> 5
```

Minimum = `min(6, 5) = 5`

Answer:

```text
3 + 5 = 8
```

Store `dp[1][0] = 8`.

### Step 3: Position (1, 1)

Current value = `4`

```text
Down     = solve(2, 1)  -> 5
Diagonal = solve(2, 2)  -> 7
```

Minimum = `min(5, 7) = 5`

Answer:

```text
4 + 5 = 9
```

Store `dp[1][1] = 9`.

### Step 4: Back to Position (0, 0)

We have:

```text
Down     = 8
Diagonal = 9
```

Minimum = `min(8, 9) = 8`

Answer:

```text
2 + 8 = 10
```

Correction: The minimum path from the example is `2 → 3 → 5 → 1`, whose sum is `11`.

The recursive calculations above need to include the remaining path correctly:

- `solve(2, 0) = 6 + min(4, 1) = 7`
- `solve(2, 1) = 5 + min(1, 8) = 6`
- `solve(2, 2) = 7 + min(8, 3) = 10`

Therefore:

```text
solve(1, 0) = 3 + min(7, 6) = 9
solve(1, 1) = 4 + min(6, 10) = 10
solve(0, 0) = 2 + min(9, 10) = 11
```

**Final Answer: `11`**

---

## Complexity Analysis

Let `n` be the number of rows.

- **Time Complexity:** `O(n²)` — Each valid triangle position is calculated at most once.
- **DP Space Complexity:** `O(n²)` — We use two 2D arrays, `dp` and `visited`.
- **Recursion Stack:** `O(n)` — Maximum recursion depth is the number of rows.

---

## Key Takeaways

1. This problem uses 2D DP because each state is identified by `(i, j)`.
2. From each position, we can move Down or Diagonal Down-Right.
3. We use recursion to explore both possible paths.
4. We use memoization to avoid recalculating the same position.
5. `dp[i][j]` stores the minimum path sum starting from position `(i, j)`.
6. `visited[i][j]` tells us whether that state has already been calculated.
7. We start from `(0, 0)` and calculate the minimum path to the bottom.

**Pattern:** Grid / 2D DP — Minimum Path Sum

**Approach:** Recursion + Memoization (Top-Down DP)
