# LC 226 — Invert Binary Tree

## Problem

Given the root of a binary tree, invert the tree and return its root.

"Invert" ka matlab hai:

    LEFT ↔ RIGHT

Har node ke left aur right child ko swap karna hai.

Example:

        1
       / \
      2   3
     / \
    4   5

After Inversion:

        1
       / \
      3   2
         / \
        5   4

---

# 1. Simple Understanding

Original Tree:

        1
       / \
      2   3
     / \
    4   5

Root `1` ke:

    left = 2
    right = 3

Invert karne ke baad:

    left = 3
    right = 2

Lekin kaam sirf root `1` par nahi rukega.

Node `2` bhi invert hoga:

        2
       / \
      4   5

becomes:

        2
       / \
      5   4

Isliye final:

        1
       / \
      3   2
         / \
        5   4

Main idea:

    Har node par LEFT aur RIGHT swap karo.

---

# 2. Pattern

Pattern:

    Mirror / Symmetry

Core operation:

    LEFT ↔ RIGHT

Tree recursive structure follow karta hai,
isliye recursion naturally use kar sakte hain.

---

# 3. Approach

Har current node par:

    1. Agar node NULL hai → return NULL
    2. Left aur Right child swap karo
    3. Left subtree ko recursively invert karo
    4. Right subtree ko recursively invert karo
    5. Current root return karo

Pseudo-code:

    invert(root):

        if root == NULL:
            return NULL

        swap(root->left, root->right)

        invert(root->left)

        invert(root->right)

        return root

---

# 4. Code

```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {

        if (root == NULL)
            return NULL;

        swap(root->left, root->right);

        invertTree(root->left);

        invertTree(root->right);

        return root;
    }
};
