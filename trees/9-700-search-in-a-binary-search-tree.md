# LC 700 — Search in a Binary Search Tree

## Problem

Given the root of a Binary Search Tree (BST) and an integer `val`,
search for a node whose value is equal to `val`.

If the node exists, return the root of that node's subtree.

If the node does not exist, return `NULL`.

Example:

        8
       / \
      3   10
     / \
    1   6
       / \
      4   7

Target:

    val = 6

Search path:

    6 < 8
    → go LEFT

    6 > 3
    → go RIGHT

    6 == 6
    → FOUND

So return the node `6`.

---

# 1. BST Foundation

BST = Binary Search Tree.

BST is a Binary Tree with an extra ordering rule.

For every node:

    LEFT SUBTREE < ROOT < RIGHT SUBTREE

Example:

        8
       / \
      3   10
     / \
    1   6
       / \
      4   7

For root `8`:

Left side:

    1, 3, 4, 6, 7

All are smaller than `8`.

Right side:

    10

All are greater than `8`.

The same rule also applies to every subtree.

---

# 2. Why BST Search Is Useful

In a normal Binary Tree, if we want to find `6`,
we may have to search both left and right.

But in a BST, the value itself tells us where to go.

At node `8`:

    Target = 6

Since:

    6 < 8

we know the target can only be in the LEFT subtree.

We can completely ignore the RIGHT subtree.

Then at `3`:

    6 > 3

So go RIGHT.

Then at `6`:

    6 == 6

Found.

---

# 3. Pattern

Pattern:

    Search in BST

Main property:

    LEFT < ROOT < RIGHT

Search rule:

    target < root->val
        → go LEFT

    target > root->val
        → go RIGHT

    target == root->val
        → FOUND

---

# 4. Approach

At every current node:

### Case 1 — Root is NULL

There is no node left to search.

Return:

    NULL

### Case 2 — Target Found

If:

    root->val == val

return:

    root

### Case 3 — Target Is Smaller

If:

    val < root->val

Because this is a BST,
target can only be in the left subtree.

So:

    searchBST(root->left, val)

### Case 4 — Target Is Larger

If:

    val > root->val

Target can only be in the right subtree.

So:

    searchBST(root->right, val)

---

# 5. Pseudocode

    searchBST(root, val):

        if root == NULL:
            return NULL

        if root->val == val:
            return root

        if val < root->val:
            return searchBST(root->left, val)

        return searchBST(root->right, val)

---

# 6. Code

class Solution {
public:

    TreeNode* searchBST(TreeNode* root, int val) {

        if (root == NULL)
            return NULL;

        if (root->val == val)
            return root;

        if (val < root->val)
            return searchBST(root->left, val);

        return searchBST(root->right, val);
    }
};

---

# 7. Code Explanation

## Step 1 — Function

    TreeNode* searchBST(TreeNode* root, int val)

Two things:

    root → current node
    val  → value we want to find

Return type is `TreeNode*`
because the question asks us to return the node/subtree if found.

---

## Step 2 — Base Case

    if (root == NULL)
        return NULL;

Agar current node NULL hai,
matlab search karne ke liye kuch nahi bacha.

So:

    return NULL

---

## Step 3 — Found the Value

    if (root->val == val)
        return root;

Agar current node ki value target ke equal hai,
target mil gaya.

Example:

    root->val = 6
    val = 6

Then:

    return root

Important:

Hum `root->val` return nahi kar rahe.

Hum:

    return root

kar rahe hain because we need the actual TreeNode.

---

## Step 4 — Target Smaller

    if (val < root->val)
        return searchBST(root->left, val);

Example:

    root = 8
    val = 6

Since:

    6 < 8

BST property tells us target LEFT mein hoga.

So:

    searchBST(root->left, val)

---

## Step 5 — Target Larger

    return searchBST(root->right, val);

Agar target smaller nahi hai,
and equal bhi nahi tha,
then target bigger hai.

Example:

    root = 3
    val = 6

Since:

    6 > 3

Go RIGHT.

---

# 8. Detailed Dry Run

BST:

        8
       / \
      3   10
     / \
    1   6
       / \
      4   7

Target:

    val = 6

Expected:

    return node 6

---

# Step 1

Call:

    searchBST(8, 6)

Current node:

    8

Check:

    8 == 6
    false

Next:

    6 < 8
    true

So:

    go LEFT

Call:

    searchBST(3, 6)

---

# Tree

        8
       / \
      3   10
     / \
    1   6
       / \
      4   7

Current node:

    3

---

# Step 2

Check:

    3 == 6
    false

Check:

    6 < 3
    false

So target is greater.

Go RIGHT:

    searchBST(6, 6)

---

# Tree

        8
       / \
      3   10
     / \
    1   6
       / \
      4   7

Current node:

    6

---

# Step 3

Check:

    6 == 6

True.

So:

    return root

We found the node.

Final returned node:

    6

---

# 9. Search Not Found

Same BST:

        8
       / \
      3   10
     / \
    1   6
       / \
      4   7

Target:

    val = 5

---

## Step 1

Current:

    8

    5 < 8

Go LEFT.

Current:

    3

---

## Step 2

    5 > 3

Go RIGHT.

Current:

    6

---

## Step 3

    5 < 6

Go LEFT.

Current:

    4

---

# Tree

        8
       / \
      3   10
     / \
    1   6
       / \
      4   7

Current:

    4

Target:

    5

Since:

    5 > 4

Go RIGHT.

But 4 ka right:

    NULL

So:

    searchBST(NULL, 5)

---

## Step 4

Root is NULL.

Therefore:

    return NULL

Final answer:

    NULL

---

# 10. Why Can We Ignore Half the Tree?

This is the main reason BST search works.

Example:

        8
       / \
      3   10
     / \
    1   6

Target = 6

At `8`:

    6 < 8

Everything in the RIGHT subtree is greater than `8`.

Therefore target `6` cannot be there.

So we ignore:

    10

and everything under it.

Only search:

    LEFT subtree

Similarly at `3`:

    6 > 3

Everything in the LEFT subtree of `3` is smaller than `3`.

So we ignore:

    1

and move RIGHT.

---

# 11. Recursion Flow

For target `6`:

    searchBST(8,6)
        ↓
    6 < 8
        ↓
    searchBST(3,6)
        ↓
    6 > 3
        ↓
    searchBST(6,6)
        ↓
    6 == 6
        ↓
    return 6

Search path:

    8 → 3 → 6

---

# 12. Important Difference: Binary Tree vs BST

## Binary Tree

Example:

        8
       / \
      10  3

This is still a Binary Tree.

But it is NOT a valid BST.

Why?

Left child:

    10 > 8

Right child:

    3 < 8

BST rule is broken.

---

## BST

        8
       / \
      3   10

Now:

    3 < 8
    10 > 8

Correct.

---

# 13. BST Search vs Normal Binary Tree Search

## Normal Binary Tree

Target may be anywhere.

We may need:

    search LEFT
    +
    search RIGHT

## BST

Value tells us the direction:

    smaller → LEFT

    bigger → RIGHT

    equal → FOUND

This is the major advantage.

---

# 14. Iterative Version

The same problem can also be solved without recursion:

    TreeNode* searchBST(TreeNode* root, int val) {

        while (root != NULL) {

            if (root->val == val)
                return root;

            if (val < root->val)
                root = root->left;
            else
                root = root->right;
        }

        return NULL;
    }

Recursive version is useful for learning Tree recursion,
while iterative version avoids recursive stack usage.

For this roadmap, focus first on the recursive version.

---

# 15. Complexity

At each step, we move only to one child.

### Time Complexity

    O(H)

where H = height of the BST.

Balanced BST:

    O(log N)

Worst case / skewed BST:

    O(N)

### Space Complexity

Recursive version:

    O(H)

because of recursion stack.

Balanced tree:

    O(log N)

Worst case:

    O(N)

---

# 16. Common Mistakes

## Mistake 1 — Search both sides

Wrong approach:

    search left
    search right

This ignores the main BST property.

Correct:

    target < root
        → LEFT

    target > root
        → RIGHT

---

## Mistake 2 — Returning value instead of node

Wrong:

    return root->val;

Correct:

    return root;

Because the function returns:

    TreeNode*

---

## Mistake 3 — Forgetting NULL case

Correct:

    if (root == NULL)
        return NULL;

---

## Mistake 4 — Using wrong direction

If:

    val < root->val

go LEFT.

If:

    val > root->val

go RIGHT.

Remember:

    Smaller → LEFT
    Bigger  → RIGHT

---

# 17. Pattern Recognition

If the question says:

    Search in BST
    Find a value in BST
    Find node with a given value

Immediately think:

    Compare target with current node.

    Smaller → LEFT
    Bigger  → RIGHT
    Equal   → FOUND

---

# 18. Important BST Rules to Remember

    LEFT < ROOT < RIGHT

    Smaller → LEFT

    Bigger → RIGHT

    Equal → Found

Also:

    Inorder traversal of BST = Sorted Order

Example:

        8
       / \
      3   10
     / \
    1   6
       / \
      4   7

Inorder:

    1 3 4 6 7 8 10

This concept will be used later in:

    LC 230 — Kth Smallest Element in a BST

---

# 19. One-Line Memory Trick

> BST Search = Compare target with root and choose only one side.

    target < root → LEFT

    target > root → RIGHT

    target == root → FOUND

---

# Final Code

class Solution {
public:

    TreeNode* searchBST(TreeNode* root, int val) {

        if (root == NULL)
            return NULL;

        if (root->val == val)
            return root;

        if (val < root->val)
            return searchBST(root->left, val);

        return searchBST(root->right, val);
    }
};

---

# Filename

700-search-in-a-binary-search-tree.md
