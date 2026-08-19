# LeetCode 20 — Valid Parentheses

**Pattern:** Stack — Matching / Pairing

**Filename:** `01-Valid-Parentheses.md`

**Folder:** `Stack/01-Matching-Pairing/`

---

# Problem

Hume ek string `s` di gayi hai jisme ye brackets ho sakte hain:

```text
(
)
{
}
[
]
```

Hume check karna hai ki string ke brackets **valid** hain ya nahi.

Valid ka matlab:

1. Har opening bracket ka corresponding closing bracket hona chahiye.
2. Brackets correct order mein close hone chahiye.
3. Nested brackets properly match hone chahiye.

---

# Examples

### Example 1

```text
s = "()"
```

Output:

```text
true
```

Kyunki:

```text
( → )
```

properly match ho raha hai.

---

### Example 2

```text
s = "()[]{}"
```

Output:

```text
true
```

Saare pairs valid hain.

---

### Example 3

```text
s = "(]"
```

Output:

```text
false
```

Kyunki:

```text
( 
]
```

match nahi karte.

---

### Example 4

```text
s = "([)]"
```

Output:

```text
false
```

Yahan:

```text
(
[
)
]
```

`[` pehle open hua tha, isliye `)` nahi aa sakta.

---

### Example 5

```text
s = "{[]}"
```

Output:

```text
true
```

Ye properly nested hai.

---

# Pattern Identification

Agar question mein ye words dikhein:

```text
balanced
matching brackets
parentheses
nested brackets
opening / closing
properly closed
```

to **Stack** ka thought aana chahiye.

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

Maan lo:

```text
{ [ ( ) ] }
```

Opening brackets:

```text
{
[
(
```

Sabse last opening bracket:

```text
(
```

hai.

Isko sabse pehle close hona chahiye:

```text
)
```

Ye exactly Stack ka LIFO behavior hai.

Isliye nested brackets ke liye Stack perfect hai.

---

# Main Rule

Is problem ka sabse important rule:

```text
Opening bracket → PUSH

Closing bracket → TOP CHECK → POP
```

Matlab:

### Opening

```text
(
[
{
```

Stack mein daalo.

### Closing

```text
)
]
}
```

Aate hi Stack ke top ko check karo.

Agar matching hai:

```text
POP
```

Agar mismatch hai:

```text
INVALID
```

---

# Bracket Matching

Matching pairs:

```text
( → )
[ → ]
{ → }
```

Ye yaad rehna chahiye.

---

# Algorithm

```text
Start
  ↓
Empty stack banao
  ↓
String ke har character ko dekho
  ↓
Opening bracket?
  ├── YES → push
  └── NO  → closing bracket
               ↓
          Stack empty?
          ├── YES → false
          └── NO
               ↓
          Top matching?
          ├── NO → false
          └── YES → pop
  ↓
Loop complete
  ↓
Stack empty?
  ├── YES → true
  └── NO → false
```

---

# Step-by-Step Approach

## Step 1 — Stack Create Karo

```cpp
stack<char> st;
```

Hum `char` stack use karenge kyunki brackets characters hain.

---

# Step 2 — String Traverse Karo

```cpp
for(char ch : s)
```

Har character ek-ek karke check karenge.

---

# Step 3 — Opening Bracket

Agar:

```cpp
ch == '('
```

ya:

```cpp
ch == '['
```

ya:

```cpp
ch == '{'
```

hai, to:

```cpp
st.push(ch);
```

Example:

```text
s = "{["
```

Process:

```text
{ → push
[ → push
```

Stack:

```text
[
{
```

---

# Step 4 — Closing Bracket

Agar opening nahi hai, to current character closing bracket hai.

Sabse pehle check:

```cpp
if(st.empty())
```

Kyun?

Kyuki closing bracket ko match karne ke liye stack mein corresponding opening bracket hona zaroori hai.

---

# Edge Case 1 — Stack Empty

Example:

```text
s = ")"
```

Initial:

```text
stack = []
```

Current:

```text
)
```

Koi opening bracket hi nahi hai.

Isliye:

```cpp
if(st.empty()){
    return false;
}
```

Answer:

```text
false
```

### Rule

> Closing bracket aaya + stack empty hai = invalid.

---

# Step 5 — Top Match Check

Suppose:

```text
s = "([)"
```

Process:

```text
( → push
[ → push
```

Stack top:

```text
[
```

Ab:

```text
)
```

aaya.

`)'` ko match karna chahiye:

```text
(
```

Lekin top:

```text
[
```

hai.

Mismatch.

Therefore:

```cpp
return false;
```

### Rule

> Closing bracket ko hamesha Stack ke top wale opening bracket ke saath match karna hai.

---

# Step 6 — Matching Ho To Pop

Agar:

```text
)
```

aur top:

```text
(
```

hai:

```cpp
st.pop();
```

Similarly:

```text
] → [
} → {
```

match hone par pop.

---

# Step 7 — Final Check

Loop complete hone ke baad:

```cpp
return st.empty();
```

Ye bahut important hai.

Kyunki possible hai ki saare characters process ho gaye ho, lekin kuch opening brackets abhi bhi stack mein bache ho.

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

End:

```text
stack = [(, (, (]
```

Empty nahi hai.

Matlab brackets close nahi hue.

So:

```text
st.empty() = false
```

Therefore:

```text
return false
```

---

# Why `return st.empty()`?

Ye line basically pooch rahi hai:

> "Kya loop ke baad saare opening brackets successfully close ho chuke hain?"

### Empty

```text
stack = []
```

→ saare brackets close ho gaye

→ `true`

### Non-empty

```text
stack = [(]
```

→ kuch brackets open reh gaye

→ `false`

---

# Detailed Dry Run 1 — Valid

Input:

```text
s = "{[]}"
```

Initial:

```text
stack = []
```

---

## Character 1 — `{`

Opening bracket.

```cpp
st.push('{');
```

Stack:

```text
{
```

---

## Character 2 — `[`

Opening bracket.

```cpp
st.push('[');
```

Stack:

```text
[
{
```

Top:

```text
[
```

---

## Character 3 — `]`

Closing bracket.

Stack empty?

```text
No
```

Top:

```text
[
```

Current:

```text
]
```

Match:

```text
[ → ]
```

So:

```cpp
st.pop();
```

Stack:

```text
{
```

---

## Character 4 — `}`

Closing bracket.

Stack empty?

```text
No
```

Top:

```text
{
```

Current:

```text
}
```

Match:

```text
{ → }
```

Pop:

```cpp
st.pop();
```

Stack:

```text
[]
```

---

## End

```cpp
return st.empty();
```

Stack empty hai:

```text
true
```

Final:

```text
Answer = true
```

---

# Detailed Dry Run 2 — Invalid

Input:

```text
s = "([)]"
```

Initial:

```text
stack = []
```

---

## `(`

Opening:

```text
push
```

Stack:

```text
(
```

---

## `[`

Opening:

```text
push
```

Stack:

```text
[
(
```

---

## `)`

Closing.

Top:

```text
[
```

Expected:

```text
(
```

Mismatch.

Therefore:

```cpp
return false;
```

Answer:

```text
false
```

---

# Detailed Dry Run 3 — Unclosed Bracket

Input:

```text
s = "((("
```

Process:

```text
( → push
( → push
( → push
```

Final stack:

```text
(
(
(
```

Loop ends.

Now:

```cpp
return st.empty();
```

Result:

```text
false
```

---

# Detailed Dry Run 4 — Closing Bracket First

Input:

```text
s = "]"
```

Initial:

```text
stack = []
```

Current:

```text
]
```

Stack empty:

```cpp
if(st.empty())
    return false;
```

Answer:

```text
false
```

---

# Complete C++ Code

```cpp
class Solution {
public:
    bool isValid(string str) {

        stack<char> st;

        for(char ch : str) {

            // Opening brackets
            if(ch == '(' || ch == '{' || ch == '[') {
                st.push(ch);
            }

            // Closing brackets
            else {

                // No opening bracket available
                if(st.empty()) {
                    return false;
                }

                // Wrong matching
                if(ch == ')' && st.top() != '(') {
                    return false;
                }

                if(ch == '}' && st.top() != '{') {
                    return false;
                }

                if(ch == ']' && st.top() != '[') {
                    return false;
                }

                // Correct pair matched
                st.pop();
            }
        }

        // All brackets should be matched
        return st.empty();
    }
};
```

---

# Code Ka Core Logic

```text
Opening:
    push

Closing:
    if stack empty → false
    if top mismatch → false
    else → pop

End:
    stack empty → true
    otherwise → false
```

---

# Common Mistake

### `top` vs `top()`

Wrong:

```cpp
st.top
```

Correct:

```cpp
st.top()
```

`top()` ek function hai jo Stack ka top element return karta hai.

---

# Why Not Just Count Brackets?

Sirf opening aur closing count karna enough nahi hai.

Example:

```text
([)]
```

Counts:

```text
2 opening
2 closing
```

Phir bhi invalid.

Kyun?

Order galat hai.

Actual Stack check:

```text
(
[
)
```

`)'` ko `[` close nahi kar sakta.

Isliye Stack **order + nesting** dono handle karta hai.

---

# Pattern Recognition

Agar question mein:

```text
Balanced
Matching
Parentheses
Brackets
Nested
Opening / Closing
```

dikhe, to:

```text
STACK
```

socho.

Mental template:

```text
OPEN  → PUSH
CLOSE → TOP MATCH → POP
```

---

# Complexity

Let:

```text
n = string length
```

Har character maximum:

```text
1 push
1 pop
```

kar sakta hai.

### Time Complexity

```text
O(n)
```

### Space Complexity

Worst case mein saare characters opening brackets ho sakte hain:

```text
(((((((
```

So:

```text
O(n)
```

---

# Interview Explanation

Agar interviewer puche:

> Why did you use Stack?

Answer:

> Brackets nested structure follow karte hain aur jo opening bracket sabse last mein aata hai, wahi sabse pehle close hota hai. Ye LIFO behavior hai, jo Stack provide karta hai. Main opening brackets ko push karta hoon aur closing bracket par stack ke top se matching check karke pop karta hoon. Agar mismatch ho ya stack empty ho to string invalid hai. End mein stack empty hona chahiye.

---

# Pattern Summary

## Pattern Name

```text
Matching / Pairing
```

## Data Structure

```text
Stack
```

## Core Rule

```text
Opening → Push
Closing → Match Top → Pop
```

## Invalid Cases

```text
1. Closing bracket + empty stack
2. Top bracket mismatch
3. End mein stack non-empty
```

## Complexity

```text
Time  : O(n)
Space : O(n)
```

---

# One-Line Revision

> **"Bracket matching mein opening brackets push karo, closing bracket par top match karo, match ho to pop karo, aur end mein stack empty hona chahiye."**

---

## LeetCode

**Problem:** 20 — Valid Parentheses

**Difficulty:** Easy

**Pattern:** Stack — Matching / Pairing

**Stack Folder:** `Stack/01-Matching-Pairing/`

**File:** `Stack/01-Matching-Pairing/01-Valid-Parentheses.md`
