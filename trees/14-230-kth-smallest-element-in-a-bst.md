# LeetCode 230 — Kth Smallest Element in a BST

## Problem

Given the root of a Binary Search Tree (BST) and an integer `k`, return the `k`-th smallest value in the BST.

---

## Example

Tree:

        5
       / \
      3   7
     / \   \
    2   4   8

`k = 3`

BST ka inorder traversal:

`2 → 3 → 4 → 5 → 7 → 8`

3rd smallest element:

`4`

Answer:

`4`

---

# Main Concept

Is problem ka sabse important concept:

**BST ka Inorder Traversal sorted order deta hai.**

Inorder:

`Left → Root → Right`

Example:

        5
       / \
      3   7
     / \   \
    2   4   8

Inorder:

`2 → 3 → 4 → 5 → 7 → 8`

Ye ascending order hai.

Isliye:

- 1st visited node = 1st smallest
- 2nd visited node = 2nd smallest
- 3rd visited node = 3rd smallest
- ...
- k-th visited node = k-th smallest

---

# Why Inorder Works in BST?

BST property:

`Left subtree < Root < Right subtree`

Isliye jab hum:

`Left → Root → Right`

follow karte hain, values automatically sorted order mein aati hain.

Normal Binary Tree mein inorder sorted zaroori nahi hai.

**BST mein specifically inorder sorted hota hai.**

---

# DFS Connection

Inorder traversal actually **DFS ka ek form** hai.

DFS ka basic idea:

**Depth mein jao, phir wapas aao.**

Inorder DFS:

`Left → Root → Right`

Humne pehle bhi DFS use kiya tha:

- Preorder
- Inorder
- Postorder
- LCA
- Subtree
- Two Sum IV

Is problem mein specifically:

**Inorder DFS**

use ho raha hai.

---

# Approach

Har node par following steps honge:

1. Pehle left subtree mein jao.
2. Left subtree complete hone ke baad current node ko visit karo.
3. `k--` karo.
4. Agar `k == 0`, current node hi answer hai.
5. Otherwise right subtree mein jao.

Flow:

`Left → Current → Right`

Aur current node par:

`k--`

Jab:

`k == 0`

then:

`answer = current node`

---

# C++ Code

    class Solution {
    public:
        void solve(TreeNode* root, int& k, int& ans) {
            if (root == NULL)
                return;

            // Left subtree
            solve(root->left, k, ans);

            // Current node
            k--;

            if (k == 0) {
                ans = root->val;
                return;
            }

            // Right subtree
            solve(root->right, k, ans);
        }

        int kthSmallest(TreeNode* root, int k) {
            int ans = -1;

            solve(root, k, ans);

            return ans;
        }
    };

---

# Code Explanation

## 1. solve() Function

    void solve(TreeNode* root, int& k, int& ans)

`root` = current node

`k` = kitne-th smallest element chahiye

`ans` = final answer store karega

---

## 2. Why `int& k`?

Humne `k` ko reference ke through pass kiya:

    int& k

Iska matlab recursion ki har call mein **same k variable** use hoga.

Agar:

`k = 3`

aur ek node process hui:

`k = 2`

Toh next recursive call mein bhi `k = 2` hi milega.

Phir:

`k = 1`

Phir:

`k = 0`

Ye important hai because hume poore inorder traversal ke across same count maintain karna hai.

---

## 3. Base Case

    if (root == NULL)
        return;

Agar current node `NULL` hai, wahan koi node nahi hai.

So simply return.

---

# 4. Left Subtree

    solve(root->left, k, ans);

Inorder ka first step:

`Left`

So current node ko process karne se pehle complete left subtree process karenge.

Important:

Ye line sirf ek node ko nahi, balki **left subtree ka poora recursive traversal** perform karti hai.

---

# 5. Current Node

Left subtree complete hone ke baad:

    k--;

Ab current node inorder sequence mein aa gaya.

Isliye ek count consume karo.

Example:

Starting:

`k = 3`

First visited node:

`k = 2`

Second visited node:

`k = 1`

Third visited node:

`k = 0`

---

# 6. Check K == 0

    if (k == 0) {
        ans = root->val;
        return;
    }

Agar current node ko visit karne ke baad:

`k == 0`

toh current node hi k-th smallest element hai.

Example:

Current node:

`4`

Before visiting:

`k = 1`

After:

`k--`

`k = 0`

So:

`ans = 4`

---

# 7. Why Return After Finding Answer?

    return;

Answer mil gaya hai.

Ab hume unnecessary traversal nahi karna.

So current recursive call se return kar do.

---

# 8. Right Subtree

    solve(root->right, k, ans);

Current node process ho gaya.

Inorder ka next step:

`Right`

So right subtree process karo.

Important:

Left aur right ke liye alag recursion code likhne ki zarurat nahi hai.

`solve()` function khud same function ko repeatedly call karta hai.

For example:

    solve(root->left, k, ans);

means:

**left child ke liye poora solve() function dobara chalega.**

Similarly:

    solve(root->right, k, ans);

means:

**right child ke liye bhi poora solve() function dobara chalega.**

---

# Detailed Dry Run

Tree:

        5
       / \
      3   7
     / \   \
    2   4   8

`k = 3`

Initial:

`k = 3`

`ans = -1`

---

## Step 1 — Start at 5

Current node:

`5`

Inorder ke according pehle left:

    solve(5->left, k, ans);

So go to:

`3`

5 ko abhi process nahi kiya.

---

## Step 2 — At 3

Current:

`3`

Pehle left:

    solve(3->left, k, ans);

Go to:

`2`

---

## Step 3 — At 2

Current:

`2`

Pehle left:

`NULL`

So return.

Ab current node `2` process hoga.

Before:

`k = 3`

After:

`k = 2`

So `2` first smallest element hai.

Right:

`NULL`

So return.

---

## Tree After Step 3

        5
       / \
      3   7
     / \   \
    2   4   8
    ↑
  visited

Visited order:

`2`

Current `k`:

`2`

---

## Step 4 — Back to 3

2 ka recursive call complete ho gaya.

Ab hum wapas `3` par aaye.

3 ka left complete ho gaya.

So ab current node `3` process hoga.

Before:

`k = 2`

Do:

`k--`

Now:

`k = 1`

So `3` second smallest hai.

Ab right:

    solve(3->right, k, ans);

Go to:

`4`

---

## Tree After Step 4

        5
       / \
      3   7
     / \   \
    2   4   8
        ↑
      current

Visited:

`2 → 3`

Current:

`k = 1`

---

# Step 5 — At 4

Current:

`4`

Pehle left:

`NULL`

Return.

Ab current `4` process hoga.

Before:

`k = 1`

Do:

`k--`

Now:

`k = 0`

Check:

    if (k == 0)

True.

So:

`ans = 4`

And:

`return`

---

## Final Tree

        5
       / \
      3   7
     / \   \
    2   4   8
        ↑
      ANSWER

Inorder order:

`2 → 3 → 4`

`k = 3`

Therefore:

`3rd smallest = 4`

Final answer:

`4`

---

# Recursion Flow

        5
       / \
      3   7
     / \   \
    2   4   8

Flow:

`solve(5)`

↓

`solve(3)`

↓

`solve(2)`

↓

`2` process

↓

Back to `3`

↓

`3` process

↓

`solve(4)`

↓

`4` process

↓

`k == 0`

↓

`ans = 4`

---

# Very Important: Why Did We Not Reach 5 and 7?

Because `k = 3`.

Inorder sequence:

`2 → 3 → 4 → 5 → 7 → 8`

3rd node par hi answer mil gaya:

`4`

So uske baad hume remaining nodes traverse karne ki zarurat nahi.

---

# Simple Visualization

BST:

        5
       / \
      3   7
     / \   \
    2   4   8

Inorder:

`2 → 3 → 4 → 5 → 7 → 8`

Count:

`1 → 2 → 3 → 4 → 5 → 6`

For `k = 3`:

`1st = 2`

`2nd = 3`

`3rd = 4`

So:

`answer = 4`

---

# Most Important Pattern

Question says:

**Kth Smallest in BST**

Immediately think:

`BST`

↓

`Inorder`

↓

`Sorted Order`

↓

`k-th visited node`

---

# Memory Trick

**BST + Kth Smallest = Inorder**

Inorder:

`Left → Root → Right`

Har node par:

`k--`

`k == 0` → answer

---

# Complexity

Let `n` = number of nodes.

## Time Complexity

Worst case mein nodes traverse ho sakte hain:

`O(n)`

## Space Complexity

Recursion stack worst case:

`O(n)`

Balanced tree mein recursion stack approximately:

`O(log n)`

---

# Interview Explanation

"I use inorder DFS because inorder traversal of a BST visits nodes in sorted ascending order. I recursively process the left subtree first, then decrement k when visiting the current node. When k becomes zero, the current node is the k-th smallest element. Then I stop further processing."

---

# Pattern

Pattern 3 — Search / BST

Problem:

**LeetCode 230 — Kth Smallest Element in a BST**

Technique:

**Inorder DFS**

Core Property:

**BST Inorder = Sorted Order**

Time:

`O(n)`

Space:

`O(n)` worst case

---

# Quick Revision

Question:

**Kth smallest in BST?**

Think:

`Inorder`

Why?

`BST Inorder = Sorted`

What do we do?

`k--`

When?

`k == 0`

Then:

`answer = root->val`
