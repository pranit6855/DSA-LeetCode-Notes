# DP #1 — LeetCode 509: Fibonacci Number

## 1. Problem Statement

Humein ek integer `n` diya hai.

Humein Fibonacci series ka `n`th number return karna hai.

Fibonacci series mein har number, uske pichhle do numbers ka sum hota hai.

Rules:

- F(0) = 0
- F(1) = 1
- F(n) = F(n - 1) + F(n - 2), jab n > 1 ho.

### Example 1

Input:

```cpp
n = 2;
```

Output:

```cpp
1
```

Explanation:

```text
F(2) = F(1) + F(0)
     = 1 + 0
     = 1
```

### Example 2

Input:

```cpp
n = 4;
```

Output:

```cpp
3
```

Explanation:

```text
F(0) = 0
F(1) = 1
F(2) = 1
F(3) = 2
F(4) = 3
```

Isliye answer `3` hai.

---

## 2. Pattern

**Pattern 1 — 1D DP & Foundation**

Is question se hum ye concepts seekhenge:

- Recursion
- Base Case
- Overlapping Subproblems
- Memoization (Top-Down DP)
- Tabulation (Bottom-Up DP)
- DP State
- DP Transition

---

## 3. Fibonacci ka Rule

Fibonacci ka formula:

```text
F(n) = F(n - 1) + F(n - 2)
```

Matlab current Fibonacci number nikalne ke liye previous two numbers chahiye.

Example:

```text
F(4) = F(3) + F(2)
     = 2 + 1
     = 3
```

Yahi formula hum DP approaches mein use karenge.

---

# APPROACH 1 — Tabulation (Bottom-Up DP)

## 4. Tabulation Kya Hai?

Tabulation mein hum chhote problems ke answers pehle calculate karte hain aur un answers ko use karke bade problems solve karte hain.

Ismein recursion use nahi hota. Hum loop se DP array fill karte hain.

### Step 1: DP State

Hum define karenge:

```text
dp[i] = ith Fibonacci number
```

Agar `n = 5` hai, toh humein `dp[0]` se `dp[5]` tak values chahiye.

### Step 2: DP Array Banao

```cpp
vector<int> dp(n + 1, 0);
```

`n = 5` hone par:

```text
Index:  0  1  2  3  4  5
dp:     0  0  0  0  0  0
```

### Step 3: Base Cases Set Karo

```cpp
dp[0] = 0;
dp[1] = 1;
```

Ab:

```text
Index:  0  1  2  3  4  5
dp:     0  1  0  0  0  0
```

### Step 4: DP Transition Apply Karo

```cpp
dp[i] = dp[i - 1] + dp[i - 2];
```

Previous two answers ko add karke current answer store karenge.

### Step 5: C++ Code

```cpp
class Solution {
public:
    int fib(int n) {
        if (n <= 1) {
            return n;
        }

        vector<int> dp(n + 1, 0);

        dp[0] = 0;
        dp[1] = 1;

        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }

        return dp[n];
    }
};
```

---

## 5. Tabulation Code Explanation

### Base Case

```cpp
if (n <= 1) {
    return n;
}
```

Agar `n = 0`, toh answer `0`.

Agar `n = 1`, toh answer `1`.

### DP Array

```cpp
vector<int> dp(n + 1, 0);
```

`n + 1` positions banate hain, kyunki indexing zero se start hoti hai.

Example: `n = 4` toh indices `0, 1, 2, 3, 4` hongi.

### Base Values

```cpp
dp[0] = 0;
dp[1] = 1;
```

Ye Fibonacci ki starting values hain.

### Loop

```cpp
for (int i = 2; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
}
```

Har index par previous two values add karte hain.

### Return

```cpp
return dp[n];
```

Humein `n`th Fibonacci number chahiye, isliye `dp[n]` return karte hain.

---

## 6. Detailed Dry Run — Tabulation

Input:

```cpp
n = 5;
```

Initially:

```text
dp = [0, 0, 0, 0, 0, 0]
```

Base cases set karne ke baad:

```text
dp = [0, 1, 0, 0, 0, 0]
```

### Iteration 1: i = 2

```cpp
dp[2] = dp[1] + dp[0];
```

Calculation:

```text
dp[2] = 1 + 0
dp[2] = 1
```

Ab:

```text
dp = [0, 1, 1, 0, 0, 0]
```

### Iteration 2: i = 3

```cpp
dp[3] = dp[2] + dp[1];
```

Calculation:

```text
dp[3] = 1 + 1
dp[3] = 2
```

Ab:

```text
dp = [0, 1, 1, 2, 0, 0]
```

### Iteration 3: i = 4

```cpp
dp[4] = dp[3] + dp[2];
```

Calculation:

```text
dp[4] = 2 + 1
dp[4] = 3
```

Ab:

```text
dp = [0, 1, 1, 2, 3, 0]
```

### Iteration 4: i = 5

```cpp
dp[5] = dp[4] + dp[3];
```

Calculation:

```text
dp[5] = 3 + 2
dp[5] = 5
```

Final DP array:

```text
dp = [0, 1, 1, 2, 3, 5]
```

Final answer:

```cpp
return dp[5];
```

Output:

```text
5
```

---

# APPROACH 2 — Memoization (Top-Down DP)

## 7. Memoization Kya Hai?

Memoization mein recursion use karte hain aur calculated answers ko DP array mein save karte hain.

Agar same answer dobara chahiye, toh usko recalculate karne ke bajaye saved answer return kar dete hain.

Example:

```text
F(5) = F(4) + F(3)
```

`F(4)` aur `F(3)` calculate karte waqt kuch subproblems repeat ho sakti hain.

Memoization unke answers save karke repeated calculations avoid karti hai.

### Step 1: DP Array Banao

```cpp
vector<int> dp(n + 1, -1);
```

`-1` ka matlab hai ki answer abhi calculate nahi hua.

Example:

```text
Index:  0   1   2   3   4   5
dp:    -1  -1  -1  -1  -1  -1
```

### Step 2: Base Case

```cpp
if (n <= 1) {
    return n;
}
```

`F(0) = 0` aur `F(1) = 1`.

### Step 3: Saved Answer Check Karo

```cpp
if (dp[n] != -1) {
    return dp[n];
}
```

Yahan `n` batata hai ki kis Fibonacci number ka answer chahiye.

Example:

```text
n = 4
dp[n] = dp[4]
```

Agar `dp[4]` mein answer already saved hai, toh wahi return karenge.

### Step 4: Calculate Karo Aur Save Karo

```cpp
dp[n] = solve(n - 1, dp) + solve(n - 2, dp);
```

Previous two Fibonacci numbers calculate karte hain aur unka sum `dp[n]` mein save kar dete hain.

### Step 5: C++ Code

```cpp
class Solution {
public:
    int solve(int n, vector<int>& dp) {
        if (n <= 1) {
            return n;
        }

        if (dp[n] != -1) {
            return dp[n];
        }

        dp[n] = solve(n - 1, dp) + solve(n - 2, dp);

        return dp[n];
    }

    int fib(int n) {
        vector<int> dp(n + 1, -1);

        return solve(n, dp);
    }
};
```

---

## 8. Memoization Code Explanation

### Function

```cpp
int solve(int n, vector<int>& dp)
```

- `n` = jis Fibonacci number ka answer chahiye.
- `dp` = calculated answers ko save karne wala vector.

### Base Case

```cpp
if (n <= 1) {
    return n;
}
```

Starting Fibonacci values directly return karte hain.

### Saved Answer Check

```cpp
if (dp[n] != -1) {
    return dp[n];
}
```

Agar `dp[n]` ka answer `-1` nahi hai, toh answer already calculate ho chuka hai.

### Recursive Formula

```cpp
dp[n] = solve(n - 1, dp) + solve(n - 2, dp);
```

Previous two Fibonacci numbers calculate karte hain aur unka sum save karte hain.

### Return

```cpp
return dp[n];
```

Saved answer return ho jaata hai.

### Main Function

```cpp
int fib(int n) {
    vector<int> dp(n + 1, -1);

    return solve(n, dp);
}
```

DP array initialize hota hai aur `solve()` ko original `n` ke saath call karte hain.

---

## 9. Detailed Dry Run — Memoization

Input:

```cpp
n = 4;
```

Initially:

```text
dp = [-1, -1, -1, -1, -1]
```

Function call:

```cpp
solve(4, dp);
```

### Step 1: F(4) Calculate Karna

Base case nahi hai aur `dp[4] = -1`, toh answer abhi saved nahi hai.

Ab calculate karenge:

```text
F(4) = F(3) + F(2)
```

### Step 2: F(3) Calculate Karna

```text
F(3) = F(2) + F(1)
```

`F(1)` base case hai, isliye answer `1` return hoga.

Ab `F(2)` calculate karna hai.

### Step 3: F(2) Calculate Karna

```text
F(2) = F(1) + F(0)
```

- `F(1) = 1`
- `F(0) = 0`

Isliye:

```text
F(2) = 1 + 0 = 1
```

Save:

```text
dp[2] = 1
```

Ab:

```text
dp = [-1, -1, 1, -1, -1]
```

### Step 4: F(3) Complete Karo

Ab `F(2) = 1` aur `F(1) = 1`.

```text
F(3) = F(2) + F(1)
     = 1 + 1
     = 2
```

Save:

```text
dp[3] = 2
```

Ab:

```text
dp = [-1, -1, 1, 2, -1]
```

### Step 5: F(4) Complete Karo

Ab:

```text
F(4) = F(3) + F(2)
```

Values:

```text
F(3) = 2
F(2) = 1
```

Calculation:

```text
F(4) = 2 + 1
F(4) = 3
```

Save:

```text
dp[4] = 3
```

Final DP array:

```text
dp = [-1, -1, 1, 2, 3]
```

Final answer:

```text
3
```

Agar dobara `F(2)` chahiye hota, toh:

```cpp
if (dp[2] != -1) {
    return dp[2];
}
```

Condition true hoti aur saved answer `1` mil jaata. `F(2)` dobara calculate nahi karna padta.

---

## 10. Memoization vs Tabulation

| Point | Memoization | Tabulation |
|---|---|---|
| Technique | Recursion + saved answers | Loop + DP table |
| Direction | Top-Down | Bottom-Up |
| Starting point | Original problem | Smallest subproblems |
| Repeated work | Saved answers reuse hote hain | Answers table mein fill hote hain |
| Recursion stack | Use hota hai | Use nahi hota |
| Time | O(n) | O(n) |
| Auxiliary space | O(n) | O(n) |

---

## 11. Time and Space Complexity

### Memoization

**Time Complexity: O(n)**

Har Fibonacci state ka answer maximum ek baar calculate hota hai.

**Space Complexity: O(n)**

DP array aur recursion stack dono space use karte hain.

### Tabulation

**Time Complexity: O(n)**

Loop `2` se `n` tak chalta hai.

**Space Complexity: O(n)**

`dp` vector mein `n + 1` values store hoti hain.

---

## 12. Important Interview Points

1. Fibonacci ka formula: `F(n) = F(n-1) + F(n-2)`.
2. Base cases: `F(0) = 0`, `F(1) = 1`.
3. DP State: `dp[i]` = ith Fibonacci number.
4. DP Transition: `dp[i] = dp[i-1] + dp[i-2]`.
5. Memoization recursion ke answers save karti hai.
6. Tabulation loop se chhote answers pehle calculate karti hai.
7. Memoization ko Top-Down aur Tabulation ko Bottom-Up DP kehte hain.
8. Dono approaches ka time complexity `O(n)` hai.

## 13. Final Takeaway

**Memoization:**

Recursion → Answer calculate karo → DP array mein save karo → Repeated answer ko reuse karo.

**Tabulation:**

Base cases set karo → Loop chalao → Previous answers se next answer nikalo → Final DP value return karo.

Dono approaches DP ki foundation hain. Aage ke questions mein hum inhi ideas ko apply karenge.
