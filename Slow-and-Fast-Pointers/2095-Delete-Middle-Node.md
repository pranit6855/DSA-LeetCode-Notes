# LeetCode 2095 - Delete the Middle Node of a Linked List

## 📌 Problem

Hume ek singly linked list di gayi hai.

Hume uska **middle node delete** karna hai.

### Example 1

```text
1 → 3 → 4 → 7 → 1 → 2 → 6
```

Total nodes:

```text
n = 7
```

Middle node:

```text
7
```

Delete karne ke baad:

```text
1 → 3 → 4 → 1 → 2 → 6
```

---

### Example 2

```text
1 → 2 → 3 → 4
```

`n = 4`

Middle node:

```text
3
```

Output:

```text
1 → 2 → 4
```

---

### Example 3

```text
2 → 1
```

Middle node:

```text
1
```

Output:

```text
2
```

---

# 🧠 Main Idea

Hum **Slow & Fast Pointer** use karenge.

```text
slow → 1 step
fast → 2 steps
```

Lekin ek important cheez:

Hume slow ko middle par nahi, **middle ke previous node** par rakhna hai.

Kyun?

Kyuki linked list me node ko directly delete nahi karte.

Agar:

```text
1 → 2 → 3 → 4
```

Aur `3` delete karna hai, to hume `2` chahiye:

```text
1 → 2 → 3 → 4
    ↑
  slow
```

Phir:

```cpp
slow->next = slow->next->next;
```

se:

```text
2 → 3 → 4
```

becomes:

```text
2 → 4
```

---

# 🔥 Pointers

Hum 3 pointers lenge:

```cpp
ListNode *slow = head;
ListNode *fast = head;
ListNode *prev = NULL;
```

### Meaning

```text
slow
 ↓
Current position

fast
 ↓
slow se 2x speed par chalega

prev
 ↓
slow ke previous node ko remember karega
```

---

# 🔥 Step 1 - Edge Cases

```cpp
if(head == NULL || head->next == NULL){
    return NULL;
}
```

### Empty list

```text
NULL
```

Middle node nahi hai.

Return:

```text
NULL
```

### One node

```text
1 → NULL
```

Wahi node middle hai.

Delete karne ke baad:

```text
NULL
```

So:

```cpp
return NULL;
```

---

# 🔥 Step 2 - Slow & Fast Start

```cpp
ListNode *slow = head;
ListNode *fast = head;
ListNode *prev = NULL;
```

Example:

```text
1 → 3 → 4 → 7 → 1 → 2 → 6
↑
slow
↑
fast
```

Initially:

```text
slow = 1
fast = 1
prev = NULL
```

---

# 🔥 Step 3 - Move Pointers

Loop:

```cpp
while(fast != NULL && fast->next != NULL){

    prev = slow;

    slow = slow->next;

    fast = fast->next->next;
}
```

Har iteration me:

```text
1. prev = slow
2. slow = slow->next
3. fast = fast->next->next
```

---

# 🔄 Dry Run - Odd Length

List:

```text
1 → 3 → 4 → 7 → 1 → 2 → 6
```

### Initially

```text
prev = NULL
slow = 1
fast = 1
```

---

### First iteration

Pehle:

```text
prev = slow
```

So:

```text
prev = 1
```

Then:

```text
slow = slow->next
```

So:

```text
slow = 3
```

Then fast 2 steps:

```text
fast = 4
```

Now:

```text
1 → 3 → 4 → 7 → 1 → 2 → 6
↑   ↑       ↑
prev slow  fast
```

---

### Second iteration

```text
prev = 3
slow = 4
fast = 1
```

---

### Third iteration

```text
prev = 4
slow = 7
fast = 6
```

Now:

```text
1 → 3 → 4 → 7 → 1 → 2 → 6
        ↑   ↑       ↑
       prev slow   fast
```

Check loop again:

```text
fast = 6
fast->next = NULL
```

Condition false.

Loop stops.

So:

```text
prev = 4
slow = 7
```

Exactly what we wanted.

---

# 🔥 Step 4 - Delete Middle

Ab:

```text
prev = 4
slow = 7
```

List:

```text
1 → 3 → 4 → 7 → 1 → 2 → 6
        ↑   ↑
       prev slow
```

We need:

```text
4 → 1
```

So:

```cpp
prev->next = slow->next;
```

`slow->next` is:

```text
1
```

Therefore:

```text
4 → 1
```

Final:

```text
1 → 3 → 4 → 1 → 2 → 6
```

---

# 🔄 Dry Run - Even Length

List:

```text
1 → 2 → 3 → 4
```

Initially:

```text
prev = NULL
slow = 1
fast = 1
```

### First iteration

```text
prev = 1
slow = 2
fast = 3
```

### Second iteration

```text
prev = 2
slow = 3
fast = NULL
```

Loop stops.

Now:

```text
1 → 2 → 3 → 4
    ↑   ↑
   prev slow
```

So:

```text
prev = 2
slow = 3
```

Delete:

```cpp
prev->next = slow->next;
```

Meaning:

```text
2 → 3 → 4
```

becomes:

```text
2 → 4
```

Final:

```text
1 → 2 → 4
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    ListNode* deleteMiddle(ListNode* head) {

        if(head == NULL || head->next == NULL){
            return NULL;
        }

        ListNode *slow = head;
        ListNode *fast = head;
        ListNode *prev = NULL;

        while(fast != NULL && fast->next != NULL){

            prev = slow;

            slow = slow->next;

            fast = fast->next->next;
        }

        prev->next = slow->next;

        return head;
    }
};
```

---

# 🔥 Most Important Part

Ye 3 lines:

```cpp
prev = slow;

slow = slow->next;

fast = fast->next->next;
```

Har round me:

```text
prev
 ↓
slow
 ↓
...
```

`prev` slow ke previous ko remember karta hai.

Loop ke end me:

```text
prev → slow → next
```

hoga.

Middle ko remove karne ke liye:

```cpp
prev->next = slow->next;
```

So:

```text
prev → slow → next
```

becomes:

```text
prev → next
```

---

# 🧠 Why `prev` Required?

Agar:

```text
1 → 2 → 3 → 4
```

Middle:

```text
3
```

Agar:

```text
slow = 3
```

to sirf `slow` se directly previous node `2` nahi mil sakta.

Isliye hum pehle se:

```cpp
prev = slow;
```

maintain karte hain.

Final:

```text
prev → slow → next
 2   →   3  →  4
```

Delete:

```cpp
prev->next = slow->next;
```

Result:

```text
2 → 4
```

---

# ⭐ Important Difference

### LC 876 - Middle of Linked List

```text
slow → Middle
```

Example:

```text
1 → 2 → 3 → 4 → 5
        ↑
      slow
```

---

### LC 2095 - Delete Middle

```text
slow → Middle
prev → Middle se pehle
```

Example:

```text
1 → 2 → 3 → 4 → 5
    ↑   ↑
  prev slow
```

Then:

```cpp
prev->next = slow->next;
```

---

# ⚠️ Common Mistakes

### 1. `prev` maintain nahi karna

Wrong:

```cpp
slow = slow->next;
fast = fast->next->next;
```

Agar middle mil gaya, previous node nahi milega.

---

### 2. Wrong deletion

Wrong:

```cpp
slow->next = slow->next->next;
```

Ye tabhi correct hai jab `slow` **middle ke previous** par ho.

Humare current approach me:

```text
slow = middle
```

isliye correct:

```cpp
prev->next = slow->next;
```

---

### 3. Edge case ignore karna

```cpp
if(head == NULL || head->next == NULL){
    return NULL;
}
```

Especially one-node list:

```text
1 → NULL
```

delete karne ke baad:

```text
NULL
```

---

# ⏱️ Time Complexity

```text
O(n)
```

Fast pointer list ko ek baar traverse karta hai.

---

# 💾 Space Complexity

```text
O(1)
```

Sirf:

```text
slow
fast
prev
```

pointers use hue hain.

---

# 🔥 Quick Revision

```text
slow = head
fast = head
prev = NULL

↓

prev = slow
slow = slow->next
fast = fast->next->next

↓

Loop ends

↓

prev → slow → next

↓

prev->next = slow->next

↓

Middle deleted
```

### One-line:

**Slow middle par jaata hai, fast 2 steps chalta hai, `prev` slow ke previous ko save karta hai, aur end me `prev->next = slow->next` karke middle delete kar dete hain.**

---

## Commit Message

```text
Add LC 2095 Delete Middle Node using slow fast and prev pointers
```
