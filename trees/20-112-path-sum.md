# LeetCode 112 — Path Sum

## Problem

Given the root of a binary tree and an integer `targetSum`, determine if the tree has a **root-to-leaf path** such that the sum of all node values along the path equals `targetSum`.

Return:

`true` → agar aisa path milta hai

`false` → agar nahi milta

---

# Root-to-Leaf Path

Root-to-leaf path ka matlab:

**Root se start karke kisi leaf node tak complete path.**

Example:

        5
       / \
      3   8
     / \
    2   4

Valid root-to-leaf paths:

`5 → 3 → 2`

`5 → 3 → 4`

`5 → 8`

But:

`5 → 3`

valid root-to-leaf path nahi hai because `3` leaf node nahi hai.

---

# Main Concept

Har node par current `targetSum` se current node ki value subtract karte jao.

Example:

Path:

`5 → 3 → 2`

Initial:

`targetSum = 10`

At `5`:

`10 - 5 = 5`

At `3`:

`5 - 3 = 2`

At `2`:

`2 - 2 = 0`

Ab `2` leaf hai aur remaining target `0` hai.

So path valid hai.

---

# Core Idea

Har node par:

`targetSum -= root->val`

Then check karo:

**Kya current node leaf hai?**

Agar leaf hai:

`targetSum == 0`

→ `true`

Otherwise:

Left aur right subtree mein recursively search karo.

---

# Why Leaf Check Is Important?

Sirf target `0` ho jaana enough nahi hai.

Hume **root-to-leaf** path chahiye.

Example:

        5
       /
      5
     /
    2

Agar kisi middle node par target sum temporarily complete ho bhi jaye, lekin current node leaf nahi hai, toh path abhi complete nahi hua.

Isliye final check leaf par hota hai.

---

# Approach

1. Agar root `NULL` hai → `false`
2. Current node ki value target se subtract karo.
3. Agar current node leaf hai:
   - `targetSum == 0` → `true`
   - otherwise → `false`
4. Agar leaf nahi hai:
   - left subtree mein check karo
   - right subtree mein check karo
5. Left ya right mein path mil jaye → `true`

---

# C++ Code

    class Solution {
    public:
        bool hasPathSum(TreeNode* root, int targetSum) {
            if (root == NULL)
                return false;

            // Current node ko target se subtract karo
            targetSum -= root->val;

            // Leaf node
            if (root->left == NULL && root->right == NULL) {
                return targetSum == 0;
            }

            // Left ya right subtree mein path dhundo
            return hasPathSum(root->left, targetSum) ||
                   hasPathSum(root->right, targetSum);
        }
    };

---

# Code Explanation

## 1. Base Case

    if (root == NULL)
        return false;

Agar current node hi nahi hai, toh koi path nahi ho sakta.

So:

`false`

---

# 2. Current Node Ki Value Subtract

    targetSum -= root->val;

Ye current node ko path sum mein include kar raha hai.

Example:

`targetSum = 10`

Current node:

`5`

After subtraction:

`targetSum = 5`

---

# 3. Leaf Node Check

    if (root->left == NULL && root->right == NULL)

Ye check karta hai ki current node leaf hai ya nahi.

Leaf node:

- left = NULL
- right = NULL

Agar leaf mil gayi, toh ab path complete ho gaya.

---

# 4. Leaf Par Target Check

    return targetSum == 0;

Agar remaining target:

`0`

hai, toh root-to-leaf path ka sum exactly target tha.

So:

`true`

Otherwise:

`false`

---

# 5. Left / Right Recursion

    return hasPathSum(root->left, targetSum) ||
           hasPathSum(root->right, targetSum);

Current node process ho chuka hai.

Ab check karo:

- left subtree mein valid path hai?
- OR right subtree mein valid path hai?

Agar kisi ek side mein mil gaya:

`true`

Dono side fail:

`false`

---

# Why `||`?

`||` means OR.

Possible cases:

`true || false = true`

`false || true = true`

`false || false = false`

So:

**Left ya right kisi bhi side valid path mil gaya toh answer true.**

---

# Detailed Dry Run

Tree:

        5
       / \
      3   8
     / \
    2   4

Target:

`10`

Expected answer:

`true`

Because:

`5 + 3 + 2 = 10`

---

## Step 1 — Node 5

Current:

`5`

Initial target:

`10`

Subtract:

`10 - 5 = 5`

Now:

`targetSum = 5`

5 leaf nahi hai.

So go left or right.

---

## Tree Again

        5
       / \
      3   8
     / \
    2   4

Current:

`5`

Remaining target:

`5`

---

# Step 2 — Node 3

We go left.

Current:

`3`

Before:

`targetSum = 5`

Subtract:

`5 - 3 = 2`

Now:

`targetSum = 2`

3 leaf nahi hai.

So:

`hasPathSum(3->left, 2)`

---

# Tree Again

        5
       / \
      3   8
     / \
    2   4

Current:

`3`

Remaining target:

`2`

---

# Step 3 — Node 2

Current:

`2`

Before:

`targetSum = 2`

Subtract:

`2 - 2 = 0`

Now:

`targetSum = 0`

Node `2` leaf hai:

`left = NULL`

`right = NULL`

So condition:

`targetSum == 0`

True.

Therefore:

`return true`

---

# Final Answer

Path:

`5 → 3 → 2`

Sum:

`5 + 3 + 2 = 10`

So:

`true`

---

# Full Tree After Successful Path

        5
       / \
      3   8
     / \
    2   4

Successful path:

`5 → 3 → 2`

Remaining target:

`10 → 5 → 2 → 0`

---

# Detailed Dry Run — Path Doesn't Match

Same tree:

        5
       / \
      3   8
     / \
    2   4

Suppose:

`targetSum = 11`

Try path:

`5 → 3 → 2`

Calculations:

`11 - 5 = 6`

`6 - 3 = 3`

`3 - 2 = 1`

Node `2` leaf hai.

But:

`targetSum = 1`

not `0`.

Therefore:

`false`

Then recursion may check another path such as:

`5 → 3 → 4`

Sum:

`12`

So that also doesn't work.

If no root-to-leaf path gives `11`:

Final answer:

`false`

---

# Important: Target Ko "Remaining Target" Samjho

Har recursion call mein `targetSum` actually:

**Kitna sum abhi aur chahiye**

represent karta hai.

Example:

Path:

`5 → 3 → 2`

Starting target:

`10`

After 5:

`5`

After 3:

`2`

After 2:

`0`

So:

`0` means exactly complete.

---

# Why Subtract Before Leaf Check?

Humne current node ki value ko pehle target mein include kiya:

    targetSum -= root->val;

Then leaf par check kiya:

    targetSum == 0

Isliye leaf ka value bhi automatically sum mein include ho gaya.

Example:

Target:

`10`

Leaf:

`2`

Remaining before leaf:

`2`

Subtract:

`2 - 2 = 0`

Then valid.

---

# Recursion Flow

Tree:

        5
       / \
      3   8
     / \
    2   4

Target:

`10`

Flow:

`hasPathSum(5,10)`

↓

`5` subtract

`10 → 5`

↓

`hasPathSum(3,5)`

↓

`3` subtract

`5 → 2`

↓

`hasPathSum(2,2)`

↓

`2` subtract

`2 → 0`

↓

Leaf + target `0`

↓

`true`

---

# Why We Check Leaf?

Question specifically asks:

**root-to-leaf path**

So if current node is not leaf, we cannot return true just because target happened to become zero.

We need to reach the leaf.

---

# Pattern Connection

This question uses:

**Tree Recursion + Root-to-Leaf Path + Remaining Target**

Pattern:

`Current value subtract karo`

↓

`Leaf?`

↓

`Target complete?`

↓

`Left / Right`

---

# Comparison With Previous Patterns

## LC 104 — Maximum Depth

`max(left, right) + 1`

## LC 110 — Balanced Tree

`height + balance check`

## LC 543 — Diameter

`left + right`

## LC 98 — Validate BST

`range / bounds`

## LC 112 — Path Sum

`remaining target + leaf check`

---

# Common Mistake

Wrong:

    if (targetSum == 0)
        return true;

Because target `0` middle node par bhi aa sakta hai.

Correct:

    if (root->left == NULL && root->right == NULL) {
        return targetSum == 0;
    }

First ensure:

**current node is leaf**

Then check:

**remaining target is 0**

---

# Complexity

Let `n` = number of nodes.

## Time Complexity

Worst case mein poori tree traverse ho sakti hai:

`O(n)`

## Space Complexity

Recursive stack tree ki height `h` tak ja sakta hai.

`O(h)`

Balanced tree:

`O(log n)`

Skewed tree:

`O(n)`

---

# Interview Explanation

"I recursively traverse the tree while maintaining the remaining target sum. At each node, I subtract its value from the target. When I reach a leaf node, I check whether the remaining target is zero. If yes, a valid root-to-leaf path exists. Otherwise, I continue searching in the left and right subtrees."

---

# Memory Trick

**Path Sum = Subtract + Leaf Check**

Har node:

`target -= value`

Leaf par:

`target == 0`

Then:

`true`

---

# LeetCode

Problem: `112`

Name: `Path Sum`

Pattern: `Path Sum`

Technique: `DFS + Remaining Target + Leaf Check`

Difficulty: Easy
