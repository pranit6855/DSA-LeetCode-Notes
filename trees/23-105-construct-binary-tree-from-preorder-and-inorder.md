# LeetCode 105 — Construct Binary Tree from Preorder and Inorder Traversal

## Problem

Given two integer arrays:

- `preorder`
- `inorder`

Construct the binary tree and return its root.

---

# Example

Preorder:

`[3, 9, 20, 15, 7]`

Inorder:

`[9, 3, 15, 20, 7]`

Constructed tree:

        3
       / \
      9   20
         /  \
        15   7

---

# Main Concept

Is problem ka core sirf do traversal properties hain:

**Preorder → Root batata hai**

**Inorder → Root ke left aur right subtree ko separate karta hai**

Memory trick:

`PREORDER = ROOT`

`INORDER = SPLIT`

---

# Preorder Property

Preorder traversal:

`Root → Left → Right`

Isliye preorder ka first available element current subtree ka root hota hai.

Example:

`preorder = [3, 9, 20, 15, 7]`

First element:

`3`

So:

`root = 3`

---

# Inorder Property

Inorder traversal:

`Left → Root → Right`

Inorder mein root ko find karo.

Example:

`inorder = [9, 3, 15, 20, 7]`

Root:

`3`

So:

`[9] 3 [15,20,7]`

Therefore:

Left subtree:

`[9]`

Right subtree:

`[15,20,7]`

---

# Why Both Traversals Are Needed?

Preorder batata hai:

**Root kaun hai?**

Inorder batata hai:

**Root ke left aur right mein kaun hai?**

Dono ko combine karke tree reconstruct kar sakte hain.

---

# Example Step by Step

Preorder:

`[3, 9, 20, 15, 7]`

Inorder:

`[9, 3, 15, 20, 7]`

## Step 1

Preorder ka first:

`3`

So:

        3

Inorder mein `3`:

`[9] 3 [15,20,7]`

So:

Left:

`[9]`

Right:

`[15,20,7]`

---

## Step 2 — Left Subtree

Left subtree mein sirf:

`9`

hai.

So:

        3
       /
      9

`9` leaf hai.

---

## Step 3 — Right Subtree

Right subtree:

`[15,20,7]`

Preorder mein current next root:

`20`

So:

        3
       / \
      9   20

Inorder mein `20`:

`[15] 20 [7]`

Therefore:

Left of 20:

`15`

Right of 20:

`7`

Final:

        3
       / \
      9   20
         /  \
        15   7

---

# Recursive Idea

Har subtree ke liye same process repeat hota hai:

1. Preorder se current root uthao.
2. Inorder mein root find karo.
3. Inorder ke left part se left subtree banao.
4. Inorder ke right part se right subtree banao.
5. Same process recursively repeat karo.

Flow:

`Preorder → Root`

↓

`Inorder → Split`

↓

`Build Left`

↓

`Build Right`

---

# Why `preIndex`?

Preorder se roots ek-ek karke uthana hai.

Isliye ek variable:

`preIndex`

rakhenge.

Initially:

`preIndex = 0`

Then:

`preorder[0] = 3`

use karke:

`preIndex = 1`

Then:

`preorder[1] = 9`

use karke:

`preIndex = 2`

Then:

`preorder[2] = 20`

and so on.

So `preIndex` tells us:

**Preorder ka next root kaunsa hai?**

---

# Why `inStart` and `inEnd`?

Hume poori inorder array har recursion mein nahi deni.

Instead current subtree ke inorder range ko track karenge.

For example:

Root `3` ka inorder index `1` hai.

Then:

Left subtree range:

`0 to 0`

Right subtree range:

`2 to 4`

So recursion ko pata hai ki current subtree ke liye inorder ka kaunsa part use karna hai.

---

# `inStart` and `inEnd` Meaning

`inStart`:

Current subtree ka inorder starting index.

`inEnd`:

Current subtree ka inorder ending index.

Agar:

`inStart > inEnd`

toh koi element nahi bacha.

So:

`return NULL`

---

# Why HashMap?

Simple approach mein inorder mein root find karne ke liye loop use kar sakte hain.

Example:

    int inIndex = inStart;

    while (inIndex <= inEnd && inorder[inIndex] != rootValue) {
        inIndex++;
    }

Ye correct hai.

But har recursion mein search karne se worst-case complexity `O(n^2)` ho sakti hai.

Better approach:

`value → inorder index`

HashMap mein store kar do.

Then root ka index `O(1)` average mein mil jayega.

---

# Simple Version Using While Loop

Learning ke liye while-loop version easy hai:

    class Solution {
    public:
        TreeNode* build(vector<int>& preorder, vector<int>& inorder,
                        int& preIndex, int inStart, int inEnd) {

            if (inStart > inEnd)
                return NULL;

            int rootValue = preorder[preIndex];
            preIndex++;

            TreeNode* root = new TreeNode(rootValue);

            int inIndex = inStart;

            while (inIndex <= inEnd && inorder[inIndex] != rootValue) {
                inIndex++;
            }

            root->left = build(preorder, inorder,
                               preIndex, inStart, inIndex - 1);

            root->right = build(preorder, inorder,
                                preIndex, inIndex + 1, inEnd);

            return root;
        }

        TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
            int preIndex = 0;

            return build(preorder, inorder,
                         preIndex, 0, inorder.size() - 1);
        }
    };

---

# Code Explanation

## 1. `build()` Function

    TreeNode* build(vector<int>& preorder,
                    vector<int>& inorder,
                    int& preIndex,
                    int inStart,
                    int inEnd)

Parameters:

`preorder` → root order

`inorder` → left/right separation

`preIndex` → next root in preorder

`inStart` → current inorder range start

`inEnd` → current inorder range end

---

# 2. Base Case

    if (inStart > inEnd)
        return NULL;

Agar current inorder range empty hai, toh subtree empty hai.

So:

`NULL`

---

# 3. Root from Preorder

    int rootValue = preorder[preIndex];

Preorder ka current element current subtree ka root hai.

Example:

`preIndex = 0`

`preorder[0] = 3`

Therefore:

`rootValue = 3`

---

# 4. Move `preIndex`

    preIndex++;

Root use ho gaya.

So next recursion ke liye preorder ka next element ready hai.

---

# 5. Create Node

    TreeNode* root = new TreeNode(rootValue);

Ab actual tree node create hota hai.

Example:

`rootValue = 3`

Tree:

        3

---

# 6. Find Root in Inorder

    int inIndex = inStart;

    while (inIndex <= inEnd && inorder[inIndex] != rootValue) {
        inIndex++;
    }

Inorder mein root ki position find kar rahe hain.

Example:

`inorder = [9,3,15,20,7]`

Root:

`3`

Index:

`1`

---

# 7. Build Left Subtree

    root->left = build(preorder, inorder,
                       preIndex,
                       inStart,
                       inIndex - 1);

Root `3` index `1` par hai.

So left subtree:

`inStart = 0`

`inEnd = 0`

Meaning:

`[9]`

Left subtree banao.

---

# 8. Build Right Subtree

    root->right = build(preorder, inorder,
                        preIndex,
                        inIndex + 1,
                        inEnd);

Root `3` index `1` par hai.

So right subtree:

`2 to 4`

Meaning:

`[15,20,7]`

Right subtree banao.

---

# 9. Return Root

    return root;

Current subtree complete ho gaya.

So us subtree ka root parent ko return kar do.

---

# `root->left = build(...)` Ka Meaning

Ye line bahut important hai:

    root->left = build(...);

Matlab:

**Left subtree recursively banao aur jo subtree root return ho, usko current node ke left pointer se connect karo.**

Example:

Agar build returns:

`9`

then:

`3->left = 9`

Tree:

        3
       /
      9

---

# `root->right = build(...)`

Same idea:

**Right subtree recursively banao aur returned root ko right pointer se attach karo.**

Example:

`20` return hua:

`3->right = 20`

Tree:

        3
       / \
      9   20

---

# Detailed Dry Run

Given:

Preorder:

`[3,9,20,15,7]`

Inorder:

`[9,3,15,20,7]`

Indexes:

`0 1 2 3 4`

Initial:

`preIndex = 0`

`inStart = 0`

`inEnd = 4`

---

# Step 1 — Root 3

Preorder:

`[3,9,20,15,7]`

`preIndex = 0`

So:

`rootValue = 3`

Then:

`preIndex = 1`

Create:

        3

Inorder:

`[9,3,15,20,7]`

3 index `1` par.

Split:

`[9] 3 [15,20,7]`

---

# Step 2 — Build Left of 3

Inorder range:

`0 to 0`

PreIndex:

`1`

Preorder[1]:

`9`

So left root:

`9`

Tree:

        3
       /
      9

`preIndex = 2`

For 9:

Left range:

`0 to -1`

Invalid → NULL.

Right range:

`1 to 0`

Invalid → NULL.

So 9 complete.

---

# Tree Again

        3
       /
      9

Now recursion returns to node 3.

---

# Step 3 — Build Right of 3

Inorder range:

`2 to 4`

Current:

`preIndex = 2`

Preorder[2]:

`20`

So:

`20` = right subtree root.

Tree:

        3
       / \
      9   20

`preIndex = 3`

---

# Step 4 — Inorder Mein 20

Inorder:

`[9,3,15,20,7]`

20 index `3` par.

Split:

`[15] 20 [7]`

So:

Left of 20:

`2 to 2`

Right of 20:

`4 to 4`

---

# Step 5 — Build Left of 20

PreIndex:

`3`

Preorder[3]:

`15`

So:

        3
       / \
      9   20
         /
        15

`preIndex = 4`

15 leaf hai.

So its left and right are NULL.

---

# Step 6 — Build Right of 20

PreIndex:

`4`

Preorder[4]:

`7`

So:

        3
       / \
      9   20
         /  \
        15   7

`preIndex = 5`

7 leaf hai.

---

# Final Tree

        3
       / \
      9   20
         /  \
        15   7

---

# Full Recursion Flow

Starting:

`build(3)`

↓

Left:

`build(9)`

↓

9 complete

↓

Back to 3

↓

Right:

`build(20)`

↓

Left:

`build(15)`

↓

15 complete

↓

Right:

`build(7)`

↓

7 complete

↓

20 complete

↓

3 complete

---

# `preIndex` Flow

Preorder:

`[3,9,20,15,7]`

Start:

`preIndex = 0`

### Root

`3`

After:

`preIndex = 1`

### Left Root

`9`

After:

`preIndex = 2`

### Right Root

`20`

After:

`preIndex = 3`

### Left of 20

`15`

After:

`preIndex = 4`

### Right of 20

`7`

After:

`preIndex = 5`

So every node gets created exactly according to preorder order.

---

# Inorder Split Flow

## Root 3

`[9] 3 [15,20,7]`

## Root 20

`[15] 20 [7]`

Final structure:

        3
       / \
      9   20
         /  \
        15   7

---

# Why Recursion Works

Har baar ek smaller subtree ban raha hai.

For root `3`:

`[9]` + `[15,20,7]`

For root `20`:

`[15]` + `[7]`

Eventually single-element subtree:

`9`

`15`

`7`

ban jaata hai.

Single element node ke liye:

Left = NULL

Right = NULL

So recursion naturally stop hoti hai.

---

# Main Pattern

**Construction questions mein traversal properties use karo.**

For LC 105:

`Preorder → ROOT`

`Inorder → SPLIT`

Then:

`Recursively build LEFT`

`Recursively build RIGHT`

---

# Memory Trick

Imagine preorder saying:

**"Main bataunga next root kaun hai."**

And inorder saying:

**"Main bataunga root ke left aur right mein kaun hai."**

So:

`PREORDER = ROOT`

`INORDER = SPLIT`

---

# Common Mistakes

## 1. Preorder ka first element bhool jaana

Always:

`preorder[preIndex]`

= current root.

---

## 2. Inorder split galat karna

Agar root index `inIndex` hai:

Left:

`inStart ... inIndex - 1`

Right:

`inIndex + 1 ... inEnd`

---

## 3. `preIndex` ko copy karna

`preIndex` should be:

`int& preIndex`

because same pointer/counter ko recursion ke across update karna hai.

---

## 4. Left and Right ranges mix karna

Left:

`inStart, inIndex - 1`

Right:

`inIndex + 1, inEnd`

---

## 5. `return root` bhoolna

Current subtree complete hone ke baad:

    return root;

Parent ko current subtree ka root chahiye.

---

# Complexity — While Loop Version

Agar inorder mein root ko har baar while-loop se search kar rahe hain:

Worst case:

`O(n²)`

Space:

`O(h)`

where `h` = tree height.

---

# Optimized Version Using HashMap

Better interview version:

    class Solution {
    public:
        unordered_map<int, int> mp;
        int preIndex = 0;

        TreeNode* build(vector<int>& preorder,
                        int inStart,
                        int inEnd) {

            if (inStart > inEnd)
                return NULL;

            int rootValue = preorder[preIndex++];

            TreeNode* root = new TreeNode(rootValue);

            int inIndex = mp[rootValue];

            root->left = build(preorder,
                               inStart,
                               inIndex - 1);

            root->right = build(preorder,
                                inIndex + 1,
                                inEnd);

            return root;
        }

        TreeNode* buildTree(vector<int>& preorder,
                            vector<int>& inorder) {

            for (int i = 0; i < inorder.size(); i++) {
                mp[inorder[i]] = i;
            }

            return build(preorder, 0, inorder.size() - 1);
        }
    };

This optimized version gives:

Time:

`O(n)`

Space:

`O(n)`

because of the hash map plus recursion.

---

# Which Version Should You Remember?

For learning the concept:

**While-loop version**

is easier.

For interviews / optimal solution:

**HashMap version**

is better.

The core concept is the same:

`Preorder → Root`

`Inorder → Split`

---

# Interview Explanation

"Preorder traversal always gives the root of the current subtree because its order is Root, Left, Right. I find this root in the inorder traversal, where everything before it belongs to the left subtree and everything after it belongs to the right subtree. I then recursively construct the left and right subtrees. I maintain a shared preorder index so that the next root is picked in preorder order."

---

# Final Revision

## Given

`Preorder = [3,9,20,15,7]`

`Inorder = [9,3,15,20,7]`

## Do

`Preorder first → 3`

`Inorder mein 3 find → split`

`[9] | 3 | [15,20,7]`

Then recursively:

`9`

and:

`20`

For 20:

`[15] | 20 | [7]`

Final:

        3
       / \
      9   20
         /  \
        15   7

---

# Pattern 6 Progress

`105 — Construct Binary Tree from Preorder + Inorder ✅`

Remaining:

`106 — Construct Binary Tree from Postorder + Inorder ⏳`

`108 — Convert Sorted Array to BST ⏳`

---

# LeetCode

Problem: `105`

Name: `Construct Binary Tree from Preorder and Inorder Traversal`

Pattern: `Construction`

Technique: `Recursion + Preorder Root + Inorder Split`

Core Idea:

`Preorder → Root`

`Inorder → Left / Right Split`

Difficulty: Medium
