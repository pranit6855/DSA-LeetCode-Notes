# LC 94 — Binary Tree Inorder Traversal

## Problem

Given the root of a binary tree, return the inorder traversal of its nodes' values.

Inorder Traversal ka rule:

    LEFT → ROOT → RIGHT

Example:

        1
       / \
      2   3
     / \
    4   5

Inorder traversal:

    4 → 2 → 5 → 1 → 3

Output:

    [4, 2, 5, 1, 3]

---

# 1. Simple Understanding

Inorder traversal mein kisi bhi current node par:

1. Pehle uske LEFT subtree ko traverse karo.
2. Phir current ROOT ko answer mein daalo.
3. Phir uske RIGHT subtree ko traverse karo.

Isliye:

    LEFT → ROOT → RIGHT

Important:

Ye rule har node/subtree par apply hota hai.

Example:

        2
       / \
      4   5

Is subtree ka inorder:

    4 → 2 → 5

Agar current node 2 hai:

    LEFT = 4
    ROOT = 2
    RIGHT = 5

So:

    4 → 2 → 5

---

# 2. Pattern

Inorder Traversal is a:

    DFS + Recursion

Tree recursively solve kiya ja sakta hai because
har node ke left aur right mein bhi chhota tree hota hai.

---

# 3. Approach

Har node par:

    1. Left subtree traverse karo.
    2. Current node ki value answer mein add karo.
    3. Right subtree traverse karo.

Pseudo-code:

    inorder(root):

        if root == NULL:
            return

        inorder(root->left)

        add root->val to answer

        inorder(root->right)

---

# 4. Code

```cpp
class Solution {
public:

    void inorder(TreeNode* root, vector<int>& ans) {

        if (root == NULL)
            return;

        inorder(root->left, ans);

        ans.push_back(root->val);

        inorder(root->right, ans);
    }

    vector<int> inorderTraversal(TreeNode* root) {

        vector<int> ans;

        inorder(root, ans);

        return ans;
    }
};
