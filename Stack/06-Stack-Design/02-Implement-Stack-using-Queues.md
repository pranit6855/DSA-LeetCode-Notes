# LeetCode 225 — Implement Stack using Queues

**Pattern:** Stack — Stack Design

---

# Problem

Hume ek **Stack** implement karna hai.

Lekin direct Stack use nahi kar sakte.

Hume sirf:

```text
Queue
```

use karke Stack ke operations implement karne hain.

Required operations:

```text
push(x)
pop()
top()
empty()
```

---

# Stack Ka Behavior

Stack:

```text
LIFO
```

Meaning:

> **Last In, First Out**

Example:

```text
push(1)
push(2)
push(3)
```

Stack:

```text
1
2
3  ← top
```

`pop()`:

```text
3
```

return karega.

---

# Queue Ka Behavior

Queue:

```text
FIFO
```

Meaning:

> **First In, First Out**

Example:

```text
push 1
push 2
push 3
```

Queue:

```text
1  2  3
↑
front
```

Agar normal queue se pop karoge:

```text
1
```

niklega.

Problem:

```text
Stack wants → 3 first
Queue gives  → 1 first
```

Hume Queue ko manipulate karke Stack ka behavior banana hai.

---

# Example

Operations:

```text
push(1)
push(2)
push(3)

top()
pop()
top()
```

Expected:

```text
top() → 3
pop() → 3
top() → 2
```

---

# Main Idea

Hum **sirf 1 Queue** use karenge.

Goal:

> **Queue ke FRONT par hamesha Stack ka TOP element rakho.**

Agar ye achieve ho gaya:

```text
Queue front
     ↓
Stack top
```

Then:

```text
top() = q.front()
pop() = q.pop()
```

easy ho jayega.

---

# Problem With Normal Push

Suppose:

```text
push(1)
```

Queue:

```text
[1]
```

Perfect.

Ab:

```text
push(2)
```

Normal queue mein:

```text
[1,2]
↑
front
```

Lekin Stack ko chahiye:

```text
[2,1]
↑
top
```

So hume new element `2` ko **front par lana hai**.

---

# Core Trick

Push ke time:

```text
1. New element queue mein push karo
2. Purane saare elements ko rotate karo
3. New element front par aa jayega
```

---

# Push(1)

Initially:

```text
q = []
```

Push:

```text
q.push(1)
```

Queue:

```text
[1]
```

Ab:

```text
front = 1
```

So:

```text
Stack top = 1
```

Perfect.

---

# Push(2)

Current Queue:

```text
[1]
```

Pehle:

```text
size = q.size()
```

So:

```text
size = 1
```

Ab `2` push:

```text
[1,2]
```

Problem:

```text
front = 1
```

Stack ko chahiye:

```text
front = 2
```

---

# Rotation

Purane elements ki count:

```text
1
```

So exactly 1 time rotate karenge.

Current:

```text
[1,2]
```

Front nikalo:

```text
1
```

Queue temporarily:

```text
[2]
```

Phir `1` ko back mein daalo:

```text
[2,1]
```

Now:

```text
front = 2
```

Perfect.

---

# Push(3)

Current Queue:

```text
[2,1]
```

Stack top:

```text
2
```

But new `3` ko top banana hai.

First push:

```text
[2,1,3]
```

Old elements count:

```text
size = 2
```

So 2 rotations.

---

## Rotation 1

Current:

```text
[2,1,3]
```

Front:

```text
2
```

Remove `2`:

```text
[1,3]
```

Put `2` at back:

```text
[1,3,2]
```

---

## Rotation 2

Current:

```text
[1,3,2]
```

Front:

```text
1
```

Remove `1`:

```text
[3,2]
```

Put `1` at back:

```text
[3,2,1]
```

Now:

```text
front = 3
```

So:

```text
Stack top = 3
```

---

# Final Queue After Pushes

```text
push(1)
push(2)
push(3)
```

Queue:

```text
[3,2,1]
 ↑
front
```

Ab Queue exactly Stack jaisa behave kar rahi hai.

---

# Why This Works?

Hum har push ke baad ensure karte hain:

```text
NEWEST ELEMENT
       ↓
     FRONT
```

Isliye:

```text
Queue Front = Stack Top
```

Then Stack ka LIFO behavior automatically mil jaata hai.

---

# Push Logic

Code:

```cpp
void push(int x) {

    int size = q.size();

    q.push(x);

    for (int i = 0; i < size; i++) {

        q.push(q.front());
        q.pop();
    }
}
```

---

# Line-by-Line Push Explanation

## Step 1

```cpp
int size = q.size();
```

Push se **pehle** queue mein kitne old elements hain, wo save karo.

Example:

```text
q = [2,1]
```

So:

```text
size = 2
```

Hum exactly 2 old elements rotate karenge.

---

## Step 2

```cpp
q.push(x);
```

New element queue ke back mein daalo.

Example:

```text
Before:
[2,1]

push(3):

[2,1,3]
```

---

## Step 3

```cpp
for (int i = 0; i < size; i++)
```

Purane elements ko exactly `size` times rotate karenge.

Why?

Because:

```text
new element already back par hai
```

Aur hum chahte hain:

```text
new element front par
```

Iske liye saare old elements ko ek-ek karke back mein bhejna padega.

---

# Rotation Ka Meaning

Ye 2 lines:

```cpp
q.push(q.front());
q.pop();
```

actually ek hi kaam karti hain:

> **Queue ke front element ko back mein shift kar do.**

Example:

```text
[2,1,3]
```

Front:

```text
2
```

Push front to back:

```text
[2,1,3,2]
```

Then pop front:

```text
[1,3,2]
```

So:

```text
[2,1,3]
```

became:

```text
[1,3,2]
```

---

# Push(3) Ka Complete Flow

Before:

```text
[2,1]
```

Save:

```text
size = 2
```

Push:

```text
[2,1,3]
```

### First rotation

```text
[1,3,2]
```

### Second rotation

```text
[3,2,1]
```

Done.

---

# Pop Operation

Ab queue:

```text
[3,2,1]
```

Stack top:

```text
3
```

Queue front:

```text
3
```

So simply:

```cpp
q.pop();
```

kar sakte hain.

Code:

```cpp
int pop() {

    int x = q.front();

    q.pop();

    return x;
}
```

---

# Why Pop Is O(1)?

Because Stack ka top already:

```text
Queue ke FRONT par
```

hai.

So:

```cpp
q.pop();
```

directly top remove kar deta hai.

No rotation required.

---

# Top Operation

Stack ka top:

```text
Queue front
```

hai.

So:

```cpp
int top() {

    return q.front();
}
```

Example:

```text
q = [3,2,1]
```

Then:

```text
top() = 3
```

---

# Empty Operation

Queue empty hai ya nahi:

```cpp
bool empty() {

    return q.empty();
}
```

Direct queue ka `empty()` use kar sakte hain.

---

# Complete Algorithm

```text
Start
  ↓
One Queue banao
  ↓
push(x)
  ↓
Old queue ka size save karo
  ↓
x ko queue ke back mein daalo
  ↓
Old elements ko rotate karo
  ↓
x front par aa jayega
  ↓
Queue front = Stack top

pop()
  ↓
Queue front remove

top()
  ↓
Queue front return

empty()
  ↓
Queue empty check
```

---

# Complete C++ Code

```cpp
class MyStack {
public:

    queue<int> q;

    MyStack() {
        
    }
    
    void push(int x) {

        int size = q.size();

        q.push(x);

        for (int i = 0; i < size; i++) {

            q.push(q.front());
            q.pop();
        }
    }
    
    int pop() {

        int x = q.front();

        q.pop();

        return x;
    }
    
    int top() {

        return q.front();
    }
    
    bool empty() {

        return q.empty();
    }
};
```

---

# Detailed Dry Run

Operations:

```text
push(1)
push(2)
push(3)
top()
pop()
top()
empty()
```

Initially:

```text
q = []
```

---

# push(1)

Before push:

```text
q = []
```

Size:

```text
size = 0
```

Push `1`:

```text
q = [1]
```

Loop:

```text
0 times
```

Final:

```text
q = [1]
```

Stack top:

```text
1
```

---

# push(2)

Before:

```text
q = [1]
```

Save:

```text
size = 1
```

Push `2`:

```text
q = [1,2]
```

1 rotation.

### Rotation

Front:

```text
1
```

Move front to back:

```text
[2,1]
```

Final:

```text
q = [2,1]
```

Stack top:

```text
2
```

---

# push(3)

Before:

```text
q = [2,1]
```

Save:

```text
size = 2
```

Push `3`:

```text
q = [2,1,3]
```

### Rotation 1

```text
[1,3,2]
```

### Rotation 2

```text
[3,2,1]
```

Final:

```text
q = [3,2,1]
```

Stack top:

```text
3
```

---

# top()

```cpp
q.front()
```

Queue:

```text
[3,2,1]
 ↑
front
```

Returns:

```text
3
```

---

# pop()

Queue:

```text
[3,2,1]
```

Front:

```text
3
```

Remove:

```text
[2,1]
```

Returns:

```text
3
```

---

# top() Again

Queue:

```text
[2,1]
```

Front:

```text
2
```

Returns:

```text
2
```

---

# empty()

Queue:

```text
[2,1]
```

Not empty.

So:

```text
false
```

---

# Important Insight

Normal Queue:

```text
[1,2,3]
```

gives:

```text
1 first
```

But hum push operation ke baad queue ko arrange kar rahe hain:

```text
[3,2,1]
```

So:

```text
3 first
```

Now Queue behaves like Stack.

---

# Push Is Expensive

Push operation mein old elements rotate karne padte hain.

If queue size:

```text
n
```

hai, then:

```text
n
```

rotations ho sakti hain.

So:

```text
push() = O(n)
```

---

# Other Operations

```text
pop()   = O(1)
top()   = O(1)
empty() = O(1)
```

Because top always front par maintained hai.

---

# Complexity

## push()

```text
O(n)
```

Old elements ko rotate karna padta hai.

## pop()

```text
O(1)
```

## top()

```text
O(1)
```

## empty()

```text
O(1)
```

## Space

```text
O(n)
```

Queue mein maximum `n` elements.

---

# Why We Save `size` Before Push?

Ye important hai:

```cpp
int size = q.size();

q.push(x);
```

`size` ko push se **pehle** lena hai.

Example:

```text
q = [1,2]
```

Old size:

```text
2
```

Push `3`:

```text
[1,2,3]
```

Ab exactly **old 2 elements** rotate karne hain.

Agar push ke baad:

```cpp
int size = q.size();
```

loge, to:

```text
size = 3
```

ho jaayega.

Then new element ko bhi rotate kar doge, jo unnecessary hai.

---

# Common Mistakes

## Mistake 1 — `size` push ke baad lena

Wrong:

```cpp
q.push(x);
int size = q.size();
```

Correct:

```cpp
int size = q.size();
q.push(x);
```

---

## Mistake 2 — All elements rotate na karna

New element ko front par lane ke liye:

```cpp
for (int i = 0; i < size; i++)
```

exactly old elements rotate karne hain.

---

## Mistake 3 — `pop()` mein rotation karna

Hamare design mein:

```text
Queue front = Stack top
```

already maintained hai.

So `pop()`:

```cpp
q.pop();
```

enough hai.

---

## Mistake 4 — `top()` ko rear se lena

Wrong:

```text
queue back
```

Correct:

```cpp
q.front()
```

Because new element har push ke baad front par hota hai.

---

# Interview Explanation

> I implement the stack using a single queue. The key idea is to make the most recently pushed element always stay at the front of the queue. During `push`, I first save the old queue size, push the new element, and then rotate all previous elements from the front to the back. This puts the new element at the front, making the queue behave like a stack. Therefore, `pop()` and `top()` can directly operate on the queue front. Push takes O(n), while pop, top, and empty take O(1).

---

# One-Line Revision

> **Har push ke baad old elements ko rotate karke new element ko Queue ke front par lao; phir Queue ka front hi Stack ka top hai.**

---

# Pattern #6 Progress

```text
[x] LC 155 — Min Stack
[x] LC 225 — Implement Stack using Queues
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
