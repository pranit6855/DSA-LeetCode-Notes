# LeetCode 946 — Validate Stack Sequences

**Pattern:** Stack — Stack Simulation

---

# Problem

Hume do integer arrays diye gaye hain:

```text
pushed
popped
```

`pushed` batata hai ki elements **kis order mein Stack mein push** honge.

`popped` batata hai ki elements **kis order mein pop hone chahiye**.

Hume check karna hai:

> Kya `pushed` ke given order mein elements ko Stack mein push karke `popped` wala exact sequence achieve kiya ja sakta hai?

Agar possible hai:

```text
true
```

Agar possible nahi hai:

```text
false
```

return karna hai.

---

# Example 1

```text
pushed = [1,2,3,4,5]

popped = [4,5,3,2,1]
```

Output:

```text
true
```

Ye sequence possible hai.

---

# Example 2

```text
pushed = [1,2,3]

popped = [3,1,2]
```

Output:

```text
false
```

Ye sequence possible nahi hai.

---

# Stack Rule

Stack ka behavior:

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
[1,2,3]
     ↑
    top
```

Sirf:

```text
3
```

pop kar sakte hain.

`1` ko directly pop nahi kar sakte because `2` aur `3` uske upar honge.

---

# Main Idea

Hum actual Stack ko **simulate** karenge.

Har `pushed` element ko:

```text
Stack mein push
```

karna hai.

Push karne ke baad check karenge:

```text
Kya Stack ka top
abhi popped[j] ke equal hai?
```

Agar haan:

```text
pop karo
j++
```

Phir dobara check karo.

---

# Main Rule

```text
Push current element
        ↓
Stack top == popped[j] ?
        ↓
      YES
        ↓
      Pop
        ↓
      j++
        ↓
Again check
```

Agar Stack top expected element nahi hai:

```text
pop mat karo
```

Aur next element push karo.

---

# Why This Works?

`popped[j]` ka matlab:

> **Abhi mujhe exactly ye element pop karna hai.**

Lekin Stack se sirf top element hi pop ho sakta hai.

So:

```text
st.top() == popped[j]
```

ho to next pop possible hai.

Aur:

```text
st.top() != popped[j]
```

ho to abhi expected element ko pop karna possible nahi hai.

---

# Example 1

```text
pushed = [1,2,3,4,5]

popped = [4,5,3,2,1]
```

Initially:

```text
stack = []
j = 0
```

So expected element:

```text
popped[0] = 4
```

---

# Push 1

```text
stack = [1]
```

Stack top:

```text
1
```

Expected:

```text
4
```

Check:

```text
1 == 4 ❌
```

So `1` ko pop nahi karenge.

---

# Push 2

```text
stack = [1,2]
```

Top:

```text
2
```

Expected:

```text
4
```

Check:

```text
2 == 4 ❌
```

No pop.

---

# Push 3

```text
stack = [1,2,3]
```

Top:

```text
3
```

Expected:

```text
4
```

Check:

```text
3 == 4 ❌
```

No pop.

---

# Push 4

```text
stack = [1,2,3,4]
```

Now:

```text
st.top() = 4

popped[j] = 4
```

Check:

```text
4 == 4 ✅
```

So:

```text
st.pop()
```

Stack:

```text
[1,2,3]
```

And:

```text
j++
```

So:

```text
j = 1
```

Now expected:

```text
popped[1] = 5
```

Stack top:

```text
3
```

Check:

```text
3 == 5 ❌
```

Stop.

---

# Push 5

```text
stack = [1,2,3,5]
```

Top:

```text
5
```

Expected:

```text
popped[1] = 5
```

Match:

```text
5 == 5 ✅
```

Pop:

```text
stack = [1,2,3]
```

`j++`:

```text
j = 2
```

Expected:

```text
popped[2] = 3
```

Stack top:

```text
3
```

Match:

```text
3 == 3 ✅
```

Pop:

```text
stack = [1,2]
```

`j = 3`

Expected:

```text
popped[3] = 2
```

Top:

```text
2
```

Match.

Pop:

```text
stack = [1]
```

`j = 4`

Expected:

```text
popped[4] = 1
```

Top:

```text
1
```

Match.

Pop:

```text
stack = []
```

`j = 5`

---

# Final Check

`popped.size()`:

```text
5
```

And:

```text
j = 5
```

So:

```cpp
j == popped.size()
```

is:

```text
5 == 5
```

true.

Therefore:

```text
Output = true
```

---

# Example 2 — Impossible Sequence

```text
pushed = [1,2,3]

popped = [3,1,2]
```

Initially:

```text
stack = []
j = 0
```

Expected:

```text
3
```

---

# Push 1

```text
stack = [1]
```

Top:

```text
1
```

Expected:

```text
3
```

No pop.

---

# Push 2

```text
stack = [1,2]
```

Top:

```text
2
```

Expected:

```text
3
```

No pop.

---

# Push 3

```text
stack = [1,2,3]
```

Top:

```text
3
```

Expected:

```text
3
```

Match.

Pop:

```text
stack = [1,2]
```

Now:

```text
j = 1
```

Next expected:

```text
popped[1] = 1
```

But Stack:

```text
[1,2]
   ↑
  top
```

Top:

```text
2
```

Expected:

```text
1
```

Check:

```text
2 == 1 ❌
```

So `1` ko pop nahi kar sakte.

Why?

Because Stack mein:

```text
1
2 ← top
```

hai.

Pehle `2` ko pop karna padega.

But `popped` mein next required element `1` hai.

So sequence impossible hai.

---

# How Algorithm Ensures Possibility?

Algorithm koi guess nahi karta.

Ye actual Stack operations simulate karta hai.

Har expected pop ke liye:

```text
popped[j]
```

check hota hai.

Agar wo Stack ke top par hai:

```text
✅ Legal pop
```

Agar wo Stack ke top par nahi hai:

```text
❌ Abhi legal pop nahi ho sakta
```

---

# Why Greedy Pop Is Safe?

Suppose:

```text
st.top() == popped[j]
```

Then current expected element ko abhi pop karna possible hai.

Agar hum use unnecessarily Stack mein rakhenge:

```text
current expected element
```

to baad mein uske upar naye elements push ho sakte hain.

Then hume us element ko pop karne ke liye pehle un naye elements ko remove karna padega.

Isliye:

> **Jab expected element top par mil jaaye, usko immediately pop karna safe hai.**

---

# Why `while`, Not `if`?

Important:

```cpp
while (...)
```

use kiya hai, `if` nahi.

Because ek push ke baad **multiple pops** possible hain.

Example:

```text
pushed = [1,2,3,4]
popped = [4,3,2,1]
```

Push `4` ke baad:

```text
stack = [1,2,3,4]
```

Ab:

```text
4 → pop
3 → pop
2 → pop
1 → pop
```

Ek hi push ke baad multiple elements pop ho gaye.

Isliye:

```cpp
while
```

required hai.

---

# Complete Algorithm

```text
Start
  ↓
Empty Stack banao
  ↓
j = 0
  ↓
pushed ke elements traverse karo
  ↓
Current element Stack mein push karo
  ↓
Kya Stack top == popped[j]?
       ↓
      YES
       ↓
     Pop
       ↓
     j++
       ↓
     Again check
       ↓
      NO
       ↓
   Next element push karo
  ↓
End
  ↓
Kya j == popped.size()?
  ↓
 YES → true
  NO → false
```

---

# Complete C++ Code

```cpp
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {

        // Actual Stack simulate karenge
        stack<int> st;

        // popped[j] = abhi kaunsa element pop hona chahiye
        int j = 0;

        // pushed ke given order mein elements push karo
        for (int i = 0; i < pushed.size(); i++) {

            // Current element ko Stack mein push karo
            st.push(pushed[i]);

            // Jab tak Stack ka top
            // expected popped element ke equal hai,
            // tab tak pop karte raho
            while (!st.empty() &&
                   j < popped.size() &&
                   st.top() == popped[j]) {

                st.pop();

                // Ab popped ka next element expected hai
                j++;
            }
        }

        // Agar poora popped sequence successfully match ho gaya,
        // to sequence valid hai.
        return j == popped.size();
    }
};
```

---

# Code Ka Major Breakdown

## 1. Stack

```cpp
stack<int> st;
```

Actual push/pop simulation ke liye.

---

## 2. `j`

```cpp
int j = 0;
```

`popped` mein current expected element ko track karta hai.

Example:

```text
popped = [4,5,3,2,1]
```

Initially:

```text
j = 0
```

So expected:

```text
4
```

---

## 3. Push

```cpp
st.push(pushed[i]);
```

`pushed` ke order ko exactly follow karna hai.

---

## 4. Match Check

```cpp
st.top() == popped[j]
```

Meaning:

> Kya abhi jo element chahiye, wo Stack ke top par available hai?

---

## 5. Pop

```cpp
st.pop();
```

Agar expected element top par hai to valid Stack operation hai.

---

## 6. `j++`

```cpp
j++;
```

Current expected pop complete ho gaya.

Ab next expected element:

```text
popped[j]
```

hai.

---

## 7. Final Check

```cpp
return j == popped.size();
```

Agar `j` poore `popped` array ko consume kar chuka hai:

```text
j == popped.size()
```

then sequence possible hai.

Otherwise impossible.

---

# Why We Don't Directly Return False Inside While?

Suppose:

```text
st.top() != popped[j]
```

to hum **abhi pop nahi kar sakte**.

But iska matlab automatically ye nahi ki sequence impossible hai.

Maybe:

```text
next pushed element
```

Stack mein aayega aur expected element top par aa jayega.

Example:

```text
pushed = [1,2,3,4]
popped = [4,3,2,1]
```

Jab Stack:

```text
[1,2,3]
```

hai:

```text
top = 3
expected = 4
```

Mismatch hai.

But sequence impossible nahi hai.

`4` abhi push hona baaki hai.

So:

```text
mismatch → wait
```

Next push ke baad fir check karo.

---

# Important Concept

Mismatch ka matlab:

```text
NOT YET POSSIBLE
```

zaroori nahi ki:

```text
IMPOSSIBLE
```

ho.

Isliye hum next element push karte rehte hain.

Final mein agar poora `popped` consume ho gaya:

```text
true
```

warna:

```text
false
```

---

# Complexity

## Time

```text
O(n)
```

Har element:

```text
push → once
pop  → maximum once
```

So total operations linear hain.

---

## Space

```text
O(n)
```

Worst case mein Stack mein saare elements ho sakte hain.

---

# Common Mistakes

## Mistake 1 — `if` instead of `while`

Wrong:

```cpp
if (st.top() == popped[j]) {
    st.pop();
}
```

Correct:

```cpp
while (!st.empty() &&
       j < popped.size() &&
       st.top() == popped[j]) {
    st.pop();
    j++;
}
```

Ek push ke baad multiple pops possible hain.

---

## Mistake 2 — `j++` bhoolna

Wrong:

```cpp
st.pop();
```

Correct:

```cpp
st.pop();
j++;
```

Because current expected element successfully pop ho gaya.

---

## Mistake 3 — `pushed` ke order ko change karna

Push order fixed hai:

```text
pushed[0]
pushed[1]
pushed[2]
...
```

Usi order mein push karna hai.

---

## Mistake 4 — Mismatch par immediately false return karna

Wrong:

```cpp
if (st.top() != popped[j]) {
    return false;
}
```

Because expected element abhi Stack mein push hi na hua ho sakta hai.

Example:

```text
stack = [1,2,3]
expected = 4
```

4 future mein push ho sakta hai.

---

# Pattern Recognition

Agar question mein:

```text
push sequence
pop sequence
validate sequence
Stack operations simulate
```

jaise words aayein:

```text
STACK SIMULATION
```

socho.

---

# Interview Explanation

> I simulate the stack using the given push order. After each push, I repeatedly check whether the stack top matches the next required element in the popped sequence. If it matches, I pop it and move to the next expected element. At the end, if all elements of the popped sequence have been matched, the sequence is valid. The solution takes O(n) time and O(n) space.

---

# One-Line Revision

> **`pushed` ko exactly simulate karo; har push ke baad jab tak `stack.top() == popped[j]` ho, pop karo aur `j++` karo; end mein `j == popped.size()` means sequence valid.**

---

# Pattern #2 Progress

```text
[ ] LC 844 — Backspace String Compare     ⏭️ SKIPPED
[x] LC 946 — Validate Stack Sequences     ✅
[ ] LC 1544 — Make The String Great
```

Pattern #2 mein **sirf 1 must-do question remaining** hai.
```
