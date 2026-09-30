# LC 572 — Subtree of Another Tree

## Problem

Given the roots of two binary trees:

    root
    subRoot

Check whether `subRoot` is a subtree of `root`.

A subtree means:

    Same values
    +
    Same structure

Example:

Root:

        3
       / \
      4   5
     / \
    1   2

subRoot:

      4
     / \
    1   2

Output:

    true

Because the subtree starting at node `4` is exactly the same as `subRoot`.

---

# 1. Simple Understanding

Humein main tree ke andar `subRoot` ko find karna hai.

Example:

        3
       / \
      4   5
     / \
    1   2

subRoot:

      4
     / \
    1   2

Sabse pehle current node `3` ko check karenge.

    3 != 4

So yahan match nahi hai.

Phir left side mein jayenge:

    4 == 4

Ab check karenge ki dono poore trees same hain ya nahi.

    4
   / \
  1   2

and

    4
   / \
  1   2

Same hain.

So:

    true

---

# 2. Pattern

Pattern:

    Mirror & Symmetry

Is question mein do concepts combine hote hain:

    Search
    +
    Same Tree

LC 100 — Same Tree ka logic yahan reuse hota hai.

---

# 3. Main Idea

`isSubtree()` ka kaam:

    Main tree ke andar possible starting points search karna.

`isSameTree()` ka kaam:

    Current subtree aur subRoot exactly same hain ya nahi check karna.

Overall:

    Search every node
        ↓
    Possible match?
        ↓
    isSameTree()
        ↓
    exact match

---

# 4. Approach

Har current node par:

    1. Agar subRoot NULL hai:
       true

    2. Agar root NULL hai:
       false

    3. Current root aur subRoot ko Same Tree se compare karo.

    4. Agar same hai:
       true

    5. Agar same nahi:
       root ke LEFT mein search karo
       OR
       root ke RIGHT mein search karo

---

# 5. Pseudo Code

    isSubtree(root, subRoot):

        if subRoot == NULL:
            return true

        if root == NULL:
            return false

        if isSameTree(root, subRoot):
            return true

        return
            isSubtree(root->left, subRoot)
            ||
            isSubtree(root->right, subRoot)

---

# 6. Code

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

    bool isSubtree(TreeNode* root, TreeNode* subRoot) {

        if (subRoot == NULL)
            return true;

        if (root == NULL)
            return false;

        if (isSameTree(root, subRoot))
            return true;

        return isSubtree(root->left, subRoot) ||
               isSubtree(root->right, subRoot);
    }
};

---

# 7. Code Explanation

## Part 1 — isSameTree()

Ye exactly LC 100 ka logic hai.

    bool isSameTree(TreeNode* p, TreeNode* q)

Iska kaam:

    Kya dono trees exactly same hain?

### Both NULL

    if (p == NULL && q == NULL)
        return true;

Dono jagah kuch nahi hai.

    NULL ↔ NULL

So:

    true

### One NULL

    if (p == NULL || q == NULL)
        return false;

Ek side node hai aur doosri side empty.

    Node ↔ NULL

So:

    false

### Values different

    if (p->val != q->val)
        return false;

Example:

    3 ↔ 4

Different values.

So:

    false

### Children compare

    return isSameTree(p->left, q->left) &&
           isSameTree(p->right, q->right);

Same position compare hoti hai:

    p.left  ↔ q.left
    p.right ↔ q.right

Dono same hone chahiye.

---

# 8. isSubtree()

    bool isSubtree(TreeNode* root, TreeNode* subRoot)

Ye main search function hai.

## Case 1 — subRoot NULL

    if (subRoot == NULL)
        return true;

Empty tree ko subtree maana jaata hai.

---

## Case 2 — root NULL

    if (root == NULL)
        return false;

Main tree khatam ho gaya,
lekin subRoot abhi bacha hua hai.

So subtree nahi mila.

---

## Case 3 — Current Node Match

    if (isSameTree(root, subRoot))
        return true;

Current node se start hone wala subtree
agar `subRoot` ke exactly same hai:

    true

Yahan LC 100 ka function use ho raha hai.

---

## Case 4 — Search Left OR Right

    return isSubtree(root->left, subRoot) ||
           isSubtree(root->right, subRoot);

Current node match nahi hui.

Ab search:

    LEFT

ya:

    RIGHT

Dono mein se kisi ek side mil gaya toh answer:

    true

Isliye:

    ||

use kiya hai.

---

# 9. Detailed Dry Run

Root tree:

        3
       / \
      4   5
     / \
    1   2

subRoot:

      4
     / \
    1   2

Expected:

    true

---

## Step 1

Call:

    isSubtree(3, 4)

Current root:

    3

subRoot root:

    4

Check:

    isSameTree(3,4)

Values:

    3 != 4

So:

    false

Current node `3` match nahi hui.

---

# Tree

        3
       / \
      4   5
     / \
    1   2

---

## Step 2 — Search Left

Code:

    isSubtree(root->left, subRoot)

`3` ka left = `4`

So:

    isSubtree(4,4)

Current root:

    4

subRoot:

    4

Check:

    isSameTree(4,4)

Values:

    4 == 4

Continue to children.

---

## Step 3 — Left Children

Root subtree:

        4
       / \
      1   2

subRoot:

      4
     / \
    1   2

Compare:

    1 ↔ 1

Values same.

Children:

    NULL ↔ NULL
    NULL ↔ NULL

So:

    isSameTree(1,1) = true

---

# Tree

        3
       / \
      4   5
     / \
    1   2

---

## Step 4 — Right Children

Now compare:

    2 ↔ 2

Values same.

Children:

    NULL ↔ NULL
    NULL ↔ NULL

So:

    isSameTree(2,2) = true

---

## Step 5 — Current Subtree

For node `4`:

    left  comparison = true
    right comparison = true

Therefore:

    true && true

So:

    isSameTree(4,4) = true

Therefore:

    isSubtree(4,4) = true

Main function also returns:

    true

---

# 10. Non-Matching Example

Root:

        3
       / \
      4   5
     / \
    1   2

subRoot:

      4
     / \
    1   9

Start:

    isSubtree(3,4)

Current:

    3 != 4

So search left.

At `4`:

    isSameTree(4,4)

Values same.

Now:

    1 ↔ 1

true.

Then:

    2 ↔ 9

Values different.

Therefore:

    isSameTree(4,4) = false

Now search continues in other parts of root.

`5` also does not match `4`.

Final:

    false

---

# 11. Important Difference: Same Tree vs Subtree

## LC 100 — Same Tree

Do trees already given hain:

    p
    q

Directly compare:

    p ↔ q

Same positions:

    p.left  ↔ q.left
    p.right ↔ q.right

---

## LC 572 — Subtree

Pehle main tree mein matching node search karo.

Then:

    isSameTree()

So:

    Search
      +
    Same Tree

---

# 12. Why `||`?

Code:

    return isSubtree(root->left, subRoot) ||
           isSubtree(root->right, subRoot);

Subtree:

    left side mein ho sakta hai
    OR
    right side mein ho sakta hai

Example:

        3
       / \
      4   5

Agar subtree `4` mein mila:

    true || false = true

Agar left mein nahi aur right mein mila:

    false || true = true

Agar dono mein nahi mila:

    false || false = false

---

# 13. Recursion Flow

Example:

        3
       / \
      4   5
     / \
    1   2

subRoot:

      4
     / \
    1   2

Flow:

    isSubtree(3,4)
        ↓
    isSameTree(3,4)
        ↓
    false
        ↓
    isSubtree(4,4)
        ↓
    isSameTree(4,4)
        ↓
    isSameTree(1,1)
        ↓
    true
        ↓
    isSameTree(2,2)
        ↓
    true
        ↓
    true

Final:

    true

---

# 14. Important Mental Model

Question ko 2 steps mein tod do:

### Step 1

    Kahan se subtree start ho sakta hai?

This is:

    isSubtree()

### Step 2

    Kya yahan se poora subtree exactly same hai?

This is:

    isSameTree()

So:

    isSubtree = SEARCH + SAME TREE

---

# 15. Common Mistakes

## Mistake 1 — Sirf root values compare karna

    root->val == subRoot->val

Ye enough nahi hai.

Poora structure bhi same hona chahiye.

Isliye:

    isSameTree()

use karte hain.

---

## Mistake 2 — Search sirf left mein karna

Wrong:

    isSubtree(root->left, subRoot)

Subtree right side mein bhi ho sakta hai.

Correct:

    isSubtree(root->left, subRoot)
    ||
    isSubtree(root->right, subRoot)

---

## Mistake 3 — `||` ki jagah `&&`

Wrong:

    left && right

Humein subtree dono sides par nahi,
sirf kisi ek side par chahiye.

Correct:

    left || right

---

## Mistake 4 — Same Tree ka logic galat karna

Same Tree mein:

    p.left  ↔ q.left
    p.right ↔ q.right

Mirror order nahi.

---

# 16. Complexity

Basic recursive solution mein worst case:

### Time Complexity

    O(N × M)

where:

    N = nodes in root
    M = nodes in subRoot

Reason:

Main tree ke multiple nodes par `isSameTree()` call ho sakta hai.

### Space Complexity

Recursion stack:

    O(H)

where H = height of main tree.

Worst case:

    O(N)

---

# 17. Pattern Recognition

Agar question bole:

    Is subRoot a subtree of root?
    Find a tree inside another tree
    Check whether a tree occurs inside another tree

Think:

    Search every node
        +
    Same Tree check

---

# 18. One-Line Memory Trick

> Subtree = Main Tree mein starting point search karo, phir Same Tree se exact match verify karo.

    isSubtree()
        ↓
    Search
        +
    isSameTree()
        ↓
    Exact Match

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

    bool isSubtree(TreeNode* root, TreeNode* subRoot) {

        if (subRoot == NULL)
            return true;

        if (root == NULL)
            return false;

        if (isSameTree(root, subRoot))
            return true;

        return isSubtree(root->left, subRoot) ||
               isSubtree(root->right, subRoot);
    }
};

---

# Filename

572-subtree-of-another-tree.md
