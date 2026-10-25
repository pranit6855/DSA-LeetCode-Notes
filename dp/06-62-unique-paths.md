# #6 — LeetCode 62: Unique Paths

## 1. Problem Statement

Ek robot ko `m x n` grid ke top-left cell `(0,0)` se bottom-right cell `(m-1,n-1)` tak pahunchna hai.

Robot sirf do directions mein move kar sakta hai:
- Right: ek column aage.
- Down: ek row neeche.

Humein total number of unique paths find karne hain.

### Example

Input:
m = 3, n = 3

Output:
6

Explanation:
Robot destination tak 6 different paths se pahunch sakta hai.

---

## 2. Approach — Recursion + Memoization

### Main Idea

Har cell tak pahunchne ke total ways calculate karenge.

Kisi bhi cell `(r,c)` par hum sirf do directions se aa sakte hain:

1. Upar wale cell `(r-1,c)` se.
2. Left wale cell `(r,c-1)` se.

Isliye:

ways(r,c) = ways(r-1,c) + ways(r,c-1)

### Base Cases

**Case 1: Starting cell**

Agar `r == 0 && c == 0` hai, toh return `1`.

Starting cell tak pahunchne ka ek hi way hai.

**Case 2: Grid ke bahar**

Agar `r < 0 || c < 0` hai, toh return `0`.

Grid ke bahar jaane wala path valid nahi hai.

**Case 3: Answer already calculated hai**

Agar `dp[r][c] != -1` hai, toh saved answer return kar denge.

Isse same calculation dobara nahi karni padegi.

### Recursion ka Flow

1. Destination cell se recursion start hota hai.
2. `up` aur `left` wale cells ke answers calculate hote hain.
3. Dono answers ko add karte hain.
4. Result ko `dp[r][c]` mein store karte hain.
5. Final answer destination cell ka result hota hai.

---

## 3. C++ Code — Memoization

```cpp
class Solution {
public:
    int solve(int r, int c, vector<vector<int>>& dp) {

        // Starting cell tak pahunchne ka 1 way hai
        if (r == 0 && c == 0) {
            return 1;
        }

        // Grid ke bahar hain
        if (r < 0 || c < 0) {
            return 0;
        }

        // Answer pehle se calculated hai
        if (dp[r][c] != -1) {
            return dp[r][c];
        }

        // Upar wale cell se aane ke ways
        int up = solve(r - 1, c, dp);

        // Left wale cell se aane ke ways
        int left = solve(r, c - 1, dp);

        // Dono directions ke ways add karke save karo
        dp[r][c] = up + left;

        return dp[r][c];
    }

    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, -1));

        // Destination cell se start karo
        return solve(m - 1, n - 1, dp);
    }
};
```

---

## 4. Code Explanation

### Function 1: uniquePaths()

```cpp
int uniquePaths(int m, int n) {
    vector<vector<int>> dp(m, vector<int>(n, -1));

    return solve(m - 1, n - 1, dp);
}
```

- `dp` ek 2D array hai, jisme initially sab values `-1` hain.
- `-1` ka matlab hai ki cell ka answer abhi calculate nahi hua.
- `solve(m-1, n-1, dp)` destination cell se calculation start karta hai.

### Function 2: solve()

```cpp
int solve(int r, int c, vector<vector<int>>& dp)
```

Iska kaam hai starting cell `(0,0)` se current cell `(r,c)` tak pahunchne ke total ways return karna.

### Up Call

```cpp
int up = solve(r - 1, c, dp);
```

Current cell ke upar wale cell ke total ways find karta hai.

### Left Call

```cpp
int left = solve(r, c - 1, dp);
```

Current cell ke left wale cell ke total ways find karta hai.

### DP Transition

```cpp
dp[r][c] = up + left;
```

Upar se aane wale ways aur left se aane wale ways ko add karke current cell ka answer store karta hai.

### Return

```cpp
return dp[r][c];
```

Calculated answer calling function ko wapas mil jaata hai.

---

## 5. Detailed Dry Run

Input:

```cpp
m = 2;
n = 2;
```

Grid:

```text
       Column 0    Column 1

Row 0    (0,0)       (0,1)
Row 1    (1,0)       (1,1)
```

Starting DP:

```text
dp = [
    [-1, -1],
    [-1, -1]
]
```

### Step 1: Destination se start

```cpp
solve(1, 1, dp);
```

Current cell `(1,1)` hai.

```cpp
up = solve(0, 1, dp);
```

Pehle `solve(0,1)` execute hoga.

### Step 2: solve(0,1)

```cpp
up = solve(-1, 1, dp);
```

Row `-1` hai, jo grid ke bahar hai.

Answer = `0`

Ab left call:

```cpp
left = solve(0, 0, dp);
```

### Step 3: solve(0,0)

Ye starting cell hai.

```cpp
if (r == 0 && c == 0) {
    return 1;
}
```

Answer = `1`

Ab `solve(0,1)` ke paas dono answers aa gaye:

```text
up   = 0
left = 1

dp[0][1] = 0 + 1
         = 1
```

Ab `solve(0,1)` return `1` karega.

### Step 4: Wapas solve(1,1)

Ab destination ke upar wale cell ka answer mil gaya:

```text
up = 1
```

Ab left call execute hogi:

```cpp
left = solve(1, 0, dp);
```

### Step 5: solve(1,0)

Pehle up call:

```cpp
up = solve(0, 0, dp);
```

Starting cell hai, toh answer `1`.

Ab left call:

```cpp
left = solve(1, -1, dp);
```

Column `-1` hai, toh grid ke bahar. Answer `0`.

Ab:

```text
dp[1][0] = 1 + 0
         = 1
```

`solve(1,0)` return `1` karega.

### Step 6: Final Answer

Ab destination `(1,1)` par wapas aate hain.

```text
up   = 1
left = 1

dp[1][1] = 1 + 1
         = 2
```

Final DP matrix:

```text
[
    [1, 1],
    [1, 2]
]
```

**Final Answer = 2**

Possible paths:
1. Right → Down
2. Down → Right

---

## 6. Memoization ka Benefit

Maan lo kisi cell ka answer pehle calculate ho chuka hai.

```cpp
if (dp[r][c] != -1) {
    return dp[r][c];
}
```

Toh function dobara us cell ka answer calculate nahi karega.

Woh seedha saved result return kar dega.

Isi technique ko **Memoization (Top-Down DP)** kehte hain.

- Recursion se subproblems solve hote hain.
- DP array answers save karta hai.
- Saved answers reuse hone se repeated calculations kam hoti hain.

---

## 7. Time and Space Complexity

Let:
- `m` = number of rows
- `n` = number of columns

**Time Complexity: O(m × n)**

Har cell ka answer maximum ek baar calculate hota hai.

**DP Space Complexity: O(m × n)**

DP matrix mein `m × n` values store hoti hain.

**Recursion Stack: O(m + n)**

Maximum recursive depth destination se start tak row aur column ko reduce karte hue hoti hai.

Total auxiliary space: **O(m × n)**

---

## 8. Interview Explanation

"Is problem ko main recursion with memoization se solve karunga. Har cell par main uske upper cell aur left cell se aane wale total paths calculate karunga. Starting cell par 1 return karunga aur grid ke bahar jaane par 0 return karunga. Repeated calculations avoid karne ke liye har cell ka answer 2D DP array mein store karunga. Is tarah time complexity O(m × n) ho jaati hai."

---

## 9. Important Points to Remember

- Starting cell `(0,0)` returns `1`.
- Invalid coordinates return `0`.
- `up` represents `solve(r-1,c,dp)`.
- `left` represents `solve(r,c-1,dp)`.
- Current cell ka answer = `up + left`.
- `dp[r][c]` calculated answer ko save karta hai.
- Destination is `(m-1,n-1)`.
- Approach: Recursion + Memoization (Top-Down DP).
- Time Complexity: `O(m × n)`.

---

## 10. Pattern Connection

**Pattern 2 — Grid DP**

Completed:
- #6 — LC 62: Unique Paths

Next:
- #7 — LC 63: Unique Paths II

LC 63 mein grid ke andar obstacles bhi honge, isliye humein invalid cells ko handle karna seekhna hoga.
