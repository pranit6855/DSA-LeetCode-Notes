# LeetCode 739 — Daily Temperatures

**Pattern:** Stack — Monotonic Stack

**Filename:**  

**Folder:** `Stack/03-Monotonic-Stack/`

---

# Problem

Hume ek integer array `temperatures` diya gaya hai.

Har day ke liye hume find karna hai:

> **Aage kitne days wait karne padenge taaki ek warmer temperature mile.**

Agar future mein koi warmer temperature nahi milta, to answer `0` hoga.

---

# Example

```text
temperatures = [73,74,75,71,69,72,76,73]
```

Output:

```text
[1,1,4,2,1,1,0,0]
```

---

# Example Explanation

## Day 0

Temperature:

```text
73
```

Next day:

```text
74
```

`74 > 73`

So answer:

```text
1
```

---

## Day 1

Temperature:

```text
74
```

Next day:

```text
75
```

`75 > 74`

So:

```text
1
```

---

## Day 2

Temperature:

```text
75
```

Aage temperatures:

```text
71, 69, 72, 76
```

Pehla warmer temperature:

```text
76
```

Day `6` par hai.

Current index = `2`

So:

```text
6 - 2 = 4
```

Answer:

```text
4
```

---

## Day 3

Temperature:

```text
71
```

First warmer temperature:

```text
72
```

Day `5` par.

So:

```text
5 - 3 = 2
```

---

## Day 4

Temperature:

```text
69
```

Next warmer:

```text
72
```

Difference:

```text
5 - 4 = 1
```

---

## Day 5

Temperature:

```text
72
```

Next warmer:

```text
76
```

Difference:

```text
6 - 5 = 1
```

---

## Day 6

Temperature:

```text
76
```

Aage:

```text
73
```

Koi warmer temperature nahi.

So:

```text
0
```

---

## Day 7

Last element hai.

Koi future day nahi.

So:

```text
0
```

---

# Pattern Identification

Agar question mein ye words aayein:

```text
next greater
warmer temperature
next larger element
first greater element
nearest greater
```

to Monotonic Stack ka thought aana chahiye.

Ye question:

```text
Monotonic Stack
```

ka classic example hai.

---

# Why Monotonic Stack?

Brute force approach mein har temperature ke liye hum future mein search kar sakte hain.

Example:

```text
73 → future mein search
74 → future mein search
75 → future mein search
...
```

Worst case mein bahut saare comparisons honge.

### Brute Force Complexity

```text
O(n²)
```

Hume isko optimize karna hai.

Yahan Monotonic Stack use karenge.

---

# Main Idea

Hum Stack mein **temperatures nahi**, unke **indexes** store karenge.

Example:

```text
temperatures = [73,74,75,71,...]
```

Indexes:

```text
0 1 2 3 ...
```

Stack:

```cpp
stack<int> st;
```

---

# Why Index Store Karna?

Kyuki answer mein hume distance chahiye:

```text
current index - old index
```

Example:

```text
old index = 3
current index = 5
```

To:

```text
5 - 3 = 2 days
```

Agar sirf temperature store karenge:

```text
71
72
```

to distance directly nahi milega.

Isliye **index store karna important hai**.

---

# Monotonic Stack Type

Is problem mein hum:

```text
Monotonic Decreasing Stack
```

use karenge.

Matlab Stack mein corresponding temperatures:

```text
75
71
69
```

jaise decreasing order mein rahenge.

Example:

```text
stack indexes = [2,3,4]

temperatures:
index 2 → 75
index 3 → 71
index 4 → 69
```

So:

```text
75 > 71 > 69
```

---

# Main Rule

Current temperature ko Stack ke top temperature se compare karo.

```text
current temperature > stack top temperature
```

Agar YES:

```text
POP
```

Kyun?

Kyuki current temperature us purane temperature ka **first warmer temperature** mil gaya.

Agar NO:

```text
PUSH
```

Current element ko future warmer temperature ka wait karne do.

---

# Core Logic

```text
Current temperature
        ↓
Compare with Stack Top
        ↓
Current > Top?
   ┌───────────┐
   YES         NO
    ↓           ↓
  POP         PUSH
    ↓
answer[index] = currentIndex - index
```

Aur ye `POP`:

```text
```

tab tak karte rahenge jab tak:

```text
current <= stack top
```

---

# Algorithm

```text
Start
  ↓
Answer array banao
  ↓
Empty Stack banao
  ↓
Har index i ko traverse karo
  ↓
Jab tak:
    Stack empty nahi hai
    AND
    current temperature > stack top temperature

    ↓

    old index = stack.top()
    pop()

    answer[old index] = i - old index

  ↓
Current index push karo
  ↓
Next index
  ↓
Loop complete
  ↓
Stack mein bache indexes ka answer 0 hi rahega
  ↓
Return answer
```

---

# Step-by-Step Approach

## Step 1 — Answer Array

```cpp
vector<int> ans(n, 0);
```

Initially sab answers:

```text
0
```

kyunki agar kisi element ko warmer temperature nahi mila, answer already `0` hona chahiye.

Example:

```text
ans = [0,0,0,0,0,0,0,0]
```

---

# Step 2 — Stack

```cpp
stack<int> st;
```

Stack mein indexes rakhenge.

---

# Step 3 — Traverse

```cpp
for(int i = 0; i < n; i++)
```

Har index ko left se right process karenge.

---

# Step 4 — While Condition

```cpp
while(!st.empty() &&
      temperatures[i] > temperatures[st.top()])
```

Do conditions hain.

### Condition 1

```cpp
!st.empty()
```

Stack mein element hona chahiye.

### Condition 2

```cpp
temperatures[i] > temperatures[st.top()]
```

Current temperature Stack ke top temperature se greater hona chahiye.

Dono true hone par pop.

---

# Step 5 — Old Index Nikalo

```cpp
int index = st.top();
```

Stack ke top wala old index store karo.

---

# Step 6 — Pop

```cpp
st.pop();
```

Old index ko Stack se remove karo because usko warmer temperature mil gaya.

---

# Step 7 — Answer Calculate

```cpp
ans[index] = i - index;
```

Current day:

```text
i
```

Old day:

```text
index
```

Wait time:

```text
i - index
```

---

# Step 8 — Current Index Push

While loop ke baad:

```cpp
st.push(i);
```

Current index ko future warmer temperature ke liye Stack mein store karo.

---

# Complete C++ Code

```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {

        int n = temperatures.size();

        vector<int> ans(n, 0);

        stack<int> st;

        for(int i = 0; i < n; i++) {

            while(!st.empty() &&
                  temperatures[i] > temperatures[st.top()]) {

                int index = st.top();
                st.pop();

                ans[index] = i - index;
            }

            st.push(i);
        }

        return ans;
    }
};
```

---

# Code Explanation

## `n`

```cpp
int n = temperatures.size();
```

Array ki length.

---

## `ans`

```cpp
vector<int> ans(n, 0);
```

Initially sabko `0` rakha.

Example:

```text
[0,0,0,0,0,0,0,0]
```

---

## Stack

```cpp
stack<int> st;
```

Unresolved days ke indexes store karega.

Meaning:

> Ye days abhi warmer temperature ka wait kar rahe hain.

---

# Detailed Dry Run

Input:

```text
temperatures = [73,74,75,71,69,72,76,73]
```

Indexes:

```text
index:         0   1   2   3   4   5   6   7
temperature:  73  74  75  71  69  72  76  73
```

Initially:

```text
stack = []
ans   = [0,0,0,0,0,0,0,0]
```

---

# i = 0

Current:

```text
73
```

Stack empty hai.

So:

```cpp
st.push(0);
```

Stack:

```text
[0]
```

Temperature represented:

```text
[73]
```

Meaning:

> Day 0 abhi warmer temperature ka wait kar raha hai.

---

# i = 1

Current:

```text
74
```

Stack:

```text
[0]
```

Top temperature:

```text
73
```

Check:

```text
74 > 73
```

True.

So index `0` ko warmer temperature mil gaya.

### Get top

```cpp
int index = st.top();
```

```text
index = 0
```

### Pop

```cpp
st.pop();
```

Stack:

```text
[]
```

### Calculate answer

```cpp
ans[0] = 1 - 0;
```

```text
ans[0] = 1
```

Answer:

```text
[1,0,0,0,0,0,0,0]
```

### Push current

```cpp
st.push(1);
```

Stack:

```text
[1]
```

---

# i = 2

Current:

```text
75
```

Stack:

```text
[1]
```

Top:

```text
74
```

Check:

```text
75 > 74
```

True.

Pop `1`.

```cpp
ans[1] = 2 - 1;
```

So:

```text
ans[1] = 1
```

Answer:

```text
[1,1,0,0,0,0,0,0]
```

Push `2`.

Stack:

```text
[2]
```

---

# i = 3

Current:

```text
71
```

Top:

```text
75
```

Check:

```text
71 > 75
```

False.

So no pop.

Push `3`.

Stack:

```text
[2,3]
```

Temperatures:

```text
75,71
```

Decreasing order.

---

# i = 4

Current:

```text
69
```

Top:

```text
71
```

Check:

```text
69 > 71
```

False.

Push `4`.

Stack:

```text
[2,3,4]
```

Temperatures:

```text
75,71,69
```

Still decreasing.

---

# i = 5

Current:

```text
72
```

Stack:

```text
[2,3,4]
```

Top:

```text
69
```

Check:

```text
72 > 69
```

True.

Pop `4`.

```cpp
ans[4] = 5 - 4;
```

So:

```text
ans[4] = 1
```

Answer:

```text
[1,1,0,0,1,0,0,0]
```

---

## Continue while

Stack:

```text
[2,3]
```

Top temperature:

```text
71
```

Check:

```text
72 > 71
```

True.

Pop `3`.

```cpp
ans[3] = 5 - 3;
```

So:

```text
ans[3] = 2
```

Answer:

```text
[1,1,0,2,1,0,0,0]
```

---

## Check again

Stack:

```text
[2]
```

Top temperature:

```text
75
```

Check:

```text
72 > 75
```

False.

Stop while loop.

Now push current index:

```cpp
st.push(5);
```

Stack:

```text
[2,5]
```

Temperatures:

```text
75,72
```

---

# i = 6

Current:

```text
76
```

Stack:

```text
[2,5]
```

Top:

```text
72
```

Check:

```text
76 > 72
```

True.

Pop `5`.

```cpp
ans[5] = 6 - 5;
```

```text
ans[5] = 1
```

Answer:

```text
[1,1,0,2,1,1,0,0]
```

---

## Continue while

Stack:

```text
[2]
```

Top:

```text
75
```

Check:

```text
76 > 75
```

True.

Pop `2`.

```cpp
ans[2] = 6 - 2;
```

```text
ans[2] = 4
```

Answer:

```text
[1,1,4,2,1,1,0,0]
```

---

## Check Again

Stack:

```text
[]
```

Empty hai.

While stop.

Push `6`:

```text
[6]
```

---

# i = 7

Current:

```text
73
```

Top:

```text
76
```

Check:

```text
73 > 76
```

False.

Push `7`.

Stack:

```text
[6,7]
```

---

# End

Final:

```text
ans = [1,1,4,2,1,1,0,0]
```

Stack:

```text
[6,7]
```

In dono ke liye warmer temperature future mein nahi mila.

Isliye:

```text
ans[6] = 0
ans[7] = 0
```

Already `0` hain.

Return:

```text
[1,1,4,2,1,1,0,0]
```

---

# Important Line

```cpp
ans[index] = i - index;
```

Example:

```text
index = 3
i = 5
```

Temperatures:

```text
day 3 → 71
day 5 → 72
```

Wait:

```text
5 - 3 = 2
```

So:

```text
ans[3] = 2
```

---

# Why While, Not If?

Ye bahut important hai.

Suppose:

```text
[75,71,69,72]
```

Jab `72` aata hai:

```text
72 > 69
```

to `69` pop.

Lekin uske baad:

```text
72 > 71
```

bhi true hai.

To `71` bhi pop karna padega.

Isliye:

```cpp
while(...)
```

use karte hain.

Agar sirf:

```cpp
if(...)
```

use karenge to sirf ek element pop hoga aur answer incomplete rahega.

---

# Why Stack Stays Decreasing?

Suppose Stack temperatures:

```text
75
71
69
```

Now `72` aaya.

`69` smaller hai:

```text
72 > 69
```

pop.

`71` bhi smaller hai:

```text
72 > 71
```

pop.

`75` greater hai:

```text
72 < 75
```

stop.

So Stack becomes:

```text
75
72
```

Still decreasing.

Isi property ko maintain karna hi **Monotonic Stack** hai.

---

# Why Every Element Is Pushed Once and Popped Once?

Har index:

```text
push → maximum 1 time
```

Aur:

```text
pop → maximum 1 time
```

Example:

```text
index 4
↓
push
↓
later pop
```

Dobara push nahi hota.

So total stack operations:

```text
O(n)
```

Hence total time:

```text
O(n)
```

---

# Complexity

## Time Complexity

```text
O(n)
```

Har element maximum ek baar push aur ek baar pop hota hai.

---

## Space Complexity

```text
O(n)
```

Worst case mein Stack mein saare indexes ho sakte hain.

Example:

```text
[90,89,88,87,86]
```

Koi warmer current element nahi aayega, so sab Stack mein rahenge.

---

# Brute Force vs Monotonic Stack

## Brute Force

Har element ke liye future search:

```text
for every i
    for every j > i
```

Complexity:

```text
O(n²)
```

---

## Monotonic Stack

```text
each index:
push once
pop once
```

Complexity:

```text
O(n)
```

---

# Common Mistakes

## Mistake 1 — Temperature Store Karna

Wrong idea:

```cpp
stack<int> st;
st.push(temperatures[i]);
```

Ye useful nahi hai because answer ke liye index difference chahiye.

Correct:

```cpp
st.push(i);
```

---

## Mistake 2 — `if` Instead of `while`

Wrong:

```cpp
if(!st.empty() &&
   temperatures[i] > temperatures[st.top()])
```

Correct:

```cpp
while(!st.empty() &&
      temperatures[i] > temperatures[st.top()])
```

Kyunki ek current temperature multiple previous smaller temperatures ko resolve kar sakta hai.

---

## Mistake 3 — Answer Formula

Wrong:

```cpp
ans[index] = i;
```

Correct:

```cpp
ans[index] = i - index;
```

Hume days ka difference chahiye.

---

## Mistake 4 — Remaining Stack Ko Manually 0 Karna

Iski zarurat nahi.

Humne:

```cpp
vector<int> ans(n, 0);
```

kiya hai.

Isliye jo indexes Stack mein end tak bach gaye, unka answer automatically `0` hai.

---

# Interview Explanation

Agar interviewer puche:

> How does the monotonic stack work?

Bol sakte ho:

> I maintain a decreasing monotonic stack of indices. For every current temperature, while the current temperature is greater than the temperature at the stack top, I pop that index because the current day is the first warmer day for it. The waiting time is the difference between the current index and the popped index. Then I push the current index into the stack. This gives O(n) time because every index is pushed and popped at most once.

---

# Pattern Recognition

Aage agar question bole:

```text
next warmer
next greater
first greater
nearest greater
daily temperature
```

to immediately socho:

```text
MONOTONIC STACK
```

Basic template:

```cpp
while(!st.empty() && current > value[st.top()]) {
    int index = st.top();
    st.pop();

    answer[index] = currentIndex - index;
}

st.push(currentIndex);
```

---

# One-Line Revision

> **Stack mein unresolved indexes rakho; jab current value Stack ke top se greater ho, top ko pop karke uska answer current index - old index se calculate karo.**

---

# Stack Roadmap Progress

## Pattern #3 — Monotonic Stack

* [x] **LeetCode 739 — Daily Temperatures**

### Remaining in Pattern #3

* [ ] LC 496 — Next Greater Element I
* [ ] LC 503 — Next Greater Element II
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
│   └── 01-Daily-Temperatures.md
│
├── 04-Next-Previous-Greater-Smaller/
│
├── 05-Boundary-Contribution/
│
└── 06-Stack-Design/
```
