# LC 102 — Binary Tree Level Order Traversal

## Problem

Given the root of a binary tree, return the level order traversal of its nodes' values.

Level order ka matlab hai tree ko **level by level** traverse karna.

Example:

        1
       / \
      2   3
     / \
    4   5

Output:

    [[1], [2,3], [4,5]]

---

# 1. Simple Understanding

Tree:

        1
       / \
      2   3
     / \
    4   5

Levels:

Level 1:
    1

Level 2:
    2 3

Level 3:
    4 5

Isliye answer:

    [[1], [2,3], [4,5]]

Important:

Ye normal traversal jaisa sirf:

    [1,2,3,4,5]

nahi chahiye.

Humein **har level ko alag vector** mein store karna hai.

---

# 2. Pattern

Level Order Traversal ka pattern:

    BFS + Queue

BFS = Breadth First Search

Queue ka rule:

    FIFO
    First In → First Out

Matlab jo node queue mein pehle aayi,
woh pehle process hogi.

---

# 3. Queue Ki Zarurat Kyun?

Tree:

        1
       / \
      2   3
     / \
    4   5

Start mein:

    Queue = [1]

1 ko process kiya.

Uske children:

    2, 3

Queue:

    [2,3]

Ab 2 pehle aaya hai, toh 2 pehle niklega.

2 ke children:

    4,5

Queue:

    [3,4,5]

Phir 3 niklega.

Queue:

    [4,5]

Isi tarah nodes level by level process hote hain.

---

# 4. Main Challenge

Agar sirf queue use karein, toh output mil jayega:

    [1,2,3,4,5]

Lekin problem mein chahiye:

    [[1], [2,3], [4,5]]

Toh humein identify karna padega:

    Current level mein kitne nodes hain?

Isi ke liye:

    int size = q.size();

use karte hain.

---

# 5. Sabse Important Concept

    int size = q.size();

Ye batata hai:

> Is waqt queue mein current level ke kitne nodes hain?

Example:

Queue:

    [1]

Toh:

    size = 1

Matlab current level mein 1 node hai.

1 process karne ke baad uske children queue mein aayenge:

    [2,3]

Next iteration mein:

    size = 2

Matlab current level mein 2 nodes hain.

Hum exactly 2 nodes process karenge:

    2
    3

Phir unke children next level ke liye queue mein chale jayenge.

---

# 6. Approach

Steps:

1. Agar root NULL hai toh empty answer return karo.
2. Root ko queue mein push karo.
3. Jab tak queue empty nahi hai:
   - Current queue ka size store karo.
   - Ek empty `level` vector banao.
   - Current level ke `size` nodes process karo.
   - Har node ki value `level` mein daalo.
   - Uske left aur right children queue mein daalo.
4. Current `level` ko `ans` mein push karo.
5. Finally `ans` return karo.

---

# 7. Code

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {

        vector<vector<int>> ans;

        if (root == NULL)
            return ans;

        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {

            int size = q.size();
            vector<int> level;

            for (int i = 0; i < size; i++) {

                TreeNode* node = q.front();
                q.pop();

                level.push_back(node->val);

                if (node->left != NULL)
                    q.push(node->left);

                if (node->right != NULL)
                    q.push(node->right);
            }

            ans.push_back(level);
        }

        return ans;
    }
};
