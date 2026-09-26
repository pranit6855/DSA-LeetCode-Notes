# Recursion Pattern

# 04. Subsets

## LeetCode 78

## Pattern
Recursion + Take / Skip

---

# 1. Problem Statement

Given an integer array `nums`, return all possible subsets.

Example:

    nums = [1,2,3]

Possible subsets:

    []
    [1]
    [2]
    [3]
    [1,2]
    [1,3]
    [2,3]
    [1,2,3]

Order does not matter.

---

# 2. Simple Language Mein Problem

Hume array ke saare possible combinations nikalne hain.

Har element ke paas sirf 2 choices hain:

    1. Element ko lena hai
    2. Element ko nahi lena hai

Example:

    nums = [1,2]

Pehle `1` ke liye:

    Take 1
    Skip 1

Uske baad `2` ke liye bhi:

    Take 2
    Skip 2

Isi se saare subsets banenge.

Final:

    [1,2]
    [1]
    [2]
    []

---

# 3. Main Idea

Har element par:

    TAKE
       OR
    SKIP

Phir next element par jao.

Example:

    [1,2]

Start:

    []

For `1`:

    Take 1  → [1]
    Skip 1  → []

For `[1]`:

    Take 2  → [1,2]
    Skip 2  → [1]

For `[]`:

    Take 2  → [2]
    Skip 2  → []

---

# 4. Recursion Tree

For:

    nums = [1,2]

Tree:

                    []
                  /    \
             Take 1    Skip 1
                [1]       []
               /  \      /  \
          Take 2 Skip 2 Take 2 Skip 2
           [1,2]   [1]   [2]    []

Final subsets:

    [1,2]
    [1]
    [2]
    []

---

# 5. Why Recursion?

Har element ka decision independent hai.

Har element ke liye:

    Take
    Skip

Ye same decision recursively next elements ke liye bhi repeat karna hai.

Isliye recursion naturally fit hoti hai.

---

# 6. What We Need To Track

Hum 3 important cheezein use karenge:

## `ans`

    vector<vector<int>> ans;

Ye saare subsets store karega.

Example:

    [1,2]
    [1]
    [2]
    []

---

## `index`

    int index

Ye batata hai ki abhi hum array ke kis element par decision le rahe hain.

Example:

    index = 0 → nums[0]
    index = 1 → nums[1]
    index = 2 → end

---

## `current`

    vector<int> current

Ye abhi jo subset ban raha hai usko store karega.

Example:

    []
    [1]
    [1,2]

---

# 7. Base Case

Base case:

    if(index == nums.size())

Matlab saare elements ka decision ho gaya.

Example:

    nums = [1,2]

Array size:

    2

Agar:

    index = 2

Toh dono elements process ho gaye.

Ab `current` ek complete subset hai.

Example:

    current = [1,2]

So:

    ans.push_back(current);

Then:

    return;

---

# 8. Take Choice

Current element ko subset mein include karna hai.

Code:

    current.push_back(nums[index]);

Example:

    current = []

and:

    nums[index] = 1

After take:

    current = [1]

Ab next element par recursion:

    solve(nums, index + 1, current);

---

# 9. Skip Choice

Ab same element ko nahi lena hai.

Lekin pehle Take wala element `current` mein add ho chuka hai.

Isliye usko remove karna padega:

    current.pop_back();

Example:

    current = [1]

After:

    pop_back()

Current:

    []

Ab hum 1 ko skip kar sakte hain.

Then:

    solve(nums, index + 1, current);

---

# 10. `push_back()` Aur `pop_back()` Ka Actual Meaning

## `push_back()`

    current.push_back(nums[index]);

Means:

    TAKE

Current element ko subset mein add karo.

---

## `pop_back()`

    current.pop_back();

Means:

    TAKE ko undo karo

Taaki Skip wali branch try kar sakein.

---

# 11. Most Important Sequence

Ye sequence bahut important hai:

    current.push_back(nums[index]);

    solve(nums, index + 1, current);

    current.pop_back();

    solve(nums, index + 1, current);

Meaning:

    Take
      ↓
    Recurse
      ↓
    Undo Take
      ↓
    Skip
      ↓
    Recurse

---

# 12. Detailed Dry Run

Input:

    nums = [1,2]

Start:

    current = []
    index = 0

Call:

    solve(nums, 0, current)

---

## Index 0

Current element:

    nums[0] = 1

Current:

    []

2 choices:

    Take 1
    Skip 1

---

# 13. Take 1

Code:

    current.push_back(nums[0]);

Current:

    [1]

Then:

    solve(nums, 1, current);

Now:

    index = 1
    current = [1]

Current element:

    nums[1] = 2

Again 2 choices.

---

# 14. Take 2

    current.push_back(nums[1]);

Current:

    [1,2]

Next:

    solve(nums, 2, current);

Now:

    index = 2

and:

    nums.size() = 2

So base case true.

Add:

    [1,2]

Answer:

    [[1,2]]

Return.

---

# 15. Skip 2

Take 2 branch complete hone ke baad:

    current.pop_back();

Current becomes:

    [1]

Ab 2 ko skip karna hai.

Call:

    solve(nums, 2, current);

Base case.

Add:

    [1]

Answer:

    [[1,2], [1]]

---

# 16. Back To Element 1

Ab 1 ka Take branch complete ho gaya.

Current:

    [1]

Ab:

    current.pop_back();

Current:

    []

Matlab 1 ko undo kar diya.

Now Skip 1 branch.

Call:

    solve(nums, 1, current);

Current:

    []

Index:

    1

---

# 17. 2 Ka Take

Take:

    current.push_back(2);

Current:

    [2]

Next:

    solve(nums, 2, current);

Base case.

Add:

    [2]

Answer:

    [[1,2], [1], [2]]

---

# 18. 2 Ka Skip

Undo:

    current.pop_back();

Current:

    []

Then Skip 2:

    solve(nums, 2, current);

Base case.

Add:

    []

Final:

    [[1,2], [1], [2], []]

---

# 19. Complete Dry Run Tree

    []
     |
     +-------------------+
     |                   |
   Take 1              Skip 1
     |                   |
    [1]                  []
     |                   |
   /   \                /   \
  T     S              T     S
  |     |              |     |
[1,2]  [1]            [2]    []

So final:

    [1,2]
    [1]
    [2]
    []

---

# 20. Code

class Solution {
public:
    vector<vector<int>> ans;

    void solve(vector<int>& nums, int index, vector<int>& current) {

        if(index == nums.size()) {
            ans.push_back(current);
            return;
        }

        // Take
        current.push_back(nums[index]);
        solve(nums, index + 1, current);

        // Skip
        current.pop_back();
        solve(nums, index + 1, current);
    }

    vector<vector<int>> subsets(vector<int>& nums) {

        vector<int> current;

        solve(nums, 0, current);

        return ans;
    }
};

---

# 21. Code Ka Simple Flow

## Step 1

    vector<vector<int>> ans;

Saare final subsets yahan store honge.

---

## Step 2

    vector<int> current;

Ye current subset hai.

Start:

    []

---

## Step 3

    solve(nums, 0, current);

Array ke first element se recursion start.

---

## Step 4

Base case:

    if(index == nums.size())

Saare elements process ho gaye.

Current subset ko answer mein add:

    ans.push_back(current);

---

## Step 5

Take:

    current.push_back(nums[index]);

Current element ko le liya.

---

## Step 6

Next element:

    solve(nums, index + 1, current);

---

## Step 7

Take ko undo:

    current.pop_back();

---

## Step 8

Skip:

    solve(nums, index + 1, current);

---

# 22. `index + 1` Kyu?

Suppose:

    index = 0

Current element:

    nums[0]

Iska decision ho gaya.

Ab next element:

    nums[1]

par jaana hai.

Isliye:

    index + 1

Example:

    index 0 → 1
    index 1 → 2
    index 2 → end

---

# 23. `current` Reference Mein `&` Kyu?

Function:

    void solve(vector<int>& nums, int index, vector<int>& current)

`current` ko reference se pass kar rahe hain.

Matlab wahi same `current` vector use hoga.

Isliye:

    push_back()

aur:

    pop_back()

se same vector ko change aur restore kar sakte hain.

Ye backtracking ke liye useful hai.

---

# 24. `ans.push_back(current)` Kyu?

Jab:

    index == nums.size()

tab `current` complete subset hota hai.

Example:

    current = [1,2]

So:

    ans.push_back(current);

Matlab answer mein `[1,2]` store kar do.

---

# 25. `pop_back()` Sabse Important Kyu?

Suppose:

    current = [1]

Hum 2 ko Take karte hain:

    current = [1,2]

Take branch complete.

Ab 2 ko Skip karna hai.

Agar `pop_back()` nahi karenge:

    current = [1,2]

hi rahega.

Jo galat hai.

Isliye:

    [1,2]
      ↓
    pop_back()
      ↓
    [1]

Ab Skip 2 possible hai.

So:

    push_back = Take

    pop_back = Undo

---

# 26. Take / Skip Pattern

Ye recursion ka bahut important pattern hai:

    Element
       ↓
    Take / Skip
      /     \
   Take     Skip
    ↓         ↓
Next       Next
Index      Index
    ↓         ↓
    Base Case
       ↓
    Answer

---

# 27. LC 17 Se Difference

LC 17:

    digit
      ↓
    multiple letters
      ↓
    choice

Example:

    2 → a,b,c

LC 78:

    element
      ↓
    2 choices

    Take
    Skip

So:

    LC 17 = Multiple Choices

    LC 78 = Take / Skip

---

# 28. LC 78 vs LC 39

Ye difference important hai.

## LC 78 — Subsets

Current element ek baar process hota hai.

Take:

    index + 1

Skip:

    index + 1

---

## LC 39 — Combination Sum

Same number multiple times use kar sakte hain.

Take:

    same index

Skip:

    index + 1

So LC 78 mein:

    Take → index + 1

LC 39 mein:

    Take → same index

---

# 29. Common Mistakes

## Mistake 1

`pop_back()` bhool jaana.

Take ke baad Skip karne ke liye undo zaroori hai.

---

## Mistake 2

Base case bhool jaana.

Correct:

    if(index == nums.size())

---

## Mistake 3

Base case par answer save na karna.

Correct:

    ans.push_back(current);

---

## Mistake 4

`index + 1` na karna.

Har decision ke baad next element par jaana hai.

---

## Mistake 5

`current` ko galat reset karna.

Har branch ko same current vector se proper Take/Undo/Skip flow follow karna hai.

---

# 30. Complexity

Har element ke 2 choices hain:

    Take
    Skip

For `n` elements:

    Total subsets = 2^n

Har subset ki length maximum `n` ho sakti hai.

Therefore:

    Time Complexity = O(n × 2^n)

Output ko store karne ke liye:

    O(n × 2^n)

Recursion depth:

    O(n)

---

# 31. Interview Explanation

"I will use recursion with a Take or Skip choice for every element.

For each index, I first include the element in the current subset and recursively process the next index.

Then I remove that element to restore the previous state and recursively process the next index without taking the element.

When I reach the end of the array, the current vector is one complete subset, so I add it to the answer.

Since every element has two choices, the total number of subsets is 2^n."

---

# 32. Pattern Recognition

Agar question bole:

- Generate all subsets
- All possible selections
- Har element ko lena ya nahi lena
- Every element has two choices

Immediately think:

    Take / Skip Recursion

---

# 33. Mental Model

Har element par bas ye pucho:

    Isko loon?

Agar haan:

    Take

Agar nahi:

    Skip

Then:

    Next element

Finally:

    All elements processed
    ↓
    Save current subset

---

# 34. One-Line Trick

"Har element ko do choices do — Take ya Skip — dono branches ko recursively explore karo, aur end mein current subset ko answer mein store karo."

---

# 35. Final Takeaway

LC 78 ka core:

    Every element
         ↓
    Take / Skip
       /     \
    Take     Skip
      ↓        ↓
   Next      Next
   Index     Index
      ↓        ↓
       Base Case
           ↓
         Answer

Important code:

    current.push_back(nums[index]);

    solve(nums, index + 1, current);

    current.pop_back();

    solve(nums, index + 1, current);

Base case:

    if(index == nums.size()) {
        ans.push_back(current);
        return;
    }

Most important concept:

    push_back() → Take

    pop_back() → Undo Take

    index + 1 → Next element

Final pattern:

    Take / Skip Recursion

Complexity:

    Time = O(n × 2^n)
    Recursion Space = O(n)
