# LeetCode 876 - Middle of the Linked List

## 📌 Problem

Hume ek **Singly Linked List** di gayi hai.

Hume uska **middle node** return karna hai.

Agar linked list me **2 middle nodes** ho (even length), to **second middle node** return karna hai.

---

# 🔹 Example 1

```text
1 → 2 → 3 → 4 → 5
```

Output:

```text
3
```

---

# 🔹 Example 2

```text
1 → 2 → 3 → 4 → 5 → 6
```

Output:

```text
4
```

Kyunki LeetCode second middle return karne ko bolta hai.

---

# 🧠 Main Idea

Hum **Slow & Fast Pointer** technique use karenge.

Rule:

```text
Slow → 1 step move karega

Fast → 2 steps move karega
```

Jab:

```text
Fast list ke end tak pahunch jayega
```

Tab:

```text
Slow automatically middle node par hoga.
```

---

# 🔥 Why Slow & Fast Pointer?

Brute Force me:

```text
1. Total nodes count karo.

2. count/2 nikal kar fir head se wapas move karo.
```

2 traversals lagenge.

Slow & Fast Pointer me:

```text
Sirf ek traversal me middle mil jata hai.
```

---

# 🔥 Variables

```cpp
ListNode* slow = head;
ListNode* fast = head;
```

Meaning:

```text
slow → 1 step move karega

fast → 2 steps move karega
```

Initially:

```text
head
 ↓

1 → 2 → 3 → 4 → 5

S
F
```

---

# 🔥 Step 1 - Move Slow

```cpp
slow = slow->next;
```

Slow har iteration me:

```text
1 step
```

move karega.

---

# 🔥 Step 2 - Move Fast

```cpp
fast = fast->next->next;
```

Fast har iteration me:

```text
2 steps
```

move karega.

---

# 🔥 Step 3 - Stop Condition

```cpp
while(fast != NULL && fast->next != NULL)
```

Loop tab tak chalega jab tak Fast safely 2 steps move kar sake.

---

# 🔄 Dry Run

Given:

```text
1 → 2 → 3 → 4 → 5
```

Initially:

```text
S = 1
F = 1
```

↓

Iteration 1

```text
S = 2
F = 3
```

↓

Iteration 2

```text
S = 3
F = 5
```

↓

Fast end par pahunch gaya.

Loop stop.

Answer:

```text
3
```

---

# 🔄 Even Case

```text
1 → 2 → 3 → 4 → 5 → 6
```

Initially:

```text
S = 1
F = 1
```

↓

```text
S = 2
F = 3
```

↓

```text
S = 3
F = 5
```

↓

```text
S = 4
F = NULL
```

Answer:

```text
4
```

Automatically second middle return hota hai.

---

# 🔥 First Middle vs Second Middle

## Second Middle

```cpp
slow = head;
fast = head;
```

Answer:

```text
4
```

---

## First Middle

```cpp
slow = head;
fast = head->next;
```

Answer:

```text
3
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {

        ListNode* slow = head;
        ListNode* fast = head;

        while(fast != NULL && fast->next != NULL){

            slow = slow->next;
            fast = fast->next->next;
        }

        return slow;
    }
};
```

---

# 🔥 Most Important Concept

Fast pointer:

```text
2x speed
```

Slow pointer:

```text
1x speed
```

Isliye:

```text
Fast End

↓

Slow Middle
```

---

# 🧠 Why While Condition?

Wrong:

```cpp
while(fast != NULL)
```

Runtime Error aa sakta hai.

Correct:

```cpp
while(fast != NULL && fast->next != NULL)
```

Kyuki Fast ko:

```cpp
fast = fast->next->next;
```

move karna hai.

---

# 🔥 Flow

```text
head
 ↓
slow = head
fast = head
 ↓
Slow = 1 step
 ↓
Fast = 2 steps
 ↓
Fast end?
 ↓ NO
Repeat
 ↓
YES
 ↓
Return slow
```

---

# ⏱️ Time Complexity

```text
O(n)
```

Sirf ek traversal.

---

# 💾 Space Complexity

```text
O(1)
```

Extra memory use nahi hoti.

---

# ⚠️ Common Mistakes

### 1.

Wrong:

```cpp
fast = fast->next;
```

Correct:

```cpp
fast = fast->next->next;
```

---

### 2.

Wrong:

```cpp
slow = slow->next->next;
```

Correct:

```cpp
slow = slow->next;
```

---

### 3.

Wrong:

```cpp
while(fast != NULL)
```

Correct:

```cpp
while(fast != NULL && fast->next != NULL)
```

---

### 4.

Confusing first and second middle.

```text
fast = head
```

↓

Second Middle

```text
fast = head->next
```

↓

First Middle

---

# 🔥 Quick Revision

```text
Slow = 1 step
      ↓
Fast = 2 steps
      ↓
Fast reaches end
      ↓
Slow reaches middle
      ↓
Return slow
```

---

# 🧠 One-Line Revision

Fast pointer 2 steps aur Slow pointer 1 step move karta hai. Jab Fast linked list ke end tak pahunchta hai, Slow middle node par hota hai.

---

# ⭐ Interview Revision

```text
Brute Force:
Count nodes → Move again

Optimal:
Slow = 1 step
Fast = 2 steps

↓

Single Traversal

↓

Middle Node
```

Pattern:

```text
Slow & Fast Pointer

↓

Middle Node

↓

One Traversal
```
