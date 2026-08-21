# LeetCode 1019 — Next Greater Node In Linked List

**Pattern:** Stack — Next / Previous Greater-Smaller

---

# Problem

Hume ek singly linked list di gayi hai.

Har node ke liye hume uske right side ka **first greater value** find karna hai.

Agar koi greater value nahi milti, to:

```text
0
```

return karna hai.

---

# Example 1

Linked List:

```text
2 → 1 → 5
```

Output:

```text
[5,5,0]
```

### Explanation

For `2`:

Right side:

```text
1 → 5
```

First greater:

```text
5
```

So:

```text
2 → 5
```

For `1`:

Right side:

```text
5
```

Greater:

```text
5
```

For `5`:

Right side mein koi greater nahi hai.

So:

```text
5 → 0
```

Final:

```text
[5,5,0]
```

---

# Example 2

Linked List:

```text
1 → 7 → 5 → 1 → 9 → 2 → 5 → 1
```

Output:

```text
[7,9,9,9,0,5,0,0]
```

---

# Pattern Identification

Agar question mein:

```text
Next Greater
First Greater on Right
Next Larger
Greater Element
```

dikhe, to:

```text
MONOTONIC STACK
```

ka thought aana chahiye.

Ye Pattern #4 ka direct example hai:

```text
Next Greater Element
```

---

# Connection With Previous Problems

Humne Pattern #3 mein arrays ke saath ye concept kiya tha:

```text
LC 739 — Daily Temperatures
LC 496 — Next Greater Element I
LC 503 — Next Greater Element II
```

Unmein:

```text
current > stack top
→ stack top ka Next Greater mil gaya
→ pop
```

Yahan bhi exact same logic hai.

Bas difference:

```text
Input = Linked List
```

---

# Main Problem

Linked List mein direct right side access nahi hota.

Array mein:

```cpp
nums[i]
```

directly access kar sakte hain.

Linked List mein:

```text
current → next → next → next
```

karke aage jaana padta hai.

Agar har node ke liye manually aage search karenge, to worst-case:

```text
O(n²)
```

ho jayega.

Isliye Monotonic Stack use karenge.

---

# Main Idea

Pehle Linked List ko array/vector mein convert karenge.

Example:

Linked List:

```text
2 → 1 → 5
```

becomes:

```text
[2,1,5]
```

Ab normal Next Greater Element ki tarah solve kar sakte hain.

---

# Why Convert Linked List To Array?

Array mein hume:

```text
index
value
```

dono mil jaate hain.

Example:

```text
index:  0 1 2
value:  2 1 5
```

Ab same monotonic stack template use kar sakte hain:

```cpp
while (!st.empty() && nums[i] > nums[st.top()])
```

---

# Data Structures

Hum use karenge:

```cpp
vector<int> nums;
vector<int> ans;
stack<int> st;
```

### `nums`

Linked List ki values store karega.

### `ans`

Har node ka Next Greater Element store karega.

### `st`

Unresolved node indexes store karega.

---

# Algorithm

```text
Start
  ↓
Linked List ko vector mein convert karo
  ↓
Answer array banao, initially 0
  ↓
Empty Stack banao
  ↓
Array ko left → right traverse karo
  ↓
Current value > Stack Top value?
  ├── YES
  │    ↓
  │   Stack top ka Next Greater = current
  │    ↓
  │   answer update
  │    ↓
  │   pop
  │    ↓
  │   Again check
  │
  └── NO
       ↓
Current index push karo
  ↓
Loop complete
  ↓
Jo indexes Stack mein bache
unka greater element nahi mila
  ↓
Answer already 0 hai
  ↓
Return
```

---

# Step 1 — Linked List To Vector

```cpp
vector<int> nums;
```

Then:

```cpp
while (head != nullptr) {
    nums.push_back(head->val);
    head = head->next;
}
```

Suppose Linked List:

```text
2 → 1 → 5
```

Process:

```text
head → 2
```

Push:

```text
nums = [2]
```

Next:

```text
head → 1
```

Push:

```text
nums = [2,1]
```

Next:

```text
head → 5
```

Push:

```text
nums = [2,1,5]
```

End:

```text
head = nullptr
```

Final:

```text
nums = [2,1,5]
```

---

# Step 2 — Answer Array

```cpp
vector<int> ans(n, 0);
```

Initially:

```text
ans = [0,0,0]
```

Why `0`?

Because agar kisi node ka greater element nahi milega, answer `0` hi hona hai.

---

# Step 3 — Stack

```cpp
stack<int> st;
```

Important:

> Stack mein **indexes** store karenge.

Example:

```text
st = [0,1]
```

means:

```text
index 0
index 1
```

abhi unresolved hain.

---

# Step 4 — Traverse Array

```cpp
for (int i = 0; i < n; i++)
```

Array:

```text
[2,1,5]
```

left to right process hoga.

---

# Step 5 — While Condition

```cpp
while (!st.empty() &&
       nums[i] > nums[st.top()])
```

Meaning:

> Current value kya Stack ke top wale unresolved value se greater hai?

Agar yes:

> Current value hi uska Next Greater Element hai.

---

# Step 6 — Index Nikalo

```cpp
int index = st.top();
```

Suppose:

```text
st.top() = 1
```

So:

```text
index = 1
```

---

# Step 7 — Pop

```cpp
st.pop();
```

Ab us index ka answer mil chuka hai.

So usko Stack mein rakhne ki zarurat nahi.

---

# Step 8 — Answer Update

```cpp
ans[index] = nums[i];
```

Example:

```text
index = 1
nums[i] = 5
```

So:

```text
ans[1] = 5
```

---

# Step 9 — Current Index Push

```cpp
st.push(i);
```

Current node ka greater abhi future mein mil sakta hai.

Isliye usko Stack mein store karte hain.

---

# Complete C++ Code

```cpp
class Solution {
public:
    vector<int> nextLargerNodes(ListNode* head) {

        vector<int> nums;

        // Linked List -> Array
        while (head != nullptr) {
            nums.push_back(head->val);
            head = head->next;
        }

        int n = nums.size();

        vector<int> ans(n, 0);

        stack<int> st;

        for (int i = 0; i < n; i++) {

            while (!st.empty() &&
                   nums[i] > nums[st.top()]) {

                int index = st.top();
                st.pop();

                ans[index] = nums[i];
            }

            st.push(i);
        }

        return ans;
    }
};
```

---

# Detailed Dry Run

Input Linked List:

```text
2 → 1 → 5
```

---

## Step 1 — Convert To Array

Final vector:

```text
nums = [2,1,5]
```

Indexes:

```text
index:  0 1 2
value:  2 1 5
```

Initial:

```text
ans = [0,0,0]
stack = []
```

---

# i = 0

Current:

```text
nums[0] = 2
```

Stack empty.

While nahi chalega.

Push:

```cpp
st.push(0);
```

Stack:

```text
[0]
```

Meaning:

```text
index 0 → value 2
```

---

# i = 1

Current:

```text
nums[1] = 1
```

Stack:

```text
[0]
```

Top:

```text
index = 0
value = nums[0] = 2
```

Check:

```text
1 > 2
```

False.

So `2` ka greater abhi nahi mila.

Push current:

```cpp
st.push(1);
```

Stack:

```text
[0,1]
```

---

# i = 2

Current:

```text
nums[2] = 5
```

Stack:

```text
[0,1]
```

Top:

```text
index = 1
value = nums[1] = 1
```

Check:

```text
5 > 1
```

True.

So:

```text
1 → 5
```

Update:

```cpp
ans[1] = 5;
```

Answer:

```text
[0,5,0]
```

Pop:

```cpp
st.pop();
```

Stack:

```text
[0]
```

---

# While Again

Ab top:

```text
index = 0
value = nums[0] = 2
```

Check:

```text
5 > 2
```

True.

So:

```text
2 → 5
```

Update:

```cpp
ans[0] = 5;
```

Answer:

```text
[5,5,0]
```

Pop:

```cpp
st.pop();
```

Stack:

```text
[]
```

---

# Push Current 5

While ke baad:

```cpp
st.push(2);
```

Stack:

```text
[2]
```

Meaning:

```text
index 2 → value 5
```

---

# Loop Complete

Stack:

```text
[2]
```

Index 2 ka value:

```text
5
```

Right side mein koi greater value nahi.

Isliye:

```text
ans[2] = 0
```

Already `0` hai.

Final:

```text
[5,5,0]
```

---

# Detailed Example 2

Linked List:

```text
1 → 7 → 5 → 1 → 9 → 2 → 5 → 1
```

Convert:

```text
nums = [1,7,5,1,9,2,5,1]
```

Initial:

```text
ans = [0,0,0,0,0,0,0,0]
stack = []
```

### `1`

```text
stack = [0]
```

### `7`

```text
7 > 1
```

So:

```text
ans[0] = 7
```

Push `7`.

```text
stack = [1]
```

### `5`

```text
5 > 7
```

False.

Push:

```text
stack = [1,2]
```

### `1`

```text
1 > 5
```

False.

Push:

```text
stack = [1,2,3]
```

### `9`

Now:

```text
9 > 1
```

So:

```text
ans[3] = 9
```

Pop.

Again:

```text
9 > 5
```

So:

```text
ans[2] = 9
```

Pop.

Again:

```text
9 > 7
```

So:

```text
ans[1] = 9
```

Pop.

Again:

```text
9 > 1
```

So:

```text
ans[0] already = 7
```

But important:

Index `0` was already popped when `7` arrived.

So it is no longer in Stack.

This is exactly why every node gets its **first greater** element, not some later greater element.

Then `9` gets pushed.

This produces:

```text
[7,9,9,9,0,5,0,0]
```

---

# Why Is The First Greater Element Guaranteed?

This is the most important part of the monotonic stack.

Suppose:

```text
2 → 1 → 5
```

When `5` arrives:

Stack:

```text
[2,1]
```

Top `1` is popped first.

Then `2` is popped.

Since we're processing from left to right, `5` is the **first greater value encountered** for both unresolved elements.

Once an element is popped, its answer is final.

---

# Why Stack Stores Index, Not Value?

We could conceptually store values, but index is cleaner.

Stack:

```text
[0,1]
```

Then:

```cpp
nums[st.top()]
```

gives the corresponding value.

Example:

```text
st.top() = 1
nums[1] = 1
```

And answer update:

```cpp
ans[index] = nums[i];
```

becomes easy.

---

# Why `while`, Not `if`?

Suppose:

```text
nums = [2,1,5]
```

When `5` comes:

```text
5 > 1
```

so index `1` resolves.

After popping:

```text
5 > 2
```

also true.

So index `0` also resolves.

One current value can solve **multiple previous unresolved elements**.

Therefore:

```cpp
while(...)
```

not:

```cpp
if(...)
```

---

# Connection With LC 739

This should feel familiar.

### LC 739

```text
temperatures
```

We used:

```cpp
while (current > nums[st.top()])
```

### LC 1019

Exactly same:

```cpp
while (nums[i] > nums[st.top()])
```

Difference:

```text
739 → Array
1019 → Linked List
```

But because Linked List doesn't give direct index access conveniently, we first convert it to an array.

---

# Common Mistakes

## Mistake 1 — Directly traverse ahead for every node

```text
For every node:
    search all next nodes
```

This can become:

```text
O(n²)
```

Use Monotonic Stack instead.

---

## Mistake 2 — Stack values instead of indexes

Better:

```cpp
st.push(i);
```

because we need to update:

```cpp
ans[index]
```

---

## Mistake 3 — `if` instead of `while`

Wrong:

```cpp
if (!st.empty() && nums[i] > nums[st.top()])
```

Correct:

```cpp
while (!st.empty() && nums[i] > nums[st.top()])
```

because one current greater element can resolve multiple previous elements.

---

## Mistake 4 — Forgetting default `0`

```cpp
vector<int> ans(n, 0);
```

Important because remaining Stack elements have no greater element.

---

# Complexity

### Linked List → Array

```text
O(n)
```

### Monotonic Stack

Every index:

```text
push → once
pop  → once
```

So:

```text
O(n)
```

### Total Time

```text
O(n)
```

### Space

Array:

```text
O(n)
```

Stack:

```text
O(n)
```

Answer:

```text
O(n)
```

Overall:

```text
O(n)
```

---

# Interview Explanation

> I first convert the linked list into an array because direct random access is not available in a linked list. Then I use a monotonic decreasing stack of indices. For each value, while it is greater than the value at the stack top, the current value is the next greater element for that index, so I update the answer and pop it. Finally, I push the current index. Any indices remaining in the stack do not have a greater value, so their answer remains zero. The time complexity is O(n).

---

# Pattern Recognition

If you see:

```text
Next Greater
First Greater on Right
Nearest Greater
Linked List + next greater
```

think:

```text
MONOTONIC STACK
```

Core template:

```cpp
while (!st.empty() && nums[i] > nums[st.top()]) {

    int index = st.top();
    st.pop();

    ans[index] = nums[i];
}

st.push(i);
```

---

# One-Line Revision

> **Linked List ko array mein convert karo, unresolved indexes Stack mein rakho, aur jab current value Stack top se greater ho to current value hi uska Next Greater hai — answer update karke pop karo.**

---

# Pattern #4 Progress

* [x] LC 1019 — Next Greater Node In Linked List
* [ ] LC 1944 — Number of Visible People in a Queue
* [ ] LC 962 — Maximum Width Ramp

---

# Folder Structure

```text
Stack/
│
├── 01-Matching-Pairing/
│   ├── 01-Valid-Parentheses.md
│   ├── 02-Minimum-Add-to-Make-Parentheses-Valid.md
│   └── 03-Minimum-Remove-to-Make-Valid-Parentheses.md
│
├── 02-Stack-Simulation/
│
├── 03-Monotonic-Stack/
│   ├── 01-Daily-Temperatures.md
│   ├── 02-Next-Greater-Element-I.md
│   ├── 03-Next-Greater-Element-II.md
│   ├── 04-Online-Stock-Span.md
│   └── 05-Final-Prices-With-a-Special-Discount-in-a-Shop.md
│
├── 04-Next-Previous-Greater-Smaller/
│   └── 01-Next-Greater-Node-In-Linked-List.md
│
├── 05-Boundary-Contribution/
│
└── 06-Stack-Design/
```
