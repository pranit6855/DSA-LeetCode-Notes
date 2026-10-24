# DP #2 — LeetCode 70: Climbing Stairs

## 1. Problem Statement

Ek staircase mein total `n` steps hain.

Tu ek baar mein:

- 1 step chadh sakta hai.
- 2 steps chadh sakta hai.

Humein calculate karna hai ki top tak pahunchne ke kitne different ways hain.

### Example 1

Input:

```cpp
n = 2;
```

Possible ways:

```text
Way 1: 1 + 1
Way 2: 2
```

Output:

```cpp
2
```

### Example 2

Input:

```cpp
n = 3;
```

Possible ways:

```text
Way 1: 1 + 1 + 1
Way 2: 1 + 2
Way 3: 2 + 1
```

Output:

```cpp
3
```

Important: `1 + 2` aur `2 + 1` different ways hain, kyunki steps chadhne ka order alag hai.

---

## 2. Pattern

**Pattern 1 — 1D Dynamic Programming**

Is question mein hum seekhenge:

- DP State
- Base Cases
- Recursion
- Memoization (Top-Down DP)
- DP Transition
- Repeated calculations ko avoid karna

---

## 3. Main Logic

Maan le humein stair `n` par pahunchna hai.

Last move do tarah ka ho sakta hai:

1. Ek step ka move.
2. Do steps ka move.

Agar last move 1 step ka tha, toh usse pehle humein stair `n - 1` par hona chahiye.

Agar last move 2 steps ka tha, toh usse pehle humein stair `n - 2` par hona chahiye.

Isliye:

```cpp
solve(n) = solve(n - 1) + solve(n - 2);
```

**Dono ke ways add honge**, kyunki last move 1 step ya 2 steps ka ho sakta hai.

---

## 4. DP State

Hum define karenge:

```text
solve(n) = n stairs chadhne ke total different ways
```

Example:

```text
solve(1) = 1
solve(2) = 2
```

Agar 1 stair hai, toh ek hi way hai.

Agar 2 stairs hain, toh do ways hain:

```text
1 + 1
2
```

---

## 5. Base Cases

```cpp
if (n <= 2) {
    return n;
}
```

Iska meaning:

- `n = 1` → 1 way.
- `n = 2` → 2 ways.

Ye starting answers hain.

---

## 6. DP Transition

```cpp
dp[n] = solve(n - 1, dp) + solve(n - 2, dp);
```

`solve(n - 1, dp)` = n - 1 stairs chadhne ke total ways.

`solve(n - 2, dp)` = n - 2 stairs chadhne ke total ways.

Dono add karne se n stairs chadhne ke total ways milte hain.

---

## 7. Memoization Kya Hai?

Memoization mein recursion ke answers `dp` array mein save hote hain.

Agar same subproblem dobara aati hai, toh saved answer return kar dete hain.

### DP Array

```cpp
vector<int> dp(n + 1, -1);
```

Agar `n = 5`:

```text
Index:  0   1   2   3   4   5
dp:    -1  -1  -1  -1  -1  -1
```

`-1` ka matlab hai answer abhi calculate nahi hua.

### Saved Answer Check

```cpp
if (dp[n] != -1) {
    return dp[n];
}
```

Agar answer pehle calculate ho chuka hai, toh dobara calculation nahi karenge.

### Answer Save Karna

```cpp
dp[n] = solve(n - 1, dp) + solve(n - 2, dp);
```

Previous states ke answers add karke current answer save kar denge.

---

## 8. C++ Code — Memoization

```cpp
class Solution {
public:
    int solve(int n, vector<int>& dp) {
        if (n <= 2) {
            return n;
        }

        if (dp[n] != -1) {
            return dp[n];
        }

        dp[n] = solve(n - 1, dp) + solve(n - 2, dp);

        return dp[n];
    }

    int climbStairs(int n) {
        vector<int> dp(n + 1, -1);

        return solve(n, dp);
    }
};
```

---

## 9. Code Explanation

### A. `solve()` Function

```cpp
int solve(int n, vector<int>& dp)
```

- `n` = stairs ki quantity jiske ways calculate karne hain.
- `dp` = calculated answers ko save karne wala vector.

### B. Base Case

```cpp
if (n <= 2) {
    return n;
}
```

Ek stair ke liye 1 way aur do stairs ke liye 2 ways hain.

Isliye in answers ko directly return karte hain.

### C. Saved Answer Check

```cpp
if (dp[n] != -1) {
    return dp[n];
}
```

Agar answer pehle se saved hai, toh calculation repeat nahi karenge.

### D. Recursive Formula

```cpp
dp[n] = solve(n - 1, dp) + solve(n - 2, dp);
```

Last move 1 step ya 2 steps ka ho sakta hai.

Dono cases ke ways add karte hain.

### E. Return Saved Answer

```cpp
return dp[n];
```

Calculated answer return hota hai.

### F. Main Function

```cpp
int climbStairs(int n) {
    vector<int> dp(n + 1, -1);

    return solve(n, dp);
}
```

DP array initialize hota hai aur recursion original `n` se start hoti hai.

---

## 10. Detailed Dry Run — n = 5

Input:

```cpp
n = 5;
```

Initially:

```text
dp = [-1, -1, -1, -1, -1, -1]
```

### Step 1: solve(5)

```cpp
solve(5, dp);
```

Base case:

```text
5 <= 2 → False
```

Saved answer check:

```text
dp[5] = -1
```

Answer saved nahi hai.

Formula:

```cpp
dp[5] = solve(4, dp) + solve(3, dp);
```

Pehle `solve(4)` calculate hoga.

### Step 2: solve(4)

Formula:

```cpp
dp[4] = solve(3, dp) + solve(2, dp);
```

Pehle `solve(3)` calculate hoga.

### Step 3: solve(3)

Formula:

```cpp
dp[3] = solve(2, dp) + solve(1, dp);
```

Base cases:

```text
solve(2) = 2
solve(1) = 1
```

Calculation:

```text
dp[3] = 2 + 1
dp[3] = 3
```

Answer save:

```text
dp = [-1, -1, -1, 3, -1, -1]
```

Ab `solve(3)` answer `3` return karega.

### Step 4: solve(4) Complete Karo

Ab:

```text
solve(3) = 3
solve(2) = 2
```

Calculation:

```text
dp[4] = 3 + 2
dp[4] = 5
```

Answer save:

```text
dp = [-1, -1, -1, 3, 5, -1]
```

Ab `solve(4)` answer `5` return karega.

### Step 5: solve(5) Complete Karo

Ab:

```text
solve(4) = 5
```

Phir code call karega:

```cpp
solve(3, dp);
```

Lekin `dp[3] = 3` pehle se saved hai.

Condition:

```cpp
if (dp[n] != -1) {
    return dp[n];
}
```

Check:

```text
dp[3] != -1
3 != -1 → True
```

Isliye direct `3` return ho jayega. `solve(3)` dobara calculate nahi hoga.

Ab:

```text
dp[5] = 5 + 3
dp[5] = 8
```

Final DP array:

```text
dp = [-1, -1, -1, 3, 5, 8]
```

Final output:

```text
8
```

---

## 11. Complete Dry Run Table

| Function call | Calculation / Decision | Answer |
|---|---|---:|
| `solve(1)` | Base case | 1 |
| `solve(2)` | Base case | 2 |
| `solve(3)` | `2 + 1` | 3 |
| `solve(4)` | `3 + 2` | 5 |
| `solve(3)` again | Saved `dp[3]` | 3 |
| `solve(5)` | `5 + 3` | 8 |

Final answer = `8`.

---

## 12. Fibonacci Aur Climbing Stairs Mein Difference

Dono mein transition similar hai:

```cpp
dp[n] = dp[n - 1] + dp[n - 2];
```

Lekin base cases alag hain.

| Question | Base Case 0 | Base Case 1 | Base Case 2 |
|---|---:|---:|---:|
| Fibonacci | `F(0) = 0` | `F(1) = 1` | `F(2) = 1` |
| Climbing Stairs | Usually `ways(0) = 1` in other formulations | `ways(1) = 1` | `ways(2) = 2` |

Is solution mein Climbing Stairs ke liye `n <= 2` directly return kiya hai.

**Important:** Formula similar hone ka matlab ye nahi ki dono questions ka meaning same hai. Fibonacci number calculate karta hai; Climbing Stairs possible ways count karta hai.

---

## 13. Time and Space Complexity

### Time Complexity: O(n)

Har subproblem ka answer maximum ek baar calculate hota hai.

Saved answer dobara calculate nahi hota.

### Auxiliary Space Complexity: O(n)

- DP array: `O(n)`
- Recursion stack: `O(n)` worst case

Overall auxiliary space `O(n)` hai.

---

## 14. Important Interview Points

1. **Pattern:** 1D Dynamic Programming.
2. `solve(n)` n stairs chadhne ke total ways batata hai.
3. Base cases: `solve(1) = 1`, `solve(2) = 2`.
4. Transition: `solve(n) = solve(n-1) + solve(n-2)`.
5. `dp[n]` calculated answer save karta hai.
6. `dp[n] != -1` hone par saved answer return hota hai.
7. Memoization repeated calculations avoid karti hai.
8. Is approach ka time aur auxiliary space dono `O(n)` hain.

---

## 15. Common Mistakes

- Ways count karne ke bajaye minimum cost samajhna.
- Base cases galat set karna.
- `solve(n - 1) + solve(n - 2)` ka meaning samjhe bina use karna.
- DP array initialize karna bhool jaana.
- `dp[n]` mein calculated answer save nahi karna.
- `dp[n] != -1` check karna bhool jaana.

---

## 16. Final Takeaway

**Climbing Stairs ka core logic:**

1. Ek move mein 1 ya 2 steps chadh sakte hain.
2. `solve(n)` current stairs tak pahunchne ke total ways batata hai.
3. Last move 1 step ya 2 steps ka ho sakta hai.
4. Dono possibilities ke answers add karte hain.
5. Memoization calculated answers save karti hai.

Formula:

```cpp
dp[n] = solve(n - 1, dp) + solve(n - 2, dp);
```

Memoization ka flow:

**Recursion → Base Case → Saved Answer Check → Answer Calculate Karo → DP Mein Save Karo → Return Karo.**
