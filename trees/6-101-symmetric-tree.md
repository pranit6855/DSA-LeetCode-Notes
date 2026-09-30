# LC 101 — Symmetric Tree

## Problem

Given the root of a binary tree, check whether the tree is symmetric around its center.

Symmetric ka matlab:

    Left side aur Right side ek-doosre ki mirror image honi chahiye.

Example:

        1
       / \
      2   2
     / \ / \
    3  4 4  3

Ye symmetric hai.

Output:

    true

---

# 1. Simple Understanding

Symmetric Tree mein humein poore tree ko compare nahi karna,
balki root ke LEFT subtree aur RIGHT subtree ko mirror manner mein compare karna hai.

Example:

        1
       / \
      2   2
     / \ / \
    3  4 4  3

Left subtree:

      2
     / \
    3   4

Right subtree:

      2
     / \
    4   3

Ye dono mirror hain.

Important:

Normal comparison:

    left.left  ↔ right.left
    left.right ↔ right.right

Nahi karna.

Symmetric Tree mein:

    left.left  ↔ right.right
    left.right ↔ right.left

compare karna hai.

---

# 2. Pattern

Pattern:

    Mirror / Symmetry

Core idea:

    LEFT ↔ RIGHT
    in mirror order

Hum recursion use karenge.

---

# 3. Approach

Hum ek helper function banayenge:

    ismirror(left, right)

Ye do nodes/subtrees ko compare karega.

Har pair ke liye:

### Case 1 — Dono NULL

    left = NULL
    right = NULL

Matlab dono sides par kuch nahi hai.

So:

    true

---

### Case 2 — Sirf ek NULL

Example:

    left = NULL
    right = 3

Structure mirror nahi ho sakta.

So:

    false

---

### Case 3 — Values different

Example:

    left->val = 2
    right->val = 3

So:

    false

---

### Case 4 — Values same

Agar values same hain,
toh unke children ko mirror order mein compare karo:

    left->left  ↔ right->right

and

    left->right ↔ right->left

Dono comparisons true hone chahiye.

---

# 4. Pseudo Code

    ismirror(left, right):

        if both NULL:
            return true

        if one is NULL:
            return false

        if left->val != right->val:
            return false

        return
            ismirror(left->left, right->right)
            &&
            ismirror(left->right, right->left)

---

# 5. Code

```cpp
class Solution {
public:

    bool ismirror(TreeNode* left, TreeNode* right) {

        if (left == NULL && right == NULL)
            return true;

        if (left == NULL || right == NULL)
            return false;

        if (left->val != right->val)
            return false;

        return ismirror(left->left, right->right) &&
               ismirror(left->right, right->left);
    }

    bool isSymmetric(TreeNode* root) {

        if (root == NULL)
            return true;

        return ismirror(root->left, root->right);
    }
};# LC 101 — Symmetric Tree

## Problem

Given the root of a binary tree, check whether the tree is symmetric around its center.

Symmetric ka matlab:

    Left side aur Right side ek-doosre ki mirror image honi chahiye.

Example:

        1
       / \
      2   2
     / \ / \
    3  4 4  3

Ye symmetric hai.

Output:

    true

---

# 1. Simple Understanding

Symmetric Tree mein humein poore tree ko compare nahi karna,
balki root ke LEFT subtree aur RIGHT subtree ko mirror manner mein compare karna hai.

Example:

        1
       / \
      2   2
     / \ / \
    3  4 4  3

Left subtree:

      2
     / \
    3   4

Right subtree:

      2
     / \
    4   3

Ye dono mirror hain.

Important:

Normal comparison:

    left.left  ↔ right.left
    left.right ↔ right.right

Nahi karna.

Symmetric Tree mein:

    left.left  ↔ right.right
    left.right ↔ right.left

compare karna hai.

---

# 2. Pattern

Pattern:

    Mirror / Symmetry

Core idea:

    LEFT ↔ RIGHT
    in mirror order

Hum recursion use karenge.

---

# 3. Approach

Hum ek helper function banayenge:

    ismirror(left, right)

Ye do nodes/subtrees ko compare karega.

Har pair ke liye:

### Case 1 — Dono NULL

    left = NULL
    right = NULL

Matlab dono sides par kuch nahi hai.

So:

    true

---

### Case 2 — Sirf ek NULL

Example:

    left = NULL
    right = 3

Structure mirror nahi ho sakta.

So:

    false

---

### Case 3 — Values different

Example:

    left->val = 2
    right->val = 3

So:

    false

---

### Case 4 — Values same

Agar values same hain,
toh unke children ko mirror order mein compare karo:

    left->left  ↔ right->right

and

    left->right ↔ right->left

Dono comparisons true hone chahiye.

---

# 4. Pseudo Code

    ismirror(left, right):

        if both NULL:
            return true

        if one is NULL:
            return false

        if left->val != right->val:
            return false

        return
            ismirror(left->left, right->right)
            &&
            ismirror(left->right, right->left)

---

# 5. Code

```cpp
class Solution {
public:

    bool ismirror(TreeNode* left, TreeNode* right) {

        if (left == NULL && right == NULL)
            return true;

        if (left == NULL || right == NULL)
            return false;

        if (left->val != right->val)
            return false;

        return ismirror(left->left, right->right) &&
               ismirror(left->right, right->left);
    }

    bool isSymmetric(TreeNode* root) {

        if (root == NULL)
            return true;

        return ismirror(root->left, root->right);
    }
};
