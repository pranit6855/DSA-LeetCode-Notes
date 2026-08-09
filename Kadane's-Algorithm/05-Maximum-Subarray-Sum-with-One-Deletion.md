# 🔥 LeetCode 1186 — Maximum Subarray Sum with One Deletion

## 📌 Problem

Hume ek integer array `arr` diya gaya hai.

Hume ek **non-empty subarray** ka maximum sum find karna hai.

Lekin ek extra option hai:

> Hum **maximum 1 element delete** kar sakte hain.

Important:

- Subarray contiguous hona chahiye.
- At most **one element** delete kar sakte hain.
- Final subarray empty nahi ho sakta.

---

# 🔹 Example 1

```text
arr = [1,-2,0,3]
```

Agar normal Kadane lagaye:

```text
1 + (-2) + 0 + 3 = 2
```

Lekin hum `-2` delete kar sakte hain:

```text
1 → 0 → 3
```

Sum:

```text
1 + 0 + 3 = 4
```

So:

```text
Output = 4
```

---

# 🔹 Example 2

```text
arr = [1,-2,-2,3]
```

Agar `-2` me se ek delete karein:

```text
1 → -2 → 3
```

Sum:

```text
1 - 2 + 3 = 2
```

But sirf:

```text
3
```

ka sum `3` hai.

So answer:

```text
3
```

---

# 🔹 Example 3

```text
arr = [-1,-1,-1]
```

Hum ek element delete kar sakte hain.

Lekin saare elements delete nahi kar sakte.

Best possible non-empty subarray:

```text
[-1]
```

So:

```text
Output = -1
```

---

# 🧠 Normal Kadane Se Kya Difference Hai?

LC 53 me hum sirf ye track karte the:

```text
bestending
```

Matlab:

> Current index par end hone wale subarray ka maximum sum.

Normal Kadane:

```text
Current element
       ↓
┌──────┴──────┐
↓             ↓
New start    Continue
```

Formula:

```cpp
bestending = max(
    arr[i],
    bestending + arr[i]
);
```

---

# 🔥 Ab Deletion Allowed Hai

Ab hume decide karna padega:

```text
Current element ko KEEP karna hai?
             OR
Current element ko DELETE karna hai?
```

Isliye sirf ek state enough nahi hai.

Hum **2 states** maintain karenge:

```text
noDelete
oneDelete
```

---

# 1️⃣ `noDelete`

`noDelete` ka meaning:

> Current index par end hone wale subarray ka maximum sum jisme **abhi tak koi element delete nahi hua**.

Ye exactly normal Kadane hai.

Formula:

```cpp
newNoDelete = max(
    arr[i],
    noDelete + arr[i]
);
```

### Do choices:

```text
arr[i]
```

→ Current element se new subarray start karo.

OR

```text
noDelete + arr[i]
```

→ Purana subarray continue karo.

---

# 2️⃣ `oneDelete`

`oneDelete` ka meaning:

> Current index par end hone wale subarray ka maximum sum jisme **one deletion use ho chuka hai**.

Yahan 2 possibilities hain.

---

## Case A — Current Element Delete Karo

Suppose:

```text
arr = [1,-2,3]
```

Current:

```text
-2
```

Agar `-2` delete kar diya:

```text
1
```

Previous state:

```text
noDelete = 1
```

So:

```text
newOneDelete = oldNoDelete
```

Important:

> Current element delete kiya hai, isliye current element ko sum me add nahi karenge.

---

## Case B — Deletion Pehle Hi Ho Chuka Hai

Suppose:

```text
[1,-2,3]
```

Humne `-2` already delete kar diya.

Ab `3` aaya.

To `3` ko keep karenge:

```text
1 + 3 = 4
```

Previous state:

```text
oneDelete = 1
```

So:

```text
newOneDelete = oldOneDelete + arr[i]
```

---

# 🔥 `oneDelete` Formula

Dono possibilities:

```text
1. Current element delete karo

oldNoDelete
```

OR

```text
2. Deletion already use ho chuka hai

oldOneDelete + arr[i]
```

Therefore:

```cpp
newOneDelete = max(
    oldNoDelete,
    oldOneDelete + arr[i]
);
```

---

# 🧠 Visual Representation

```text
                 arr[i]
                   ↓
          ┌────────┴────────┐
          ↓                 ↓
      KEEP current      DELETE current
          ↓                 ↓
     noDelete          oldNoDelete
          ↓
     oneDelete
```

More clearly:

```text
                 Current Element
                       ↓
        ┌──────────────┴──────────────┐
        ↓                             ↓
     KEEP it                       DELETE it
        ↓                             ↓
oldOneDelete + arr[i]          oldNoDelete
        │                             │
        └──────────────┬──────────────┘
                       ↓
                 newOneDelete
```

---

# 🔥 Complete State Diagram

## `noDelete`

```text
oldNoDelete
     │
     ├──────────────→ + arr[i]
     │
     │
     └── OR ──→ arr[i]
                     ↓
               newNoDelete
```

Formula:

```cpp
newNoDelete = max(
    arr[i],
    oldNoDelete + arr[i]
);
```

---

## `oneDelete`

```text
oldNoDelete
     │
     │ current DELETE
     ↓
newOneDelete

OR

oldOneDelete
     │
     │ current KEEP
     ↓
oldOneDelete + arr[i]
     ↓
newOneDelete
```

Formula:

```cpp
newOneDelete = max(
    oldNoDelete,
    oldOneDelete + arr[i]
);
```

---

# 🔥 Why Two States?

Suppose:

```text
arr = [1,-2,0,3]
```

Hume track karna hai:

```text
Without deletion:
1 → -2 → 0 → 3
```

Aur:

```text
With deletion:
1 → [delete -2] → 0 → 3
```

Dono ka state alag hai.

At `0`:

```text
noDelete
```

ka matlab:

```text
Abhi tak kuch delete nahi kiya.
```

While:

```text
oneDelete
```

ka matlab:

```text
Ek deletion already use ho chuka hai.
```

Isliye dono ko separately maintain karna zaroori hai.

---

# 🔄 Detailed Dry Run

Array:

```text
[1,-2,0,3]
```

Initially:

```text
noDelete = 1
oneDelete = 1
ans = 1
```

Why?

First element se hi start karna padega because subarray **non-empty** hona chahiye.

---

## i = 1

Current:

```text
-2
```

### `noDelete`

Choices:

```text
c1 = -2
c2 = 1 + (-2)
   = -1
```

So:

```text
newNoDelete = max(-2,-1)
             = -1
```

---

### `oneDelete`

Choice 1:

Current `-2` ko delete karo:

```text
oldNoDelete = 1
```

Choice 2:

Deletion already use hua hota:

```text
oldOneDelete + (-2)
= 1 - 2
= -1
```

So:

```text
newOneDelete = max(1,-1)
             = 1
```

State:

```text
noDelete = -1
oneDelete = 1
ans = 1
```

---

# i = 2

Current:

```text
0
```

### `noDelete`

```text
c1 = 0

c2 = -1 + 0
   = -1
```

Therefore:

```text
newNoDelete = 0
```

---

### `oneDelete`

Delete current `0`:

```text
oldNoDelete = -1
```

OR deletion already used:

```text
oldOneDelete + 0
= 1 + 0
= 1
```

Therefore:

```text
newOneDelete = max(-1,1)
             = 1
```

State:

```text
noDelete = 0
oneDelete = 1
ans = 1
```

---

# i = 3

Current:

```text
3
```

### `noDelete`

```text
c1 = 3

c2 = 0 + 3
   = 3
```

So:

```text
newNoDelete = 3
```

---

### `oneDelete`

Delete current `3`:

```text
oldNoDelete = 0
```

OR deletion already used:

```text
oldOneDelete + 3
= 1 + 3
= 4
```

So:

```text
newOneDelete = max(0,4)
             = 4
```

Final:

```text
ans = 4
```

---

# 🎯 Actual Best Subarray

Original:

```text
[1,-2,0,3]
```

Delete:

```text
-2
```

Remaining:

```text
[1,0,3]
```

Sum:

```text
1 + 0 + 3 = 4
```

Therefore:

```text
Answer = 4
```

---

# ⚠️ Most Important Concept

`oneDelete` ka naam dekh kar confuse mat hona.

Ye:

```text
"Exactly one deletion is mandatory"
```

nahi hai.

Hum **at most one deletion** allow kar rahe hain.

Isliye `oneDelete` state me deletion use hua ho sakta hai, aur kuch cases me same maximum sum deletion ke bina bhi represent ho sakta hai.

Example:

```text
[5]
```

Answer:

```text
5
```

Hum `5` ko delete nahi karenge because subarray empty ho jayega.

---

# ⚠️ Why `oneDelete = arr[0]`?

Hum initially:

```cpp
int noDelete = arr[0];
int oneDelete = arr[0];
```

rakhte hain.

`0` nahi.

Agar:

```text
arr = [-5,-2,-8]
```

ho aur hum `0` initialize karein, to algorithm galat tarah se empty subarray ko consider kar sakta hai.

But problem me:

```text
Subarray must be non-empty.
```

So first element se initialize karna safer hai.

---

# 🔥 Why Temporary Variables?

Code me:

```cpp
int newNoDelete = ...
int newOneDelete = ...
```

use karna important hai.

Because:

```text
newOneDelete
```

calculate karte time hume **old `noDelete`** chahiye.

Agar pehle hi:

```cpp
noDelete = newNoDelete;
```

kar diya, then:

```cpp
oneDelete
```

me new value accidentally use ho jayegi.

---

# ❌ Wrong Order

```cpp
noDelete = max(arr[i], noDelete + arr[i]);

oneDelete = max(noDelete, oneDelete + arr[i]);
```

Problem:

```text
oneDelete
```

ab **old noDelete** nahi, balki **new noDelete** use karega.

---

# ✅ Correct Order

Pehle:

```cpp
int newNoDelete = ...
int newOneDelete = ...
```

Then:

```cpp
noDelete = newNoDelete;
oneDelete = newOneDelete;
```

---

# 💻 Final C++ Code

```cpp
class Solution {
public:
    int maximumSum(vector<int>& arr) {

        int noDelete = arr[0];
        int oneDelete = arr[0];
        int ans = arr[0];

        for(int i = 1; i < arr.size(); i++) {

            int c1 = arr[i];
            int c2 = noDelete + arr[i];

            int newNoDelete = max(c1, c2);

            int d1 = noDelete;
            int d2 = oneDelete + arr[i];

            int newOneDelete = max(d1, d2);

            noDelete = newNoDelete;
            oneDelete = newOneDelete;

            ans = max(ans, max(noDelete, oneDelete));
        }

        return ans;
    }
};
```

---

# 🧩 Code Line-by-Line

### Initial states

```cpp
int noDelete = arr[0];
int oneDelete = arr[0];
int ans = arr[0];
```

First element se start.

---

### Normal Kadane

```cpp
int c1 = arr[i];
int c2 = noDelete + arr[i];
```

Choices:

```text
New subarray
OR
Old subarray continue
```

Then:

```cpp
int newNoDelete = max(c1, c2);
```

---

### Deletion State

```cpp
int d1 = noDelete;
```

Meaning:

```text
Current element DELETE karo.
```

Because current element add nahi hua.

---

```cpp
int d2 = oneDelete + arr[i];
```

Meaning:

```text
Deletion pehle hi ho chuka hai,
current element ko KEEP karo.
```

Then:

```cpp
int newOneDelete = max(d1, d2);
```

---

### Update States

```cpp
noDelete = newNoDelete;
oneDelete = newOneDelete;
```

---

### Global Answer

```cpp
ans = max(ans, max(noDelete, oneDelete));
```

Current position par dono states me jo best hai, usko overall answer se compare karo.

---

# 🔥 Example — Why Deletion Helps

```text
arr = [1,-2,0,3]
```

Normal Kadane:

```text
1 + (-2) + 0 + 3
= 2
```

With deletion:

```text
1 + 0 + 3
= 4
```

So:

```text
noDelete = 3
oneDelete = 4
```

Answer:

```text
4
```

---

# 🔥 Example — Deletion Doesn't Help

```text
arr = [1,2,3,4]
```

Normal maximum:

```text
1 + 2 + 3 + 4
= 10
```

Deleting any positive number decreases the sum.

So:

```text
noDelete = 10
```

and answer:

```text
10
```

This is why final answer checks **both states**.

---

# 🔥 Example — All Negative

```text
arr = [-1,-2,-3]
```

Best non-empty subarray:

```text
[-1]
```

Answer:

```text
-1
```

We cannot return `0`, because that would mean taking an empty subarray.

---

# 🧠 Pattern Connection With Previous Questions

## LC 53 — Maximum Subarray

One state:

```text
bestending
```

```text
bestending =
max(
    current,
    bestending + current
)
```

---

## LC 152 — Maximum Product

Two states:

```text
bestending
worstending
```

Because:

```text
negative × negative = positive
```

---

## LC 918 — Circular Subarray

Use:

```text
normal maximum
+
minimum subarray
+
total sum
```

---

## LC 1186 — One Deletion

Two states:

```text
noDelete
oneDelete
```

Because:

```text
Current KEEP
OR
Current DELETE
```

---

# 🔥 Kadane Pattern Evolution

```text
LC 53
   ↓
One state
bestending
   ↓
LC 152
   ↓
Two states
max + min
   ↓
LC 918
   ↓
Maximum + Minimum + Total
   ↓
LC 1186
   ↓
Two states
noDelete + oneDelete
```

---

# ⚠️ Common Mistakes

### 1. Deletion ko compulsory samajhna

Wrong thinking:

```text
Ek element delete karna hi padega.
```

Actually:

```text
At most ONE deletion
```

allowed hai.

---

### 2. `oneDelete` ko sirf deleted element samajhna

`oneDelete` ek **state** hai.

It represents:

```text
Current index tak maximum sum
with at most one deletion available/used in the state.
```

---

### 3. Current element delete karte time usko add karna

Wrong:

```cpp
oldNoDelete + arr[i]
```

if current is deleted.

Correct:

```cpp
oldNoDelete
```

Because deleted element sum me nahi aayega.

---

### 4. Old state overwrite kar dena

Wrong:

```cpp
noDelete = newNoDelete;
oneDelete = max(noDelete, ...);
```

Correct:

```cpp
newNoDelete
newOneDelete

↓
then update both
```

---

### 5. `0` initialization

Wrong:

```cpp
noDelete = 0;
oneDelete = 0;
```

All-negative arrays me empty subarray accidentally consider ho sakta hai.

Correct:

```cpp
arr[0]
```

---

# ⏱️ Complexity

### Time

```text
O(n)
```

Array ko ek baar traverse karte hain.

### Space

```text
O(1)
```

Sirf kuch variables maintain kar rahe hain.

---

# 🧠 Quick Revision

Remember:

```text
noDelete
    ↓
Normal Kadane
```

```text
oneDelete
    ↓
Current delete
OR
Deletion already used
```

Formulas:

```cpp
newNoDelete =
    max(arr[i], noDelete + arr[i]);
```

```cpp
newOneDelete =
    max(noDelete, oneDelete + arr[i]);
```

Final:

```cpp
ans = max(ans, max(noDelete, oneDelete));
```

---

# ⭐ One-Line Memory Trick

> **`noDelete` normal Kadane hai; `oneDelete` me ya to current element ko delete karo (`oldNoDelete`) ya pehle deletion ho chuka hai aur current ko add karo (`oldOneDelete + current`).**

---

# 📁 GitHub Structure

```text
Kadane's-Algorithm/
│
├── 01-Maximum-Subarray.md
├── 02-Maximum-Product-Subarray.md
├── 03-Maximum-Sum-Circular-Subarray.md
├── 04-Best-Time-to-Buy-and-Sell-Stock.md
└── 05-Maximum-Subarray-Sum-with-One-Deletion.md
```

### Commit Message

```text
Add LC 1186 Maximum Subarray Sum with One Deletion
```
