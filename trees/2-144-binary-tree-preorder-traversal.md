# LC 144 — Binary Tree Preorder Traversal

## Problem

Given the root of a binary tree, return the preorder traversal of its nodes' values.

Preorder ka rule:

    ROOT → LEFT → RIGHT

Example:

        1
       / \
      2   3
     / \
    4   5

Preorder:

    1 → 2 → 4 → 5 → 3

Output:

    [1, 2, 4, 5, 3]

---

# 1. Simple Understanding

Preorder traversal mein kisi bhi node par pahunchte hi:

1. Sabse pehle current node ko visit karo.
2. Uske left subtree mein jao.
3. Left complete hone ke baad right subtree mein jao.

Isliye:

    ROOT → LEFT → RIGHT

Important:

"Root" ka matlab sirf poore tree ka root nahi hai.

Har recursive call mein jo current node hoti hai,
woh us subtree ki root hoti hai.

Example:

        1
       / \
      2   3
     / \
    4   5

Jab hum `2` par hote hain:

        2
       / \
      4   5

Yahan `2` current subtree ka root hai.

---

# 2. Pattern

Preorder:

    ROOT → LEFT → RIGHT

Tree traversal ka ye ek DFS pattern hai.

Hum recursion use karenge.

    DFS + Recursion

---

# 3. Approach

Har node par:

    1. Current node ki value answer mein daalo.
    2. Left subtree recursively traverse karo.
    3. Right subtree recursively traverse karo.

Pseudo-code:

    preorder(root):

        if root == NULL:
            return

        add root->val to answer

        preorder(root->left)

        preorder(root->right)

---

# 4. Why Recursion?

Tree naturally recursive structure follow karta hai.

Example:

        1
       / \
      2   3
     / \
    4   5

Root `1` ke andar:

    Left subtree = tree rooted at 2
    Right subtree = tree rooted at 3

Isi logic ko har subtree par repeat kar sakte hain.

Isliye:

    solve(root)
        ↓
    solve(left)
        ↓
    solve(right)

---

# 5. Code

```cpp
class Solution {
public:

    void preorder(TreeNode* root, vector<int>& ans) {

        if(root == NULL)
            return;

        ans.push_back(root->val);

        preorder(root->left, ans);

        preorder(root->right, ans);
    }

    vector<int> preorderTraversal(TreeNode* root) {

        vector<int> ans;

        preorder(root, ans);

        return ans;
    }
};
