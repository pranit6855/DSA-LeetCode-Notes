# DP #4 — LeetCode 198: House Robber

## 1. Problem Statement

Ek street mein multiple houses hain. Har house mein kuch amount of money rakha hai.

Humein maximum money rob karna hai, lekin ek restriction hai:

**Do adjacent houses ko ek saath rob nahi kar sakte**, warna alarm baj jayega.

Humein maximum amount return karna hai jo bina adjacent houses rob kiye mil sakta hai.

### Example

Input:

```cpp
nums = [2, 7, 9, 3, 1];
```

Output:

```cpp
12
```

Explanation:

Hum House 0, House 2 aur House 4 rob kar sakte hain.

```text
House 0 = 2
House 2 = 9
House 4 = 1

Total = 2 + 9 + 1
Total = 12
```

Koi bhi do adjacent houses rob nahi hue.

Isliye maximum amount `12` hai.

---

## 2. Pattern

**Pattern 1 — 1D Dynamic Programming**

Is question mein hum seekhenge:

- Pick / Skip pattern
- Recursion
- Memoization (Top-Down DP)
- DP State
- DP Transition
- Maximum value calculate karna

---

## 3. Main Logic — Pick or Skip

Har house par humare paas do choices hoti hain.

### Choice 1: Pick

Current house ko rob kar lo.

Agar current house ko rob kiya, toh uske adjacent previous house ko rob nahi kar sakte.

Example:

```text
nums = [2, 7, 9, 3, 1]
```

Agar House 4 pick karenge, toh House 3 pick nahi kar sakte.

Isliye House 4 pick karne ke baad seedha House 2 tak ka maximum answer lenge.

Formula:

```cpp
nums[i] + solve(i - 2, nums, dp)
```

Yahan:
- `nums[i]` = current house ka money.
- `solve(i - 2, nums, dp)` = current house ke previous non-adjacent houses se maximum money.

### Choice 2: Skip

Current house ko chhod do aur previous house tak ka maximum answer le lo.

Formula:

```cpp
solve(i - 1, nums, dp)
```

### Dono mein se maximum choose karo

```cpp
dp[i] = max(
    nums[i] + solve(i - 2, nums, dp),
    solve(i - 1, nums, dp)
);
```

Ye House Robber ka main DP transition hai.

---

## 4. DP State

Hum define karenge:

```text
solve(i) = index 0 se index i tak ke houses se milne wala maximum money,
           bina kisi do adjacent houses ko rob kiye.
```

Example:

```text
nums = [2, 7, 9, 3, 1]
Index =  0  1  2  3  4
```

Agar `solve(4)` call karte hain, toh hum saare houses consider karenge aur maximum possible money nikalenge.

---

## 5. Base Cases

### Case 1: Koi house nahi bacha

```cpp
if (i < 0) {
    return 0;
}
```

Agar index negative ho gaya, toh koi house available nahi hai.

Isliye maximum money `0` return karenge.

### Case 2: Sirf pehla house bacha

```cpp
if (i == 0) {
    return nums[0];
}
```

Agar sirf House 0 consider kar rahe hain, toh usko rob karke uske saare paise le sakte hain.

Example:

```text
nums[0] = 2

solve(0) = 2
```

---

## 6. Memoization Kya Hai?

Memoization mein hum calculated answers ko `dp` array mein save karte hain.

Agar kisi index ka answer pehle calculate ho chuka hai, toh usko dobara calculate karne ke bajaye saved answer return kar dete hain.

### Step 1: DP Array

```cpp
vector<int> dp(n, -1);
```

Agar:

```cpp
nums = [2, 7, 9, 3, 1];
```

Toh initially:

```text
Index:  0   1   2   3   4
dp:    -1  -1  -1  -1  -1
```

`-1` ka matlab hai answer abhi calculate nahi hua.

### Step 2: Saved Answer Check

```cpp
if (dp[i] != -1) {
    return dp[i];
}
```

Agar `dp[i]` mein answer already saved hai, toh usko directly return kar denge.

### Step 3: Answer Calculate Aur Save Karo

```cpp
int pick = nums[i] + solve(i - 2, nums, dp);
int skip = solve(i - 1, nums, dp);

dp[i] = max(pick, skip);
```

Dono options calculate karke maximum answer `dp[i]` mein save kar denge.

---

## 7. C++ Code — Memoization

```cpp
class Solution {
public:
    int solve(int i, vector<int>& nums, vector<int>& dp) {
        if (i < 0) {
            return 0;
        }

        if (i == 0) {
            return nums[0];
        }

        if (dp[i] != -1) {
            return dp[i];
        }

        int pick = nums[i] + solve(i - 2, nums, dp);
        int skip = solve(i - 1, nums, dp);

        dp[i] = max(pick, skip);

        return dp[i];
    }

    int rob(vector<int>& nums) {
        int n = nums.size();
        vector<int> dp(n, -1);

        return solve(n - 1, nums, dp);
    }
};
```

---

## 8. Code Explanation

### A. `solve()` Function

```cpp
int solve(int i, vector<int>& nums, vector<int>& dp)
```

- `i` = last house ka index jise consider kar rahe hain.
- `nums` = har house mein kitna money hai.
- `dp` = calculated answers save karne wala vector.

### B. Base Case — No House

```cpp
if (i < 0) {
    return 0;
}
```

Koi house nahi bacha, toh money `0` hai.

### C. Base Case — First House

```cpp
if (i == 0) {
    return nums[0];
}
```

Agar sirf House 0 hai, toh uske paise return karenge.

### D. Saved Answer Check

```cpp
if (dp[i] != -1) {
    return dp[i];
}
```

Agar answer already calculate ho chuka hai, toh recursion dobara nahi chalani padegi.

### E. Pick Option

```cpp
int pick = nums[i] + solve(i - 2, nums, dp);
```

Current house ke paise lete hain.

Adjacent previous house nahi le sakte, isliye `i - 2` par jaate hain.

### F. Skip Option

```cpp
int skip = solve(i - 1, nums, dp);
```

Current house ko chhod dete hain.

Ab previous house tak ka maximum money consider karte hain.

### G. Maximum Choose Karo

```cpp
dp[i] = max(pick, skip);
```

Pick aur skip ke answers mein se maximum ko `dp[i]` mein save karte hain.

### H. Answer Return

```cpp
return dp[i];
```

Calculated maximum money return karte hain.

### I. Main Function

```cpp
int rob(vector<int>& nums) {
    int n = nums.size();
    vector<int> dp(n, -1);

    return solve(n - 1, nums, dp);
}
```

Pehle total houses ki quantity nikalte hain.

`solve(n - 1, nums, dp)` last house se start karta hai aur index 0 tak ke saare houses consider karta hai.

---

## 9. Detailed Dry Run

Input:

```cpp
nums = [2, 7, 9, 3, 1];
```

Index:

```text
Index:  0  1  2  3  4
Money:  2  7  9  3  1
```

Initially:

```text
n = 5
dp = [-1, -1, -1, -1, -1]
```

Main function call karega:

```cpp
solve(n - 1, nums, dp);
```

Yaani:

```cpp
solve(4, nums, dp);
```

### Step 1: `solve(4)`

Current house:

```text
nums[4] = 1
```

Pick formula:

```cpp
int pick = nums[4] + solve(2, nums, dp);
```

Calculation abhi pending hai, kyunki `solve(2)` ka answer chahiye.

Skip formula:

```cpp
int skip = solve(3, nums, dp);
```

Pehle `solve(2)` calculate karte hain.

### Step 2: `solve(2)`

Current house:

```text
nums[2] = 9
```

Pick:

```cpp
int pick = nums[2] + solve(0, nums, dp);
```

Matlab House 2 ke paise lenge, aur adjacent House 1 ko skip karenge.

`solve(0)` base case hai:

```text
solve(0) = nums[0] = 2
```

Ab pick calculate karo:

```text
pick = 9 + 2
pick = 11
```

House 2 aur House 0 se total `11` milte hain.

Ab skip:

```cpp
int skip = solve(1, nums, dp);
```

### Step 3: `solve(1)`

Current house:

```text
nums[1] = 7
```

Pick:

```cpp
int pick = nums[1] + solve(-1, nums, dp);
```

`solve(-1)` base case hai:

```text
solve(-1) = 0
```

Isliye:

```text
pick = 7 + 0
pick = 7
```

Ab skip:

```cpp
int skip = solve(0, nums, dp);
```

Base case se:

```text
skip = 2
```

Ab maximum:

```cpp
dp[1] = max(pick, skip);
```

Calculation:

```text
dp[1] = max(7, 2)
dp[1] = 7
```

Answer save:

```text
dp = [-1, 7, -1, -1, -1]
```

`solve(1)` return karega `7`.

### Step 4: Wapas `solve(2)` par

Ab `solve(2)` ke paas:

```text
pick = 11
skip = 7
```

Maximum:

```cpp
dp[2] = max(pick, skip);
```

Calculation:

```text
dp[2] = max(11, 7)
dp[2] = 11
```

Answer save:

```text
dp = [-1, 7, 11, -1, -1]
```

`solve(2)` return karega `11`.

**Meaning:** House 0 se House 2 tak maximum money `11` mil sakta hai.

### Step 5: Wapas `solve(4)` par

Ab `solve(4)` ke pick option mein:

```text
nums[4] = 1
solve(2) = 11
```

Calculation:

```text
pick = 1 + 11
pick = 12
```

Ab `solve(3)` calculate karna hai, jo skip option hai.

### Step 6: `solve(3)`

Current house:

```text
nums[3] = 3
```

Pick:

```cpp
int pick = nums[3] + solve(1, nums, dp);
```

`dp[1] = 7` pehle se saved hai.

Condition:

```cpp
if (dp[i] != -1) {
    return dp[i];
}
```

Check:

```text
dp[1] != -1
7 != -1 → True
```

Isliye `solve(1)` dobara calculate nahi hoga. Direct `7` return hoga.

Calculation:

```text
pick = 3 + 7
pick = 10
```

Ab skip:

```cpp
int skip = solve(2, nums, dp);
```

`dp[2] = 11` saved hai, isliye direct `11` return hoga.

Ab:

```text
pick = 10
skip = 11
```

Maximum:

```cpp
dp[3] = max(pick, skip);
```

Calculation:

```text
dp[3] = max(10, 11)
dp[3] = 11
```

Ab:

```text
dp = [-1, 7, 11, 11, -1]
```

`solve(3)` return karega `11`.

### Step 7: Wapas `solve(4)` par

Ab `solve(4)` ke dono options mil gaye:

```text
pick = 12
skip = 11
```

Maximum:

```cpp
dp[4] = max(pick, skip);
```

Calculation:

```text
dp[4] = max(12, 11)
dp[4] = 12
```

Final DP array:

```text
dp = [-1, 7, 11, 11, 12]
```

`solve(4)` return karega:

```text
12
```

### Final Output

```cpp
return solve(n - 1, nums, dp);
```

Output:

```text
12
```

---

## 10. Complete Dry Run Table

| Function | Pick | Skip | Saved Answer |
|---|---:|---:|---:|
| `solve(0)` | Base case | — | 2 return |
| `solve(1)` | 7 | 2 | `dp[1] = 7` |
| `solve(2)` | 11 | 7 | `dp[2] = 11` |
| `solve(3)` | 10 | 11 | `dp[3] = 11` |
| `solve(4)` | 12 | 11 | `dp[4] = 12` |

Final answer = `12`.

---

## 11. Pick Aur Skip Ka Difference

### Pick

```cpp
int pick = nums[i] + solve(i - 2, nums, dp);
```

Current house ke paise lete hain.

Isliye adjacent previous house skip karna padta hai, aur `i - 2` par jaate hain.

### Skip

```cpp
int skip = solve(i - 1, nums, dp);
```

Current house ko chhod dete hain.

Isliye previous house ko consider kar sakte hain, aur `i - 1` par jaate hain.

### Maximum

```cpp
dp[i] = max(pick, skip);
```

Dono decisions ke answers mein se maximum choose karte hain.

---

## 12. Time and Space Complexity

### Time Complexity: O(n)

Memoization mein har index ka answer maximum ek baar calculate hota hai.

Calculated answers dobara calculate nahi hote.

### Auxiliary Space Complexity: O(n)

- DP array: `O(n)`
- Recursion stack: `O(n)` worst case

Overall auxiliary space `O(n)` hai.

---

## 13. Important Interview Points

1. **Pattern:** 1D DP.
2. Har house par do choices hoti hain: Pick ya Skip.
3. Pick karne par adjacent house nahi le sakte, isliye `i - 2` par jaate hain.
4. Skip karne par `i - 1` par jaate hain.
5. `max(pick, skip)` maximum money select karta hai.
6. `dp[i]` calculated maximum answer save karta hai.
7. Base cases: `i < 0` par `0`, aur `i == 0` par `nums[0]`.
8. Memoization repeated calculations avoid karti hai.

---

## 14. Common Mistakes

- Pick karne par `solve(i - 1)` call karna. Isse adjacent house bhi include ho sakta hai.
- Pick aur Skip ko ulta samajhna.
- `max()` ki jagah `min()` use karna.
- Base cases galat likhna.
- Calculated answer `dp[i]` mein save nahi karna.
- DP array ko `-1` se initialize karna bhool jaana.

---

## 15. Final Takeaway

House Robber ka core logic:

1. Current house ko Pick kar sakte hain.
2. Pick karne par previous adjacent house skip hoga.
3. Current house ko Skip bhi kar sakte hain.
4. Pick aur Skip mein se maximum answer choose karenge.
5. Calculated answers ko DP array mein save karenge.

Formula:

```cpp
int pick = nums[i] + solve(i - 2, nums, dp);
int skip = solve(i - 1, nums, dp);

dp[i] = max(pick, skip);
```

**Memoization flow:**

Recursion → Base Case → Saved Answer Check → Pick Calculate Karo → Skip Calculate Karo → Maximum Save Karo → Return Karo.
