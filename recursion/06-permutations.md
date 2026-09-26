# 06 - Permutations

## LeetCode 46 - Permutations

### Problem

Given an array `nums` containing distinct integers, return all possible permutations.

### Example

Input:

[1,2,3]

Output:

[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]

---

# Simple Understanding

Permutation ka matlab hai:

**Elements ka order change karke saare possible arrangements banana.**

For `[1,2,3]`:

- 1 ke baad 2,3
- 2 ke baad 1,3
- 3 ke baad 1,2

Isliye total:

3! = 6 permutations

---

# Main Idea - Swap Method

Yahan hum `used[]` array use nahi karenge.

Hum directly array ke elements ko `swap()` karenge.

Har recursion level par ek position ko fix karenge.

For example:

[1,2,3]

Sabse pehle `index = 0`

Matlab:

**position 0 par kaunsa element aayega?**

Possible choices:

- 1
- 2
- 3

Isliye hum loop chalayenge:

for(int i = index; i < nums.size(); i++)

Aur:

swap(nums[index], nums[i])

Isse selected element current position par aa jaata hai.

---

# index Ka Meaning

`index` batata hai:

**Abhi kaunsi position fill karni hai.**

Example:

[1,2,3]

### index = 0

Position 0 fill karni hai.

Possible:
- 1
- 2
- 3

### index = 1

Position 1 fill karni hai.

Ab position 0 already fix ho chuki hai.

Example:

[1,2,3]

Ab sirf:

- 2
- 3

me se choose karna hai.

### index = 2

Ab last position fill karni hai.

Sirf ek element bacha hoga.

### index = 3

Array completely fill ho gaya.

Permutation ready hai.

---

# Why Loop Starts From index?

Code:

for(int i = index; i < nums.size(); i++)

Hum `0` se start nahi karte.

Kyun?

Kyuki `index` se pehle wali positions already fix ho chuki hoti hain.

Example:

[1,2,3]

Agar:

index = 1

to position 0 already fixed hai.

Suppose:

[1,2,3]

Ab position 1 aur 2 ko arrange karna hai.

Isliye:

i = 1

se start karenge.

---

# Swap Ka Role

Code:

swap(nums[index], nums[i]);

Ye current position ke liye ek element choose karta hai.

Example:

nums = [1,2,3]

index = 0

### i = 0

swap(0,0)

[1,2,3]

Matlab position 0 ke liye `1` choose kiya.

### i = 1

swap(0,1)

[2,1,3]

Matlab position 0 ke liye `2` choose kiya.

### i = 2

swap(0,2)

[3,2,1]

Matlab position 0 ke liye `3` choose kiya.

---

# Recursion

Choice karne ke baad:

solve(nums, index + 1);

Matlab:

**Ab next position fill karo.**

Example:

[1,2,3]

Agar position 0 par `1` fix kiya:

[1,2,3]

Ab:

solve(nums,1)

Matlab position 1 fill karni hai.

Ab choices:

- 2
- 3

---

# Backtracking

Sabse important part:

swap(nums[index], nums[i]);

solve(nums, index + 1);

swap(nums[index], nums[i]);

Second swap ko **undo** karna bolte hain.

Pehla swap:

**Choice**

Second swap:

**Undo Choice**

Taaki next choice try kar sakein.

---

# Why Second Swap Important Hai?

Example:

[1,2,3]

index = 0

i = 1

First swap:

swap(0,1)

Array:

[2,1,3]

Ab recursion chalega aur `[2,1,3]` se permutations banenge.

Recursion ke baad hume array ko original state me laana hai.

Second swap:

swap(0,1)

Array wapas:

[1,2,3]

Ab next:

i = 2

try kar sakte hain.

Agar second swap nahi karte, to next branch wrong array se start hogi.

---

# Base Case

Code:

if(index == nums.size()) {
    ans.push_back(nums);
    return;
}

Jab:

index == nums.size()

iska matlab:

**Saari positions fill ho chuki hain.**

Example:

[2,3,1]

index = 3

Array completely ready hai.

Is permutation ko answer me store kar denge.

---

# Complete Code

class Solution {
public:
    vector<vector<int>> ans;

    void solve(vector<int>& nums, int index) {

        if(index == nums.size()) {
            ans.push_back(nums);
            return;
        }

        for(int i = index; i < nums.size(); i++) {

            // Choose
            swap(nums[index], nums[i]);

            // Fill next position
            solve(nums, index + 1);

            // Undo choice
            swap(nums[index], nums[i]);
        }
    }

    vector<vector<int>> permute(vector<int>& nums) {

        solve(nums, 0);

        return ans;
    }
};

---

# Detailed Dry Run

Input:

[1,2,3]

## Step 1

index = 0

Loop:

i = 0

Swap:

swap(0,0)

Array:

[1,2,3]

Recursion:

solve(index = 1)

---

## Step 2

index = 1

Loop:

i = 1

Swap:

swap(1,1)

Array:

[1,2,3]

Recursion:

solve(index = 2)

---

## Step 3

index = 2

Loop:

i = 2

Swap:

swap(2,2)

Array:

[1,2,3]

Recursion:

solve(index = 3)

Now:

index == nums.size()

So:

[1,2,3]

answer me add.

Then return.

---

## Backtrack

Second swap:

swap(2,2)

Array same:

[1,2,3]

Now index = 2 ka loop khatam.

Return to index = 1.

---

## index = 1

Ab:

i = 2

Swap:

swap(1,2)

Array:

[1,3,2]

Recursion:

solve(index = 2)

Ab base case par:

[1,3,2]

answer me add.

Backtrack:

swap(1,2)

Array:

[1,2,3]

---

# First Element 2

Ab index = 0 par wapas aate hain.

Next:

i = 1

Swap:

swap(0,1)

Array:

[2,1,3]

Ab recursion baaki two elements ko arrange karega.

Milenge:

[2,1,3]

[2,3,1]

---

# First Element 3

Next:

i = 2

Swap:

swap(0,2)

Array:

[3,2,1]

Remaining elements arrange karne par:

[3,2,1]

[3,1,2]

---

# Final Answer

[
    [1,2,3],
    [1,3,2],
    [2,1,3],
    [2,3,1],
    [3,2,1],
    [3,1,2]
]

---

# Recursion Tree Concept

For:

[1,2,3]

First position:

        1
       / \
      2   3

        2
       / \
      1   3

        3
       / \
      2   1

Actually recursion concept ko aise samjho:

Position 0:

1 / 2 / 3

Agar 1 choose hua:

    1
   / \
  2   3

Result:

[1,2,3]
[1,3,2]

Agar 2 choose hua:

    2
   / \
  1   3

Result:

[2,1,3]
[2,3,1]

Agar 3 choose hua:

    3
   / \
  1   2

Result:

[3,1,2]
[3,2,1]

Total = 6

---

# Code Flow

solve(nums, 0)

↓

index = 0

↓

Current position ke liye har possible element try karo

↓

swap()

↓

solve(index + 1)

↓

Next position fill karo

↓

Jab saari positions fill ho jaayein

↓

answer me permutation add karo

↓

wapas aao

↓

second swap se array restore karo

↓

next choice try karo

---

# Most Important 3 Lines

Permutation problem me ye 3 lines main logic hain:

swap(nums[index], nums[i]);

solve(nums, index + 1);

swap(nums[index], nums[i]);

Meaning:

1. First swap → choose
2. Recursion → next position fill karo
3. Second swap → undo

Ye hi **Backtracking** ka core pattern hai.

---

# Backtracking Kya Hai?

Backtracking ka simple meaning:

**Choice lo → aage jao → kaam complete karo → choice undo karo → next choice try karo.**

Permutation me:

Choose
→ Recursion
→ Undo
→ Next Choice

---

# Time Complexity

Total permutations:

n!

Har permutation ko store/copy karne me `O(n)` lag sakta hai.

Therefore:

**Time Complexity = O(n × n!)**

---

# Space Complexity

Recursion depth:

n

Isliye recursion stack:

**O(n)**

Answer ko ignore karke auxiliary space:

**O(n)**

Answer storage alag se:

**O(n × n!)**

---

# Common Mistakes

### 1. Second swap bhool jaana

Wrong:

swap(nums[index], nums[i]);
solve(nums, index + 1);

Backtracking nahi hoga.

Correct:

swap(nums[index], nums[i]);
solve(nums, index + 1);
swap(nums[index], nums[i]);

---

### 2. Loop ko 0 se start karna

Wrong:

for(int i = 0; i < nums.size(); i++)

Correct:

for(int i = index; i < nums.size(); i++)

Kyuki `index` se pehle wali positions already fixed hain.

---

### 3. Base Case galat likhna

Correct:

if(index == nums.size())

Tabhi array completely fill hua hai.

---

### 4. index + 1 nahi karna

Recursive call:

solve(nums, index + 1);

Har call me next position par move karna hai.

---

# Permutations vs Subsets

## Subsets

Har element ke paas 2 choices:

- Take
- Skip

So:

2^n subsets

## Permutations

Har position par remaining elements me se ek choice.

So:

n!

---

# Pattern Recognition

Question me agar:

- All possible arrangements
- Every possible ordering
- Different orderings
- Arrange all elements
- Generate permutations

aisa kuch likha ho,

to permutation + backtracking think karo.

---

# Interview Explanation

Interview me simple language me aise explain kar sakte ho:

"We use backtracking with swapping. The index represents the current position that we need to fill. For every position, we try every remaining element by swapping it with the current index. Then we recursively fill the next position. Once all positions are filled, we store the permutation. After recursion, we swap back to undo the choice and try the next possibility."

---

# One-Line Mental Model

**Current position fix karo → remaining elements try karo → recursion karo → swap back karo.**

---

# Final Takeaway

LC 46 ka pura concept:

Position choose karo
→ swap karo
→ next position par recursion
→ permutation complete hone par store karo
→ swap back karke undo karo
→ next choice try karo

Permutation = **Backtracking + Swap**

Sabse important:

**First swap = Choose**

**Second swap = Undo**

**index = Current position**

**i = Current position ke liye kaunsa remaining element choose kar rahe ho**
