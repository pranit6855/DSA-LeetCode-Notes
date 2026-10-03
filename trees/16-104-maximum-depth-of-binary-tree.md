# LeetCode 104 — Maximum Depth of Binary Tree

## Problem

Given the root of a binary tree, return its **maximum depth**.

Maximum depth ka matlab:

**Root se kisi bhi leaf node tak longest path mein kitne nodes hain.**

---

# Example

Tree:

        1
       / \
      2   3
     /
    4

Longest path:

`1 → 2 → 4`

Total nodes:

`3`

So:

`Maximum Depth = 3`

---

# Main Concept

Har node ke liye hume find karna hai:

- Left subtree ki depth
- Right subtree ki depth

Phir dono mein se maximum depth lenge.

Current node ko count karne ke liye `+1` karenge.

Formula:

`depth(root) = max(depth(left), depth(right)) + 1`

---

# Why `+1`?

Suppose leaf node hai:

        4

Iske left aur right dono `NULL` hain.

NULL ki depth:

`0`

So:

`depth(4) = max(0, 0) + 1`

`= 1`

Ye `+1` current node `4` ke liye hai.

---

# NULL ki Depth

Agar:

    root == NULL

Toh koi node nahi hai.

Therefore:

`depth = 0`

Code:

    if (root == NULL)
        return 0;

---

# Approach

Har node par:

1. Agar root NULL hai → `0` return karo.
2. Left subtree ki maximum depth nikalo.
3. Right subtree ki maximum depth nikalo.
4. Dono mein maximum lo.
5. Current node ko include karne ke liye `+1` karo.

Flow:

`Left Depth`
+
`Right Depth`
↓
`max(left, right)`
↓
`+1`
↓
`Current Node ki Depth`

---

# C++ Code

    class Solution {
    public:
        int maxDepth(TreeNode* root) {
            if (root == NULL)
                return 0;

            int left = maxDepth(root->left);

            int right = maxDepth(root->right);

            return max(left, right) + 1;
        }
    };

---

# Code Explanation

## 1. Function

    int maxDepth(TreeNode* root)

Function current node se maximum depth calculate karta hai.

`root` = current node.

Return type `int` hai because depth ek number hai.

---

# 2. Base Case

    if (root == NULL)
        return 0;

Agar tree/subtree mein node hi nahi hai, depth `0` hogi.

---

# 3. Left Subtree

    int left = maxDepth(root->left);

Is line ka matlab:

**Current node ke left subtree ki maximum depth calculate karo.**

Important:

Ye sirf left child ko check nahi karta.

`maxDepth()` recursively left ke poore subtree ko traverse karta hai.

Example:

        1
       /
      2
     /
    3

Agar current `1` hai:

    maxDepth(1->left)

means:

    maxDepth(2)

phir:

    maxDepth(3)

phir NULL.

So poore left subtree ki depth calculate hoti hai.

---

# 4. Right Subtree

    int right = maxDepth(root->right);

Same logic.

Current node ke right subtree ki maximum depth calculate karta hai.

---

# 5. Final Formula

    return max(left, right) + 1;

Ye sabse important line hai.

Meaning:

**Left aur right subtree mein jo deeper hai usko lo, phir current node ke liye 1 add karo.**

Example:

`left = 3`

`right = 2`

Then:

`max(3, 2) + 1`

`= 4`

So current node ki maximum depth = `4`.

---

# Detailed Dry Run

Tree:

        1
       / \
      2   3
     /
    4

Initial call:

    maxDepth(1)

---

## Step 1 — Node 1

Current node:

`1`

Code first left call karega:

    maxDepth(1->left)

So:

`maxDepth(2)`

Tree:

        1
       / \
      2   3
     /
    4

Current:

`2`

---

## Step 2 — Node 2

Current node:

`2`

Again first left:

    maxDepth(2->left)

So:

`maxDepth(4)`

---

## Step 3 — Node 4

Current:

`4`

Tree:

        1
       / \
      2   3
     /
    4

Node `4` ka left:

`NULL`

So:

    maxDepth(NULL)

returns:

`0`

Node `4` ka right bhi:

`NULL`

So:

    maxDepth(NULL)

returns:

`0`

Now node `4`:

`left = 0`

`right = 0`

Therefore:

`max(0, 0) + 1`

`= 1`

So:

**Depth of node 4 = 1**

---

## Tree After Node 4

        1
       / \
      2   3
     /
    4

Node 4 ki depth:

`1`

Ab answer `1` return hoga to node `2`.

---

# Step 4 — Back to Node 2

Ab node `2` ko left subtree se answer mil gaya:

`left = 1`

Node `2` ka right:

`NULL`

So:

`right = 0`

Now:

`max(1, 0) + 1`

`= 2`

Therefore:

**Depth of node 2 = 2**

---

## Tree Again

        1
       / \
      2   3
     /
    4

Node 2 tak longest path:

`2 → 4`

Depth:

`2`

Ab ye answer node `1` ko return hoga.

---

# Step 5 — Back to Node 1

Node `1` ko left subtree se:

`left = 2`

Ab right subtree calculate karenge:

    maxDepth(1->right)

So:

`maxDepth(3)`

---

# Step 6 — Node 3

Node `3`:

        1
       / \
      2   3
     /
    4

Node `3` ka left:

`NULL`

So:

`left = 0`

Node `3` ka right:

`NULL`

So:

`right = 0`

Now:

`max(0, 0) + 1`

`= 1`

Therefore:

**Depth of node 3 = 1**

---

## Tree Again

        1
       / \
      2   3
     /
    4

Now root `1` ke paas:

`left = 2`

`right = 1`

---

# Step 7 — Node 1 Final Calculation

Code:

    return max(left, right) + 1;

Values:

`left = 2`

`right = 1`

So:

`max(2, 1) + 1`

`= 3`

Therefore:

`Maximum Depth = 3`

---

# Final Answer

`3`

Longest path:

`1 → 2 → 4`

---

# Recursion Flow

Tree:

        1
       / \
      2   3
     /
    4

Flow downward:

`maxDepth(1)`
↓
`maxDepth(2)`
↓
`maxDepth(4)`
↓
`maxDepth(NULL)`

Then answers come back upward:

`NULL → 0`
↓
`4 → 1`
↓
`2 → 2`
↓
`3 → 1`
↓
`1 → 3`

This is a **bottom-up recursion** pattern.

---

# Important Pattern

Is problem mein hum:

**Pehle children ka answer nikalte hain**

then:

**current node ka answer calculate karte hain.**

Flow:

`Left Answer`
↓
`Right Answer`
↓
`max(left, right)`
↓
`+1`

---

# Why `max()`?

Question hai:

**Maximum depth**

Isliye left aur right mein jo path longer hai, wahi choose karna hai.

Example:

`left = 5`

`right = 2`

Then:

`max(5, 2) = 5`

Current node add:

`5 + 1 = 6`

---

# Complexity

Let `n` = number of nodes.

## Time Complexity

Har node ko maximum ek baar visit karte hain.

`O(n)`

## Space Complexity

Recursive call stack tree ki height `h` tak ja sakta hai.

`O(h)`

Balanced tree mein:

`O(log n)`

Skewed tree mein:

`O(n)`

---

# Interview Explanation

"I use recursion to calculate the maximum depth of the left and right subtrees. If the current node is null, its depth is zero. Otherwise, I take the maximum of the left and right subtree depths and add one for the current node."

---

# Memory Trick

**Maximum Depth:**

`NULL → 0`

Otherwise:

`max(left, right) + 1`

---

# Pattern 4 — Validation

Current progress:

`104 — Maximum Depth of Binary Tree ✅`

Remaining:

`110 — Balanced Binary Tree`
`543 — Diameter of Binary Tree`
`98 — Validate Binary Search Tree`

---

# LeetCode

Problem: `104`

Name: `Maximum Depth of Binary Tree`

Pattern: `Validation`

Technique: `DFS / Recursion`

Core Formula:

`max(leftDepth, rightDepth) + 1`

Difficulty: Easy
