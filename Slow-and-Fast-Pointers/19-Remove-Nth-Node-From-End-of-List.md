# LeetCode 19 - Remove Nth Node From End of List

## 📌 Problem

Hume ek **Singly Linked List** aur ek integer `n` diya gaya hai.

Hume **end se nth node delete** karni hai.

Aur modified linked list return karni hai.

---

# 🔹 Example

```text
Head

1 → 2 → 3 → 4 → 5

n = 2
```

End se

```text
5 = 1st

4 = 2nd
```

Delete

```text
4
```

Output

```text
1 → 2 → 3 → 5
```

---

# 🧠 Main Idea

Delete karne ke liye hume

```text
Previous Node
```

chahiye.

Suppose

```text
3 → 4 → 5
```

Delete

```text
4
```

Tab hi delete kar sakte hain jab

```text
3
```

par khade ho.

Kyuki

```cpp
3->next = 5;
```

karna hota hai.

---

# 🔥 Why Dummy Node?

Suppose

```text
1 → 2 → 3
```

Aur delete karna hai

```text
1
```

Problem

```text
1
```

ka previous node hi nahi hai.

Isliye ek fake node bana dete hain.

```text
Dummy → 1 → 2 → 3
```

Ab

```text
1
```

ka previous

```text
Dummy
```

hai.

Ab delete karna easy ho gaya.

---

# 🔥 Step 1 - Create Dummy

```cpp
ListNode *dummy = new ListNode(0);

dummy->next = head;
```

Ab list

```text
Dummy → 1 → 2 → 3 → 4 → 5
```

---

# 🔥 Step 2 - Create Two Pointers

```cpp
ListNode *slow = dummy;

ListNode *fast = dummy;
```

Initially

```text
S
F

Dummy → 1 → 2 → 3 → 4 → 5
```

---

# 🔥 Step 3 - Move Fast

Dummy use kiya hai.

Isliye

```text
Fast = n+1 steps
```

move karega.

Code

```cpp
for(int i=0; i<=n; i++){

    fast = fast->next;
}
```

Example

```text
Dummy → 1 → 2 → 3 → 4 → 5

n = 2
```

Move

```text
Step 1

Dummy → 1 → 2 → 3 → 4 → 5

    F

Step 2

Dummy → 1 → 2 → 3 → 4 → 5

        F

Step 3

Dummy → 1 → 2 → 3 → 4 → 5

            F
```

---

# 🔥 Step 4 - Move Both

Ab

```cpp
while(fast != NULL)
```

Dono pointers

```text
1 step
```

move karenge.

```cpp
slow = slow->next;

fast = fast->next;
```

---

# 🔄 Dry Run

Initially

```text
S
F

Dummy → 1 → 2 → 3 → 4 → 5
```

Fast ko

```text
3 steps
```

move kiya.

```text
S

Dummy → 1 → 2 → 3 → 4 → 5

            F
```

Ab dono move.

↓

```text
Dummy → 1 → 2 → 3 → 4 → 5

    S           F
```

↓

```text
Dummy → 1 → 2 → 3 → 4 → 5

        S           F
```

↓

```text
Dummy → 1 → 2 → 3 → 4 → 5

            S           F
```

↓

```text
Dummy → 1 → 2 → 3 → 4 → 5

                S           NULL
```

Stop.

Dekho

```text
Slow
```

node

```text
3
```

par hai.

Aur delete karna hai

```text
4
```

Exactly previous node mil gaya.

---

# 🔥 Step 5 - Delete Node

```cpp
slow->next = slow->next->next;
```

Before

```text
3 → 4 → 5
```

After

```text
3 ─────→ 5
```

Node

```text
4
```

list se remove ho gayi.

---

# 🔥 Step 6 - Return Answer

```cpp
return dummy->next;
```

Dummy fake node tha.

Original linked list return kar di.

---

# 💻 C++ Code

```cpp
class Solution {
public:

    ListNode* removeNthFromEnd(ListNode* head, int n) {

        ListNode *dummy = new ListNode(0);

        dummy->next = head;

        ListNode *slow = dummy;
        ListNode *fast = dummy;

        for(int i=0; i<=n; i++){

            fast = fast->next;
        }

        while(fast != NULL){

            slow = slow->next;
            fast = fast->next;
        }

        slow->next = slow->next->next;

        return dummy->next;
    }
};
```

---

# 🔥 Most Important Concept

Dummy node sirf

```text
Head Delete
```

wale edge case ko normal banata hai.

Aur

```text
Fast = n+1 steps
```

isliye move karte hain taaki

```text
Slow
```

delete hone wali node ke **just previous** par ruk jaye.

---

# 🔥 Flow

```text
Create Dummy

↓

Dummy → Head

↓

Slow = Dummy

↓

Fast = Dummy

↓

Fast = n+1 Steps

↓

Move Slow & Fast

↓

Fast == NULL

↓

Slow Previous Node

↓

slow->next = slow->next->next

↓

Return dummy->next
```

---

# ⏱️ Time Complexity

```text
O(n)
```

---

# 💾 Space Complexity

```text
O(1)
```

---

# ⚠️ Common Mistakes

### 1.

Wrong

```cpp
for(int i=0; i<n; i++)
```

Correct

```cpp
for(int i=0; i<=n; i++)
```

Dummy use kiya hai.

Isliye

```text
n+1
```

steps.

---

### 2.

Wrong

```cpp
return head;
```

Correct

```cpp
return dummy->next;
```

---

### 3.

Dummy node na banana.

Head delete wale test case fail ho jayenge.

---

### 4.

Wrong

```cpp
slow = slow->next->next;
```

Correct

```cpp
slow->next = slow->next->next;
```

---

# 🔥 Quick Revision

```text
Dummy

↓

Slow = Dummy

↓

Fast = Dummy

↓

Fast n+1 Steps

↓

Move Both

↓

Slow Previous Node

↓

Delete

↓

Return dummy->next
```

---

# 🧠 One-Line Revision

Dummy node banao, Fast ko `n+1` steps aage bhejo, phir Slow aur Fast ko saath move karo. Jab Fast `NULL` par pahunch jaye, Slow delete hone wali node ke previous par hoga.

---

# ⭐ Interview Revision

```text
Dummy

↓

Fast = n+1

↓

Move Both

↓

Slow Previous

↓

Delete Node

↓

Return dummy->next
```

Pattern

```text
Slow & Fast Pointer

↓

Two Pointer Gap

↓

Delete Node
```
