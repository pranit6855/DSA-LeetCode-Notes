# LeetCode 653 — Two Sum IV - Input is a BST

## Problem

Given the root of a Binary Search Tree (BST) and an integer `k`, return `true` if there exist **two different nodes** in the tree such that:

`node1.val + node2.val = k`

Otherwise return `false`.

---

## Example

Tree:

        5
       / \
      3   6
     / \   \
    2   4   7

`k = 9`

There are two nodes:

`5 + 4 = 9`

So answer is:

`true`

---

# Main Idea

Ye problem basically:

**Two Sum + Tree Traversal + HashSet**

Normal Two Sum mein hum array ke elements ko traverse karte hain.

Har element `x` ke liye:

`required = k - x`

Phir check karte hain:

`required` pehle mila hai ya nahi?

Tree mein bhi exactly same logic use karenge.

Har node ke liye:

`required = k - root->val`

Agar required value hash set mein already present hai:

`true`

Otherwise current value ko set mein store kar do.

Then left aur right subtree mein recursively search karo.

---

# Why HashSet?

Hume quickly check karna hai ki required value pehle visit hui hai ya nahi.

`unordered_set` mein:

- Insert → average `O(1)`
- Search → average `O(1)`

Isliye ye Two Sum ke liye perfect hai.

---

# Important Point: Check Before Insert

Ye bahut important hai.

Hum pehle check karte hain:

`if (st.count(required))`

uske baad:

`st.insert(root->val)`

Reason:

Same node ko khud ke saath pair nahi karna hai.

Example:

Current node = `5`

`k = 10`

Toh:

`required = 10 - 5 = 5`

Agar hum pehle `5` set mein insert kar dete aur phir check karte, toh current node khud ko hi pair kar leta.

Galat:

`5 + 5 = 10`

Jabki tree mein ek hi `5` node ho sakta hai.

Isliye:

1. Required value check karo
2. Current value insert karo

---

# Approach

Har node par:

1. Agar node `NULL` hai → `false`
2. `required = k - root->val`
3. Check karo kya `required` set mein hai
4. Agar hai → `true`
5. Current value ko set mein insert karo
6. Left subtree mein search karo
7. Right subtree mein search karo
8. Left ya right mein pair mil jaye → `true`

---

# C++ Code

    class Solution {
    public:
        bool solve(TreeNode* root, int k, unordered_set<int>& st) {
            if (root == NULL)
                return false;

            int required = k - root->val;

            if (st.count(required))
                return true;

            st.insert(root->val);

            return solve(root->left, k, st) ||
                   solve(root->right, k, st);
        }

        bool findTarget(TreeNode* root, int k) {
            unordered_set<int> st;
            return solve(root, k, st);
        }
    };

---

# Code Explanation

## 1. solve() function

    bool solve(TreeNode* root, int k, unordered_set<int>& st)

Ye recursive function hai.

`root` = current node

`k` = target sum

`st` = already visited values ka hash set

`&st` use kiya hai taaki har recursive call mein same set use ho.

---

## 2. Base Case

    if (root == NULL)
        return false;

Agar tree ke kisi point par `NULL` mil gaya, iska matlab wahan koi node nahi hai.

Toh pair nahi mila:

`false`

---

## 3. Required Value

    int required = k - root->val;

Maan lo:

`k = 9`

Current node:

`5`

Then:

`required = 9 - 5`

`required = 4`

Matlab hume `5` ke saath pair banane ke liye `4` chahiye.

---

## 4. Check Required Value

    if (st.count(required))
        return true;

`st.count(required)` check karta hai ki required value set mein present hai ya nahi.

Example:

    st = {5, 3, 2}

Current node:

`4`

Then:

`required = 9 - 4 = 5`

Since `5` already set mein hai:

`st.count(5)` → true

So:

`return true`

Pair mil gaya:

`5 + 4 = 9`

---

## 5. Current Value Store Karna

    st.insert(root->val);

Agar pair abhi nahi mila, toh current node ki value set mein store kar dete hain.

Example:

Current node = `3`

Required:

`9 - 3 = 6`

6 set mein nahi hai.

So:

`st = {5, 3}`

Ab future nodes `3` ke saath pair bana sakte hain.

---

# 6. Left and Right Subtree

    return solve(root->left, k, st) ||
           solve(root->right, k, st);

Ye line initially confusing lag sakti hai.

Iska simple meaning:

**Left subtree mein pair dhundo OR Right subtree mein pair dhundo.**

Agar left mein mil gaya:

`true || anything = true`

Agar left mein nahi mila aur right mein mil gaya:

`false || true = true`

Agar dono mein nahi mila:

`false || false = false`

---

# Why `return`?

`solve()` function ka return type `bool` hai.

Isliye recursive call bhi:

`true` ya `false`

return karegi.

Suppose:

    solve(left, k, st) -> true

Then current function bhi:

    return true;

kar dega.

Ye `true` upar wali recursive calls tak propagate hota chala jayega.

---

# Detailed Dry Run

Tree:

        5
       / \
      3   6
     / \   \
    2   4   7

`k = 9`

Initial:

    st = {}

---

## Step 1: Node 5

Current:

`5`

Required:

`9 - 5 = 4`

Set:

`{}`

4 nahi hai.

Insert 5:

`st = {5}`

Then left subtree.

---

## Step 2: Node 3

Current:

`3`

Required:

`9 - 3 = 6`

6 set mein nahi hai.

Insert 3:

`st = {5, 3}`

Then left subtree.

Full tree:

        5
       / \
      3   6
     / \   \
    2   4   7

Current node = `2`

---

## Step 3: Node 2

Current:

`2`

Required:

`9 - 2 = 7`

7 set mein nahi hai.

Insert 2:

`st = {5, 3, 2}`

2 ke left aur right dono NULL hain.

So `solve(2)` → `false`

Ab recursion backtrack karke node `3` par aati hai.

---

## Step 4: Node 4

Ab 3 ka right subtree process hoga.

Full tree:

        5
       / \
      3   6
     / \   \
    2   4   7

Current node = `4`

Required:

`9 - 4 = 5`

Ab set:

`{5, 3, 2}`

5 already present hai.

Therefore:

`st.count(5) = true`

So:

`solve(4) -> true`

Pair:

`4 + 5 = 9`

---

# How True Goes Back

Node 4 ne:

`true`

return kiya.

Toh node 3 ki line:

    return solve(root->left, k, st) ||
           solve(root->right, k, st);

becomes:

    false || true

which is:

`true`

So node 3 bhi `true` return karta hai.

Then node 5:

`true`

return karta hai.

Finally:

`findTarget()` → `true`

---

# Final Answer

`true`

Because:

`5 + 4 = 9`

and `5` and `4` are two different nodes.

---

# Recursion Flow

        5
       / \
      3   6
     / \   \
    2   4   7

Flow:

`5`
→ check 5
→ store 5
→ go left

`3`
→ check 3
→ store 3
→ go left

`2`
→ check 2
→ store 2
→ no pair
→ return false

Back to `3`
→ go right

`4`
→ required = 5
→ 5 already in set
→ return true

Then true travels back:

`4 -> 3 -> 5 -> findTarget`

---

# Pattern to Remember

Is problem ko dekhte hi:

`Two values sum to K`

So think:

`required = K - current`

Then:

`Have I seen required before?`

For a tree:

**DFS + HashSet**

---

# Complexity

Let `n` = number of nodes.

## Time Complexity

Har node maximum ek baar visit hota hai.

`O(n)`

## Space Complexity

HashSet mein maximum `n` values store ho sakti hain.

Recursive stack bhi worst case `O(n)` ho sakta hai.

Overall auxiliary space:

`O(n)`

---

# Interview Explanation

Interviewer ko simple language mein:

"I traverse the tree using DFS. For every node, I calculate the value required to make the target sum as `k - node->val`. I keep previously visited node values in an unordered_set. If the required value is already present, I return true. Otherwise I insert the current value and continue searching in the left and right subtrees."

---

# One-Line Memory Trick

**Current value ke liye partner chahiye = `k - current`.**

**Partner pehle mila = `true`.**

**Nahi mila = current ko set mein daalo aur tree mein aage jao.**

---

# LeetCode

Problem: 653

Name: Two Sum IV - Input is a BST

Pattern: Search / BST

Technique: DFS + HashSet

Difficulty: Easy
