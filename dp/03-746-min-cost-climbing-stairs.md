# DP #3 — LeetCode 746: Min Cost Climbing Stairs

## 1. Problem Statement

Humein ek integer array `cost` diya gaya hai.

Har `cost[i]` batata hai ki stair `i` par step rakhne ka cost kitna hai.

Rules:
- Hum starting mein step 0 ya step 1 se shuru kar sakte hain.
- Ek move mein 1 ya 2 steps chadh sakte hain.
- Top tak pahunchne ki minimum total cost return karni hai.
- Top array ke last index ke just baad hota hai.

### Example

Input:

```cpp
cost = [10, 15, 20];
```

Output:

```cpp
15
```

Explanation:

Do options hain.

**Option 1: Step 0 se start karna**

```text
Step 0 ka cost = 10
Step 1 ka cost = 15

Total cost = 10 + 15 = 25
```

**Option 2: Step 1 se start karna**

```text
Step 1 ka cost = 15

Step 1 se 2 steps chadhkar top par pahunch sakte hain.

Total cost = 15
```

Minimum cost = `15`.

---

## 2. Pattern

**Pattern 1 — 1D Dynamic Programming**

Is question mein hum seekhenge:

- DP State
- Recursion
- Memoization (Top-Down DP)
- Base Case
- DP Transition
- Minimum cost calculate karna

---

## 3. Approach — Memoization

Memoization mein hum recursion use karte hain aur calculated answers ko `dp` array mein save karte hain.

Agar kisi problem ka answer pehle calculate ho chuka hai, toh usko dobara calculate karne ke bajaye saved answer return karte hain.

### Step 1: `solve(i)` ka meaning

Hum define karenge:

**`solve(i)` = stair `i` par land karne ki minimum total cost, including us stair ka cost.**

Example:

```text
cost = [10, 15, 20]

solve(0) = 10
solve(1) = 15
```

Ye hamare base cases hain.

### Step 2: Base Case

```cpp
if (i <= 1) {
    return cost[i];
}
```

Agar `i = 0`, toh `cost[0]` return hoga.

Agar `i = 1`, toh `cost[1]` return hoga.

In dono stairs tak pahunchne ki cost directly available hai.

### Step 3: Previous answers check karo

```cpp
if (dp[i] != -1) {
    return dp[i];
}
```

`-1` ka matlab answer abhi calculate nahi hua.

Agar `dp[i]` mein answer saved hai, toh usko directly return kar denge.

### Step 4: DP Transition

Kisi stair `i` par pahunchne ke do options hain:

- Stair `i - 1` se ek step chadhkar.
- Stair `i - 2` se do steps chadhkar.

Dono routes ki minimum cost compare karenge.

```cpp
dp[i] = cost[i] + min(
    solve(i - 1, cost, dp),
    solve(i - 2, cost, dp)
);
```

Yahan:

- `solve(i - 1, cost, dp)` = ek step pehle tak pahunchne ki minimum cost.
- `solve(i - 2, cost, dp)` = do steps pehle tak pahunchne ki minimum cost.
- `min()` = dono mein se cheaper route choose karta hai.
- `cost[i]` = current stair ka cost.

### Step 5: Calculated answer save karo

```cpp
return dp[i];
```

Current stair ka answer `dp[i]` mein save ho chuka hai, isliye use return karenge.

---

## 4. C++ Code — Memoization

```cpp
class Solution {
public:
    int solve(int i, vector<int>& cost, vector<int>& dp) {
        if (i <= 1) {
            return cost[i];
        }

        if (dp[i] != -1) {
            return dp[i];
        }

        dp[i] = cost[i] + min(
            solve(i - 1, cost, dp),
            solve(i - 2, cost, dp)
        );

        return dp[i];
    }

    int minCostClimbingStairs(vector<int>& cost) {
        int n = cost.size();
        vector<int> dp(n, -1);

        return min(
            solve(n - 1, cost, dp),
            solve(n - 2, cost, dp)
        );
    }
};
```

---

## 5. Code Explanation

### A. `solve()` function

```cpp
int solve(int i, vector<int>& cost, vector<int>& dp)
```

- `i` = jis stair tak pahunchne ki minimum cost calculate karni hai.
- `cost` = har stair ka cost.
- `dp` = calculated answers ko save karne wala vector.

### B. Base Case

```cpp
if (i <= 1) {
    return cost[i];
}
```

Step 0 aur Step 1 ke answers directly return hote hain.

### C. Saved answer check karna

```cpp
if (dp[i] != -1) {
    return dp[i];
}
```

Agar answer already save hai, toh recursion dobara nahi chalayenge.

Example:

```text
dp[2] = 30
```

Agar `solve(2)` dobara call hua, toh seedha `30` return hoga.

### D. Minimum cost calculate karna

```cpp
dp[i] = cost[i] + min(
    solve(i - 1, cost, dp),
    solve(i - 2, cost, dp)
);
```

Pichhli do stairs tak pahunchne ki minimum costs compare karte hain.

Jo route cheaper hai, uski cost mein current stair ka cost add karte hain.

### E. Main function

```cpp
int minCostClimbingStairs(vector<int>& cost) {
    int n = cost.size();
    vector<int> dp(n, -1);

    return min(
        solve(n - 1, cost, dp),
        solve(n - 2, cost, dp)
    );
}
```

Sabse pehle:

```cpp
int n = cost.size();
```

`n` stairs ki total quantity hai.

Phir:

```cpp
vector<int> dp(n, -1);
```

Har stair ka answer save karne ke liye `n` positions wala vector banate hain. Initially sab values `-1` hain.

---

## 6. Main Function Mein Doosra `min()` Kyun Hai?

Ye is question ka important concept hai.

```cpp
return min(
    solve(n - 1, cost, dp),
    solve(n - 2, cost, dp)
);
```

Is `min()` ka kaam top tak pahunchne ke dono options compare karna hai.

**Top array ke andar nahi hai.** Top last stair ke just baad hai.

Top tak pahunchne ke liye:

1. Last stair `n - 1` se ek step aage ja sakte hain.
2. Second-last stair `n - 2` se do steps aage ja sakte hain.

Isliye dono stairs tak pahunchne ki minimum costs compare karte hain.

### Dono `min()` ka difference

**Pehla `min()` — `solve()` ke andar**

```cpp
cost[i] + min(
    solve(i - 1, cost, dp),
    solve(i - 2, cost, dp)
);
```

Ye kisi particular stair tak pahunchne ka cheaper route choose karta hai.

**Doosra `min()` — Main function mein**

```cpp
min(
    solve(n - 1, cost, dp),
    solve(n - 2, cost, dp)
);
```

Ye poori staircase ke top tak pahunchne ka cheaper route choose karta hai.

Dono alag decisions hain, isliye dono `min()` ki zaroorat hai.

---

## 7. Detailed Dry Run

Input:

```cpp
cost = [10, 15, 20, 5];
```

Indices:

```text
Index:  0   1   2   3
Cost:  10  15  20   5
```

Yahan:

- Step 0 ka cost = 10
- Step 1 ka cost = 15
- Step 2 ka cost = 20
- Step 3 ka cost = 5
- Top array ke bahar hai.

Initially:

```text
n = 4

dp = [-1, -1, -1, -1]
```

Main function ye call karega:

```cpp
solve(3, cost, dp)
```

### Step 1: `solve(3)` call hota hai

`i = 3`

Base case:

```cpp
if (i <= 1)
```

Check:

```text
3 <= 1 → False
```

Saved answer check:

```cpp
if (dp[3] != -1)
```

Check:

```text
dp[3] = -1

-1 != -1 → False
```

Answer abhi calculate nahi hua.

Ab formula execute hoga:

```cpp
dp[3] = cost[3] + min(
    solve(2, cost, dp),
    solve(1, cost, dp)
);
```

Values:

```text
cost[3] = 5

solve(2) aur solve(1) calculate karne hain.
```

Pehle `solve(2)` calculate hoga.

### Step 2: `solve(2)` call hota hai

Formula:

```cpp
dp[2] = cost[2] + min(
    solve(1, cost, dp),
    solve(0, cost, dp)
);
```

Yahan:

```text
cost[2] = 20
```

Humein `solve(1)` aur `solve(0)` chahiye.

**`solve(1)`**

Base case true hai, kyunki `i = 1`.

```text
solve(1) = cost[1]
solve(1) = 15
```

**`solve(0)`**

Base case true hai, kyunki `i = 0`.

```text
solve(0) = cost[0]
solve(0) = 10
```

Ab `solve(2)` ke paas:

```text
solve(1) = 15
solve(0) = 10
```

Pehla `min()`:

```cpp
dp[2] = cost[2] + min(15, 10);
```

Calculation:

```text
min(15, 10) = 10

dp[2] = 20 + 10
dp[2] = 30
```

Answer save ho gaya:

```text
dp = [-1, -1, 30, -1]
```

Ab:

```text
solve(2) = 30
```

**Meaning:** Step 2 tak pahunchne ki minimum cost `30` hai.

### Step 3: Wapas `solve(3)` par aao

Ab `solve(3)` ko `solve(2) = 30` mil gaya.

Doosri recursive call:

```cpp
solve(1, cost, dp)
```

Base case se answer milega:

```text
solve(1) = 15
```

Ab `solve(3)` ke paas:

```text
cost[3] = 5
solve(2) = 30
solve(1) = 15
```

Formula:

```cpp
dp[3] = cost[3] + min(30, 15);
```

Calculation:

```text
min(30, 15) = 15

dp[3] = 5 + 15
dp[3] = 20
```

Answer save ho gaya:

```text
dp = [-1, -1, 30, 20]
```

Ab:

```text
solve(3) = 20
```

**Meaning:** Step 3 tak pahunchne ki minimum cost `20` hai.

### Step 4: Main function mein wapas aao

Main function mein:

```cpp
solve(n - 1, cost, dp)
```

Yaani:

```cpp
solve(3, cost, dp) = 20
```

Ab doosri call:

```cpp
solve(n - 2, cost, dp)
```

Yaani:

```cpp
solve(2, cost, dp)
```

Lekin `dp[2] = 30` pehle se saved hai.

Condition:

```cpp
if (dp[i] != -1) {
    return dp[i];
}
```

Check:

```text
dp[2] != -1

30 != -1 → True
```

Isliye answer dobara calculate nahi hoga. Direct `30` return ho jayega.

Ab main function mein:

```text
first = 20
second = 30
```

Final line:

```cpp
return min(first, second);
```

Calculation:

```text
min(20, 30) = 20
```

### Final Output

```text
20
```

### Complete Dry Run Table

| Function / Step | Calculation | Answer |
|---|---|---:|
| `solve(0)` | Base case → `cost[0]` | 10 |
| `solve(1)` | Base case → `cost[1]` | 15 |
| `solve(2)` | `20 + min(15, 10)` | 30 |
| `solve(3)` | `5 + min(30, 15)` | 20 |
| Main function | `min(solve(3), solve(2))` | **20** |

---

## 8. Important Interview Points

1. **Pattern:** 1D Dynamic Programming.
2. `solve(i)` stair `i` tak land karne ki minimum total cost batata hai.
3. Base cases: `solve(0) = cost[0]` aur `solve(1) = cost[1]`.
4. Transition: `cost[i] + min(solve(i-1), solve(i-2))`.
5. `dp[i]` calculated answer ko save karta hai.
6. `dp[i] != -1` ka matlab answer pehle se calculate ho chuka hai.
7. Main function mein `min(solve(n-1), solve(n-2))` top tak pahunchne ke dono options compare karta hai.
8. Memoization ko Top-Down DP kehte hain.

---

## 9. Time and Space Complexity

### Time Complexity: O(n)

Har stair ka answer maximum ek baar calculate hota hai. Saved answers dobara calculate nahi kiye jaate.

### Auxiliary Space Complexity: O(n)

- DP array: `O(n)`
- Recursion stack: `O(n)` worst case

Overall auxiliary space `O(n)` hai.

---

## 10. Common Mistakes

- `solve(i)` ka meaning clear na rakhna.
- Current stair ka `cost[i]` add karna bhool jaana.
- `dp[i] != -1` check karna bhool jaana.
- `dp` mein calculated answer save nahi karna.
- Main function mein sirf `solve(n-1)` return karna.
- Top ko array ka last element samajhna.
- Andar aur bahar wale `min()` ko same purpose ka samajhna.

---

## 11. Final Takeaway

**Memoization ka flow:**

1. `solve(i)` current stair tak minimum cost calculate karta hai.
2. Base cases starting stairs ki cost return karte hain.
3. Recurrence previous do stairs ki minimum cost choose karti hai.
4. Calculated answers `dp` mein save hote hain.
5. Main function last two stairs tak pahunchne ki minimum costs compare karta hai.

**Yaad rakh bhai:** Andar wala `min()` ek stair tak pahunchne ka cheaper route choose karta hai. Main function wala `min()` final top tak pahunchne ka cheaper route choose karta hai.
