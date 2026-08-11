# 🔗 LeetCode 328 — Odd Even Linked List

## 📌 Problem

Given a singly linked list, rearrange the nodes such that:

1. All nodes at **odd positions** come first.
2. All nodes at **even positions** come after them.
3. The relative order within the odd and even groups should remain the same.

### Example

Input:

1 → 2 → 3 → 4 → 5 → NULL

Positions:

Position:  1   2   3   4   5
Value:     1   2   3   4   5
           O   E   O   E   O

Odd-position nodes:

1 → 3 → 5

Even-position nodes:

2 → 4

Final:

1 → 3 → 5 → 2 → 4 → NULL

---

# ⚠️ Important

Odd/Even ka matlab **value odd/even nahi hai**.

Ye question **position** ke basis par hai.

Example:

2 → 4 → 1 → 7 → 6

Positions:

1st   2nd   3rd   4th   5th
 ↓     ↓     ↓     ↓     ↓
 2     4     1     7     6

Odd positions:

2 → 1 → 6

Even positions:

4 → 7

Answer:

2 → 1 → 6 → 4 → 7

---

# 🧠 Approach

Hum linked list ko physically do separate lists me nahi tod rahe.

Hum existing nodes ke `next` pointers ko modify karke logically do chains banayenge:

Odd chain:

1 → 3 → 5

Even chain:

2 → 4

Then:

Odd chain → Even chain

Final:

1 → 3 → 5 → 2 → 4

---

# 🔥 Three Pointers

Hum 3 pointers use karenge:

```cpp
ListNode* odd;
ListNode* even;
ListNode* evenHead;
