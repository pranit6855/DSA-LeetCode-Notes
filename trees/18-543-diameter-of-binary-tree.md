# LeetCode 543 — Diameter of Binary Tree

## Problem

Given the root of a binary tree, return the **diameter** of the tree.

The diameter is the length of the **longest path between any two nodes** in the tree.

Important:

**LeetCode diameter ko number of edges mein count karta hai.**

---

# Example

Tree:

        1
       / \
      2   3
     / \
    4   5

Longest path:

`4 → 2 → 1 → 3`

Edges:

`4 → 2 = 1`

`2 → 1 = 1`

`1 → 3 = 1`

Total:

`3`

So:

`Diameter = 3`

---

# Main Concept

Ye problem directly **LC 104 — Maximum Depth of Binary Tree** se connected hai.

LC 104 mein humne height nikali:

`height = max(leftHeight, rightHeight) + 1`

LC 543 mein hume har node par do cheezein karni hain:

1. Current node ke through diameter calculate karna.
2. Current subtree ki height parent ko return karna.

---

# Diameter Through a Node

Maan lo current node:

        2
       / \
      4   5

Left subtree ki height:

`1`

Right subtree ki height:

`1`

Current node ke through longest path:

`4 → 2 → 5`

Diameter:

`leftHeight + rightHeight`

`= 1 + 1`

`= 2`

---

# Important Formula

## Diameter Through Current Node

`leftHeight + rightHeight`

## Current Node Ki Height

`max(leftHeight, rightHeight) + 1`

So ek node par:

`diameterCandidate = left + right`

and

`height = max(left, right) + 1`

---

# Why `+1` Diameter Mein Nahi?

Diameter **edges** mein count hota hai.

Example:

        1
       / \
      2   3
     /
    4

Path:

`4 → 2 → 1 → 3`

Edges:

`3`

Node heights:

Left subtree of `1` ki height = `2`

Right subtree of `1` ki height = `1`

So:

`left + right = 2 + 1 = 3`

Correct.

Agar `+1` karte:

`4`

jo wrong hota.

---

# Approach

Har node par:

1. Left subtree ki height nikalo.
2. Right subtree ki height nikalo.
3. Current node ke through diameter = `left + right`.
4. Global maximum diameter update karo.
5. Parent ke liye current subtree ki height return karo.

Flow:

`Left Height`

↓

`Right Height`

↓

`left + right`

↓

`Global Diameter Update`

↓

`max(left, right) + 1`

↓

`Parent ko Height Return`

---

# Why Global Diameter?

Diameter zaroori nahi ki root ke through ho.

Example:

        1
       /
      2
     / \
    4   5

Longest path:

`4 → 2 → 5`

Ye node `2` ke through hai.

Root `1` ke through nahi.

Isliye har node par diameter calculate karna padega.

Aur maximum value ko store karne ke liye:

    int diameter = 0;

use karte hain.

---

# C++ Code

    class Solution {
    public:
        int diameter = 0;

        int height(TreeNode* root) {
            if (root == NULL)
                return 0;

            int left = height(root->left);

            int right = height(root->right);

            diameter = max(diameter, left + right);

            return max(left, right) + 1;
        }

        int diameterOfBinaryTree(TreeNode* root) {
            height(root);

            return diameter;
        }
    };

---

# Code Explanation

## 1. Global Diameter

    int diameter = 0;

Ye poori tree ka maximum diameter store karega.

Har node par ek new diameter candidate mil sakta hai.

---

# 2. `height()` Function

    int height(TreeNode* root)

Ye function:

- subtree ki height calculate karta hai
- aur har node par diameter update karta hai

---

# 3. Base Case

    if (root == NULL)
        return 0;

Agar current node NULL hai:

`height = 0`

---

# 4. Left Height

    int left = height(root->left);

Left subtree ki complete height recursively calculate hoti hai.

Ye sirf left child ko check nahi karta.

Poora left subtree process hota hai.

---

# 5. Right Height

    int right = height(root->right);

Same logic.

Right subtree ki complete height calculate hoti hai.

---

# 6. Diameter Update

    diameter = max(diameter, left + right);

Ye most important line hai.

Current node ke through possible diameter:

`left + right`

Then compare with previous global diameter.

Example:

Previous diameter:

`2`

Current node ka diameter:

`3`

Then:

`diameter = max(2, 3)`

`= 3`

---

# 7. Return Height

    return max(left, right) + 1;

Ye diameter ke liye nahi hai.

Ye current subtree ki **height parent ko return** karta hai.

Example:

`left = 2`

`right = 1`

Then:

`max(2,1) + 1`

`= 3`

Parent ko pata chalega ki current subtree ki height `3` hai.

---

# 8. Final Function

    int diameterOfBinaryTree(TreeNode* root) {
        height(root);
        return diameter;
    }

`height(root)` call karke poori tree traverse hoti hai.

Har node par diameter update hota hai.

Finally global `diameter` return kar dete hain.

---

# Detailed Dry Run

Tree:

        1
       / \
      2   3
     / \
    4   5

Initial:

`diameter = 0`

---

## Step 1 — Node 4

Tree:

        1
       / \
      2   3
     / \
    4   5

Current node:

`4`

4 ke left:

`NULL`

So:

`left = 0`

4 ke right:

`NULL`

So:

`right = 0`

Diameter candidate:

`left + right`

`= 0 + 0`

`= 0`

Global:

`diameter = max(0,0) = 0`

Height:

`max(0,0) + 1`

`= 1`

Return:

`height(4) = 1`

---

# Tree Again

        1
       / \
      2   3
     / \
    4   5

Ab `4` ka answer `1` hai.

Ye node `2` ko return hota hai.

---

# Step 2 — Node 5

Node `2` ke right subtree mein `5` hai.

Current:

`5`

Left:

`0`

Right:

`0`

Diameter candidate:

`0 + 0 = 0`

Global diameter remains:

`0`

Height:

`max(0,0) + 1`

`= 1`

Return:

`height(5) = 1`

---

# Tree Again

        1
       / \
      2   3
     / \
    4   5

Ab node `2` ko:

`left = 1`

`right = 1`

mil gaya.

---

# Step 3 — Node 2

Current:

`2`

Left height:

`1`

Right height:

`1`

Current node ke through diameter:

`1 + 1`

`= 2`

Global:

`diameter = max(0,2)`

`= 2`

Path:

`4 → 2 → 5`

Edges:

`2`

So diameter = `2`.

---

# Node 2 Ki Height

Ab parent `1` ko height chahiye.

Formula:

`max(left, right) + 1`

`max(1,1) + 1`

`= 2`

So:

`height(2) = 2`

---

# Tree Again

        1
       / \
      2   3
     / \
    4   5

Ab root `1` ko:

`left = 2`

mil gaya.

---

# Step 4 — Node 3

Current:

`3`

Left:

`0`

Right:

`0`

Diameter candidate:

`0`

Global:

`max(2,0) = 2`

Height:

`max(0,0) + 1`

`= 1`

So:

`height(3) = 1`

---

# Final Tree

        1
       / \
      2   3
     / \
    4   5

Root `1` ko:

`left = 2`

`right = 1`

---

# Step 5 — Node 1

Current node ke through diameter:

`left + right`

`= 2 + 1`

`= 3`

Global previous:

`2`

So:

`diameter = max(2,3)`

`= 3`

---

# Root Ki Height

Parent ki requirement ke liye:

`max(2,1) + 1`

`= 3`

So:

`height(1) = 3`

---

# Final Answer

`diameter = 3`

Longest path:

`4 → 2 → 1 → 3`

Edges:

`3`

Therefore:

`Answer = 3`

---

# Recursion Flow

Tree:

        1
       / \
      2   3
     / \
    4   5

Pehle neeche:

`height(1)`

↓

`height(2)`

↓

`height(4)`

↓

Return

Then:

`height(5)`

↓

Return to `2`

Then:

`height(3)`

↓

Return to `1`

Then final diameter update at `1`.

---

# Important Difference Between Height and Diameter

Ye dono confuse mat karna.

## Height

Parent ko return hoti hai:

`max(left,right) + 1`

## Diameter

Global answer update hota hai:

`left + right`

Example:

        2
       / \
      4   5

Left height:

`1`

Right height:

`1`

Diameter:

`1 + 1 = 2`

Height:

`max(1,1) + 1 = 2`

Dono same aa sakte hain, but meaning completely different hai.

---

# Why One DFS Is Enough?

Ek recursive traversal mein hum dono kaam simultaneously kar sakte hain:

`height(left)`

↓

`height(right)`

↓

`diameter update`

↓

`height return`

Isliye separate height traversal aur diameter traversal ki need nahi.

---

# Complexity

Let `n` = number of nodes.

## Time Complexity

Har node maximum once visit hota hai.

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

"I use DFS to calculate the height of each subtree. At every node, the longest path passing through that node is the sum of the left and right subtree heights. I update a global diameter with this value. Then I return the height of the current subtree as `max(left, right) + 1` for the parent."

---

# Memory Trick

LC 104:

`Height = max(left, right) + 1`

LC 543:

`Diameter = left + right`

`Height = max(left, right) + 1`

So:

**543 = Height + Diameter Update**

---

# Pattern 4 Progress

Pattern 4 — Validation

`104 — Maximum Depth ✅`

`110 — Balanced Binary Tree ✅`

`543 — Diameter of Binary Tree ✅`

Remaining:

`98 — Validate Binary Search Tree ⏳`

---

# LeetCode

Problem: `543`

Name: `Diameter of Binary Tree`

Pattern: `Validation`

Technique: `DFS + Height + Global Maximum`

Core Formula:

`diameter = leftHeight + rightHeight`

Height Formula:

`max(leftHeight, rightHeight) + 1`

Difficulty: Easy
