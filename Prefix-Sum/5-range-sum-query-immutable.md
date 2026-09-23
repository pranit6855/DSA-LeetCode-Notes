# Prefix Sum Pattern

# 02. Range Sum Query - Immutable

## LeetCode 303

---

## Pattern

**Prefix Sum**

---

# 1. Problem Statement

Hume ek integer array `nums` diya gaya hai.

Hume multiple queries milengi jisme hume `left` aur `right` index diya hoga.

Hume `left` se `right` tak ke elements ka sum return karna hai.

Example:

nums = [1, 2, 3, 4, 5]

Query:

`sumRange(1, 3)`

Matlab:

`nums[1] + nums[2] + nums[3]`

`= 2 + 3 + 4`

`= 9`

---

# 2. Simple Language Mein Problem

Simple words mein:

> Mujhe array ke kisi bhi given range ka sum batana hai.

Example:

Array:

`[1, 2, 3, 4, 5]`

Agar query hai:

`left = 1`

`right = 3`

To sirf ye elements chahiye:

`[2, 3, 4]`

Sum:

`2 + 3 + 4 = 9`

---

# 3. Brute Force Approach

Simple approach mein har query ke liye `left` se `right` tak loop chala sakte hain.

Example:

nums = [1, 2, 3, 4, 5]

Query:

`sumRange(1, 3)`

Loop:

`2 + 3 + 4`

Answer:

`9`

Agar bahut saari queries hain, to har baar same elements ko baar-baar add karna padega.

Problem:

**Repeated calculation**

Agar `n` elements aur bahut saari queries hain, to total time kaafi badh sakta hai.

---

# 4. Prefix Sum Se Problem Solve Karna

Hum pehle ek Prefix Sum array bana lenge.

Original:

`nums = [1, 2, 3, 4, 5]`

Prefix Sum:

`prefix = [1, 3, 6, 10, 15]`

Meaning:

`prefix[0] = 1`

`prefix[1] = 1 + 2 = 3`

`prefix[2] = 1 + 2 + 3 = 6`

`prefix[3] = 1 + 2 + 3 + 4 = 10`

`prefix[4] = 1 + 2 + 3 + 4 + 5 = 15`

---

# 5. Prefix Sum Array Ka Meaning

Bahut important:

`prefix[i]`

ka matlab hai:

> Index `0` se index `i` tak ka total sum.

Example:

`prefix[3] = 10`

Matlab:

`nums[0] + nums[1] + nums[2] + nums[3]`

`= 1 + 2 + 3 + 4`

`= 10`

---

# 6. Ab Range Sum Kaise Nikale?

Suppose:

`nums = [1, 2, 3, 4, 5]`

Query:

`sumRange(1, 3)`

Hume chahiye:

`2 + 3 + 4 = 9`

Prefix:

`[1, 3, 6, 10, 15]`

`prefix[3] = 10`

Lekin `10` mein kya included hai?

`1 + 2 + 3 + 4`

Hume chahiye:

`2 + 3 + 4`

Extra:

`1`

Ye extra part remove karne ke liye:

`prefix[0] = 1`

Subtract:

`10 - 1 = 9`

Answer:

`9`

---

# 7. Main Formula

Agar:

`left > 0`

to:

`sum = prefix[right] - prefix[left - 1]`

Ye Prefix Sum ka most important formula hai.

---

# 8. Formula Ko Samjho

Suppose:

`left = 2`

`right = 4`

Array:

`[1, 2, 3, 4, 5]`

Hume chahiye:

`3 + 4 + 5`

Prefix:

`[1, 3, 6, 10, 15]`

Right tak ka total:

`prefix[4] = 15`

Left se just pehle tak ka total:

`prefix[left - 1]`

`= prefix[1]`

`= 3`

Ab:

`15 - 3 = 12`

Answer:

`3 + 4 + 5 = 12`

---

# 9. Visual Understanding

Array:

`[1, 2, 3, 4, 5]`

Query:

`left = 2`

`right = 4`

Required:

`[3, 4, 5]`

Prefix:

`[1, 3, 6, 10, 15]`

Right tak:

`1 + 2 + 3 + 4 + 5 = 15`

Left se pehle:

`1 + 2 = 3`

Subtract:

`15 - 3 = 12`

Remaining:

`3 + 4 + 5 = 12`

So:

**Right tak ka total - Left se pehle ka total = Required Range Sum**

---

# 10. Special Case: left == 0

Agar:

`left = 0`

To formula:

`prefix[left - 1]`

ban jayega:

`prefix[-1]`

Jo invalid hai.

Example:

`sumRange(0, 3)`

Hume chahiye:

`1 + 2 + 3 + 4`

Prefix:

`[1, 3, 6, 10, 15]`

Direct:

`prefix[3] = 10`

Isliye:

`if(left == 0)`

return:

`prefix[right]`

---

# 11. Main Logic

Do cases hain.

## Case 1: left == 0

Answer:

`prefix[right]`

---

## Case 2: left > 0

Answer:

`prefix[right] - prefix[left - 1]`

---

# 12. Code

class NumArray {
public:
    vector<int> prefix;

    NumArray(vector<int>& nums) {
        prefix = nums;

        for(int i = 1; i < prefix.size(); i++) {
            prefix[i] = prefix[i - 1] + prefix[i];
        }
    }

    int sumRange(int left, int right) {
        if(left == 0) {
            return prefix[right];
        }

        return prefix[right] - prefix[left - 1];
    }
};

---

# 13. Code Explanation

## Step 1: Prefix Array

`vector<int> prefix;`

Ek vector banaya jisme Prefix Sum store karenge.

---

## Step 2: Constructor

`NumArray(vector<int>& nums)`

Jab object create hoga, ye constructor call hoga.

Hum:

`prefix = nums;`

karke original array ko copy kar lete hain.

Example:

Original:

`[1, 2, 3, 4, 5]`

Prefix initially:

`[1, 2, 3, 4, 5]`

---

# 14. Prefix Sum Build Karna

Code:

`for(int i = 1; i < prefix.size(); i++)`

Index `1` se start karte hain.

Because index `0` already first prefix sum hai.

Then:

`prefix[i] = prefix[i - 1] + prefix[i];`

---

# 15. Detailed Dry Run: Prefix Build

Input:

`nums = [1, 2, 3, 4, 5]`

Initially:

`prefix = [1, 2, 3, 4, 5]`

---

## i = 1

`prefix[1] = prefix[0] + prefix[1]`

`= 1 + 2`

`= 3`

Array:

`[1, 3, 3, 4, 5]`

---

## i = 2

`prefix[2] = prefix[1] + prefix[2]`

`= 3 + 3`

`= 6`

Array:

`[1, 3, 6, 4, 5]`

---

## i = 3

`prefix[3] = prefix[2] + prefix[3]`

`= 6 + 4`

`= 10`

Array:

`[1, 3, 6, 10, 5]`

---

## i = 4

`prefix[4] = prefix[3] + prefix[4]`

`= 10 + 5`

`= 15`

Final Prefix:

`[1, 3, 6, 10, 15]`

---

# 16. Detailed Dry Run: sumRange()

Suppose query:

`sumRange(1, 3)`

So:

`left = 1`

`right = 3`

Since:

`left != 0`

Formula:

`prefix[right] - prefix[left - 1]`

Put values:

`prefix[3] - prefix[0]`

Prefix:

`[1, 3, 6, 10, 15]`

So:

`10 - 1`

`= 9`

Answer:

`9`

---

# 17. Another Dry Run

Query:

`sumRange(2, 4)`

Required:

`nums[2] + nums[3] + nums[4]`

`= 3 + 4 + 5`

`= 12`

Prefix:

`[1, 3, 6, 10, 15]`

Formula:

`prefix[4] - prefix[1]`

`= 15 - 3`

`= 12`

Answer:

`12`

---

# 18. Another Dry Run: left = 0

Query:

`sumRange(0, 3)`

Required:

`1 + 2 + 3 + 4`

`= 10`

Since:

`left == 0`

Return:

`prefix[3]`

`= 10`

Answer:

`10`

---

# 19. Why Subtract prefix[left - 1]?

This is the most important concept of this question.

Suppose:

`left = 2`

`right = 4`

Prefix:

`[1, 3, 6, 10, 15]`

`prefix[4] = 15`

This represents:

`1 + 2 + 3 + 4 + 5`

But hume chahiye:

`3 + 4 + 5`

Extra part:

`1 + 2`

Ye exactly:

`prefix[1]`

hai.

So:

`15 - 3`

`= 12`

Therefore:

`3 + 4 + 5 = 12`

---

# 20. Core Intuition

Prefix Sum ka use karke hum bade sum ko do parts mein tod dete hain:

`0 → right`

minus

`0 → left - 1`

Remaining:

`left → right`

Mathematically:

`Sum(left, right)`

`= Sum(0, right) - Sum(0, left - 1)`

Prefix Sum mein:

`Sum(left, right)`

`= prefix[right] - prefix[left - 1]`

---

# 21. Why Is This Fast?

Without Prefix Sum:

Har query ke liye:

`left` se `right` tak loop.

Agar range large hai, bahut saare elements add karne padenge.

With Prefix Sum:

Sirf subtraction:

`prefix[right] - prefix[left - 1]`

So each query takes:

`O(1)`

time.

---

# 22. Time Complexity

## Constructor / Prefix Building

Array ko ek baar traverse karna:

`O(n)`

---

## sumRange()

Har query mein sirf subtraction:

`O(1)`

---

## Overall

Preprocessing:

`O(n)`

Each query:

`O(1)`

Agar `q` queries hain:

`O(n + q)`

---

# 23. Space Complexity

Prefix Sum array store kar rahe hain:

`O(n)`

Isliye:

**Space Complexity = O(n)**

---

# 24. LC 1480 vs LC 303

## LC 1480

Question:

Running Sum nikalna.

Main idea:

`prefix[i] = prefix[i - 1] + nums[i]`

---

## LC 303

Question:

Kisi bhi range ka sum quickly nikalna.

Main idea:

`prefix[right] - prefix[left - 1]`

So:

`LC 1480`

teaches:

**Prefix Sum banana**

And:

`LC 303`

teaches:

**Prefix Sum ko range queries ke liye use karna**

---

# 25. Common Mistakes

## Mistake 1

Formula galat likhna:

`prefix[right] - prefix[left]`

Ye generally wrong hai.

Correct:

`prefix[right] - prefix[left - 1]`

---

## Mistake 2

`left == 0` case bhool jana.

Agar:

`left = 0`

to:

`left - 1 = -1`

Invalid index ho jayega.

Correct:

`if(left == 0)`

return:

`prefix[right]`

---

## Mistake 3

Prefix array ko samajhna nahi.

`prefix[i]`

ka matlab:

**0 se i tak ka sum**

Example:

`prefix[3] = nums[0] + nums[1] + nums[2] + nums[3]`

---

# 26. Pattern Recognition

Agar question mein:

- Multiple range sum queries
- Sum from index `L` to `R`
- Range sum
- Query sum
- Sum between two indices

aisa kuch dikhe, to Prefix Sum immediately think karo.

Basic formula:

`prefix[right] - prefix[left - 1]`

---

# 27. Interview Explanation

Agar interviewer puche:

**"How did you solve this?"**

Answer:

"I used Prefix Sum to preprocess the array. I created a prefix array where prefix[i] stores the sum from index 0 to i. Then for every range query, if left is zero, I return prefix[right]. Otherwise, I calculate prefix[right] - prefix[left - 1]. This makes each range query O(1)."

Hinglish version:

"Main pehle Prefix Sum array bana raha hoon jisme har index par 0 se us index tak ka sum store hai. Fir range query ke liye right tak ka total minus left se just pehle tak ka total karta hoon. Isliye har query O(1) mein answer ho jaati hai."

---

# 28. Most Important Formula

For:

`left > 0`

Use:

`prefix[right] - prefix[left - 1]`

For:

`left == 0`

Use:

`prefix[right]`

---

# 29. One-Line Intuition

**Right tak ka pura sum - Left se pehle ka sum = Left se Right tak ka sum**

---

# 30. Final Revision

Array:

`[1, 2, 3, 4, 5]`

Prefix:

`[1, 3, 6, 10, 15]`

Query:

`sumRange(1, 3)`

Formula:

`prefix[3] - prefix[0]`

`= 10 - 1`

`= 9`

Answer:

`9`

---

# 31. Pattern Flow So Far

## Question 1 — LC 1480

**Running Sum of 1d Array**

Learned:

`Previous Prefix + Current Element`

---

## Question 2 — LC 303

**Range Sum Query - Immutable**

Learned:

`prefix[right] - prefix[left - 1]`

---

# 32. Key Takeaways

Remember these two things:

### Prefix Build

`prefix[i] = prefix[i - 1] + nums[i]`

### Range Query

`prefix[right] - prefix[left - 1]`

And special case:

`left == 0`

then:

`prefix[right]`

---

# 33. Final Mental Model

Prefix Sum:

`[1, 2, 3, 4, 5]`

↓

`[1, 3, 6, 10, 15]`

Then:

`Range Sum`

↓

`Right Prefix`

minus

`Before Left Prefix`

↓

`Answer`

Example:

`sumRange(2,4)`

↓

`prefix[4] - prefix[1]`

↓

`15 - 3`

↓

`12`

---

## LeetCode

**303 - Range Sum Query - Immutable**

## Pattern

**Prefix Sum**

## Main Formula

`prefix[right] - prefix[left - 1]`

## Time

`O(n)` preprocessing

`O(1)` per query

## Space

`O(n)`
