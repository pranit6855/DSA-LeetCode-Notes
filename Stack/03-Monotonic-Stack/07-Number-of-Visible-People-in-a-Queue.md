# LeetCode 1944 — Number of Visible People in a Queue

**Pattern:** Stack — Next / Previous Greater-Smaller

---

# Problem

Hume ek array `heights` diya gaya hai.

Har person ke liye find karna hai:

> Us person ke right side mein **kitne log visible** hain.

---

# Visibility Rule

Person `i` person `j` ko dekh sakta hai agar:

* `j` current person ke right mein hai.
* Beech mein koi aisa person nahi hai jo current person aur `j` dono se taller ho.

Simple language mein:

> Current person apne right mein shorter logon ko dekh sakta hai, aur jo first taller/equal person milta hai, usko bhi dekh sakta hai. Uske baad taller person sabko block kar deta hai.

---

# Example

```text
heights = [10,6,8,5,11,9]
```

Output:

```text
[3,1,2,1,1,0]
```

---

# Example Explanation

## Person 10

Right side:

```text
6,8,5,11,9
```

Visible:

```text
6
8
11
```

`11` ke baad `9` ko `10` nahi dekh sakta because `11` block kar raha hai.

So:

```text
10 → 3 people
```

---

## Person 6

Right side:

```text
8,5,11,9
```

`8` first taller person hai.

`8` ke baad baaki log 6 ko visible nahi honge.

So:

```text
6 → 1
```

---

## Person 8

Right:

```text
5,11,9
```

Visible:

```text
5
11
```

`11` block karega `9` ko.

So:

```text
8 → 2
```

---

## Person 5

Right:

```text
11,9
```

`11` first taller person hai.

So:

```text
5 → 1
```

---

## Person 11

Right:

```text
9
```

`9` shorter hai aur visible hai.

So:

```text
11 → 1
```

---

## Person 9

Right mein koi person nahi.

So:

```text
9 → 0
```

Final:

```text
[3,1,2,1,1,0]
```

---

# Pattern Identification

Agar question mein:

```text
visible people
right side
next taller
shorter people
first taller person
```

jaise concepts aayein, to:

```text
MONOTONIC STACK
```

ka thought aana chahiye.

---

# Why Right to Left?

Question mein current person ko:

```text
RIGHT SIDE
```

ke log dekhne hain.

Isliye hum:

```text
RIGHT → LEFT
```

process karenge.

Example:

```text
10  6  8  5  11  9
                    ↑
                  start
```

Jab hum current person par aate hain, uske right side ke log already process ho chuke hote hain.

Isliye unki useful information Stack mein available hoti hai.

---

# Main Idea

Hum **Monotonic Decreasing Stack** use karenge.

Stack mein:

```text
INDEXES
```

store karenge.

Example:

```text
stack = [4,2,1]
```

means:

```text
index 4 → height 11
index 2 → height 8
index 1 → height 6
```

---

# Why Index Store Karna?

Stack mein:

```cpp
stack<int> st;
```

rakhenge.

Agar:

```text
st.top() = 2
```

to:

```cpp
heights[st.top()]
```

se actual height milegi:

```text
heights[2] = 8
```

Isliye comparison possible hai:

```cpp
heights[st.top()] < heights[i]
```

---

# Core Visibility Logic

Current height:

```text
h = heights[i]
```

Stack top person:

```text
heights[st.top()]
```

### Case 1 — Top shorter hai

Agar:

```text
heights[st.top()] < heights[i]
```

to current person us shorter person ko dekh sakta hai.

So:

```cpp
ans[i]++;
st.pop();
```

Meaning:

```text
shorter person visible
→ count
→ remove from Stack
```

---

### Case 2 — Stack mein taller/equal person bacha

Jab `while` rukta hai aur Stack empty nahi hoti:

```cpp
if (!st.empty()) {
    ans[i]++;
}
```

Matlab:

> Saare shorter visible people already pop ho gaye. Jo first person Stack mein bacha hai, wo current se taller/equal hai.

Wo bhi visible hai.

So `+1`.

Lekin uske baad stop.

---

# Important Point About `if (!st.empty())`

Ye line khud comparison nahi kar rahi:

```cpp
if (!st.empty())
```

Ye sirf check karti hai:

> Stack mein koi person bacha hai ya nahi?

Lekin `while` pehle ye guarantee kar chuka hai ki:

```text
heights[st.top()] < heights[i]
```

false hai.

Therefore:

```text
heights[st.top()] >= heights[i]
```

So bacha hua person taller/equal hai.

Isliye:

```cpp
ans[i]++;
```

correct hai.

---

# Example — Current Height 8

Right side:

```text
5,11,9
```

Useful Stack:

```text
[11,5]
```

Current:

```text
8
```

Top:

```text
5
```

Check:

```text
5 < 8
```

True.

So:

```text
5 visible
count = 1
pop
```

Now Stack:

```text
[11]
```

Again:

```text
11 < 8
```

False.

`while` stops.

Stack non-empty:

```text
[11]
```

So:

```text
count++
```

Now:

```text
count = 2
```

Thus:

```text
8 can see 5 and 11
```

`9` is blocked by `11`.

---

# Algorithm

```text
Start
  ↓
Answer array banao
  ↓
Empty Stack banao
  ↓
Right → Left traverse karo
  ↓
Current height = heights[i]
  ↓
Jab tak:
    Stack non-empty
    AND
    Stack top height < current height

    ↓
    shorter person visible
    count++
    pop

  ↓
Agar Stack non-empty:
    first taller/equal person visible
    count++

  ↓
Current index push karo
  ↓
Next person
  ↓
Return answer
```

---

# Step-by-Step Approach

## Step 1 — Answer Array

```cpp
vector<int> ans(n, 0);
```

Initially:

```text
[0,0,0,0,0,0]
```

Every answer starts at `0`.

---

## Step 2 — Stack

```cpp
stack<int> st;
```

Stack mein indexes store honge.

---

## Step 3 — Traverse Right to Left

```cpp
for (int i = n - 1; i >= 0; i--)
```

Example:

```text
i = 5
i = 4
i = 3
i = 2
i = 1
i = 0
```

---

## Step 4 — Pop Shorter People

```cpp
while (!st.empty() && heights[st.top()] < heights[i])
```

Current person taller hai than Stack top.

So:

```cpp
ans[i]++;
st.pop();
```

---

## Step 5 — Taller/Equal Person

While loop finish hone ke baad:

```cpp
if (!st.empty())
```

Agar Stack empty nahi hai:

```cpp
ans[i]++;
```

Because first taller/equal person visible hai.

---

## Step 6 — Current Person Push

```cpp
st.push(i);
```

Current person ko left wale future persons ke right side mein available rakho.

---

# Complete C++ Code

```cpp
class Solution {
public:
    vector<int> canSeePersonsCount(vector<int>& heights) {

        int n = heights.size();

        vector<int> ans(n, 0);

        stack<int> st;

        for (int i = n - 1; i >= 0; i--) {

            while (!st.empty() &&
                   heights[st.top()] < heights[i]) {

                ans[i]++;
                st.pop();
            }

            if (!st.empty()) {
                ans[i]++;
            }

            st.push(i);
        }

        return ans;
    }
};
```

---

# Detailed Dry Run

Input:

```text
heights = [10,6,8,5,11,9]
```

Indexes:

```text
index:    0   1   2   3   4   5
height:  10   6   8   5  11   9
```

Initial:

```text
ans = [0,0,0,0,0,0]
stack = []
```

---

# i = 5 — Height 9

Right side mein koi person nahi.

Stack:

```text
[]
```

`while` nahi chalega.

`if (!st.empty())` false.

Push:

```cpp
st.push(5);
```

Stack:

```text
[5]
```

Meaning:

```text
index 5 → height 9
```

Answer:

```text
[0,0,0,0,0,0]
```

---

# i = 4 — Height 11

Current:

```text
11
```

Stack:

```text
[5]
```

Top index:

```text
5
```

Top height:

```text
heights[5] = 9
```

Check:

```text
9 < 11
```

True.

So:

```cpp
ans[4]++;
```

Now:

```text
ans[4] = 1
```

Pop:

```cpp
st.pop();
```

Stack:

```text
[]
```

Push current:

```cpp
st.push(4);
```

Stack:

```text
[4]
```

---

# i = 3 — Height 5

Current:

```text
5
```

Stack:

```text
[4]
```

Top height:

```text
11
```

Check:

```text
11 < 5
```

False.

While stop.

Stack non-empty:

```text
[4]
```

So:

```cpp
ans[3]++;
```

Now:

```text
ans[3] = 1
```

`11` is visible.

Push:

```cpp
st.push(3);
```

Stack:

```text
[4,3]
```

---

# i = 2 — Height 8

Current:

```text
8
```

Stack:

```text
[4,3]
```

Top index:

```text
3
```

Top height:

```text
5
```

Check:

```text
5 < 8
```

True.

So:

```text
ans[2]++
```

Now:

```text
ans[2] = 1
```

Pop:

```text
stack = [4]
```

---

## While Again

Top:

```text
index = 4
height = 11
```

Check:

```text
11 < 8
```

False.

Stop.

Stack non-empty:

```text
[4]
```

Therefore:

```text
ans[2]++
```

Now:

```text
ans[2] = 2
```

Push current:

```text
stack = [4,2]
```

So:

```text
8 can see:
5
11
```

---

# i = 1 — Height 6

Current:

```text
6
```

Stack:

```text
[4,2]
```

Top index:

```text
2
```

Top height:

```text
8
```

Check:

```text
8 < 6
```

False.

So while doesn't run.

Stack non-empty:

```text
[4,2]
```

Therefore:

```text
ans[1]++
```

So:

```text
ans[1] = 1
```

Push:

```text
stack = [4,2,1]
```

---

# i = 0 — Height 10

Current:

```text
10
```

Stack:

```text
[4,2,1]
```

Top index:

```text
1
```

Top height:

```text
6
```

Check:

```text
6 < 10
```

True.

So:

```text
ans[0]++
```

Now:

```text
ans[0] = 1
```

Pop:

```text
stack = [4,2]
```

---

## While Again

Top index:

```text
2
```

Top height:

```text
8
```

Check:

```text
8 < 10
```

True.

So:

```text
ans[0]++
```

Now:

```text
ans[0] = 2
```

Pop:

```text
stack = [4]
```

---

## While Again

Top index:

```text
4
```

Top height:

```text
11
```

Check:

```text
11 < 10
```

False.

While stop.

Stack non-empty:

```text
[4]
```

So:

```text
ans[0]++
```

Now:

```text
ans[0] = 3
```

Push:

```text
stack = [4,0]
```

---

# Final Answer

```text
ans = [3,1,2,1,1,0]
```

Correct:

```text
[3,1,2,1,1,0]
```

---

# Most Important Dry Run — Height 4

Example:

```text
heights = [4,2,3,5]
```

For current `4`:

Stack:

```text
[3,2,1]
```

Corresponding heights:

```text
5,3,2
```

### Top = 2

```text
2 < 4
```

Visible:

```text
count = 1
```

Pop.

### Top = 3

```text
3 < 4
```

Visible:

```text
count = 2
```

Pop.

### Top = 5

```text
5 < 4
```

False.

While stops.

Stack non-empty:

```text
[3]
```

So `5` is the first taller/equal person.

```text
count = 3
```

Final:

```text
4 can see:
2,3,5
```

Answer:

```text
3
```

---

# Why `if (!st.empty())` Works

Important:

```cpp
if (!st.empty()) {
    ans[i]++;
}
```

Ye khud ye check nahi karta:

```text
"Is top taller?"
```

Instead, `while` ke end par ye already guaranteed hai.

While ki condition thi:

```cpp
heights[st.top()] < heights[i]
```

Jab while stop hua, iska matlab:

```text
heights[st.top()] < heights[i]
```

false hai.

Therefore:

```text
heights[st.top()] >= heights[i]
```

So jo person bacha hai:

```text
taller OR equal
```

hai.

Isi liye `+1`.

---

# Why `st.push(i)`?

```cpp
st.push(i);
```

Current person ka index Stack mein save kar rahe hain.

Example:

```text
i = 2
```

to:

```text
st.push(2);
```

Later kisi left-side person ke liye:

```cpp
heights[st.top()]
```

se us person ki height mil sakti hai.

---

# Why Stack Stores Indexes?

Because:

```cpp
heights[st.top()]
```

chahiye.

Example:

```text
st.top() = 4
```

Then:

```text
heights[4] = 11
```

So Stack:

```text
[4,2,1]
```

actually means:

```text
11,8,6
```

---

# Why Right To Left?

Question right side ki visibility poochta hai.

So:

```text
RIGHT → LEFT
```

better hai.

Jab current person par aate hain:

```text
current
   ↓
right side already processed
   ↓
Stack contains useful right-side people
```

---

# Why `while`, Not `if`?

Ek current person multiple shorter people ko dekh sakta hai.

Example:

```text
4 → 2 → 3 → 5
```

Current `4`:

```text
2 < 4 → visible
3 < 4 → visible
5 > 4 → visible, stop
```

So:

```text
2 pops
3 pops
5 remains
```

Ek hi current element ke liye multiple pops possible hain.

Therefore:

```cpp
while(...)
```

---

# Stack Pattern

Ye **Monotonic Decreasing Stack** idea hai.

Stack mein heights roughly:

```text
11
8
6
```

jaise decreasing order maintain hoti hain.

Jab larger current aata hai:

```text
current > top
```

to shorter elements pop ho jaate hain.

---

# Common Mistakes

## Mistake 1 — Stack mein Height Push Karna

Wrong:

```cpp
st.push(heights[i]);
```

Correct:

```cpp
st.push(i);
```

Because later:

```cpp
heights[st.top()]
```

use karna hai.

---

## Mistake 2 — Left to Right Automatically Karna

Question right side ki visibility poochta hai.

Isliye right to left zyada natural hai.

---

## Mistake 3 — `if` Instead of `while`

Wrong:

```cpp
if (!st.empty() && heights[st.top()] < heights[i])
```

Correct:

```cpp
while (!st.empty() && heights[st.top()] < heights[i])
```

Multiple shorter people visible ho sakte hain.

---

## Mistake 4 — Remaining Taller Person Ko Ignore Karna

While ke baad:

```cpp
if (!st.empty()) {
    ans[i]++;
}
```

important hai.

First taller/equal person bhi visible hota hai.

---

# Complexity

### Time

```text
O(n)
```

Har index Stack mein maximum ek baar push aur ek baar pop hota hai.

### Space

```text
O(n)
```

Stack + answer array.

---

# Interview Explanation

> I process the queue from right to left using a monotonic decreasing stack of indices. For each person, all shorter people on top of the stack are visible, so I count them and pop them. After removing all shorter people, if the stack is still non-empty, the remaining top person is the first taller or equal person and is also visible, so I add one. Finally, I push the current index. This gives O(n) time and O(n) space.

---

# One-Line Revision

> **Right se left jao, current se chhote Stack-top people ko count karke pop karo, aur agar ek taller/equal person Stack mein bacha hai to usko bhi count karo.**

---

# Pattern #4 Progress

* [x] LC 1019 — Next Greater Node In Linked List
* [x] LC 1944 — Number of Visible People in a Queue
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
│   ├── 01-Next-Greater-Node-In-Linked-List.md
│   └── 02-Number-of-Visible-People-in-a-Queue.md
│
├── 05-Boundary-Contribution/
│
└── 06-Stack-Design/
```

