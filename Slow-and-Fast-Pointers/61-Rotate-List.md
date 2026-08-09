# LeetCode 61 - Rotate List

## 📌 Problem

Hume ek linked list di gayi hai aur integer `k`.

List ko **right side `k` times rotate** karna hai.

Example:

```text
1 → 2 → 3 → 4 → 5
k = 2
```

Output:

```text
4 → 5 → 1 → 2 → 3
```

---

# 🧠 Main Idea

Right se `k` nodes front me aa jayengi.

```text
1 → 2 → 3 | 4 → 5
          ↑
       New Tail
```

Final:

```text
4 → 5 → 1 → 2 → 3
```

Isliye hume find karna hai:

```text
New Tail
```

---

# 🔥 Step 1 - Edge Cases

```cpp
if(head == NULL || head->next == NULL || k == 0){
    return head;
}
```

### `head == NULL`

List empty:

```text
NULL
```

Kuch rotate nahi karna.

### `head->next == NULL`

Sirf ek node:

```text
1 → NULL
```

Rotate karne ke baad bhi same.

### `k == 0`

Rotation hi nahi karna.

---

# 🔥 Step 2 - Length Find Karo

```cpp
ListNode *tail = head;
int n = 1;

while(tail->next != NULL){
    tail = tail->next;
    n++;
}
```

Example:

```text
1 → 2 → 3 → 4 → 5 → NULL
```

After loop:

```text
n = 5
tail = 5
```

---

# 🔥 Step 3 - `k % n`

```cpp
k = k % n;
```

Kyunki `n` rotations ke baad list wapas same ho jaati hai.

Example:

```text
n = 5
k = 7
```

Then:

```text
7 % 5 = 2
```

So 7 rotations = 2 effective rotations.

Agar:

```cpp
k == 0
```

to:

```cpp
return head;
```

---

# 🔥 Step 4 - New Tail Find Karo

Formula:

```text
New Tail Position = n - k
```

Example:

```text
n = 5
k = 2

n - k = 3
```

So third node new tail hai:

```text
1 → 2 → 3 → 4 → 5
          ↑
       New Tail
```

Slow ko third node tak le jayenge.

```cpp
ListNode *slow = head;

for(int i = 1; i < n-k; i++){
    slow = slow->next;
}
```

After loop:

```text
slow = 3
```

---

# 🔥 Step 5 - New Head

New tail ke next node ko new head bana do.

```cpp
ListNode *newhead = slow->next;
```

Diagram:

```text
1 → 2 → 3 → 4 → 5
          ↑   ↑
        slow newhead
```

So:

```text
slow = 3
newhead = 4
```

---

# 🔥 Step 6 - Last Node ko Old Head Se Connect

Last node:

```text
5
```

Old head:

```text
1
```

So:

```cpp
tail->next = head;
```

Means:

```text
5 → 1
```

Ab circular list:

```text
1 → 2 → 3 → 4 → 5
↑                 |
└─────────────────┘
```

---

# 🔥 Step 7 - Circle Break Karo

New tail:

```text
3
```

Uska next currently:

```text
3 → 4
```

Isko todna hai:

```cpp
slow->next = NULL;
```

Now:

```text
3 → NULL
```

Final list:

```text
4 → 5 → 1 → 2 → 3 → NULL
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {

        if(head == NULL || head->next == NULL || k == 0){
            return head;
        }

        ListNode *tail = head;
        int n = 1;

        while(tail->next != NULL){
            tail = tail->next;
            n++;
        }

        k = k % n;

        if(k == 0){
            return head;
        }

        ListNode *slow = head;

        for(int i = 1; i < n-k; i++){
            slow = slow->next;
        }

        ListNode *newhead = slow->next;

        tail->next = head;

        slow->next = NULL;

        return newhead;
    }
};
```

---

# 🔄 Complete Dry Run

Input:

```text
1 → 2 → 3 → 4 → 5
k = 2
```

### Length

```text
n = 5
tail = 5
```

### Effective k

```text
2 % 5 = 2
```

### New Tail

```text
n - k = 3
```

```text
1 → 2 → 3 → 4 → 5
          ↑
        slow
```

### New Head

```text
newhead = slow->next
```

```text
1 → 2 → 3 → 4 → 5
          ↑   ↑
        slow newhead
```

### Connect Tail to Old Head

```text
5 → 1
```

Circular:

```text
1 → 2 → 3 → 4 → 5
↑                 |
└─────────────────┘
```

### Break After New Tail

```text
3 → NULL
```

Final:

```text
4 → 5 → 1 → 2 → 3 → NULL
```

---

# ⭐ Important Concepts

```text
Length
   ↓
k % n
   ↓
New Tail = n - k
   ↓
New Head = New Tail->next
   ↓
Last Node → Old Head
   ↓
New Tail → NULL
```

---

# ⚠️ Common Mistakes

### 1. `head == NULL` check bhoolna

Agar:

```text
head = NULL
```

aur directly:

```cpp
tail->next
```

karoge, NULL pointer dereference hoga.

---

### 2. `k % n` bhoolna

Large `k` ke case me unnecessary rotations ho sakti hain.

---

### 3. New Tail galat find karna

Correct:

```text
n - k
```

For:

```text
n = 5
k = 2
```

New Tail:

```text
3
```

---

### 4. Last node ko old head se connect na karna

Correct:

```cpp
tail->next = head;
```

Means:

```text
5 → 1
```

---

### 5. Circle break na karna

Correct:

```cpp
slow->next = NULL;
```

Warna infinite circular linked list ban jayegi.

---

# ⏱️ Time Complexity

```text
O(n)
```

Length find karne ke liye ek traversal aur new tail find karne ke liye ek traversal.

---

# 💾 Space Complexity

```text
O(1)
```

Extra linked list/array nahi banaya.

---

# 🧠 One-Line Revision

**Length nikalo, `k % n` karo, `(n-k)`th node ko new tail banao, last node ko old head se connect karo aur new tail ke baad `NULL` karke circle tod do.**

---

# 🔥 Quick Revision

```text
1 → 2 → 3 → 4 → 5

k = 2

n = 5

n-k = 3

slow = 3

newhead = 4

5 → 1

3 → NULL

Answer:

4 → 5 → 1 → 2 → 3
```
