# LeetCode 921 — Minimum Add to Make Parentheses Valid

**Pattern:** Stack — Matching / Pairing

**Filename:** `02-Minimum-Add-to-Make-Parentheses-Valid.md`

**Folder:** `Stack/01-Matching-Pairing/`

---

# Problem

Hume ek string `s` di gayi hai jisme sirf:

```text
(
)
```

hote hain.

Hume minimum number of parentheses **add** karne hain taaki string valid parentheses string ban jaye.

### Valid Parentheses ka matlab

1. Har opening `(` ka corresponding closing `)` hona chahiye.
2. Har closing `)` se pehle uska matching `(` available hona chahiye.
3. Parentheses proper order mein hone chahiye.

---

# Examples

## Example 1

```text
Input:
s = "())"

Output:
1
```

Explanation:

```text
())
```

Last `)` ke liye koi unmatched `(` available nahi hai.

Ek `(` add karna padega.

```text
()()
```

Minimum additions:

```text
1
```

---

## Example 2

```text
Input:
s = "((("

Output:
3
```

Teen opening brackets hain:

```text
(((
```

Har opening bracket ke liye ek closing bracket chahiye:

```text
((()))
```

Isliye answer:

```text
3
```

---

## Example 3

```text
Input:
s = "())("

Output:
2
```

Yahan:

```text
())
```

mein ek extra `)` hai.

Aur end mein:

```text
(
```

unmatched hai.

Therefore:

```text
1 + 1 = 2
```

---

# Pattern Identification

Agar question mein ye words dikhein:

```text
parentheses
matching
balanced
opening / closing
valid parentheses
nested
brackets
```

to Stack ka thought aana chahiye.

Ye Stack ka:

```text
Matching / Pairing Pattern
```

hai.

---

# Why Stack?

Stack ka rule hai:

```text
LIFO
Last In First Out
```

Parentheses mein jo opening bracket sabse last aata hai, wahi pehle close hona chahiye.

Example:

```text
((()))
```

Opening brackets:

```text
(
(
(
```

Sabse last wala `(` sabse pehle `)` se match karega.

Ye exactly Stack ka LIFO behavior hai.

Isliye Stack naturally matching problem ke liye useful hai.

---

# Main Idea

Hum Stack mein **unmatched opening brackets** store karenge.

### Opening bracket mile

```text
(
```

to:

```text
PUSH
```

### Closing bracket mile

```text
)
```

to:

* Agar Stack empty hai → matching `(` nahi hai → ek `(` add karna padega.
* Agar Stack empty nahi hai → ek `(` available hai → `POP`.

### End mein

Agar Stack mein kuch opening brackets bach gaye hain, to har ek ke liye ek closing `)` add karna padega.

---

# Algorithm

```text
Start
  ↓
Empty Stack banao
  ↓
ans = 0
  ↓
String ke har character ko traverse karo
  ↓
Current character '(' hai?
  ├── YES → push
  └── NO  → ')'
             ↓
        Stack empty?
        ├── YES → ans++
        └── NO  → pop
  ↓
Loop complete
  ↓
Remaining '(' = stack.size()
  ↓
ans += stack.size()
  ↓
Return ans
```

---

# Step-by-Step Approach

## Step 1 — Stack Create Karo

```cpp
stack<char> st;
```

Stack mein unmatched `(` store honge.

---

## Step 2 — Answer Variable

```cpp
int ans = 0;
```

Ye count karega ki kitne parentheses add karne hain.

---

## Step 3 — String Traverse Karo

```cpp
for(char ch : s)
```

Har character ko ek-ek karke process karenge.

---

# Case 1 — Opening Bracket

Agar:

```cpp
ch == '('
```

to:

```cpp
st.push(ch);
```

Example:

```text
s = "((("
```

Process:

```text
( → push
( → push
( → push
```

Stack:

```text
(
(
(
```

---

# Case 2 — Closing Bracket

Agar:

```text
ch == ')'
```

to do cases honge.

---

## Case 2A — Stack Empty

Example:

```text
s = ")"
```

Initial:

```text
stack = []
ans = 0
```

Current character:

```text
)
```

Koi opening bracket available nahi hai.

Isliye hume ek `(` add karna padega.

```cpp
ans++;
```

Now:

```text
ans = 1
```

Important:

Yahan Stack mein kuch push nahi karna.

Reason:

Jo `(` hum add kar rahe hain, woh isi `)` ko immediately match karega.

---

## Case 2B — Stack Empty Nahi Hai

Example:

```text
s = "()"
```

Pehle:

```text
(
```

Stack:

```text
(
```

Ab:

```text
)
```

aaya.

Ek unmatched opening bracket available hai.

So:

```cpp
st.pop();
```

Pair complete ho gaya:

```text
()
```

---

# Important Rule

Closing bracket ke time:

```text
Stack empty
    ↓
No matching '('
    ↓
ans++
```

Aur:

```text
Stack non-empty
    ↓
Matching '(' available
    ↓
pop()
```

---

# Final Step — Remaining Opening Brackets

Suppose:

```text
s = "((("
```

Loop complete hone ke baad:

```text
stack.size() = 3
```

Ye 3 opening brackets unmatched hain.

Har ek ke liye ek `)` add karna padega.

Therefore:

```cpp
ans += st.size();
```

---

# Complete C++ Code

```cpp
class Solution {
public:
    int minAddToMakeValid(string s) {
        stack<char> st;
        int ans = 0;

        for(char ch : s) {

            // Opening bracket
            if(ch == '(') {
                st.push(ch);
            }

            // Closing bracket
            else {
                // No opening bracket available
                if(st.empty()) {
                    ans++;
                }

                // Matching opening bracket available
                else {
                    st.pop();
                }
            }
        }

        // Remaining unmatched '('
        ans += st.size();

        return ans;
    }
};
```

---

# Code Explanation

## Stack

```cpp
stack<char> st;
```

Unmatched opening parentheses store karne ke liye.

---

## Answer

```cpp
int ans = 0;
```

Minimum required additions ko store karta hai.

---

## Opening bracket

```cpp
if(ch == '(') {
    st.push(ch);
}
```

Opening bracket ko future matching ke liye Stack mein save kar lo.

---

## Closing bracket

```cpp
else {
```

Yahan `ch == ')'`.

---

## Stack Empty

```cpp
if(st.empty()) {
    ans++;
}
```

Koi opening bracket available nahi hai.

Therefore ek `(` add karna padega.

---

## Stack Non-Empty

```cpp
else {
    st.pop();
}
```

Ek unmatched `(` mil gaya.

Current `)` usko match karega.

---

## Remaining Opening Brackets

```cpp
ans += st.size();
```

End mein Stack mein jo `(` bache hain unke liye `)` add karne padenge.

---

# Detailed Dry Run

## Input

```text
s = "())("
```

Initial state:

```text
stack = []
ans = 0
```

---

## Character 1 — `(`

Opening bracket.

```text
push
```

Stack:

```text
(
```

Answer:

```text
0
```

---

## Character 2 — `)`

Stack empty nahi hai.

Therefore:

```text
pop
```

Stack:

```text
[]
```

Answer:

```text
0
```

Valid pair:

```text
()
```

---

## Character 3 — `)`

Stack empty hai.

Therefore:

```text
ans++
```

Now:

```text
ans = 1
```

We need to add one `(`.

---

## Character 4 — `(`

Opening bracket.

```text
push
```

Stack:

```text
(
```

Answer:

```text
1
```

---

# Loop Complete

Ab:

```text
stack.size() = 1
```

Ek unmatched `(` bacha hua hai.

Therefore ek `)` add karna padega.

```cpp
ans += st.size();
```

So:

```text
ans = 1 + 1
ans = 2
```

Final answer:

```text
2
```

---

# Another Dry Run

## Input

```text
s = "()))"
```

Initial:

```text
stack = []
ans = 0
```

### First character — `(`

```text
push
```

Stack:

```text
(
```

### Second character — `)`

Stack non-empty:

```text
pop
```

Stack:

```text
[]
```

### Third character — `)`

Stack empty:

```text
ans++
```

Now:

```text
ans = 1
```

### Fourth character — `)`

Stack empty:

```text
ans++
```

Now:

```text
ans = 2
```

End:

```text
stack.size() = 0
```

Final:

```text
2
```

---

# Edge Cases

## 1. Already Valid

```text
s = "()"
```

Process:

```text
( → push
) → pop
```

End:

```text
stack = []
ans = 0
```

Answer:

```text
0
```

---

## 2. Only Opening Brackets

```text
s = "((("
```

End:

```text
stack.size() = 3
```

Answer:

```text
3
```

---

## 3. Only Closing Brackets

```text
s = ")))"
```

Har `)` par Stack empty hai.

Therefore:

```text
ans = 3
```

---

## 4. Empty String

```text
s = ""
```

Nothing to add.

Answer:

```text
0
```

---

# Common Mistake

Ye code galat hai:

```cpp
if(st.empty()) {
    ans++;
}

st.pop();
```

Problem:

Agar Stack empty hua to `ans++` ke baad bhi `st.pop()` execute hoga.

Empty Stack par `pop()` nahi karna chahiye.

Correct:

```cpp
if(st.empty()) {
    ans++;
}
else {
    st.pop();
}
```

---

# Why Are We Not Pushing the Added `(`?

Suppose current character:

```text
)
```

hai aur Stack empty hai.

Hum bolte hain:

```text
ans++;
```

Matlab logically ek `(` insert kar diya.

Example:

```text
)
```

becomes:

```text
()
```

Added `(` isi `)` ko match kar raha hai.

Isliye usko Stack mein push karke future ke liye rakhne ki zarurat nahi hai.

---

# Why Do We Add `stack.size()` at the End?

Suppose:

```text
s = "((("
```

Sabhi opening brackets Stack mein bach jayenge:

```text
(
(
(
```

Har unmatched `(` ke liye exactly ek `)` chahiye.

Therefore:

```text
number of required ')' = stack.size()
```

So:

```cpp
ans += st.size();
```

---

# Why Is This Minimum?

Har unmatched closing `)` ke liye minimum ek `(` add karna compulsory hai.

Aur end mein har unmatched opening `(` ke liye minimum ek `)` add karna compulsory hai.

Hum exactly itne hi additions kar rahe hain.

Therefore answer minimum hai.

---

# Stack Pattern Connection

Is problem mein main thought:

```text
Opening bracket
      ↓
    PUSH
      ↓
Closing bracket
      ↓
Matching opening available?
      ↓
 ┌───────────────┐
 YES             NO
 ↓               ↓
POP             ans++
```

End:

```text
Remaining '('
      ↓
Need ')'
      ↓
ans += stack.size()
```

---

# Optimized Approach

Is problem ko stack ki zarurat ke bina bhi solve kiya ja sakta hai.

Hum sirf unmatched `(` ka count maintain kar sakte hain.

```cpp
class Solution {
public:
    int minAddToMakeValid(string s) {
        int open = 0;
        int ans = 0;

        for(char ch : s) {

            if(ch == '(') {
                open++;
            }

            else {
                if(open > 0) {
                    open--;
                }
                else {
                    ans++;
                }
            }
        }

        return ans + open;
    }
};
```

---

# Optimized Approach Explanation

`open` ka matlab:

```text
Currently kitne unmatched '(' hain
```

### `(` mile

```cpp
open++;
```

### `)` mile aur `open > 0`

Ek opening bracket available hai:

```cpp
open--;
```

### `)` mile aur `open == 0`

Koi opening bracket available nahi hai:

```cpp
ans++;
```

### End mein

Jo opening brackets remaining hain:

```text
open
```

unke liye same number of `)` chahiye.

Therefore:

```cpp
return ans + open;
```

---

# Complexity

## Stack Solution

Let:

```text
n = length of string
```

### Time Complexity

```text
O(n)
```

Har character exactly once process hota hai.

### Space Complexity

```text
O(n)
```

Worst case:

```text
((((((
```

mein Stack mein `n` elements ho sakte hain.

---

# Optimized Solution Complexity

### Time

```text
O(n)
```

### Space

```text
O(1)
```

Sirf counters use ho rahe hain.

---

# Interview Explanation

Agar interviewer puche:

> Explain your approach.

Tum bol sakte ho:

> I use a stack to keep track of unmatched opening parentheses. Whenever I see `(`, I push it into the stack. When I see `)`, if the stack is empty, there is no opening parenthesis available, so I add one and increment the answer. Otherwise, I pop one opening parenthesis because it forms a valid pair. After processing the complete string, any opening parentheses left in the stack need corresponding closing parentheses, so I add the stack size to the answer. The time complexity is O(n) and the space complexity is O(n).

---

# Pattern Recognition

Aage kisi question mein agar:

```text
Matching
Balanced
Parentheses
Opening / Closing
Nested
Valid brackets
```

dikhe, to pehle Stack sochna.

Mental template:

```text
OPEN
 ↓
PUSH

CLOSE
 ↓
CHECK
 ↓
MATCH → POP
NO MATCH → HANDLE INVALID CASE
```

---

# Important Learning

Is question se 3 important cheezein yaad rakho:

### 1. Stack empty hone par blindly pop nahi karna

```cpp
if(st.empty())
```

pehle check karo.

### 2. Remaining opening brackets bhi count karne hain

```cpp
ans += st.size();
```

### 3. Matching problem mein LIFO behavior important hai

Last opened bracket pehle close hota hai.

---

# One-Line Revision

> **`(` ko Stack mein push karo, `)` ke liye available `(` ko pop karo, unmatched `)` ke liye `ans++` karo, aur end mein remaining `(` ke liye `ans += stack.size()` karo.**

---

# Stack Roadmap Progress

## Pattern #1 — Matching / Pairing

* [x] LeetCode 20 — Valid Parentheses
* [x] LeetCode 921 — Minimum Add to Make Parentheses Valid

## Current Pattern

```text
Stack/01-Matching-Pairing/
```

## Next

* [ ] Pattern #1 ka next question

---

# Stack Folder Structure

```text
Stack/
│
├── 01-Matching-Pairing/
│   ├── 01-Valid-Parentheses.md
│   └── 02-Minimum-Add-to-Make-Parentheses-Valid.md
│
├── 02-Stack-Simulation/
│
├── 03-Monotonic-Stack/
│
├── 04-Next-Previous-Greater-Smaller/
│
├── 05-Boundary-Contribution/
│
└── 06-Stack-Design/
```
