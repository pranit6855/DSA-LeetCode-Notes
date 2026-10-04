# LeetCode 113 — Path Sum II

## Problem

Given the root of a binary tree and an integer `targetSum`, return all the **root-to-leaf paths** where the sum of node values is equal to `targetSum`.

Each path should be returned as a list of node values.

---

# Example

Tree:

        5
       / \
      4   8
     /   / \
    11  13  4
   /  \      \
  7    2      5

`targetSum = 22`

Valid paths:

`5 → 4 → 11 → 2`

`5 → 8 → 4 → 5`

So answer:

[
    [5,4,11,2],
    [5,8,4,5]
]

---

# Connection With LC 112

LC 112 mein humne check kiya tha:

**Kya koi root-to-leaf path target sum deta hai?**

Output:

`true / false`

LC 113 mein:

**Saare valid root-to-leaf paths return karne hain.**

So LC 113 = LC 112 + path tracking.

---

# Root-to-Leaf Path

Root se start karke leaf tak jaana root-to-leaf path hai.

Example:

        5
       / \
      3   8
     / \
    2   4

Valid paths:

`5 → 3 → 2`

`5 → 3 → 4`

`5 → 8`

`5 → 3` complete path nahi hai because `3` leaf nahi hai.

---

# Main Concept

Hume do cheezein maintain karni hain:

## 1. Current Path

    vector<int> path;

Ye current root-to-current-node path store karega.

Example:

`[5,4,11]`

## 2. Answer

    vector<vector<int>> ans;

Ye saare valid paths store karega.

---

# Backtracking

LC 113 ka sabse important new concept:

**Backtracking**

Pattern:

`push`

↓

`recurse`

↓

`pop`

Current node ko path mein add karo:

    path.push_back(root->val);

Left aur right explore karo.

Kaam complete hone ke baad current node remove karo:

    path.pop_back();

---

# `push_back()` Kyun?

Suppose path hai:

`5 → 4 → 11`

Then:

    path = [5,4,11]

Ab next node `2` hai:

    path.push_back(2);

Now:

    path = [5,4,11,2]

Ye current path ko represent karta hai.

---

# `pop_back()` Kyun?

Maan lo `2` ka kaam complete ho gaya.

Ab recursion wapas `11` par aayegi.

Path mein `2` nahi rehna chahiye because ab hum `11` ke doosre branch ko explore kar sakte hain.

So:

    path.pop_back();

Path:

`[5,4,11]`

Yahi backtracking hai.

---

# Important: Do `pop_back()` Kyu Hain?

Code mein `pop_back()` do places par ho sakta hai.

## Leaf case

    path.pop_back();
    return;

Leaf par path check karne ke baad immediately return kar dete hain.

## Non-leaf case

    solve(left,...);
    solve(right,...);

    path.pop_back();

Current node ke dono children explore karne ke baad current node remove karte hain.

Important:

**Ek execution mein same node ko dono baar pop nahi karte.**

Leaf case mein `return` ho jata hai, isliye neeche wala `pop_back()` execute nahi hota.

---

# Remaining Target

LC 112 ki tarah hum target ko reduce karenge.

Har node par:

    targetSum -= root->val;

Example:

Target = `22`

Path:

`5 → 4 → 11 → 2`

Calculations:

`22 - 5 = 17`

`17 - 4 = 13`

`13 - 11 = 2`

`2 - 2 = 0`

Leaf par target `0`:

Valid path.

---

# Approach

Har node par:

1. Agar `NULL` hai → return.
2. Current node ko `path` mein add karo.
3. Current node ki value `targetSum` se subtract karo.
4. Agar leaf hai:
   - `targetSum == 0` ho toh path ko answer mein add karo.
   - Current node ko path se remove karo.
   - Return.
5. Left subtree explore karo.
6. Right subtree explore karo.
7. Current node ko path se remove karo.

---

# C++ Code

    class Solution {
    public:
        void solve(TreeNode* root, int targetSum,
                   vector<int>& path,
                   vector<vector<int>>& ans) {

            if (root == NULL)
                return;

            // Current node ko path mein add karo
            path.push_back(root->val);

            // Current node ki value subtract karo
            targetSum -= root->val;

            // Leaf node
            if (root->left == NULL && root->right == NULL) {

                if (targetSum == 0) {
                    ans.push_back(path);
                }

                // Backtracking
                path.pop_back();
                return;
            }

            // Left subtree
            solve(root->left, targetSum, path, ans);

            // Right subtree
            solve(root->right, targetSum, path, ans);

            // Backtracking
            path.pop_back();
        }

        vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
            vector<vector<int>> ans;
            vector<int> path;

            solve(root, targetSum, path, ans);

            return ans;
        }
    };

---

# Code Explanation

## 1. `solve()` Function

    void solve(TreeNode* root, int targetSum,
               vector<int>& path,
               vector<vector<int>>& ans)

Four things:

`root` → current node

`targetSum` → remaining target

`path` → current path

`ans` → all valid paths

---

# 2. Base Case

    if (root == NULL)
        return;

Agar node NULL hai toh path nahi ho sakta.

Simply return.

---

# 3. Current Node Path Mein Add

    path.push_back(root->val);

Current node ko current path mein add kar diya.

Example:

Before:

`[5,4]`

Current node = `11`

After:

`[5,4,11]`

---

# 4. Target Reduce

    targetSum -= root->val;

Current node ka value path sum mein include ho gaya.

Example:

Target:

`22`

Current:

`5`

After:

`17`

---

# 5. Leaf Check

    if (root->left == NULL && root->right == NULL)

Ye check karta hai ki current node leaf hai ya nahi.

Leaf ka:

`left = NULL`

`right = NULL`

---

# 6. Valid Path Check

    if (targetSum == 0) {
        ans.push_back(path);
    }

Agar leaf par remaining target zero hai, toh current root-to-leaf path valid hai.

Then:

    ans.push_back(path);

current path ki copy answer mein store kar deta hai.

Important:

`ans` mein copy store hoti hai.

Baad mein `path` change hone se stored answer change nahi hota.

---

# 7. Leaf Backtracking

    path.pop_back();
    return;

Leaf ka kaam complete ho gaya.

Current node ko path se remove kar do.

Then return.

Example:

Before:

`[5,4,11,2]`

After:

`[5,4,11]`

---

# 8. Left Subtree

    solve(root->left, targetSum, path, ans);

Left child ke liye same function dobara run hota hai.

Current `path` aur updated `targetSum` pass hote hain.

---

# 9. Right Subtree

    solve(root->right, targetSum, path, ans);

Same logic right subtree ke liye.

---

# 10. Final Backtracking

    path.pop_back();

Current node ke dono children process hone ke baad current node remove karte hain.

Example:

Path:

`[5,4,11]`

11 ke left aur right dono explore ho gaye.

Ab 11 ki zarurat current path mein nahi.

So:

`[5,4]`

---

# Detailed Dry Run

Tree:

        5
       / \
      4   8
     /   / \
    11  13  4
   /  \      \
  7    2      5

`targetSum = 22`

Expected valid paths:

`5 → 4 → 11 → 2`

`5 → 8 → 4 → 5`

---

# Step 1 — Node 5

Initial:

`path = []`

`target = 22`

Push 5:

`path = [5]`

Subtract:

`target = 22 - 5 = 17`

5 leaf nahi hai.

Go left.

---

# Tree Again

        5
       / \
      4   8
     /   / \
    11  13  4
   /  \      \
  7    2      5

Current:

`5`

Path:

`[5]`

Target:

`17`

---

# Step 2 — Node 4

Push:

`path = [5,4]`

Target:

`17 - 4 = 13`

4 leaf nahi hai.

Go left.

---

# Step 3 — Node 11

Push:

`path = [5,4,11]`

Target:

`13 - 11 = 2`

11 leaf nahi hai.

Go left to `7`.

---

# Step 4 — Node 7

Push:

`path = [5,4,11,7]`

Target:

`2 - 7 = -5`

7 is leaf.

Target:

`-5`

not zero.

So this is not a valid path.

Backtrack:

`path.pop_back()`

Path becomes:

`[5,4,11]`

Return.

---

# Tree Again

        5
       / \
      4   8
     /   / \
    11  13  4
   /  \      \
  7    2      5

Now recursion comes back to node `11`.

Node `11` ka right child `2` hai.

---

# Step 5 — Node 2

Push:

`path = [5,4,11,2]`

Target:

`2 - 2 = 0`

Node `2` leaf hai.

Check:

`target == 0`

True.

So:

    ans.push_back(path);

Stored answer:

`[5,4,11,2]`

---

# Backtracking After 2

Current path:

`[5,4,11,2]`

Pop:

`path.pop_back()`

Now:

`[5,4,11]`

Return.

---

# Back to Node 11

Node 11 ka left aur right dono complete.

So final backtracking:

`path.pop_back()`

Path:

`[5,4]`

Return to node 4.

---

# Node 4 Complete

Node 4 ka left subtree complete.

4 ka right NULL hai.

Then:

`path.pop_back()`

Path:

`[5]`

Return to root 5.

---

# Tree Again

        5
       / \
      4   8
     /   / \
    11  13  4
   /  \      \
  7    2      5

Ab left side complete.

Current path:

`[5]`

Ab root 5 ka right explore hoga.

---

# Step 6 — Node 8

Push:

`path = [5,8]`

Target:

`22 - 5 - 8`

`= 9`

8 leaf nahi hai.

Explore left 13.

---

# Step 7 — Node 13

Push:

`path = [5,8,13]`

Target:

`9 - 13 = -4`

13 leaf hai.

Target zero nahi.

Invalid path.

Pop 13:

`[5,8]`

---

# Tree Again

        5
       / \
      4   8
     /   / \
    11  13  4
   /  \      \
  7    2      5

Now node 8 ka right child `4`.

---

# Step 8 — Node 4

Push:

`path = [5,8,4]`

Target:

`9 - 4 = 5`

4 leaf nahi hai because right child `5` hai.

Go right.

---

# Step 9 — Node 5

Push:

`path = [5,8,4,5]`

Target:

`5 - 5 = 0`

5 leaf hai.

Target zero.

Valid path.

Store:

`[5,8,4,5]`

---

# Backtracking

After saving:

`[5,8,4,5]`

Pop:

`[5,8,4]`

Return.

Then node 4 complete:

Pop:

`[5,8]`

Then node 8 complete:

Pop:

`[5]`

Then root complete:

Pop:

`[]`

---

# Final Answer

Answer contains:

`[5,4,11,2]`

`[5,8,4,5]`

So:

[
    [5,4,11,2],
    [5,8,4,5]
]

---

# Backtracking Visualization

Starting:

`[]`

At 5:

`[5]`

At 4:

`[5,4]`

At 11:

`[5,4,11]`

At 2:

`[5,4,11,2]`

Valid → store.

Pop:

`[5,4,11]`

Return.

Pop:

`[5,4]`

Return.

Pop:

`[5]`

Then explore right side.

This is exactly:

`Push → Explore → Pop`

---

# Why `ans.push_back(path)`?

Suppose:

`path = [5,4,11,2]`

We do:

    ans.push_back(path);

The path gets stored in `ans`.

Then we can safely backtrack:

    path.pop_back();

The stored answer remains:

`[5,4,11,2]`

because vector is copied into `ans`.

---

# Why We Need Backtracking?

Suppose we don't remove nodes.

First path:

`[5,4,11,2]`

Then while exploring another branch, path might incorrectly become:

`[5,4,11,2,7]`

which is wrong.

We need to remove nodes when returning from recursion.

Hence:

`push_back()`

when entering a node.

`pop_back()`

when leaving a node.

---

# Important Pattern

For path problems involving all paths:

`push`

↓

`recurse`

↓

`pop`

This pattern is called **backtracking**.

---

# LC 112 vs LC 113

## LC 112

Question:

**Does a valid path exist?**

Return:

`true / false`

Main idea:

`remaining target`

---

## LC 113

Question:

**What are all valid paths?**

Return:

`vector<vector<int>>`

Need:

`remaining target + path + backtracking`

---

# Common Mistakes

## Mistake 1

Leaf ke bina target check karna.

Wrong:

    if (targetSum == 0)
        ans.push_back(path);

Correct:

    if (root->left == NULL && root->right == NULL) {
        if (targetSum == 0) {
            ans.push_back(path);
        }
    }

---

## Mistake 2

`pop_back()` bhool jaana.

Without backtracking, old nodes next paths mein bhi reh jayenge.

---

## Mistake 3

Current node ko path mein add na karna.

Always:

    path.push_back(root->val);

before exploring children.

---

# Complexity

Let `n` = number of nodes.

Tree traversal:

`O(n)`

But since valid paths store karne hain, output size bhi consider karna padta hai.

Path copying can add extra cost proportional to path length.

In typical interview analysis:

**Traversal:** `O(n)`

**Auxiliary recursion/path space:** `O(h)`

**Output space:** depends on number and length of valid paths.

---

# Interview Explanation

"I use DFS with backtracking. I maintain the current root-to-node path in a vector and the remaining target sum. At every node, I add its value to the path and subtract it from the target. When I reach a leaf and the remaining target becomes zero, I store the current path in the result. After exploring a node's children, I remove that node from the path to backtrack and explore other paths."

---

# Memory Trick

**LC 113 = LC 112 + path vector + backtracking**

Main pattern:

`push_back`

↓

`DFS left/right`

↓

`pop_back`

Leaf:

`target == 0`

→ `ans.push_back(path)`

---

# LeetCode

Problem: `113`

Name: `Path Sum II`

Pattern: `Path Sum`

Technique: `DFS + Backtracking + Path Tracking`

Difficulty: Medium
