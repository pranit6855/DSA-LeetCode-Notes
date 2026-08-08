# LeetCode 287 - Find the Duplicate Number

## 📌 Problem

Hume ek array diya gaya hai.

- Array size = `n + 1`
- Har element `1` se `n` ke beech hoga.
- Sirf **ek number duplicate** hai.
- Duplicate number return karna hai.

⚠️ Constraints

```text
Array modify nahi kar sakte.

Extra Space O(1) honi chahiye.
```

Isliye HashSet ya Sorting use nahi kar sakte.

---

# 🔹 Example

```text
nums = [1,3,4,2,2]
```

Output

```text
2
```

---

# 🧠 Main Idea

Ye question dekhne me Array ka hai.

Lekin isse hum Linked List ki tarah treat karte hain.

Rule

```text
Current Index

↓

nums[index]

↓

Next Index
```

Example

```text
nums = [1,3,4,2,2]

Index

0 1 2 3 4

Value

1 3 4 2 2
```

Connection

```text
0 → 1

1 → 3

3 → 2

2 → 4

4 → 2
```

Diagram

```text
0 → 1 → 3 → 2 → 4
          ↑       ↓
          ← ← ← ← ←
```

Dekho

```text
2
```

par cycle ban gayi.

Aur wahi duplicate number bhi hai.

---

# 🔥 Why Slow & Fast Pointer?

Ye question exactly

```text
LC 142
```

jaisa hai.

Bas difference itna hai

Linked List

```cpp
slow = slow->next;

fast = fast->next->next;
```

Array

```cpp
slow = nums[slow];

fast = nums[nums[fast]];
```

---

# 🔥 Variables

```cpp
int slow = 0;

int fast = 0;
```

⚠️ Important

Yahan

```text
0
```

se initialize karna hai.

Not

```cpp
nums[0]
```

Kyunki

```text
Index 0
```

hamara starting point hai.

---

# 🔥 Step 1 - Detect Cycle

```cpp
while(true)
```

Guaranteed duplicate hai.

Matlab cycle bhi guaranteed hai.

---

# 🔥 Step 2 - Move Slow

```cpp
slow = nums[slow];
```

Slow

```text
1 step
```

move karega.

---

# 🔥 Step 3 - Move Fast

```cpp
fast = nums[nums[fast]];
```

Fast

```text
2 steps
```

move karega.

---

# 🔥 Step 4 - Meeting Point

```cpp
if(slow == fast)
```

Agar dono mil gaye.

Matlab cycle detect ho gayi.

Loop stop.

---

# 🔥 Step 5 - New Pointer

```cpp
int temp = 0;
```

Yahan

```text
Head = Index 0
```

isliye

```cpp
temp = 0;
```

---

# 🔥 Step 6 - Move Both

```cpp
while(temp != slow){

    temp = nums[temp];

    slow = nums[slow];
}
```

Ab

```text
temp

↓

1 step
```

Aur

```text
slow

↓

1 step
```

Dono move karenge.

Jahan milenge,

Wahi duplicate number hai.

---

# 🔥 Step 7 - Return

```cpp
return slow;
```

Ya

```cpp
return temp;
```

Dono same honge.

---

# 🔄 Dry Run

```text
nums = [1,3,4,2,2]
```

Initially

```text
slow = 0

fast = 0
```

↓

Iteration 1

```text
slow = 1

fast = 3
```

↓

Iteration 2

```text
slow = 3

fast = 4
```

↓

Iteration 3

```text
slow = 2

fast = 4
```

↓

Iteration 4

```text
slow = 4

fast = 4
```

Meeting ✔️

---

Second Phase

```text
temp = 0

slow = 4
```

↓

```text
temp = 1

slow = 2
```

↓

```text
temp = 3

slow = 4
```

↓

```text
temp = 2

slow = 2
```

Meeting

Answer

```text
2
```

---

# 💻 C++ Code

```cpp
class Solution {
public:

    int findDuplicate(vector<int>& nums) {

        int slow = 0;
        int fast = 0;

        while(true){

            slow = nums[slow];

            fast = nums[nums[fast]];

            if(slow == fast){
                break;
            }
        }

        int temp = 0;

        while(temp != slow){

            temp = nums[temp];

            slow = nums[slow];
        }

        return slow;
    }
};
```

---

# 🔥 Most Important Concept

Treat Array like Linked List.

```text
Index

↓

Value

↓

Next Index
```

Cycle ka starting point hi duplicate number hota hai.

---

# 🔥 Flow

```text
Start

↓

slow = 0

fast = 0

↓

slow = nums[slow]

↓

fast = nums[nums[fast]]

↓

Meet ?

↓

YES

↓

temp = 0

↓

temp = nums[temp]

↓

slow = nums[slow]

↓

Meet Again

↓

Return slow
```

---

# ⏱️ Time Complexity

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

Wrong

```cpp
int slow = nums[0];
```

Correct

```cpp
int slow = 0;
```

---

### 2.

Wrong

```cpp
int fast = nums[0];
```

Correct

```cpp
int fast = 0;
```

---

### 3.

Wrong

```cpp
fast = nums[fast];
```

Correct

```cpp
fast = nums[nums[fast]];
```

Fast hamesha

```text
2 steps
```

move karega.

---

### 4.

Wrong

```cpp
while(temp != NULL)
```

Correct

```cpp
while(temp != slow)
```

---

### 5.

Wrong

```cpp
temp = nums[0];
```

Correct

```cpp
temp = 0;
```

---

# 🔥 Quick Revision

```text
Treat Array as Linked List

↓

Slow = 1 step

↓

Fast = 2 steps

↓

Meet

↓

temp = 0

↓

temp & slow

1 step

↓

Meet Again

↓

Duplicate Number
```

---

# 🧠 One-Line Revision

Array ko Linked List ki tarah treat karo. Floyd Cycle Detection use karo. Meeting ke baad ek pointer index `0` se aur ek meeting point se 1-1 step move karo. Jahan dono milenge wahi duplicate number hoga.

---

# ⭐ Interview Revision

```text
Array

↓

Index → Value

↓

Cycle

↓

Slow & Fast

↓

Meeting

↓

temp = 0

↓

Meet Again

↓

Duplicate Number
```

Pattern

```text
Slow & Fast Pointer

↓

Floyd Cycle Detection

↓

Find Duplicate Number
```
