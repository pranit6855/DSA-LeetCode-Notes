# 🔗 LeetCode 19 — Remove Nth Node From End of List

## 📌 Problem

Hume ek singly linked list di gayi hai aur ek integer `n`.

Hume list ke **end se nth node remove** karna hai.

Example:

```text
Input:

[1] → [2] → [3] → [4] → [5] → NULL

n = 2
```

End se counting:

```text
5 → 1st
4 → 2nd  ← DELETE
3 → 3rd
2 → 4th
1 → 5th
```

Output:

```text
[1] → [2] → [3] → [5] → NULL
```

---

# 🧠 Approach Used Here

Is solution me hum **slow-fast pointer use nahi karenge**.

Simple:

```text
2 PASS APPROACH
```

### Pass 1

List ki length find karo.

```text
[1] → [2] → [3] → [4] → [5]

len = 5
```

### Pass 2

Calculate karo ki delete hone wale node ke **previous node** tak kitna move karna hai.

Then:

```cpp
temp->next = temp->next->next;
```

se node ko skip kar do.

---

# 🔥 Why Length Find Karna?

Question me position:

```text
FROM END
```

di hui hai.

But linked list ko normally:

```text
FROM START
```

traverse karte hain.

Example:

```text
[1] → [2] → [3] → [4] → [5]
```

If:

```text
n = 2
```

then end se 2nd:

```text
[4]
```

hai.

Start se iska position:

```text
4th
```

hai.

Formula:

```text
position from start = len - n + 1
```

But hume **delete karne wale node ke previous** par jaana hai.

Isliye 0-based indexing use karte hue:

```text
pos = len - n - 1
```

Yahan `pos` = previous node ka index.

---

# 📊 Example

```text
[1] → [2] → [3] → [4] → [5]
```

Indexes:

```text
index:  0     1     2     3     4
       [1] → [2] → [3] → [4] → [5]
```

Given:

```text
len = 5
n = 2
```

So:

```cpp
pos = len - n - 1;
```

```text
pos = 5 - 2 - 1
    = 2
```

Index `2`:

```text
[3]
```

So `temp` ko `[3]` par le jaana hai.

Because:

```text
[3] → [4] → [5]
 ↑      ↑
temp   delete
```

Then:

```cpp
temp->next = temp->next->next;
```

Result:

```text
[3] ─────────→ [5]
```

Final:

```text
[1] → [2] → [3] → [5]
```

---

# 🔥 Step 1 — Length Find Karo

```cpp
int len = 0;
ListNode* temp = head;

while(temp != NULL) {
    len++;
    temp = temp->next;
}
```

Suppose:

```text
[1] → [2] → [3] → [4] → [5]
```

Traversal:

```text
temp → 1
len = 1

temp → 2
len = 2

temp → 3
len = 3

temp → 4
len = 4

temp → 5
len = 5

temp → NULL
```

Finally:

```text
len = 5
temp = NULL
```

---

# ⚠️ Important

Length nikalne ke baad:

```cpp
temp
```

already:

```text
NULL
```

par hai.

Isliye second traversal se pehle:

```cpp
temp = head;
```

**mandatory hai.**

```text
temp = NULL
       ↓
temp = head
       ↓
[1] → [2] → [3] → ...
```

---

# 🔥 Step 2 — Head Delete Edge Case

Agar:

```text
n == len
```

to end se nth node actually **head** hai.

Example:

```text
[1] → [2] → [3]
```

and:

```text
n = 3
len = 3
```

End se counting:

```text
3 → 1st
2 → 2nd
1 → 3rd ← DELETE
```

So head delete karna hai.

```cpp
if(n == len) {
    return head->next;
}
```

Before:

```text
head
 ↓
[1] → [2] → [3]
```

After:

```text
head
 ↓
[2] → [3]
```

---

# ⭐ Why `n == len`?

Because:

```text
n = total number of nodes
```

means end se count karte hue hum first node/head tak pahunch gaye.

Therefore:

```cpp
if(n == len)
```

is the head deletion case.

---

# 🔥 Step 3 — Position Calculate

Head case handle karne ke baad:

```cpp
int pos = len - n - 1;
```

### Why `-1`?

Because hum **delete hone wale node par nahi**, uske **previous node** par jaana chahte hain.

Example:

```text
len = 5
n = 2
```

Delete:

```text
[4]
```

Previous:

```text
[3]
```

0-based index:

```text
[3] = index 2
```

Formula:

```text
5 - 2 - 1 = 2
```

Exactly.

---

# 🔥 Step 4 — Temp Ko Head Par Reset

Length find karne ke baad `temp == NULL`.

So:

```cpp
temp = head;
```

Now:

```text
temp
 ↓
[1] → [2] → [3] → [4] → [5]
```

---

# 🔥 Step 5 — Temp Ko `pos` Tak Le Jao

```cpp
for(int i = 0; i < pos; i++) {
    temp = temp->next;
}
```

For:

```text
pos = 2
```

Start:

```text
index: 0
temp → [1]
```

### i = 0

```text
0 < 2
```

Move:

```text
temp → [2]
```

### i = 1

```text
1 < 2
```

Move:

```text
temp → [3]
```

### i = 2

```text
2 < 2
```

False.

Stop.

So:

```text
[1] → [2] → [3] → [4] → [5]
              ↑      ↑
             temp   delete
```

---

# 🔥 Step 6 — Node Skip Karo

Ab:

```cpp
temp->next = temp->next->next;
```

Before:

```text
[3] → [4] → [5]
 ↑      ↑
temp   delete
```

`temp->next`:

```text
[4]
```

`temp->next->next`:

```text
[5]
```

So:

```cpp
temp->next = temp->next->next;
```

means:

```text
[3] ─────→ [5]
```

Final:

```text
[1] → [2] → [3] → [5] → NULL
```

---

# 💻 Final Code

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {

        int len = 0;
        ListNode* temp = head;

        // Step 1: Find length
        while(temp != NULL) {
            len++;
            temp = temp->next;
        }

        // Step 2: If head needs to be deleted
        if(n == len) {
            return head->next;
        }

        // Step 3: Find previous node of node to delete
        int pos = len - n - 1;

        // Step 4: Reset temp
        temp = head;

        // Step 5: Move temp to previous node
        for(int i = 0; i < pos; i++) {
            temp = temp->next;
        }

        // Step 6: Skip the node to delete
        temp->next = temp->next->next;

        return head;
    }
};
```

---

# 🔍 Code Line-by-Line

### Length

```cpp
int len = 0;
ListNode* temp = head;
```

Length count karne ke liye.

---

### Traverse

```cpp
while(temp != NULL) {
    len++;
    temp = temp->next;
}
```

List ki length find.

---

### Head case

```cpp
if(n == len) {
    return head->next;
}
```

Agar first node delete karna hai.

---

### Position

```cpp
int pos = len - n - 1;
```

Delete node ke previous ka **0-based index**.

---

### Reset

```cpp
temp = head;
```

Important because length traversal ke baad:

```text
temp = NULL
```

ho chuka tha.

---

### Move

```cpp
for(int i = 0; i < pos; i++) {
    temp = temp->next;
}
```

Previous node tak jao.

---

### Delete/Skip

```cpp
temp->next = temp->next->next;
```

Delete node ko linked-list chain se skip karo.

---

### Return

```cpp
return head;
```

Head change nahi hua, except `n == len` case jo already separately return ho chuka hai.

---

# 🧠 Why We Don't Actually Need `delete`

Hum yahan:

```cpp
temp->next = temp->next->next;
```

kar rahe hain.

Isse node linked list se **disconnect** ho jata hai.

Example:

```text
Before:

A → B → C
```

After:

```text
A → C
```

`B` returned list ka part nahi raha.

LeetCode solution ke perspective se pointer unlink karna sufficient hai.

---

# ⚠️ Common Mistake #1 — `temp = head` Bhoolna

Wrong:

```cpp
while(temp != NULL) {
    len++;
    temp = temp->next;
}

for(int i = 0; i < pos; i++) {
    temp = temp->next;
}
```

Length loop ke baad:

```text
temp = NULL
```

So:

```cpp
temp->next
```

invalid access hoga.

Correct:

```cpp
temp = head;
```

---

# ⚠️ Common Mistake #2 — `pos = len - n`

Agar:

```text
len = 5
n = 2
```

to:

```text
len - n = 3
```

Index 3 is:

```text
[4]
```

But hume `[4]` ke previous `[3]` par jaana hai.

Therefore:

```cpp
pos = len - n - 1;
```

---

# ⚠️ Common Mistake #3 — `i = 1`

Humne yahan 0-based indexing choose ki hai.

```cpp
int pos = len - n - 1;
```

So loop bhi:

```cpp
for(int i = 0; i < pos; i++)
```

hona chahiye.

Example:

```text
pos = 2
```

Start:

```text
temp → index 0
```

Two moves:

```text
index 0 → index 1 → index 2
```

Therefore:

```cpp
i = 0
```

se start.

---

# ⚠️ Common Mistake #4 — Head Case Ignore Karna

Example:

```text
[1] → [2] → [3]
n = 3
```

Formula:

```text
pos = 3 - 3 - 1
    = -1
```

Negative position par loop nahi chalega.

Isliye:

```cpp
if(n == len) {
    return head->next;
}
```

pehle handle karna zaroori hai.

---

# 🔥 Dry Run 1

Input:

```text
[1] → [2] → [3] → [4] → [5]
n = 2
```

### Length

```text
len = 5
```

### Head check

```text
2 == 5
false
```

### Position

```text
pos = 5 - 2 - 1
    = 2
```

### Temp

```text
temp → [1]
```

Loop:

```text
i = 0 → temp = [2]
i = 1 → temp = [3]
```

Now:

```text
[1] → [2] → [3] → [4] → [5]
              ↑      ↑
             temp   delete
```

Skip:

```cpp
temp->next = temp->next->next;
```

Output:

```text
[1] → [2] → [3] → [5]
```

---

# 🔥 Dry Run 2 — Delete Last Node

Input:

```text
[1] → [2] → [3] → [4]
n = 1
```

Length:

```text
len = 4
```

Position:

```text
pos = 4 - 1 - 1
    = 2
```

Index 2:

```text
[3]
```

So:

```text
[1] → [2] → [3] → [4]
              ↑      ↑
             temp   delete
```

Then:

```cpp
temp->next = temp->next->next;
```

Since `[4]->next = NULL`:

```text
[3]->next = NULL
```

Output:

```text
[1] → [2] → [3] → NULL
```

---

# 🔥 Dry Run 3 — Delete Head

Input:

```text
[1] → [2] → [3]
n = 3
```

Length:

```text
len = 3
```

Check:

```text
n == len
3 == 3
```

True.

Return:

```cpp
head->next;
```

Output:

```text
[2] → [3]
```

---

# 🔥 Dry Run 4 — Only One Node

Input:

```text
[1]
n = 1
```

Length:

```text
len = 1
```

Condition:

```text
n == len
1 == 1
```

True.

```cpp
return head->next;
```

Since:

```text
head->next = NULL
```

Output:

```text
NULL
```

Correct.

---

# 🆚 Two-Pass vs Slow-Fast

## Our Approach

```text
Pass 1:
length calculate

Pass 2:
correct position par jao
```

Complexity:

```text
Time  = O(n)
Space = O(1)
```

---

## Slow-Fast Approach

```text
fast ko n steps aage
↓
slow + fast together
↓
slow previous node par
```

Complexity:

```text
Time  = O(n)
Space = O(1)
```

Both are valid.

### Why we use this one here?

Because tum simple indexing approach practice kar rahe ho:

```text
length
↓
position
↓
traverse
↓
skip node
```

---

# 🧠 Important Formula

For:

```text
len = total nodes
n = nth node from end
```

### Delete node ka 0-based index:

```text
len - n
```

### Delete node ke previous ka 0-based index:

```text
len - n - 1
```

Humko previous node chahiye, isliye:

```cpp
pos = len - n - 1;
```

---

# ⭐ Core Operation

Almost pura deletion isi line par depend karta hai:

```cpp
temp->next = temp->next->next;
```

Meaning:

```text
temp → DELETE → next
```

becomes:

```text
temp ─────────→ next
```

---

# 🔥 Pattern Recognition

Agar linked list question bole:

```text
Remove nth node from end
```

Do possible approaches immediately yaad karo:

### Approach 1

```text
Length → Position → Traverse → Delete
```

### Approach 2

```text
Slow + Fast
```

Is implementation me:

```text
Length → Position
```

use kiya hai.

---

# 📊 Complexity

### Time

```text
O(n)
```

First traversal:

```text
O(n)
```

Second traversal:

```text
O(n)
```

Total:

```text
O(n) + O(n)
= O(n)
```

Constant factors ignore karne par.

### Space

```text
O(1)
```

Sirf:

```cpp
temp
len
pos
```

use kiya.

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
├── 07-Linked-List-Cycle-II.md
└── 08-Remove-Nth-Node-From-End.md
```

### Git Commit

```text
Add LC 19 Remove Nth Node From End
```

---

# ⭐ One-Line Memory Trick

> **Pehle length nikalo → `pos = len-n-1` se previous node ka index nikalo → `temp` ko wahan le jao → `temp->next = temp->next->next` karke node skip karo.**
