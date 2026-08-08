# LeetCode 206 - Reverse Linked List

## 📌 Problem

Hume ek **Singly Linked List** di gayi hai.

Us linked list ko reverse karke return karna hai.

---

# 🔹 Example

Input

```text
1 → 2 → 3 → 4 → 5
```

Output

```text
5 → 4 → 3 → 2 → 1
```

---

# 🧠 Main Idea

Reverse karne ka matlab hai har node ka

```text
next
```

pointer ulta karna.

Example

Before

```text
1 → 2 → 3
```

After

```text
3 → 2 → 1
```

---

# 🤔 Problem

Suppose

```text
1 → 2 → 3
```

Agar hum direct

```cpp
1->next = NULL;
```

kar den.

To

```text
2 → 3
```

ka address kho jayega.

Isliye pehle next node save karni padti hai.

---

# 🔥 Variables

Hum 3 pointers use karte hain.

```cpp
ListNode *prev = NULL;

ListNode *curr = head;

ListNode *next;
```

Meaning

```text
prev → Reverse ho chuki list ka head

curr → Current node

next → Next node ka address save karega
```

---

# 🔥 Initial State

```text
NULL ← 1 → 2 → 3 → 4 → 5

       ↑
      curr

prev = NULL
```

---

# 🔥 Step 1 - Save Next

```cpp
next = curr->next;
```

Example

```text
NULL ← 1 → 2 → 3

       ↑    ↑

     curr  next
```

Ab

```text
2
```

ka address safe hai.

---

# 🔥 Step 2 - Reverse Link

```cpp
curr->next = prev;
```

Before

```text
1 → 2
```

After

```text
1 → NULL
```

Current node ka direction reverse ho gaya.

---

# 🔥 Step 3 - Move Prev

```cpp
prev = curr;
```

Diagram

```text
NULL ← 1

       ↑

     prev
```

Ab reverse list ka head

```text
1
```

ban gaya.

---

# 🔥 Step 4 - Move Curr

```cpp
curr = next;
```

Diagram

```text
NULL ← 1     2 → 3 → 4

             ↑

           curr
```

Ab next node process hogi.

---

# 🔄 Dry Run

Initially

```text
prev = NULL

curr = 1
```

List

```text
1 → 2 → 3
```

---

Iteration 1

```text
next = 2

1 → NULL

prev = 1

curr = 2
```

List

```text
NULL ← 1

2 → 3
```

---

Iteration 2

```text
next = 3

2 → 1

prev = 2

curr = 3
```

List

```text
NULL ← 1 ← 2

3
```

---

Iteration 3

```text
next = NULL

3 → 2 → 1

prev = 3

curr = NULL
```

Loop stop.

---

# 🔥 Why Return Prev?

Loop khatam hone ke baad

```text
curr = NULL
```

Matlab current list ke bahar chala gaya.

Lekin

```text
prev
```

new head ko point kar raha hota hai.

Diagram

```text
prev

↓

3 → 2 → 1 → NULL
```

Isliye

```cpp
return prev;
```

---

# 💻 C++ Code

```cpp
class Solution {
public:

    ListNode* reverseList(ListNode* head) {

        ListNode *prev = NULL;
        ListNode *curr = head;

        while(curr != NULL){

            ListNode *next = curr->next;

            curr->next = prev;

            prev = curr;

            curr = next;
        }

        return prev;
    }
};
```

---

# 🔥 Most Important Concept

Har iteration me sirf 4 kaam hote hain.

```text
Save Next

↓

Reverse Link

↓

Move Prev

↓

Move Curr
```

Ye 4 steps hi pura Reverse Linked List hai.

---

# 🔥 Flow

```text
prev = NULL

↓

curr = head

↓

Save Next

↓

Reverse Link

↓

Move Prev

↓

Move Curr

↓

curr == NULL ?

↓

NO → Repeat

↓

YES

↓

Return Prev
```

---

# ⏱️ Time Complexity

```text
O(n)
```

Har node sirf ek baar visit hoti hai.

---

# 💾 Space Complexity

```text
O(1)
```

Sirf 3 pointers use hue hain.

---

# ⚠️ Common Mistakes

### 1.

Wrong

```cpp
curr->next = prev;
```

se pehle

```cpp
next = curr->next;
```

save nahi karna.

Result

```text
Remaining list ka address kho jayega.
```

---

### 2.

Wrong Order

```cpp
prev = curr;

curr->next = prev;
```

Isse self-loop ban jayega.

Correct order

```cpp
next = curr->next;

curr->next = prev;

prev = curr;

curr = next;
```

---

### 3.

Wrong

```cpp
return head;
```

Correct

```cpp
return prev;
```

Reverse hone ke baad

```text
Prev = New Head
```

---

# 🔥 Quick Revision

```text
prev = NULL

↓

curr = head

↓

Save Next

↓

Reverse Link

↓

Move Prev

↓

Move Curr

↓

Return Prev
```

---

# 🧠 One-Line Revision

Har node ka next pointer reverse karo. Address lose na ho isliye pehle next save karo, phir link reverse karo, prev aur curr ko update karo. End me prev hi new head hota hai.

---

# ⭐ Interview Revision

```text
prev

↓

curr

↓

next Save

↓

Reverse Link

↓

Move Prev

↓

Move Curr

↓

Return Prev
```

Pattern

```text
Pointer Manipulation

↓

Reverse Linked List
```
