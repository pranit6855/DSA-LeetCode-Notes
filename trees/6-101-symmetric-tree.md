# LC 101 — Symmetric Tree

## Problem

Given the root of a binary tree, check whether the tree is symmetric around its center.

Symmetric ka matlab:
Left aur Right subtree ek-doosre ki mirror image honi chahiye.

Example:

        1
       / \
      2   2
     / \ / \
    3  4 4  3

Output:

    true

Non-symmetric example:

        1
       / \
      2   2
       \   \
        3   3

Output:

    false

---

# 1. Simple Understanding

Humein root ke left aur right subtree ko compare karna hai.

Normal comparison:

    left.left  ↔ right.left
    left.right ↔ right.right

Lekin Symmetric Tree mein mirror comparison chahiye:

    left.left  ↔ right.right
    left.right ↔ right.left

Example:

        1
       / \
      2   2
     / \ / \
    3  4 4  3

Comparison:

    2 ↔ 2
    3 ↔ 3
    4 ↔ 4

Structure bhi mirror hai.

Therefore:

    true

---

# 2. Pattern

Pattern:

    Mirror / Symmetry

Main idea:

    Left subtree ↔ Right subtree

    in MIRROR order

Ye recursion se solve hoga.

---

# 3. Approach

Ek helper function banayenge:

    isMirror(left, right)

Ye do nodes/subtrees ko mirror way mein compare karega.

Har pair ke liye 4 checks:

### Case 1 — Dono NULL

    NULL ↔ NULL

Dono jagah kuch nahi hai.

    return true

### Case 2 — Ek NULL

    NULL ↔ Node

Structure same nahi ho sakta.

    return false

### Case 3 — Values different

    left->val != right->val

Mirror nodes ki values same honi chahiye.

    return false

### Case 4 — Values same

Ab children ko mirror order mein compare karo:

    left.left  ↔ right.right

    left.right ↔ right.left

Dono comparisons true hone chahiye.

---

# 4. Pseudo Code

    isMirror(left, right):

        if both NULL:
            return true

        if one is NULL:
            return false

        if values are different:
            return false

        return
            isMirror(left.left, right.right)
            &&
            isMirror(left.right, right.left)

---

# 5. Code

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

---

# 6. Code Explanation

## Helper Function

    bool ismirror(TreeNode* left, TreeNode* right)

Iska kaam hai check karna:

    Kya left aur right subtrees mirror hain?

left:

    Left side ka current node

right:

    Right side ka current node

---

## Step 1 — Both NULL

    if (left == NULL && right == NULL)
        return true;

Agar dono NULL hain toh dono sides ka structure yahan tak same hai.

Example:

    NULL ↔ NULL

So:

    true

---

## Step 2 — One NULL

    if (left == NULL || right == NULL)
        return false;

Agar ek side node hai aur doosri side NULL hai:

    Node ↔ NULL

Toh symmetry break ho gayi.

So:

    false

---

## Step 3 — Values Check

    if (left->val != right->val)
        return false;

Mirror nodes ki values same honi chahiye.

Example:

    2 ↔ 3

Different values:

    false

---

## Step 4 — Mirror Children

    return ismirror(left->left, right->right) &&
           ismirror(left->right, right->left);

Ye sabse important line hai.

Comparison:

    left.left  ↔ right.right

    left.right ↔ right.left

Dono true hone chahiye.

Isliye:

    &&

use kiya hai.

---

# 7. Main Function

    bool isSymmetric(TreeNode* root)

Agar root NULL hai:

    if (root == NULL)
        return true;

Empty tree symmetric maana jaata hai.

Otherwise:

    return ismirror(root->left, root->right);

Root ke left aur right ko mirror way mein compare karna hai.

---

# 8. Detailed Dry Run

Tree:

        1
       / \
      2   2
     / \ / \
    3  4 4  3

Expected:

    true

---

## Step 1

Call:

    isSymmetric(1)

Root NULL nahi hai.

So:

    ismirror(root->left, root->right)

Means:

    ismirror(2, 2)

---

# Tree

        1
       / \
      2   2
     / \ / \
    3  4 4  3

Current:

    left = 2
    right = 2

Both present.

Check values:

    2 == 2

So continue.

---

## Step 2 — First Mirror Pair

Code:

    ismirror(left->left, right->right)

Left 2 ka left:

    3

Right 2 ka right:

    3

So:

    ismirror(3, 3)

Values:

    3 == 3

Now children:

    NULL ↔ NULL
    NULL ↔ NULL

Both true.

Therefore:

    ismirror(3,3) = true

---

# Tree

        1
       / \
      2   2
     / \ / \
    3  4 4  3

---

## Step 3 — Second Mirror Pair

Code:

    ismirror(left->right, right->left)

Left 2 ka right:

    4

Right 2 ka left:

    4

So:

    ismirror(4,4)

Values:

    4 == 4

Children:

    NULL ↔ NULL
    NULL ↔ NULL

So:

    true

---

## Step 4 — Back to 2,2

Ab dono comparisons true:

    left.left  ↔ right.right = true

    left.right ↔ right.left = true

Therefore:

    true && true

So:

    ismirror(2,2) = true

---

# Tree

        1
       / \
      2   2
     / \ / \
    3  4 4  3

---

## Step 5 — Final Result

Main function:

    return ismirror(root->left, root->right);

Returned:

    true

Final output:

    true

---

# 9. Non-Symmetric Dry Run

Tree:

        1
       / \
      2   2
       \   \
        3   3

Call:

    ismirror(2,2)

Values same:

    2 == 2

Now mirror comparison:

    left.left  ↔ right.right

That becomes:

    NULL ↔ 3

One is NULL.

So:

    return false

Final:

    false

---

# 10. Most Important Mirror Logic

Suppose:

        1
       / \
      2   2
     / \ / \
    3  4 4  3

For symmetry:

    left.left  ↔ right.right

    left.right ↔ right.left

Not:

    left.left  ↔ right.left

    left.right ↔ right.right

Because humein MIRROR compare karna hai.

---

# 11. Invert Tree vs Symmetric Tree

## LC 226 — Invert Binary Tree

Goal:

    Tree ko modify karna

Operation:

    LEFT ↔ RIGHT

Example:

        1
       / \
      2   3

becomes:

        1
       / \
      3   2

---

## LC 101 — Symmetric Tree

Goal:

    Tree ko modify nahi karna

Bas check karna hai:

    Kya left aur right mirror hain?

Comparison:

    left.left  ↔ right.right

    left.right ↔ right.left

---

# 12. Recursion Flow

For:

        1
       / \
      2   2
     / \ / \
    3  4 4  3

Flow:

    ismirror(2,2)
        ↓
    ismirror(3,3)
        ↓
    true
        ↓
    ismirror(4,4)
        ↓
    true
        ↓
    true

Final:

    true

---

# 13. Why AND (&&)?

Code:

    return ismirror(left->left, right->right) &&
           ismirror(left->right, right->left);

Dono sides mirror honi chahiye.

Example:

    First comparison = true
    Second comparison = false

Then:

    true && false = false

So tree symmetric nahi hai.

---

# 14. Common Mistakes

## Mistake 1 — Wrong child comparison

Wrong:

    ismirror(left->left, right->left)

    ismirror(left->right, right->right)

Correct:

    ismirror(left->left, right->right)

    ismirror(left->right, right->left)

---

## Mistake 2 — Helper ka result return nahi karna

Wrong:

    ismirror(root->left, root->right);

Correct:

    return ismirror(root->left, root->right);

---

## Mistake 3 — One NULL case ignore karna

Wrong idea:

    NULL ↔ Node

ko valid maan lena.

Correct:

    if (left == NULL || right == NULL)
        return false;

---

## Mistake 4 — Sirf values compare karna

Sirf values same hona enough nahi hai.

Example:

        1
       / \
      2   2
     /     \
    3       3

Values mirror-looking hain, lekin structure check karna bhi zaroori hai.

---

# 15. Complexity

Har node ko maximum ek baar process karte hain.

Time Complexity:

    O(N)

N = number of nodes.

Space Complexity:

    O(H)

H = height of tree because of recursion stack.

Worst case:

    O(N)

Balanced tree:

    O(log N)

---

# 16. Pattern Recognition

Agar question mein ye words aaye:

    Symmetric Tree
    Mirror Tree
    Mirror Image
    Symmetric around center

Immediately think:

    Compare left and right in mirror order.

Core:

    left.left  ↔ right.right

    left.right ↔ right.left

---

# 17. One-Line Memory Trick

    SYMMETRIC = SAME VALUES + MIRROR STRUCTURE

Remember:

    LEFT.LEFT   ↔ RIGHT.RIGHT

    LEFT.RIGHT  ↔ RIGHT.LEFT

---

# Final Code

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

---

# Filename

101-symmetric-tree.md
