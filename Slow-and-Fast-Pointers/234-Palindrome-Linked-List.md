# LeetCode 234 - Palindrome Linked List

## 📌 Problem

Hume ek **Singly Linked List** di gayi hai.

Check karna hai ki linked list **Palindrome** hai ya nahi.

Agar palindrome hai

```text
return true
```

warna

```text
return false
```

---

# 🔹 Example 1

```text
1 → 2 → 2 → 1
```

Output

```text
true
```

---

# 🔹 Example 2

```text
1 → 2
```

Output

```text
false
```

---

# 🧠 Main Idea

Ye question 3 concepts ko combine karta hai.

```text
1. Find Middle (LC 876)

↓

2. Reverse Second Half (LC 206)

↓

3. Compare Both Halves
```

---

# 🔥 Step 1 - Find Middle

Use Slow & Fast Pointer.

```cpp
ListNode *slow = head;
ListNode *fast = head;
```

Loop

```cpp
while(fast != NULL && fast->next != NULL){

    slow = slow->next;
    fast = fast->next->next;
}
```

Example

```text
head
 ↓
1 → 2 → 2 → 1
      ↑
    slow
```

Middle mil gaya.

---

# 🔥 Step 2 - Reverse Second Half

Middle se end tak reverse karenge.

```cpp
ListNode *second = reverse(slow);
```

Before Reverse

```text
2 → 1 → NULL
```

After Reverse

```text
second
 ↓
1 → 2 → NULL
```

---

# 🔥 Step 3 - First Pointer

```cpp
ListNode *first = head;
```

Ab

```text
first
 ↓
1 → 2 → 2 → NULL
```

Aur

```text
second
 ↓
1 → 2 → NULL
```

Ab compare karenge.

---

# 🔥 Step 4 - Compare

```cpp
while(second != NULL)
```

Sirf second half tak compare karna hai.

```cpp
if(first->val != second->val){

    return false;
}
```

Agar values same hain.

```cpp
first = first->next;

second = second->next;
```

Move both pointers.

---

# 🔄 Dry Run

Original

```text
1 → 2 → 2 → 1
```

Middle

```text
1 → 2 → 2 → 1
      ↑
    slow
```

Reverse Second Half

```text
first
 ↓
1 → 2 → 2 → NULL

second
 ↓
1 → 2 → NULL
```

Compare

```text
1 == 1 ✅

2 == 2 ✅
```

Second list finish.

Return

```text
true
```

---

# 🔄 Example 2

```text
1 → 2
```

Middle

```text
2
```

Reverse

```text
2
```

Compare

```text
1 != 2
```

Return

```text
false
```

---

# 💻 C++ Code

```cpp
class Solution {
public:

    ListNode* reverse(ListNode* head){

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

    bool isPalindrome(ListNode* head) {

        if(head == NULL || head->next == NULL){
            return true;
        }

        ListNode *slow = head;
        ListNode *fast = head;

        while(fast != NULL && fast->next != NULL){

            slow = slow->next;
            fast = fast->next->next;
        }

        ListNode *second = reverse(slow);

        ListNode *first = head;

        while(second != NULL){

            if(first->val != second->val){
                return false;
            }

            first = first->next;
            second = second->next;
        }

        return true;
    }
};
```

---

# 🔥 Most Important Concept

Hum original list ko direct compare nahi karte.

Hum compare karte hain:

```text
First Half

VS

Reversed Second Half
```

Example

```text
Original

1 → 2 → 2 → 1

↓

Reverse Second Half

First

1 → 2

Second

1 → 2
```

Ab comparison easy ho jata hai.

---

# 🔥 Flow

```text
Head

↓

Find Middle

↓

Reverse Second Half

↓

first = head

↓

second = reversed list

↓

Compare Values

↓

Mismatch ?

↓

YES → false

↓

NO

↓

Move Both

↓

second == NULL

↓

Return true
```

---

# ⏱️ Time Complexity

```text
O(n)
```

- Find Middle → O(n)
- Reverse → O(n)
- Compare → O(n)

Overall

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

Compare original list directly.

❌ Wrong

Second half ko reverse karna zaruri hai.

---

### 2.

Wrong

```cpp
while(first != NULL)
```

Correct

```cpp
while(second != NULL)
```

Second half chhoti ya equal hoti hai.

---

### 3.

Middle find karne ke baad

```cpp
reverse(head);
```

❌ Wrong

Correct

```cpp
reverse(slow);
```

Sirf second half reverse hogi.

---

### 4.

Reverse function me

```cpp
return head;
```

❌ Wrong

Correct

```cpp
return prev;
```

---

# 🔥 Quick Revision

```text
Find Middle

↓

Reverse Second Half

↓

first = head

↓

second = reverse(slow)

↓

Compare

↓

Return true / false
```

---

# 🧠 One-Line Revision

Pehle middle find karo, middle se second half reverse karo, phir first half aur reversed second half ko compare karo. Agar sab values same hain to linked list palindrome hai.

---

# ⭐ Interview Revision

```text
Middle

↓

Reverse

↓

Compare

↓

Palindrome
```

Pattern

```text
Slow & Fast Pointer

+

Reverse Linked List

+

Two Pointer Comparison
```
