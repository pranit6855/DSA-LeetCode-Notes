# LeetCode 716 — Max Stack

**Pattern:** Stack — Stack Design

---

# Problem

Hume ek special Stack design karna hai jisme normal Stack ke operations ke saath maximum related operations bhi available hon.

Required operations:

```text
push(x)
pop()
top()
peekMax()
popMax()
```

---

# Stack Ka Normal Behavior

Stack:

```text
LIFO
```

Meaning:

> Last In, First Out

Example:

```text
push(5)
push(1)
push(3)
```

Stack:

```text
[5,1,3]
       ↑
      top
```

So:

```text
top() → 3
```

Aur:

```text
pop() → 3
```

---

# Extra Operations

## `peekMax()`

Stack ka maximum element return karo.

Lekin maximum ko remove nahi karna.

Example:

```text
[5,1,3]
```

Then:

```text
peekMax() → 5
```

Stack same rahega:

```text
[5,1,3]
```

---

# `popMax()`

Stack ka maximum element:

1. find karo
2. remove karo
3. return karo

Example:

```text
[5,1,3]
```

Then:

```text
popMax() → 5
```

Remaining Stack:

```text
[1,3]
```

---

# Important Condition — Duplicate Maximum

Suppose:

```text
[5,1,5,3]
```

Maximum:

```text
5
```

do baar hai.

`popMax()` ko **topmost maximum** remove karna hai.

Topmost maximum:

```text
[5,1,5,3]
     ↑
     5
```

So:

```text
popMax() → 5
```

Remaining:

```text
[5,1,3]
```

Bottom wala `5` remove nahi hoga.

---

# Main Problem

Normal Stack se:

```text
push()
pop()
top()
```

easy hai.

Lekin:

```text
peekMax()
popMax()
```

difficult hai.

`peekMax()` ke liye maximum track karna hoga.

`popMax()` ke liye maximum agar middle mein hai to us tak pahunchna hoga.

---

# Main Approach

Hum **2 permanent Stacks + 1 temporary Stack** use karenge.

```text
1. st
   ↓
   Actual values

2. maxSt
   ↓
   Har level ka current maximum

3. temp
   ↓
   popMax() ke time temporary elements
```

---

# Stack 1 — Normal Stack

```cpp
stack<int> st;
```

Isme actual values store hongi.

Example:

```text
[5,1,5,3]
```

---

# Stack 2 — Maximum Stack

```cpp
stack<int> maxSt;
```

Ye har level par current maximum store karega.

Example:

```text
push(5)
push(1)
push(5)
push(3)
```

Normal Stack:

```text
st = [5,1,5,3]
```

Maximum Stack:

```text
maxSt = [5,5,5,5]
```

Meaning:

```text
After 5 → maximum = 5
After 1 → maximum = 5
After 5 → maximum = 5
After 3 → maximum = 5
```

---

# Why `maxSt`?

Agar sirf normal Stack hota:

```text
[5,1,5,3]
```

to maximum find karne ke liye poora Stack scan karna padta.

That would be:

```text
O(n)
```

But `maxSt` ki wajah se:

```cpp
maxSt.top()
```

direct maximum deta hai.

So:

```text
peekMax() = O(1)
```

---

# Push Operation

Suppose:

```text
push(x)
```

Normal Stack:

```cpp
st.push(x);
```

Maximum Stack mein:

```text
new maximum
=
max(x, previous maximum)
```

---

# Example — push(5)

Initially:

```text
st = []
maxSt = []
```

Push `5`:

```text
st = [5]
maxSt = [5]
```

---

# Example — push(1)

Current value:

```text
1
```

Previous maximum:

```text
5
```

So:

```text
max(1,5) = 5
```

Stacks:

```text
st    = [5,1]
maxSt = [5,5]
```

---

# Example — push(5)

Current:

```text
5
```

Previous maximum:

```text
5
```

So:

```text
max(5,5) = 5
```

Stacks:

```text
st    = [5,1,5]
maxSt = [5,5,5]
```

---

# Example — push(3)

Current:

```text
3
```

Previous maximum:

```text
5
```

So:

```text
max(3,5) = 5
```

Stacks:

```text
st    = [5,1,5,3]
maxSt = [5,5,5,5]
```

---

# Push Code

```cpp
void push(int x) {

    st.push(x);

    if (maxSt.empty()) {
        maxSt.push(x);
    }
    else {
        maxSt.push(max(x, maxSt.top()));
    }
}
```

---

# Pop Operation

Normal Stack aur Maximum Stack synchronized hain.

So:

```cpp
st.pop();
maxSt.pop();
```

dono se top remove karenge.

Example:

Before:

```text
st    = [5,1,5,3]
maxSt = [5,5,5,5]
```

After `pop()`:

```text
st    = [5,1,5]
maxSt = [5,5,5]
```

---

# Top Operation

Normal Stack ka top:

```cpp
st.top()
```

return karega.

Example:

```text
st = [5,1,5,3]
```

So:

```text
top() = 3
```

---

# peekMax()

Maximum Stack ka top:

```cpp
maxSt.top()
```

current maximum deta hai.

Example:

```text
maxSt = [5,5,5,5]
```

So:

```text
peekMax() = 5
```

Complexity:

```text
O(1)
```

---

# Ab Sabse Important — popMax()

Yahi LC 716 ka main tricky part hai.

Suppose:

```text
st = [5,1,5,3]
```

Maximum:

```text
5
```

Topmost maximum:

```text
[5,1,5,3]
     ↑
     5
```

Hume is `5` ko remove karna hai.

Problem:

> Stack mein direct middle element remove nahi kar sakte.

Hum sirf top se element remove kar sakte hain.

So hume maximum ke upar ke elements temporarily remove karne padenge.

---

# Temporary Stack

```cpp
stack<int> temp;
```

Iska kaam:

> Maximum ke upar jo elements hain unko temporarily save karna.

Initially:

```text
st   = [5,1,5,3]
temp = []
```

---

# Step 1 — Maximum Pata Karo

```cpp
int maxValue = maxSt.top();
```

So:

```text
maxValue = 5
```

---

# Step 2 — Top Se Maximum Tak Jao

Code:

```cpp
while (st.top() != maxValue)
```

Meaning:

> Jab tak Stack ka top maximum nahi hai, top elements ko temporary Stack mein move karo.

---

# First Iteration

Current:

```text
st = [5,1,5,3]
```

Top:

```text
3
```

Check:

```text
3 != 5
```

True.

So `3` ko temp mein bhejo.

```cpp
int value = st.top();

st.pop();
maxSt.pop();

temp.push(value);
```

Now:

```text
st   = [5,1,5]
temp = [3]
```

---

# Second Iteration

Current:

```text
st = [5,1,5]
```

Top:

```text
5
```

Check:

```text
5 != 5
```

False.

Loop stop.

Ab hume **topmost maximum mil gaya**.

---

# Step 3 — Maximum Remove Karo

```cpp
st.pop();
maxSt.pop();
```

Now:

```text
st   = [5,1]
temp = [3]
```

And:

```text
popMax() → 5
```

---

# Step 4 — Temporary Elements Wapas Lao

Ab humne `3` temporarily nikala tha.

Original Stack ko same order mein restore karna hai.

```cpp
while (!temp.empty()) {

    int value = temp.top();
    temp.pop();

    st.push(value);

    if (maxSt.empty()) {
        maxSt.push(value);
    }
    else {
        maxSt.push(max(value, maxSt.top()));
    }
}
```

Temp:

```text
[3]
```

Pop temp:

```text
value = 3
```

Original:

```text
st = [5,1,3]
```

---

# maxSt Ko Bhi Restore Karna Hai

Ye important hai.

Current:

```text
maxSt = [5,5]
```

Ab `3` wapas add karna hai.

Current maximum:

```text
max(3,5)
=
5
```

So:

```text
maxSt = [5,5,5]
```

Ab dono stacks synchronized hain.

---

# Final State

Before:

```text
st    = [5,1,5,3]
maxSt = [5,5,5,5]
```

`popMax()`:

```text
top 3 → temp
top 5 → remove
temp 3 → restore
```

After:

```text
st    = [5,1,3]
maxSt = [5,5,5]
```

Correct.

---

# Complete `popMax()` Code

```cpp
int popMax() {

    int maxValue = maxSt.top();

    stack<int> temp;

    while (st.top() != maxValue) {

        int value = st.top();

        st.pop();
        maxSt.pop();

        temp.push(value);
    }

    // Remove topmost maximum
    st.pop();
    maxSt.pop();

    // Restore temporary elements
    while (!temp.empty()) {

        int value = temp.top();
        temp.pop();

        st.push(value);

        if (maxSt.empty()) {
            maxSt.push(value);
        }
        else {
            maxSt.push(max(value, maxSt.top()));
        }
    }

    return maxValue;
}
```

---

# `popMax()` In 4 Steps

```text
1. Maximum pata karo
        ↓
2. Maximum ke upar ke elements temp mein daalo
        ↓
3. Topmost maximum remove karo
        ↓
4. Temp elements wapas restore karo
```

---

# Complete C++ Code

```cpp
class MaxStack {
public:

    stack<int> st;
    stack<int> maxSt;

    MaxStack() {
        
    }
    
    void push(int x) {

        st.push(x);

        if (maxSt.empty()) {
            maxSt.push(x);
        }
        else {
            maxSt.push(max(x, maxSt.top()));
        }
    }
    
    int pop() {

        int x = st.top();

        st.pop();
        maxSt.pop();

        return x;
    }
    
    int top() {

        return st.top();
    }
    
    int peekMax() {

        return maxSt.top();
    }
    
    int popMax() {

        int maxValue = maxSt.top();

        stack<int> temp;

        while (st.top() != maxValue) {

            int value = st.top();

            st.pop();
            maxSt.pop();

            temp.push(value);
        }

        st.pop();
        maxSt.pop();

        while (!temp.empty()) {

            int value = temp.top();

            temp.pop();

            st.push(value);

            if (maxSt.empty()) {
                maxSt.push(value);
            }
            else {
                maxSt.push(max(value, maxSt.top()));
            }
        }

        return maxValue;
    }
};
```

---

# Detailed Dry Run

Operations:

```text
push(5)
push(1)
push(5)
push(3)
peekMax()
popMax()
top()
peekMax()
```

---

# Initial

```text
st    = []
maxSt = []
```

---

# push(5)

```text
st    = [5]
maxSt = [5]
```

---

# push(1)

```text
max(1,5) = 5
```

```text
st    = [5,1]
maxSt = [5,5]
```

---

# push(5)

```text
max(5,5) = 5
```

```text
st    = [5,1,5]
maxSt = [5,5,5]
```

---

# push(3)

```text
max(3,5) = 5
```

```text
st    = [5,1,5,3]
maxSt = [5,5,5,5]
```

---

# peekMax()

```text
maxSt.top()
```

Output:

```text
5
```

Stack same rahega:

```text
st = [5,1,5,3]
```

---

# popMax()

Maximum:

```text
maxValue = 5
```

Temporary:

```text
temp = []
```

Top:

```text
3
```

`3 != 5`.

Move:

```text
st   = [5,1,5]
temp = [3]
```

Again top:

```text
5
```

Maximum mil gaya.

Remove:

```text
st   = [5,1]
temp = [3]
```

Restore `3`:

```text
st = [5,1,3]
```

Restore max information:

```text
maxSt = [5,5,5]
```

Return:

```text
5
```

---

# top()

```text
st = [5,1,3]
```

Top:

```text
3
```

So:

```text
top() = 3
```

---

# peekMax()

```text
maxSt = [5,5,5]
```

So:

```text
peekMax() = 5
```

---

# Duplicate Maximum Ka Benefit

Example:

```text
[5,1,5,3]
```

Maximum `5` do baar hai.

`popMax()`:

```text
top → 3
```

temporarily hataega.

Then:

```text
5
```

milte hi stop karega.

So **topmost maximum** remove hoga.

Remaining:

```text
[5,1,3]
```

Bottom wala `5` safe rahega.

---

# Why `maxSt.pop()` Also?

Normal Stack:

```text
st
```

aur Maximum Stack:

```text
maxSt
```

same positions represent karte hain.

So jab:

```cpp
st.pop();
```

hoga:

```cpp
maxSt.pop();
```

bhi hona chahiye.

Otherwise dono out of sync ho jayenge.

---

# Why `maxSt` Restore Karna Padta Hai?

`popMax()` mein temporary elements nikaal kar original Stack mein wapas daalte hain.

Example:

```text
st = [5,1]
temp = [3]
```

`3` wapas aaya:

```text
st = [5,1,3]
```

To maximum Stack mein bhi third level create karna padega.

Previous maximum:

```text
5
```

Current:

```text
3
```

So:

```text
max(3,5) = 5
```

Thus:

```text
maxSt = [5,5,5]
```

---

# Why `maxSt.top()` Gives Maximum?

Because every position stores:

```text
maximum from bottom up to this position
```

Example:

```text
st:
[5,1,7,3]
```

maxSt:

```text
[5,5,7,7]
```

So current complete Stack ka maximum:

```text
maxSt.top() = 7
```

---

# Complexity

## push()

```text
O(1)
```

Normal Stack + max Stack mein constant work.

---

## pop()

```text
O(1)
```

Dono stacks ka top pop.

---

## top()

```text
O(1)
```

---

## peekMax()

```text
O(1)
```

`maxSt.top()`.

---

## popMax()

```text
O(n)
```

Worst case mein maximum bottom par ho sakta hai.

Then almost poora Stack temporary Stack mein move karna padega.

---

# Space Complexity

Permanent stacks:

```text
st
maxSt
```

and temporary:

```text
temp
```

Worst case:

```text
O(n)
```

---

# Common Mistakes

## Mistake 1 — `peekMax()` mein normal Stack scan karna

Wrong:

```text
poora Stack scan
```

Correct:

```cpp
maxSt.top()
```

---

## Mistake 2 — `popMax()` mein first maximum remove karna

Topmost maximum remove karna hai.

Isliye:

```cpp
while (st.top() != maxValue)
```

top se move karo.

---

## Mistake 3 — Temporary elements wapas na lana

Wrong:

```text
maximum remove → function end
```

Aisa karne se Stack ke upar ke elements lost ho jaayenge.

Correct:

```text
temp → original Stack
```

restore karo.

---

## Mistake 4 — `maxSt` restore na karna

Agar `st` restore kiya but `maxSt` nahi:

```text
st       = correct
maxSt    = wrong
```

Then `peekMax()` galat answer dega.

---

# Pattern #6 Ka Main Learning

LC 155 mein:

```text
Normal Stack
+
Min Stack
```

LC 716 mein:

```text
Normal Stack
+
Max Stack
+
Temporary Stack
```

So Stack Design pattern mein:

> **Stack ke saath auxiliary data structure maintain karke extra operations ko efficient banana seekhte hain.**

---

# Interview Explanation

> I maintain two synchronized stacks: the normal stack stores values, while the auxiliary max stack stores the maximum value seen up to each position. This makes `peekMax()` O(1). For `popMax()`, I store the current maximum, temporarily pop elements from the top until the topmost maximum is reached, remove that maximum, and then restore the temporary elements while rebuilding the max stack information. This gives O(n) for `popMax()` and O(1) for the other operations.

---

# One-Line Revision

> **Max Stack mein `maxSt.top()` se maximum O(1) mein milta hai, aur `popMax()` mein maximum tak ke top elements `temp` mein shift karke topmost maximum remove karte hain, phir temp ko wapas restore karte hain.**

---

# Pattern #6 Progress

```text
[x] LC 155 — Min Stack
[x] LC 225 — Implement Stack using Queues
[x] LC 716 — Max Stack
```

**Pattern #6 — COMPLETE ✅**
