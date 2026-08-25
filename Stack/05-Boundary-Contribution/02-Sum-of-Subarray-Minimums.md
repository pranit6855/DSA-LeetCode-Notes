# LeetCode 907 — Sum of Subarray Minimums

**Pattern:** Stack — Boundary / Contribution

---

# Problem

Hume ek integer array `arr` diya gaya hai.

Hume **har possible subarray ka minimum element** find karke un sabka sum return karna hai.

Example:

```text
arr = [3,1,2,4]
```

---

# Example

Array:

```text
[3,1,2,4]
```

Saare subarrays:

```text
[3]          → min = 3
[1]          → min = 1
[2]          → min = 2
[4]          → min = 4

[3,1]        → min = 1
[1,2]        → min = 1
[2,4]        → min = 2

[3,1,2]      → min = 1
[1,2,4]      → min = 1

[3,1,2,4]    → min = 1
```

Sum:

```text
3 + 1 + 2 + 4
+ 1 + 1 + 2
+ 1 + 1
+ 1

= 17
```

So:

```text
Output = 17
```

---

# Brute Force Approach

Brute force mein hum:

```text
Har possible subarray
        ↓
Uska minimum find karo
        ↓
Answer mein add karo
```

kar sakte hain.

Problem ye hai ki total bahut saare subarrays hote hain.

Number of subarrays:

```text
n * (n + 1) / 2
```

Large input ke liye brute force slow ho sakta hai.

---

# Main Observation

Brute force mein hum pooch rahe the:

> Har subarray ka minimum kya hai?

Ab question ko reverse kar do:

> **Har element kitne subarrays mein minimum ban sakta hai?**

Agar kisi element ko pata chal gaya ki woh kitne subarrays mein minimum hai, to uska contribution:

```text
element value × number of subarrays
```

hoga.

Phir sab elements ke contributions add kar denge.

---

# Contribution Idea

Example:

```text
arr = [3,1,2,4]
```

Element:

```text
1
```

index:

```text
1
```

`1` kin subarrays mein minimum hai?

```text
[1]
[3,1]
[1,2]
[3,1,2]
[1,2,4]
[3,1,2,4]
```

Total:

```text
6
```

So `1` ka contribution:

```text
1 × 6 = 6
```

---

# Boundary / Contribution Idea

Har element ke liye hume do boundaries chahiye:

```text
Previous Smaller
        ↓
     Current
        ↓
Next Smaller or Equal
```

In boundaries ki help se hum calculate karenge:

```text
Kitne left choices hain?
Kitne right choices hain?
```

Then:

```text
left choices × right choices
```

se pata chalega ki current element kitne subarrays mein minimum hai.

---

# Main Formula

For every index `i`:

```text
leftChoices = i - left[i]

rightChoices = right[i] - i
```

Then:

```text
number of subarrays
=
leftChoices × rightChoices
```

Contribution:

```text
arr[i] × leftChoices × rightChoices
```

Final answer:

```text
sum of all contributions
```

---

# Example — Element `1`

```text
arr = [3,1,2,4]
```

For:

```text
i = 1
```

Previous smaller:

```text
left[1] = -1
```

Next smaller or equal:

```text
right[1] = 4
```

So:

```text
leftChoices = 1 - (-1)
            = 2
```

Possible starting indexes:

```text
0
1
```

Right choices:

```text
rightChoices = 4 - 1
             = 3
```

Possible ending indexes:

```text
1
2
3
```

Total subarrays:

```text
2 × 3 = 6
```

Contribution:

```text
1 × 6 = 6
```

---

# Why Multiplication Works?

A valid subarray containing current element can independently choose:

```text
one valid starting position
+
one valid ending position
```

So:

```text
number of subarrays
=
leftChoices × rightChoices
```

Example:

```text
leftChoices = 2
rightChoices = 3
```

Therefore:

```text
2 × 3 = 6
```

---

# Connection With LC 84

## LC 84 — Largest Rectangle in Histogram

Wahan:

```text
Current Bar
    ↓
Left Smaller
    ↓
Right Smaller
    ↓
Maximum Width
    ↓
Height × Width
```

## LC 907 — Sum of Subarray Minimums

Yahan:

```text
Current Element
    ↓
Previous Smaller
    ↓
Next Smaller or Equal
    ↓
Left Choices
    ↓
Right Choices
    ↓
Number of Subarrays
    ↓
Element × Number of Subarrays
```

So:

> **LC 907 = Boundary Finding + Contribution Counting**

---

# Why Monotonic Stack?

Hume har element ke liye:

```text
Previous Smaller
Next Smaller or Equal
```

efficiently find karna hai.

Agar manually search karein:

```text
Har element
    ↓
Left mein search
    ↓
Right mein search
```

to `O(n²)` ho sakta hai.

Isliye Monotonic Stack use karenge.

---

# Solution Ke 3 Major Blocks

Pura solution 3 blocks mein hai:

```text
BLOCK 1
Previous Smaller
      ↓
left[]

BLOCK 2
Next Smaller or Equal
      ↓
right[]

BLOCK 3
Contribution
      ↓
arr[i] × leftChoices × rightChoices
```

---

# BLOCK 1 — Previous Smaller

Goal:

```text
left[i]
```

mein store karna:

> `i` ke left mein nearest smaller element ka index.

---

# Block 1 Direction

Hum:

```text
LEFT → RIGHT
```

jaayenge.

Kyun?

Kyuki hume:

```text
Previous Smaller
```

chahiye.

---

# Block 1 Stack

Stack mein:

```text
INDEXES
```

store honge.

Example:

```text
st = [1,2]
```

means:

```text
index 1
index 2
```

Actual value:

```cpp
arr[st.top()]
```

se milegi.

---

# Block 1 Code

```cpp
for (int i = 0; i < n; i++) {

    while (!st.empty() && arr[st.top()] > arr[i]) {
        st.pop();
    }

    if (st.empty()) {
        left[i] = -1;
    }
    else {
        left[i] = st.top();
    }

    st.push(i);
}
```

---

# Block 1 — Pop Logic

Condition:

```cpp
arr[st.top()] > arr[i]
```

Meaning:

```text
Top > Current
```

Agar top current se bada hai:

> Top current ka previous smaller nahi ho sakta.

So:

```text
POP
```

---

# Example

```text
arr = [5,4,3,2]
```

Current:

```text
2
```

Stack:

```text
[5,4,3]
```

Top:

```text
3
```

Check:

```text
3 > 2
```

True.

Pop:

```text
[5,4]
```

Again:

```text
4 > 2
```

Pop.

Again:

```text
5 > 2
```

Pop.

Stack empty.

Therefore:

```text
left[3] = -1
```

Meaning:

> `2` ke left mein koi smaller element nahi hai.

---

# Block 1 — Kab Stop Karega?

Example:

```text
arr = [3,1,2]
```

Current:

```text
2
```

Top:

```text
1
```

Check:

```text
1 > 2
```

False.

So pop nahi karenge.

Matlab:

```text
1 < 2
```

and `1` nearest previous smaller hai.

Therefore:

```text
left[2] = 1
```

---

# Block 1 Dry Run

Array:

```text
arr = [3,1,2,4]
```

Initially:

```text
st = []
left = [?, ?, ?, ?]
```

## i = 0

Current:

```text
3
```

Stack empty.

So:

```text
left[0] = -1
```

Push:

```text
st = [0]
```

---

## i = 1

Current:

```text
1
```

Top:

```text
3
```

Check:

```text
3 > 1
```

True.

Pop:

```text
st = []
```

Stack empty:

```text
left[1] = -1
```

Push:

```text
st = [1]
```

---

## i = 2

Current:

```text
2
```

Top:

```text
1
```

Check:

```text
1 > 2
```

False.

So:

```text
left[2] = 1
```

Push:

```text
st = [1,2]
```

---

## i = 3

Current:

```text
4
```

Top:

```text
2
```

Check:

```text
2 > 4
```

False.

So:

```text
left[3] = 2
```

Push:

```text
st = [1,2,3]
```

---

# Block 1 Final

```text
left = [-1,-1,1,2]
```

Meaning:

```text
index 0 → no previous smaller
index 1 → no previous smaller
index 2 → previous smaller at index 1
index 3 → previous smaller at index 2
```

---

# Clear Stack

Block 1 ke baad Stack mein purane indexes bache hue hain.

So Stack clear karenge:

```cpp
while (!st.empty()) {
    st.pop();
}
```

Now:

```text
st = []
```

---

# BLOCK 2 — Next Smaller or Equal

Goal:

```text
right[i]
```

mein:

> `i` ke right mein first smaller or equal element ka index.

---

# Block 2 Direction

Hum:

```text
RIGHT → LEFT
```

jaayenge.

Kyun?

Kyuki hume:

```text
Next Smaller / Equal
```

chahiye.

---

# Block 2 Code

```cpp
for (int i = n - 1; i >= 0; i--) {

    while (!st.empty() && arr[st.top()] >= arr[i]) {
        st.pop();
    }

    if (st.empty()) {
        right[i] = n;
    }
    else {
        right[i] = st.top();
    }

    st.push(i);
}
```

---

# Block 2 — Pop Logic

Condition:

```cpp
arr[st.top()] >= arr[i]
```

Meaning:

```text
Top >= Current
```

Agar top current se:

```text
bada OR equal
```

hai:

> Top current ka next smaller/equal nahi ho sakta.

So:

```text
POP
```

---

# Why `>=`?

Example:

```text
arr = [2,2]
```

Current second `2`.

Left side ka `2` equal hai:

```text
2 >= 2
```

True.

So equal value ko boundary treatment mein include karne ke liye yahan `>=` use karte hain.

907 mein duplicates important hain.

Isliye tie-breaking:

```text
Previous Smaller
        >
Next Smaller or Equal
        >=
```

rakha hai.

Ye ensure karta hai ki equal minimum values ke case mein same subarray unnecessarily multiple times count na ho.

---

# Block 2 — `right = n`

Agar Stack empty ho:

```cpp
if (st.empty()) {
    right[i] = n;
}
```

Matlab right side mein koi smaller/equal nahi mila.

`n` actual index nahi hai.

Ye:

```text
ARRAY KE END
```

ki imaginary boundary hai.

Example:

```text
n = 4
```

So:

```text
right[i] = 4
```

means:

> right mein smaller/equal nahi hai.

---

# Block 2 Dry Run

Array:

```text
arr = [3,1,2,4]
```

Initially:

```text
st = []
right = [?, ?, ?, ?]
```

## i = 3

Current:

```text
4
```

Stack empty.

So:

```text
right[3] = 4
```

Push:

```text
st = [3]
```

---

## i = 2

Current:

```text
2
```

Top:

```text
4
```

Check:

```text
4 >= 2
```

True.

Pop:

```text
st = []
```

Stack empty:

```text
right[2] = 4
```

Push:

```text
st = [2]
```

---

## i = 1

Current:

```text
1
```

Top:

```text
2
```

Check:

```text
2 >= 1
```

True.

Pop.

Stack empty:

```text
right[1] = 4
```

Push:

```text
st = [1]
```

---

## i = 0

Current:

```text
3
```

Top:

```text
1
```

Check:

```text
1 >= 3
```

False.

So:

```text
right[0] = 1
```

Push:

```text
st = [1,0]
```

---

# Block 2 Final

```text
right = [1,4,4,4]
```

Meaning:

```text
index 0 → next smaller/equal = 1
index 1 → no smaller/equal = 4
index 2 → no smaller/equal = 4
index 3 → no smaller/equal = 4
```

---

# BLOCK 3 — Contribution

Ab dono boundaries mil gayi:

```text
left
right
```

Ab har element ka contribution calculate karenge.

Code:

```cpp
for (int i = 0; i < n; i++) {

    long long leftChoices = i - left[i];

    long long rightChoices = right[i] - i;

    long long contribution =
        arr[i] * leftChoices * rightChoices;

    ans = (ans + contribution) % MOD;
}
```

---

# `leftChoices`

```cpp
long long leftChoices = i - left[i];
```

Meaning:

> Current element ko minimum rakhte hue kitni starting positions possible hain?

Example:

```text
i = 1
left[i] = -1
```

So:

```text
leftChoices = 1 - (-1)
            = 2
```

Starting positions:

```text
0
1
```

---

# `rightChoices`

```cpp
long long rightChoices = right[i] - i;
```

Meaning:

> Current element ko minimum rakhte hue kitne ending positions possible hain?

For `1`:

```text
right[1] = 4
i = 1
```

So:

```text
rightChoices = 4 - 1
             = 3
```

Ending positions:

```text
1
2
3
```

---

# Number of Subarrays

```text
leftChoices × rightChoices
```

For element `1`:

```text
2 × 3 = 6
```

So `1` exactly 6 subarrays mein minimum hai.

---

# Contribution

```cpp
long long contribution =
    arr[i] * leftChoices * rightChoices;
```

For `1`:

```text
1 × 2 × 3
= 6
```

---

# Complete Example

Array:

```text
arr = [3,1,2,4]
```

Boundaries:

```text
left  = [-1,-1,1,2]
right = [1,4,4,4]
```

Now contributions:

### Element 3

```text
i = 0

leftChoices = 0 - (-1)
            = 1

rightChoices = 1 - 0
             = 1

contribution = 3 × 1 × 1
             = 3
```

### Element 1

```text
i = 1

leftChoices = 1 - (-1)
            = 2

rightChoices = 4 - 1
             = 3

contribution = 1 × 2 × 3
             = 6
```

### Element 2

```text
i = 2

leftChoices = 2 - 1
            = 1

rightChoices = 4 - 2
             = 2

contribution = 2 × 1 × 2
             = 4
```

### Element 4

```text
i = 3

leftChoices = 3 - 2
            = 1

rightChoices = 4 - 3
             = 1

contribution = 4 × 1 × 1
             = 4
```

---

# Final Contribution Table

```text
Element    Contribution
-----------------------
3          3
1          6
2          4
4          4
-----------------------
Total      17
```

So:

```text
answer = 17
```

---

# Complete C++ Code

```cpp
class Solution {
public:
    int sumSubarrayMins(vector<int>& arr) {

        int n = arr.size();

        vector<int> left(n);
        vector<int> right(n);

        stack<int> st;

        // Block 1: Previous Smaller
        for (int i = 0; i < n; i++) {

            while (!st.empty() && arr[st.top()] > arr[i]) {
                st.pop();
            }

            if (st.empty()) {
                left[i] = -1;
            }
            else {
                left[i] = st.top();
            }

            st.push(i);
        }

        // Clear Stack
        while (!st.empty()) {
            st.pop();
        }

        // Block 2: Next Smaller or Equal
        for (int i = n - 1; i >= 0; i--) {

            while (!st.empty() && arr[st.top()] >= arr[i]) {
                st.pop();
            }

            if (st.empty()) {
                right[i] = n;
            }
            else {
                right[i] = st.top();
            }

            st.push(i);
        }

        // Block 3: Contribution
        long long ans = 0;

        int MOD = 1000000007;

        for (int i = 0; i < n; i++) {

            long long leftChoices = i - left[i];

            long long rightChoices = right[i] - i;

            long long contribution =
                arr[i] * leftChoices * rightChoices;

            ans = (ans + contribution) % MOD;
        }

        return ans;
    }
};
```

---

# Why `long long`?

Ye calculation:

```text
arr[i] × leftChoices × rightChoices
```

large ho sakta hai.

Isliye:

```cpp
long long
```

use kiya hai.

---

# Why MOD?

LeetCode answer ko:

```text
1,000,000,007
```

ke modulo mein return karna hota hai.

Isliye:

```cpp
ans = (ans + contribution) % MOD;
```

---

# Common Mistakes

## Mistake 1 — Stack mein value push karna

Wrong:

```cpp
st.push(arr[i]);
```

Correct:

```cpp
st.push(i);
```

---

## Mistake 2 — Block 1 mein `>=`

Hum use kar rahe hain:

```cpp
arr[st.top()] > arr[i]
```

---

## Mistake 3 — Block 2 mein `>`

Hum use kar rahe hain:

```cpp
arr[st.top()] >= arr[i]
```

---

## Mistake 4 — `leftChoices` wrong

Wrong:

```text
i - left[i] + 1
```

Correct:

```text
i - left[i]
```

---

## Mistake 5 — `rightChoices` wrong

Wrong:

```text
right[i] - i + 1
```

Correct:

```text
right[i] - i
```

---

# Complexity

### Time

```text
O(n)
```

Block 1:

```text
O(n)
```

Block 2:

```text
O(n)
```

Block 3:

```text
O(n)
```

Overall:

```text
O(n)
```

### Space

```text
O(n)
```

Because:

```text
left array
right array
stack
```

---

# Interview Explanation

> We do not enumerate every subarray. Instead, we calculate the contribution of each element as the minimum. For every index, we find its previous smaller and next smaller-or-equal element using monotonic stacks. These boundaries tell us how many choices we have for the left and right endpoints of a subarray where the current element remains the minimum. The number of such subarrays is `(i - left[i]) * (right[i] - i)`, so the contribution is `arr[i]` multiplied by that count. Finally, we add all contributions modulo `1e9 + 7`.

---

# One-Line Revision

> **Previous Smaller + Next Smaller/Equal → Left Choices × Right Choices → Element ka Contribution → Sab Contributions ka Sum.**

---

# Pattern #5 Progress

```text
[x] LC 84  — Largest Rectangle in Histogram
[x] LC 907 — Sum of Subarray Minimums
[ ] LC 85  — Maximal Rectangle
```
