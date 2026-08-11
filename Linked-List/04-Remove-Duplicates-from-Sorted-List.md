# 🔗 LeetCode 83 — Remove Duplicates from Sorted List

## 📌 Problem

Hume ek **sorted singly linked list** di gayi hai.

Hume usme se duplicate values remove karni hain, taaki har value sirf **ek baar** aaye.

Example:

```text
Input:

[1] → [1] → [2] → NULL
```

Output:

```text
[1] → [2] → NULL
```

Another example:

```text
Input:

[1] → [1] → [2] → [3] → [3] → NULL
```

Output:

```text
[1] → [2] → [3] → NULL
```

List already ascending order me sorted hoti hai. :contentReference[oaicite:1]{index=1}

---

# 🧠 Main Idea

Sabse important observation:

> **List sorted hai, isliye duplicate values hamesha adjacent/consecutive nodes par hongi.** :contentReference[oaicite:2]{index=2}

Example:

```text
[1] → [1] → [2] → [3] → [3]
```

Duplicate pairs:

```text
1 → 1
3 → 3
```

Isliye hume poori list me har value search karne ki zarurat nahi.

Bas:

```text
curr
  ↓
[1] → [1]
```

me compare karna hai:

```cpp
curr->val == curr->next->val
```

---

# 🔥 Pointer Approach

Sirf ek pointer:

```cpp
ListNode* curr = head;
```

use karenge.

Diagram:

```text
curr
 ↓
[1] → [1] → [2] → [3] → [3] → NULL
```

Har step par:

```text
current node
      ↓
compare
      ↓
next node
```

---

# 1️⃣ Duplicate Mil Gaya

Suppose:

```text
curr
 ↓
[1] → [1] → [2] → NULL
        ↑
      duplicate
```

Check:

```cpp
curr->val == curr->next->val
```

```text
1 == 1
```

True.

Ab second `1` ko remove karna hai.

Hum:

```cpp
curr->next = curr->next->next;
```

kar denge.

Before:

```text
[1] → [1] → [2]
 ↑      ↑
curr  duplicate
```

After:

```text
[1] ─────────→ [2]
 ↑
curr
```

Second `1` ko chain se skip kar diya.

---

# ⭐ Important: `curr` Move Nahi Karega

Ye bahut important hai.

Suppose:

```text
[1] → [1] → [1] → [2]
 ↑
curr
```

First duplicate remove:

```text
[1] → [1] → [2]
 ↑
curr
```

Abhi bhi:

```text
curr->val == curr->next->val
```

hai:

```text
1 == 1
```

So dobara duplicate remove karna padega.

Therefore duplicate milne par:

```cpp
curr->next = curr->next->next;
```

**but**

```cpp
curr = curr->next;
```

nahi karna.

---

# 2️⃣ Duplicate Nahi Mila

Suppose:

```text
curr
 ↓
[1] → [2] → [3]
```

Check:

```text
1 == 2
```

False.

Matlab current node duplicate nahi hai.

Ab aage move karo:

```cpp
curr = curr->next;
```

Result:

```text
        curr
         ↓
[1] → [2] → [3]
```

---

# 🔥 Two Cases

## Case 1 — Duplicate

```cpp
if(curr->val == curr->next->val)
```

Then:

```cpp
curr->next = curr->next->next;
```

### Pointer:

```text
curr SAME
```

---

## Case 2 — No Duplicate

```cpp
else
```

Then:

```cpp
curr = curr->next;
```

### Pointer:

```text
curr AAGE
```

---

# 🧠 Main Pattern

```text
Duplicate mila
      ↓
Node ko skip karo
      ↓
curr SAME


Duplicate nahi mila
      ↓
curr ko aage move karo
```

Yaad rakhne ke liye:

```text
DUPLICATE
   ↓
LINK CHANGE

NO DUPLICATE
   ↓
POINTER MOVE
```

---

# 🔄 Dry Run

Input:

```text
[1] → [1] → [1] → [2] → [3] → [3] → NULL
```

---

## Step 1

```text
curr
 ↓
[1] → [1] → [1] → [2] → [3] → [3]
```

Compare:

```text
1 == 1
```

Duplicate.

Apply:

```cpp
curr->next = curr->next->next;
```

Now:

```text
curr
 ↓
[1] → [1] → [2] → [3] → [3]
```

`curr` same position.

---

# Step 2

Again:

```text
1 == 1
```

Duplicate.

Skip:

```text
curr
 ↓
[1] → [2] → [3] → [3]
```

Again `curr` same.

---

# Step 3

Now:

```text
1 == 2
```

Not duplicate.

So:

```cpp
curr = curr->next;
```

Now:

```text
        curr
         ↓
[1] → [2] → [3] → [3]
```

---

# Step 4

Compare:

```text
2 == 3
```

False.

Move:

```text
          curr
           ↓
[1] → [2] → [3] → [3]
```

---

# Step 5

Compare:

```text
3 == 3
```

True.

Skip:

```cpp
curr->next = curr->next->next;
```

Result:

```text
[1] → [2] → [3] → NULL
              ↑
             curr
```

Now:

```text
curr->next == NULL
```

So loop stop.

Final:

```text
[1] → [2] → [3] → NULL
```

---

# 🔥 Why `curr->next != NULL`?

Hum condition me:

```cpp
curr->next->val
```

access kar rahe hain.

Isliye `curr->next` exist karna chahiye.

Correct:

```cpp
while(curr != NULL && curr->next != NULL)
```

Agar:

```text
curr
 ↓
[3] → NULL
```

to:

```text
curr->next == NULL
```

Ab hum:

```cpp
curr->next->val
```

nahi kar sakte.

Isliye loop stop.

---

# ⚠️ Why `curr != NULL` Also?

Normally loop ke through `curr` move karte hue:

```cpp
curr = curr->next;
```

ho sakta hai ki `curr` eventually `NULL` ho.

So safe condition:

```cpp
while(curr != NULL && curr->next != NULL)
```

Meaning:

> Current node bhi exist kare aur uske baad next node bhi exist kare.

---

# 🧩 Complete Algorithm

### Step 1

```cpp
ListNode* curr = head;
```

---

### Step 2

Jab tak current aur next node exist karte hain:

```cpp
while(curr != NULL && curr->next != NULL)
```

---

### Step 3

Compare:

```cpp
curr->val == curr->next->val
```

---

### Step 4 — Duplicate

```cpp
curr->next = curr->next->next;
```

`curr` ko move mat karo.

---

### Step 5 — Different

```cpp
curr = curr->next;
```

---

### Step 6

Finally:

```cpp
return head;
```

---

# 💻 Final C++ Code

```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {

        ListNode* curr = head;

        while(curr != NULL && curr->next != NULL) {

            if(curr->val == curr->next->val) {

                curr->next = curr->next->next;
            }
            else {

                curr = curr->next;
            }
        }

        return head;
    }
};
```

---

# 🔍 Code Line-by-Line

### 1. Current pointer

```cpp
ListNode* curr = head;
```

List ke first node se start.

---

### 2. Loop

```cpp
while(curr != NULL && curr->next != NULL)
```

Current aur next dono available hone chahiye.

---

### 3. Duplicate check

```cpp
if(curr->val == curr->next->val)
```

Current aur next ki value same hai?

---

### 4. Duplicate remove

```cpp
curr->next = curr->next->next;
```

Next node ko skip kar do.

Example:

```text
Before:

[1] → [1] → [2]
        ↑
      remove
```

After:

```text
[1] ─────→ [2]
```

---

### 5. No duplicate

```cpp
curr = curr->next;
```

Aage move.

---

### 6. Return

```cpp
return head;
```

Head change nahi hua, sirf links modify hue hain.

---

# 🤔 `head` Return Kyu?

Yahan first node delete nahi kar rahe.

Example:

```text
[1] → [1] → [2]
 ↑
head
```

Duplicate remove karne ke baad:

```text
[1] → [2]
 ↑
head
```

Head wahi `[1]` hai.

Isliye:

```cpp
return head;
```

---

# 🔥 Compare With Previous Questions

## Insert

Middle insertion:

```cpp
newNode->next = curr->next;
curr->next = newNode;
```

---

## Delete Specific Node

```cpp
temp = curr->next;
curr->next = temp->next;
delete temp;
```

---

## Remove Duplicate

Yahan actual node delete karna mandatory nahi hai.

Hum simply:

```cpp
curr->next = curr->next->next;
```

karke duplicate node ko **skip** kar dete hain.

---

# 🧠 Why We Don't Need `delete` Here?

LeetCode ka expected task linked-list structure ko modify karke duplicates remove karna hai.

Hum:

```cpp
curr->next = curr->next->next;
```

se duplicate node ko reachable chain se hata dete hain.

Conceptually:

```text
Before:

A → B → C

After:

A → C

B is no longer part of the returned list.
```

Is problem ke standard single-pass solution me isi pointer adjustment ka use hota hai. :contentReference[oaicite:3]{index=3}

---

# ⚠️ Common Mistake #1

### Duplicate milne ke baad `curr` move karna

Wrong:

```cpp
if(curr->val == curr->next->val) {
    curr->next = curr->next->next;
    curr = curr->next;   // ❌
}
```

Problem:

```text
[1] → [1] → [1]
 ↑
curr
```

First duplicate skip karne ke baad:

```text
[1] → [1]
 ↑
curr
```

Agar `curr` move kar diya:

```text
        curr
         ↓
[1] → [1]
```

To duplicate chain ka check miss ho sakta hai.

Correct:

```cpp
if(curr->val == curr->next->val) {
    curr->next = curr->next->next;
}
```

**No movement.**

---

# ⚠️ Common Mistake #2

### `curr->next` check nahi karna

Wrong:

```cpp
while(curr != NULL) {
    if(curr->val == curr->next->val)
```

Agar:

```text
curr
 ↓
[3] → NULL
```

to:

```cpp
curr->next->val
```

invalid access hoga.

Correct:

```cpp
while(curr != NULL && curr->next != NULL)
```

---

# ⚠️ Common Mistake #3

### Sorted property ignore karna

Hum sirf adjacent nodes compare kar rahe hain:

```cpp
curr->val == curr->next->val
```

Ye **sirf isliye sufficient hai because list sorted hai**.

Example:

```text
[1] → [2] → [1]
```

Agar unsorted hoti, duplicate `1` door ho sakta tha.

But problem guarantees ascending sorted order. :contentReference[oaicite:4]{index=4}

---

# 🔥 Pattern Recognition

Ye question:

```text
Linked List
+
Sorted Data
+
Adjacent Comparison
+
Pointer Manipulation
```

ka pattern hai.

General pattern:

```text
curr
 ↓
current → next
```

Compare:

```text
current == next
```

Then either:

```text
duplicate
   ↓
skip next
```

or:

```text
different
   ↓
move curr
```

---

# 📊 Dry Run Table

Input:

```text
[1] → [1] → [2] → [3] → [3]
```

| `curr` | `curr->next` | Same? | Action |
|---|---|---|---|
| 1 | 1 | Yes | Skip next |
| 1 | 2 | No | Move curr |
| 2 | 3 | No | Move curr |
| 3 | 3 | Yes | Skip next |
| 3 | NULL | — | Stop |

Output:

```text
[1] → [2] → [3] → NULL
```

---

# ⏱️ Complexity

### Time

```text
O(n)
```

List ko single pass me traverse karte hain. Standard iterative solution linear time leta hai. :contentReference[oaicite:5]{index=5}

### Space

```text
O(1)
```

Sirf `curr` pointer use kar rahe hain.

No extra array/hashmap.

---

# 🧠 Quick Revision

### Initialization

```cpp
ListNode* curr = head;
```

### Loop

```cpp
while(curr != NULL && curr->next != NULL)
```

### Duplicate

```cpp
curr->next = curr->next->next;
```

### Different

```cpp
curr = curr->next;
```

### Return

```cpp
return head;
```

---

# ⭐ One-Line Memory Trick

> **Sorted list me duplicate hamesha next node ke paas milega; duplicate mile to `curr->next` ko skip karo, duplicate na mile to `curr` ko aage badhao.**

---

# 📁 Linked List Progress

```text
Linked-List/
│
├── 01-Convert-Binary-Number-to-Integer.md
├── 02-Middle-of-the-Linked-List.md
├── 03-Reverse-Linked-List.md
├── 04-Merge-Two-Sorted-Lists.md
└── 05-Remove-Duplicates-from-Sorted-List.md
```

### Git Commit

```text
Add LC 83 Remove Duplicates from Sorted List
```
