# LC 235 — Lowest Common Ancestor of a Binary Search Tree

## Problem

Given the root of a Binary Search Tree (BST) and two nodes `p` and `q`,
find their Lowest Common Ancestor (LCA).

LCA = Lowest Common Ancestor.

The LCA is the lowest node in the tree that has both `p` and `q`
as descendants. A node can also be a descendant of itself.

Example:

          6
        /   \
       2     8
      / \   / \
     0   4 7   9
        / \
       3   5

p = 2
q = 8

LCA = 6

---

# 1. Simple Understanding

BST ki important property:

    LEFT < ROOT < RIGHT

Because of this property, humein LCA search karne ke liye
poora tree traverse nahi karna padta.

Current node ko root maan lo.

### Case 1 — p aur q dono chhote hain

    p < root
    q < root

Dono LEFT subtree mein hain.

So:

    go LEFT

### Case 2 — p aur q dono bade hain

    p > root
    q > root

Dono RIGHT subtree mein hain.

So:

    go RIGHT

### Case 3 — Ek left aur ek right

Example:

    p < root < q

Ya:

    q < root < p

Dono alag sides mein hain.

So current root hi LCA hai.

### Case 4 — Current root khud p ya q hai

Example:

    root = 2
    p = 2
    q = 4

Then `2` hi LCA hai.

So current root return karenge.

---

# 2. Pattern

Pattern:

    Search

Concepts used:

    BST Property
    +
    LCA
    +
    Recursion

Main BST rule:

    Smaller → LEFT
    Bigger  → RIGHT
    Split   → Current root is LCA

---

# 3. Approach

At every current node:

    1. Check whether p and q are both smaller than root.
       If yes → move LEFT.

    2. Check whether p and q are both greater than root.
       If yes → move RIGHT.

    3. Otherwise, current root is LCA.

Why otherwise?

Because:

    One node is on LEFT
    and
    one node is on RIGHT

or:

    Current root itself is p or q.

In both cases current root is the lowest common ancestor.

---

# 4. Pseudocode

    LCA(root, p, q):

        if p < root AND q < root:
            return LCA(root->left, p, q)

        if p > root AND q > root:
            return LCA(root->right, p, q)

        return root

---

# 5. Code

class Solution {
public:

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {

        if (p->val < root->val && q->val < root->val) {
            return lowestCommonAncestor(root->left, p, q);
        }

        if (p->val > root->val && q->val > root->val) {
            return lowestCommonAncestor(root->right, p, q);
        }

        return root;
    }
};

---

# 6. Code Explanation

## Function

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q)

Parameters:

    root → current node
    p    → first target node
    q    → second target node

Return:

    LCA node

---

## Step 1 — Both on LEFT

    if (p->val < root->val && q->val < root->val)

Suppose:

    root = 8
    p = 2
    q = 4

Then:

    2 < 8
    4 < 8

Dono LEFT subtree mein hain.

So:

    return lowestCommonAncestor(root->left, p, q);

---

## Step 2 — Both on RIGHT

    if (p->val > root->val && q->val > root->val)

Suppose:

    root = 6
    p = 7
    q = 9

Then:

    7 > 6
    9 > 6

Dono RIGHT subtree mein hain.

So:

    return lowestCommonAncestor(root->right, p, q);

---

## Step 3 — Otherwise

    return root;

Agar first condition false hai
aur second condition bhi false hai,
then p and q are not both on the same side.

Example:

    p = 2
    q = 8
    root = 6

    2 < 6
    8 > 6

Ek LEFT mein hai aur ek RIGHT mein.

Therefore:

    6 = LCA

---

# 7. Detailed Dry Run

BST:

          6
        /   \
       2     8
      / \   / \
     0   4 7   9
        / \
       3   5

Take:

    p = 2
    q = 8

Expected:

    6

---

## Step 1

Current:

    root = 6
    p = 2
    q = 8

Check first:

    2 < 6 → true
    8 < 6 → false

First condition false.

Check second:

    2 > 6 → false
    8 > 6 → true

Second condition also false.

So:

    return root

Root = 6

Therefore:

    LCA = 6

---

# 8. Dry Run — Both on LEFT

Same BST:

          6
        /   \
       2     8
      / \   / \
     0   4 7   9
        / \
       3   5

Take:

    p = 2
    q = 4

---

## Step 1

Current root:

    6

Check:

    2 < 6 → true
    4 < 6 → true

Both are smaller.

So:

    go LEFT

Next call:

    LCA(2, 2, 4)

---

# Tree

          6
        /   \
       2     8
      / \   / \
     0   4 7   9
        / \
       3   5

Current:

    root = 2
    p = 2
    q = 4

---

## Step 2

First condition:

    2 < 2 → false

Second condition:

    2 > 2 → false

So:

    return root

Therefore:

    LCA = 2

Why?

Because `2` itself is an ancestor of `4`,
and it is also one of the target nodes.

---

# 9. Dry Run — Both on RIGHT

Take:

    p = 7
    q = 9

Same tree.

---

## Step 1

Current root:

    6

Check:

    7 > 6 → true
    9 > 6 → true

Both are greater.

So:

    go RIGHT

Next:

    LCA(8, 7, 9)

---

# Tree

          6
        /   \
       2     8
      / \   / \
     0   4 7   9
        / \
       3   5

Current root:

    8

p = 7

q = 9

Check:

    7 < 8 → true
    9 < 8 → false

First condition false.

Second:

    7 > 8 → false
    9 > 8 → true

Second false too.

So:

    return 8

Therefore:

    LCA = 8

---

# 10. Why BST Property Makes It Easy

Normal Binary Tree mein:

    LEFT < ROOT < RIGHT

ka rule nahi hota.

So LCA find karte waqt
left aur right dono explore karne pad sakte hain.

BST mein:

    Smaller → LEFT
    Bigger → RIGHT

So direction immediately decide ho jaati hai.

This is the main advantage.

---

# 11. Recursion Flow

For:

    p = 2
    q = 4

Flow:

    LCA(6,2,4)
        ↓
    2 < 6
    4 < 6
        ↓
    LEFT
        ↓
    LCA(2,2,4)
        ↓
    split / current node = p
        ↓
    return 2

Final:

    2

---

# 12. Same Tree vs Symmetric Tree vs BST LCA

## Same Tree — LC 100

Two complete trees compare karte hain.

    Tree 1 ↔ Tree 2

Same positions:

    left ↔ left
    right ↔ right

---

## Symmetric Tree — LC 101

One tree ke left and right ko mirror compare karte hain.

    left.left ↔ right.right
    left.right ↔ right.left

---

## LCA of BST — LC 235

Two target nodes diye hain.

BST property use karke decide karte hain:

    both smaller → LEFT

    both larger → RIGHT

    otherwise → ROOT

---

# 13. Common Mistakes

## Mistake 1 — Dono targets same side mein check na karna

Important:

    p < root
    AND
    q < root

Dono true hone chahiye.

Similarly:

    p > root
    AND
    q > root

---

## Mistake 2 — Split ko alag complicated case banana

Split case ke liye separate condition ki zarurat nahi.

Agar:

    both left = false
    both right = false

Then:

    current root = LCA

So simply:

    return root;

---

## Mistake 3 — BST property ignore karke full DFS karna

Possible hai but unnecessary.

BST mein direction directly decide ho sakti hai.

---

## Mistake 4 — root->val ko return karna

Wrong:

    return root->val;

Correct:

    return root;

Question actual TreeNode return karne ko keh raha hai.

---

# 14. Complexity

At every step we move to only one subtree.

Time Complexity:

    O(H)

where H = height of BST.

Balanced BST:

    O(log N)

Worst-case skewed BST:

    O(N)

Space Complexity for recursive solution:

    O(H)

because of recursion stack.

Balanced:

    O(log N)

Worst case:

    O(N)

---

# 15. Iterative Version

Same logic without recursion:

    class Solution {
    public:

        TreeNode* lowestCommonAncestor(TreeNode* root,
                                       TreeNode* p,
                                       TreeNode* q) {

            while (root != NULL) {

                if (p->val < root->val && q->val < root->val) {
                    root = root->left;
                }
                else if (p->val > root->val && q->val > root->val) {
                    root = root->right;
                }
                else {
                    return root;
                }
            }

            return NULL;
        }
    };

For learning Tree recursion,
recursive version is important.

---

# 16. Pattern Recognition

If interview asks:

    Lowest Common Ancestor in a BST

Immediately think:

    Compare p and q with current root.

    both smaller → LEFT

    both larger  → RIGHT

    otherwise    → current root

---

# 17. One-Line Memory Trick

> BST LCA = Dono ek side mein hain toh us side jao; split hote hi current root LCA hai.

    p < root AND q < root → LEFT

    p > root AND q > root → RIGHT

    Otherwise              → ROOT
