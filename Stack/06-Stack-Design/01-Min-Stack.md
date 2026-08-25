# LeetCode 155 — Min Stack

**Pattern:** Stack — Stack Design

---

# Problem

Hume ek special Stack design karna hai.

Normal Stack ke operations:

```text
push(x)
pop()
top()
```

ke saath hume ek extra operation bhi chahiye:

```text
getMin()
```

`getMin()` ko Stack ke andar ka **minimum element** return karna hai.

Important condition:

```text
push()
pop()
top()
getMin()
```

sab operations **O(1)** time mein hone chahiye.

---

# Example

Operations:

```text
push(-2)
push(0)
push(-3)
getMin()
pop()
top()
getMin()
```

### After push(-2)

Stack:

```text
[-2]
```

Minimum:

```text
-2
```

---

### After push(0)

Stack:

```text
[-2,0]
```

Minimum:

```text
-2
```

---

### After push(-3)

Stack:

```text
[-2,0,-3]
```

Minimum:

```text
-3
```

So:

```text
getMin()
```

returns:

```text
-3
```

---

### After pop()

`-3` remove ho jaata hai.

Stack:

```text
[-2,0]
```

Now:

```text
top()
```

returns:

```text
0
```

And:

```text
getMin()
```

returns:

```text
-2
```

---

# Normal Stack Ki Problem

Normal Stack mein:

```text
push()
pop()
top()
```

easy hain.

But:

```text
getMin()
```

agar normal Stack se karna ho, to minimum find karne ke liye poora Stack scan karna padega.

Example:

```text
[5,3,7,2,9]
```

Minimum:

```text
2
```

find karne ke liye:

```text
5
3
7
2
9
```

sab check karna padega.

So:

```text
getMin() = O(n)
```

But question demand karta hai:

```text
getMin() = O(1)
```

---

# Main Idea

Hum **2 Stacks** maintain karenge.

```text
Stack 1 → Normal Stack
Stack 2 → Minimum Stack
```

---

# Stack 1 — Normal Stack

Isme actual values store hongi.

```text
st
```

Example:

```text
[-2,0,-3]
```

---

# Stack 2 — Min Stack

Isme har level par **current minimum** store karenge.

```text
minSt
```

Example:

```text
push(-2)
push(0)
push(-3)
```

Normal Stack:

```text
st = [-2,0,-3]
```

Minimum Stack:

```text
minSt = [-2,-2,-3]
```

Meaning:

```text
After -2 → minimum = -2
After  0 → minimum = -2
After -3 → minimum = -3
```

---

# Why This Works

Har position par `minSt` remember kar raha hai:

> **Is point tak Stack ka minimum kya hai?**

Isliye jab bhi:

```text
getMin()
```

call hoga, hume poora Stack scan nahi karna padega.

Simply:

```cpp
minSt.top()
```

return kar denge.

So:

```text
getMin() = O(1)
```

---

# Push Operation

Suppose:

```text
push(x)
```

Agar normal Stack hai:

```cpp
st.push(x);
```

Minimum Stack mein hume compare karna hai:

```text
current value
vs
previous minimum
```

Formula:

```text
new minimum = min(x, previous minimum)
```

---

# Example 1 — First Push

```text
push(-2)
```

Normal Stack:

```text
st = [-2]
```

Min Stack empty hai.

To:

```text
minSt = [-2]
```

---

# Example 2 — Push 0

Current:

```text
0
```

Previous minimum:

```text
-2
```

Compare:

```text
min(0,-2)
=
-2
```

So:

```text
st    = [-2,0]
minSt = [-2,-2]
```

---

# Example 3 — Push -3

Current:

```text
-3
```

Previous minimum:

```text
-2
```

Compare:

```text
min(-3,-2)
=
-3
```

So:

```text
st    = [-2,0,-3]
minSt = [-2,-2,-3]
```

---

# Push Ka Core Formula

```text
new minimum
=
min(current value, previous minimum)
```

Code:

```cpp
minSt.push(min(val, minSt.top()));
```

---

# Pop Operation

Normal Stack se top remove:

```cpp
st.pop();
```

Min Stack se bhi corresponding top remove:

```cpp
minSt.pop();
```

Dono Stacks ka size same rahega.

Example:

Before:

```text
st    = [-2,0,-3]
minSt = [-2,-2,-3]
```

After:

```text
pop()
```

becomes:

```text
st    = [-2,0]
minSt = [-2,-2]
```

Ab `minSt.top()`:

```text
-2
```

So current minimum automatically correct hai.

---

# Why Pop Both?

Because both stacks same positions represent kar rahe hain.

Example:

```text
Position 0
st    → -2
minSt → -2

Position 1
st    → 0
minSt → -2

Position 2
st    → -3
minSt → -3
```

Agar normal Stack se top remove kiya hai:

```text
Position 2
```

to Min Stack se bhi:

```text
Position 2
```

remove karna padega.

Isliye:

```cpp
st.pop();
minSt.pop();
```

---

# Top Operation

Normal Stack ka top directly:

```cpp
st.top()
```

return karega.

Example:

```text
st = [-2,0]
```

Then:

```text
top()
=
0
```

Complexity:

```text
O(1)
```

---

# getMin Operation

Minimum Stack ka top:

```cpp
minSt.top()
```

current minimum hai.

Example:

```text
minSt = [-2,-2]
```

So:

```text
getMin()
=
-2
```

Complexity:

```text
O(1)
```

---

# Complete Algorithm

```text
Start
  ↓
2 stacks banao
  ↓
Normal Stack
+
Minimum Stack
  ↓
push(x)
  ↓
Normal Stack mein x push karo
  ↓
Min Stack mein:
min(x, previous minimum) push karo
  ↓
pop()
  ↓
Dono stacks se top pop karo
  ↓
top()
  ↓
Normal Stack ka top
  ↓
getMin()
  ↓
Min Stack ka top
```

---

# Complete C++ Code

```cpp
class MinStack {
public:

    stack<int> st;
    stack<int> minSt;

    MinStack() {
        
    }
    
    void push(int val) {

        st.push(val);

        if (minSt.empty()) {
            minSt.push(val);
        }
        else {
            minSt.push(min(val, minSt.top()));
        }
    }
    
    void pop() {

        st.pop();
        minSt.pop();
    }
    
    int top() {

        return st.top();
    }
    
    int getMin() {

        return minSt.top();
    }
};
```

---

# Code Line-by-Line

## 1. Normal Stack

```cpp
stack<int> st;
```

Actual values store hongi.

---

## 2. Minimum Stack

```cpp
stack<int> minSt;
```

Har level ka minimum store karega.

---

# Constructor

```cpp
MinStack() {
    
}
```

Constructor hai.

Object create hone par call hota hai.

Hume yahan kuch initialize nahi karna because dono stacks automatically empty start hoti hain.

---

# Push

```cpp
void push(int val)
```

Stack mein `val` insert karna hai.

---

## Normal Stack Push

```cpp
st.push(val);
```

Actual value save kar do.

---

## Min Stack Empty?

```cpp
if (minSt.empty())
```

Agar first element hai, to wahi current minimum hoga.

So:

```cpp
minSt.push(val);
```

---

## Min Stack Already Has Value

```cpp
else {
    minSt.push(min(val, minSt.top()));
}
```

Current value:

```text
val
```

Previous minimum:

```text
minSt.top()
```

Dono mein jo chhota hai:

```text
min(val, minSt.top())
```

Min Stack mein save karo.

---

# Pop

```cpp
void pop()
```

Normal Stack se:

```cpp
st.pop();
```

Min Stack se:

```cpp
minSt.pop();
```

Dono synchronized rahenge.

---

# Top

```cpp
int top() {
    return st.top();
}
```

Normal Stack ke top ko return karo.

---

# getMin

```cpp
int getMin() {
    return minSt.top();
}
```

Minimum Stack ka top hi current minimum hai.

---

# Detailed Dry Run

Operations:

```text
push(-2)
push(0)
push(-3)
getMin()
pop()
top()
getMin()
```

Initial:

```text
st    = []
minSt = []
```

---

# Operation 1 — push(-2)

Normal Stack:

```text
st = [-2]
```

Min Stack empty tha.

So:

```text
minSt = [-2]
```

---

# Operation 2 — push(0)

Normal:

```text
st = [-2,0]
```

Previous minimum:

```text
-2
```

Current:

```text
0
```

Compare:

```text
min(0,-2)
=
-2
```

So:

```text
minSt = [-2,-2]
```

---

# Operation 3 — push(-3)

Normal:

```text
st = [-2,0,-3]
```

Previous minimum:

```text
-2
```

Current:

```text
-3
```

Compare:

```text
min(-3,-2)
=
-3
```

So:

```text
minSt = [-2,-2,-3]
```

---

# Operation 4 — getMin()

Simply:

```cpp
minSt.top()
```

Min Stack:

```text
[-2,-2,-3]
```

Top:

```text
-3
```

Answer:

```text
-3
```

No scanning.

Therefore:

```text
O(1)
```

---

# Operation 5 — pop()

Before:

```text
st    = [-2,0,-3]
minSt = [-2,-2,-3]
```

Pop both:

```text
st    = [-2,0]
minSt = [-2,-2]
```

---

# Operation 6 — top()

```cpp
st.top()
```

Normal Stack:

```text
[-2,0]
```

Top:

```text
0
```

Answer:

```text
0
```

---

# Operation 7 — getMin()

Min Stack:

```text
[-2,-2]
```

Top:

```text
-2
```

Answer:

```text
-2
```

---

# Another Example

Operations:

```text
push(5)
push(3)
push(7)
push(2)
```

Normal Stack:

```text
[5,3,7,2]
```

Min Stack:

```text
[5,3,3,2]
```

Har level ka minimum:

```text
5         → min = 5
5,3       → min = 3
5,3,7     → min = 3
5,3,7,2   → min = 2
```

So:

```text
getMin()
=
2
```

Now `pop()`:

```text
st    = [5,3,7]
minSt = [5,3,3]
```

Now:

```text
getMin()
=
3
```

Again `pop()`:

```text
st    = [5,3]
minSt = [5,3]
```

So:

```text
getMin()
=
3
```

---

# Why Not Just One `min` Variable?

Suppose:

```text
push(5)
push(3)
push(2)
```

Current minimum:

```text
2
```

Agar ek variable:

```text
min = 2
```

rakha.

Ab `pop()` karke `2` remove kar diya.

Current minimum kya hai?

```text
3
```

Ab hume dobara Stack scan karna padega.

So `pop()` ke baad minimum efficiently recover karna difficult hai.

---

# Why Second Stack Solves It

Min Stack already previous minimum yaad rakhta hai.

Before pop:

```text
st    = [5,3,2]
minSt = [5,3,2]
```

After pop:

```text
st    = [5,3]
minSt = [5,3]
```

So previous minimum:

```text
3
```

automatically available hai.

No scanning required.

---

# Why Two Stacks Are Synchronized

Position wise:

```text
Normal Stack    Min Stack
-----------     ---------
5               5
3               3
7               3
2               2
```

Each `minSt` entry tells:

> Is position tak Stack ka minimum kya hai?

Isliye jab top remove hota hai:

```text
Normal Stack → pop
Min Stack    → pop
```

both remain synchronized.

---

# Core Pattern

```text
Normal Stack
     +
Auxiliary Stack
     ↓
Extra information maintain karo
     ↓
O(1) operation achieve karo
```

Min Stack mein auxiliary stack:

```text
minSt
```

minimum information maintain kar raha hai.

---

# Common Mistakes

## Mistake 1 — Sirf Normal Stack mein push karna

Wrong:

```cpp
st.push(val);
```

and Min Stack update nahi karna.

Correct:

```cpp
st.push(val);
minSt.push(...);
```

---

## Mistake 2 — Pop sirf Normal Stack se karna

Wrong:

```cpp
st.pop();
```

Correct:

```cpp
st.pop();
minSt.pop();
```

Dono synchronized hone chahiye.

---

## Mistake 3 — getMin() mein poora Stack scan karna

Wrong:

```text
Stack scan → minimum find
```

Isse:

```text
O(n)
```

lagta hai.

Correct:

```cpp
return minSt.top();
```

So:

```text
O(1)
```

---

# Complexity

### push()

```text
O(1)
```

### pop()

```text
O(1)
```

### top()

```text
O(1)
```

### getMin()

```text
O(1)
```

### Space

```text
O(n)
```

Because two stacks maintain kar rahe hain.

---

# Interview Explanation

> I use two stacks. The first stack stores the actual values, while the second stack stores the minimum value seen up to each corresponding position. On every push, I push the new value into the normal stack and `min(value, current minimum)` into the minimum stack. On pop, I pop from both stacks. The top operation uses the normal stack, and `getMin()` simply returns the top of the minimum stack. This makes all operations O(1).

---

# One-Line Revision

> **Normal Stack values rakhta hai, Min Stack har level ka current minimum rakhta hai; isliye `getMin() = minSt.top()` directly O(1) mein mil jaata hai.**

---

# Pattern #6 Progress

```text
[x] LC 155 — Min Stack
[ ] LC 225 — Implement Stack using Queues
[ ] LC 716 — Max Stack
```

# Stack Roadmap Progress

```text
Pattern #1 — Matching / Pairing ✅
Pattern #3 — Monotonic Stack ✅
Pattern #4 — Next / Previous Greater-Smaller ✅
Pattern #5 — Boundary / Contribution ✅
Pattern #6 — Stack Design 🔄
Pattern #2 — Stack Simulation ⏸️
```
```
