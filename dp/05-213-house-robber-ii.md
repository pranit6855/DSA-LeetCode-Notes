# DP #5 — LeetCode 213: House Robber II

## 1. Problem Statement

Ek street mein houses circle mein arranged hain. Har house mein kuch money rakha hai, jo `nums` array mein diya gaya hai.

Humein maximum money rob karna hai, lekin ek restriction hai:

**Do adjacent houses ko ek saath rob nahi kar sakte.**

Houses circle mein hain, isliye first house aur last house bhi adjacent hain.

Humein maximum amount return karna hai jo bina adjacent houses rob kiye mil sakta hai.

### Example 1

Input:

```cpp
nums = [2, 3, 2];
```

Output:

```cpp
3
```

Explanation:

First house aur last house adjacent hain. Isliye dono ko ek saath rob nahi kar sakte.

- House 0 se 2 money le sakte hain.
- House 1 se 3 money le sakte hain.
- House 2 se 2 money le sakte hain.

Maximum money = `3`.

### Example 2

Input:

```cpp
nums = [1, 2, 3, 1];
```

Output:

```cpp
4
```

Explanation:

House 0 aur House 2 rob kar sakte hain.

```text
1 + 3 = 4
```

Isliye answer `4` hai.

---

## 2. Pattern

**Pattern 1 — 1D Dynamic Programming**

Is question mein hum seekhenge:

- Pick / Skip DP
- Memoization (Top-Down DP)
- Circular array problem
- Problem ko do cases mein divide karna
- LC 198 ke logic ko reuse karna

---

## 3. LC 198 Aur LC 213 Mein Difference

### LC 198 — House Robber

Houses straight line mein hain.

```text
House 0 — House 1 — House 2 — House 3
```

First aur last house directly adjacent nahi hain.

### LC 213 — House Robber II

Houses circle mein hain.

```text
House 0 — House 1 — House 2 — House 3
   |___________________________|
```

First aur last house bhi adjacent hain.

Isliye first aur last house ko ek saath rob nahi kar sakte.

---

## 4. Main Approach — Problem Ko 2 Cases Mein Divide Karo

First aur last house ko ek saath select karna allowed nahi hai.

Isliye hum problem ko do cases mein solve karenge.

### Case 1: Last House Ko Exclude Karo

Sirf index `0` se `n - 2` tak ke houses consider karenge.

```cpp
robRange(nums, 0, n - 2);
```

Example:

```text
Original: [2, 3, 2]

Consider: [2, 3]
```

Last house ko include nahi kiya.

Ab normal House Robber (LC 198) wala Pick / Skip logic apply karenge.

### Case 2: First House Ko Exclude Karo

Sirf index `1` se `n - 1` tak ke houses consider karenge.

```cpp
robRange(nums, 1, n - 1);
```

Example:

```text
Original: [2, 3, 2]

Consider: [3, 2]
```

First house ko include nahi kiya.

Is range par bhi normal House Robber ka logic apply karenge.

### Final Answer

Dono cases ke answers compare karenge:

```cpp
return max(case1, case2);
```

Dono mein se maximum money dene wala case final answer hoga.

---

## 5. Memoization Ka Logic

Hum LC 198 ka Pick / Skip logic hi use karenge.

Lekin is baar ek extra parameter `start` bhi hoga.

```cpp
solve(i, start, nums, dp)
```

### `i` Kya Hai?

`i` current house ka index hai, jahan se hum decision le rahe hain.

### `start` Kya Hai?

`start` current range ka starting index hai.

- Case 1 mein `start = 0`.
- Case 2 mein `start = 1`.

Jab recursion `start` se pehle chali jaati hai, tab us range mein koi house nahi bacha.

```cpp
if (i < start) {
    return 0;
}
```

### `dp[i]` Kya Store Karta Hai?

```text
dp[i] = Current range mein index start se i tak
        maximum rob kiya ja sakne wala money.
```

Har case ke liye alag `dp` array banayenge, kyunki dono ranges ke starting indices alag hain.

### Saved Answer Check

```cpp
if (dp[i] != -1) {
    return dp[i];
}
```

Agar answer pehle se calculate ho chuka hai, toh wahi return kar denge.

---

## 6. Pick / Skip Formula

### Choice 1: Pick

```cpp
int pick = nums[i] + solve(i - 2, start, nums, dp);
```

Current house ke paise le lenge.

Lekin adjacent previous house ko rob nahi kar sakte, isliye `i - 2` par jayenge.

### Choice 2: Skip

```cpp
int skip = solve(i - 1, start, nums, dp);
```

Current house chhod denge aur previous house tak ka maximum answer lenge.

### Maximum Choose Karo

```cpp
dp[i] = max(pick, skip);
```

Dono choices mein se maximum money choose karke `dp[i]` mein save kar denge.

---

## 7. C++ Code — Memoization

```cpp
class Solution {
public:
    int solve(int i, int start, vector<int>& nums, vector<int>& dp) {
        if (i < start) {
            return 0;
        }

        if (dp[i] != -1) {
            return dp[i];
        }

        int pick = nums[i] + solve(i - 2, start, nums, dp);
        int skip = solve(i - 1, start, nums, dp);

        dp[i] = max(pick, skip);

        return dp[i];
    }

    int robRange(vector<int>& nums, int start, int end) {
        vector<int> dp(nums.size(), -1);

        return solve(end, start, nums, dp);
    }

    int rob(vector<int>& nums) {
        int n = nums.size();

        if (n == 1) {
            return nums[0];
        }

        int case1 = robRange(nums, 0, n - 2);
        int case2 = robRange(nums, 1, n - 1);

        return max(case1, case2);
    }
};
```

---

## 8. Code Explanation

### A. `solve()` Function

```cpp
int solve(int i, int start, vector<int>& nums, vector<int>& dp)
```

- `i` = current house ka index.
- `start` = current range ka starting index.
- `nums` = har house mein money.
- `dp` = calculated answers store karne wala vector.

### B. Base Case

```cpp
if (i < start) {
    return 0;
}
```

Agar current index allowed range ke starting index se pehle chala gaya, toh koi house available nahi hai.

Isliye `0` return karenge.

Example:

```text
start = 1
i = 0

0 < 1 → True
```

Koi house consider nahi hoga, isliye answer `0`.

### C. Saved Answer Check

```cpp
if (dp[i] != -1) {
    return dp[i];
}
```

Agar answer already calculated hai, toh recursion dobara nahi chalegi.

### D. Pick

```cpp
int pick = nums[i] + solve(i - 2, start, nums, dp);
```

Current house ke paise lete hain aur adjacent previous house ko skip karte hain.

### E. Skip

```cpp
int skip = solve(i - 1, start, nums, dp);
```

Current house chhodkar previous house tak ka maximum answer lete hain.

### F. Maximum Save Karo

```cpp
dp[i] = max(pick, skip);
```

Dono choices mein se maximum answer save karte hain.

### G. `robRange()` Function

```cpp
int robRange(vector<int>& nums, int start, int end) {
    vector<int> dp(nums.size(), -1);

    return solve(end, start, nums, dp);
}
```

Ye helper function ek specific range ke houses par normal House Robber logic apply karta hai.

Example:

```cpp
robRange(nums, 0, n - 2);
```

Ismein first house se second-last house tak consider hoga.

Har call mein naya DP array banaya jaata hai.

### H. Main Function

```cpp
int rob(vector<int>& nums) {
    int n = nums.size();

    if (n == 1) {
        return nums[0];
    }

    int case1 = robRange(nums, 0, n - 2);
    int case2 = robRange(nums, 1, n - 1);

    return max(case1, case2);
}
```

**Step 1:** Total houses ki quantity nikalte hain.

**Step 2:** Agar sirf ek house hai, toh uska money return kar dete hain.

**Step 3:** Case 1 solve karte hain, jisme last house excluded hai.

**Step 4:** Case 2 solve karte hain, jisme first house excluded hai.

**Step 5:** Dono answers mein se maximum return karte hain.

---

## 9. Detailed Dry Run — nums = [2, 3, 2]

Input:

```cpp
nums = [2, 3, 2];
```

Indices:

```text
Index:  0  1  2
Money:  2  3  2
```

Initially:

```text
n = 3
```

Sirf ek house nahi hai, isliye dono cases solve karenge.

### Case 1: Last House Exclude Karo

Function call:

```cpp
int case1 = robRange(nums, 0, n - 2);
```

Values put karo:

```cpp
robRange(nums, 0, 1);
```

Is range mein index `0` aur `1` consider honge.

```text
nums = [2, 3, 2]
Range = [0, 1]
start = 0
end = 1
```

Naya DP array:

```text
dp = [-1, -1, -1]
```

Ab call:

```cpp
solve(1, 0, nums, dp);
```

### Step 1: `solve(1, 0, nums, dp)`

Current house:

```text
i = 1
nums[1] = 3
```

Base case check:

```text
1 < 0 → False
```

Saved answer check:

```text
dp[1] = -1
```

Answer saved nahi hai.

#### Pick

```cpp
int pick = nums[1] + solve(-1, 0, nums, dp);
```

Current house ke `3` rupaye lenge.

Pick karne ke baad index `-1` par jayenge.

```text
-1 < 0 → True
```

Base case se:

```text
solve(-1) = 0
```

Isliye:

```text
pick = 3 + 0
pick = 3
```

#### Skip

```cpp
int skip = solve(0, 0, nums, dp);
```

Current house skip karke index `0` par jayenge.

### Step 2: `solve(0, 0, nums, dp)`

Current house:

```text
nums[0] = 2
```

Base case:

```text
0 < 0 → False
```

Saved answer nahi hai.

#### Pick

```cpp
pick = nums[0] + solve(-2, 0, nums, dp);
```

Calculation:

```text
pick = 2 + 0
pick = 2
```

#### Skip

```cpp
skip = solve(-1, 0, nums, dp);
```

Base case se:

```text
skip = 0
```

Ab maximum:

```cpp
dp[0] = max(2, 0);
```

Calculation:

```text
dp[0] = 2
```

DP array:

```text
dp = [2, -1, -1]
```

`solve(0)` return karega `2`.

### Step 3: `solve(1)` Complete Karo

Ab:

```text
pick = 3
skip = 2
```

Maximum:

```cpp
dp[1] = max(3, 2);
```

Calculation:

```text
dp[1] = 3
```

DP array:

```text
dp = [2, 3, -1]
```

Isliye:

```text
case1 = 3
```

Case 1 ka answer `3` hai.

---

### Case 2: First House Exclude Karo

Ab call:

```cpp
int case2 = robRange(nums, 1, n - 1);
```

Values:

```cpp
robRange(nums, 1, 2);
```

Is range mein index `1` aur `2` consider honge.

```text
start = 1
end = 2

dp = [-1, -1, -1]
```

Ab:

```cpp
solve(2, 1, nums, dp);
```

### Step 1: `solve(2, 1, nums, dp)`

Current house:

```text
nums[2] = 2
```

Base case:

```text
2 < 1 → False
```

Saved answer nahi hai.

#### Pick

```cpp
pick = nums[2] + solve(0, 1, nums, dp);
```

Current house ke `2` rupaye lene hain.

Lekin next recursive call:

```text
i = 0
start = 1
```

Base case:

```text
0 < 1 → True
```

Iska matlab current range ke bahar chale gaye.

Isliye:

```text
solve(0, 1, nums, dp) = 0
```

Ab:

```text
pick = 2 + 0
pick = 2
```

#### Skip

```cpp
skip = solve(1, 1, nums, dp);
```

Current house chhodkar index `1` par jayenge.

### Step 2: `solve(1, 1, nums, dp)`

Current house:

```text
nums[1] = 3
```

Base case:

```text
1 < 1 → False
```

Saved answer nahi hai.

#### Pick

```cpp
pick = nums[1] + solve(-1, 1, nums, dp);
```

Base case:

```text
-1 < 1 → True
```

Isliye:

```text
solve(-1) = 0
pick = 3 + 0
pick = 3
```

#### Skip

```cpp
skip = solve(0, 1, nums, dp);
```

Base case:

```text
0 < 1 → True
```

Isliye:

```text
skip = 0
```

Ab:

```cpp
dp[1] = max(3, 0);
```

Calculation:

```text
dp[1] = 3
```

DP array:

```text
dp = [-1, 3, -1]
```

`solve(1)` return karega `3`.

### Step 3: `solve(2)` Complete Karo

Ab:

```text
pick = 2
skip = 3
```

Maximum:

```cpp
dp[2] = max(2, 3);
```

Calculation:

```text
dp[2] = 3
```

DP array:

```text
dp = [-1, 3, 3]
```

Isliye:

```text
case2 = 3
```

Case 2 ka answer bhi `3` hai.

---

### Step 3: Final Answer

Main function mein:

```text
case1 = 3
case2 = 3
```

Final line:

```cpp
return max(case1, case2);
```

Calculation:

```text
max(3, 3) = 3
```

Final output:

```text
3
```

---

## 10. Complete Dry Run Summary

Input:

```text
nums = [2, 3, 2]
```

| Case | Considered indices | Houses considered | Maximum money |
|---|---|---|---:|
| Case 1 — Last exclude | `0` to `1` | `[2,3]` | 3 |
| Case 2 — First exclude | `1` to `2` | `[3,2]` | 3 |

Final answer:

```text
max(3, 3) = 3
```

---

## 11. LC 198 Aur LC 213 Ka Connection

### LC 198 — House Robber

Normal straight-line houses solve karte hain.

```cpp
int pick = nums[i] + solve(i - 2, nums, dp);
int skip = solve(i - 1, nums, dp);

dp[i] = max(pick, skip);
```

### LC 213 — House Robber II

Wahi Pick / Skip logic use hota hai, lekin circular restriction handle karne ke liye do ranges solve karte hain:

```cpp
int case1 = robRange(nums, 0, n - 2);
int case2 = robRange(nums, 1, n - 1);

return max(case1, case2);
```

**Main difference:** LC 198 mein ek range solve hoti hai; LC 213 mein first aur last house ke conflict ko avoid karne ke liye do ranges solve hoti hain.

---

## 12. Time and Space Complexity

### Time Complexity: O(n)

Case 1 maximum `n-1` houses process karta hai.

Case 2 bhi maximum `n-1` houses process karta hai.

Dono linear passes hain:

```text
O(n) + O(n) = O(n)
```

### Auxiliary Space Complexity: O(n)

Har `robRange()` call ke liye:

- DP array: `O(n)`
- Recursion stack: `O(n)` worst case

Dono cases sequentially run hote hain, parallel nahi. Isliye overall auxiliary space `O(n)` hai.

---

## 13. Important Interview Points

1. **Pattern:** 1D DP + Memoization.
2. Circular restriction ke kaaran first aur last house ek saath rob nahi kar sakte.
3. Problem ko do ranges mein divide karte hain.
4. Case 1 mein last house exclude hota hai.
5. Case 2 mein first house exclude hota hai.
6. Dono ranges mein LC 198 ka Pick / Skip logic use hota hai.
7. Final answer `max(case1, case2)` hota hai.
8. Ek hi house ho, toh directly `nums[0]` return karte hain.

---

## 14. Common Mistakes

- First aur last house ko ek saath consider karna.
- Sirf ek range solve karna aur doosri miss karna.
- Case 1 aur Case 2 ka starting index galat dena.
- Har range ke liye `dp` initialize karna bhool jaana.
- `i < start` base case ka meaning ignore karna.
- Final answer mein `min()` use karna, jabki maximum money chahiye aur `max()` use karna hai.

---

## 15. Final Takeaway

House Robber II ka core logic:

1. Circular restriction identify karo.
2. Last house exclude karke ek normal House Robber problem solve karo.
3. First house exclude karke doosri normal House Robber problem solve karo.
4. Dono cases mein Memoization ka Pick / Skip logic use karo.
5. Dono answers mein se maximum return karo.

**Final formula:**

```cpp
int case1 = robRange(nums, 0, n - 2);
int case2 = robRange(nums, 1, n - 1);

return max(case1, case2);
```

**Memoization flow:**

Range choose karo → Base Case → Saved Answer Check → Pick Calculate Karo → Skip Calculate Karo → Maximum Save Karo → Return Karo.
