# LeetCode 110 — Balanced Binary Tree

## Problem

Given the root of a binary tree, determine if the tree is height-balanced.

A binary tree is height-balanced if, for every node:

`abs(leftHeight - rightHeight) <= 1`

Matlab kisi bhi node par left subtree aur right subtree ki height ka difference `1` se zyada nahi hona chahiye.

---

# Example — Balanced Tree

        1
       / \
      2   3
     /
    4

At node `1`:

Left height = `2`

Right height = `1`

Difference:

`abs(2 - 1) = 1`

Since:

`1 <= 1`

Tree balanced hai.

---

# Example — Unbalanced Tree

        1
       /
      2
     /
    3
   /
  4

At node `1`:

Left height = `3`

Right height = `0`

Difference:

`abs(3 - 0) = 3`

Since:

`3 > 1`

Tree unbalanced hai.

---

# Main Concept

Is problem ka connection directly **LC 104 — Maximum Depth of Binary Tree** se hai.

LC 104 mein humne seekha:

`height = max(leftHeight, rightHeight) + 1`

LC 110 mein hume height ke saath balance bhi check karna hai.

So har node par:

1. Left subtree ki height nikalo.
2. Right subtree ki height nikalo.
3. Difference check karo.
4. Agar difference `> 1` hai → unbalanced.
5. Otherwise current node ki height return karo.

---

# Important Idea — `-1`

Hum `-1` ko ek special signal ke liye use karenge.

Normal height:

`0, 1, 2, 3, ...`

Special value:

`-1 = Unbalanced`

Matlab `-1` actual height nahi hai.

Ye sirf ek message hai:

**"Is subtree mein imbalance mil gaya."**

---

# Approach

Har node par:

`left = height(left subtree)`

`right = height(right subtree)`

Phir:

`abs(left - right) > 1`

Agar true:

`return -1`

Otherwise:

`return max(left, right) + 1`

---

# Important Optimization

Agar left subtree already unbalanced hai:

`left == -1`

toh hume right subtree ya current node ki balance calculate karne ki zarurat nahi.

Simply:

`return -1`

Same right subtree ke liye bhi.

Isse imbalance milte hi result upar propagate ho jata hai.

---

# C++ Code

    class Solution {
    public:
        int height(TreeNode *root) {
            if (root == NULL) {
                return 0;
            }

            int left = height(root->left);

            if (left == -1) {
                return -1;
            }

            int right = height(root->right);

            if (right == -1) {
                return -1;
            }

            if (abs(right - left) > 1) {
                return -1;
            }

            return max(right, left) + 1;
        }

        bool isBalanced(TreeNode* root) {
            int ans = height(root);

            if (ans == -1) {
                return false;
            }
            else {
                return true;
            }
        }
    };

---

# Code Explanation

## 1. `height()` Function

    int height(TreeNode *root)

Is function ka kaam do cheezein karna hai:

1. Subtree ki height calculate karna.
2. Check karna ki subtree balanced hai ya nahi.

Return values:

`0 or positive number` → balanced

`-1` → unbalanced

---

# 2. Base Case

    if (root == NULL) {
        return 0;
    }

Agar current node `NULL` hai toh koi node nahi hai.

So height:

`0`

---

# 3. Left Height

    int left = height(root->left);

Ye left subtree ki complete height calculate karta hai.

Ye sirf immediate left child ko nahi check karta.

Recursion poore left subtree mein jaati hai.

Example:

        1
       /
      2
     /
    3

At node `1`:

`height(root->left)`

means:

`height(2)`

which internally:

`height(3)`

calculate karega.

---

# 4. Left Already Unbalanced?

    if (left == -1) {
        return -1;
    }

Agar left subtree ne `-1` return kiya:

Meaning:

**Left subtree already unbalanced hai.**

Toh current subtree bhi unbalanced hai.

Isliye:

`return -1`

---

# 5. Right Height

    int right = height(root->right);

Same logic.

Right subtree ki complete height calculate karo.

---

# 6. Right Already Unbalanced?

    if (right == -1) {
        return -1;
    }

Agar right subtree already unbalanced hai:

`return -1`

---

# 7. Balance Check

    if (abs(right - left) > 1) {
        return -1;
    }

Ye main condition hai.

Hum calculate karte hain:

`abs(leftHeight - rightHeight)`

Agar result:

`> 1`

hai, toh current node balanced nahi hai.

Therefore:

`return -1`

---

# Why `abs()`?

Suppose:

`left = 3`

`right = 1`

Difference:

`3 - 1 = 2`

Suppose:

`left = 1`

`right = 3`

Difference:

`1 - 3 = -2`

Hume dono cases mein `2` chahiye.

Isliye:

`abs(left - right)`

use karte hain.

---

# 8. Return Height

    return max(right, left) + 1;

Agar current node balanced hai, toh uski height calculate karo.

Formula:

`max(leftHeight, rightHeight) + 1`

`+1` current node ke liye hai.

Example:

`left = 2`

`right = 1`

Then:

`max(2,1) + 1`

`= 3`

So current subtree ki height `3`.

---

# 9. `isBalanced()`

Tumhara code:

    bool isBalanced(TreeNode* root) {
        int ans = height(root);

        if (ans == -1) {
            return false;
        }
        else {
            return true;
        }
    }

Pehle:

`height(root)`

call hota hai.

Agar result:

`-1`

→ tree unbalanced.

Otherwise:

`0,1,2,3...`

→ tree balanced.

---

# `isBalanced()` ko Simple Form Mein

Tumhara code:

    int ans = height(root);

    if (ans == -1) {
        return false;
    }
    else {
        return true;
    }

Isko logically:

    return height(root) != -1;

ke equivalent samajh sakte ho.

Both are correct.

---

# Detailed Dry Run — Balanced Tree

Tree:

        1
       / \
      2   3
     /
    4

Initial:

`height(1)`

---

## Step 1 — Node 4

Node `4` ke dono children NULL.

So:

`left = 0`

`right = 0`

Difference:

`abs(0 - 0) = 0`

Balanced.

Height:

`max(0,0) + 1 = 1`

Return:

`1`

---

## Tree Again

        1
       / \
      2   3
     /
    4

Now node `2` ko left se:

`left = 1`

Right NULL hai:

`right = 0`

Difference:

`abs(1 - 0) = 1`

Balanced.

Height:

`max(1,0) + 1 = 2`

Return:

`2`

---

## Tree Again

        1
       / \
      2   3
     /
    4

Now node `3`:

Left = `0`

Right = `0`

Difference:

`0`

Height:

`1`

Return:

`1`

---

## Root 1

Ab root ke paas:

`left = 2`

`right = 1`

Difference:

`abs(2 - 1) = 1`

Balanced.

Height:

`max(2,1) + 1`

`= 3`

So:

`height(1) = 3`

`isBalanced()` gets:

`ans = 3`

Since:

`3 != -1`

Return:

`true`

Final answer:

`true`

---

# Detailed Dry Run — Unbalanced Tree

Tree:

        1
       /
      2
     /
    3
   /
  4

---

## Node 4

`left = 0`

`right = 0`

Height:

`1`

---

## Node 3

`left = 1`

`right = 0`

Difference:

`1`

Balanced.

Height:

`2`

---

## Node 2

`left = 2`

`right = 0`

Difference:

`2`

Now:

`2 > 1`

So:

`height(2) = -1`

---

## Node 1

Node `1` receives:

`left = -1`

Immediately:

    if (left == -1) {
        return -1;
    }

So:

`height(1) = -1`

No further checking needed.

Finally:

`isBalanced()`

gets:

`ans = -1`

Therefore:

`return false`

Final answer:

`false`

---

# Recursion Flow

For:

        1
       / \
      2   3
     /
    4

Flow downward:

`height(1)`

↓

`height(2)`

↓

`height(4)`

↓

`NULL`

Then answers come back upward:

`4 → 1`

`2 → 2`

`3 → 1`

`1 → 3`

This is **bottom-up recursion**.

---

# Important Pattern

Ye LC 104 ka extension hai.

## LC 104

Sirf:

`max(left,right) + 1`

## LC 110

Pehle:

`abs(left-right) <= 1`

phir:

`max(left,right) + 1`

So:

**Height + Balance Check**

---

# Memory Trick

Har node par:

`Left Height`

↓

`Right Height`

↓

`Difference`

↓

If `> 1`:

`-1`

Otherwise:

`max(left,right) + 1`

---

# Complexity

Let `n` = number of nodes.

## Time Complexity

Har node ko maximum once process karte hain.

`O(n)`

## Space Complexity

Recursion stack tree ki height ke according hota hai.

`O(h)`

Balanced tree:

`O(log n)`

Skewed tree:

`O(n)`

---

# Interview Explanation

"I calculate the height of the left and right subtrees recursively. At every node, I check whether the absolute difference between the two heights is greater than one. If it is, I return `-1` as a signal that the subtree is unbalanced. Otherwise, I return the height using `max(left, right) + 1`. Finally, if the root returns `-1`, the tree is not balanced."

---

# Key Takeaways

1. Balanced means height difference at every node is at most `1`.
2. Use recursion to calculate subtree heights.
3. `-1` is used as an unbalanced signal.
4. If left or right returns `-1`, propagate `-1`.
5. Otherwise return `max(left,right) + 1`.
6. This is an optimized bottom-up solution.
7. LC 110 is basically LC 104 + balance condition.

---

# LeetCode

Problem: `110`

Name: `Balanced Binary Tree`

Pattern: `Validation`

Technique: `DFS + Bottom-Up Recursion`

Core Formula:

`abs(leftHeight - rightHeight) <= 1`

Height Formula:

`max(leftHeight, rightHeight) + 1`

Difficulty: Easy
