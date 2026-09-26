# 08 - Combination Sum II

## LeetCode 40 - Combination Sum II

### Problem

Given an integer array `candidates` and an integer `target`, return all unique combinations where the chosen numbers sum to `target`.

Important:

- Har element **sirf ek baar** use ho sakta hai.
- Array me duplicate values ho sakti hain.
- Duplicate combinations answer me nahi honi chahiye.

---

# Example

Input:

candidates = [10,1,2,7,6,1,5]
target = 8

After sorting:

[1,1,2,5,6,7,10]

Output:

[
    [1,1,6],
    [1,2,5],
    [1,7],
    [2,6]
]

---

# Simple Understanding

Hume array me se kuch elements choose karne hain jinka sum `target` ke equal ho.

Example:

target = 8

Possible:

1 + 1 + 6 = 8

1 + 2 + 5 = 8

1 + 7 = 8

2 + 6 = 8

Ye saare valid combinations hain.

---

# Main Pattern

Ye problem:

**Combination + Backtracking + Duplicate Handling**

Use karta hai.

Basic flow:

Sort
→ Loop
→ Duplicate skip
→ Element choose
→ Target reduce
→ Next index par recursion
→ Element undo

---

# LC 39 vs LC 40

Ye difference bahut important hai.

## LC 39 - Combination Sum

Same element ko baar-baar use kar sakte hain.

Example:

[2,3,6,7]

target = 7

[2,2,3]

allowed hai.

Isliye recursion me:

solve(..., i, ...)

use hota hai.

---

## LC 40 - Combination Sum II

Har array element **sirf ek baar** use kar sakte hain.

Isliye:

solve(..., i + 1, ...)

use hota hai.

Example:

Agar `i = 2` wala element choose kar liya,

to next recursion `3` se start hogi.

---

# Complete Code

class Solution {
public:
    vector<vector<int>> ans;

    void solve(vector<int>& candidates, int index, int target, vector<int>& current) {

        if(target == 0) {
            ans.push_back(current);
            return;
        }

        for(int i = index; i < candidates.size(); i++) {

            // Same level par duplicate skip karo
            if(i > index && candidates[i] == candidates[i - 1]) {
                continue;
            }

            // Sorted array hai, aage sab aur bade honge
            if(candidates[i] > target) {
                break;
            }

            // Take
            current.push_back(candidates[i]);

            // Current element ko dobara use nahi karna
            solve(candidates, i + 1, target - candidates[i], current);

            // Undo
            current.pop_back();
        }
    }

    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {

        sort(candidates.begin(), candidates.end());

        vector<int> current;

        solve(candidates, 0, target, current);

        return ans;
    }
};

---

# Code Explanation

## 1. ans

vector<vector<int>> ans;

Isme saari valid combinations store hongi.

Example:

[
    [1,1,6],
    [1,2,5],
    [1,7],
    [2,6]
]

---

# 2. solve()

void solve(vector<int>& candidates, int index, int target, vector<int>& current)

Isme 4 important cheezein hain.

### candidates

Sorted input array.

### index

Ab hum kis index se elements choose kar sakte hain.

### target

Abhi kitna sum aur chahiye.

### current

Abhi jo combination bana rahe hain.

Example:

current = [1,2]

target = 5

Matlab 1 aur 2 already choose kar chuke hain aur ab 5 ka sum aur chahiye.

---

# 3. Base Case

if(target == 0) {
    ans.push_back(current);
    return;
}

Agar target 0 ho gaya:

Matlab current combination ka sum exactly original target ke equal ho gaya.

Example:

target = 8

1 + 2 + 5 = 8

So:

current = [1,2,5]

target = 0

Combination valid hai.

Answer me add:

ans.push_back(current);

Then:

return;

Kyunki answer mil gaya, ab is branch ko aur explore nahi karna.

---

# 4. For Loop

for(int i = index; i < candidates.size(); i++)

Current level par available elements ko one by one try karta hai.

Example:

candidates = [1,1,2,5,6,7,10]

index = 0

Then:

i = 0
i = 1
i = 2
i = 3
...

---

# 5. Duplicate Skip

if(i > index && candidates[i] == candidates[i - 1]) {
    continue;
}

Ye problem ka most important part hai.

Suppose:

[1,1,2,5,6,7,10]

Same recursion level par pehla `1` already try ho chuka hai.

Agar second `1` ko bhi same level se start karenge, same combination duplicate generate ho sakta hai.

Isliye second duplicate ko skip kar dete hain.

---

# `i > index` ka Meaning

Ye check karta hai ki hum same recursion level par hain.

Example:

index = 0

i = 0

i > index:

0 > 0

false

So first `1` allowed.

Next:

i = 1

1 > 0

true

Aur:

candidates[1] == candidates[0]

1 == 1

true

So duplicate skip.

---

# Important

Duplicate value ko completely ban nahi karte.

Sirf:

**same recursion level par duplicate choice skip karte hain.**

Example:

[1,1,6]

Yahan dono `1` use ho sakte hain.

Pehla `1` choose hua:

current = [1]

Next recursion me second `1` choose kar sakte hain:

current = [1,1]

So `[1,1,6]` valid hai.

---

# 6. candidates[i] > target

if(candidates[i] > target) {
    break;
}

Ye sorting ki wajah se possible hai.

Example:

target = 5

Current element:

6

6 > 5

Toh 6 ko choose karke target 0 nahi ho sakta.

Aur array sorted hai, so uske baad:

7
10

bhi target se bade honge.

Isliye:

break;

---

# 7. Take

current.push_back(candidates[i]);

Matlab current number ko combination me choose kar liya.

Example:

current = [1,2]

candidates[i] = 5

After:

current = [1,2,5]

---

# 8. Recursive Call

solve(candidates, i + 1, target - candidates[i], current);

Is line ke 2 major kaam hain.

## Target Reduce

Agar:

target = 8

and current number:

5

Then:

new target = 8 - 5 = 3

---

## i + 1

Current element already use ho gaya.

Isliye next recursion current element ke next index se start hogi.

Example:

i = 3

To:

i + 1 = 4

So index 4 se aage choices hongi.

Ye ensure karta hai ki **same array element dobara use nahi hota**.

---

# 9. Backtracking

current.pop_back();

Ye chosen element ko remove karta hai.

Example:

current = [1,2,5]

Recursion complete.

pop_back():

current = [1,2]

Ab next choice try kar sakte hain.

So:

push_back = Choose

recursive call = Explore

pop_back = Undo

---

# Detailed Dry Run

Input:

candidates = [10,1,2,7,6,1,5]

target = 8

After sorting:

[1,1,2,5,6,7,10]

Start:

current = []

index = 0

target = 8

---

## Branch 1

i = 0

Choose `1`

current:

[1]

target:

7

Call:

solve(..., 1, 7, [1])

---

## Branch 1.1

i = 1

Choose second `1`

current:

[1,1]

target:

6

Then choose `6`

current:

[1,1,6]

target:

0

Valid combination:

[1,1,6]

Add to answer.

Then backtrack.

---

## Duplicate Check

Back to:

current = [1]

At same level:

i = 2

Now value `2` hai, so allowed.

But second `1` at same level was skipped because:

candidates[i] == candidates[i - 1]

---

## Branch 2

current:

[1,2]

target:

5

Choose `5`

current:

[1,2,5]

target:

0

Valid.

---

## Branch 3

current:

[1,7]

target:

0

Valid.

---

## Branch 4

Back to empty.

Choose `2`.

current:

[2]

target:

6

Choose `6`.

current:

[2,6]

target:

0

Valid.

---

# Final Answer

[
    [1,1,6],
    [1,2,5],
    [1,7],
    [2,6]
]

---

# Why `i + 1`?

Ye interview me bahut important question ho sakta hai.

Suppose:

[1,2,5,6]

Humne `2` choose kiya.

Agar same `2` ko dobara use karna allowed hota, to:

solve(..., i, ...)

hota.

But Combination Sum II me har element sirf once use karna hai.

Therefore:

solve(..., i + 1, ...)

Matlab:

**Current element khatam, ab next index se start.**

---

# Why `target == 0` par Return?

Suppose:

current = [1,7]

target = 0

Combination already valid hai.

Agar return nahi karenge, to loop continue karega aur unnecessary recursion ho sakti hai.

Isliye:

if(target == 0) {
    ans.push_back(current);
    return;
}

---

# Backtracking Flow

Combination Sum II ka exact flow:

Choose:

current.push_back(nums[i])

↓

Target reduce:

target - nums[i]

↓

Next element:

i + 1

↓

Recursive call

↓

Kaam complete

↓

Undo:

current.pop_back()

↓

Next choice

---

# LC 90 vs LC 40

Dono me duplicate handling similar hai.

## LC 90 - Subsets II

Goal:

All unique subsets

No target.

Example:

[1,2,2]

---

## LC 40 - Combination Sum II

Goal:

Unique combinations whose sum = target.

Target maintain karna padta hai.

Example:

[1,1,2,5,6,7,10]

target = 8

---

# Important Conditions

### Valid combination

target == 0

### Duplicate skip

if(i > index && candidates[i] == candidates[i - 1])

### Too large

if(candidates[i] > target)

### Take

current.push_back(candidates[i])

### Next element

solve(..., i + 1, ...)

### Undo

current.pop_back()

---

# Common Mistakes

## 1. `index + 1` use karna

Wrong:

solve(candidates, index + 1, ...)

Correct:

solve(candidates, i + 1, ...)

Because selected element is at `i`.

---

## 2. Target 0 par return na karna

Correct:

if(target == 0) {
    ans.push_back(current);
    return;
}

---

## 3. Sorting bhool jaana

Correct:

sort(candidates.begin(), candidates.end());

Duplicate values ko adjacent lane ke liye sorting necessary hai.

---

## 4. Duplicate check hata dena

Then same combinations multiple times aa sakti hain.

---

## 5. Same element ko reuse karna

Combination Sum II me:

solve(..., i + 1, ...)

hona chahiye.

Not:

solve(..., i, ...)

---

# Complexity

Worst case me total subsets approximately:

2^n

Har combination ko copy/store karne me `O(n)` lag sakta hai.

So:

Time Complexity = O(n × 2^n)

Sorting:

O(n log n)

Overall worst-case:

O(n × 2^n)

Auxiliary recursion/current space:

O(n)

Answer storage:

O(n × 2^n)

---

# Interview Explanation

"I first sort the array so that duplicate values become adjacent. Then I use backtracking to build combinations. For each position, I try every available element. If the current element is the same as the previous element at the same recursion level, I skip it to avoid duplicate combinations. After choosing an element, I reduce the target and recurse from `i + 1` because each element can be used only once. After recursion, I remove the element using `pop_back()` to backtrack."

---

# Pattern Recognition

Question me agar:

- Combination ka sum target ke equal ho
- Every element only once use karna ho
- Duplicates possible ho
- Unique combinations chahiye

Then think:

**Combination Sum II → Backtracking + Sorting + Duplicate Skip**

---

# Mental Model

Sort

↓

Duplicate same level par skip

↓

Element choose

↓

Target reduce

↓

`i + 1`

↓

Recursion

↓

Pop / Undo

---

# One Line Takeaway

**Combination Sum II = Combination Sum + Each Element Once + Duplicate Skip**

Most important:

`i + 1` = element reuse nahi hoga

`target - candidates[i]` = remaining target

`i > index && candidates[i] == candidates[i-1]` = same-level duplicate skip

`pop_back()` = backtracking
