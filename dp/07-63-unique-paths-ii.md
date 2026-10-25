# #7 — LeetCode 63: Unique Paths II

## 1. Problem Statement

Ek robot ko `m x n` grid ke top-left cell `(0,0)` se bottom-right cell `(m-1,n-1)` tak jaana hai.

Robot sirf do directions mein move kar sakta hai:

- Right: ek column aage.
- Down: ek row neeche.

Grid mein kuch obstacles bhi hain.

- `0` = Empty cell, jahan se move kar sakte hain.
- `1` = Obstacle, jahan se move nahi kar sakte.

Humein destination tak pahunchne ke total unique paths find karne hain.

### Example

Input:

```cpp
obstacleGrid = {
    {0, 0, 0},
    {0, 1, 0},
    {0, 0, 0}
};
```

Output:

```text
2
```

Explanation:

Beech wale cell `(1,1)` par obstacle hai. Robot us cell se nahi ja sakta.

Destination tak sirf 2 valid paths hain:

Path 1:
Right → Right → Down → Down

Path 2:
Down → Down → Right → Right

---

## 2. Approach — Recursion + Memoization

### Main Idea

LC 62 — Unique Paths ka logic hi use hoga, lekin ab obstacle wale cells ko handle karna hai.

Har cell `(r,c)` tak pahunchne ke total ways calculate karenge.

Kisi cell tak hum sirf do directions se aa sakte hain:

1. Upar wale cell `(r-1,c)` se.
2. Left wale cell `(r,c-1)` se.

Agar current cell obstacle nahi hai, toh:

```text
ways(r,c) = ways(r-1,c) + ways(r,c-1)
```

Agar current cell obstacle hai, toh us cell tak pahunchne ke valid ways `0` honge.

### Base Cases

**Case 1: Grid ke bahar**

```cpp
if (r < 0 || c < 0) {
    return 0;
}
```

Agar row ya column negative ho gaya, toh hum grid ke bahar hain. Isliye valid paths `0` hain.

**Case 2: Obstacle**

```cpp
if (grid[r][c] == 1) {
    return 0;
}
```

Obstacle wale cell se nahi ja sakte. Isliye `0` return hoga.

**Case 3: Starting Cell**

```cpp
if (r == 0 && c == 0) {
    return 1;
}
```

Starting cell tak pahunchne ka ek hi way hai, isliye `1` return karenge.

Obstacle check starting-cell check se pehle karna zaroori hai. Agar starting cell par hi obstacle hai, toh answer `0` hona chahiye.

**Case 4: Answer Already Calculated**

```cpp
if (dp[r][c] != -1) {
    return dp[r][c];
}
```

Agar cell ka answer pehle se calculated hai, toh wahi answer return kar denge.

### DP Transition

```cpp
int up = solve(r - 1, c, grid, dp);
int left = solve(r, c - 1, grid, dp);

dp[r][c] = up + left;
```

Upar se aane wale paths aur left se aane wale paths add karke current cell ka answer calculate karenge.

---

## 3. C++ Code — Memoization

```cpp
class Solution {
public:
    int solve(int r, int c, vector<vector<int>>& grid,
              vector<vector<int>>& dp) {

        // Grid ke bahar hain
        if (r < 0 || c < 0) {
            return 0;
        }

        // Obstacle hai, is cell se nahi ja sakte
        if (grid[r][c] == 1) {
            return 0;
        }

        // Starting cell
        if (r == 0 && c == 0) {
            return 1;
        }

        // Answer pehle se calculate hai
        if (dp[r][c] != -1) {
            return dp[r][c];
        }

        // Upar aur left se aane wale paths
        int up = solve(r - 1, c, grid, dp);
        int left = solve(r, c - 1, grid, dp);

        // Dono directions ke paths add karo
        dp[r][c] = up + left;

        return dp[r][c];
    }

    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();

        vector<vector<int>> dp(m, vector<int>(n, -1));

        // Destination cell se start karo
        return solve(m - 1, n - 1, obstacleGrid, dp);
    }
};
```

---

## 4. Code Explanation

### Function 1: uniquePathsWithObstacles()

```cpp
int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
    int m = obstacleGrid.size();
    int n = obstacleGrid[0].size();

    vector<vector<int>> dp(m, vector<int>(n, -1));

    return solve(m - 1, n - 1, obstacleGrid, dp);
}
```

- `m` grid ki total rows hain.
- `n` grid ke total columns hain.
- `dp` ek 2D array hai jisme initially sab values `-1` hain.
- `-1` ka matlab hai ki cell ka answer abhi calculate nahi hua.
- `solve(m-1, n-1, ...)` destination cell se recursion start karta hai.

### Function 2: solve()

```cpp
int solve(int r, int c, vector<vector<int>>& grid,
          vector<vector<int>>& dp)
```

Ye function current cell `(r,c)` tak pahunchne ke total valid paths return karta hai.

### Up Call

```cpp
int up = solve(r - 1, c, grid, dp);
```

Current cell ke upar wale cell tak pahunchne ke total ways calculate karta hai.

### Left Call

```cpp
int left = solve(r, c - 1, grid, dp);
```

Current cell ke left wale cell tak pahunchne ke total ways calculate karta hai.

### DP Transition

```cpp
dp[r][c] = up + left;
```

Dono directions ke valid paths add karke current cell ka answer save karta hai.

### Return

```cpp
return dp[r][c];
```

Calculated answer calling function ko return ho jaata hai.

---

## 5. Detailed Dry Run

Input:

```cpp
obstacleGrid = {
    {0, 0, 0},
    {0, 1, 0},
    {0, 0, 0}
};
```

Grid:

```text
         Column 0   Column 1   Column 2

Row 0       0          0          0
Row 1       0          1          0
Row 2       0          0          0
```

Destination `(2,2)` hai.

### Step 1: Destination se Start

```cpp
solve(2, 2, grid, dp);
```

Current cell `(2,2)` empty hai.

Pehle upar wale cell ka answer calculate hoga:

```cpp
up = solve(1, 2, grid, dp);
```

### Step 2: solve(1,2)

Cell `(1,2)` empty hai.

Iske upar aur left ke answers calculate karenge:

```cpp
up = solve(0, 2, grid, dp);
left = solve(1, 1, grid, dp);
```

Pehle `solve(0,2)` execute hoga.

### Step 3: solve(0,2)

Top row mein hain.

- `solve(-1,2)` → Grid ke bahar, answer `0`.
- `solve(0,1)` → Iska answer `1` hoga.

Isliye:

```text
dp[0][2] = 0 + 1
         = 1
```

Ab `solve(0,2)` return `1` karega.

### Step 4: solve(1,1) — Obstacle

Ab left call execute hogi:

```cpp
left = solve(1, 1, grid, dp);
```

Grid mein:

```text
grid[1][1] = 1
```

Isliye ye condition true hogi:

```cpp
if (grid[r][c] == 1) {
    return 0;
}
```

Obstacle se nahi ja sakte, toh answer `0` return hoga.

Ab `solve(1,2)` ke paas:

```text
up = 1
left = 0

dp[1][2] = 1 + 0
         = 1
```

### Step 5: Wapas solve(2,2)

Ab destination ke upar wale cell ka answer mil gaya:

```text
up = 1
```

Ab destination ke left wale cell ko calculate karenge:

```cpp
left = solve(2, 1, grid, dp);
```

### Step 6: solve(2,1)

Cell `(2,1)` empty hai.

```cpp
up = solve(1, 1, grid, dp);
left = solve(2, 0, grid, dp);
```

- `solve(1,1)` → Obstacle hai, answer `0`.
- `solve(2,0)` → Answer `1`.

Isliye:

```text
dp[2][1] = 0 + 1
         = 1
```

### Step 7: Final Answer

Ab destination `(2,2)` par wapas aate hain:

```text
up   = 1
left = 1

dp[2][2] = 1 + 1
         = 2
```

Final DP matrix:

```text
[
    [1, 1, 1],
    [1, 0, 1],
    [1, 1, 2]
]
```

**Final Answer = 2**

Dhyan do:

- Obstacle cell `(1,1)` ka answer `0` hai.
- Destination `(2,2)` ka answer `2` hai.
- Har cell ka number batata hai ki starting cell se wahan tak kitne valid paths hain.

---

## 6. Memoization ka Benefit

```cpp
if (dp[r][c] != -1) {
    return dp[r][c];
}
```

Agar kisi cell ka answer pehle se calculate ho chuka hai, toh usko dobara calculate nahi karna padta.

Calculated answer yahan save hota hai:

```cpp
dp[r][c] = up + left;
```

Is technique ko **Memoization (Top-Down DP)** kehte hain.

- Recursion subproblems solve karta hai.
- DP array answers ko save karta hai.
- Saved answers reuse hone se repeated calculations kam hoti hain.

---

## 7. Time and Space Complexity

Let:
- `m` = number of rows
- `n` = number of columns

**Time Complexity: O(m × n)**

Har cell ka answer maximum ek baar calculate hota hai. Cached cells ke liye saved answer return hota hai.

**DP Space Complexity: O(m × n)**

DP matrix mein `m × n` values store hoti hain.

**Recursion Stack: O(m + n)**

Recursion ki maximum depth row aur column ko reduce karte hue hoti hai.

Total auxiliary space: **O(m × n)**

---

## 8. Interview Explanation

"Main is problem ko recursion with memoization se solve karunga. Har cell ke liye upar aur left se aane wale valid paths calculate karunga. Agar cell obstacle hai, toh zero return karunga. Starting cell par one return hoga aur grid ke bahar jaane par zero return hoga. Repeated calculations avoid karne ke liye har cell ka result 2D DP array mein store karunga. Isse time complexity O(m × n) ho jaati hai."

---

## 9. Important Points to Remember

- `0` = Empty cell.
- `1` = Obstacle.
- Obstacle wale cell se valid paths `0` hain.
- Starting cell `(0,0)` returns `1`, provided it is not an obstacle.
- Invalid coordinates return `0`.
- `up = solve(r-1,c,grid,dp)`.
- `left = solve(r,c-1,grid,dp)`.
- Current cell ka answer = `up + left`.
- Destination = `(m-1,n-1)`.
- Approach = Recursion + Memoization (Top-Down DP).
- Time Complexity = `O(m × n)`.

---

## 10. Pattern Progress

### Pattern 2 — Grid DP

- #6 — LC 62: Unique Paths ✅
- #7 — LC 63: Unique Paths II ✅
- #8 — LC 64: Minimum Path Sum
- #9 — LC 120: Triangle
- #10 — LC 931: Minimum Falling Path Sum

Next question: **#8 — LeetCode 64: Minimum Path Sum**.
