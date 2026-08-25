# LeetCode 84 — Largest Rectangle in Histogram

**Pattern:** Stack — Boundary / Contribution

---

# Problem

Hume ek integer array `heights` diya gaya hai.

Har element histogram ke ek bar ki height represent karta hai.

Har bar ki width `1` hai.

Hume histogram ke andar banne wale **largest rectangle ka area** find karna hai.

---

# Example

```text
heights = [2,1,5,6,2,3]
```

Output:

```text
10
```

---

# Rectangle Ka Meaning

Hum continuous bars ka ek group choose kar sakte hain.

Example:

```text
[5,6]
```

Minimum height:

```text
min(5,6) = 5
```

Width:

```text
2
```

Area:

```text
5 × 2 = 10
```

So answer at least `10` hai.

---

# Important Rule

Agar range hai:

```text
[5,6,2]
```

to rectangle ki height:

```text
min(5,6,2) = 2
```

hogi.

Width:

```text
3
```

Area:

```text
2 × 3 = 6
```

Hum `5` height ka rectangle nahi bana sakte because beech mein `2` ki bar hai.

---

# Brute Force Approach

Har possible continuous range check karenge.

Har `i` ko left boundary maanenge.

Phir `j` ko right side mein move karenge.

Saath mein range ka minimum height maintain karenge.

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {

        int n = heights.size();
        int ans = 0;

        for (int i = 0; i < n; i++) {

            int minHeight = INT_MAX;

            for (int j = i; j < n; j++) {

                minHeight = min(minHeight, heights[j]);

                int width = j - i + 1;

                int area = minHeight * width;

                ans = max(ans, area);
            }
        }

        return ans;
    }
};
```

---

# Brute Force Complexity

```text
Time  : O(n²)
Space : O(1)
```

Large input par TLE aa sakta hai.

---

# Main Observation

Brute force mein hum ye kar rahe the:

```text
Range
  ↓
Minimum height
  ↓
Minimum height × width
```

Ab reverse thinking karo:

> **Har bar ko rectangle ki minimum height maan lo.**

Phir hume pata karna hai:

```text
Ye bar left mein kitni door tak extend kar sakti hai?
Ye bar right mein kitni door tak extend kar sakti hai?
```

Yahi **Boundary / Contribution** pattern hai.

---

# Boundary Idea

Example:

```text
heights = [2,1,5,6,2,3]
```

Bar:

```text
5
```

index:

```text
2
```

Agar `5` ko rectangle ki minimum height maan rahe hain:

## Left Boundary

`5` ke left mein:

```text
2,1
```

Pehli smaller bar:

```text
1
```

index:

```text
1
```

So:

```text
left = 1
```

## Right Boundary

`5` ke right mein:

```text
6,2,3
```

Pehli smaller bar:

```text
2
```

index:

```text
4
```

So:

```text
right = 4
```

---

# Width

Boundary bars rectangle mein include nahi hoti.

So:

```text
width = right - left - 1
```

For `5`:

```text
width = 4 - 1 - 1
      = 2
```

Actual bars:

```text
[5,6]
```

Area:

```text
5 × 2 = 10
```

---

# Problem With Manual Boundary Search

Har bar ke liye manually:

```text
left smaller
right smaller
```

search karenge to `O(n²)` ho sakta hai.

Isliye:

```text
Monotonic Stack
```

use karenge.

---

# Stack Type

Hum:

```text
Monotonic Increasing Stack
```

use karenge.

Stack mein:

```text
indexes
```

store honge.

Corresponding heights increasing order mein rahengi.

Example:

```text
heights:

1
5
6
```

Stack:

```text
[1,5,6]
```

because:

```text
1 < 5 < 6
```

---

# Why Increasing Stack?

Jab current bar chhoti hoti hai:

```text
current < stack top
```

to Stack ke top wali taller bar ka **Right Smaller Boundary** mil gaya.

Current bar hi uski first smaller bar hai.

Isliye top ko pop karenge.

---

# Example

Stack:

```text
[1,5,6]
```

Current:

```text
2
```

Top:

```text
6
```

Check:

```text
6 > 2
```

So:

```text
6 ka right smaller = current index
```

6 ko pop karenge.

Ab:

```text
[1,5]
```

Top `5` hai:

```text
5 > 2
```

So:

```text
5 ka right smaller = current index
```

5 ko bhi pop karenge.

---

# Left Boundary Kaise Milegi?

Jab bar pop hoti hai:

```cpp
int index = st.top();
st.pop();
```

Pop ke baad:

```cpp
st.top()
```

us bar ka **nearest smaller left boundary** hota hai.

Agar Stack empty ho gaya:

```text
left = -1
```

Example:

Before:

```text
[1,5,6]
```

6 pop hua.

After:

```text
[1,5]
```

So 6 ke liye:

```text
left = index of 5
```

---

# Right Boundary Kaise Milegi?

```cpp
int right = i;
```

Current bar popped bar se smaller hai.

Therefore current index hi first smaller element on right hai.

---

# Width Formula

```cpp
int width = right - left - 1;
```

Why `-1`?

Because `left` aur `right` boundary bars khud rectangle mein include nahi hoti.

Actual rectangle:

```text
left + 1
...
right - 1
```

tak hota hai.

---

# Area Formula

```cpp
int area = height * width;
```

---

# Core Algorithm

```text
Start
  ↓
Empty Increasing Monotonic Stack
  ↓
Array ko Left → Right traverse karo
  ↓
Current height nikalo
  ↓
Jab tak:
    Stack non-empty
    AND
    Stack top height > current height

    ↓
    Top bar ka Right Boundary mil gaya
    ↓
    Top ko pop karo
    ↓
    Pop ke baad Stack top = Left Boundary
    ↓
    Width calculate karo
    ↓
    Area calculate karo
    ↓
    Maximum update karo

  ↓
Current index push karo
  ↓
End
```

---

# Complete C++ Code

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {

        int n = heights.size();

        stack<int> st;

        int ans = 0;

        for (int i = 0; i <= n; i++) {

            int currentHeight;

            if (i == n) {
                currentHeight = 0;
            }
            else {
                currentHeight = heights[i];
            }

            while (!st.empty() &&
                   heights[st.top()] > currentHeight) {

                int index = st.top();
                st.pop();

                int height = heights[index];

                int right = i;

                int left;

                if (st.empty()) {
                    left = -1;
                }
                else {
                    left = st.top();
                }

                int width = right - left - 1;

                int area = height * width;

                ans = max(ans, area);
            }

            st.push(i);
        }

        return ans;
    }
};
```

---

# Line-by-Line Explanation

## 1. Array Size

```cpp
int n = heights.size();
```

Example:

```text
heights = [2,1,5,6,2,3]
n = 6
```

---

## 2. Stack

```cpp
stack<int> st;
```

Stack mein **indexes** store honge.

Example:

```text
[1,2,3]
```

means:

```text
index 1
index 2
index 3
```

Actual heights:

```cpp
heights[1]
heights[2]
heights[3]
```

se milengi.

---

## 3. Answer

```cpp
int ans = 0;
```

Largest area yahan store karenge.

---

# 4. Loop

```cpp
for (int i = 0; i <= n; i++)
```

Normally last valid index:

```text
n - 1
```

hai.

Lekin extra iteration:

```text
i = n
```

bhi chahiye.

---

# 5. Extra `i == n`

```cpp
if (i == n) {
    currentHeight = 0;
}
```

End par imaginary `0` use karte hain.

Ye Stack mein bachi hui bars ko pop karwa deta hai.

---

# 6. Normal Current Height

```cpp
else {
    currentHeight = heights[i];
}
```

Normal case mein actual bar ki height milegi.

---

# 7. Main While

```cpp
while (!st.empty() &&
       heights[st.top()] > currentHeight)
```

Meaning:

> Stack top wali bar current bar se taller hai.

So current bar uski first smaller bar hai.

Therefore top bar ka rectangle calculate kar sakte hain.

---

# 8. Save Popped Index

```cpp
int index = st.top();
```

Popped bar ka index save karo.

---

# 9. Pop

```cpp
st.pop();
```

Ab us bar ko process karenge.

---

# 10. Height

```cpp
int height = heights[index];
```

Popped bar ki actual height.

---

# 11. Right Boundary

```cpp
int right = i;
```

Current index right smaller boundary hai.

---

# 12. Left Boundary

```cpp
int left;

if (st.empty()) {
    left = -1;
}
else {
    left = st.top();
}
```

Pop ke baad Stack top nearest smaller left boundary deta hai.

Agar Stack empty:

```text
left = -1
```

---

# 13. Width

```cpp
int width = right - left - 1;
```

Boundary bars khud rectangle mein include nahi hoti.

---

# 14. Area

```cpp
int area = height * width;
```

Rectangle area:

```text
height × width
```

---

# 15. Maximum

```cpp
ans = max(ans, area);
```

Largest area save karo.

---

# 16. Push Current

```cpp
st.push(i);
```

Current bar ko future processing ke liye Stack mein save karo.

---

# Detailed Dry Run

Input:

```text
heights = [2,1,5,6,2,3]
```

Indexes:

```text
index:   0  1  2  3  4  5
height:  2  1  5  6  2  3
```

Initial:

```text
st = []
ans = 0
```

---

## i = 0

Current:

```text
2
```

Stack empty.

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

Top height:

```text
2
```

Check:

```text
2 > 1
```

True.

Pop index `0`.

```text
height = 2
right = 1
```

Stack empty:

```text
left = -1
```

Width:

```text
1 - (-1) - 1
= 1
```

Area:

```text
2 × 1
= 2
```

So:

```text
ans = 2
```

Push current:

```text
st = [1]
```

---

# i = 2

Current:

```text
5
```

Top:

```text
1
```

Check:

```text
1 > 5
```

False.

Push:

```text
st = [1,2]
```

---

# i = 3

Current:

```text
6
```

Top:

```text
5
```

Check:

```text
5 > 6
```

False.

Push:

```text
st = [1,2,3]
```

---

# i = 4

Current:

```text
2
```

Stack:

```text
[1,2,3]
```

Top:

```text
index 3 → height 6
```

Check:

```text
6 > 2
```

True.

### Pop 6

```text
height = 6
right = 4
```

After pop:

```text
st = [1,2]
```

So:

```text
left = 2
```

Width:

```text
4 - 2 - 1
= 1
```

Area:

```text
6 × 1
= 6
```

Answer:

```text
ans = 6
```

---

## Same i = 4 — Pop 5

Top:

```text
index 2 → height 5
```

Check:

```text
5 > 2
```

True.

Pop.

```text
height = 5
right = 4
```

After pop:

```text
st = [1]
```

So:

```text
left = 1
```

Width:

```text
4 - 1 - 1
= 2
```

Area:

```text
5 × 2
= 10
```

Update:

```text
ans = 10
```

This rectangle is:

```text
[5,6]
```

---

### While Stop

Top:

```text
height = 1
```

Check:

```text
1 > 2
```

False.

Push index `4`:

```text
st = [1,4]
```

---

# i = 5

Current:

```text
3
```

Top:

```text
height = 2
```

Check:

```text
2 > 3
```

False.

Push:

```text
st = [1,4,5]
```

---

# i = 6

`i == n`.

So:

```text
currentHeight = 0
```

Now remaining bars pop hongi.

### Pop 3

```text
height = 3
right = 6
left = 4
```

Width:

```text
6 - 4 - 1
= 1
```

Area:

```text
3
```

---

### Pop 2

```text
height = 2
right = 6
left = 1
```

Width:

```text
6 - 1 - 1
= 4
```

Area:

```text
2 × 4
= 8
```

---

### Pop 1

Stack empty:

```text
left = -1
right = 6
```

Width:

```text
6 - (-1) - 1
= 6
```

Area:

```text
1 × 6
= 6
```

Answer remains:

```text
10
```

---

# Final Answer

```text
10
```

---

# Most Important Formula

Whenever a bar is popped:

```text
height = popped bar height

right = current index

left =
    -1 if stack empty
    stack.top() otherwise

width = right - left - 1

area = height × width
```

---

# Why This Is Boundary / Contribution

Har bar ke liye:

```text
Left Smaller
     ↓
Current Bar
     ↓
Right Smaller
```

Then:

```text
Maximum Width
     ↓
Height × Width
     ↓
Area Contribution
```

---

# Complexity

```text
Time  : O(n)
Space : O(n)
```

Every index maximum once push aur once pop hota hai.

---

# Common Mistakes

## Mistake 1 — Stack mein height store karna

Wrong:

```cpp
st.push(heights[i]);
```

Correct:

```cpp
st.push(i);
```

---

## Mistake 2 — Width formula galat

Correct:

```cpp
width = right - left - 1;
```

---

## Mistake 3 — Extra `0` iteration bhoolna

Correct:

```cpp
for (int i = 0; i <= n; i++)
```

and:

```cpp
if (i == n) {
    currentHeight = 0;
}
```

---

## Mistake 4 — Right boundary galat lena

Correct:

```cpp
right = i;
```

Current smaller bar hi first smaller on right hai.

---

# Interview Explanation

> I use an increasing monotonic stack of indices. Whenever the current height is smaller than the height at the stack top, the current index becomes the right smaller boundary for the popped bar. After popping, the new stack top becomes its left smaller boundary. Using `width = right - left - 1`, I calculate the area for that bar and update the maximum. I use an extra iteration with height `0` to process all remaining bars. The solution runs in O(n) time and O(n) space.

---

# One-Line Revision

> **Current smaller bar aate hi top bar ka right boundary current index hota hai, pop ke baad Stack top left boundary hota hai, aur `right - left - 1` se width nikal kar `height × width` area calculate karte hain.**

---

# Pattern #5 Progress

- [x] LC 84 — Largest Rectangle in Histogram
- [ ] LC 907 — Sum of Subarray Minimums
- [ ] LC 85 — Maximal Rectangle
