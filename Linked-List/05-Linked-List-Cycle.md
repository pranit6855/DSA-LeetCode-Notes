# 🔗 LeetCode 141 — Linked List Cycle

## 📌 Problem

Hume ek singly linked list di gayi hai.

Check karna hai ki linked list me **cycle/loop exist karta hai ya nahi**.

Agar kisi node ka `next` kisi previous node ko point kar raha hai, to cycle hai. :contentReference[oaicite:0]{index=0}

---

# 🔹 Example 1 — Cycle Hai

```text
[3] → [2] → [0] → [-4]
       ↑             ↓
       └─────────────┘
```

Yahan:

```text
-4 → 2
```

hai.

Isliye hum continuously traverse karte rahenge:

```text
3 → 2 → 0 → -4 → 2 → 0 → -4 → 2 → ...
```

Kabhi `NULL` nahi milega.

Therefore:

```text
Output = true
```

---

# 🔹 Example 2 — Cycle Nahi Hai

```text
[1] → [2] → NULL
```

Traversal:

```text
1 → 2 → NULL
```

`NULL` mil gaya.

Therefore:

```text
Output = false
```

---

# 🧠 Main Approach — Slow & Fast Pointer

Yahan **Floyd's Cycle Detection Algorithm** use karenge.

Do pointers:

```text
slow
fast
```

### `slow`

```text
1 step
```

### `fast`

```text
2 steps
```

Ye cycle detection ka standard O(n) time, O(1) extra-space approach hai. :contentReference[oaicite:1]{index=1}

---

# 🔥 Initial Position

Dono ko `head` se start karenge:

```cpp
ListNode* slow = head;
ListNode* fast = head;
```

Example:

```text
[1] → [2] → [3] → [4] → NULL
 ↑
slow
 ↑
fast
```

Dono initially same node par hain.

### ⚠️ Important

**Starting me `slow == fast` check nahi karna.**

Kyunki:

```text
slow = head
fast = head
```

to obviously:

```text
slow == fast
```

hoga.

Check **move karne ke baad** karenge.

---

# 🔄 Pointer Movement

Har iteration:

```cpp
slow = slow->next;
fast = fast->next->next;
```

Matlab:

```text
slow → 1 step
fast → 2 steps
```

---

# 🔹 Case 1 — No Cycle

Example:

```text
[1] → [2] → [3] → [4] → NULL
```

Initially:

```text
slow = 1
fast = 1
```

### First move

```text
slow = 2
fast = 3
```

### Second move

```text
slow = 3
fast = NULL
```

`fast` end tak pahunch gaya.

So:

```text
No cycle
```

Return:

```cpp
false
```

---

# 🔹 Case 2 — Cycle Hai

Example:

```text
[1] → [2] → [3] → [4]
       ↑           ↓
       └───────────┘
```

Cycle:

```text
2 → 3 → 4 → 2 → 3 → 4 → ...
```

Starting:

```text
slow = 1
fast = 1
```

Move:

```text
slow → 2
fast → 3
```

Next:

```text
slow → 3
fast → 2
```

Next:

```text
slow → 4
fast → 4
```

Ab:

```text
slow == fast
```

🔥 Cycle detected.

Return:

```cpp
true;
```

---

# 🤔 Slow aur Fast Milenge Kyu?

Ye sabse important intuition hai.

Cycle ko ek **circular track** samjho:

```text
       → [2] → [3]
      ↑           ↓
     [4] ← [5] ← [6]
```

Slow:

```text
1 step
```

Fast:

```text
2 steps
```

Fast har round me slow se **1 extra step** gain karta hai.

Isliye agar dono cycle ke andar hain, fast eventually slow ko catch kar lega. :contentReference[oaicite:2]{index=2}

Simple analogy:

```text
slow = normal runner
fast = double-speed runner

closed track hai
      ↓
fast eventually slow ko lap karega
      ↓
same node par milenge
```

---

# 🔥 Actual Detection Logic

Loop:

```cpp
while(fast != NULL && fast->next != NULL)
```

Andar:

```cpp
slow = slow->next;
fast = fast->next->next;
```

Then:

```cpp
if(slow == fast) {
    return true;
}
```

Agar loop khatam ho gaya:

```text
fast == NULL
OR
fast->next == NULL
```

Matlab fast ko end mil gaya.

Therefore:

```cpp
return false;
```

---

# 💻 Final Code

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {

        ListNode* slow = head;
        ListNode* fast = head;

        while(fast != NULL && fast->next != NULL) {

            slow = slow->next;
            fast = fast->next->next;

            if(slow == fast) {
                return true;
            }
        }

        return false;
    }
};
```

---

# 🔍 Code Line-by-Line

## 1. Slow pointer

```cpp
ListNode* slow = head;
```

`slow` first node se start.

---

## 2. Fast pointer

```cpp
ListNode* fast = head;
```

`fast` bhi first node se start.

---

## 3. Safe traversal

```cpp
while(fast != NULL && fast->next != NULL)
```

Fast ko 2 steps move karna hai:

```cpp
fast->next->next
```

Isliye:

```text
fast
fast->next
```

dono valid hone chahiye.

---

## 4. Slow move

```cpp
slow = slow->next;
```

1 step.

---

## 5. Fast move

```cpp
fast = fast->next->next;
```

2 steps.

---

## 6. Meeting check

```cpp
if(slow == fast)
```

Agar same **node/address** par aa gaye:

```text
Cycle exists
```

So:

```cpp
return true;
```

---

## 7. Loop ke baad

Agar fast `NULL` tak pahunch gaya:

```text
No cycle
```

So:

```cpp
return false;
```

---

# ⚠️ `slow == fast` ka Matlab Kya?

Ye:

```cpp
slow == fast
```

**values compare nahi karta.**

Ye check karta hai ki dono pointers **same node ko point kar rahe hain ya nahi**.

Example:

```text
[5] → [5] → [7]
```

Agar dono alag `5` nodes ko point kar rahe hain:

```text
slow → first [5]

fast → second [5]
```

then:

```text
slow == fast
```

❌ false.

Value same hone se matter nahi karta.

Hume:

```text
same node/address
```

chahiye.

---

# 🔥 Middle vs Cycle

Tumne abhi LC 876 me bhi slow-fast use kiya tha.

Dono ka basic movement same:

```cpp
slow = slow->next;
fast = fast->next->next;
```

But **purpose different** hai.

---

## LC 876 — Middle

```text
slow → 1 step
fast → 2 steps
```

Loop ke baad:

```text
slow = middle
```

So:

```cpp
return slow;
```

---

## LC 141 — Cycle

```text
slow → 1 step
fast → 2 steps
```

Move karne ke **baad**:

```cpp
if(slow == fast)
```

then:

```text
cycle exists
```

So:

```cpp
return true;
```

---

# ⭐ Easy Memory Trick

```text
LC 876:
fast reaches END
        ↓
slow = MIDDLE
```

```text
LC 141:
slow MEETS fast
        ↓
CYCLE
```

---

# ⚠️ Common Mistake #1

### Start me `slow == fast` check karna

Wrong:

```cpp
slow = head;
fast = head;

if(slow == fast)
    return true;
```

Ye hamesha first iteration me true ho jayega.

Kyunki:

```text
slow
 ↓
[1]
 ↑
fast
```

Dono initially same node ko point kar rahe hain.

Correct:

```cpp
slow = slow->next;
fast = fast->next->next;

if(slow == fast)
    return true;
```

---

# ⚠️ Common Mistake #2

### `fast->next` check nahi karna

Wrong:

```cpp
while(fast != NULL) {
    fast = fast->next->next;
}
```

Agar:

```text
fast
 ↓
[4] → NULL
```

then:

```text
fast->next = NULL
```

aur:

```cpp
fast->next->next
```

invalid ho jayega.

Correct:

```cpp
while(fast != NULL && fast->next != NULL)
```

---

# ⚠️ Common Mistake #3

### `fast` ko ek hi step move karna

Wrong:

```cpp
fast = fast->next;
```

Then dono same speed se chalenge.

Cycle detect karne ke liye relative speed difference chahiye.

Correct:

```cpp
fast = fast->next->next;
```

---

# 🔥 Edge Case — Empty List

```text
head = NULL
```

Then:

```text
slow = NULL
fast = NULL
```

Condition:

```cpp
fast != NULL
```

false.

Loop nahi chalega.

Return:

```cpp
false;
```

Correct.

---

# 🔥 Edge Case — Single Node, No Cycle

```text
[1] → NULL
```

```text
fast->next == NULL
```

Loop nahi chalega.

Return:

```text
false
```

Correct.

---

# 🔥 Edge Case — Single Node, Self Cycle

```text
      ┌──────┐
      ↓      │
     [1] ────┘
```

Yahan:

```text
1 → 1 → 1 → ...
```

Initially:

```text
slow = 1
fast = 1
```

First iteration:

```text
slow = 1
fast = 1
```

Ab:

```text
slow == fast
```

So:

```text
true
```

Correct.

---

# 🧠 Full Algorithm

```text
1. slow = head
2. fast = head

3. Jab tak fast aur fast->next exist kare:
       slow ko 1 step
       fast ko 2 steps

       agar slow == fast:
           cycle hai

4. Agar fast NULL tak pahunch gaya:
       cycle nahi hai
```

---

# 📊 Dry Run

Example:

```text
[3] → [2] → [0] → [-4]
       ↑             ↓
       └─────────────┘
```

Movement:

| Iteration | Slow | Fast |
|---|---|---|
| Start | 3 | 3 |
| 1 | 2 | 0 |
| 2 | 0 | 2 |
| 3 | -4 | -4 |

At iteration 3:

```text
slow == fast
```

Therefore:

```text
true
```

---

# ⏱️ Complexity

### Time

```text
O(n)
```

Worst case me pointers list/cycle ko linear number of steps me traverse karte hain. :contentReference[oaicite:3]{index=3}

### Space

```text
O(1)
```

Sirf do pointers use kiye:

```text
slow
fast
```

Koi HashSet/array nahi.

---

# 🆚 HashSet Approach vs Slow/Fast

Cycle detect karne ka ek aur method hai:

```text
Har visited node ko HashSet me store karo.
```

Agar same node dobara mila:

```text
cycle
```

But:

```text
HashSet → O(n) extra space
Slow/Fast → O(1) extra space
```

Floyd's slow-fast approach isliye preferred hai jab constant extra space chahiye. :contentReference[oaicite:4]{index=4}

---

# 🧠 Pattern Recognition

Question me agar words aaye:

```text
cycle
loop
circular linked list
repeated node
```

to immediately socho:

```text
FAST & SLOW POINTER
```

Floyd's algorithm ka main pattern:

```text
slow = 1 step
fast = 2 steps

      ↓

slow == fast
      ↓
cycle
```

---

# 🔥 Most Important Comparison

Ab tak slow-fast ke 2 patterns:

### Middle

```cpp
while(fast != NULL && fast->next != NULL) {
    slow = slow->next;
    fast = fast->next->next;
}

return slow;
```

### Cycle

```cpp
while(fast != NULL && fast->next != NULL) {
    slow = slow->next;
    fast = fast->next->next;

    if(slow == fast)
        return true;
}

return false;
```

**Movement same hai. Result check different hai.**

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
└── 06-Linked-List-Cycle.md
```

### Git Commit

```text
Add LC 141 Linked List Cycle
```

---

# ⭐ One-Line Memory Trick

> **Slow 1 step aur fast 2 steps. Agar fast NULL tak pahunch gaya → cycle nahi. Agar move karne ke baad slow aur fast same node par mil gaye → cycle hai.** :contentReference[oaicite:5]{index=5}
