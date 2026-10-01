# LC 236 — Lowest Common Ancestor of a Binary Tree

## Problem

Given the root of a Binary Tree and two nodes `p` and `q`,
find their Lowest Common Ancestor (LCA).

LCA = Lowest Common Ancestor.

The LCA is the lowest node in the tree that has both `p` and `q`
as descendants.

Important:

A node can also be a descendant of itself.

---

# 1. Simple Understanding

Example:

          3
         / \
        5   1
       / \ / \
      6  2 0  8
        / \
       7   4

Suppose:

    p = 5
    q = 1

Both nodes are under `3`.

So:

    LCA = 3

Another example:

          3
         / \
        5   1
       / \
      6   2
         / \
        7   4

Suppose:

    p = 5
    q = 4

Here `4` is inside the subtree of `5`.

So:

    LCA = 5

Because `5` itself is one of the target nodes and is an ancestor of `4`.

---

# 2. Pattern

Pattern:

    Search

This is the Lowest Common Ancestor pattern for a normal Binary Tree.

Important:

This is NOT a BST problem.

So we cannot use:

    smaller → LEFT
    bigger → RIGHT

Instead, we may need to search both subtrees.

---

# 3. Main Idea

At every node, ask:

    Kya p ya q left subtree mein mila?
    Kya p ya q right subtree mein mila?

Recursion har subtree se ek result return karegi.

Possible results:

    NULL
    p
    q
    LCA

Main important case:

    left != NULL
    right != NULL

Matlab ek target LEFT subtree se mila
aur doosra RIGHT subtree se.

So:

    current root = LCA

---

# 4. Approach

For every current node:

## Case 1 — root is NULL

Agar current subtree empty hai:

    return NULL

---

## Case 2 — root itself is p or q

Agar:

    root == p

or:

    root == q

Toh current root ko return karo.

Why?

Because a node can be an ancestor of itself.

---

## Case 3 — Search Left

    left = LCA(root->left, p, q)

Left subtree se pata chalega ki wahan
p ya q ya LCA mila ya nahi.

---

## Case 4 — Search Right

    right = LCA(root->right, p, q)

Right subtree se bhi same information milegi.

---

## Case 5 — Both sides returned something

    left != NULL
    right != NULL

Matlab ek target left mein hai
aur doosra right mein.

Therefore:

    root = LCA

---

## Case 6 — Only Left returned something

    left != NULL
    right == NULL

Matlab useful result left subtree ke andar hai.

So:

    return left

---

## Case 7 — Only Right returned something

    left == NULL
    right != NULL

So:

    return right

---

# 5. Pseudocode

    LCA(root, p, q):

        if root == NULL:
            return NULL

        if root == p OR root == q:
            return root

        left = LCA(root->left, p, q)

        right = LCA(root->right, p, q)

        if left != NULL AND right != NULL:
            return root

        if left != NULL:
            return left

        return right

---

# 6. Code

class Solution {
public:

    TreeNode* lowestCommonAncestor(TreeNode* root,
                                   TreeNode* p,
                                   TreeNode* q) {

        if (root == NULL)
            return NULL;

        if (root == p || root == q)
            return root;

        TreeNode* left =
            lowestCommonAncestor(root->left, p, q);

        TreeNode* right =
            lowestCommonAncestor(root->right, p, q);

        if (left != NULL && right != NULL)
            return root;

        if (left != NULL)
            return left;

        return right;
    }
};

---

# 7. Code Explanation

## Function

    TreeNode* lowestCommonAncestor(TreeNode* root,
                                   TreeNode* p,
                                   TreeNode* q)

Parameters:

    root → current node
    p    → first target
    q    → second target

Return:

    LCA node

---

## Step 1 — NULL Check

    if (root == NULL)
        return NULL;

Agar current node exist nahi karti,
toh yahan p ya q nahi milega.

---

## Step 2 — Current Node is p or q

    if (root == p || root == q)
        return root;

Suppose:

    root = 5
    p = 5
    q = 4

Current node `p` hai.

So `5` ko return kar denge.

Important:

    Node can be ancestor of itself.

---

## Step 3 — Search LEFT

    TreeNode* left =
        lowestCommonAncestor(root->left, p, q);

Current node ke left subtree mein search karo.

Result:

    NULL

or:

    some target / ancestor

---

## Step 4 — Search RIGHT

    TreeNode* right =
        lowestCommonAncestor(root->right, p, q);

Right subtree ko bhi search karo.

Normal Binary Tree hone ki wajah se
dono sides explore karna pad sakta hai.

---

# 8. Most Important Line

    if (left != NULL && right != NULL)
        return root;

Iska matlab:

    left side se kuch mila
    +
    right side se kuch mila

Example:

          3
         / \
        5   1

Suppose:

    p = 5
    q = 1

Then:

    left = 5
    right = 1

So:

    left != NULL
    right != NULL

Therefore:

    root = 3

And:

    LCA = 3

---

# 9. Why Return left?

    if (left != NULL)
        return left;

Suppose:

    left = 5
    right = NULL

Matlab useful result sirf left subtree mein mila.

Current root LCA nahi hai.

So:

    return left

---

# 10. Why Return right?

    return right;

Agar:

    left = NULL
    right = 1

Toh useful result right subtree mein mila.

So:

    return right

Agar dono NULL hote:

    NULL

hi return hota.

---

# 11. Detailed Dry Run

Tree:

          3
         / \
        5   1
       / \ / \
      6  2 0  8
        / \
       7   4

Take:

    p = 5
    q = 1

Expected:

    LCA = 3

---

## Step 1 — Root 3

Current:

    root = 3
    p = 5
    q = 1

Check:

    3 == 5 → false
    3 == 1 → false

So continue.

Search left:

    LCA(5, 5, 1)

Search right:

    LCA(1, 5, 1)

---

## Step 2 — Left Side

Current root:

    5

Tree:

          3
         / \
        5   1
       / \ / \
      6  2 0  8
        / \
       7   4

Check:

    5 == 5 → true

So:

    return 5

Therefore root `3` gets:

    left = 5

---

## Step 3 — Right Side

Current root:

    1

Check:

    1 == 5 → false
    1 == 1 → true

So:

    return 1

Therefore root `3` gets:

    right = 1

---

## Tree

          3
         / \
        5   1
       / \ / \
      6  2 0  8
        / \
       7   4

Current results:

    left = 5
    right = 1

---

## Step 4 — Both Sides Have Result

Check:

    left != NULL
    right != NULL

Both are true.

So:

    return root

Current root:

    3

Therefore:

    LCA = 3

---

# 12. Another Dry Run

Take:

    p = 5
    q = 4

Tree:

          3
         / \
        5   1
       / \
      6   2
         / \
        7   4

---

## Step 1 — Root 3

Current:

    root = 3

Not p and not q.

Search left and right.

---

## Step 2 — Root 5

Current:

    root = 5

Since:

    root == p

Return:

    5

No need to search below `5`.

---

## Step 3 — Back to Root 3

Right subtree does not contain `4`
in this example.

So effectively:

    left = 5
    right = NULL

Since only left returned something:

    return left

Therefore:

    5

So:

    LCA = 5

---

# 13. Important Mental Model

Recursion ka main kaam hai:

> Har subtree se information upar bhejna.

For each subtree:

    "Mujhe p/q/LCA mila hai ya nahi?"

Then parent decides.

Example:

          3
         / \
        5   1

Left says:

    "Mujhe p mila."

Right says:

    "Mujhe q mila."

Parent `3` thinks:

    "Ek left mein hai,
     ek right mein."

Therefore:

    "Main LCA hoon."

---

# 14. Same Tree vs LCA

LC 100 — Same Tree:

    p.left  ↔ q.left
    p.right ↔ q.right

Dono trees compare karne hain.

LC 236 — LCA:

    Search left subtree
    Search right subtree

Then results combine karke LCA find karna hai.

---

# 15. LCA of Binary Tree vs LCA of BST

## LC 236 — Binary Tree

No ordering property.

So:

    Search LEFT
    Search RIGHT

Both may be needed.

---

## LC 235 — BST

BST property available:

    LEFT < ROOT < RIGHT

So:

    both smaller → LEFT
    both larger  → RIGHT
    otherwise    → ROOT

BST version more direct hai.

---

# 16. Common Mistakes

## Mistake 1 — Only search one side

Normal Binary Tree mein target kisi bhi side ho sakta hai.

So:

    left AND right

dono explore karne pad sakte hain.

---

## Mistake 2 — p/q check bhoolna

Correct:

    if (root == p || root == q)
        return root;

Important when one target is an ancestor of the other.

---

## Mistake 3 — Both NULL ka case galat samajhna

If:

    left == NULL
    right == NULL

then:

    return right

means:

    return NULL

which is correct.

---

## Mistake 4 — `left + right` ka meaning miss karna

Remember:

    left != NULL
    &&
    right != NULL

means:

    One target/result came from LEFT
    One target/result came from RIGHT

Therefore:

    current root = LCA

---

# 17. Complexity

Every node may be visited once.

Time Complexity:

    O(N)

where N = number of nodes.

Space Complexity:

    O(H)

where H = height of the tree,
because of recursion stack.

Worst case:

    O(N)

Balanced tree:

    O(log N)

---

# 18. Pattern Recognition

If interview asks:

    Lowest Common Ancestor of Binary Tree

Think:

    root NULL → NULL

    root == p/q → root

    search LEFT

    search RIGHT

    both found → root

    one found → return that side

---

# 19. One-Line Memory Trick

> LCA Binary Tree = Left se result + Right se result mile, toh current root LCA hai.

    NULL → NULL

    root == p/q → root

    left + right → root

    only left → left

    only right → right

---

# Final Code

class Solution {
public:

    TreeNode* lowestCommonAncestor(TreeNode* root,
                                   TreeNode* p,
                                   TreeNode* q) {

        if (root == NULL)
            return NULL;

        if (root == p || root == q)
            return root;

        TreeNode* left =
            lowestCommonAncestor(root->left, p, q);

        TreeNode* right =
            lowestCommonAncestor(root->right, p, q);

        if (left != NULL && right != NULL)
            return root;

        if (left != NULL)
            return left;

        return right;
    }
};

---

# Filename

236-lowest-common-ancestor-of-a-binary-tree.md
