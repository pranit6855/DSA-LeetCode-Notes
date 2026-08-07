# LeetCode 141 - Linked List Cycle

## 📌 Problem

Hume ek **Singly Linked List** di gayi hai.

Hume check karna hai ki linked list me **cycle** present hai ya nahi.

Return:

```text
true  → Agar cycle present hai

false → Agar cycle present nahi hai
```

---

# 🔹 Example 1

```text
1 → 2 → 3 → 4
      ↑     ↓
      ← ← ←
```

Output

```text
true
```

Reason:

Last node NULL ko point nahi kar rahi.

Wo wapas kisi previous node ko point kar rahi hai.

---

# 🔹 Example 2

```text
1 → 2 → 3 → 4 → NULL
```

Output

```text
false
```

Reason:

Last node NULL ko point kar rahi hai.

---

# 🧠 Main Idea

Hum **Slow & Fast Pointer** technique use karenge.

Rule:

```text
Slow → 1 step move karega

Fast → 2 steps move karega
```

Ab do cases ho sakte hain.

### Case 1

Agar cycle nahi hogi

```text
Fast

↓

NULL
```

tak pahunch jayega.

Answer:

```text
false
```

---

### Case 2

Agar cycle hogi

```text
Fast

2 steps

↓

Slow

1 step
```

Fast pointer eventually Slow pointer ko catch kar lega.

Answer:

```text
true
```

---

# 🔥 Why Slow & Fast Pointer?

Imagine circular race track.

```text
🏃 Runner

🏍️ Bike
```

Runner:

```text
1 step
```

Bike:

```text
2 steps
```

Bike hamesha runner ko ek time baad pakad legi.

Exactly wahi Linked List cycle me hota hai.

---

# 🔥 Variables

```cpp
ListNode* slow = head;
ListNode* fast = head;
```

Meaning:

```text
slow → 1 step

fast → 2 steps
```

Initially

```text
head
 ↓

1 → 2 → 3 → 4

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

# 🔥 Step 3 - Check Meeting

```cpp
if(slow == fast)
```

Agar dono same node par aa gaye,

Matlab:

```text
Cycle Present
```

Immediately:

```cpp
return true;
```

---

# 🔥 Step 4 - Fast Reaches NULL

Agar

```text
Fast == NULL

OR

Fast->next == NULL
```

ho gaya,

Matlab list end ho gayi.

Cycle nahi hai.

Return:

```cpp
false;
```

---

# 🔄 Dry Run (No Cycle)

```text
1 → 2 → 3 → 4 → NULL
```

Initially

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

F = NULL
```

Fast end tak pahunch gaya.

Answer

```text
false
```

---

# 🔄 Dry Run (Cycle)

```text
1 → 2 → 3 → 4
      ↑     ↓
      ← ← ←
```

Initially

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

F = 2
```

↓

Iteration 3

```text
S = 4

F = 4
```

Dekho

```text
Slow == Fast
```

Answer

```text
true
```

---

# 💻 C++ Code

```cpp
class Solution {
public:

    bool hasCycle(ListNode *head) {

        ListNode *slow = head;
        ListNode *fast = head;

        while(fast != NULL && fast->next != NULL){

            slow = slow->next;
            fast = fast->next->next;

            if(slow == fast){
                return true;
            }
        }

        return false;
    }
};
```

---

# 🔥 Most Important Concept

Do cases:

```text
Fast reaches NULL

↓

No Cycle
```

OR

```text
Fast meets Slow

↓

Cycle Present
```

Yehi pura question ka logic hai.

---

# 🧠 Why While Condition?

Wrong

```cpp
while(fast != NULL)
```

Correct

```cpp
while(fast != NULL && fast->next != NULL)
```

Reason:

Fast pointer:

```cpp
fast = fast->next->next;
```

move karega.

Agar:

```cpp
fast->next == NULL
```

hua,

to Runtime Error aa jayega.

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
Slow == Fast ?
 ↓ YES
Return true
 ↓ NO
Fast NULL ?
 ↓ YES
Return false
 ↓ NO
Repeat
```

---

# ⏱️ Time Complexity

```text
O(n)
```

Har node maximum limited baar visit hota hai.

---

# 💾 Space Complexity

```text
O(1)
```

Extra memory use nahi hoti.

---

# ⚠️ Common Mistakes

### 1.

Wrong

```cpp
while(fast != NULL)
```

Correct

```cpp
while(fast != NULL && fast->next != NULL)
```

---

### 2.

Meeting check pehle karna.

Wrong

```cpp
if(slow == fast)
```

before moving.

Initially dono head par hi hote hain.

Isliye pehle move karo.

Correct

```cpp
slow = slow->next;
fast = fast->next->next;

if(slow == fast)
```

---

### 3.

Fast ko 1 step move karna.

Wrong

```cpp
fast = fast->next;
```

Correct

```cpp
fast = fast->next->next;
```

---

# 🔥 Quick Revision

```text
Slow = 1 step
      ↓
Fast = 2 steps
      ↓
Fast reaches NULL ?
      ↓ YES
No Cycle
      ↓
OR
      ↓
Fast meets Slow
      ↓
Cycle Present
```

---

# 🧠 One-Line Revision

Fast pointer 2 steps aur Slow pointer 1 step move karta hai. Agar cycle hogi to Fast pointer Slow ko zarur catch karega, warna Fast NULL tak pahunch jayega.

---

# ⭐ Interview Revision

```text
No Cycle

↓

Fast → NULL

↓

Return false
```

```text
Cycle

↓

Fast == Slow

↓

Return true
```

Pattern

```text
Slow & Fast Pointer

↓

Cycle Detection

↓

Meeting Point

↓

True / False
```
