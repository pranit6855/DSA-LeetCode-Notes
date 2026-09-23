# Prefix Sum Pattern

# 01. Running Sum of 1d Array

## LeetCode 1480

---

## Pattern

**Prefix Sum**

---

# 1. Problem Statement

Hume ek integer array `nums` diya gaya hai.

Hume har index tak ka total sum nikalna hai.

Isko **Running Sum** kehte hain.

Agar array hai:

`[1, 2, 3, 4]`

To:

- Index 0 tak sum = `1`
- Index 1 tak sum = `1 + 2 = 3`
- Index 2 tak sum = `1 + 2 + 3 = 6`
- Index 3 tak sum = `1 + 2 + 3 + 4 = 10`

Isliye answer hoga:

`[1, 3, 6, 10]`

---

# 2. Simple Language Mein Problem

Simple words mein:

> Har element ke liye us element tak ka total sum nikalna hai.

Example:

`nums = [2, 5, 3, 7]`

To:

`2`

`2 + 5 = 7`

`2 + 5 + 3 = 10`

`2 + 5 + 3 + 7 = 17`

Final answer:

`[2, 7, 10, 17]`

---

# 3. Prefix Sum Kya Hota Hai?

Prefix Sum ka simple meaning:

> Kisi index tak jitna sum bana hai, usko store karna.

Example:

`nums = [2, 5, 3, 7]`

Prefix Sum:

`[2, 7, 10, 17]`

Kyuki:

- `2` → first element ka sum
- `2 + 5 = 7`
- `2 + 5 + 3 = 10`
- `2 + 5 + 3 + 7 = 17`

So:

**Prefix Sum = Current index tak ka total sum**

---

# 4. Main Formula

Prefix Sum ka basic formula:

`prefix[i] = prefix[i - 1] + nums[i]`

Matlab:

**Previous Prefix Sum + Current Element = Current Prefix Sum**

Example:

`nums = [1, 2, 3, 4]`

At `i = 1`:

`prefix[1] = prefix[0] + nums[1]`

`= 1 + 2`

`= 3`

At `i = 2`:

`prefix[2] = prefix[1] + nums[2]`

`= 3 + 3`

`= 6`

At `i = 3`:

`prefix[3] = prefix[2] + nums[3]`

`= 6 + 4`

`= 10`

---

# 5. Important Observation

Sabse important cheez:

Hume har baar beginning se sum calculate karne ki zarurat nahi hai.

Example:

Agar hume index 3 ka sum chahiye:

Normal way:

`1 + 2 + 3 + 4`

Lekin Prefix Sum mein:

`Previous Prefix Sum + Current Element`

`6 + 4 = 10`

Yani hum previous calculation ko reuse kar rahe hain.

Yehi Prefix Sum ka main idea hai.

---

# 6. Code

class Solution {
public:
    vector<int> runningSum(vector<int>& nums) {

        for(int i = 1; i < nums.size(); i++) {
            nums[i] = nums[i - 1] + nums[i];
        }

        return nums;
    }
};

---

# 7. Code Ka Simple Explanation

## Step 1

Loop `i = 1` se start kiya:

`for(int i = 1; i < nums.size(); i++)`

Index `0` se start nahi kiya kyuki first element khud hi apna prefix sum hai.

Example:

`nums = [1, 2, 3, 4]`

Index `0`:

`1`

Yaha previous element available hi nahi hai.

Isliye `i = 1` se start karte hain.

---

# 8. Main Logic Wali Line

Ye sabse important line hai:

`nums[i] = nums[i - 1] + nums[i];`

Iska matlab:

**Current element ko previous prefix sum ke saath add karo.**

Example:

`nums = [1, 2, 3, 4]`

Initially:

`[1, 2, 3, 4]`

---

# 9. Detailed Dry Run

## Initial Array

`[1, 2, 3, 4]`

---

## i = 1

Current element:

`nums[1] = 2`

Previous element:

`nums[0] = 1`

Formula:

`nums[1] = nums[0] + nums[1]`

`= 1 + 2`

`= 3`

Array becomes:

`[1, 3, 3, 4]`

Ab important baat:

`nums[1]` ab `2` nahi hai.

Ab `nums[1] = 3` hai.

Aur ye `index 1` ka Prefix Sum hai.

---

## i = 2

Current element:

`nums[2] = 3`

Previous prefix sum:

`nums[1] = 3`

Formula:

`nums[2] = nums[1] + nums[2]`

`= 3 + 3`

`= 6`

Array:

`[1, 3, 6, 4]`

Ab:

`nums[2] = 6`

Ye index 2 tak ka Prefix Sum hai.

---

## i = 3

Current element:

`nums[3] = 4`

Previous prefix sum:

`nums[2] = 6`

Formula:

`nums[3] = nums[2] + nums[3]`

`= 6 + 4`

`= 10`

Array:

`[1, 3, 6, 10]`

---

# 10. Final Answer

`[1, 3, 6, 10]`

---

# 11. Dry Run Table

| i | Current Array | Calculation | Updated Array |
|---|---|---|---|
| Start | `[1,2,3,4]` | - | `[1,2,3,4]` |
| 1 | `nums[1] = 2` | `1 + 2 = 3` | `[1,3,3,4]` |
| 2 | `nums[2] = 3` | `3 + 3 = 6` | `[1,3,6,4]` |
| 3 | `nums[3] = 4` | `6 + 4 = 10` | `[1,3,6,10]` |

Final:

`[1,3,6,10]`

---

# 12. Why Is nums[i - 1] Already a Prefix Sum?

Ye bahut important concept hai.

Initially:

`nums = [1,2,3,4]`

Jab `i = 1`:

`nums[1] = nums[0] + nums[1]`

`nums[1] = 1 + 2 = 3`

Ab array:

`[1,3,3,4]`

Ab jab `i = 2` aaya:

`nums[1]` ki value `2` nahi hai.

Wo already `3` ban chuki hai.

Aur `3` kya hai?

`1 + 2`

Yani previous Prefix Sum.

Isliye:

`nums[2] = nums[1] + nums[2]`

`= 3 + 3`

`= 6`

Isi tarah har next index par previous value already prefix sum hoti hai.

---

# 13. In-Place Prefix Sum

Humne alag Prefix Sum array nahi banaya.

Example:

Normal approach mein:

`nums = [1,2,3,4]`

Aur:

`prefix = [1,3,6,10]`

Do arrays hote.

Lekin humne original `nums` ko hi modify kar diya:

`nums = [1,3,6,10]`

Isko **In-Place Prefix Sum** bol sakte hain.

Isliye extra array ki zarurat nahi padi.

---

# 14. Brute Force Approach

Agar hume har index ka sum independently calculate karna hota:

`[1,2,3,4]`

To:

Index 0:

`1`

Index 1:

`1 + 2`

Index 2:

`1 + 2 + 3`

Index 3:

`1 + 2 + 3 + 4`

Problem ye hai ki baar-baar same elements add ho rahe hain.

Prefix Sum mein hum previous answer reuse karte hain.

Instead of:

`1 + 2 + 3`

Hum karte hain:

`Previous Sum + Current Element`

`3 + 3 = 6`

---

# 15. Why Prefix Sum Is Better?

Prefix Sum mein:

`Previous Prefix Sum + Current Element`

Bas ek addition karna hai.

Array ko sirf ek baar traverse karte hain.

Isliye:

**Time Complexity = O(n)**

---

# 16. Complexity

## Time Complexity

`O(n)`

Kyuki array ko ek baar traverse kar rahe hain.

Agar `n` elements hain, to approximately `n` operations hongi.

---

## Space Complexity

`O(1)`

Kyuki humne koi extra array nahi banaya.

Original array ko hi modify kiya hai.

---

# 17. Pattern Recognition

Future mein agar question mein ye words ya idea dikhe:

- Running Sum
- Cumulative Sum
- Sum till index
- Sum from beginning
- Sum of elements from start
- Range Sum
- Subarray Sum

To Prefix Sum ke baare mein sochna.

Basic Prefix Sum formula:

`prefix[i] = prefix[i - 1] + nums[i]`

---

# 18. Prefix Sum Ka Core Template

Basic Prefix Sum:

`prefix[0] = nums[0]`

Then:

`prefix[i] = prefix[i - 1] + nums[i]`

Ya agar original array ko modify karna ho:

`nums[i] = nums[i - 1] + nums[i]`

---

# 19. Interview Mein Kaise Explain Karna Hai?

Agar interviewer puche:

**"How did you solve this problem?"**

Simple answer:

"I used the Prefix Sum approach. The first element is already its own prefix sum, so I start from index 1. For every index, I add the current element to the previous prefix sum and store the result in the same array. This gives O(n) time complexity and O(1) extra space."

Hinglish mein:

"Main Prefix Sum use kar raha hoon. Index 0 already apna prefix sum hai, isliye index 1 se start karunga. Har index par previous prefix sum mein current element add karke wahi value array mein store kar dunga. Isse O(n) time aur O(1) extra space lagega."

---

# 20. Common Confusion

## Q1. i = 0 se loop kyu nahi?

Kyuki index 0 ka koi previous element nahi hota.

`nums[i - 1]`

Agar `i = 0` hua:

`nums[-1]`

Invalid ho jayega.

Isliye:

`i = 1`

---

## Q2. nums[i - 1] original value hai?

Starting mein hoti hai.

Lekin jaise-jaise loop chalta hai, previous positions prefix sum ban jaati hain.

Example:

`[1,2,3,4]`

After `i = 1`:

`[1,3,3,4]`

Ab `nums[1] = 3` hai.

Ye previous prefix sum hai.

---

## Q3. Kya original array change ho raha hai?

Haan.

Hum:

`nums[i] = nums[i - 1] + nums[i]`

use kar rahe hain.

Isliye original values overwrite ho rahi hain.

---

## Q4. Extra array kyu nahi banaya?

Kyuki same array ko prefix array ki tarah use kar sakte hain.

Isse space:

`O(1)`

ho jaata hai.

---

# 21. Example 2

Input:

`nums = [5, 2, 1, 3]`

Start:

`[5,2,1,3]`

i = 1:

`5 + 2 = 7`

`[5,7,1,3]`

i = 2:

`7 + 1 = 8`

`[5,7,8,3]`

i = 3:

`8 + 3 = 11`

`[5,7,8,11]`

Answer:

`[5,7,8,11]`

---

# 22. Example 3

Input:

`nums = [10, -2, 5, -3]`

Start:

`[10,-2,5,-3]`

i = 1:

`10 + (-2) = 8`

`[10,8,5,-3]`

i = 2:

`8 + 5 = 13`

`[10,8,13,-3]`

i = 3:

`13 + (-3) = 10`

`[10,8,13,10]`

Answer:

`[10,8,13,10]`

Prefix Sum negative numbers ke saath bhi same tarike se work karta hai.

---

# 23. Most Important Concept From This Question

Is question se hume Prefix Sum ka basic pattern yaad hona chahiye:

`Previous Prefix Sum + Current Value`

Visual:

`Previous Prefix Sum`
             +
`Current Element`
             ↓
`Current Prefix Sum`

Example:

`6 + 4 = 10`

So:

`[1,3,6,10]`

---

# 24. What We Learn From LC 1480

Is question se 5 important cheezein seekhi:

1. Prefix Sum kya hota hai
2. Previous prefix sum ko reuse kaise karte hain
3. In-place prefix sum kaise banate hain
4. O(n) time mein solve kaise karte hain
5. O(1) extra space kaise maintain karte hain

---

# 25. Future Prefix Sum Problems

Ye basic concept aage bahut important hoga.

Hum gradually dekhenge:

`Prefix Sum`
        ↓
`Range Sum`
        ↓
`Prefix Sum + HashMap`
        ↓
`Prefix Sum + Remainder`
        ↓
`Prefix Sum + Frequency`
        ↓
`Prefix Sum + 0/-1 Transformation`

Isi foundation par:

- LC 303
- LC 560
- LC 974
- LC 525
- LC 930

jaise questions solve karenge.

---

# 26. Final Revision

Problem:

**LeetCode 1480 - Running Sum of 1d Array**

Pattern:

**Prefix Sum**

Core formula:

`prefix[i] = prefix[i - 1] + nums[i]`

In-place formula:

`nums[i] = nums[i - 1] + nums[i]`

Loop:

`for(int i = 1; i < nums.size(); i++)`

Time:

`O(n)`

Space:

`O(1)`

Example:

Input:

`[1,2,3,4]`

Output:

`[1,3,6,10]`

---

# One Line To Remember

**Prefix Sum = Previous Prefix Sum + Current Element**

`nums[i] = nums[i - 1] + nums[i]`

Ye Prefix Sum pattern ka **basic foundation** hai.
