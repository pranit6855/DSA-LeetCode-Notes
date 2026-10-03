# LeetCode 450 — Delete Node in a BST

## Problem

Given the root of a Binary Search Tree (BST) and an integer `key`, delete the node with value `key` from the BST.

After deletion, the resulting tree must still satisfy the BST property.

Return the root of the updated BST.

---

# Example

Given:

        5
       / \
      3   7
     / \   \
    2   4   8

Delete:

`3`

Final tree:

        5
       / \
      4   7
     /     \
    2       8

---

# Main Idea

BST mein node search karna already known hai:

`key < root->val`
→ left jao

`key > root->val`
→ right jao

`key == root->val`
→ node mil gaya, ab delete karo

Deletion ke 3 cases hote hain:

1. No child
2. One child
3. Two children

---

# Case 1 — No Child

Example:

        5
       / \
      3   7

Agar `3` leaf hai:

        5
       / \
      3   7

`3` ke left aur right dono `NULL` hain.

Delete karne ke baad:

        5
         \
          7

Code:

    if (root->left == NULL && root->right == NULL) {
        return NULL;
    }

Meaning:

Current node ko remove kar do aur uski jagah `NULL` return karo.

---

# Case 2 — One Child

## Only Right Child

Example:

      7
       \
        8
         \
          9

Delete `8`.

`8` ka sirf right child `9` hai.

Result:

      7
       \
        9

Code:

    if (root->left == NULL) {
        return root->right;
    }

Meaning:

Current node ko hatao aur uski right child ko uski jagah return kar do.

---

## Only Left Child

Example:

      7
     /
    5
   /
  4

Delete `5`.

Result:

      7
     /
    4

Code:

    if (root->right == NULL) {
        return root->left;
    }

Meaning:

Current node ko hatao aur uski left child ko uski jagah return kar do.

---

# Case 3 — Two Children

Ye sabse important case hai.

Example:

        5
       / \
      3   7
     / \
    2   4

Delete `3`.

Node `3` ke dono children hain:

Left = `2`

Right = `4`

Direct `3` ko remove nahi kar sakte because BST structure maintain karna hai.

Isliye hum `3` ki jagah ek suitable value laayenge.

---

# Inorder Successor

Two-child case mein hum **inorder successor** use kar sakte hain.

Inorder successor:

**Right subtree ka smallest element**

Example:

        3
       / \
      2   4

Right subtree:

    4

So successor:

`4`

---

# Successor Find Karna

Code:

    TreeNode* successor = root->right;

    while (successor->left != NULL) {
        successor = successor->left;
    }

Meaning:

1. Right subtree mein jao.
2. Phir jitna left ja sakte ho, left jao.
3. Jo last node milega, wahi right subtree ka smallest node hai.

---

# Example with Bigger Right Subtree

Suppose:

        5
       / \
      3   8
         / \
        6   9
       /
      4

Agar `5` delete karna hai:

Right subtree:

        8
       / \
      6   9
     /
    4

Right subtree ka smallest:

`4`

Path:

`8 → 6 → 4`

So successor = `4`.

---

# Why Right Subtree Ka Minimum?

BST mein:

`Left < Root < Right`

Current node ko replace karne ke liye aisi value chahiye jo:

- current node ke right side mein valid ho
- current node se badi ho
- lekin right subtree mein sabse chhoti ho

Right subtree ka minimum exactly ye condition satisfy karta hai.

---

# Two-Child Case — Step by Step

Tree:

        5
       / \
      3   7
     / \
    2   4

Delete `3`.

## Step 1

Successor:

    TreeNode* successor = root->right;

`root = 3`

So:

`successor = 4`

---

## Step 2

Find smallest:

    while (successor->left != NULL) {
        successor = successor->left;
    }

4 ka left `NULL` hai.

So successor remains `4`.

---

## Step 3

Successor ki value current node mein copy karo:

    root->val = successor->val;

Current:

`3`

Successor:

`4`

So current node ki value:

`3 → 4`

Temporary tree:

        5
       / \
      4   7
     / \
    2   4

Ab do `4` hain.

Ye temporary state hai.

---

## Step 4

Original successor node delete karo.

Code:

    root->right = deleteNode(root->right, successor->val);

Meaning:

Current node ki value `4` ho gayi hai, lekin original right subtree mein purana `4` abhi bhi pada hai.

Isliye right subtree mein:

`4`

ko delete karo.

Eventually:

        5
       / \
      4   7
     /
    2

Final valid BST.

---

# Why We Copy the Value?

Hum successor node ko directly current node par shift nahi kar rahe.

Hum sirf:

    root->val = successor->val;

kar rahe hain.

Then old successor node ko recursively delete kar dete hain.

Isse BST structure easily maintain hota hai.

---

# Important Recursion Pattern

Ye lines bahut important hain:

    root->left = deleteNode(root->left, key);

    root->right = deleteNode(root->right, key);

Inka meaning:

**Left subtree mein deletion karo aur updated subtree ko left pointer mein attach karo.**

Similarly:

**Right subtree mein deletion karo aur updated subtree ko right pointer mein attach karo.**

---

# Why `return` Nahi Lagana Hai Yahan?

Correct:

    if (key < root->val) {
        root->left = deleteNode(root->left, key);
    }

Incorrect:

    if (key < root->val) {
        return root->left = deleteNode(root->left, key);
    }

Why?

Because hume current root ko preserve karna hai.

Example:

        5
       /
      3
     /
    2

Agar `3` delete karna hai:

`deleteNode(3, 3)` updated subtree return karega.

Parent `5` ko ye receive karna hai:

    root->left = updated_subtree;

So `5` ka left pointer update hoga.

Finally:

    return root;

Current root `5` upar return hoga.

Agar directly `return root->left = ...` kar diya, toh current root `5` return nahi hoga.

---

# Complete Approach

## Step 1 — Search the node

If:

    key < root->val

Go left.

If:

    key > root->val

Go right.

If:

    key == root->val

Node found.

---

## Step 2 — Delete according to number of children

### 0 child

Return:

`NULL`

### 1 child

Return the existing child.

### 2 children

1. Find inorder successor.
2. Successor = right subtree ka minimum.
3. Current node ki value successor se replace karo.
4. Original successor ko delete karo.

---

# Final C++ Code

    class Solution {
    public:
        TreeNode* deleteNode(TreeNode* root, int key) {

            // Base case
            if (root == NULL) {
                return NULL;
            }

            // Key is smaller → go left
            if (key < root->val) {
                root->left = deleteNode(root->left, key);
            }

            // Key is larger → go right
            else if (key > root->val) {
                root->right = deleteNode(root->right, key);
            }

            // Node found
            else {

                // Case 1: No child
                if (root->left == NULL && root->right == NULL) {
                    return NULL;
                }

                // Case 2: Only right child
                if (root->left == NULL) {
                    return root->right;
                }

                // Case 2: Only left child
                if (root->right == NULL) {
                    return root->left;
                }

                // Case 3: Two children

                // Start from right subtree
                TreeNode* successor = root->right;

                // Find smallest node in right subtree
                while (successor->left != NULL) {
                    successor = successor->left;
                }

                // Copy successor value
                root->val = successor->val;

                // Delete original successor
                root->right = deleteNode(root->right, successor->val);
            }

            return root;
        }
    };

---

# Detailed Dry Run

Tree:

        5
       / \
      3   7
     / \
    2   4

Key:

`3`

---

## Step 1 — At 5

Current:

`5`

Key:

`3`

Since:

`3 < 5`

Go left.

Code:

    root->left = deleteNode(root->left, key);

So:

`deleteNode(3, 3)`

---

## Step 2 — At 3

Current:

`3`

Key:

`3`

So:

`key == root->val`

Node found.

Check children:

Left = `2`

Right = `4`

Both exist.

Therefore:

**Case 3: Two children**

---

## Step 3 — Find Successor

Start:

`successor = root->right`

So:

`successor = 4`

4 ka left `NULL` hai.

Therefore successor:

`4`

---

## Step 4 — Replace Value

Before:

        5
       / \
      3   7
     / \
    2   4

Execute:

    root->val = successor->val;

After:

        5
       / \
      4   7
     / \
    2   4

---

## Step 5 — Delete Duplicate Successor

Execute:

    root->right = deleteNode(root->right, successor->val);

Current node ka right subtree:

`4`

So:

`deleteNode(4, 4)`

4 leaf node hai.

Therefore:

`return NULL`

Ab current root ka right:

`NULL`

Final:

        5
       / \
      4   7
     /
    2

---

# Recursion Flow

For searching:

        5
       /
      3

Flow:

`deleteNode(5, 3)`

↓

`deleteNode(3, 3)`

↓

Node found

↓

Delete

Then updated subtree goes back:

`3 subtree → 5->left`

So parent-child connection remains correct.

---

# Three Cases Quick Revision

## Case 1

        5
       /
      3

`3` has 0 children.

Return:

`NULL`

---

## Case 2

        5
       /
      3
     /
    2

Delete `3`.

Return:

`2`

Result:

        5
       /
      2

---

## Case 3

        5
       /
      3
     / \
    2   4

Delete `3`.

Successor:

`4`

Replace:

`3 → 4`

Delete original `4`.

Result:

        5
       /
      4
     /
    2

---

# Important Pattern

When you see:

**Delete a node in BST**

Think:

`Search`

↓

`Node found`

↓

`0 child / 1 child / 2 children`

For 2 children:

`Right subtree`

↓

`Minimum / leftmost`

↓

`Copy value`

↓

`Delete old successor`

---

# Memory Trick

**BST Delete:**

`Find → Delete Cases`

0 child:

`NULL`

1 child:

`Return child`

2 children:

`Successor → Copy → Delete successor`

---

# Complexity

Let `h` = height of the BST.

Searching for the node takes:

`O(h)`

Finding the successor takes:

`O(h)`

Deleting the successor takes:

`O(h)`

Overall:

`O(h)`

For a balanced BST:

`O(log n)`

For a skewed BST:

`O(n)`

Recursive space:

`O(h)`

---

# Interview Explanation

"I first search for the key using the BST property. Once I find the node, there are three cases. If it is a leaf, I return null. If it has one child, I return that child. If it has two children, I find the inorder successor, which is the minimum node in the right subtree, copy its value into the current node, and then recursively delete the original successor node."

---

# Key Takeaways

1. BST search uses ordering.
2. Deletion has three cases.
3. For two children, use inorder successor.
4. Inorder successor = minimum in right subtree.
5. `root->left = deleteNode(...)` updates the subtree attached to the parent.
6. Do not return directly from the left/right recursive assignment.
7. Finally return the current `root`.

---

# LeetCode

Problem: `450`

Name: `Delete Node in a BST`

Pattern: `Search / BST`

Technique: `BST Recursion + Inorder Successor`

Difficulty: Medium
