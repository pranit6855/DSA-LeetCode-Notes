# LC 100 — Same Tree

## Problem

Given the roots of two binary trees `p` and `q`, check whether the two trees are the same or not.

Two trees are considered the same when:

1. Their structure is exactly the same.
2. Corresponding nodes have the same values.

---

# 1. Simple Understanding

Example:

Tree 1:

        1
       / \
      2   3

Tree 2:

        1
       / \
      2   3

Both structure and values are same.

Answer:

    true

---

## Different Value

Tree 1:

        1
       / \
      2   3

Tree 2:

        1
       / \
      2   4

Here:

    3 != 4

So:

    false

---

## Different Structure

Tree 1:

        1
       /
      2

Tree 2:

        1
         \
          2

Values are same, but structure is different.

So:

    false

---

# 2. Pattern

Pattern:

    Mirror & Symmetry

But unlike Symmetric Tree, here we compare:

    Tree 1 LEFT  ↔ Tree 2 LEFT
    Tree 1 RIGHT ↔ Tree 2 RIGHT

So this is a direct same-position comparison.

---

# 3. Approach

Use recursion and compare both trees simultaneously.

For every pair of nodes:

### Case 1 — Both NULL

    p = NULL
    q = NULL

Both positions are empty.

Return:

    true

### Case 2 — One NULL

    p = NULL
    q = node

or:

    p = node
    q = NULL

Structure is different.

Return:

    false

### Case 3 — Values Different

    p->val != q->val

Return:

    false

### Case 4 — Values Same

Now compare their children:

    p->left  ↔ q->left
    p->right ↔ q->right

Both comparisons must be true.

---

# 4. Pseudocode

    isSameTree(p, q):

        if both NULL:
            return true

        if one is NULL:
            return false

        if p->val != q->val:
            return false

        return
            isSameTree(p->left, q->left)
            &&
            isSameTree(p->right, q->right)

---

# 5. Code

class Solution {
public:

    bool isSameTree(TreeNode* p, TreeNode* q) {

        if (p == NULL && q == NULL)
            return true;

        if (p == NULL || q == NULL)
            return false;

        if (p->val != q->val)
            return false;

        return isSameTree(p->left, q->left) &&
               isSameTree(p->right, q->right);
    }
};

---

# 6. Code Explanation

## Step 1 — Both NULL

    if (p == NULL && q == NULL)
        return true;

Agar dono nodes NULL hain,
toh dono trees mein current position par same structure hai.

Example:

    NULL ↔ NULL

So:

    true

---

## Step 2 — One NULL

    if (p == NULL || q == NULL)
        return false;

Agar ek node hai aur doosri side NULL:

    Node ↔ NULL

Toh structure same nahi hai.

So:

    false

---

## Step 3 — Value Comparison

    if (p->val != q->val)
        return false;

Corresponding nodes ki values same honi chahiye.

Example:

    p = 3
    q = 4

Then:

    3 != 4

So:

    false

---

## Step 4 — Left and Right Comparison

    return isSameTree(p->left, q->left) &&
           isSameTree(p->right, q->right);

Values same hone ke baad
dono trees ke corresponding child subtrees compare karte hain.

Important:

    p->left  ↔ q->left

    p->right ↔ q->right

Dono true hone chahiye.

Isliye:

    &&

use kiya hai.

---

# 7. Main Idea

Har recursive call mein do current nodes compare ho rahi hain.

    p = Tree 1 ka current node
    q = Tree 2 ka current node

Aur har pair par:

    NULL check
    value check
    left check
    right check

Same process recursively repeat hota hai.

---

# 8. Detailed Dry Run

Tree 1:

        1
       / \
      2   3

Tree 2:

        1
       / \
      2   3

Expected:

    true

---

## Step 1

Call:

    isSameTree(1, 1)

Both present.

Values:

    1 == 1

So continue.

Now compare left:

    isSameTree(2, 2)

---

## Tree

Tree 1:

        1
       / \
      2   3

Tree 2:

        1
       / \
      2   3

---

## Step 2 — Node 2

Current pair:

    2 ↔ 2

Values:

    2 == 2

So continue.

Compare left:

    NULL ↔ NULL

Both NULL:

    true

Compare right:

    NULL ↔ NULL

Both NULL:

    true

Therefore:

    isSameTree(2,2) = true

---

## Back to Node 1

Left subtree comparison:

    true

Now compare right:

    isSameTree(3,3)

---

## Step 3 — Node 3

Current pair:

    3 ↔ 3

Values:

    3 == 3

Left:

    NULL ↔ NULL

    true

Right:

    NULL ↔ NULL

    true

Therefore:

    isSameTree(3,3) = true

---

## Step 4 — Final

For root `1`:

    left comparison  = true
    right comparison = true

Therefore:

    true && true

So:

    isSameTree(1,1) = true

Final answer:

    true

---

# 9. Different Value Dry Run

Tree 1:

        1
       / \
      2   3

Tree 2:

        1
       / \
      2   4

Start:

    isSameTree(1,1)

Values:

    1 == 1

Compare left:

    isSameTree(2,2)

Values:

    2 == 2

Left:

    NULL ↔ NULL
    true

Right:

    NULL ↔ NULL
    true

So:

    true

Now right:

    isSameTree(3,4)

Values:

    3 != 4

Immediately:

    false

Therefore:

    true && false = false

Final:

    false

---

# 10. Different Structure Dry Run

Tree 1:

        1
       /
      2

Tree 2:

        1
         \
          2

Start:

    isSameTree(1,1)

Values:

    1 == 1

Compare left:

    Tree 1 left = 2
    Tree 2 left = NULL

So:

    isSameTree(2,NULL)

One is NULL.

Therefore:

    false

No need to continue.

Final:

    false

---

# 11. Same Tree vs Symmetric Tree

## Same Tree — LC 100

Corresponding positions compare:

    p->left  ↔ q->left

    p->right ↔ q->right

Example:

    Tree 1             Tree 2

       1                   1
      / \                 / \
     2   3               2   3

Compare:

    2 ↔ 2
    3 ↔ 3

---

## Symmetric Tree — LC 101

Mirror positions compare:

    left.left  ↔ right.right

    left.right ↔ right.left

Example:

        1
       / \
      2   2
     / \ / \
    3  4 4  3

Compare:

    3 ↔ 3
    4 ↔ 4

---

# 12. Important Difference

Remember:

    SAME TREE
    same position

    p->left  ↔ q->left
    p->right ↔ q->right

While:

    SYMMETRIC TREE
    mirror position

    left.left  ↔ right.right
    left.right ↔ right.left

---

# 13. Recursion Flow

For:

        1
       / \
      2   3

and:

        1
       / \
      2   3

Flow:

    isSameTree(1,1)
        ↓
    isSameTree(2,2)
        ↓
    NULL ↔ NULL
        ↓
    NULL ↔ NULL
        ↓
    true
        ↓
    isSameTree(3,3)
        ↓
    NULL ↔ NULL
        ↓
    NULL ↔ NULL
        ↓
    true
        ↓
    true

Final:

    true

---

# 14. Why && Is Used

Code:

    return isSameTree(p->left, q->left) &&
           isSameTree(p->right, q->right);

Dono sides same honi chahiye.

Example:

    left  = true
    right = false

Then:

    true && false = false

So entire tree same nahi hai.

---

# 15. Common Mistakes

## Mistake 1 — Only compare values

Wrong idea:

    p->val == q->val

Sirf values same hone se tree same nahi hota.

Structure bhi same honi chahiye.

---

## Mistake 2 — Wrong child comparison

Same Tree mein:

    p->left  ↔ q->left
    p->right ↔ q->right

Mirror comparison nahi karna.

---

## Mistake 3 — NULL check skip karna

Correct:

    if (p == NULL && q == NULL)
        return true;

    if (p == NULL || q == NULL)
        return false;

---

## Mistake 4 — Recursive result return na karna

Wrong:

    isSameTree(p->left, q->left);
    isSameTree(p->right, q->right);

Correct:

    return isSameTree(p->left, q->left) &&
           isSameTree(p->right, q->right);

---

# 16. Complexity

Har corresponding node pair ko maximum ek baar compare karte hain.

Time Complexity:

    O(N)

where N is the number of nodes.

Space Complexity:

    O(H)

where H is the height of the tree because of recursion stack.

Worst case:

    O(N)

Balanced tree:

    O(log N)

---

# 17. Pattern Recognition

Agar question bole:

    Check if two binary trees are same

Immediately think:

    Compare two trees recursively

At every pair:

    both NULL → true
    one NULL → false
    values different → false
    otherwise:
        left ↔ left
        right ↔ right

---

# 18. One-Line Memory Trick

> Same Tree = Same values + Same structure + Same positions.

    p.left  ↔ q.left

    p.right ↔ q.right

---

# Final Code

class Solution {
public:

    bool isSameTree(TreeNode* p, TreeNode* q) {

        if (p == NULL && q == NULL)
            return true;

        if (p == NULL || q == NULL)
            return false;

        if (p->val != q->val)
            return false;

        return isSameTree(p->left, q->left) &&
               isSameTree(p->right, q->right);
    }
};

---

# Filename

100-same-tree.md
