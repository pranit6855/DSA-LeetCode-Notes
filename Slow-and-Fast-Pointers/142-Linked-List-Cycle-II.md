# LeetCode 142 - Linked List Cycle II

## 📌 Problem

Hume ek **Singly Linked List** di gayi hai.

Agar linked list me cycle present hai, to **cycle jis node se start hoti hai us node ko return karna hai.**

Agar cycle nahi hai,

Return:

```text
NULL
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
Node 2
```

Kyunki cycle node **2** se start ho rahi hai.

---

# 🔹 Example 2

```text
1 → 2 → 3 → NULL
```

Output

```text
NULL
```

Kyunki cycle present hi nahi hai.

---

# 🧠 Main Idea

Ye question **LC 141** ka extension hai.

Sabse pehle hume check karna hai ki cycle present hai ya nahi.

Uske baad cycle kis node se start ho rahi hai wo find karna hai.

Is question ko 2 parts me solve karte hain.

---

# 🔥 Part 1 - Detect Cycle

Hum Slow & Fast Pointer use karenge.

```text
Slow → 1 step

Fast → 2 steps
```

Agar

```text
Slow == Fast
```

Matlab cycle present hai.

Agar

```text
Fast == NULL

OR

Fast->next == NULL
```

Matlab cycle present nahi hai.

Return

```text
NULL
```

---

# 🔥 Part 2 - Find Starting Node

Jab

```text
Slow == Fast
```

ho jaye,

Ek naya pointer banayenge.

```cpp
ListNode *temp = head;
```

Ab:

```text
temp → 1 step

slow → 1 step
```

Dono ko ek-ek step move karenge.

Jahan dono milenge,

Wahi cycle ka starting node hoga.

---

# 🔥 Why temp = head ?

Meeting point cycle ke andar hota hai.

Floyd Algorithm prove karta hai ki:

```text
Head se ek pointer

+

Meeting point se ek pointer
```

Agar dono

```text
1 step
```

move kare,

To dono cycle ke starting node par hi milte hain.

Isliye:

```cpp
ListNode *temp = head;
```

banate hain.

---

# 🔥 Variables

```cpp
ListNode *slow = head;

ListNode *fast = head;
```

Meaning

```text
Slow → 1 step

Fast → 2 steps
```

---

# 🔥 Step 1 - Detect Cycle

```cpp
while(fast != NULL && fast->next != NULL)
```

Loop tab tak chalega jab tak Fast safely 2 steps move kar sake.

---

# 🔥 Step 2 - Move Pointers

```cpp
slow = slow->next;

fast = fast->next->next;
```

---

# 🔥 Step 3 - Meeting Point

```cpp
if(slow == fast)
```

Agar dono mil gaye,

Matlab cycle present hai.

---

# 🔥 Step 4 - New Pointer

```cpp
ListNode *temp = head;
```

Ek pointer head se start kar diya.

---

# 🔥 Step 5 - Move Both

```cpp
while(temp != slow){

    temp = temp->next;

    slow = slow->next;
}
```

Dono ko sirf

```text
1 step
```

move karna hai.

Jab

```text
temp == slow
```

ho jayega,

Matlab cycle ka starting node mil gaya.

---

# 🔥 Step 6 - Return Answer

```cpp
return temp;
```

Kyuki

```text
temp

==

slow
```

Aur dono cycle ke starting node par khade hain.

---

# 💻 C++ Code

```cpp
class Solution {
public:

    ListNode *detectCycle(ListNode *head) {

        ListNode *slow = head;
        ListNode *fast = head;

        while(fast != NULL && fast->next != NULL){

            slow = slow->next;
            fast = fast->next->next;

            if(slow == fast){

                ListNode *temp = head;

                while(temp != slow){

                    temp = temp->next;
                    slow = slow->next;
                }

                return temp;
            }
        }

        return NULL;
    }
};
```

---

# 🔥 Most Important Concept

Question do parts me solve hota hai.

Part 1

```text
Detect Cycle
```

Part 2

```text
Find Starting Node
```

Remember

```text
Meet

↓

temp = head

↓

temp & slow

1 step

↓

Meet Again

↓

Cycle Start
```

---

# 🔥 Flow

```text
Head
 ↓
slow = head
fast = head
 ↓
Slow = 1 step
Fast = 2 steps
 ↓
Slow == Fast ?
 ↓ NO
Repeat
 ↓ YES
Cycle Present
 ↓
temp = head
 ↓
temp = temp->next
slow = slow->next
 ↓
temp == slow ?
 ↓ NO
Repeat
 ↓ YES
Return temp
```

---

# ⏱️ Time Complexity

```text
O(n)
```

Ek traversal me cycle detect hoti hai aur ek traversal me starting node mil jati hai.

---

# 💾 Space Complexity

```text
O(1)
```

Extra memory use nahi hoti.

---

# ⚠️ Common Mistakes

### 1.

Meeting ke baad direct

```cpp
return slow;
```

Wrong.

Pehle

```cpp
temp = head;
```

banana padega.

---

### 2.

Wrong

```cpp
temp = temp->next->next;
```

Correct

```cpp
temp = temp->next;
```

Dono pointers sirf

```text
1 step
```

move karenge.

---

### 3.

Meeting ke baad

```cpp
fast
```

ko move karna.

Wrong.

Correct

```cpp
temp
```

aur

```cpp
slow
```

move honge.

---

### 4.

Cycle detect na hone par

```cpp
return head;
```

Wrong.

Correct

```cpp
return NULL;
```

---

# 🔥 Quick Revision

```text
Slow = 1 step
      ↓
Fast = 2 steps
      ↓
Meet ?
      ↓ YES
temp = head
      ↓
temp & slow
1 step
      ↓
Meet Again
      ↓
Return Cycle Start
```

---

# 🧠 One-Line Revision

Pehle Floyd Algorithm se cycle detect karo. Meeting ke baad ek pointer head se aur ek meeting point se 1-1 step move karo. Jahan dono milenge wahi cycle ka starting node hoga.

---

# ⭐ Interview Revision

```text
Detect Cycle

↓

Slow == Fast

↓

temp = head

↓

temp & slow

1 step

↓

Meeting Point

↓

Return Cycle Start
```

Pattern

```text
Slow & Fast Pointer

↓

Floyd Cycle Detection

↓

Find Cycle Start
```
