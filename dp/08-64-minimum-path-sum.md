# #8 — LeetCode 64: Minimum Path Sum

## 1. Problem Statement

Ek `m x n` grid diya gaya hai. Har cell mein ek integer value hai.

Humein top-left cell `(0,0)` se bottom-right cell `(m-1,n-1)` tak jaana hai.

Robot sirf do directions mein move kar sakta hai:

- Right: ek column aage.
- Down: ek row neeche.

Humein aisa path find karna hai jiska total sum minimum ho.

Starting cell aur destination cell, dono ki values sum mein include hongi.

### Example

Input:

```cpp
grid = {
    {1, 3, 1},
    {1, 5, 1},
    {4, 2, 1}
};
```

Output:

```text
7
```

Explanation:

Minimum-sum path:

```text
1 → 3 → 1 → 1 → 1
```

Total sum:

```text
1 + 3 + 1 + 1 + 1 = 7
```

Isliye final answer `7` hai.

---

## 2. Approach — Recursion + Memoization

### Main Idea

Har cell `(r,c)` tak pahunchne ka minimum path sum calculate karenge.

Kisi cell par hum sirf do directions se aa sakte hain:

1. Upar wale cell `(r-1,c)` se.
2. Left wale cell `(r,c-1)` se.

Dono directions ka minimum sum nikaalenge aur current cell ki value add karenge.

### DP Transition

```cpp
dp[r][c] = grid[r][c] + min(up, left);
```

Yahan:

- `up` = upar wale cell se aane ka minimum sum.
- `left` = left wale cell se aane ka minimum sum.
- `grid[r][c]` = current cell ki value.

### Base Cases

#### Case 1: Grid ke bahar

```cpp
if (r < 0 || c < 0) {
    return INT_MAX;
}
```

Agar row ya column negative ho gaya, toh hum grid ke bahar hain.

`INT_MAX` C++ mein maximum representable integer value hai. Is problem mein ise infinity jaisi badi value maante hain, taaki invalid direction minimum select na ho.

Example:

```text
up   = INT_MAX
left = 4

min(INT_MAX, 4) = 4
```

Isliye valid direction select hogi.

**Important:** Minimum path sum mein invalid direction ke liye `0` return nahi kar sakte, kyunki `min(0, 4) = 0` galat result dega.

`INT_MAX` ko current cell ki value ke saath tabhi add karne ka risk hota hai jab dono directions invalid hon. Valid rectangular grid mein har cell ke liye starting cell se ek valid path hota hai, isliye normal problem constraints mein minimum valid direction se aata hai.

#### Case 2: Starting Cell

```cpp
if (r == 0 && c == 0) {
    return grid[0][0];
}
```

Starting cell ka minimum sum uski apni value hogi.

Agar `grid[0][0] = 1`, toh answer `1` return hoga.

#### Case 3: Answer Already Calculated

```cpp
if (dp[r][c] != -1) {
    return dp[r][c];
}
```

Agar cell ka answer pehle se calculate ho chuka hai, toh saved answer return kar denge.

Isse same calculation dobara nahi karni padegi.

### Recursion Flow

1. Destination cell se recursion start karo.
2. Upar wale cell ka minimum sum calculate karo.
3. Left wale cell ka minimum sum calculate karo.
4. Dono mein se minimum choose karo.
5. Current cell ki value add karo.
6. Answer ko `dp[r][c]` mein save karo.

---

## 3. C++ Code — Memoization

```cpp
class Solution {
public:
    int solve(int r, int c,
              vector<vector<int>>& grid,
              vector<vector<int>>& dp) {

        // Grid ke bahar hain
        if (r < 0 || c < 0) {
            return INT_MAX;
        }

        // Starting cell
        if (r == 0 && c == 0) {
            return grid[0][0];
        }

        // Answer pehle calculate ho chuka hai
        if (dp[r][c] != -1) {
            return dp[r][c];
        }

        // Upar se aane wala minimum sum
        int up = solve(r - 1, c, grid, dp);

        // Left se aane wala minimum sum
        int left = solve(r, c - 1, grid, dp);

        // Minimum sum calculate karke save karo
        dp[r][c] = grid[r][c] + min(up, left);

        return dp[r][c];
    }

    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();

        vector<vector<int>> dp(m, vector<int>(n, -1));

        // Destination cell se start karo
        return solve(m - 1, n - 1, grid, dp);
    }
};
```

---

## 4. Code Explanation

### Function 1: minPathSum()

```cpp
int minPathSum(vector<vector<int>>& grid) {
    int m = grid.size();
    int n = grid[0].size();

    vector<vector<int>> dp(m, vector<int>(n, -1));

    return solve(m - 1, n - 1, grid, dp);
}
```

- `m` = total rows.
- `n` = total columns.
- `dp` = answers save karne ke liye 2D array.
- Initially sab values `-1` hain.
- `solve(m-1, n-1, ...)` destination cell se calculation start karta hai.

### Function 2: solve()

```cpp
int solve(int r, int c,
          vector<vector<int>>& grid,
          vector<vector<int>>& dp)
```

Is function ka kaam hai starting cell se current cell `(r,c)` tak aane ka minimum path sum return karna.

### Up Call

```cpp
int up = solve(r - 1, c, grid, dp);
```

Current cell ke upar wale cell ka minimum path sum calculate karta hai.

### Left Call

```cpp
int left = solve(r, c - 1, grid, dp);
```

Current cell ke left wale cell ka minimum path sum calculate karta hai.

### Minimum Choose Karna

```cpp
dp[r][c] = grid[r][c] + min(up, left);
```

Dono directions ke sums compare hote hain. Jo chhota hai, usmein current cell ki value add hoti hai.

### Return

```cpp
return dp[r][c];
```

Calculated minimum sum calling function ko return ho jaata hai.

---

## 5. Detailed Dry Run

Input:

```cpp
grid = {
    {1, 3, 1},
    {1, 5, 1},
    {4, 2, 1}
};
```

Destination cell `(2,2)` hai.

### Starting DP Matrix

```text
dp = [
    [-1, -1, -1],
    [-1, -1, -1],
    [-1, -1, -1]
]
```

`-1` ka matlab hai ki answer abhi calculate nahi hua.

### Step 1: Destination Se Start

```cpp
solve(2, 2, grid, dp);
```

Is cell ki value `1` hai.

Do calls hongi:

```cpp
up = solve(1, 2, grid, dp);
left = solve(2, 1, grid, dp);
```

Code mein `up` pehle likha hai, isliye pehle `solve(1,2)` complete hoga.

### Step 2: solve(1,2)

Current cell ki value `1` hai.

```cpp
up = solve(0, 2, grid, dp);
left = solve(1, 1, grid, dp);
```

Pehle `solve(0,2)` execute hoga.

### Step 3: solve(0,2)

Current cell ki value `1` hai.

```cpp
up = solve(-1, 2, grid, dp);
left = solve(0, 1, grid, dp);
```

`solve(-1,2)` grid ke bahar hai, toh `INT_MAX` return hoga.

Ab `solve(0,1)` calculate hoga.

#### solve(0,1)

Current cell ki value `3` hai.

- Up direction grid ke bahar hai, toh `INT_MAX`.
- Left direction `solve(0,0)` hai.

Starting cell:

```cpp
solve(0, 0, grid, dp);
```

Return:

```cpp
grid[0][0] = 1;
```

Ab `solve(0,1)` ke liye:

```text
up   = INT_MAX
left = 1
```

Calculation:

```cpp
dp[0][1] = 3 + min(INT_MAX, 1);
          = 3 + 1;
          = 4;
```

Ab `solve(0,1)` return `4` karega.

Wapas `solve(0,2)` par:

```text
up   = INT_MAX
left = 4
```

Calculation:

```cpp
dp[0][2] = 1 + min(INT_MAX, 4);
          = 1 + 4;
          = 5;
```

Ab `solve(0,2)` return `5` karega.

### Step 4: Wapas solve(1,2)

Ab:

```text
up = 5
```

Doosri call:

```cpp
left = solve(1, 1, grid, dp);
```

Cell `(1,1)` ki value `5` hai.

```cpp
up = solve(0, 1, grid, dp);
left = solve(1, 0, grid, dp);
```

`solve(0,1)` ka answer pehle se `dp[0][1] = 4` mein saved hai. Isliye saved answer `4` return hoga.

Ab `solve(1,0)` calculate karenge.

#### solve(1,0)

- Up: `solve(0,0)` → `1`.
- Left: `solve(1,-1)` → `INT_MAX`.

Calculation:

```cpp
dp[1][0] = 1 + min(1, INT_MAX);
          = 1 + 1;
          = 2;
```

Ab `solve(1,0)` return `2` karega.

Wapas `solve(1,1)` par:

```text
up   = 4
left = 2
```

Calculation:

```cpp
dp[1][1] = 5 + min(4, 2);
          = 5 + 2;
          = 7;
```

Ab `solve(1,1)` return `7` karega.

Wapas `solve(1,2)` par:

```text
up   = 5
left = 7
```

Calculation:

```cpp
dp[1][2] = 1 + min(5, 7);
          = 1 + 5;
          = 6;
```

Ab `solve(1,2)` return `6` karega.

### Step 5: Destination Ke Left Wala Cell

Ab original call `solve(2,2)` mein:

```text
up = 6
```

Ab calculate hoga:

```cpp
left = solve(2, 1, grid, dp);
```

Cell `(2,1)` ki value `2` hai.

```cpp
up = solve(1, 1, grid, dp);
left = solve(2, 0, grid, dp);
```

`solve(1,1)` ka saved answer `7` hai.

Ab `solve(2,0)` calculate karenge.

#### solve(2,0)

- Up: `solve(1,0)` → saved answer `2`.
- Left: `solve(2,-1)` → `INT_MAX`.

Calculation:

```cpp
dp[2][0] = 4 + min(2, INT_MAX);
          = 4 + 2;
          = 6;
```

Ab `solve(2,0)` return `6` karega.

Wapas `solve(2,1)` par:

```text
up   = 7
left = 6
```

Calculation:

```cpp
dp[2][1] = 2 + min(7, 6);
          = 2 + 6;
          = 8;
```

Ab `solve(2,1)` return `8` karega.

### Step 6: Final Destination

Ab destination `(2,2)` par wapas aate hain.

```text
Current cell value = 1

up   = 6
left = 8
```

Calculation:

```cpp
dp[2][2] = 1 + min(6, 8);
          = 1 + 6;
          = 7;
```

Final Answer = **7**

### Final DP Matrix

```text
[
    [1, 4, 5],
    [2, 7, 6],
    [6, 8, 7]
]
```

Har DP cell mein starting point se us cell tak ka minimum path sum saved hai.

Destination `dp[2][2] = 7` hai, isliye final answer `7` hai.

---

## 6. INT_MAX Kyun Use Kiya?

```cpp
if (r < 0 || c < 0) {
    return INT_MAX;
}
```

`INT_MAX` integer ki maximum representable value hai. Isko yahan infinity jaisa treat karte hain.

Minimum choose karte waqt invalid direction select nahi hogi.

Example:

```text
up   = INT_MAX
left = 4

min(up, left) = 4
```

Agar invalid direction ke liye `0` return karte, toh:

```text
min(0, 4) = 0
```

Ye galat hota, kyunki valid path ka sum `4` hai.

### Difference Yaad Rakho

- LC 62 — Unique Paths: invalid direction ke `0` paths hote hain, toh `0` return karo.
- LC 64 — Minimum Path Sum: invalid direction ko minimum banne se rokna hai, toh `INT_MAX` return karo.

---

## 7. Memoization Ka Benefit

```cpp
if (dp[r][c] != -1) {
    return dp[r][c];
}
```

Agar kisi cell ka answer already calculate ho gaya hai, toh recursion usse dobara calculate nahi karta.

Answer yahan save hota hai:

```cpp
dp[r][c] = grid[r][c] + min(up, left);
```

Is technique ko **Memoization (Top-Down DP)** kehte hain.

- Recursion subproblems solve karta hai.
- DP array results save karta hai.
- Saved results reuse hone se repeated calculations avoid hoti hain.

---

## 8. Time and Space Complexity

Let:
- `m` = rows
- `n` = columns

### Time Complexity: O(m × n)

Har cell ka answer maximum ek baar calculate hota hai.

### DP Space: O(m × n)

DP matrix mein `m × n` values store hoti hain.

### Recursion Stack: O(m + n)

Recursive calls mein row aur column decrease hote hain. Maximum depth O(m+n) hoti hai.

Total auxiliary space: **O(m × n)**.

---

## 9. Interview Explanation

"Main is problem ko recursion with memoization se solve karunga. Har cell ke liye upper aur left cells se aane wale minimum path sums calculate karunga. Dono mein se minimum choose karke current cell ki value add karunga. Starting cell par uski value return hogi, aur invalid coordinates ke liye INT_MAX use karunga taaki invalid direction minimum select na ho. Repeated calculations avoid karne ke liye har cell ka answer 2D DP array mein save karunga. Time complexity O(m × n) hogi."

---

## 10. Important Points to Remember

- `solve(r,c)` current cell tak minimum path sum return karta hai.
- Starting cell returns `grid[0][0]`.
- Invalid coordinates return `INT_MAX`.
- `up = solve(r-1,c,grid,dp)`.
- `left = solve(r,c-1,grid,dp)`.
- Current cell ki value bhi total mein add hoti hai.
- Transition: `grid[r][c] + min(up,left)`.
- `dp[r][c]` calculated answer ko save karta hai.
- Destination: `(m-1,n-1)`.
- Approach: Recursion + Memoization.
- Time Complexity: `O(m × n)`.
- Total Auxiliary Space: `O(m × n)`.

---

## 11. Pattern Progress

### Pattern 2 — Grid DP

- #6 — LC 62: Unique Paths ✅
- #7 — LC 63: Unique Paths II ✅
- #8 — LC 64: Minimum Path Sum ✅
- #9 — LC 120: Triangle
- #10 — LC 931: Minimum Falling Path Sum

Next Question: **#9 — LeetCode 120: Triangle**.
