# LC 701 — Insert into a Binary Search Tree

## Problem

Given the root of a Binary Search Tree (BST) and an integer `val`,
insert `val` into the BST and return the root of the updated tree.

The BST property must remain valid after insertion.

BST rule:

    LEFT < ROOT < RIGHT

Example:

        8
       / \
      3   10
     / \
    1   6

Insert:

    val = 5

After insertion:

        8
       / \
      3   10
     / \
    1   6
       /
      5

---

# 1. Simple Understanding

BST mein har value ki position already decide hoti hai.

Rule:

    val < root->val
        → LEFT

    val > root->val
        → RIGHT

Jab NULL position mil jaaye:

    → new node create karo

Example:

        8
       / \
      3   10
     / \
    1   6

Insert `5`.

Start at `8`:

    5 < 8
    → LEFT

At `3`:

    5 > 3
    → RIGHT

At `6`:

    5 < 6
    → LEFT

At `6` ka LEFT:

    NULL

So yahin:

    new TreeNode(5)

insert hoga.

---

# 2. Pattern

Pattern:

    Search / BST

Concepts:

    BST property
    Recursion
    Pointer connection
    Tree insertion

Main rule:

    Smaller → LEFT
    Bigger  → RIGHT
    NULL    → Insert here

---

# 3. Approach

Har current node par:

### Case 1 — Current node NULL

Correct insertion position mil gayi.

Create:

    new TreeNode(val)

and return it.

### Case 2 — Value smaller

    val < root->val

Go LEFT:

    root->left = insertIntoBST(root->left, val)

### Case 3 — Value bigger

    val > root->val

Go RIGHT:

    root->right = insertIntoBST(root->right, val)

### Finally

    return root

Current subtree ka root wapas return karna hai.

---

# 4. Most Important Concept

Ye line:

    root->left = insertIntoBST(root->left, val);

ka matlab:

> Left subtree mein value insert karo aur jo updated subtree return ho, usko current node ke left pointer se attach karo.

Same thing on right:

    root->right = insertIntoBST(root->right, val);

---

# 5. Pseudocode

    insert(root, val):

        if root == NULL:
            return new TreeNode(val)

        if val < root->val:
            root->left = insert(root->left, val)

        else:
            root->right = insert(root->right, val)

        return root

---

# 6. Code

class Solution {
public:

    TreeNode* insertIntoBST(TreeNode* root, int val) {

        if (root == NULL)
            return new TreeNode(val);

        if (val < root->val) {
            root->left = insertIntoBST(root->left, val);
        }
        else {
            root->right = insertIntoBST(root->right, val);
        }

        return root;
    }
};

---

# 7. Code Explanation

## Function

    TreeNode* insertIntoBST(TreeNode* root, int val)

Parameters:

    root → current node
    val  → value to insert

Return:

    updated subtree root

---

## Step 1 — NULL Check

    if (root == NULL)
        return new TreeNode(val);

Agar current root NULL hai,
matlab correct insertion position mil gayi.

Example:

    root->left = NULL

Then:

    insertIntoBST(NULL, 5)

creates:

    5

and returns that new node.

---

## Step 2 — Value Smaller

    if (val < root->val)

BST rule ke according smaller value LEFT mein jayegi.

So:

    root->left = insertIntoBST(root->left, val);

Important:

Sirf recursive call nahi karni.

Returned node ko:

    root->left

mein attach karna hai.

---

## Step 3 — Value Bigger

    else

Agar value smaller nahi hai,
toh right side mein jayegi.

    root->right = insertIntoBST(root->right, val);

---

## Step 4 — Return Root

    return root;

Current subtree ka root unchanged rahega,
except uske left/right pointers update ho sakte hain.

Parent ko updated subtree return karna zaroori hai.

---

# 8. Why `root->left = ...` Is Needed

Example:

        8
       / \
      3   10

Insert:

    2

At node `8`:

    2 < 8

So:

    root->left = insertIntoBST(root->left, 2);

Here:

    root->left = 3

So call becomes:

    insertIntoBST(3, 2)

At node `3`:

    2 < 3

Again:

    root->left = insertIntoBST(root->left, 2);

But `3` ka left NULL hai.

So:

    insertIntoBST(NULL, 2)

returns:

    new node 2

Now assignment happens:

    3->left = new node 2

So tree becomes:

        8
       / \
      3   10
     /
    2

Without the assignment,
new node return toh hoti,
but tree ke andar attach nahi hoti.

---

# 9. Detailed Dry Run

BST:

        8
       / \
      3   10
     / \
    1   6

Insert:

    val = 5

Expected:

        8
       / \
      3   10
     / \
    1   6
       /
      5

---

## Step 1 — Root 8

Current:

    root = 8
    val = 5

Check:

    5 < 8

True.

So:

    root->left = insertIntoBST(root->left, 5)

`root->left = 3`

So next call:

    insertIntoBST(3, 5)

---

# Tree

        8
       / \
      3   10
     / \
    1   6

Current node:

    3

---

## Step 2 — Node 3

Check:

    5 < 3

False.

So:

    5 > 3

Go RIGHT:

    root->right = insertIntoBST(root->right, 5)

`3` ka right = `6`.

Next call:

    insertIntoBST(6, 5)

---

# Tree

        8
       / \
      3   10
     / \
    1   6

Current node:

    6

---

## Step 3 — Node 6

Check:

    5 < 6

True.

Go LEFT:

    root->left = insertIntoBST(root->left, 5)

`6` ka left = NULL.

So:

    insertIntoBST(NULL, 5)

---

# Tree

        8
       / \
      3   10
     / \
    1   6

Current:

    root = NULL

---

## Step 4 — NULL Position

Condition:

    root == NULL

True.

So:

    return new TreeNode(5)

A new node is created:

        5

---

# Step 5 — Back to Node 6

Before recursive call:

    6->left = NULL

Recursive call returned:

    new node 5

So assignment:

    6->left = 5

Tree becomes:

        8
       / \
      3   10
     / \
    1   6
       /
      5

Then:

    return 6

---

# Step 6 — Back to Node 3

Node 3 ke right par recursive insertion hua.

Returned subtree root:

    6

So:

    3->right = 6

Then:

    return 3

---

# Step 7 — Back to Node 8

Node 8 ke left par recursive insertion hua.

Returned subtree root:

    3

So:

    8->left = 3

Then:

    return 8

---

# Final Tree

        8
       / \
      3   10
     / \
    1   6
       /
      5

Inserted node:

    5

---

# 10. Recursion Flow

Insert `5`:

    insert(8,5)
        ↓
    5 < 8
        ↓
    insert(3,5)
        ↓
    5 > 3
        ↓
    insert(6,5)
        ↓
    5 < 6
        ↓
    insert(NULL,5)
        ↓
    create node 5
        ↓
    return 5
        ↓
    6->left = 5
        ↓
    return 6
        ↓
    3->right = 6
        ↓
    return 3
        ↓
    8->left = 3
        ↓
    return 8

---

# 11. Important Pointer Concept

Suppose:

    root = 6

and:

    root->left = NULL

We call:

    insertIntoBST(root->left, 5)

which means:

    insertIntoBST(NULL, 5)

This creates and returns node `5`.

But parent node `6` ko us new node se connect karna padega.

That is why:

    root->left = insertIntoBST(root->left, 5);

After the call:

    6->left = 5

This is the key recursion + pointer connection concept.

---

# 12. Why Return `root`?

Suppose current subtree:

        6
       /
      5

Its root is still:

    6

So after insertion:

    return root

returns the root of the updated subtree.

Parent can then connect that updated subtree.

Example:

    3->right = updated subtree rooted at 6

This allows changes to travel back up the recursion.

---

# 13. BST Search vs BST Insert

## Search — LC 700

Goal:

    Find a node

Rule:

    smaller → LEFT
    bigger  → RIGHT
    equal   → FOUND

---

## Insert — LC 701

Goal:

    Find the correct NULL position

Rule:

    smaller → LEFT
    bigger  → RIGHT
    NULL    → CREATE NODE

Both use the same BST direction logic.

Difference:

    Search → return existing node
    Insert → create node at NULL

---

# 14. Common Mistakes

## Mistake 1 — Forgetting assignment

Wrong:

    insertIntoBST(root->left, val);

Correct:

    root->left = insertIntoBST(root->left, val);

Same for right.

---

## Mistake 2 — Forgetting NULL case

Correct:

    if (root == NULL)
        return new TreeNode(val);

This is where insertion actually happens.

---

## Mistake 3 — Not returning root

Correct:

    return root;

Without it,
the updated subtree cannot be properly passed back to the parent.

---

## Mistake 4 — Wrong direction

Remember:

    val < root->val → LEFT

    val > root->val → RIGHT

---

## Mistake 5 — Inserting at the first smaller/larger node

Example:

        8
       /
      3

Insert 5.

You cannot directly put 5 as a child of 3 without checking its right side.

You must follow:

    5 > 3
    → RIGHT

And continue until NULL.

---

# 15. Complexity

We follow only one path from root to insertion position.

### Time Complexity

    O(H)

where H = height of BST.

Balanced BST:

    O(log N)

Worst-case skewed BST:

    O(N)

### Space Complexity

Recursive call stack:

    O(H)

Balanced:

    O(log N)

Worst case:

    O(N)

---

# 16. Pattern Recognition

If interview asks:

    Insert a value into BST

Immediately think:

    Compare with current node.

    Smaller → LEFT
    Bigger  → RIGHT
    NULL    → new node

And recursively attach the returned subtree:

    root->left = insert(...)
    root->right = insert(...)

---

# 17. One-Line Memory Trick

> BST insertion = Correct direction choose karo, NULL tak jao, new node banao, aur recursion ke through us node ko parent se connect karo.

    Smaller → LEFT
    Bigger  → RIGHT
    NULL → INSERT
