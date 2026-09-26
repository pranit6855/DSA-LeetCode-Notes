# 07 - Subsets II

## LeetCode 90 - Subsets II

### Problem

Given an integer array `nums` that may contain duplicate elements, return all possible subsets.

The answer must not contain duplicate subsets.

Example:

Input:

[1,2,2]

Output:

[
  [],
  [1],
  [2],
  [1,2],
  [2,2],
  [1,2,2]
]

---

# Simple Understanding

Ye problem exactly LC 78 - Subsets jaisa hai.

Difference bas itna hai:

LC 78 me elements unique the.

Example:

[1,2,3]

LC 90 me duplicates ho sakte hain.

Example:

[1,2,2]

Problem:

Agar hum normal subsets generate karenge, to same subset multiple times aa sakta hai.

For example:

[1,2]

do baar generate ho sakta hai because array me 2 do baar hai.

Hume duplicate subsets remove karne hain.

---

# Main Idea

Is problem me 3 important cheezein hain:

1. Array ko sort karo.
2. Backtracking se subsets banao.
3. Same recursion level par duplicate element ko skip karo.

Sorting ke baad:

[2,1,2]

ban jayega:

[1,2,2]

Ab duplicate elements paas-paas aa gaye.

---

# Code

class Solution {
public:
    vector<vector<int>> ans;

    void solve(vector<int>& nums, int index, vector<int>& current) {

        ans.push_back(current);

        for(int i = index; i < nums.size(); i++) {

            if(i > index && nums[i - 1] == nums[i]) {
                continue;
            }

            current.push_back(nums[i]);

            solve(nums, i + 1, current);

            current.pop_back();
        }
    }

    vector<vector<int>> subsetsWithDup(vector<int>& nums) {

        vector<int> current;

        sort(nums.begin(), nums.end());

        solve(nums, 0, current);

        return ans;
    }
};

---

# Function Explanation

## 1. ans

vector<vector<int>> ans;

Isme saare unique subsets store honge.

Example:

[
    [],
    [1],
    [1,2],
    [2],
    [2,2]
]

---

# 2. solve()

void solve(vector<int>& nums, int index, vector<int>& current)

Isme 3 cheezein hain:

### nums

Original array.

### index

Ab hum array me kis index se elements choose kar sakte hain.

### current

Abhi jo subset build ho raha hai.

Example:

nums = [1,2,2]

current = [1,2]

index = 2

Matlab:

[1,2] already ban gaya hai aur ab index 2 se aage choices check karni hain.

---

# 3. ans.push_back(current)

ans.push_back(current);

Har current subset khud ek valid subset hai.

Example:

current = []

Ye valid hai.

current = [1]

Ye bhi valid hai.

current = [1,2]

Ye bhi valid hai.

Isliye har recursive call ke start me current ko answer me store kar dete hain.

---

# 4. For Loop

for(int i = index; i < nums.size(); i++)

Ye current level par remaining elements ko one by one try karta hai.

Example:

nums = [1,2,2]

index = 0

To loop:

i = 0
i = 1
i = 2

tak ja sakta hai.

---

# 5. Duplicate Skip

if(i > index && nums[i - 1] == nums[i]) {
    continue;
}

Ye LC 90 ka sabse important part hai.

Iska meaning:

Agar same recursion level par same value already try kar chuke hain, to us duplicate value ko dobara try mat karo.

---

# 6. nums[i - 1] == nums[i]

Example:

[1,2,2]

i = 2

nums[2] = 2

nums[1] = 2

Dono same hain.

Matlab duplicate element hai.

---

# 7. i > index

Ye check karta hai ki duplicate same recursion level par hai ya nahi.

Example:

index = 0

i = 0

i > index

0 > 0

false.

So first 1 allowed hai.

Then:

i = 1

1 > 0

true.

But nums[1] = 2 and nums[0] = 1.

Same nahi hain, so 2 allowed.

Then:

i = 2

2 > 0

true.

nums[2] = 2
nums[1] = 2

same hain.

So skip.

---

# Why Same Level Duplicate Skip Karna Hai?

Suppose:

[1,2,2]

Agar index = 0 par:

first 2 choose karte ho:

[2]

Aur phir second 2 ko bhi same index 0 se choose karoge:

[2]

To dono same subset generate karenge.

Isliye same level par second 2 skip kar dete hain.

---

# Important Point

Hum duplicate element ko completely ban nahi kar rahe.

Hum sirf:

**same level par duplicate choice ko skip kar rahe hain.**

Example:

[2,2]

First 2 choose karke:

[2]

banaya.

Ab deeper recursion me second 2 choose kar sakte hain:

[2,2]

Ye valid hai.

Isliye duplicate skip sirf same level par hota hai.

---

# 8. Take

current.push_back(nums[i]);

Ye current element ko subset me add karta hai.

Example:

current = []

nums[i] = 1

After:

current = [1]

Ye choice lena hai.

---

# 9. Recursive Call

solve(nums, i + 1, current);

Ye bahut important hai.

Humne element `i` choose kar liya.

Ab next elements se subset banana hai.

Isliye:

i + 1

se recursion start hoti hai.

Example:

i = 1

Element 2 choose kiya.

Ab second 2 se aage jaana hai.

So:

solve(nums, 2, current);

---

# Why i + 1 and Not index + 1?

Ye common mistake hai.

Suppose:

index = 0

and:

i = 2

Humne index 2 wala element choose kiya.

To next available position `3` hogi.

Isliye:

solve(nums, i + 1, current);

Correct.

Agar `index + 1` likhenge to hum galat jagah se recursion start kar denge.

---

# 10. Backtracking

current.pop_back();

Element ko remove karke previous state me return karte hain.

Example:

Before:

[1,2]

pop_back()

After:

[1]

Matlab:

2 ko undo kar diya.

Ab loop next choice try kar sakta hai.

---

# Complete Flow

Sort:

[1,2,2]

↓

solve(nums, 0, [])

↓

[] answer me add

↓

1 choose

↓

[1]

↓

2 choose

↓

[1,2]

↓

second 2 choose

↓

[1,2,2]

↓

backtrack

↓

[1,2]

↓

same level ka duplicate 2 skip

↓

backtrack

↓

[]

↓

2 choose

↓

[2]

↓

next 2 choose

↓

[2,2]

↓

backtrack

↓

second 2 same level duplicate tha

↓

skip

---

# Detailed Dry Run

Input:

[1,2,2]

After sort:

[1,2,2]

---

## Step 1

solve(nums, 0, [])

First:

ans.push_back([])

Answer:

[]

---

## Step 2

i = 0

nums[0] = 1

Duplicate check:

i > index

0 > 0

false

So 1 choose kar sakte hain.

current:

[1]

Call:

solve(nums, 1, [1])

---

## Step 3

Current:

[1]

Isko answer me add karo.

Answer:

[]
[1]

Now:

index = 1

Loop:

i = 1

Choose first 2.

current:

[1,2]

Call:

solve(nums, 2, [1,2])

---

## Step 4

Current:

[1,2]

Answer me add.

Then:

i = 2

Second 2 choose kar sakte hain.

current:

[1,2,2]

Answer me add.

Then pop:

[1,2]

---

## Step 5

Back to index = 1.

Now i = 2.

Check:

i > index

2 > 1

true

and:

nums[2] == nums[1]

2 == 2

true

So:

continue;

Matlab second 2 ko same level par dobara start nahi karna.

---

# Why [1,2,2] Still Exists?

Because first 2 ke deeper recursion me second 2 choose kiya tha.

So:

[1,2,2]

valid subset hai.

Humne usse remove nahi kiya.

---

# Next Branch

Back to:

[]

Now:

i = 1

Choose 2.

current:

[2]

Call:

solve(nums, 2, [2])

Then:

i = 2

Second 2 choose.

current:

[2,2]

Store.

---

# Final Answer

[
    [],
    [1],
    [1,2],
    [1,2,2],
    [2],
    [2,2]
]

Order different ho sakta hai, but duplicate subsets nahi hone chahiye.

---

# LC 78 vs LC 90

## LC 78 - Subsets

Array:

[1,2,3]

Normal backtracking.

Each element:

Take / Skip

---

## LC 90 - Subsets II

Array:

[1,2,2]

Backtracking same hai.

Extra:

Sort + Duplicate Skip

Main difference:

if(i > index && nums[i] == nums[i-1])

---

# Backtracking Pattern

General pattern:

Choose
→ Recursion
→ Undo

Yahan:

current.push_back(nums[i]);

solve(nums, i + 1, current);

current.pop_back();

Meaning:

Take
→ Next level
→ Undo

---

# Complexity

Agar `n` elements hain, maximum subsets:

2^n

Har subset ko store/copy karne me O(n) lag sakta hai.

So worst-case:

Time Complexity = O(n × 2^n)

Sorting:

O(n log n)

Overall:

O(n × 2^n)

Worst case me duplicates nahi hain.

Space Complexity:

Recursion depth = O(n)

Current subset = O(n)

Answer storage:

O(n × 2^n)

---

# Common Mistakes

## Mistake 1

sort() bhool jaana.

Correct:

sort(nums.begin(), nums.end());

Sorting ke bina duplicate elements adjacent nahi honge.

---

## Mistake 2

Wrong recursive call:

solve(nums, index + 1, current);

Correct:

solve(nums, i + 1, current);

Kyuki selected element `i` par hai.

---

## Mistake 3

Duplicate check hata dena.

Then duplicate subsets aa sakte hain.

---

## Mistake 4

Duplicate element ko completely skip karna.

Ye bhi wrong hai.

Example:

[2,2]

[2,2] valid subset hai.

Sirf same level ke duplicate start ko skip karna hai.

---

# Interview Explanation

"First I sort the array so that duplicate elements become adjacent. Then I use backtracking to generate subsets. At every recursion level, I try each available element. If the current element is the same as the previous one and I am at the same recursion level, I skip it to avoid duplicate subsets. After choosing an element, I recursively process the next index, and then remove it using pop_back for backtracking."

---

# Pattern Recognition

Question me agar:

- all subsets
- unique subsets
- array contains duplicates
- duplicate answers should not be present

aisa dikhe, then:

**Subsets + Backtracking + Duplicate Handling**

think karo.

Main steps:

Sort

→ For loop

→ Same-level duplicate skip

→ Push

→ Recursion

→ Pop

---

# Most Important Line

if(i > index && nums[i] == nums[i - 1]) {
    continue;
}

Simple meaning:

**Same level par same value already try ho chuki hai, isliye duplicate choice skip karo.**

---

# Mental Model

LC 90 ko yaad rakhne ka easiest way:

**Sort → Choose → Recurse → Undo**

Aur duplicate ke liye:

**Same Level Duplicate → Skip**

---

# One Line Takeaway

**Subsets II = Subsets + Sorting + Same-Level Duplicate Skip**
