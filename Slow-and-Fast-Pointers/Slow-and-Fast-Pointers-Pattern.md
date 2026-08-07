# Slow & Fast Pointers (Two Pointers) Pattern

# 📌 What is Slow & Fast Pointer?

Slow & Fast Pointer ek **Two Pointer Technique** hai jo mainly **Linked List** problems me use hoti hai.

Isme hum do pointers use karte hain:

```text
Slow Pointer → 1 step move karta hai

Fast Pointer → 2 steps move karta hai
```

Har iteration me:

```cpp
slow = slow->next;

fast = fast->next->next;
```

---

# 🧠 Main Idea

Fast pointer slow pointer se **double speed** se move karta hai.

Isliye:

```text
Fast end tak pahunchta hai

↓

Slow exactly middle ke aas-paas hota hai.
```

Aur agar Linked List me cycle hai,

to:

```text
Fast eventually Slow ko catch kar lega.
```

Yahi pura pattern ka base hai.

---

# 🔥 Visualization

Linked List

```text
1 → 2 → 3 → 4 → 5 → NULL
```

Initially

```text
S
F

↓

1 → 2 → 3 → 4 → 5
```

---

## Iteration 1

Slow:

```text
1 → 2
```

Fast:

```text
1 → 3
```

Diagram

```text
1 → 2 → 3 → 4 → 5

    S   F
```

---

## Iteration 2

Slow:

```text
2 → 3
```

Fast:

```text
3 → 5
```

Diagram

```text
1 → 2 → 3 → 4 → 5

        S       F
```

---

## Iteration 3

Fast:

```text
NULL
```

Loop stop.

Slow:

```text
3
```

Middle node.

---

# 🔥 Why Does It Work?

Suppose Linked List me:

```text
10 nodes
```

Fast:

```text
2 steps
```

move karega.

Slow:

```text
1 step
```

move karega.

Jab Fast:

```text
10 steps
```

chal chuka hoga,

Slow:

```text
5 steps
```

chala hoga.

Matlab:

```text
Fast End

↓

Slow Middle
```

---

# 🔥 Standard Template

```cpp
ListNode* slow = head;
ListNode* fast = head;

while(fast != NULL && fast->next != NULL){

    slow = slow->next;

    fast = fast->next->next;
}
```

Loop ke baad:

```text
slow
```

Middle node par hota hai.

---

# 🔥 Why This While Condition?

Hum Fast ko:

```cpp
fast = fast->next->next;
```

move kara rahe hain.

Agar:

```cpp
fast == NULL
```

ya

```cpp
fast->next == NULL
```

hua,

to:

```cpp
fast->next->next
```

Runtime Error dega.

Isliye condition:

```cpp
while(fast != NULL && fast->next != NULL)
```

---

# 🔥 Dry Run

Linked List

```text
1 → 2 → 3 → 4 → 5 → NULL
```

Initially

```text
Slow = 1

Fast = 1
```

---

### Loop 1

```text
Slow = 2

Fast = 3
```

---

### Loop 2

```text
Slow = 3

Fast = 5
```

---

### Loop 3

Fast next NULL.

Loop stop.

Answer:

```text
Slow = 3
```

---

# 🔥 Even Number of Nodes

Example

```text
1 → 2 → 3 → 4 → 5 → 6
```

Movement

```text
S,F = 1

↓

S=2

F=3

↓

S=3

F=5

↓

S=4

F=NULL
```

Answer:

```text
4
```

LeetCode 876 me second middle return hota hai.

---

# 🔥 Cycle Detection

Example

```text
1 → 2 → 3 → 4
      ↑     ↓
      ← ← ←
```

Slow:

```text
1 step
```

Fast:

```text
2 steps
```

Movement

```text
Slow

1 → 2 → 3 → 4

↓

2 → 3 → 4 → 2

↓

3 → 4 → 2 → 3

↓

4 → 2 → 3 → 4
```

Eventually

```text
Fast == Slow
```

Cycle present.

---

# 🏃 Race Track Analogy

Imagine:

```text
Runner

and

Bike
```

Runner:

```text
1 lap per minute
```

Bike:

```text
2 laps per minute
```

### Straight Road

Bike destination pe pahunch jayegi.

Runner beech me hoga.

Exactly:

```text
Fast End

↓

Slow Middle
```

---

### Circular Track

Bike faster hai.

Chahe runner kitna bhi aage ho,

Bike usko ek time baad catch kar legi.

Exactly same:

```text
Cycle

↓

Fast catches Slow
```

---

# 🔥 Where Is It Used?

## 1. Middle of Linked List

Question:

```text
Return middle node.
```

Use:

```text
Slow & Fast Pointer
```

---

## 2. Linked List Cycle

Question:

```text
Cycle hai ya nahi?
```

Use:

```text
Fast catches Slow
```

---

## 3. Linked List Cycle II

Question:

```text
Cycle kahan start hoti hai?
```

Pehle detect.

Fir starting node find.

---

## 4. Happy Number

Linked List nahi hai.

Numbers repeat hote hain.

Cycle detect karte hain.

---

## 5. Find Duplicate Number

Array ko linked list treat karte hain.

Floyd Algorithm.

---

## 6. Remove Nth Node From End

Do pointers ke beech fixed distance maintain karte hain.

---

## 7. Palindrome Linked List

Steps

```text
Middle Find

↓

Reverse Second Half

↓

Compare
```

---

## 8. Reorder List

Steps

```text
Middle

↓

Reverse

↓

Merge
```

---

# 🔥 Pattern Recognition

Question me agar words aaye:

```text
Middle

Cycle

Loop

Meeting Point

Palindrome

Nth Node From End

Reorder
```

Immediately socho:

```text
Slow & Fast Pointer
```

---

# 🔥 General Flow

```text
Head

↓

Slow = Head

Fast = Head

↓

Fast 2 step

↓

Slow 1 step

↓

Fast End ?

↓

YES

↓

Middle Mil Gaya
```

Cycle case

```text
Head

↓

Slow = Head

Fast = Head

↓

Fast 2 step

↓

Slow 1 step

↓

Fast == Slow ?

↓

YES

↓

Cycle Present
```

---

# ⏱️ Time Complexity

Most questions

```text
O(n)
```

---

# 💾 Space Complexity

```text
O(1)
```

Extra memory use nahi hoti.

---

# ⚠️ Common Mistakes

## Mistake 1

Wrong

```cpp
while(fast!=NULL)
```

Correct

```cpp
while(fast!=NULL && fast->next!=NULL)
```

---

## Mistake 2

Fast ko 1 step move karna.

Wrong

```cpp
fast=fast->next;
```

Correct

```cpp
fast=fast->next->next;
```

---

## Mistake 3

Slow ko 2 steps move kar dena.

Wrong

```cpp
slow=slow->next->next;
```

Correct

```cpp
slow=slow->next;
```

---

## Mistake 4

NULL check bhool jana.

Runtime Error aa jayega.

---

# 📚 Important Questions

## Easy

- LC 876 → Middle of Linked List
- LC 141 → Linked List Cycle
- LC 202 → Happy Number

---

## Medium

- LC 142 → Linked List Cycle II
- LC 287 → Find Duplicate Number
- LC 19 → Remove Nth Node From End

---

## Medium+

- LC 234 → Palindrome Linked List
- LC 143 → Reorder List

---

# ⭐ Quick Revision

```text
Two Pointers

↓

Slow = 1 step

↓

Fast = 2 steps

↓

Fast reaches end

↓

Slow reaches middle
```

Cycle

```text
Slow = 1

↓

Fast = 2

↓

Fast catches Slow

↓

Cycle Exists
```

---

# 🎯 One-Line Revision

> Slow pointer 1 step aur Fast pointer 2 steps move karta hai. Jab Fast list ke end tak pahunchta hai to Slow middle me hota hai. Agar list circular ho to Fast Slow ko zarur catch karega.

---

# 🚀 Pattern Summary

```text
Linked List

↓

Slow = 1 Step

↓

Fast = 2 Steps

↓

Fast End
      ↓
Middle

OR

Fast Meets Slow
      ↓
Cycle
```
