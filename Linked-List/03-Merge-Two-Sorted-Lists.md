# 🔗 LeetCode 21 — Merge Two Sorted Lists

## 📌 Problem

Hume do **sorted singly linked lists** di gayi hain:

```text
list1:
[1] → [2] → [4] → NULL

list2:
[1] → [3] → [4] → NULL
```

Dono lists ko merge karke ek single sorted linked list return karni hai.

Expected:

```text
[1] → [1] → [2] → [3] → [4] → [4] → NULL
```

Important:

> Hume nodes ko merge karna hai; unnecessary new nodes ki complete list banane ki zarurat nahi hai. :contentReference[oaicite:1]{index=1}

---

# 🧠 Main Idea

Dono lists already sorted hain.

Isliye hume unhe dobara sort karne ki zarurat nahi.

Har step par:

```text
list1 ka current value
        VS
list2 ka current value
```

compare karenge.

Jo smaller hai:

```text
        ↓
result me attach
```

Phir **usi list ka pointer aage badhayenge**.

---

# 🔥 Example

```text
list1:
[1] → [2] → [4]

list2:
[1] → [3] → [4]
```

Compare:

```text
1 vs 1
```

Ek `1` choose karo.

Then:

```text
2 vs 1
```

`1` choose.

Then:

```text
2 vs 3
```

`2` choose.

Then:

```text
4 vs 3
```

`3` choose.

Then:

```text
4 vs 4
```

Ek `4` choose.

Finally doosre `4` ko attach kar do.

Result:

```text
[1] → [1] → [2] → [3] → [4] → [4]
```

---

# 🧩 Dummy Node

Result list banane ke liye ek dummy node use karenge.

```cpp
ListNode* dummy = new ListNode(-1);
ListNode* curr = dummy;
```

Initially:

```text
dummy
 ↓
[-1] → NULL
 ↑
curr
```

`-1` actual answer ka part nahi hai.

Ye sirf **helper node** hai.

---

# 🤔 Dummy Node Kyu?

Agar dummy use na karein, to first node ke liye special handling karni padegi:

```text
Kya result empty hai?
Agar haan:
    head = first selected node
```

Dummy se ye problem easy ho jati hai.

Hum directly:

```cpp
curr->next = selectedNode;
```

kar sakte hain.

Dummy node linked-list merging me head handling simplify karta hai. :contentReference[oaicite:2]{index=2}

---

# 🔹 `curr` Kya Karega?

`curr` merged list ke **last node** ko point karega.

Starting:

```text
dummy
 ↓
[-1] → NULL
 ↑
curr
```

First node attach karne ke baad:

```text
dummy
 ↓
[-1] → [1]
          ↑
         curr
```

Phir `curr` ko aage move:

```cpp
curr = curr->next;
```

Now:

```text
dummy
 ↓
[-1] → [1]
          ↑
         curr
```

Actually `curr` `[1]` node ko point kar raha hai.

Next selected node isi ke baad attach hoga.

---

# 🔥 Core Loop

Jab tak dono lists me nodes available hain:

```cpp
while(list1 != NULL && list2 != NULL)
```

Kyun?

Kyunki comparison tabhi possible hai jab:

```text
list1 exists
AND
list2 exists
```

---

# 1️⃣ Compare

```cpp
if(list1->val <= list2->val)
```

Agar list1 ka value smaller/equal hai:

```text
list1 ka node choose karo
```

Otherwise:

```text
list2 ka node choose karo
```

---

# 2️⃣ Selected Node Attach

Agar:

```text
list1->val <= list2->val
```

then:

```cpp
curr->next = list1;
```

Matlab:

```text
curr → list1
```

Example:

```text
curr
 ↓
[1] → NULL

list1
 ↓
[2] → [4]
```

After:

```text
curr
 ↓
[1] → [2] → [4]
```

---

# 3️⃣ Selected List Pointer Move

Agar list1 ka node use kar liya:

```cpp
list1 = list1->next;
```

Example:

Before:

```text
list1
 ↓
[2] → [4] → NULL
```

After:

```text
list1
       ↓
[4] → NULL
```

So selected node result me chala gaya aur `list1` next available node par aa gaya.

---

# 4️⃣ `curr` Move

Node attach karne ke baad:

```cpp
curr = curr->next;
```

Example:

```text
[1] → [2]
       ↑
      curr
```

Ab `curr` latest merged node par hai.

Next node isi ke baad attach hoga.

---

# 🔄 Complete Dry Run

Input:

```text
list1:
[1] → [2] → [4] → NULL

list2:
[1] → [3] → [4] → NULL
```

Initial:

```text
dummy
 ↓
[-1] → NULL
 ↑
curr
```

---

## Step 1

Compare:

```text
1 vs 1
```

`list1` choose.

```text
curr->next = list1;
```

Result:

```text
[-1] → [1]
          ↑
         curr
```

Move:

```text
list1 → [2] → [4]
```

---

## Step 2

Compare:

```text
2 vs 1
```

`list2` choose.

Result:

```text
[-1] → [1] → [1]
                ↑
               curr
```

Move:

```text
list2 → [3] → [4]
```

---

## Step 3

Compare:

```text
2 vs 3
```

`list1` choose.

```text
[-1] → [1] → [1] → [2]
                     ↑
                    curr
```

Move:

```text
list1 → [4]
```

---

## Step 4

Compare:

```text
4 vs 3
```

`list2` choose.

```text
[-1] → [1] → [1] → [2] → [3]
                          ↑
                         curr
```

Move:

```text
list2 → [4]
```

---

## Step 5

Compare:

```text
4 vs 4
```

Equal.

Because code me:

```cpp
<=
```

use kiya hai, list1 choose hoga.

```text
[-1] → [1] → [1] → [2] → [3] → [4]
                                 ↑
                                curr
```

Move:

```text
list1 = NULL
```

---

# ⚠️ One List Khatam

Ab:

```text
list1 = NULL
```

But:

```text
list2:
[4] → NULL
```

Ab comparison possible nahi hai.

Lekin list2 already sorted hai.

Isliye remaining list ko directly attach kar sakte hain:

```cpp
curr->next = list2;
```

Final:

```text
dummy
 ↓
[-1] → [1] → [1] → [2] → [3] → [4] → [4] → NULL
```

---

# 🤔 Remaining List Directly Kyu Attach Kar Sakte Hain?

Suppose:

```text
Result:
[1] → [2] → [3]

Remaining:
[4] → [5] → [6]
```

Remaining list already sorted hai.

Bas:

```cpp
curr->next = list2;
```

karne se:

```text
[1] → [2] → [3] → [4] → [5] → [6]
```

ban jayega.

Hume `[4]`, `[5]`, `[6]` ko individually process karne ki zarurat nahi.

Important pointer concept:

```text
curr->next = list2;
```

sirf `[4]` ko connect karta hua dikhta hai, but `[4]` ka existing `next` `[5]` ko point kar raha hai, `[5]` ka next `[6]` ko.

So **poori remaining chain attach ho jaati hai**.

---

# 🔥 `dummy->next` Kyu Return?

Final structure:

```text
dummy
 ↓
[-1] → [1] → [1] → [2] → [3] → [4] → [4]
         ↑
      actual head
```

Dummy ka `-1` actual answer nahi hai.

Actual merged list:

```text
[1] → [1] → [2] → [3] → [4] → [4]
 ↑
dummy->next
```

Therefore:

```cpp
return dummy->next;
```

---

# ❌ `return dummy` Kyu Wrong?

Agar:

```cpp
return dummy;
```

karoge:

```text
[-1] → [1] → [1] → [2] → ...
```

Extra `-1` output me aa jayega.

Isliye:

```cpp
return dummy->next;
```

---

# ❓ `dummy = NULL` Kyu Nahi?

Agar:

```cpp
ListNode* dummy = NULL;
```

kiya:

```text
dummy → NULL
```

Aur:

```cpp
curr = dummy;
```

to:

```text
curr → NULL
```

Ab:

```cpp
curr->next = list1;
```

invalid hoga.

Kyunki `curr` kisi actual node ko point hi nahi kar raha.

Dummy node ka purpose hi hai:

```text
dummy
 ↓
[-1] → ...
```

Ek starting/helper node provide karna.

---

# 🧠 Important Pointer Difference

Ye dono same nahi hain:

```cpp
ListNode* dummy = new ListNode(-1);
ListNode* curr = dummy;
```

Initially:

```text
dummy ──────┐
            ↓
          [-1]
            ↑
           curr
```

Dono pointers **same node** ko point kar rahe hain.

But baad me:

```cpp
curr = curr->next;
```

karne par:

```text
dummy
 ↓
[-1] → [1] → [2]
          ↑
         curr
```

`dummy` wahi purane `-1` par hai.

`curr` aage move kar gaya.

Isi wajah se hum:

```cpp
dummy->next
```

se beginning retrieve kar sakte hain.

---

# 💻 Final C++ Code

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {

        ListNode* dummy = new ListNode(-1);
        ListNode* curr = dummy;

        while(list1 != NULL && list2 != NULL) {

            if(list1->val <= list2->val) {

                curr->next = list1;
                list1 = list1->next;
            }
            else {

                curr->next = list2;
                list2 = list2->next;
            }

            curr = curr->next;
        }

        if(list1 != NULL) {
            curr->next = list1;
        }
        else {
            curr->next = list2;
        }

        return dummy->next;
    }
};
```

---

# 🔍 Code Line-by-Line

### Dummy

```cpp
ListNode* dummy = new ListNode(-1);
```

Helper node.

---

### Current pointer

```cpp
ListNode* curr = dummy;
```

Merged list ke current end ko track karega.

---

### Compare

```cpp
while(list1 != NULL && list2 != NULL)
```

Dono lists ke nodes available hain tab tak comparison.

---

### List 1 smaller

```cpp
if(list1->val <= list2->val)
```

Then:

```cpp
curr->next = list1;
```

List1 ka node attach.

```cpp
list1 = list1->next;
```

List1 pointer move.

---

### Otherwise

```cpp
curr->next = list2;
list2 = list2->next;
```

List2 ka node attach aur pointer move.

---

### Curr Move

```cpp
curr = curr->next;
```

Merged list ke latest node par move.

---

### Remaining List

```cpp
if(list1 != NULL)
    curr->next = list1;
else
    curr->next = list2;
```

Jo list bachi hai, directly attach.

---

### Return

```cpp
return dummy->next;
```

Dummy ko skip karke actual merged list ka head return.

---

# ⚠️ Common Mistakes

## 1. `curr` ko move karna bhool jana

Agar:

```cpp
curr = curr->next;
```

nahi kiya, to har baar same position par node attach karoge.

---

## 2. Selected list pointer move nahi karna

Agar:

```cpp
list1 = list1->next;
```

nahi kiya, to same node repeatedly select ho sakta hai.

---

## 3. `dummy` return karna

Wrong:

```cpp
return dummy;
```

Correct:

```cpp
return dummy->next;
```

---

## 4. Remaining list ko individually process karna

Necessary nahi.

```cpp
curr->next = list1;
```

ya:

```cpp
curr->next = list2;
```

enough hai because remaining list already sorted hai.

---

# 🔥 Pattern Recognition

Ye question ek important **Two Pointers + Dummy Node** pattern hai.

```text
list1 pointer
      ↓
   compare
      ↑
list2 pointer

      ↓

   curr pointer
      ↓
merged result
```

### Pattern:

```text
COMPARE
   ↓
SMALLER NODE PICK
   ↓
ATTACH TO curr
   ↓
SELECTED LIST MOVE
   ↓
curr MOVE
   ↓
REPEAT
```

---

# 🧠 Quick Revision

### Initialization:

```cpp
dummy = new ListNode(-1);
curr = dummy;
```

### While both exist:

```cpp
while(list1 != NULL && list2 != NULL)
```

### Pick smaller:

```cpp
curr->next = list1;
```

or:

```cpp
curr->next = list2;
```

### Move selected list:

```cpp
list1 = list1->next;
```

or:

```cpp
list2 = list2->next;
```

### Move result pointer:

```cpp
curr = curr->next;
```

### Attach remaining:

```cpp
curr->next = list1;
```

or:

```cpp
curr->next = list2;
```

### Return:

```cpp
return dummy->next;
```

---

# ⏱️ Complexity

Let:

```text
n = length of list1
m = length of list2
```

### Time

```text
O(n + m)
```

Har node maximum ek baar process hota hai. :contentReference[oaicite:3]{index=3}

### Extra Space

```text
O(1)
```

Hum existing nodes ko reconnect kar rahe hain; new result list ke liye `O(n+m)` nodes create nahi kar rahe. :contentReference[oaicite:4]{index=4}

---

# 📁 Linked List Progress

```text
Linked-List/
│
├── 01-Convert-Binary-Number-to-Integer.md
├── 02-Middle-of-the-Linked-List.md
├── 03-Reverse-Linked-List.md
└── 04-Merge-Two-Sorted-Lists.md
```

### Git Commit

```text
Add LC 21 Merge Two Sorted Lists
```

### ⭐ One-Line Memory Trick

> **Dono sorted lists ke current nodes compare karo, chhota node `curr` ke baad attach karo, selected pointer aur `curr` ko move karo; ek list khatam ho to doosri ko directly attach karke `dummy->next` return karo.**
