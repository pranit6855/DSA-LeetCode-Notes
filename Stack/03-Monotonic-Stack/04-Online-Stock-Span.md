# LeetCode 901 — Online Stock Span

**Pattern:** Stack — Monotonic Stack

---

# Problem

Hume stock ki daily price one by one receive hoti hai.

Har new price ke liye hume uska **span** return karna hai.

### Span ka meaning

> Current price se **less than or equal** price kitne consecutive previous days tak thi, including today.

---

# Example

Prices:

```text
100, 80, 60, 70, 60, 75, 85
```

Output:

```text
1, 1, 1, 2, 1, 4, 6
```

---

# Example Explanation

## Price = 100

Pehla day hai.

Sirf current day count hoga:

```text
span = 1
```

---

## Price = 80

Previous price:

```text
100
```

Check:

```text
100 <= 80
```

False.

So:

```text
span = 1
```

---

## Price = 60

Previous:

```text
80
```

Check:

```text
80 <= 60
```

False.

So:

```text
span = 1
```

---

## Price = 70

Previous:

```text
60
```

Check:

```text
60 <= 70
```

True.

So:

```text
60, 70
```

dono count honge.

```text
span = 2
```

Lekin `80 > 70`, so wahi stop.

---

## Price = 60

Previous price:

```text
70
```

Check:

```text
70 <= 60
```

False.

So:

```text
span = 1
```

---

## Price = 75

Previous prices:

```text
100, 80, 60, 70, 60
```

Peeche se check:

```text
60 <= 75
70 <= 75
60 <= 75
80 <= 75  ❌
```

So current day ke saath total:

```text
60, 70, 60, 75
```

Span:

```text
4
```

---

## Price = 85

Peeche:

```text
75 <= 85
60 <= 85
70 <= 85
60 <= 85
80 <= 85
100 <= 85 ❌
```

So total:

```text
75, 60, 70, 60, 80, 85
```

Span:

```text
6
```

---

# Pattern Identification

Agar question mein:

```text
stock span
previous smaller
previous greater
consecutive previous values
less than or equal
```

jaise concepts aayein, to:

```text
MONOTONIC STACK
```

socho.

---

# Main Idea

Brute force mein har new price ke liye peeche jaakar prices check kar sakte hain.

Lekin worst case mein:

```text
O(n²)
```

ho sakta hai.

Hum **Monotonic Decreasing Stack** use karenge.

---

# What Does Stack Store?

Stack mein sirf price store nahi karenge.

Hum:

```text
(price, span)
```

store karenge.

C++:

```cpp
stack<pair<int,int>> st;
```

### Pair ka meaning

```text
first  = price
second = span
```

Example:

```text
(70,2)
```

matlab:

> Price `70` ka already calculated span `2` hai.

---

# Why Store Span?

Ye question ka main trick hai.

Suppose Stack mein:

```text
(70,2)
```

hai.

Iska matlab `70` ke span mein already 2 days included hain:

```text
60, 70
```

Ab current price:

```text
75
```

aati hai.

Since:

```text
75 >= 70
```

to sirf `70` ko count nahi karna.

`70` ka poora span bhi current span mein include hoga.

So:

```text
current span = 1 + 2
              = 3
```

Isi ko **span compression** samajh sakte ho.

---

# Core Rule

Har new price ke liye:

```text
span = 1
```

Current day khud count hoga.

Phir:

```text
while stack non-empty
AND
top price <= current price
```

to:

```text
span += top span
pop()
```

Finally:

```text
(price, span)
```

Stack mein push karo.

---

# Algorithm

```text
Start
  ↓
Empty Stack banao
  ↓
New price receive karo
  ↓
span = 1
  ↓
Stack empty nahi hai?
AND
top price <= current price?
  ↓
YES
  ↓
current span += top span
  ↓
pop
  ↓
Again check
  ↓
Current (price, span) push karo
  ↓
Return span
```

---

# Step-by-Step Approach

## Step 1 — Stack

```cpp
stack<pair<int, int>> st;
```

Stack:

```text
(price, span)
```

store karega.

---

# Step 2 — `next(price)`

Har baar LeetCode new price ke liye:

```cpp
next(price)
```

call karta hai.

Example:

```cpp
next(100);
next(80);
next(60);
```

Har call corresponding span return karega.

---

# Step 3 — Current Span

```cpp
int span = 1;
```

Current day ko include karna hai.

Isliye initially `1`.

---

# Step 4 — While Condition

```cpp
while (!st.empty() && st.top().first <= price)
```

Do conditions:

### Condition 1

```cpp
!st.empty()
```

Stack empty nahi hona chahiye.

### Condition 2

```cpp
st.top().first <= price
```

Top price current price se chhoti ya equal honi chahiye.

---

# Step 5 — Top Span Add Karo

```cpp
span += st.top().second;
```

Ye important line hai.

Example:

```text
current span = 1
top span = 2
```

Then:

```text
span = 1 + 2
     = 3
```

Matlab top price ke peeche ke 2 valid days bhi current span mein add ho gaye.

---

# Step 6 — Pop

```cpp
st.pop();
```

Top ka span current mein include ho chuka hai, so ab Stack mein uski need nahi.

---

# Step 7 — Current Pair Push

```cpp
st.push({price, span});
```

Current price ke saath uska complete span save kar do.

---

# Step 8 — Return

```cpp
return span;
```

Current price ka answer mil gaya.

---

# Complete C++ Code

```cpp
class StockSpanner {
public:
    stack<pair<int, int>> st;

    StockSpanner() {
    }
    
    int next(int price) {

        int span = 1;

        while (!st.empty() && st.top().first <= price) {

            span += st.top().second;
            st.pop();
        }

        st.push({price, span});

        return span;
    }
};
```

---

# Constructor

```cpp
StockSpanner() {
}
```

Ye constructor hai.

Jab object create hota hai:

```cpp
StockSpanner spanner;
```

constructor automatically call hota hai.

Yahan constructor empty hai because Stack ko separately initialize karne ki zarurat nahi.

Stack automatically empty start hota hai.

---

# Detailed Dry Run

Prices:

```text
100, 80, 60, 70, 60, 75, 85
```

Expected:

```text
1, 1, 1, 2, 1, 4, 6
```

---

# Day 1 — `next(100)`

Initial:

```text
stack = []
```

```cpp
span = 1;
```

So:

```text
span = 1
```

While:

```text
stack empty
```

so no pop.

Push:

```text
(100,1)
```

Stack:

```text
(100,1)
```

Return:

```text
1
```

---

# Day 2 — `next(80)`

Start:

```text
stack = [(100,1)]
span = 1
```

Top price:

```text
100
```

Check:

```text
100 <= 80
```

False.

No pop.

Push:

```text
(80,1)
```

Stack:

```text
(100,1)
(80,1)
```

Return:

```text
1
```

---

# Day 3 — `next(60)`

Start:

```text
stack = [(100,1),(80,1)]
span = 1
```

Top:

```text
80
```

Check:

```text
80 <= 60
```

False.

Push:

```text
(60,1)
```

Stack:

```text
(100,1)
(80,1)
(60,1)
```

Return:

```text
1
```

---

# Day 4 — `next(70)`

Start:

```text
stack = [(100,1),(80,1),(60,1)]
span = 1
```

Top:

```text
(60,1)
```

Check:

```text
60 <= 70
```

True.

Add top span:

```text
span = 1 + 1
     = 2
```

Pop:

```text
stack = [(100,1),(80,1)]
```

Check again.

Top:

```text
80
```

```text
80 <= 70
```

False.

Stop.

Push:

```text
(70,2)
```

Stack:

```text
(100,1)
(80,1)
(70,2)
```

Return:

```text
2
```

---

# Day 5 — `next(60)`

Start:

```text
stack = [(100,1),(80,1),(70,2)]
span = 1
```

Top:

```text
70
```

Check:

```text
70 <= 60
```

False.

Push:

```text
(60,1)
```

Stack:

```text
(100,1)
(80,1)
(70,2)
(60,1)
```

Return:

```text
1
```

---

# Day 6 — `next(75)`

This is the most important case.

Start:

```text
stack = [(100,1),(80,1),(70,2),(60,1)]
span = 1
```

Current:

```text
75
```

---

## First Pop

Top:

```text
(60,1)
```

Check:

```text
60 <= 75
```

True.

Add span:

```text
span = 1 + 1
     = 2
```

Pop:

```text
stack = [(100,1),(80,1),(70,2)]
```

---

## Second Pop

Top:

```text
(70,2)
```

Check:

```text
70 <= 75
```

True.

Now add its complete span:

```text
span = 2 + 2
     = 4
```

Pop:

```text
stack = [(100,1),(80,1)]
```

---

## Next Check

Top:

```text
80
```

Check:

```text
80 <= 75
```

False.

Stop.

Push:

```text
(75,4)
```

Stack:

```text
(100,1)
(80,1)
(75,4)
```

Return:

```text
4
```

---

# Why Span = 4?

Current:

```text
75
```

Previous consecutive valid prices:

```text
60
70
60
75
```

Total:

```text
4
```

But we didn't manually count all 4.

We used:

```text
1
+
1
+
2
```

Where:

```text
1 = current day
1 = previous 60
2 = 70 ka already calculated span
```

So:

```text
1 + 1 + 2 = 4
```

This is the main optimization.

---

# Day 7 — `next(85)`

Start:

```text
stack = [(100,1),(80,1),(75,4)]
span = 1
```

Current:

```text
85
```

---

## First Pop

Top:

```text
(75,4)
```

Check:

```text
75 <= 85
```

True.

Add:

```text
span = 1 + 4
     = 5
```

Pop.

Stack:

```text
(100,1)
(80,1)
```

---

## Second Pop

Top:

```text
(80,1)
```

Check:

```text
80 <= 85
```

True.

Add:

```text
span = 5 + 1
     = 6
```

Pop.

Stack:

```text
(100,1)
```

---

## Next Check

Top:

```text
100
```

Check:

```text
100 <= 85
```

False.

Stop.

Push:

```text
(85,6)
```

Return:

```text
6
```

---

# Final Output

```text
Price:  100  80  60  70  60  75  85
Span:     1   1   1   2   1   4   6
```

So:

```text
[1,1,1,2,1,4,6]
```

---

# Why `<=`?

Question bolta hai:

> Less than **or equal to** current price.

Suppose:

```text
100, 100
```

Second `100` ke liye:

```text
100 <= 100
```

true hai.

So second `100` ka span:

```text
2
```

Isliye condition:

```cpp
st.top().first <= price
```

hai.

---

# Why `span = 1`?

Current day khud included hai.

Suppose:

```text
price = 60
```

Agar koi previous valid price nahi hai:

```text
span = 1
```

because current day itself counts.

---

# Why Stack Is Monotonic?

Stack mein prices:

```text
100
80
70
60
```

jaise decreasing order maintain karte hain.

Jab current price bigger hota hai:

```text
current >= top
```

to smaller/equal values pop ho jaati hain.

Isliye:

**Monotonic Decreasing Stack**

---

# Important Learning

Is question ka main concept:

```text
Previous smaller/equal prices
        ↓
Their spans already known
        ↓
Current price bigger/equal
        ↓
Add those spans
        ↓
Pop them
```

---

# 739 vs 901

## LC 739 — Daily Temperatures

```text
current > top
↓
pop
↓
distance calculate
```

## LC 901 — Stock Span

```text
current >= top
↓
pop
↓
previous span add
```

So Monotonic Stack ka **engine same hai**, lekin answer calculate karne ka logic different hai.

---

# Common Mistakes

## Mistake 1 — Sirf price store karna

Wrong:

```cpp
stack<int> st;
```

Better:

```cpp
stack<pair<int,int>> st;
```

Because hume price ke saath span bhi remember karna hai.

---

## Mistake 2 — `span = 0`

Wrong:

```cpp
int span = 0;
```

Correct:

```cpp
int span = 1;
```

Current day khud count hona chahiye.

---

## Mistake 3 — `<` use karna

Wrong:

```cpp
st.top().first < price
```

Correct:

```cpp
st.top().first <= price
```

Because equal prices bhi span mein count hoti hain.

---

## Mistake 4 — Sirf `span++`

Wrong idea:

```text
70 >= 60
→ span++
```

Ye enough nahi hai.

Correct:

```cpp
span += st.top().second;
```

Because popped price ke peeche already multiple days ka span ho sakta hai.

---

# Complexity

## Time Complexity

```text
O(n)
```

Har price Stack mein maximum ek baar push aur ek baar pop hota hai.

Although ek `next()` call ke andar multiple pops ho sakte hain, total operations across all calls linear rehte hain.

---

## Space Complexity

```text
O(n)
```

Worst case Stack mein `n` pairs ho sakte hain.

---

# Interview Explanation

> I maintain a decreasing monotonic stack storing pairs of `(price, span)`. For every new price, I start its span at 1 for the current day. While the top price is less than or equal to the current price, I add the top element's already-computed span to the current span and pop it. This lets me skip multiple previous days at once because their span has already been compressed. Finally, I push `(price, span)` and return the span.

---

# One-Line Revision

> **Current price se chhoti/equal top prices ko pop karo aur unke stored spans ko current span mein add karo.**

---

# Pattern #3 Progress

* [x] LC 739 — Daily Temperatures
* [x] LC 496 — Next Greater Element I
* [x] LC 503 — Next Greater Element II
* [x] LC 901 — Online Stock Span
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
│   ├── 03-Next-Greater-Element-II.md
│   └── 04-Online-Stock-Span.md
│
├── 04-Next-Previous-Greater-Smaller/
│
├── 05-Boundary-Contribution/
│
└── 06-Stack-Design/
```
