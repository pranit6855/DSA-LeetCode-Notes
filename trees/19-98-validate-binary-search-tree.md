# LeetCode 98 — Validate Binary Search Tree

## Problem

Given the root of a binary tree, determine whether it is a valid Binary Search Tree (BST).

A valid BST follows:

`left subtree values < root value < right subtree values`

But this condition **sirf immediate children** ke liye nahi hoti.

Har node ko apne ancestors se milne wali complete valid range follow karni hoti hai.

---

# Example — Valid BST

        5
       / \
      3   7
     / \   \
    2   4   8

Is tree mein:

- `3 < 5`
- `7 > 5`
- `2 < 3`
- `4 > 3` and `4 < 5`
- `8 > 7` and `8 > 5`

So tree valid BST hai.

Answer:

`true`

---

# Important Trap

Sirf parent ke saath comparison karna enough nahi hai.

Example:

        5
       / \
      3   7
     / \
    2   6

Agar hum sirf immediate parent compare karein:

`6 > 3`

Toh sahi lagega.

Lekin `6`, root `5` ke **left subtree** mein hai.

Isliye usko:

`6 < 5`

hona chahiye.

But:

`6 > 5`

So tree invalid hai.

Answer:

`false`

---

# Main Concept — Range / Bounds

Har node ko ek allowed range milegi.

Function ko hum 3 cheezein denge:

`root`

`minVal`

`maxVal`

Matlab:

**Current node ki value `minVal` aur `maxVal` ke beech honi chahiye.**

Condition:

`minVal < root->val < maxVal`

---

# Root ki Range

Root ke liye koi restriction nahi hoti.

So:

`(-∞, +∞)`

Code:

    solve(root, LLONG_MIN, LLONG_MAX)

---

# Left Subtree ki Range

Agar current node:

`5`

hai, toh left subtree mein sab values:

`5 se chhoti`

honi chahiye.

So left ki range:

`(minVal, 5)`

Code:

    solve(root->left, minVal, root->val)

---

# Right Subtree ki Range

Agar current node:

`5`

hai, toh right subtree mein sab values:

`5 se badi`

honi chahiye.

So right ki range:

`(5, maxVal)`

Code:

    solve(root->right, root->val, maxVal)

---

# Range Narrow Kaise Hoti Hai?

Example:

        5
       / \
      3   7
     / \   \
    2   4   8

## Root 5

Range:

`(-∞, +∞)`

## Left 3

Range:

`(-∞, 5)`

## 3 ka Left 2

Range:

`(-∞, 3)`

## 3 ka Right 4

Range:

`(3, 5)`

## Right 7

Range:

`(5, +∞)`

## 7 ka Right 8

Range:

`(7, +∞)`

Har node apni range ke andar hona chahiye.

---

# Core Approach

Har node par:

1. Agar node `NULL` hai → `true`
2. Check karo current value valid range mein hai ya nahi.
3. Agar range ke bahar hai → `false`
4. Left subtree ko updated range do.
5. Right subtree ko updated range do.
6. Left aur right dono valid hone chahiye.

Flow:

`Current Node`

↓

`Range Check`

↓

Invalid?

`YES → false`

`NO → Left + Right`

↓

`left && right`

---

# C++ Code

    class Solution {
    public:
        bool solve(TreeNode* root, long long minVal, long long maxVal) {
            // NULL node valid hai
            if (root == NULL)
                return true;

            // Current node allowed range ke bahar hai
            if (root->val <= minVal || root->val >= maxVal)
                return false;

            // Left subtree
            // Range: (minVal, current value)
            bool left = solve(root->left, minVal, root->val);

            // Right subtree
            // Range: (current value, maxVal)
            bool right = solve(root->right, root->val, maxVal);

            // Dono subtree valid hone chahiye
            return left && right;
        }

        bool isValidBST(TreeNode* root) {
            return solve(root, LLONG_MIN, LLONG_MAX);
        }
    };

---

# Code Explanation

## 1. `solve()` Function

    bool solve(TreeNode* root, long long minVal, long long maxVal)

Function ko:

- current node
- minimum allowed value
- maximum allowed value

mil raha hai.

Ye `true` ya `false` return karega.

`true`:

Subtree valid BST hai.

`false`:

Subtree invalid hai.

---

# 2. Base Case

    if (root == NULL)
        return true;

Agar node NULL hai, toh wahan BST violation possible nahi hai.

So `true`.

---

# 3. Range Check

    if (root->val <= minVal || root->val >= maxVal)
        return false;

Ye sabse important condition hai.

Current value valid tab hai jab:

`minVal < root->val < maxVal`

Agar:

`root->val <= minVal`

ya:

`root->val >= maxVal`

toh range break ho gayi.

So:

`return false`

---

# `||` Ka Meaning

`||` means **OR**.

So:

    root->val <= minVal || root->val >= maxVal

ka meaning:

**Current value minimum se bahut chhoti/equal hai OR maximum se bahut badi/equal hai.**

Dono mein se ek bhi condition true hui toh node invalid hai.

---

# Example

Range:

`(3, 5)`

Current value:

`4`

Check:

`4 <= 3` → false

`4 >= 5` → false

So:

`false || false = false`

Isliye `if` execute nahi hoga.

`4` valid hai.

---

# Invalid Example

Range:

`(3, 5)`

Current value:

`6`

Check:

`6 <= 3` → false

`6 >= 5` → true

So:

`false || true = true`

Therefore:

    return false;

Matlab:

**Current node invalid hai.**

---

# 4. Left Subtree

    bool left = solve(root->left, minVal, root->val);

Suppose current value:

`5`

Left subtree ke liye maximum allowed value `5` hogi.

So:

`left range = (minVal, 5)`

Example:

Root:

        5
       /
      3

3 ko:

`(-∞, 5)`

range milegi.

---

# 5. Right Subtree

    bool right = solve(root->right, root->val, maxVal);

Suppose current value:

`5`

Right subtree ke liye minimum allowed value `5` hogi.

So:

`right range = (5, maxVal)`

Example:

        5
          \
           7

7 ko:

`(5, +∞)`

range milegi.

---

# 6. `left && right`

    return left && right;

`&&` means **AND**.

Dono subtree valid hone chahiye.

Examples:

`true && true = true`

`true && false = false`

`false && true = false`

`false && false = false`

So:

**Ek bhi subtree invalid hua toh poori tree invalid.**

---

# Important Difference: `||` vs `&&`

Range check:

    if (root->val <= minVal || root->val >= maxVal)

Yahan `||` because:

**Value left side se range break kar sakti hai OR right side se.**

Final:

    return left && right;

Yahan `&&` because:

**Left bhi valid AND right bhi valid hona chahiye.**

---

# Detailed Dry Run — Valid BST

Tree:

        5
       / \
      3   7
     / \   \
    2   4   8

Initial:

    isValidBST(root)

Calls:

    solve(5, LLONG_MIN, LLONG_MAX)

---

## Step 1 — Node 5

Current:

`5`

Range:

`(-∞, +∞)`

5 valid hai.

Now left:

    solve(3, -∞, 5)

---

# Step 2 — Node 3

Current:

`3`

Allowed range:

`(-∞, 5)`

3 valid hai.

Now left:

    solve(2, -∞, 3)

---

# Step 3 — Node 2

Current:

`2`

Range:

`(-∞, 3)`

2 valid hai.

Left = NULL:

`true`

Right = NULL:

`true`

So:

`true && true = true`

Node 2 returns:

`true`

---

# Tree Again

        5
       / \
      3   7
     / \   \
    2   4   8

Ab node 3 ko left se:

`true`

mil gaya.

Now right:

    solve(4, 3, 5)

---

# Step 4 — Node 4

Range:

`(3, 5)`

Current:

`4`

Check:

`3 < 4 < 5`

Valid.

Children NULL.

So:

`solve(4) = true`

Node 3:

`left = true`

`right = true`

Therefore:

`true && true = true`

So:

`solve(3) = true`

---

# Tree Again

        5
       / \
      3   7
     / \   \
    2   4   8

Root 5 ko left subtree se:

`true`

mil gaya.

Now right:

    solve(7, 5, +∞)

---

# Step 5 — Node 7

Range:

`(5, +∞)`

Current:

`7`

Valid.

Left = NULL:

`true`

Right:

    solve(8, 7, +∞)

---

# Step 6 — Node 8

Range:

`(7, +∞)`

Current:

`8`

Valid.

Children NULL.

So:

`solve(8) = true`

Node 7:

`true && true = true`

So:

`solve(7) = true`

---

# Final Root 5

Root ko:

`left = true`

`right = true`

mil gaya.

Final:

`true && true`

`= true`

So:

`isValidBST(root) = true`

---

# Detailed Dry Run — Invalid BST

Tree:

        5
       / \
      3   7
     / \
    2   6

Problem node:

`6`

---

## Step 1 — Node 5

Range:

`(-∞, +∞)`

Valid.

Left:

    solve(3, -∞, 5)

---

# Step 2 — Node 3

Range:

`(-∞, 5)`

Valid.

3 ka right:

    solve(6, 3, 5)

---

# Step 3 — Node 6

Allowed range:

`(3, 5)`

Current:

`6`

Check:

`6 <= 3` → false

`6 >= 5` → true

So:

`false || true = true`

Therefore:

    return false;

So:

`solve(6) = false`

---

# False Kaise Upar Jaata Hai?

Node 3 ko right se:

`false`

milta hai.

Left maan lo:

`true`

Then:

`true && false`

`= false`

So:

`solve(3) = false`

Root 5 ko left se:

`false`

milta hai.

Then eventually:

`isValidBST() = false`

---

# Important Observation

Ye invalid tree:

        5
       / \
      3   7
     / \
    2   6

Node 6 ke liye:

`6 > 3`

but:

`6 < 5`

hona required tha because 6 root 5 ke left subtree mein hai.

Allowed range:

`(3,5)`

Actual:

`6`

So invalid.

---

# Why Parent Comparison Is Not Enough

Wrong thinking:

`6 > 3`

Therefore valid.

Correct thinking:

`3 < 6 < 5`

Ye false hai.

Isliye range/bounds method use karte hain.

---

# Why `long long`?

Code mein:

    bool solve(TreeNode* root, long long minVal, long long maxVal)

use kiya hai.

Aur starting:

    solve(root, LLONG_MIN, LLONG_MAX)

Reason:

Agar tree mein `INT_MIN` ya `INT_MAX` actual node values ho sakti hain, toh `int` boundary use karne se edge cases aa sakte hain.

`long long` safe range provide karta hai.

---

# Recursion Pattern

Current Node:

`5`

Left:

`(-∞, 5)`

Right:

`(5, +∞)`

If left node = `3`:

Left of 3:

`(-∞, 3)`

Right of 3:

`(3, 5)`

So range recursively narrow hoti jaati hai.

---

# Main Memory Trick

**BST Validate = Range Check**

Har node ke paas:

`min < value < max`

Then:

Left:

`(min, value)`

Right:

`(value, max)`

---

# One-Line Algorithm

`Current value range mein hai?`

↓

No → `false`

Yes →

`Left validate`

AND

`Right validate`

---

# Complexity

Let `n` = number of nodes.

## Time Complexity

Har node maximum ek baar visit hota hai.

`O(n)`

## Space Complexity

Recursion stack tree ki height `h` tak ja sakta hai.

`O(h)`

Balanced tree:

`O(log n)`

Skewed tree:

`O(n)`

---

# Interview Explanation

"I validate the BST using recursive range constraints. Each node is given a valid range. For the left subtree, the maximum bound becomes the current node value, and for the right subtree, the minimum bound becomes the current node value. If any node falls outside its allowed range, I return false. Finally, both left and right subtrees must be valid."

---

# Pattern 4 Complete

Pattern 4 — Validation

`104 — Maximum Depth of Binary Tree ✅`

`110 — Balanced Binary Tree ✅`

`543 — Diameter of Binary Tree ✅`

`98 — Validate Binary Search Tree ✅`

So:

**Pattern 4 = COMPLETE**

---

# Tree Overall Progress

```text
Pattern 1 — Traversal             ✅
Pattern 2 — Mirror & Symmetry     ✅
Pattern 3 — Search / BST          ✅
Pattern 4 — Validation            ✅

Pattern 5 — Path Sum              ⏳
Pattern 6 — Construction          ⏳
