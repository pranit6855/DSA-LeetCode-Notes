# LeetCode 124 — Binary Tree Maximum Path Sum

## Problem

Given the root of a binary tree, return the **maximum path sum** of any non-empty path in the tree.

A path can start and end at **any node**.

Important:

**Path ko root se start karna zaroori nahi hai.**

---

# Example

        -10
        /  \
       9    20
           /  \
          15   7

Best path:

`15 → 20 → 7`

Sum:

`15 + 20 + 7 = 42`

Answer:

`42`

---

# What Is a Path?

A path means:

**Ek node se doosre node tak connected nodes ka sequence.**

Example:

        20
       /  \
      15   7

Possible path:

`15 → 20 → 7`

But a path cannot branch.

For example, parent tak jaate hue hum ek node se left aur right dono branches simultaneously continue nahi kar sakte.

---

# Main Concept

Har node par hum do different calculations karte hain:

## 1. Global Answer

Current node ke through complete path bana sakte hain:

`left → root → right`

So:

`left + root->val + right`

This can use **both sides**.

---

## 2. Parent Ko Return

Agar current node ka parent hai, toh path parent se current node ke through sirf **ek side** mein continue ho sakta hai.

So:

`root->val + max(left, right)`

This uses only **one side**.

---

# Most Important Rule

## Global Answer

    left + root->val + right

Dono sides allowed hain.

## Parent Ko Return

    root->val + max(left, right)

Sirf ek side allowed hai.

---

# Why?

Example:

        10
          \
           20
          /  \
         15   7

At node `20`:

Left contribution = `15`

Right contribution = `7`

Complete path through `20`:

`15 → 20 → 7`

Sum:

`15 + 20 + 7 = 42`

So global answer candidate:

`42`

But if `20` has to connect with its parent `10`, the path can be:

`10 → 20 → 15`

or:

`10 → 20 → 7`

It cannot use both branches and still remain a single path.

Therefore return:

`20 + max(15,7)`

`= 35`

---

# Negative Values

Tree mein negative values bhi ho sakti hain.

Example:

        5
       /
     -10

`-10` ko include karne se sum decrease hoga.

So negative contribution ko ignore kar sakte hain.

Use:

    max(0, contribution)

Meaning:

- positive contribution → use it
- negative contribution → ignore it by taking `0`

---

# Why `max(0, solve(...))`?

Suppose:

`left = -5`

Using `-5` is harmful.

So:

    max(0, -5)

gives:

`0`

Matlab:

**Left side mat lo.**

Suppose:

`left = 8`

Then:

    max(0, 8)

gives:

`8`

Matlab:

**Left side use karo.**

---

# Approach

Har node par:

1. Left subtree ka maximum contribution nikalo.
2. Right subtree ka maximum contribution nikalo.
3. Negative contributions ko ignore karo.
4. Current node ke through complete path calculate karo.
5. Global maximum answer update karo.
6. Parent ko current node ka best one-side contribution return karo.

Flow:

`Left Contribution`

↓

`Right Contribution`

↓

`Global Path = Left + Current + Right`

↓

`Update Answer`

↓

`Return = Current + max(Left, Right)`

---

# C++ Code

    class Solution {
    public:
        int ans = INT_MIN;

        int solve(TreeNode* root) {
            if (root == NULL)
                return 0;

            int left = max(0, solve(root->left));

            int right = max(0, solve(root->right));

            // Current node ke through complete path
            ans = max(ans, left + root->val + right);

            // Parent ko sirf ek best side deni hai
            return root->val + max(left, right);
        }

        int maxPathSum(TreeNode* root) {
            solve(root);
            return ans;
        }
    };

---

# Code Explanation

## 1. Global Answer

    int ans = INT_MIN;

Ye poori tree ka maximum path sum store karega.

`INT_MIN` se initialize karte hain because tree ki values negative bhi ho sakti hain.

Example:

        -5
        /
      -10
      /
    -20

Answer negative ho sakta hai.

So `0` se initialize karna wrong ho sakta hai.

---

# 2. `solve()` Function

    int solve(TreeNode* root)

Ye function:

1. Current subtree ka best contribution parent ko return karta hai.
2. Har node par global maximum update karta hai.

---

# 3. Base Case

    if (root == NULL)
        return 0;

NULL subtree ka contribution:

`0`

Matlab koi node nahi hai, so koi contribution nahi.

---

# 4. Left Contribution

    int left = max(0, solve(root->left));

Pehle left subtree ka best contribution calculate karo.

Agar contribution negative hai:

`max(0, negative) = 0`

So negative branch ignore ho jaayegi.

---

# 5. Right Contribution

    int right = max(0, solve(root->right));

Same logic right subtree ke liye.

---

# 6. Current Path

    ans = max(ans, left + root->val + right);

Ye sabse important line hai.

Current node ko center maan ke complete path:

`left → root → right`

ka sum calculate hota hai.

Then global answer ke saath compare karte hain.

---

# 7. Parent Ko Return

    return root->val + max(left, right);

Ye line alag purpose ke liye hai.

Parent ke path mein current node se sirf ek branch continue ho sakti hai.

Isliye:

`max(left, right)`

use karte hain.

---

# Golden Difference

## Answer ke liye

`left + root + right`

Dono sides allowed.

## Parent ke liye

`root + max(left,right)`

Sirf one side.

---

# Detailed Dry Run

Tree:

        -10
        /  \
       9    20
           /  \
          15   7

Initial:

`ans = INT_MIN`

---

## Step 1 — Node 9

Node `9` leaf hai.

Left:

`0`

Right:

`0`

Current path:

`0 + 9 + 0`

`= 9`

So:

`ans = 9`

Return to parent:

`9 + max(0,0)`

`= 9`

So:

`solve(9) = 9`

---

# Tree Again

        -10
        /  \
       9    20
           /  \
          15   7

Now root `-10` ko:

`left = 9`

mil gaya.

---

# Step 2 — Node 15

Node `15` leaf hai.

Left = `0`

Right = `0`

Current path:

`15`

So:

`ans = max(9,15)`

`= 15`

Return:

`15`

So:

`solve(15) = 15`

---

# Step 3 — Node 7

Node `7` leaf hai.

Left = `0`

Right = `0`

Current path:

`7`

Global:

`ans = max(15,7)`

`= 15`

Return:

`7`

So:

`solve(7) = 7`

---

# Tree Again

        -10
        /  \
       9    20
           /  \
          15   7

Node `20` ko:

`left = 15`

`right = 7`

mil gaya.

---

# Step 4 — Node 20

Current path through 20:

`15 → 20 → 7`

Sum:

`15 + 20 + 7`

`= 42`

Update:

`ans = max(15,42)`

`= 42`

So global answer candidate is `42`.

---

# Node 20 Se Parent Ko Kya Return Hoga?

Parent is `-10`.

20 cannot give both `15` and `7` to its parent.

So:

`20 + max(15,7)`

`= 20 + 15`

`= 35`

Therefore:

`solve(20) = 35`

Meaning:

**"Parent, meri best single-side contribution 35 hai."**

---

# Tree Again

        -10
        /  \
       9    20
           /  \
          15   7

Root `-10` ko:

`left = 9`

`right = 35`

mil gaya.

---

# Step 5 — Node -10

Current path through `-10`:

`9 → -10 → 20 → 15`

Sum:

`9 + (-10) + 35`

`= 34`

Notice 35 already represents:

`20 + 15`

So current path:

`9 + (-10) + 20 + 15`

`= 34`

Update:

`ans = max(42,34)`

`= 42`

So `42` remains the best.

---

# Final Answer

`42`

Best path:

`15 → 20 → 7`

Sum:

`15 + 20 + 7 = 42`

---

# Full Recursion Flow

        -10
        /  \
       9    20
           /  \
          15   7

Downward:

`solve(-10)`

↓

`solve(9)`

↓

return `9`

Then:

`solve(20)`

↓

`solve(15)`

↓

return `15`

↓

`solve(7)`

↓

return `7`

Then node `20`:

`left = 15`

`right = 7`

Global path:

`15 + 20 + 7 = 42`

Return to parent:

`20 + max(15,7) = 35`

Then node `-10`:

`left = 9`

`right = 35`

Global path:

`9 + (-10) + 35 = 34`

Final global answer remains:

`42`

---

# Why `ans` Is Global?

Best path root ke through aana zaroori nahi.

Example:

        -10
        /  \
       9    20
           /  \
          15   7

Best path:

`15 → 20 → 7`

Root `-10` is not part of it.

Isliye har node par:

    ans = max(ans, left + root->val + right);

calculate karna padega.

---

# Negative Contribution Example

Tree:

        5
       /
     -10

At `-10`:

Contribution = `-10`

But:

    max(0, -10)

`= 0`

At node `5`:

Left contribution = `0`

Right contribution = `0`

Current path:

`0 + 5 + 0`

`= 5`

So negative branch ignore ho gayi.

Answer:

`5`

---

# Important Warning About All-Negative Trees

Suppose:

        -3
       /  \
     -2   -5

Correct answer:

`-2`

Not `0`.

That's why:

    int ans = INT_MIN;

use karna important hai.

We only use `max(0, contribution)` for child contributions.

But the global answer must still consider negative node values.

---

# `0` Ka Meaning

When we write:

    int left = max(0, solve(root->left));

`0` ka matlab:

**"Is side se koi useful contribution nahi lena."**

Ye actual tree height or actual node value nahi hai.

---

# Most Important Formula

## Global Path

    left + root->val + right

Example:

`15 + 20 + 7 = 42`

---

## Parent Return

    root->val + max(left,right)

Example:

`20 + max(15,7) = 35`

---

# Why Can't Parent Take Both Sides?

Suppose:

        10
          \
           20
          /  \
         15   7

Agar parent ko return karte:

`20 + 15 + 7`

Then parent ke saath add karne par:

`10 + 20 + 15 + 7`

Ye actual path nahi hai because it branches at `20`.

A path is a single chain.

Therefore parent ko only one branch return karni hai.

---

# Backtracking Is Not Used Here

LC 113 mein humne:

`push_back`

and

`pop_back`

use kiya because actual path store karna tha.

LC 124 mein hume actual path store nahi karna.

So:

**No path vector**

**No push_back/pop_back**

We only calculate numerical contributions.

---

# Connection With Previous Questions

## LC 104 — Maximum Depth

`max(left,right) + 1`

## LC 110 — Balanced Tree

Use left/right heights and check difference.

## LC 543 — Diameter

`left + right`

and return:

`max(left,right) + 1`

## LC 124 — Maximum Path Sum

`left + root + right`

and return:

`root + max(left,right)`

So LC 124 uses the same **bottom-up recursion** idea but with path sums.

---

# Memory Trick

Remember two lines:

    ans = max(ans, left + root->val + right);

and:

    return root->val + max(left, right);

### Story

**Answer ke liye:**

"Current node ke dono bachchon ko mila ke longest path bana sakta hoon."

**Parent ko:**

"Main ek hi side leke parent ke saath continue kar sakta hoon."

---

# Interview Explanation

"I use bottom-up DFS. For each node, I calculate the maximum contribution from the left and right subtrees, ignoring negative contributions. The best path passing through the current node can use both sides, so I update the global answer with `left + node + right`. However, when returning to the parent, the path can continue through only one branch, so I return `node + max(left, right)`."

---

# Complexity

Let `n` = number of nodes.

## Time Complexity

Every node is visited once.

`O(n)`

## Space Complexity

Recursion stack depends on tree height `h`.

`O(h)`

Balanced tree:

`O(log n)`

Skewed tree:

`O(n)`

---

# Key Takeaways

1. Path can start and end at any node.
2. Negative child contributions can be ignored using `max(0, gain)`.
3. Global answer can use both left and right.
4. Parent return can use only one side.
5. `ans` must start from `INT_MIN` because all values can be negative.
6. This is a bottom-up DFS / recursion pattern.

---

# Pattern 5 Progress

Pattern 5 — Path Sum

`112 — Path Sum ✅`

`113 — Path Sum II ✅`

`124 — Binary Tree Maximum Path Sum ✅`

**Pattern 5 = COMPLETE**

---

# Tree Overall Progress

Pattern 1 — Traversal ✅

Pattern 2 — Mirror & Symmetry ✅

Pattern 3 — Search / BST ✅

Pattern 4 — Validation ✅

Pattern 5 — Path Sum ✅

Pattern 6 — Construction ⏳

Remaining:

`105 — Construct Binary Tree from Preorder + Inorder`

`106 — Construct Binary Tree from Postorder + Inorder`

`108 — Convert Sorted Array to BST`

---

# LeetCode

Problem: `124`

Name: `Binary Tree Maximum Path Sum`

Pattern: `Path Sum`

Technique: `DFS + Bottom-Up Recursion + Global Maximum`

Core Formula:

`Global = left + root + right`

`Return = root + max(left,right)`

Difficulty: Hard
