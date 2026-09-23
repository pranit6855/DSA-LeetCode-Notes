# Prefix Sum Pattern

# 05. Contiguous Array

## LeetCode 525

---

## Pattern

Prefix Sum + HashMap

---

# 1. Problem Statement

Hume ek binary array `nums` diya hai.

Binary array ka matlab hai array mein sirf:

`0` aur `1`

honge.

Hume **longest contiguous subarray** ki length find karni hai jisme:

`0s ki count = 1s ki count`

Example:

nums = [0,1]

Isme:

`0 ki count = 1`

`1 ki count = 1`

Isliye poora array valid hai.

Answer:

`2`

---

# 2. Simple Language Mein Problem

Simple words mein:

> Hume array ke andar sabse lamba continuous part find karna hai jisme jitne `0` hain utne hi `1` hain.

Example:

nums = [0,1,0,1]

Poora array:

`[0,1,0,1]`

0 ki count:

`2`

1 ki count:

`2`

So answer:

`4`

---

# 3. Subarray Kya Hota Hai?

Subarray ka matlab:

> Continuous elements ka group.

Example:

nums = [0,1,0]

Valid subarrays:

`[0]`

`[1]`

`[0]`

`[0,1]`

`[1,0]`

`[0,1,0]`

Lekin:

`[0,0]`

valid subarray nahi hai because ye continuous positions se nahi bana.

---

# 4. Problem Ko Directly Solve Karna Difficult Kyu Hai?

Hume har possible subarray check karna padega.

Har subarray mein:

`0 ki count`

aur

`1 ki count`

compare karni padegi.

Ye brute force approach expensive ho sakti hai.

Isliye Prefix Sum ka use karenge.

---

# 5. Main Trick

Ye question ka sabse important trick:

`0 → -1`

`1 → +1`

Example:

Original:

`[0,1,0,1]`

Convert:

`[-1,+1,-1,+1]`

Ab socho:

Agar kisi subarray ka sum `0` aa gaya, to iska matlab:

`-1 aur +1 equal quantity mein hain`

Original array mein iska matlab:

`0 aur 1 equal quantity mein hain`

---

# 6. Why 0 → -1 and 1 → +1?

Suppose:

`[0,1]`

Convert:

`[-1,+1]`

Sum:

`-1 + 1 = 0`

Yani:

`1 zero`

and

`1 one`

equal hain.

---

Another example:

`[0,1,0,1]`

Convert:

`[-1,+1,-1,+1]`

Sum:

`-1 + 1 - 1 + 1`

`= 0`

Original array mein:

`2 zeros`

and

`2 ones`

hain.

Isliye valid.

---

# 7. Prefix Sum Ka Role

Ab hum converted array ka Prefix Sum maintain karenge.

Example:

Original:

`[0,1,0,1]`

Converted:

`[-1,+1,-1,+1]`

Prefix Sum:

Index 0:

`-1`

Index 1:

`-1 + 1 = 0`

Index 2:

`0 - 1 = -1`

Index 3:

`-1 + 1 = 0`

So:

`[-1,0,-1,0]`

Notice:

`sum = 0`

Index `1` par aaya.

Phir:

`sum = 0`

Index `3` par dobara aaya.

Same Prefix Sum dobara mil gaya.

Iska matlab in dono positions ke beech ka sum `0` hai.

Aur sum `0` ka matlab:

`equal 0s and 1s`

---

# 8. Same Prefix Sum Ka Concept

Agar:

`prefix[i] == prefix[j]`

to:

`prefix[j] - prefix[i] = 0`

Matlab `i+1` se `j` tak ka subarray sum `0` hai.

Yahan:

`0 → -1`

aur

`1 → +1`

kiya hua hai.

So:

`subarray sum = 0`

means:

`equal number of 0s and 1s`

---

# 9. Example

Converted array:

`[-1,+1,-1,+1]`

Prefix:

`[-1,0,-1,0]`

Index:

`0  1  2  3`

Prefix:

`-1 0 -1 0`

Sum `0`:

First time:

`index = 1`

Second time:

`index = 3`

Between them:

`index 2 to 3`

Subarray:

`[0,1]`

Length:

`2`

---

# 10. HashMap Mein Kya Store Karna Hai?

Yahan HashMap mein:

`prefix sum → first index`

store karenge.

Example:

`sum = 0`

first time index `-1` par maana.

So:

`mp[0] = -1`

Agar baad mein `sum = 0` index `3` par mila:

`length = 3 - (-1)`

`= 4`

---

# 11. mp[0] = -1 Kyu?

Ye bahut important hai.

`-1` actual array ka index nahi hai.

Ye ek imaginary position hai jo array ke start se just pehle hai.

Visual:

`-1 | 0 | 1 | 2`

`   | array starts`

Matlab:

`mp[0] = -1`

ka meaning:

> Prefix Sum `0` ko array start hone se pehle ek baar exist maana.

Isse agar valid subarray index `0` se start hota hai, to uski length correctly calculate ho jaati hai.

---

# 12. Example of mp[0] = -1

nums:

`[0,1]`

Convert:

`[-1,+1]`

Prefix:

Index 0:

`sum = -1`

Index 1:

`sum = 0`

Map mein:

`mp[0] = -1`

already hai.

Current index:

`i = 1`

Length:

`i - mp[sum]`

`= 1 - (-1)`

`= 2`

Valid subarray:

`[0,1]`

Length:

`2`

---

# 13. Length Formula

Sabse important line:

`int length = i - mp[sum];`

Yahan:

`i`

= current index

and:

`mp[sum]`

= same Prefix Sum ka first index

So:

`current index - first index`

gives:

`valid subarray length`

---

# 14. Why Formula Works?

Suppose same Prefix Sum:

first time:

`index = 2`

dobara:

`index = 6`

To valid subarray:

`index 3 to 6`

hai.

Elements:

`3,4,5,6`

Total:

`4 elements`

Formula:

`6 - 2`

`= 4`

Isliye:

`length = currentIndex - firstIndex`

---

# 15. Why First Index Store Karna Hai?

Suppose same Prefix Sum multiple times aaye.

Example:

`sum = 0`

first time:

`index = 1`

second time:

`index = 4`

third time:

`index = 7`

Agar current index `10` hai:

First index use karne par:

`10 - 1 = 9`

Agar latest index use karoge:

`10 - 7 = 3`

Hume longest subarray chahiye.

Isliye:

> Same Prefix Sum ka **first occurrence** hi store karna hai.

Isi wajah se code mein map update sirf `else` mein hota hai.

---

# 16. Algorithm

Har element ke liye:

### Step 1

Agar element `0` hai:

`sum = sum - 1`

Agar element `1` hai:

`sum = sum + 1`

---

### Step 2

Check karo:

`Kya ye Prefix Sum pehle map mein aa chuka hai?`

---

### Step 3

Agar haan:

`length = current index - first index`

Then:

`ans = max(ans, length)`

---

### Step 4

Agar pehli baar aaya:

`mp[sum] = current index`

---

# 17. Code

class Solution {
public:
    int findMaxLength(vector<int>& nums) {

        unordered_map<int, int> mp;

        mp[0] = -1;

        int sum = 0;
        int ans = 0;

        for(int i = 0; i < nums.size(); i++) {

            if(nums[i] == 0) {
                sum = sum - 1;
            }
            else {
                sum = sum + 1;
            }

            if(mp.find(sum) != mp.end()) {

                int length = i - mp[sum];

                ans = max(ans, length);
            }
            else {

                mp[sum] = i;
            }
        }

        return ans;
    }
};

---

# 18. Code Explanation

## Map

`unordered_map<int, int> mp;`

Map mein store karenge:

`Prefix Sum → First Index`

---

## Initial Value

`mp[0] = -1;`

Prefix Sum `0` ko array start hone se pehle index `-1` par store maana.

---

## Variables

`int sum = 0;`

Current Prefix Sum store karega.

`int ans = 0;`

Maximum valid subarray length store karega.

---

# 19. 0 Ko -1 Banana

Code:

`if(nums[i] == 0)`

then:

`sum = sum - 1;`

Because:

`0 → -1`

---

# 20. 1 Ko +1 Banana

Else:

`sum = sum + 1;`

Because:

`1 → +1`

---

# 21. Same Prefix Sum Check

Code:

`if(mp.find(sum) != mp.end())`

Meaning:

> Kya ye Prefix Sum map mein pehle se present hai?

Agar present hai:

`same Prefix Sum`

mil gaya.

So beech ka sum:

`0`

hai.

Therefore:

`equal 0s and 1s`

hain.

---

# 22. Length Calculate Karna

Code:

`int length = i - mp[sum];`

Example:

Current index:

`i = 5`

Same Prefix Sum first mila tha:

`mp[sum] = 2`

So:

`length = 5 - 2`

`= 3`

Valid subarray:

`index 3 to 5`

Total:

`3 elements`

---

# 23. Maximum Length

Code:

`ans = max(ans, length);`

Har valid subarray ki length compare karte hain.

Agar current length bigger hai:

`ans`

update ho jayega.

Example:

`ans = 4`

Current length:

`6`

Then:

`ans = 6`

---

# 24. First Occurrence Store Karna

Code:

`else`

`mp[sum] = i;`

Sirf tab store karenge jab Prefix Sum pehli baar mila ho.

Agar already present hai, update nahi karenge.

Reason:

**Earliest index gives longest possible subarray.**

---

# 25. Detailed Dry Run

Input:

`nums = [0,1,0,1]`

Initially:

`sum = 0`

`ans = 0`

Map:

`0 → -1`

---

## i = 0

Value:

`0`

So:

`sum = 0 - 1`

`sum = -1`

Map mein `-1` nahi hai.

Store:

`-1 → 0`

Map:

`0 → -1`

`-1 → 0`

Answer:

`0`

---

## i = 1

Value:

`1`

So:

`sum = -1 + 1`

`sum = 0`

Map mein `0` already hai.

`mp[0] = -1`

Length:

`1 - (-1)`

`= 2`

Answer:

`max(0,2) = 2`

---

## i = 2

Value:

`0`

So:

`sum = 0 - 1`

`sum = -1`

Map mein `-1` already hai.

`mp[-1] = 0`

Length:

`2 - 0`

`= 2`

Answer:

`max(2,2) = 2`

Map update nahi karenge.

Because first occurrence `0` better hai.

---

## i = 3

Value:

`1`

So:

`sum = -1 + 1`

`sum = 0`

Map:

`mp[0] = -1`

Length:

`3 - (-1)`

`= 4`

Answer:

`max(2,4) = 4`

Final answer:

`4`

---

# 26. Dry Run Table

| i | nums[i] | sum | Map mein mila? | Length | ans |
|---|---:|---:|---|---:|---:|
| Start | - | 0 | `0 → -1` | - | 0 |
| 0 | 0 | -1 | No | - | 0 |
| 1 | 1 | 0 | Yes, first at -1 | 2 | 2 |
| 2 | 0 | -1 | Yes, first at 0 | 2 | 2 |
| 3 | 1 | 0 | Yes, first at -1 | 4 | 4 |

Final:

`4`

---

# 27. Most Important Concept

Is question ka pura logic:

`0 → -1`

`1 → +1`

Then:

`Prefix Sum`

Then:

`Same Prefix Sum`

Then:

`Subarray Sum = 0`

Then:

`Equal number of 0s and 1s`

Then:

`Current Index - First Index`

Then:

`Maximum Length`

---

# 28. LC 560 Se Connection

LC 560 mein humne kiya:

`currSum - k`

Yahan:

`same prefix sum`

reason:

Agar:

`prefix[j] = prefix[i]`

then:

`prefix[j] - prefix[i] = 0`

So subarray sum `0`.

---

# 29. LC 525 Mein HashMap Kya Store Kar Raha Hai?

Important difference:

LC 560:

`prefix sum → frequency`

LC 974:

`remainder → frequency`

LC 525:

`prefix sum → first index`

Why?

Because LC 525 mein hume:

**count nahi, longest length**

chahiye.

---

# 30. Common Mistakes

## Mistake 1

0 ko 0 hi rakhna.

Wrong:

`0 → 0`

Correct:

`0 → -1`

---

## Mistake 2

1 ko 1 ki jagah kuch aur karna.

Correct:

`1 → +1`

---

## Mistake 3

`mp[0] = 0` kar dena.

Correct:

`mp[0] = -1`

Because `-1` array ke start se just pehle imaginary position hai.

---

## Mistake 4

Same Prefix Sum aane par map update karna.

Wrong:

`mp[sum] = i`

har baar.

Correct:

Sirf first time:

`else`

`mp[sum] = i`

---

## Mistake 5

Length formula galat karna.

Correct:

`length = i - mp[sum]`

---

# 31. Complexity

## Time Complexity

`O(n)`

Array ko ek baar traverse karte hain.

HashMap lookup average `O(1)` hai.

---

## Space Complexity

`O(n)`

Worst case mein map mein `n` different Prefix Sums store ho sakte hain.

---

# 32. Interview Explanation

Agar interviewer puche:

"How did you solve this?"

Simple answer:

> I converted every `0` into `-1` and every `1` into `+1`. Now a subarray having equal number of zeros and ones will have sum zero. I use Prefix Sum and store the first index of every Prefix Sum in a HashMap. Whenever the same Prefix Sum appears again, the subarray between those indices has equal zeros and ones. I calculate its length and keep the maximum.

Hinglish:

> Main `0` ko `-1` aur `1` ko `+1` treat karta hoon. Isse equal zeros aur ones wala subarray ka sum `0` ho jaata hai. Main Prefix Sum ka first index HashMap mein store karta hoon. Agar same Prefix Sum dobara milta hai, to unke beech ka subarray valid hai. Uski length `current index - first index` se nikalta hoon aur maximum length store karta hoon.

---

# 33. One-Line Formula

`0 → -1`

`1 → +1`

`Same Prefix Sum → Valid`

`Length = Current Index - First Index`

---

# 34. Why mp[0] = -1?

Remember this visually:

`-1 | 0 | 1 | 2`

`    ↑`

Array yahan se start hota hai.

`mp[0] = -1`

means:

**Prefix Sum 0 ko array start hone se just pehle maana hai.**

Isse:

`[0,1]`

jaise subarray ki length:

`1 - (-1) = 2`

correctly milti hai.

---

# 35. Final Mental Model

`Binary Array`

↓

`0 → -1`

`1 → +1`

↓

`Prefix Sum`

↓

`Same Prefix Sum`

↓

`Subarray Sum = 0`

↓

`Equal 0s and 1s`

↓

`Current Index - First Index`

↓

`Maximum Length`

---

# 36. Prefix Sum Roadmap Progress

## 01. LC 1480 — Running Sum of 1d Array

Basic Prefix Sum

Done

---

## 02. LC 303 — Range Sum Query - Immutable

Prefix Sum + Range Query

Done

---

## 03. LC 560 — Subarray Sum Equals K

Prefix Sum + HashMap

Done

---

## 04. LC 974 — Subarray Sums Divisible by K

Prefix Sum + Remainder + HashMap

Done

---

## 05. LC 525 — Contiguous Array

0 → -1 / 1 → +1

Prefix Sum + First Index

Done

---

# Important Concepts Learned So Far

`Prefix Sum`

`Range Sum`

`Prefix Sum + HashMap`

`Current Sum - Target`

`Same Remainder`

`0 → -1`

`1 → +1`

`Same Prefix Sum`

`First Index`

`Subarray Length`

---

# Final Takeaway

LC 525 ka core idea:

**Equal 0s and 1s ko directly count karne ke bajaye 0 ko -1 aur 1 ko +1 bana do.**

Then:

**Same Prefix Sum dobara milna = beech mein sum 0 = equal 0s and 1s.**

Longest ke liye:

**First occurrence store karo.**

Length:

`current index - first occurrence index`

Maximum:

`ans = max(ans, length)`

## LeetCode

525 - Contiguous Array

## Pattern

Prefix Sum + HashMap

## Time

O(n)

## Space

O(n)
