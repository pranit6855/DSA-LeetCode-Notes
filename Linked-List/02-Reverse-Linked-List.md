# 🔗 LeetCode 206 — Reverse Linked List

## 📌 Problem

Hume ek singly linked list di gayi hai.

Hume poori linked list ko **reverse** karke new head return karna hai.

Example:

```text
Input:

head
 ↓
[1] → [2] → [3] → [4] → NULL
```

Reverse karne ke baad:

```text
Output:

head
 ↓
[4] → [3] → [2] → [1] → NULL
```

---

# 🧠 Main Idea

Normal linked list me arrows:

```text
1 → 2 → 3 → 4 → NULL
```

Reverse karne ke baad arrows:

```text
NULL ← 1 ← 2 ← 3 ← 4
```

Ya normal direction me:

```text
4 → 3 → 2 → 1 → NULL
```

Matlab har node ka:

```cpp
next
```

change karna hai.

---

# 🔥 Sabse Important Problem

Suppose:

```text
[1] → [2] → [3] → NULL
```

Agar hum directly:

```cpp
curr->next = prev;
```

kar dein:

```text
NULL ← [1]    [2] → [3] → NULL
```

To `2` ka address lose ho sakta hai, kyunki `1` ka original `next` change ho gaya.

Isliye **pehle next node ka address save karna zaroori hai.**

---

# ⭐ 3 Pointers

Hum 3 pointers use karenge:

```text
prev
curr
next
```

### `prev`

Current node ka reverse connection kis node ki taraf hona chahiye.

### `curr`

Jis node par abhi kaam kar rahe hain.

### `next`

Original next node ka address temporarily save karega.

---

# 🔹 Initial State

```cpp
ListNode* prev = NULL;
ListNode* curr = head;
```

Suppose:

```text
head
 ↓
[1] → [2] → [3] → [4] → NULL
```

Then:

```text
prev
 ↓
NULL

curr
 ↓
[1] → [2] → [3] → [4] → NULL
```

---

# 🔥 Four-Step Pattern

Har iteration me exactly 4 kaam:

```text
1. SAVE
2. REVERSE
3. MOVE prev
4. MOVE curr
```

Code:

```cpp
next = curr->next;

curr->next = prev;

prev = curr;

curr = next;
```

Is pattern ko yaad rakhna bahut important hai.

---

# 1️⃣ SAVE — `next`

```cpp
ListNode* next = curr->next;
```

Starting:

```text
prev
 ↓
NULL

curr
 ↓
[1] → [2] → [3] → [4] → NULL
```

`next` ko:

```text
[2]
```

ka address mil gaya.

Diagram:

```text
prev       curr       next
 ↓          ↓          ↓
NULL       [1] →      [2] → [3] → [4] → NULL
```

### Why save?

Because next step me:

```cpp
curr->next = prev;
```

karne wale hain.

Agar pehle `next` save nahi kiya, to original:

```text
1 → 2
```

connection lose ho jayega.

---

# 2️⃣ REVERSE — `curr->next = prev`

```cpp
curr->next = prev;
```

Currently:

```text
prev       curr       next
 ↓          ↓          ↓
NULL       [1]        [2] → [3] → [4]
```

Ab:

```text
[1] → NULL
```

ban jayega.

Diagram:

```text
       curr
        ↓
NULL ← [1]    [2] → [3] → [4] → NULL
```

Matlab:

```text
1 → 2
```

ko:

```text
1 → NULL
```

kar diya.

---

# 3️⃣ MOVE `prev`

Ab:

```cpp
prev = curr;
```

So:

```text
       prev
        ↓
NULL ← [1]    [2] → [3] → [4] → NULL
```

Ab `prev` reversed part ke first/end node ko point kar raha hai.

---

# 4️⃣ MOVE `curr`

Ab:

```cpp
curr = next;
```

`next` me pehle hi `[2]` save tha.

So:

```text
       prev       curr
        ↓          ↓
NULL ← [1]       [2] → [3] → [4] → NULL
```

Ab next iteration `[2]` par hogi.

---

# 🔄 Second Iteration

Current state:

```text
       prev       curr
        ↓          ↓
NULL ← [1]       [2] → [3] → [4] → NULL
```

## Step 1 — Save

```cpp
next = curr->next;
```

Now:

```text
prev       curr       next
 ↓          ↓          ↓
[1]        [2]        [3]
```

---

## Step 2 — Reverse

```cpp
curr->next = prev;
```

Now:

```text
NULL ← [1] ← [2]    [3] → [4] → NULL
```

---

## Step 3 — Move prev

```cpp
prev = curr;
```

```text
             prev
              ↓
NULL ← [1] ← [2]    [3] → [4] → NULL
```

---

## Step 4 — Move curr

```cpp
curr = next;
```

```text
             prev       curr
              ↓          ↓
NULL ← [1] ← [2]       [3] → [4] → NULL
```

---

# 🔄 Third Iteration

Same process.

Before:

```text
              prev       curr
               ↓          ↓
NULL ← [1] ← [2]       [3] → [4] → NULL
```

Save:

```text
next = [4]
```

Reverse:

```text
NULL ← [1] ← [2] ← [3]    [4] → NULL
```

Move:

```text
prev = [3]
curr = [4]
```

---

# 🔄 Fourth Iteration

Before:

```text
                  prev       curr
                   ↓          ↓
NULL ← [1] ← [2] ← [3]       [4] → NULL
```

Save:

```text
next = NULL
```

Reverse:

```cpp
curr->next = prev;
```

Result:

```text
NULL ← [1] ← [2] ← [3] ← [4]
```

Then:

```cpp
prev = curr;
curr = next;
```

So:

```text
prev
 ↓
[4] → [3] → [2] → [1] → NULL

curr
 ↓
NULL
```

---

# 🎯 Loop End

Condition:

```cpp
while(curr != NULL)
```

Ab:

```text
curr = NULL
```

So loop stop.

But reversed list ka first node kaun hai?

```text
prev
 ↓
[4] → [3] → [2] → [1] → NULL
```

Therefore:

```cpp
return prev;
```

---

# 🔥 Why `return prev`?

Original:

```text
head
 ↓
[1] → [2] → [3] → [4] → NULL
```

Reverse ke baad:

```text
[4] → [3] → [2] → [1] → NULL
 ↑
prev
```

Original `head` ab:

```text
[1] → NULL
```

ko represent karta hai.

New head:

```text
[4]
```

hai.

Aur `prev` exactly `[4]` par hai.

Therefore:

```cpp
return prev;
```

---

# 💻 Final Code

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {

        ListNode* prev = NULL;
        ListNode* curr = head;

        while(curr != NULL) {

            ListNode* next = curr->next;

            curr->next = prev;

            prev = curr;

            curr = next;
        }

        return prev;
    }
};
```

---

# 🧩 Code Line-by-Line

### 1. `prev`

```cpp
ListNode* prev = NULL;
```

Initially first node ke peeche kuch nahi hai.

So:

```text
prev = NULL
```

---

### 2. `curr`

```cpp
ListNode* curr = head;
```

Current pointer first node se start karega.

```text
curr
 ↓
[1] → [2] → [3] → NULL
```

---

### 3. Save next

```cpp
ListNode* next = curr->next;
```

Current node ke original next ko save karna.

---

### 4. Reverse arrow

```cpp
curr->next = prev;
```

Ye actual reversal hai.

---

### 5. `prev` move

```cpp
prev = curr;
```

Reversed part ko ek node aage badhao.

---

### 6. `curr` move

```cpp
curr = next;
```

Original list ke next node par move karo.

---

### 7. Return

```cpp
return prev;
```

`prev` ab new head hai.

---

# 🔥 Core Pattern

Ye 4 lines **ratne ke liye nahi, sequence samajhne ke liye**:

```cpp
ListNode* next = curr->next; // SAVE

curr->next = prev;           // REVERSE

prev = curr;                 // MOVE PREV

curr = next;                 // MOVE CURR
```

Memory trick:

```text
SAVE
 ↓
REVERSE
 ↓
PREV AAGE
 ↓
CURR AAGE
```

---

# ⚠️ Common Mistake #1

### Wrong order:

```cpp
curr->next = prev;

next = curr->next;
```

Problem:

```text
curr->next
```

already change ho gaya.

Original next node ka address lose ho sakta hai.

### Correct:

```cpp
next = curr->next;
curr->next = prev;
```

**Pehle save, phir reverse.**

---

# ⚠️ Common Mistake #2

`prev` ko return karna bhoolna.

Wrong:

```cpp
return head;
```

Because `head` original first node ko point kar raha tha.

Reverse ke baad:

```text
[1] → NULL
```

new head nahi hai.

Correct:

```cpp
return prev;
```

---

# ⚠️ Common Mistake #3

`curr` ko move nahi karna:

```cpp
curr = next;
```

Agar ye nahi kiya to `curr` same node par rahega aur infinite loop ho jayega.

---

# ⚠️ Common Mistake #4

`prev` ko move nahi karna:

```cpp
prev = curr;
```

Agar nahi kiya to har node ka `next` `NULL` hi set hota rahega.

---

# 🔥 Empty List Edge Case

Agar:

```text
head = NULL
```

Then:

```cpp
prev = NULL;
curr = NULL;
```

Loop chalega hi nahi.

Return:

```cpp
return prev;
```

So:

```text
NULL
```

Correct.

---

# 🔥 Single Node Edge Case

```text
head
 ↓
[10] → NULL
```

Initial:

```text
prev = NULL
curr = [10]
```

One iteration:

```text
next = NULL
curr->next = NULL
prev = [10]
curr = NULL
```

Return:

```text
[10]
```

Correct.

---

# 🧠 Dry Run Table

For:

```text
[1] → [2] → [3] → NULL
```

| Step | `prev` | `curr` | `next` |
|---|---|---|---|
| Start | `NULL` | `1` | — |
| 1 | `1` | `2` | `2` |
| 2 | `2` | `3` | `3` |
| 3 | `3` | `NULL` | `NULL` |

Final:

```text
prev
 ↓
[3] → [2] → [1] → NULL
```

---

# 🧠 Linked List Pattern

Reverse linked list ka main concept:

```text
Current node ka arrow
        ↓
Previous node ki taraf
```

Normal:

```text
1 → 2 → 3 → 4
```

Reverse:

```text
1 ← 2 ← 3 ← 4
```

Then new head:

```text
4 → 3 → 2 → 1
```

---

# 🔥 Why Only O(1) Extra Space?

Humne koi new linked list nahi banayi.

Sirf:

```text
prev
curr
next
```

3 pointers use kiye.

So:

```text
Space = O(1)
```

Time:

```text
Har node exactly ek baar visit
```

Therefore:

```text
Time = O(n)
```

---

# ⏱️ Complexity

### Time Complexity

```text
O(n)
```

`n` nodes hain aur har node ek baar process hota hai.

### Space Complexity

```text
O(1)
```

Extra linked list nahi banayi.

---

# ⭐ Quick Revision

```text
Original:

[1] → [2] → [3] → [4] → NULL
```

Use:

```text
prev = NULL
curr = head
```

Then repeatedly:

```cpp
next = curr->next;
curr->next = prev;
prev = curr;
curr = next;
```

Finally:

```cpp
return prev;
```

### One-line memory trick:

> **Next ko save karo → current ka arrow previous ki taraf karo → prev aur curr dono ko aage move karo.**

---

# 📁 Linked List Folder

```text
Linked-List/
│
├── 01-Convert-Binary-Number-to-Integer.md
├── 02-Middle-of-the-Linked-List.md
└── 03-Reverse-Linked-List.md
```

### Git Commit

```text
Add LC 206 Reverse Linked List detailed notes and solution
```
