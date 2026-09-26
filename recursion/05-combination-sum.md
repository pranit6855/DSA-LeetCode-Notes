# Recursion Pattern

# 05. Combination Sum

## LeetCode 39

## Pattern
Recursion + Take / Skip + Same Index

---

# 1. Problem Statement

Given an array `candidates` and an integer `target`.

Hume aise combinations find karne hain jinka sum exactly `target` ke equal ho.

Example:

    candidates = [2,3,6,7]
    target = 7

Valid combinations:

    [2,2,3]
    [7]

Because:

    2 + 2 + 3 = 7
    7 = 7

---

# 2. Simple Language Mein Problem

Hume array se numbers choose karke target banana hai.

Example:

    candidates = [2,3,6,7]
    target = 7

Hum:

    2 + 2 + 3

se 7 bana sakte hain.

Ya directly:

    7

se 7 bana sakte hain.

Important rule:

Ek number ko multiple times use kar sakte hain.

Example:

    2 + 2 + 3

Yahan `2` do baar use hua.

---

# 3. Main Idea

Har current number ke paas 2 choices hain:

    TAKE
    SKIP

Lekin yahan ek important difference hai.

Agar number TAKE kiya:

    Same index par rehna hai

Kyunki same number ko dobara use kar sakte hain.

Agar number SKIP kiya:

    Next index par jaana hai

So:

    TAKE → same index

    SKIP → index + 1

---

# 4. Example

candidates:

    [2,3,6,7]

target:

    7

Start:

    index = 0
    current = []

Current number:

    2

Do choices:

    Take 2
    Skip 2

---

# 5. TAKE 2

Current:

    [2]

Target:

    7 - 2 = 5

Ab `2` ko dobara le sakte hain.

Isliye:

    index same rahega

Again:

    Take 2

Current:

    [2,2]

Target:

    5 - 2 = 3

Again:

    Take 2

Current:

    [2,2,2]

Target:

    3 - 2 = 1

Ab 2 lene se:

    target = -1

So ye path invalid hai.

---

# 6. Backtrack

Ab previous choice undo karni hai.

    current.pop_back();

Current:

    [2,2]

Ab 2 ko nahi lenge.

Skip:

    index + 1

Next number:

    3

Take 3:

    [2,2,3]

Target:

    7 - 2 - 2 - 3

    = 0

Target 0 ho gaya.

So:

    [2,2,3]

valid combination hai.

---

# 7. Another Valid Combination

Start se `7` ko choose kar sakte hain.

Current:

    [7]

Target:

    7 - 7
    = 0

So:

    [7]

also valid.

Final answer:

    [2,2,3]
    [7]

---

# 8. Base Cases

Is question mein 2 important base cases hain.

## Target == 0

Combination complete.

    if(target == 0)

Then:

    ans.push_back(current);

and:

    return;

---

## Target < 0

Target cross ho gaya.

Example:

    target = 1

and current number:

    2

Then:

    target = 1 - 2
           = -1

Ye path valid nahi hai.

So:

    return;

---

# 9. Important Base Case Correction

Ye galat hai:

    if(index == candidates.size()) {
        ans.push_back(current);
        return;
    }

Kyunki `index` end ho sakta hai even when target 0 nahi hua.

Example:

    candidates = [2,3]
    target = 7

Suppose current:

    [2,2]

Remaining target:

    3

Agar index end ho gaya, iska matlab ye valid answer nahi hai.

Isliye answer tabhi add hoga jab:

    target == 0

---

# 10. Recursion Flow

Current number:

    candidates[index]

Then:

    TAKE
       ↓
    target - current number
       ↓
    SAME INDEX

OR

    SKIP
       ↓
    target same
       ↓
    INDEX + 1

---

# 11. LC 78 Se Difference

Ye bahut important hai.

## LC 78 — Subsets

Element ko ek baar process karte the.

    TAKE → index + 1

    SKIP → index + 1

---

## LC 39 — Combination Sum

Same number multiple times use kar sakte hain.

    TAKE → SAME INDEX

    SKIP → index + 1

Ye LC 39 ka main trick hai.

---

# 12. Detailed Dry Run

Input:

    candidates = [2,3,6,7]
    target = 7

Start:

    current = []
    index = 0
    target = 7

Current number:

    2

---

## Take 2

Current:

    [2]

Target:

    5

Index:

    0

Again 2 available.

---

## Take 2

Current:

    [2,2]

Target:

    3

Index:

    0

Again 2.

---

## Take 2

Current:

    [2,2,2]

Target:

    1

Again 2 lene par:

    target = -1

So invalid.

Return.

---

## Backtrack

Remove last 2:

    [2,2]

Now Skip 2.

Index:

    1

Current number:

    3

Take 3:

    [2,2,3]

Target:

    0

Base case.

Answer mein:

    [2,2,3]

---

# 13. `push_back()` Aur `pop_back()`

## push_back

    current.push_back(candidates[index]);

Means:

    TAKE

Example:

    []
    ↓
    [2]

---

## pop_back

    current.pop_back();

Means:

    Undo TAKE

Example:

    [2,2,3]
    ↓
    [2,2]

Ye next choice try karne ke liye hota hai.

---

# 14. Why Same Index on Take?

Suppose:

    candidates = [2,3,6,7]

Current:

    2

Hume `2` ko multiple times use karna allowed hai.

So:

    Take 2
    ↓
    Take 2
    ↓
    Take 2

Isliye:

    solve(candidates, index, ...)

same index.

---

# 15. Why `index + 1` on Skip?

Agar hum current number nahi lena chahte:

    2 skip

Toh next available candidate:

    3

So:

    index + 1

---

# 16. Code

class Solution {
public:
    vector<vector<int>> ans;

    void solve(vector<int>& candidates, int index, int target, vector<int>& current) {

        if(target == 0) {
            ans.push_back(current);
            return;
        }

        if(target < 0 || index == candidates.size()) {
            return;
        }

        // Take
        current.push_back(candidates[index]);

        solve(candidates, index, target - candidates[index], current);

        // Undo Take
        current.pop_back();

        // Skip
        solve(candidates, index + 1, target, current);
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {

        vector<int> current;

        solve(candidates, 0, target, current);

        return ans;
    }
};

---

# 17. Code Ka Simple Flow

## Step 1

    vector<vector<int>> ans;

All valid combinations store honge.

---

## Step 2

    vector<int> current;

Current combination.

Start:

    []

---

## Step 3

    solve(candidates, 0, target, current);

Index 0 se recursion start.

---

## Step 4

Base case:

    if(target == 0)

Target complete.

Current answer mein add.

---

## Step 5

Invalid case:

    if(target < 0 || index == candidates.size())

Path impossible hai.

Return.

---

## Step 6

TAKE:

    current.push_back(candidates[index]);

Current number choose karo.

---

## Step 7

Recursive TAKE:

    solve(candidates, index, target - candidates[index], current);

Important:

    index same

Kyunki number dobara use kar sakte hain.

---

## Step 8

UNDO:

    current.pop_back();

Last selected number remove karo.

---

## Step 9

SKIP:

    solve(candidates, index + 1, target, current);

Current number ko skip karke next candidate par jao.

---

# 18. Main Code Pattern

LC 39 ke heart mein ye hai:

    // Take
    current.push_back(candidates[index]);

    solve(candidates, index,
          target - candidates[index],
          current);

    // Undo
    current.pop_back();

    // Skip
    solve(candidates, index + 1,
          target,
          current);

---

# 19. Most Important Difference

Remember:

    LC 78:
    TAKE → index + 1

    LC 39:
    TAKE → same index

Because:

    LC 78 → element once

    LC 39 → element unlimited times

---

# 20. Recursion Tree Idea

For:

    candidates = [2,3,7]
    target = 7

Start with 2:

                    target = 7
                       2
                /             \
             TAKE             SKIP
               ↓                ↓
          target = 5          3
               2              ...
          /         \
      TAKE          SKIP
        ↓             ↓
   target = 3         3
      2               ↓
      ...           target = 0

Valid branches eventually:

    [2,2,3]
    [7]

---

# 21. Common Mistakes

## Mistake 1

TAKE mein `index + 1` kar dena.

Wrong for this problem:

    Take → index + 1

Correct:

    Take → same index

---

## Mistake 2

Target 0 ke bina answer add karna.

Correct:

    target == 0

only then answer.

---

## Mistake 3

Target negative ke case ko handle na karna.

Correct:

    target < 0 → return

---

## Mistake 4

Take ke baad `pop_back()` bhool jaana.

Skip branch galat current ke saath run hogi.

---

## Mistake 5

Index end par pahunchne ke baad bhi answer add karna.

Index end hona alone valid combination nahi hai.

Target bhi 0 hona chahiye.

---

# 22. Complexity

Exact complexity input aur target values par depend karti hai because same number multiple times use ho sakta hai.

Recursion bahut saari combinations explore kar sakti hai.

Roughly exponential search space hota hai.

Output size bhi potentially exponential ho sakta hai.

Important point:

    Recursion explores many possible combinations.

---

# 23. Interview Explanation

"I will use recursion with a Take or Skip approach.

For every candidate, I have two choices.

If I take the current candidate, I keep the same index because the same number can be reused unlimited times. I also reduce the target by that candidate's value.

If I skip the current candidate, I move to the next index.

When target becomes zero, I have found a valid combination and add it to the answer.

If target becomes negative or there are no more candidates, I stop that branch."

---

# 24. Pattern Recognition

Agar question mein:

- Target sum banana hai
- Candidates diye hain
- Same element multiple times use allowed hai
- All combinations chahiye

Think:

    Recursion + Take / Skip

But remember:

    Take → Same Index

---

# 25. Mental Model

Current number:

    Take?
       ↓
    YES → target reduce
           same index

    NO
       ↓
    Skip
       ↓
    next index

Then:

    target == 0
       ↓
    Answer

---

# 26. One-Line Trick

"Combination Sum mein current number ko lo toh same index par raho, kyunki number reuse ho sakta hai; skip karo toh next index par jao."

---

# 27. Final Takeaway

LC 39 ka complete flow:

    Current Candidate
          ↓
      /         \
    TAKE       SKIP
      ↓           ↓
 Same Index    Next Index
      ↓           ↓
 Target reduce  Target same
      ↓           ↓
        Recursion
             ↓
    target == 0 → Answer

Important:

    TAKE → same index

    SKIP → index + 1

    target == 0 → valid combination

    target < 0 → invalid branch

Core Pattern:

    Take / Skip + Reuse Same Element
