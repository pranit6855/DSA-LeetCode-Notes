# 🔗 LeetCode 142 — Linked List Cycle II

## 📌 Problem

Hume ek singly linked list di gayi hai.

Hume check karna hai ki list me **cycle hai ya nahi**.

Agar cycle hai, to hume **cycle ka starting node return** karna hai.

Agar cycle nahi hai:

```cpp
return NULL;
```

---

# 🔹 Example 1

```text
Input:

[3] → [2] → [0] → [-4]
       ↑             ↓
       └─────────────┘
```

Yahan:

```text
-4 → 2
```

isliye cycle hai.

Cycle:

```text
[2] → [0] → [-4]
 ↑             ↓
 └─────────────┘
```

Cycle ka starting node:

```text
[2]
```

So:

```text
Output = Node containing 2
```

---

# 🔹 Example 2

```text
[1] → [2] → NULL
```

Koi cycle nahi.

So:

```text
Output = NULL
```

---

# 🔹 Example 3

```text
[1] → [1]
 ↑     ↓
 └─────┘
```

Second node ka `next` first node ko point karta hai.

Cycle start:

```text
first [1]
```

---

# 🧠 Main Idea

Ye question **LC 141 — Linked List Cycle** ka extension hai.

LC 141 me hum sirf ye find karte the:

```text
Cycle hai?
YES / NO
```

LC 142 me:

```text
Cycle hai?
       ↓
Cycle kaha se start ho rahi hai?
```

Isliye yahan **2 phases** honge.

---

# 🔥 Two Phase Approach

```text
PHASE 1
↓
Cycle detect karo

PHASE 2
↓
Cycle ka starting node find karo
```

---

# 🟢 PHASE 1 — Cycle Detect

Slow & Fast pointer use karenge.

```cpp
ListNode* slow = head;
ListNode* fast = head;
```

Movement:

```text
slow → 1 step
fast → 2 steps
```

So:

```cpp
slow = slow->next;
fast = fast->next->next;
```

---

# 🔄 Phase 1 Example

Suppose:

```text
[3] → [2] → [0] → [-4]
       ↑             ↓
       └─────────────┘
```

Initially:

```text
slow = 3
fast = 3
```

---

### First Move

```text
slow → 2
fast → 0
```

---

### Second Move

```text
slow → 0
fast → 2
```

---

### Third Move

```text
slow → -4
fast → -4
```

Now:

```cpp
slow == fast
```

Cycle confirm.

---

# ⚠️ Important

First meeting point **cycle ka starting node zaroori nahi hai**.

Ye bahut important hai.

Suppose:

```text
[3] → [2] → [0] → [-4]
       ↑             ↓
       └─────────────┘
```

Cycle start:

```text
[2]
```

Lekin slow aur fast cycle ke andar kisi aur node par mil sakte hain.

Therefore:

```cpp
if(slow == fast)
```

ka matlab sirf:

```text
Cycle exists
```

hai.

**Abhi answer return nahi karna.**

---

# 🟡 PHASE 2 — Cycle Start Find

Jab:

```cpp
slow == fast
```

ho jaye:

```cpp
slow = head;
```

kar do.

Fast ko wahi meeting point par rehne do.

Now:

```text
slow → head
fast → meeting point
```

---

# 🔥 Ab Dono Same Speed Se Chalenge

Phase 1 me:

```text
slow → 1 step
fast → 2 steps
```

Phase 2 me:

```text
slow → 1 step
fast → 1 step
```

Code:

```cpp
slow = slow->next;
fast = fast->next;
```

Jab:

```cpp
slow == fast
```

dobara hoga:

```text
That node = cycle starting node
```

---

# 🤔 Phase 2 Kyu Kaam Karta Hai?

Isko distance se samjho.

List ko 3 parts me imagine karo:

```text
HEAD
 ↓
A → B → C → D → E
         ↑       ↓
         └───────┘
```

Cycle start:

```text
C
```

Maan lo:

```text
a = head se cycle start tak distance
```

```text
b = cycle start se meeting point tak distance
```

```text
L = cycle ki total length
```

---

# 🧮 Phase 1 Me Kya Hua?

Slow ne meeting point tak:

```text
a + b
```

distance travel ki.

Fast twice speed se gaya:

```text
2(a + b)
```

Fast aur slow ke travel ka difference:

```text
2(a + b) - (a + b)

= a + b
```

Ye difference cycle ke complete rounds ka multiple hota hai.

Therefore:

```text
a + b = kL
```

So:

```text
a = kL - b
```

Iska meaning:

> Head se cycle start tak jitna distance hai, meeting point se cycle start tak utna hi effective distance cycle ke andar remaining hai, modulo cycle length.

Isliye agar:

```text
slow = head
fast = meeting point
```

aur dono same speed se chale:

```text
slow → 1 step
fast → 1 step
```

to dono **cycle start** par milenge.

---

# 🔄 Phase 2 Visual

Suppose first meeting:

```text
[3] → [2] → [0] → [-4]
       ↑             ↓
       └─────────────┘
              ↑
           meeting
```

Now:

```text
slow = head
```

So:

```text
slow
 ↓
[3] → [2] → [0] → [-4]
       ↑             ↓
       └─────────────┘
                     ↑
                    fast
```

Ab dono 1 step:

```text
slow → [2]
fast → [2]
```

So:

```text
slow == fast
```

and:

```text
[2]
```

is the cycle start.

---

# 🧩 Complete Algorithm

## Step 1

Initialize:

```cpp
ListNode* slow = head;
ListNode* fast = head;
```

---

## Step 2

Cycle detect karo:

```cpp
while(fast != NULL && fast->next != NULL)
```

---

## Step 3

Pointers move:

```cpp
slow = slow->next;
fast = fast->next->next;
```

---

## Step 4

Agar:

```cpp
slow == fast
```

to cycle mil gayi.

---

## Step 5

Slow ko head par reset:

```cpp
slow = head;
```

---

## Step 6

Dono ko 1-1 step move:

```cpp
while(slow != fast) {
    slow = slow->next;
    fast = fast->next;
}
```

---

## Step 7

Jahan milenge:

```cpp
return slow;
```

---

## Step 8

Agar Phase 1 me fast NULL ho gaya:

```cpp
return NULL;
```

---

# 💻 Final C++ Code

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {

        ListNode* slow = head;
        ListNode* fast = head;

        // Phase 1: Detect cycle
        while(fast != NULL && fast->next != NULL) {

            slow = slow->next;
            fast = fast->next->next;

            if(slow == fast) {

                // Phase 2: Find cycle start
                slow = head;

                while(slow != fast) {

                    slow = slow->next;
                    fast = fast->next;
                }

                return slow;
            }
        }

        // No cycle
        return NULL;
    }
};
```

---

# 🔍 Code Line-by-Line

## 1. Slow

```cpp
ListNode* slow = head;
```

Slow head se start.

---

## 2. Fast

```cpp
ListNode* fast = head;
```

Fast bhi head se start.

---

## 3. Safe Loop

```cpp
while(fast != NULL && fast->next != NULL)
```

Fast ko:

```cpp
fast->next->next
```

karna hai.

Isliye:

```text
fast != NULL
```

aur:

```text
fast->next != NULL
```

dono required hain.

---

## 4. Move Slow

```cpp
slow = slow->next;
```

One step.

---

## 5. Move Fast

```cpp
fast = fast->next->next;
```

Two steps.

---

## 6. Meeting Check

```cpp
if(slow == fast)
```

Agar dono same node ko point kar rahe hain:

```text
Cycle exists
```

---

## 7. Slow Reset

```cpp
slow = head;
```

Ab Phase 2 start.

---

## 8. Same Speed

```cpp
while(slow != fast)
```

Dono:

```text
1 step
```

move karenge.

---

## 9. Move

```cpp
slow = slow->next;
fast = fast->next;
```

---

## 10. Return

Loop tab stop hoga jab:

```text
slow == fast
```

Ye node:

```text
Cycle ka starting node
```

hai.

So:

```cpp
return slow;
```

---

# ⚠️ Common Mistake #1

## First Meeting Point Ko Return Karna

Wrong:

```cpp
if(slow == fast) {
    return slow;
}
```

Ye **LC 142 me wrong** hai.

Kyunki first meeting point sirf cycle ke andar koi node hai.

Cycle ka start zaroori nahi.

Correct:

```cpp
if(slow == fast) {

    slow = head;

    while(slow != fast) {
        slow = slow->next;
        fast = fast->next;
    }

    return slow;
}
```

---

# ⚠️ Common Mistake #2

## Phase 2 Me Fast Ko 2 Steps Karna

Wrong:

```cpp
fast = fast->next->next;
```

Phase 2 me.

Correct:

```cpp
fast = fast->next;
```

Because Phase 2 me:

```text
slow → 1
fast → 1
```

---

# ⚠️ Common Mistake #3

## Phase 2 Me `fast = head` Karna

Wrong:

```cpp
fast = head;
```

Hum sirf:

```cpp
slow = head;
```

reset karte hain.

Fast ko **first meeting point** par rehne dete hain.

```text
slow → head
fast → meeting point
```

---

# ⚠️ Common Mistake #4

## Starting Me `slow == fast` Check Karna

Initially:

```cpp
slow = head;
fast = head;
```

So:

```text
slow == fast
```

already true hai.

Isliye comparison **move ke baad** karna hai.

---

# 🔥 LC 141 vs LC 142

Ye dono questions ko confuse mat karna.

## LC 141 — Linked List Cycle

Question:

```text
Cycle hai ya nahi?
```

Logic:

```cpp
if(slow == fast)
    return true;
```

---

## LC 142 — Linked List Cycle II

Question:

```text
Cycle start kaha hai?
```

Logic:

```text
Phase 1:
slow == fast
       ↓
cycle detected

Phase 2:
slow = head

both 1 step
       ↓
slow == fast
       ↓
cycle start
```

---

# 🔥 LC 876 vs LC 141 vs LC 142

### LC 876 — Middle

```text
slow → 1
fast → 2

loop end
↓
return slow
```

### LC 141 — Cycle

```text
slow → 1
fast → 2

slow == fast
↓
return true
```

### LC 142 — Cycle Start

```text
slow → 1
fast → 2

slow == fast
↓
slow = head
↓
both → 1 step
↓
slow == fast
↓
return slow
```

### ⭐ Ye teen patterns ek saath yaad rakho:

```text
876 → slow = middle

141 → slow == fast = cycle

142 → first meeting
      ↓
      reset slow to head
      ↓
      both 1 step
      ↓
      second meeting = cycle start
```

---

# 🔥 Edge Case 1 — Empty List

```text
head = NULL
```

Then:

```text
slow = NULL
fast = NULL
```

Loop chalega hi nahi.

Return:

```cpp
NULL
```

Correct.

---

# 🔥 Edge Case 2 — Single Node, No Cycle

```text
[1] → NULL
```

```text
fast->next == NULL
```

Loop nahi chalega.

Return:

```cpp
NULL
```

---

# 🔥 Edge Case 3 — Single Node Self Cycle

```text
     ┌──────┐
     ↓      │
    [1] ────┘
```

Initially:

```text
slow = 1
fast = 1
```

First movement:

```text
slow = 1
fast = 1
```

Meeting.

Then:

```cpp
slow = head;
```

Dono same node par already hain.

```text
slow == fast
```

So return:

```text
Node [1]
```

Correct.

---

# 🔥 Edge Case 4 — Cycle Starts at Head

```text
      ┌───────────────┐
      ↓               │
[1] → [2] → [3] → [4]─┘
```

Cycle start:

```text
[1]
```

Phase 1 cycle detect karega.

Phase 2:

```text
slow = head
```

Aur meeting point se fast move karega.

Eventually:

```text
slow == fast == [1]
```

So `[1]` return.

---

# 🧠 Dry Run Table

Example:

```text
[3] → [2] → [0] → [-4]
       ↑             ↓
       └─────────────┘
```

### Phase 1

| Step | Slow | Fast |
|---|---|---|
| Start | 3 | 3 |
| 1 | 2 | 0 |
| 2 | 0 | 2 |
| 3 | -4 | -4 |

Meeting:

```text
-4
```

**But -4 answer nahi hai.**

---

### Phase 2

Reset:

```text
slow = 3
fast = -4
```

Then both 1 step:

| Step | Slow | Fast |
|---|---|---|
| Start | 3 | -4 |
| 1 | 2 | 2 |

Meeting:

```text
2
```

Therefore:

```text
Cycle Start = 2
```

---

# 🧠 Why We Don't Need Extra Space?

Alternative approach:

```text
HashSet
```

Use karke visited nodes store kar sakte the.

But usme:

```text
O(n)
```

extra space lagta.

Slow & Fast:

```text
slow
fast
```

sirf 2 pointers use karte hain.

Therefore:

```text
Space = O(1)
```

---

# ⏱️ Complexity

### Time

```text
O(n)
```

Worst case me list/cycle ko linear number of steps me process karte hain.

### Space

```text
O(1)
```

Sirf two pointers use hote hain.

---

# 🔥 Pattern Recognition

Agar question bole:

```text
Linked List
+
Cycle
+
Find starting node
```

Immediately think:

```text
Floyd's Cycle Detection
```

### Pattern:

```text
PHASE 1
slow = 1 step
fast = 2 steps

        ↓

slow == fast
        ↓
cycle found
```

Then:

```text
PHASE 2
slow = head

slow = 1 step
fast = 1 step

        ↓

slow == fast
        ↓
cycle start
```

---

# ⭐ Quick Revision

```text
slow = head
fast = head
```

### Phase 1:

```cpp
while(fast != NULL && fast->next != NULL) {

    slow = slow->next;
    fast = fast->next->next;

    if(slow == fast) {
        break;
    }
}
```

### Phase 2:

```cpp
slow = head;

while(slow != fast) {

    slow = slow->next;
    fast = fast->next;
}
```

Finally:

```cpp
return slow;
```

If Phase 1 never finds a meeting:

```cpp
return NULL;
```

---

# ⭐ One-Line Memory Trick

> **Pehle slow 1 aur fast 2 se meeting point find karo; meeting ke baad slow ko head par reset karo aur dono ko 1-1 step chalao — jahan dobara milenge wahi cycle ka starting node hai.**

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
└── 07-Linked-List-Cycle-II.md
```

### Git Commit

```text
Add LC 142 Linked List Cycle II
```
