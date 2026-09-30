# LC 145 — Binary Tree Postorder Traversal

## Problem

Given the root of a binary tree, return the postorder traversal of its nodes' values.

Postorder Traversal ka rule:

    LEFT → RIGHT → ROOT

Example:

        1
       / \
      2   3
     / \
    4   5

Postorder traversal:

    4 → 5 → 2 → 3 → 1

Output:

    [4, 5, 2, 3, 1]

---

# 1. Simple Understanding

Postorder traversal mein kisi bhi current node par:

1. Pehle LEFT subtree ko traverse karo.
2. Phir RIGHT subtree ko traverse karo.
3. Sabse last mein current ROOT ko answer mein daalo.

Isliye:

    LEFT → RIGHT → ROOT

Important:

Ye rule har node/subtree par apply hota hai.

Example:

        2
       / \
      4   5

Is subtree ka postorder:

    4 → 5 → 2

Pehle dono children,
phir parent.

---

# 2. Pattern

Postorder Traversal is a:

    DFS + Recursion

Tree ko recursively solve karenge.

---

# 3. Approach

Har node par:

    1. Left subtree traverse karo.
    2. Right subtree traverse karo.
    3. Current node ki value answer mein add karo.

Pseudo-code:

    postorder(root):

        if root == NULL:
            return

        postorder(root->left)

        postorder(root->right)

        add root->val to answer

---

# 4. Code

```cpp
class Solution {
public:

    void postorder(TreeNode* root, vector<int>& ans) {

        if (root == NULL)
            return;

        postorder(root->left, ans);

        postorder(root->right, ans);

        ans.push_back(root->val);
    }

    vector<int> postorderTraversal(TreeNode* root) {

        vector<int> ans;

        postorder(root, ans);

        return ans;
    }
};
