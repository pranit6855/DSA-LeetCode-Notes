# LeetCode 503 — Next Greater Element II

**Pattern:** Stack — Monotonic Stack

---

# Problem

Hume ek circular integer array `nums` diya gaya hai.

Har element ke liye hume uske right side ka **first greater element** find karna hai.

Agar greater element nahi milta:

```text
-1
```

return karna hai.

---

# What Does Circular Mean?

Normal array:

```text
[1,2,1]
```

Normally last element ke baad array khatam.

Lekin circular array mein last ke baad first element dobara aata hai:

```text
1 → 2 → 1 → 1 → 2 → 1 → ...
```

Isliye last element bhi array ke beginning mein greater element search kar sakta hai.

---

# Example 1

```text
Input:
nums = [1,2,1]

Output:
[2,-1,2]
```

### Explanation

Index `0`:

```text
1 → 2
```

Answer:

```text
2
```

Index `1`:

```text
2 → 1 → 1 → ...
```

Koi value `2` se greater nahi hai.

Answer:

```text
-1
```

Index `2`:

```text
1 → 1 → 2
```

Circularly next greater:

```text
2
```

Final:

```text
[2,-1,2]
```

---

# Example 2

```text
Input:
nums = [5,4,3,2,1]

Output:
[-1,5,5,5,5]
```

Circularly:

```text
1 → 5
2 → 5
3 → 5
4 → 5
5 → no greater
```

---

# Pattern Identification

Agar question mein:

```text
Next Greater
Next Larger
First Greater
Greater Element on Right
Circular Array
```

jaise words aayein, to:

```text
MONOTONIC STACK
```

ka thought aana chahiye.

---

# Connection With Previous Questions

## LC 739 — Daily Temperatures

```text
Current > Stack Top
→ Pop
→ Answer calculate
```

## LC 496 — Next Greater Element I

```text
Current > Stack Top
→ Pop
→ Stack Top ka answer = Current
```

## LC 503 — Next Greater Element II

Same monotonic stack:

```text
Current > Stack Top
→ Pop
```

Bas extra feature:

```text
Circular Array
→ 2 × n traversal
→ i % n
```

So:

> **LC 503 = LC 496 + Circular Array**

---

# Main Idea

Hum:

```text
Monotonic Decreasing Stack
```

use karenge.

Stack mein **indexes** store karenge.

Example:

```text
nums = [1,2,1]

stack:
[0,1,2]
```

Stack ke indexes ka use karke actual values compare karenge:

```cpp
nums[st.top()]
```

---

# Why Store Index?

Agar Stack mein:

```text
0,1,2
```

indexes hain, to:

```cpp
nums[st.top()]
```

se actual value mil sakti hai.

Example:

```text
st.top() = 2
nums[2] = 1
```

Isliye jab hum current value ko compare karte hain:

```cpp
nums[current] > nums[st.top()]
```

to comparison actual values ke beech hota hai.

---

# Main Trick — Circular Array

Normal array ko:

```text
[1,2,1]
```

circularly process karne ke liye hum usko logically:

```text
[1,2,1,1,2,1]
```

ki tarah consider karenge.

Actual array duplicate nahi karna.

Instead:

```cpp
for(int i = 0; i < 2 * n; i++)
```

use karenge.

---

# `i % n`

Actual index nikalne ke liye:

```cpp
int curidx = i % n;
```

Example:

```text
n = 3
```

Then:

```text
i = 0 → curidx = 0
i = 1 → curidx = 1
i = 2 → curidx = 2
i = 3 → curidx = 0
i = 4 → curidx = 1
i = 5 → curidx = 2
```

So:

```text
i:
0 1 2 3 4 5

curidx:
0 1 2 0 1 2
```

Ye circular behavior create karta hai.

---

# Algorithm

```text
Start
  ↓
n = size
  ↓
ans = -1 se initialize
  ↓
Empty Stack banao
  ↓
2*n times traverse karo
  ↓
current index = i % n
  ↓
Current value > Stack Top value?
  ├── YES
  │    ↓
  │   Stack Top ka Next Greater mil gaya
  │    ↓
  │   answer update
  │    ↓
  │   pop
  │    ↓
  │   Again check
  │
  └── NO
       ↓
Continue

  ↓
First n elements ke indexes Stack mein push karo
  ↓
Return ans
```

---

# Step-by-Step Approach

## Step 1 — `n`

```cpp
int n = nums.size();
```

Example:

```text
nums = [1,2,1]
n = 3
```

---

# Step 2 — Answer Array

```cpp
vector<int> ans(n, -1);
```

Initially:

```text
ans = [-1,-1,-1]
```

Agar kisi element ka greater nahi mila to `-1` already ready hai.

---

# Step 3 — Stack

```cpp
stack<int> st;
```

Important:

> Stack mein **indexes** store honge.

Not values.

Correct:

```cpp
st.push(curidx);
```

---

# Step 4 — Traverse Twice

```cpp
for(int i = 0; i < 2 * n; i++)
```

Why `2*n`?

Because circular array mein kisi element ko maximum next `n-1` positions tak search karna pad sakta hai.

Example:

```text
[1,2,1]
```

last `1` ko beginning ka `2` dekhna hai.

---

# Step 5 — Current Index

```cpp
int curidx = i % n;
```

Ye actual array index deta hai.

---

# Step 6 — Monotonic Stack While Loop

```cpp
while(!st.empty() && nums[curidx] > nums[st.top()])
```

Ye poore question ka main logic hai.

Meaning:

```text
Current value > Stack Top value
```

Agar true:

> Current value Stack top wale element ka Next Greater hai.

---

# Step 7 — Old Index Save Karo

```cpp
int index = st.top();
```

Suppose:

```text
st.top() = 2
```

Then:

```text
index = 2
```

---

# Step 8 — Answer Update

```cpp
ans[index] = nums[curidx];
```

Example:

```text
index = 2
current value = 2
```

Then:

```text
ans[2] = 2
```

---

# Step 9 — Pop

```cpp
st.pop();
```

Current greater element mil gaya, so old element ko Stack mein rakhne ki zarurat nahi.

---

# Step 10 — First Pass Mein Push

```cpp
if(i < n) {
    st.push(curidx);
}
```

Ye condition bahut important hai.

### First pass

```text
i = 0 to n-1
```

Elements ko Stack mein add karenge.

### Second pass

```text
i = n to 2n-1
```

Sirf circular effect se unresolved elements ko solve karenge.

Same index ko dobara Stack mein push nahi karenge.

---

# Complete C++ Code

```cpp
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {

        int n = nums.size();

        stack<int> st;

        vector<int> ans(n, -1);

        for(int i = 0; i < 2 * n; i++) {

            int curidx = i % n;

            while(!st.empty() &&
                  nums[curidx] > nums[st.top()]) {

                int index = st.top();

                ans[index] = nums[curidx];

                st.pop();
            }

            if(i < n) {
                st.push(curidx);
            }
        }

        return ans;
    }
};
```

---

# Detailed Dry Run

Input:

```text
nums = [1,2,1]
```

Indexes:

```text
index:  0  1  2
value:  1  2  1
```

Initial:

```text
stack = []
ans   = [-1,-1,-1]
```

---

# i = 0

```text
curidx = 0 % 3 = 0
```

Current value:

```text
nums[0] = 1
```

Stack empty.

While nahi chalega.

Since:

```text
0 < 3
```

push:

```cpp
st.push(0);
```

Stack:

```text
[0]
```

Meaning:

```text
index 0 → value 1
```

---

# i = 1

```text
curidx = 1 % 3 = 1
```

Current:

```text
nums[1] = 2
```

Stack:

```text
[0]
```

Top:

```text
st.top() = 0
```

Top value:

```text
nums[0] = 1
```

Check:

```text
2 > 1
```

True.

So index `0` ka Next Greater:

```text
2
```

Update:

```cpp
ans[0] = 2;
```

Answer:

```text
[2,-1,-1]
```

Pop:

```cpp
st.pop();
```

Stack:

```text
[]
```

Now first pass mein:

```text
1 < 3
```

So:

```cpp
st.push(1);
```

Stack:

```text
[1]
```

---

# i = 2

```text
curidx = 2 % 3 = 2
```

Current:

```text
nums[2] = 1
```

Top:

```text
st.top() = 1
```

Value:

```text
nums[1] = 2
```

Check:

```text
1 > 2
```

False.

No pop.

First pass:

```text
2 < 3
```

So push:

```cpp
st.push(2);
```

Stack:

```text
[1,2]
```

Answer:

```text
[2,-1,-1]
```

---

# First Pass Complete

Array:

```text
[1,2,1]
```

Stack:

```text
[1,2]
```

These are unresolved:

```text
index 1 → value 2
index 2 → value 1
```

Ab circular part start.

---

# i = 3

```text
curidx = 3 % 3 = 0
```

So again:

```text
nums[0] = 1
```

Stack:

```text
[1,2]
```

Top index:

```text
2
```

Top value:

```text
nums[2] = 1
```

Check:

```text
1 > 1
```

False.

Nothing happens.

Now:

```text
i < n
3 < 3
```

False.

So push nahi karenge.

Stack remains:

```text
[1,2]
```

---

# i = 4

```text
curidx = 4 % 3 = 1
```

Current:

```text
nums[1] = 2
```

Stack:

```text
[1,2]
```

Top index:

```text
2
```

Top value:

```text
nums[2] = 1
```

Check:

```text
2 > 1
```

True.

So:

```text
index 2 → next greater = 2
```

Update:

```cpp
ans[2] = 2;
```

Answer:

```text
[2,-1,2]
```

Pop:

```cpp
st.pop();
```

Stack:

```text
[1]
```

Check while again.

Top index:

```text
1
```

Top value:

```text
nums[1] = 2
```

Current:

```text
2
```

Check:

```text
2 > 2
```

False.

Stop.

No push because:

```text
4 < 3
```

false.

---

# i = 5

```text
curidx = 5 % 3 = 2
```

Current:

```text
nums[2] = 1
```

Stack:

```text
[1]
```

Top value:

```text
nums[1] = 2
```

Check:

```text
1 > 2
```

False.

No pop.

No push.

---

# Final

```text
ans = [2,-1,2]
```

Return:

```text
[2,-1,2]
```

---

# Why Index 2 Gets `2`

Original:

```text
index: 0 1 2
value: 1 2 1
```

Index `2` is last:

```text
1
```

Circularly after index `2`:

```text
index 0 → 1
index 1 → 2
```

The first greater value is:

```text
2
```

So:

```text
ans[2] = 2
```

---

# Why Index 1 Gets `-1`

Index `1`:

```text
value = 2
```

Circularly:

```text
2 → 1 → 1 → 2 → ...
```

Koi value `2` se greater nahi hai.

So:

```text
ans[1] = -1
```

---

# Why We Do Not Push in Second Round

Important:

```cpp
if(i < n) {
    st.push(curidx);
}
```

First round:

```text
0,1,2
```

Push.

Second round:

```text
3,4,5
```

No push.

Agar second round mein bhi push kar diya:

```text
same indexes duplicate
```

ho jayenge.

Isse Stack galat ho sakta hai.

---

# Why Stack Stores Index, Not Value

Tumhare code mein ye correction important tha.

Correct:

```cpp
st.push(curidx);
```

Because Stack contains:

```text
0,1,2
```

Then:

```cpp
nums[st.top()]
```

actual value deta hai.

Example:

```text
st.top() = 2
nums[2] = 1
```

Agar tum:

```cpp
st.push(nums[curidx]);
```

karte:

```text
stack = [1,2,1]
```

then `st.top()` value hoti, index nahi.

Phir:

```cpp
nums[st.top()]
```

incorrect meaning dene lagega.

So:

> **Agar `nums[st.top()]` use kar rahe ho, to Stack mein index hi store karo.**

---

# Why `while`, Not `if`?

Example:

```text
nums = [1,2,3]
```

Suppose Stack:

```text
[0,1]
```

Values:

```text
1,2
```

Current:

```text
3
```

Then:

```text
3 > 2
```

So index `1` solve.

Pop.

Again:

```text
3 > 1
```

So index `0` bhi solve.

Isliye:

```cpp
while(...)
```

required hai.

---

# Monotonic Stack Property

Stack values decreasing order mein maintain hoti hain.

Example:

```text
Stack indexes:
[1,2]

Values:
[2,1]
```

So:

```text
2 > 1
```

Current bigger aaye:

```text
2 > 1
```

small value pop.

This maintains the monotonic property.

---

# Common Mistake

Wrong:

```cpp
st.push(nums[curidx]);
```

Correct:

```cpp
st.push(curidx);
```

Because:

```cpp
nums[st.top()]
```

requires `st.top()` to be an index.

---

# Common Mistake

Wrong:

```cpp
while(!st.empty()) {
    ...
    st.pop();
}

st.push(curidx);
```

without controlling second round.

Correct:

```cpp
if(i < n) {
    st.push(curidx);
}
```

because only first pass mein push karna hai.

---

# Common Mistake

Wrong:

```cpp
for(int i = 0; i < n; i++)
```

Ye circular array ka full search nahi karega.

Correct:

```cpp
for(int i = 0; i < 2 * n; i++)
```

---

# Common Mistake

Wrong:

```cpp
nums[i]
```

because `i` `n` se bada ho sakta hai.

Correct:

```cpp
int curidx = i % n;
nums[curidx]
```

---

# Complexity

## Time Complexity

```text
O(n)
```

Although hum `2*n` iterations kar rahe hain, constant `2` ignore hota hai.

Har index Stack mein maximum:

```text
1 push
1 pop
```

kar sakta hai.

---

## Space Complexity

```text
O(n)
```

Stack and answer array.

---

# Interview Explanation

> I use a decreasing monotonic stack of indices. Since the array is circular, I traverse it twice using `2*n` iterations and use `i % n` to map the virtual index back to the original array. Whenever the current value is greater than the value at the stack top, the current value is the next greater element for that index, so I update the answer and pop it. I only push indices during the first pass to avoid duplicates. The time complexity is O(n) and space complexity is O(n).

---

# Connection With LC 739

### LC 739

```text
Current > nums[stack.top()]
        ↓
Pop
        ↓
ans[index] = currentIndex - index
```

### LC 503

```text
Current > nums[stack.top()]
        ↓
Pop
        ↓
ans[index] = currentValue
```

Difference:

```text
739 → distance
503 → greater value
```

---

# Core Template

```cpp
for(int i = 0; i < 2 * n; i++) {

    int curidx = i % n;

    while(!st.empty() &&
          nums[curidx] > nums[st.top()]) {

        int index = st.top();
        st.pop();

        ans[index] = nums[curidx];
    }

    if(i < n) {
        st.push(curidx);
    }
}
```

Is template ko samajh lena = **Circular Monotonic Stack ka main pattern samajh lena**.

---

# One-Line Revision

> **Circular array ke liye array ko 2 times logically traverse karo, `i % n` se index nikalo, current value agar Stack top se greater ho to top ko pop karke uska answer current value bana do.**

---

# Pattern #3 Progress

* [x] LC 739 — Daily Temperatures
* [x] LC 496 — Next Greater Element I
* [x] LC 503 — Next Greater Element II
* [ ] LC 901 — Online Stock Span
* [ ] LC 1475 — Final Prices With a Special Discount in a Shop
* [ ] LC 1673 — Find the Most Competitive Subsequence

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
│   └── 03-Next-Greater-Element-II.md
│
├── 04-Next-Previous-Greater-Smaller/
│
├── 05-Boundary-Contribution/
│
└── 06-Stack-Design/
```
