# LeetCode 152 - Maximum Product Subarray

## 📌 Problem

Given an integer array `nums`, find a **contiguous subarray** that has the largest product and return the product.

The subarray must contain **at least one element**.

---

## 🔹 Example 1

```text
Input:
nums = [2,3,-2,4]

Output:
6
```

Possible subarrays:

```text
[2]           → 2
[3]           → 3
[-2]          → -2
[4]           → 4
[2,3]         → 6
[3,-2]        → -6
[2,3,-2]      → -12
[-2,4]        → -8
```

Maximum product:

```text
2 × 3 = 6
```

Therefore:

```text
Answer = 6
```

---

# 🔹 Example 2

```text
Input:
nums = [-2,0,-1]

Output:
0
```

Because:

```text
[-2] → -2
[0]  → 0
[-1] → -1
[-2,0] → 0
```

Maximum product is:

```text
0
```

---

# 🔹 Example 3

```text
Input:
nums = [-2,3,-4]

Output:
24
```

Because:

```text
(-2) × 3 × (-4) = 24
```

This example is important because it explains why we need to maintain both:

```text
Maximum Product
Minimum Product
```

---

# 🧠 Why Normal Kadane's Algorithm Is Not Enough?

In LC 53 - Maximum Subarray, we were finding the maximum **sum**.

There we maintained:

```text
bestending
```

For every element:

```text
continue the subarray
        OR
start a new subarray
```

But Maximum Product Subarray has an extra problem:

## ❗ Negative Numbers

With multiplication:

```text
negative × negative = positive
```

Example:

```text
-2 × -3 = 6
```

So a value that is currently the **minimum/most negative product** can become the **maximum product** when multiplied by another negative number.

Therefore, we cannot only store the maximum.

We must store:

```text
bestending
worstending
```

---

# 🔥 Two Important Variables

## 1. `bestending`

`bestending` means:

> Maximum product of a subarray that **ends at the current index**.

Example:

```text
nums = [2,3]
```

At `3`:

```text
[3]       → 3
[2,3]     → 6
```

So:

```text
bestending = 6
```

---

## 2. `worstending`

`worstending` means:

> Minimum product of a subarray that **ends at the current index**.

Why do we need this?

Because:

```text
negative × negative = positive
```

Example:

```text
[-2,3,-4]
```

At `3`:

```text
[3]       → 3
[-2,3]    → -6
```

So:

```text
bestending = 3
worstending = -6
```

Now `-4` arrives.

```text
-6 × -4 = 24
```

So the previously worst product becomes the best product.

---

# 🔥 Third Variable - `res`

```cpp
int res = nums[0];
```

`res` stores:

> The maximum product found anywhere in the array so far.

Difference:

```text
bestending
```

means:

```text
Best product that MUST end at current index
```

while:

```text
res
```

means:

```text
Best product found anywhere so far
```

---

# 🧠 Main Decision

For every `nums[i]`, we have three possibilities.

Suppose:

```text
current = nums[i]
```

### Choice 1 - Start a new subarray

```cpp
c1 = nums[i];
```

Example:

```text
previous product = -10
current = 5
```

Instead of:

```text
-10 × 5 = -50
```

we can simply start:

```text
5
```

---

### Choice 2 - Continue the maximum product

```cpp
c2 = nums[i] * bestending;
```

Example:

```text
bestending = 6
nums[i] = 4
```

Then:

```text
6 × 4 = 24
```

---

### Choice 3 - Continue the minimum product

```cpp
c3 = nums[i] * worstending;
```

This is important when the current number is negative.

Example:

```text
worstending = -6
nums[i] = -4
```

Then:

```text
-6 × -4 = 24
```

---

# 🔥 Therefore

We calculate:

```cpp
int c1 = nums[i];
int c2 = nums[i] * bestending;
int c3 = nums[i] * worstending;
```

Then:

```cpp
bestending = max(c1, max(c2, c3));
```

and:

```cpp
worstending = min(c1, min(c2, c3));
```

---

# ⚠️ Important: Old Values

Both `bestending` and `worstending` must be calculated using the **old values**.

That's why we first store:

```cpp
c1
c2
c3
```

and only then update:

```cpp
bestending
worstending
```

Otherwise, if we update `bestending` first and then use it to calculate `worstending`, we could accidentally use the **new value** instead of the previous value.

---

# 🔄 Complete Dry Run

Consider:

```text
nums = [2,3,-2,4]
```

---

## Initial State

```text
bestending = 2
worstending = 2
res = 2
```

We start from index `1`.

---

## i = 1

Current:

```text
nums[i] = 3
```

Three choices:

```text
c1 = 3
c2 = 3 × 2 = 6
c3 = 3 × 2 = 6
```

Maximum:

```text
bestending = 6
```

Minimum:

```text
worstending = 3
```

Update result:

```text
res = max(2,6)
    = 6
```

Current state:

```text
bestending = 6
worstending = 3
res = 6
```

---

## i = 2

Current:

```text
nums[i] = -2
```

Three choices:

```text
c1 = -2

c2 = -2 × 6
   = -12

c3 = -2 × 3
   = -6
```

Maximum:

```text
bestending = -2
```

Minimum:

```text
worstending = -12
```

Result:

```text
res = max(6,-2)
    = 6
```

Current state:

```text
bestending = -2
worstending = -12
res = 6
```

---

## i = 3

Current:

```text
nums[i] = 4
```

Three choices:

```text
c1 = 4

c2 = 4 × (-2)
   = -8

c3 = 4 × (-12)
   = -48
```

Maximum:

```text
bestending = 4
```

Minimum:

```text
worstending = -48
```

Result:

```text
res = max(6,4)
    = 6
```

Final answer:

```text
6
```

---

# 🔥 Important Dry Run With Two Negatives

This example is more important for understanding the algorithm:

```text
nums = [-2,3,-4]
```

---

## Start

```text
bestending = -2
worstending = -2
res = -2
```

---

## `3`

```text
c1 = 3
c2 = 3 × (-2) = -6
c3 = 3 × (-2) = -6
```

Therefore:

```text
bestending = 3
worstending = -6
res = 3
```

---

## `-4`

Now:

```text
c1 = -4

c2 = -4 × 3
   = -12

c3 = -4 × (-6)
   = 24
```

Notice:

```text
worstending = -6
```

looked bad.

But:

```text
-6 × -4 = 24
```

So:

```text
bestending = 24
```

Final:

```text
res = 24
```

This is the **main reason we maintain `worstending`**.

---

# 🔥 Zero Handling

Zero is also automatically handled by the three choices.

Example:

```text
nums = [2,3,0,4]
```

When:

```text
nums[i] = 0
```

We get:

```text
c1 = 0
c2 = 0
c3 = 0
```

Therefore:

```text
bestending = 0
worstending = 0
```

The next element can start a new subarray.

For `4`:

```text
c1 = 4
c2 = 4 × 0 = 0
c3 = 4 × 0 = 0
```

So:

```text
bestending = 4
```

This naturally allows the algorithm to start a new subarray after zero.

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {

        int res = nums[0];

        int bestending = nums[0];
        int worstending = nums[0];

        for(int i = 1; i < nums.size(); i++){

            int c1 = nums[i];

            int c2 = nums[i] * bestending;

            int c3 = nums[i] * worstending;

            bestending = max(c1, max(c2, c3));

            worstending = min(c1, min(c2, c3));

            res = max(res, bestending);
        }

        return res;
    }
};
```

---

# 🧩 Code Explanation

### Initialization

```cpp
int res = nums[0];
int bestending = nums[0];
int worstending = nums[0];
```

We start with the first element because the subarray must contain **at least one element**.

---

### Loop

```cpp
for(int i = 1; i < nums.size(); i++)
```

Start from the second element because the first element is already used for initialization.

---

### Three Choices

```cpp
int c1 = nums[i];
```

Start a new subarray.

```cpp
int c2 = nums[i] * bestending;
```

Continue the previous maximum product.

```cpp
int c3 = nums[i] * worstending;
```

Continue the previous minimum product.

---

### Update Maximum

```cpp
bestending = max(c1, max(c2, c3));
```

Choose the largest product that ends at the current index.

---

### Update Minimum

```cpp
worstending = min(c1, min(c2, c3));
```

Choose the smallest product that ends at the current index.

This minimum can become useful later if another negative number appears.

---

### Update Answer

```cpp
res = max(res, bestending);
```

`bestending` is the best product ending **here**.

`res` is the best product found **anywhere**.

---

# ⭐ Why `res` Does NOT Need `worstending`

We only want the **maximum** product as the final answer.

Therefore:

```cpp
res = max(res, bestending);
```

is enough.

`worstending` is not directly a candidate for the final answer.

Its purpose is to help us calculate a future `bestending`.

Example:

```text
-6 → -4
```

Current:

```text
worstending = -6
```

Then:

```text
-6 × -4 = 24
```

So `worstending` helps produce:

```text
bestending = 24
```

---

# 🔥 LC 53 vs LC 152

## LC 53 - Maximum Subarray

For sum:

```text
current element
      ↓
continue OR restart
      ↓
bestending
```

Only one state is needed:

```text
bestending
```

---

## LC 152 - Maximum Product Subarray

For product:

```text
current element
      ↓
┌─────┼─────┐
↓     ↓     ↓
num  max   min
      ×     ×
└─────┼─────┘
      ↓
bestending
worstending
```

Two states are required because of negative numbers.

---

# 🧠 Important Pattern

This is still a **Kadane-style pattern**.

General idea:

```text
Current Position
      ↓
Possible Previous States
      ↓
Choose Best State
      ↓
Update Global Answer
```

For Maximum Product:

```text
Current element
      ↓
nums[i]
nums[i] × bestending
nums[i] × worstending
      ↓
bestending
worstending
      ↓
res
```

---

# ⚠️ Common Mistakes

## 1. Only keeping maximum

Wrong idea:

```cpp
int bestending;
```

and ignoring minimum.

Why wrong?

```text
negative × negative = positive
```

---

## 2. Updating `bestending` before calculating `worstending`

Don't do:

```cpp
bestending = ...
worstending = nums[i] * bestending;
```

because now `worstending` is using the **new** `bestending`.

Instead calculate:

```cpp
c1
c2
c3
```

first.

Then update both.

---

## 3. Forgetting the current element itself

You must consider:

```cpp
c1 = nums[i];
```

because sometimes starting fresh is better than continuing.

Example:

```text
[-10, 5]
```

Continuing:

```text
-10 × 5 = -50
```

Starting fresh:

```text
5
```

So `5` is better.

---

## 4. Using `0` as initial value

Don't initialize:

```cpp
bestending = 0;
```

because arrays can contain only negative numbers.

Example:

```text
[-5,-2,-8]
```

Correct answer:

```text
-2
```

Initialize using:

```cpp
nums[0]
```

instead.

---

# 🔄 All Negative Example

```text
nums = [-5,-2,-8]
```

Initial:

```text
bestending = -5
worstending = -5
res = -5
```

At `-2`:

```text
c1 = -2
c2 = (-2) × (-5) = 10
c3 = (-2) × (-5) = 10
```

So:

```text
bestending = 10
worstending = -2
res = 10
```

Notice:

```text
[-5,-2] = 10
```

So answer is actually:

```text
10
```

At `-8`:

```text
c1 = -8
c2 = 10 × -8 = -80
c3 = -2 × -8 = 16
```

Therefore:

```text
bestending = 16
```

Final:

```text
[-5,-2,-8]
```

Product:

```text
(-5) × (-2) × (-8) = -80
```

But:

```text
(-2) × (-8) = 16
```

is better.

So:

```text
Answer = 16
```

---

# ⏱️ Complexity

### Time

```text
O(n)
```

Array ko sirf ek baar traverse karte hain.

### Space

```text
O(1)
```

Sirf constant number of variables use hote hain.

---

# ⭐ Final Revision

Remember these three choices:

```cpp
int c1 = nums[i];

int c2 = nums[i] * bestending;

int c3 = nums[i] * worstending;
```

Then:

```cpp
bestending = max(c1, max(c2, c3));

worstending = min(c1, min(c2, c3));

res = max(res, bestending);
```

### One-line concept:

> **Maximum product ke liye current value, previous maximum product aur previous minimum product — teeno possibilities check karo, kyunki negative × negative maximum bana sakta hai.**

---

# 📁 Kadane's Algorithm Folder

```text
Kadane's-Algorithm/
│
├── 01-Maximum-Subarray.md
└── 02-Maximum-Product-Subarray.md
```

### Commit Message

```text
Add LC 152 Maximum Product Subarray detailed notes
```
```
