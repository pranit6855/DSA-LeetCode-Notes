
# 10. Minimum Falling Path Sum — LeetCode 931

## Problem

Given an `n x n` matrix, find the minimum falling path sum starting from any element in the first row and ending at any element in the last row.

From position `(i, j)`, we can move to the next row in three directions:

1. Down: `(i + 1, j)`
2. Diagonal Right: `(i + 1, j + 1)`
3. Diagonal Left: `(i + 1, j - 1)`

We need to return the minimum path sum.

### Example

Input:

```text
2   1   3
6   5   4
7   8   9
```

Output:

```text
13
```

Explanation:

One minimum path is:

```text
2 → 5 → 7 = 14
```

The actual minimum path is:

```text
1 → 4 → 8 = 13
```

Therefore, the answer is `13`.

---

## Approach: Recursion + Memoization (Top-Down DP)

We start from every possible column of the first row.

For each position `(i, j)`, we explore three possible movements:

1. Down
2. Diagonal Right
3. Diagonal Left

We calculate the minimum result among these three paths and add the current matrix element.

### Recurrence Relation

```text
solve(i, j) = matrix[i][j] +
              min(solve(i + 1, j),
                  solve(i + 1, j + 1),
                  solve(i + 1, j - 1))
```

### Base Cases

**1. Boundary Condition**

```cpp
if (j < 0 || j >= n) {
    return INT_MAX;
}
```

If the column goes outside the matrix, the path is invalid.

We return `INT_MAX` so that the invalid path is not selected by `min()`.

**2. Last Row**

```cpp
if (i == n - 1) {
    return matrix[i][j];
}
```

When we reach the last row, return the current element because no further movement is required.

### Memoization

We use two 2D arrays:

- `dp`: Stores the minimum falling path sum starting from each position.
- `visited`: Tracks whether a position has already been calculated.

```cpp
if (visited[i][j]) {
    return dp[i][j];
}
```

If a position has already been calculated, return its stored answer instead of repeating recursion.

We use a separate `visited` array so that we don't need to use a special number as an uncomputed marker.

---

## C++ Code

```cpp
class Solution {
public:
    int solve(int i, int j, vector<vector<int>>& matrix,
              vector<vector<int>>& dp,
              vector<vector<bool>>& visited) {

        int n = matrix.size();

        // Boundary condition
        if (j < 0 || j >= n) {
            return INT_MAX;
        }

        // Base case
        if (i == n - 1) {
            return matrix[i][j];
        }

        // Already calculated
        if (visited[i][j]) {
            return dp[i][j];
        }

        // Down
        int down = solve(i + 1, j, matrix, dp, visited);

        // Diagonal Right
        int right = solve(i + 1, j + 1, matrix, dp, visited);

        // Diagonal Left
        int left = solve(i + 1, j - 1, matrix, dp, visited);

        // Store answer
        visited[i][j] = true;

        return dp[i][j] = matrix[i][j] + min(down, min(right, left));
    }

    int minFallingPathSum(vector<vector<int>>& matrix) {
        int n = matrix.size();

        vector<vector<int>> dp(n, vector<int>(n, 0));
        vector<vector<bool>> visited(n, vector<bool>(n, false));

        int ans = INT_MAX;

        // Try every column of the first row
        for (int j = 0; j < n; j++) {
            ans = min(ans, solve(0, j, matrix, dp, visited));
        }

        return ans;
    }
};
```

---

## Code Explanation

### 1. Function Parameters

```cpp
int solve(int i, int j, vector<vector<int>>& matrix,
          vector<vector<int>>& dp,
          vector<vector<bool>>& visited)
```

- `i`: Current row.
- `j`: Current column.
- `matrix`: Original input matrix.
- `dp`: Stores calculated minimum path sums.
- `visited`: Tracks whether a position has already been calculated.

The function returns the minimum falling path sum starting from `(i, j)`.

### 2. Boundary Condition

```cpp
if (j < 0 || j >= n) {
    return INT_MAX;
}
```

Valid column indexes are from `0` to `n - 1`.

If we go outside these boundaries, return `INT_MAX`.

For example, if `n = 3`:

| Column | Valid / Invalid |
|---|---|
| -1 | Invalid |
| 0 | Valid |
| 1 | Valid |
| 2 | Valid |
| 3 | Invalid |

### 3. Base Case

```cpp
if (i == n - 1) {
    return matrix[i][j];
}
```

When we reach the last row, return the current element.

### 4. Memoization Check

```cpp
if (visited[i][j]) {
    return dp[i][j];
}
```

If the current position has already been calculated, return the stored answer.

This avoids repeated calculations.

### 5. Down Movement

```cpp
int down = solve(i + 1, j, matrix, dp, visited);
```

Move one row down while staying in the same column.

### 6. Diagonal Right Movement

```cpp
int right = solve(i + 1, j + 1, matrix, dp, visited);
```

Move one row down and one column to the right.

### 7. Diagonal Left Movement

```cpp
int left = solve(i + 1, j - 1, matrix, dp, visited);
```

Move one row down and one column to the left.

### 8. Calculate and Store Answer

```cpp
visited[i][j] = true;

return dp[i][j] = matrix[i][j] + min(down, min(right, left));
```

- Find the minimum among the three possible paths.
- Add the current matrix element.
- Store the answer in `dp`.
- Mark the position as visited.

### 9. Main Function

```cpp
int minFallingPathSum(vector<vector<int>>& matrix) {
    int n = matrix.size();

    vector<vector<int>> dp(n, vector<int>(n, 0));
    vector<vector<bool>> visited(n, vector<bool>(n, false));

    int ans = INT_MAX;

    for (int j = 0; j < n; j++) {
        ans = min(ans, solve(0, j, matrix, dp, visited));
    }

    return ans;
}
```

Important:

Unlike Triangle, here we can start from any column in the first row.

Therefore, we call:

```cpp
solve(0, j, matrix, dp, visited);
```

for every column `j`.

We keep updating `ans` with the minimum result.

---

## Dry Run

Input:

```text
2   1   3
6   5   4
7   8   9
```

We can start from any of the three columns in the first row.

### Starting from Column 0

Starting value = `2`

Possible next movements:

- Down → `6`
- Right → `5`
- Left → Invalid

Minimum path:

```text
2 → 5 → 7
```

Sum = `14`

### Starting from Column 1

Starting value = `1`

Possible next movements:

- Down → `5`
- Right → `4`
- Left → `6`

The best path is:

```text
1 → 4 → 8
```

Sum = `13`

### Starting from Column 2

Starting value = `3`

Possible next movements:

- Down → `4`
- Right → Invalid
- Left → `5`

The best path is:

```text
3 → 4 → 7
```

Sum = `14`

### Final Comparison

```text
Starting from column 0 = 14
Starting from column 1 = 13
Starting from column 2 = 14
```

Therefore:

```text
ans = min(14, 13, 14)

Final Answer = 13
```

---

## Complexity Analysis

Let `n` be the number of rows and columns.

- **Time Complexity:** `O(n²)` — Each cell is calculated at most once because of memoization.
- **DP Space Complexity:** `O(n²)` — We use two 2D arrays, `dp` and `visited`.
- **Recursion Stack:** `O(n)` — Maximum recursion depth is the number of rows.

---

## Triangle vs Minimum Falling Path Sum

| Feature | LC 120 — Triangle | LC 931 — Falling Path Sum |
|---|---|---|
| Starting position | Fixed `(0,0)` | Any first-row column |
| Movement | Down, Down-Right | Down, Down-Left, Down-Right |
| Input | Triangle | Square matrix |
| DP state | `(i,j)` | `(i,j)` |
| Approach | Memoization | Memoization |

## Key Takeaways

1. Both questions use 2D DP.
2. `solve(i,j)` represents the minimum path sum starting from the current position.
3. Memoization prevents repeated calculations.
4. Boundary conditions are important when moving diagonally.
5. Unlike Triangle, this problem allows three movements.
6. We must check every possible starting column in the first row.
7. `INT_MAX` is used to represent an invalid path.

**Pattern:** Grid / 2D DP — Minimum Path Sum

**Approach:** Recursion + Memoization (Top-Down DP)
