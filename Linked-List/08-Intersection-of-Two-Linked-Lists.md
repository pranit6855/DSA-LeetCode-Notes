# 🔗 LeetCode 160 — Intersection of Two Linked Lists

## 📌 Problem

Hume do singly linked lists di gayi hain:

```text
headA
headB
```

Hume find karna hai ki dono linked lists **kis node par intersect karti hain**.

Agar intersection hai:

```text
return intersection node
```

Agar intersection nahi hai:

```text
return NULL
```

---

# 🧠 Sabse Important: Intersection Ka Matlab

Intersection ka matlab:

> Dono lists ka **same actual node** share karna.

Sirf value same hona intersection nahi hai.

### ❌ Same value, but no intersection

```text
List A:

[1] → [2] → NULL


List B:

[1] → [3] → NULL
```

Dono me `1` hai, but ye **alag nodes** hain.

So:

```text
No intersection
```

---

# ✅ Actual Intersection

```text
List A:

[4] → [1] ─────┐
                ↓
               [8] → [4] → [5] → NULL
                ↑
List B:         │
[5] → [6] → [1]┘
```

Yahan `[8]` ke baad dono same nodes use kar rahi hain.

So:

```text
Answer = [8]
```

---

# 🔥 Node Value vs Node Address

Ye bahut important hai.

Wrong:

```cpp
pA->val == pB->val
```

Ye sirf values compare karega.

Correct:

```cpp
pA == pB
```

Ye check karega ki dono pointers **same actual node** ko point kar rahe hain.

Example:

```text
A: [5] → [2]
     ↑
     different node

B: [5] → [3]
     ↑
     different node
```

Although:

```text
A->val == B->val
```

but:

```text
A != B
```

So intersection nahi hai.

---

# 🧠 Main Problem

Maan lo:

```text
A: A1 → A2 → C1 → C2 → C3
B: B1 → B2 → B3 → C1 → C2 → C3
```

Common part:

```text
C1 → C2 → C3
```

But:

```text
A unique part = 2 nodes
B unique part = 3 nodes
```

Dono different positions se common part me enter kar rahe hain.

Agar simply:

```cpp
pA = headA;
pB = headB;
```

karke same speed se chalaoge, to initially aligned nahi honge.

---

# 🔥 Main Trick — Switch Heads

Do pointers:

```cpp
ListNode* pA = headA;
ListNode* pB = headB;
```

Dono ko **1-1 step** move karenge.

Lekin special rule:

### Jab `pA` NULL ho:

```cpp
pA = headB;
```

Matlab:

```text
A khatam
   ↓
B se start
```

### Jab `pB` NULL ho:

```cpp
pB = headA;
```

Matlab:

```text
B khatam
   ↓
A se start
```

---

# ⭐ Main Pattern

```text
pA:

A → A → A → NULL → B → B → B
                 ↑
              switch


pB:

B → B → B → NULL → A → A → A
                 ↑
              switch
```

Isko short me yaad rakho:

```text
A → B
B → A
```

---

# 🤔 Why Does Switching Work?

Maan lo:

```text
A = 2 unique + 3 common
B = 3 unique + 3 common
```

So:

```text
A:

A1 → A2 → C1 → C2 → C3
```

and:

```text
B:

B1 → B2 → B3 → C1 → C2 → C3
```

B ke paas 1 extra unique node hai.

---

# 🔄 Normal Traversal

Initially:

```text
pA → A1
pB → B1
```

Move:

```text
pA → A2
pB → B2
```

Move:

```text
pA → C1
pB → B3
```

Ab:

```text
pA → C2
pB → C1
```

Dono aligned nahi hain.

---

# 🔥 Ab Switch

A eventually:

```text
C3 → NULL
```

par pahunchta hai.

So:

```cpp
pA = headB;
```

Ab A pointer B ki list traverse karega.

Similarly B jab NULL par pahunchta hai:

```cpp
pB = headA;
```

Ab B pointer A ki list traverse karega.

---

# 🧠 Why This Equalizes the Distance

Suppose:

```text
Length A = 5
Length B = 6
```

A pointer:

```text
A ki 5 nodes
+
B ki 6 nodes
```

travel karega.

Total:

```text
11 nodes
```

B pointer:

```text
B ki 6 nodes
+
A ki 5 nodes
```

travel karega.

Total:

```text
11 nodes
```

Therefore:

```text
Dono same total distance travel karte hain.
```

Unique prefix ka length difference automatically cancel ho jata hai.

---

# 🔥 Simple Visualization

Maan lo:

```text
A = 2 unique + common
B = 3 unique + common
```

Then:

```text
pA:

A1 → A2 → C1 → C2 → C3 → B1 → B2 → B3 → C1
 ↑
start A                              ↑
                                  common


pB:

B1 → B2 → B3 → C1 → C2 → C3 → A1 → A2 → C1
 ↑
start B                              ↑
                                  common
```

Dono finally:

```text
C1
```

par milenge.

Therefore:

```cpp
pA == pB
```

---

# 🔥 Algorithm

### Step 1

Initialize:

```cpp
ListNode* pA = headA;
ListNode* pB = headB;
```

---

### Step 2

Jab tak dono same node par nahi hain:

```cpp
while(pA != pB)
```

---

### Step 3

`pA` ko move karo:

```cpp
if(pA == NULL)
    pA = headB;
else
    pA = pA->next;
```

Meaning:

```text
pA end par
   ↓
headB par jump
```

---

### Step 4

`pB` ko move karo:

```cpp
if(pB == NULL)
    pB = headA;
else
    pB = pB->next;
```

Meaning:

```text
pB end par
   ↓
headA par jump
```

---

### Step 5

Jab:

```cpp
pA == pB
```

ho:

```cpp
return pA;
```

---

# 💻 Final C++ Code

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {

        ListNode* pA = headA;
        ListNode* pB = headB;

        while(pA != pB) {

            if(pA == NULL)
                pA = headB;
            else
                pA = pA->next;

            if(pB == NULL)
                pB = headA;
            else
                pB = pB->next;
        }

        return pA;
    }
};
```

---

# 🔍 Code Line-by-Line

## 1. Two pointers

```cpp
ListNode* pA = headA;
ListNode* pB = headB;
```

`pA` list A se start.

`pB` list B se start.

---

## 2. Loop condition

```cpp
while(pA != pB)
```

Jab tak dono same node par nahi hain.

Important:

```cpp
pA != pB
```

**address/node compare karta hai**, values nahi.

---

## 3. Move pA

```cpp
if(pA == NULL)
    pA = headB;
else
    pA = pA->next;
```

Agar list A khatam:

```text
pA → NULL
```

to:

```text
pA → headB
```

---

## 4. Move pB

```cpp
if(pB == NULL)
    pB = headA;
else
    pB = pB->next;
```

B khatam:

```text
pB → NULL
```

to:

```text
pB → headA
```

---

## 5. Return

```cpp
return pA;
```

Do possibilities:

### Intersection hai:

```text
pA == pB == intersection node
```

Return:

```text
intersection node
```

### Intersection nahi hai:

Eventually:

```text
pA == pB == NULL
```

Return:

```text
NULL
```

---

# 🔄 Dry Run — Intersection Hai

Input:

```text
A:

[4] → [1] → [8] → [4] → [5] → NULL


B:

[5] → [6] → [1] → [8] → [4] → [5] → NULL
```

Common node:

```text
[8]
```

---

### Start

```text
pA → 4
pB → 5
```

---

### Move 1

```text
pA → 1
pB → 6
```

---

### Move 2

```text
pA → 8
pB → 1
```

---

### Move 3

```text
pA → 4
pB → 8
```

---

### Move 4

```text
pA → 5
pB → 4
```

---

### Move 5

`pA` reaches:

```text
NULL
```

So:

```cpp
pA = headB;
```

Now:

```text
pA → 5(B)
pB → 5(common)
```

Important: same **value `5`** hone ka matlab automatically intersection nahi. Abhi ye check pointer identity se hoga.

---

### Continue

Eventually:

```text
pA → 8
pB → 8
```

Now:

```cpp
pA == pB
```

True.

Return:

```text
[8]
```

---

# 🔥 Dry Run — No Intersection

Suppose:

```text
A:

[1] → [2] → [3] → NULL


B:

[4] → [5] → NULL
```

Pointers apni lists traverse karenge.

When A ends:

```text
pA = headB
```

When B ends:

```text
pB = headA
```

Dono complete opposite list bhi traverse kar lenge.

Eventually:

```text
pA = NULL
pB = NULL
```

So:

```cpp
pA == pB
```

True.

Loop stop.

```cpp
return pA;
```

returns:

```text
NULL
```

Correct.

---

# ⚠️ Common Mistake #1

## Value Compare Karna

Wrong:

```cpp
while(pA->val != pB->val)
```

Because same value wale different nodes ho sakte hain.

Correct:

```cpp
while(pA != pB)
```

---

# ⚠️ Common Mistake #2

## `pA == NULL` Par Stop Karna

Wrong:

```cpp
while(pA != NULL)
```

Isse intersection milne se pehle logic stop ho sakta hai.

Hum NULL ko **switching point** ke tarah use kar rahe hain.

Correct:

```cpp
while(pA != pB)
```

---

# ⚠️ Common Mistake #3

## Sirf A Ko Switch Karna

Wrong:

```cpp
if(pA == NULL)
    pA = headB;
```

but B ko switch nahi kiya.

Dono pointers ko switch karna zaroori hai:

```cpp
if(pA == NULL)
    pA = headB;

if(pB == NULL)
    pB = headA;
```

---

# ⚠️ Common Mistake #4

## `pA = headA` Again Karna

Wrong:

```cpp
if(pA == NULL)
    pA = headA;
```

Isse pA same list ko baar-baar traverse karta rahega.

Correct:

```cpp
if(pA == NULL)
    pA = headB;
```

Because:

```text
A → B
B → A
```

---

# 🧠 Alternative Approach — Length Method

Ek aur approach hai:

```text
1. A ki length
2. B ki length
3. Difference find
4. Longer list ka pointer difference steps aage
5. Dono pointers simultaneously move
6. pA == pB
```

Example:

```text
A length = 5
B length = 8

difference = 3
```

B pointer ko 3 steps aage karo.

Then both same position se common part traverse karenge.

### But hamara approach:

```text
A → B
B → A
```

me length calculate karne ki zarurat nahi.

---

# 🆚 Two Approaches

| Approach | Time | Space |
|---|---:|---:|
| Length Difference | O(n + m) | O(1) |
| Switch Heads | O(n + m) | O(1) |
| HashSet | O(n + m) | O(n) |

Hamne:

```text
Switch Heads
```

approach use kiya.

---

# 🔥 Why No Extra Data Structure?

Hume:

```text
array
set
map
```

ki zarurat nahi.

Sirf:

```cpp
pA
pB
```

two pointers use kar rahe hain.

Therefore:

```text
Space = O(1)
```

---

# ⏱️ Complexity

Let:

```text
n = length of A
m = length of B
```

### Time

```text
O(n + m)
```

Dono pointers maximum combined lists ko traverse karte hain.

### Space

```text
O(1)
```

Only two pointers.

---

# 🧠 Pattern Recognition

Question me agar bole:

```text
Two linked lists
+
Find common/intersection node
```

Immediately think:

```text
Two Pointers
```

### Main trick:

```text
pA → A → B
pB → B → A
```

Then:

```text
pA == pB
```

---

# ⭐ Most Important Concept

Ye line:

```cpp
if(pA == NULL)
    pA = headB;
```

ka matlab:

```text
A khatam
↓
B ki journey start
```

Aur:

```cpp
if(pB == NULL)
    pB = headA;
```

ka matlab:

```text
B khatam
↓
A ki journey start
```

So:

```text
A + B
B + A
```

Dono ki total distance same.

---

# 🔥 One-Line Memory Trick

> **Ek pointer A→B traverse karega, doosra B→A; dono same total distance cover karenge, isliye agar intersection hai to dono same actual node par milenge.**

---

# 📁 Linked List Progress

```text
Linked-List/
│
├── 01-Convert-Binary-Number-to-Integer.md
├── 02-Middle-of-the-Linked-List.md
├── 03-Reverse-Linked-List.md
├── 04-Merge-Two-Sorted-Lists.md
├── 05-Remove-Duplicates-from-Sorted-List.md
├── 06-Linked-List-Cycle.md
├── 07-Linked-List-Cycle-II.md
├── 08-Remove-Nth-Node-From-End.md
└── 09-Intersection-of-Two-Linked-Lists.md
```

### Git Commit

```text
Add LC 160 Intersection of Two Linked Lists
```

---

# 🧠 Quick Revision

```cpp
ListNode* pA = headA;
ListNode* pB = headB;

while(pA != pB) {

    if(pA == NULL)
        pA = headB;
    else
        pA = pA->next;

    if(pB == NULL)
        pB = headA;
    else
        pB = pB->next;
}

return pA;
```

### बस 3 चीजें:

```text
1. pA = headA
2. pB = headB
3. A खत्म → B, B खत्म → A
```

Then:

```text
pA == pB
↓
answer
```
