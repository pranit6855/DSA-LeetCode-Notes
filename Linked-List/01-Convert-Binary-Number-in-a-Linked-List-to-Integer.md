# 🔗 LeetCode 1290 — Convert Binary Number in a Linked List to Integer

## 📌 Problem

Hume ek **singly linked list** di gayi hai.

Har node ki value sirf:

```text
0 ya 1
```

ho sakti hai.

Ye poori linked list ek **binary number** represent karti hai.

Hume us binary number ko **decimal integer** me convert karke return karna hai.

---

# 🔹 Example 1

```text
head
 ↓
[1] → [0] → [1] → NULL
```

Linked list represent karti hai:

```text
101
```

Binary `101` ko decimal me convert karo:

```text
1 × 2² + 0 × 2¹ + 1 × 2⁰

= 1 × 4 + 0 × 2 + 1 × 1

= 4 + 0 + 1

= 5
```

So:

```text
Output = 5
```

---

# 🔹 Example 2

```text
head
 ↓
[1] → [0] → [0] → [1] → NULL
```

Binary:

```text
1001
```

Decimal:

```text
1×8 + 0×4 + 0×2 + 1×1

= 8 + 0 + 0 + 1

= 9
```

Output:

```text
9
```

---

# 🧠 Basic Idea

Hum linked list ko left se right traverse karenge.

Ek variable:

```cpp
int ans = 0;
```

rakhenge.

Har node par:

```cpp
ans = ans * 2 + current value;
```

karेंगे.

---

# 🤔 `ans = ans * 2 + temp->val` Kyu?

Ye sabse important concept hai.

Ye formula random nahi hai.

Binary number ka **base = 2** hota hai.

Jab binary number me ek naya digit add karte hain, purane number ko ek position left shift karna padta hai.

Binary me ek position left shift:

```text
× 2
```

hota hai.

Then current digit add karte hain.

Therefore:

```text
New Answer
=
Old Answer × 2
+
Current Digit
```

So:

```cpp
ans = ans * 2 + temp->val;
```

---

# 🔥 Decimal Se Compare Karo

Agar decimal number build kar rahe hote:

```text
123
```

Digits:

```text
1 → 2 → 3
```

Formula hota:

```text
ans = ans × 10 + digit
```

Kyunki decimal ka base:

```text
10
```

hai.

Binary ka base:

```text
2
```

hai.

Therefore:

```text
Binary:

ans = ans × 2 + digit
```

---

# 🔄 Example — `101`

Linked list:

```text
head
 ↓
[1] → [0] → [1] → NULL
```

Initially:

```text
ans = 0
```

---

## Step 1 — Current = `1`

Formula:

```text
ans = ans × 2 + current
```

So:

```text
ans = 0 × 2 + 1
    = 1
```

Now:

```text
ans = 1
```

---

## Step 2 — Current = `0`

Ab:

```text
ans = 1
```

Formula:

```text
ans = 1 × 2 + 0
    = 2
```

Now:

```text
ans = 2
```

Notice binary me:

```text
1
```

ke baad `0` add kiya:

```text
10
```

`10` binary = `2` decimal.

---

## Step 3 — Current = `1`

Ab:

```text
ans = 2
```

Formula:

```text
ans = 2 × 2 + 1
    = 5
```

So:

```text
101(binary) = 5(decimal)
```

---

# 🔥 Dry Run Table

For:

```text
[1] → [0] → [1] → [1]
```

| Current | Old `ans` | Formula | New `ans` |
|---|---:|---:|---:|
| 1 | 0 | `0×2 + 1` | 1 |
| 0 | 1 | `1×2 + 0` | 2 |
| 1 | 2 | `2×2 + 1` | 5 |
| 1 | 5 | `5×2 + 1` | 11 |

So:

```text
1011₂ = 11₁₀
```

---

# 🧠 Why Not Direct Binary Conversion?

Hum theoretically powers of 2 use karke bhi kar sakte hain:

```text
1011

1×8 + 0×4 + 1×2 + 1×1
= 11
```

But linked list me hume pehle length nahi pata.

Hum simply:

```text
left → right
```

traverse karte hue answer build kar sakte hain.

Isliye:

```cpp
ans = ans * 2 + temp->val;
```

much easier hai.

---

# 🔗 Linked List Traversal

Normal linked list traversal:

```cpp
ListNode* temp = head;

while(temp != NULL) {

    // current node

    temp = temp->next;
}
```

Yahan bhi exactly same traversal use hoga.

Bas current node par:

```cpp
ans = ans * 2 + temp->val;
```

karna hai.

---

# 🧩 Complete Approach

### Step 1

Answer initialize:

```cpp
int ans = 0;
```

---

### Step 2

Temporary pointer:

```cpp
ListNode* temp = head;
```

---

### Step 3

List traverse karo:

```cpp
while(temp != NULL)
```

---

### Step 4

Current binary digit ko answer me add karo:

```cpp
ans = ans * 2 + temp->val;
```

---

### Step 5

Next node:

```cpp
temp = temp->next;
```

---

### Step 6

List khatam hone ke baad:

```cpp
return ans;
```

---

# 💻 Final C++ Code

```cpp
class Solution {
public:
    int getDecimalValue(ListNode* head) {

        int ans = 0;

        ListNode* temp = head;

        while(temp != NULL) {

            ans = ans * 2 + temp->val;

            temp = temp->next;
        }

        return ans;
    }
};
```

---

# 🔍 Code Line-by-Line

### 1.

```cpp
int ans = 0;
```

Initially koi digit process nahi hua.

---

### 2.

```cpp
ListNode* temp = head;
```

`temp` ko first node par rakha.

```text
temp
 ↓
[1] → [0] → [1] → NULL
```

---

### 3.

```cpp
while(temp != NULL)
```

Jab tak current node exist karta hai, traverse karte raho.

---

### 4.

```cpp
ans = ans * 2 + temp->val;
```

Current binary digit ko answer me add kar rahe hain.

---

### 5.

```cpp
temp = temp->next;
```

Next node par move.

```text
temp
 ↓
[1] → [0] → [1]
```

then:

```text
        temp
         ↓
[1] → [0] → [1]
```

---

### 6.

```cpp
return ans;
```

Entire binary number ko decimal me convert karne ke baad answer return.

---

# ⚠️ Common Mistake

### ❌ Sirf `ans += temp->val`

Agar:

```text
101
```

hai:

```text
1 + 0 + 1 = 2
```

But correct answer:

```text
5
```

Isliye positional value consider karni padegi.

Correct:

```cpp
ans = ans * 2 + temp->val;
```

---

# ⚠️ Common Mistake 2

Ye bhi wrong:

```cpp
ans = ans * 10 + temp->val;
```

Kyunki binary ka base `2` hai.

```text
Decimal → ×10
Binary  → ×2
```

---

# 🧠 Formula Ka General Pattern

Ye bahut useful concept hai.

Agar kisi number ka base `B` hai:

```text
newAns = oldAns × B + digit
```

### Decimal

```text
B = 10

ans = ans × 10 + digit
```

### Binary

```text
B = 2

ans = ans × 2 + digit
```

---

# 🔥 Linked List Pattern

Is question me actual Linked List ka main part sirf traversal hai:

```cpp
ListNode* temp = head;

while(temp != NULL) {

    // current node par kaam

    temp = temp->next;
}
```

Ye pattern aage bahut questions me repeatedly use hoga.

---

# 🧠 Quick Revision

```text
Linked List:
[1] → [0] → [1]

        ↓

Traverse left → right

        ↓

ans = ans × 2 + current digit

        ↓

101₂ = 5₁₀
```

### One-line trick:

> **Binary number build karte waqt purane answer ko 2 se multiply karo aur current digit add karo.**

---

# ⏱️ Complexity

### Time

```text
O(n)
```

Har node ko exactly ek baar visit karte hain.

### Space

```text
O(1)
```

Sirf `ans` aur `temp` variables use ho rahe hain.

---

# 📁 Linked List Roadmap

```text
Linked-List/
│
├── 01-Convert-Binary-Number-to-Integer.md
```

Next questions:

```text
02 - Middle of the Linked List        (LC 876)
03 - Reverse Linked List              (LC 206)
04 - Merge Two Sorted Lists           (LC 21)
05 - Remove Duplicates                (LC 83)
06 - Linked List Cycle                (LC 141)
...
```

### Git Commit

```text
Add LC 1290 Convert Binary Number to Integer
```
