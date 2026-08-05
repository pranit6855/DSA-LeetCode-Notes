# LeetCode 2486 - Append Characters to String to Make Subsequence

## 📌 Problem

Hume do strings di gayi hain:

```text
s
t
```

Hume `s` ke **end me minimum characters append** karne hain taaki `t`, `s` ki **subsequence** ban jaye.

Hume return karna hai:

```text
Minimum number of characters jo s ke end me add karne padenge.
```

---

# ❓ Subsequence Kya Hoti Hai?

Subsequence me characters ka **order same rehna chahiye**, lekin unke beech me extra characters ho sakte hain.

Example:

```text
s = "abcde"
```

Iski subsequence:

```text
"ace" ✅
```

Because:

```text
a → c → e
```

same order me present hain.

Ye zaroori nahi ki characters continuous ho.

---

## Example

```text
s = "abcde"
t = "acefg"
```

`s` ke andar:

```text
a
c
e
```

same order me already present hain.

Matlab `t` ka:

```text
"ace"
```

part already match ho gaya.

Lekin:

```text
"fg"
```

remaining hai.

So `s` ke end me:

```text
"fg"
```

append karna padega.

Final:

```text
s = "abcdefg"
```

Ab:

```text
t = "acefg"
```

iski subsequence hai.

So:

```text
Output = 2
```

---

# 💡 Approach - Two Pointers

Hum do pointers use karenge:

```text
i → string s ko traverse karega

j → string t ko traverse karega
```

Starting:

```cpp
int i = 0;
int j = 0;
```

Matlab dono strings ke first character se start karenge.

---

# 🧠 Main Idea

Hum `s` ke andar ye find karna chahte hain ki `t` ka **kitna starting portion already subsequence hai**.

Example:

```text
s = "abcde"
t = "acefg"
```

`s` me:

```text
a → c → e
```

mil raha hai.

So `t` ka:

```text
"ace"
```

already match ho gaya.

Remaining:

```text
"fg"
```

ko append karna padega.

---

# 🔹 Pointer Meaning

Consider:

```text
s = "abcde"
t = "acefg"
```

Starting:

```text
s = a b c d e
    ↑
    i

t = a c e f g
    ↑
    j
```

`i`:

```text
s me current character check karega
```

`j`:

```text
t ka current required character batayega
```

---

# 🔍 Main Loop

Code:

```cpp
while(i < s.size() && j < t.size())
```

Loop tab tak chalega jab tak:

```text
s me characters bache hain
```

AND

```text
t me characters bache hain
```

---

# 🔹 Character Match

Hum check karte hain:

```cpp
if(s[i] == t[j])
```

Agar dono characters same hain, matlab `t` ka current required character `s` me mil gaya.

So:

```cpp
j++;
```

kar denge.

---

# ❓ Match Hone Par `j++` Kyu?

Suppose:

```text
t = "acefg"
```

Aur currently:

```text
t[j] = 'a'
```

Hume `s` me `'a'` mil gaya.

Ab `'a'` dobara search nahi karna.

Next required character:

```text
'c'
```

hai.

Isliye:

```cpp
j++;
```

---

# 🔹 `i++` Har Baar Kyu Hota Hai?

Code:

```cpp
i++;
```

`if` ke bahar hai.

Matlab:

```text
match hua → i++

match nahi hua → tab bhi i++
```

Reason:

`i` ka kaam `s` ko traverse karna hai.

Current `s[i]` check ho chuka hai.

Ab hume `s` ka next character check karna hai.

---

# 🔥 Match vs No Match

## Match Hua

Agar:

```text
s[i] == t[j]
```

to:

```cpp
j++;
i++;
```

Dono move karenge.

Because:

```text
s ka current character process ho gaya

AND

t ka current character bhi mil gaya
```

---

## Match Nahi Hua

Agar:

```text
s[i] != t[j]
```

to:

```text
j same rahega
```

because hume abhi bhi `t` ka wahi character chahiye.

Lekin:

```cpp
i++;
```

because `s` me next character search karna hai.

---

# 🔄 Complete Dry Run

Consider:

```text
s = "abcde"
t = "acefg"
```

Starting:

```text
i = 0
j = 0
```

---

## Step 1

```text
s = a b c d e
    ↑
    i

t = a c e f g
    ↑
    j
```

Compare:

```text
s[i] = 'a'
t[j] = 'a'
```

Match:

```text
'a' == 'a' ✅
```

So:

```cpp
j++;
```

Now:

```text
j = 1
```

Then:

```cpp
i++;
```

Now:

```text
i = 1
```

---

## Step 2

Pointers:

```text
s = a b c d e
      ↑
      i

t = a c e f g
      ↑
      j
```

Current:

```text
s[i] = 'b'
t[j] = 'c'
```

Compare:

```text
'b' != 'c' ❌
```

Match nahi hua.

So `j` ko move nahi karenge.

```text
j = 1
```

same rahega.

Lekin `s` ka current `'b'` useless hai.

So:

```cpp
i++;
```

Now:

```text
i = 2
```

---

## Step 3

Pointers:

```text
s = a b c d e
        ↑
        i

t = a c e f g
      ↑
      j
```

Current:

```text
s[i] = 'c'
t[j] = 'c'
```

Match:

```text
'c' == 'c' ✅
```

So:

```cpp
j++;
```

Now:

```text
j = 2
```

Then:

```cpp
i++;
```

Now:

```text
i = 3
```

---

## Step 4

Pointers:

```text
s = a b c d e
          ↑
          i

t = a c e f g
        ↑
        j
```

Current:

```text
s[i] = 'd'
t[j] = 'e'
```

Compare:

```text
'd' != 'e' ❌
```

So:

```text
j same
```

and:

```cpp
i++;
```

Now:

```text
i = 4
```

---

## Step 5

Pointers:

```text
s = a b c d e
            ↑
            i

t = a c e f g
        ↑
        j
```

Current:

```text
s[i] = 'e'
t[j] = 'e'
```

Match:

```text
'e' == 'e' ✅
```

So:

```cpp
j++;
```

Now:

```text
j = 3
```

And:

```cpp
i++;
```

Now:

```text
i = 5
```

---

# 🛑 Loop Stop

`s` ka size:

```text
5
```

Aur:

```text
i = 5
```

Condition:

```cpp
i < s.size()
```

becomes:

```text
5 < 5 ❌
```

So loop stop.

---

# 🧠 Ab `j = 3` Ka Meaning Kya Hai?

Ye question ka sabse important point hai.

`t`:

```text
a c e f g
0 1 2 3 4
```

Current:

```text
j = 3
```

Iska matlab `t` ke first **3 characters successfully match ho chuke hain**:

```text
a
c
e
```

Matched part:

```text
"ace"
```

Remaining:

```text
"fg"
```

---

# 🔥 Final Formula

Code:

```cpp
return t.size() - j;
```

`t` ka size:

```text
5
```

Matched characters:

```text
j = 3
```

Remaining:

```text
5 - 3 = 2
```

So:

```text
Answer = 2
```

---

# ❓ `t.size() - j` Kyu?

Simple:

```text
Total characters in t
-
Already matched characters
=
Characters still required
```

Mathematically:

```text
Remaining = Total - Matched
```

Code:

```cpp
t.size() - j
```

---

# 🔹 Example

```text
t = "acefg"
```

Total:

```text
5
```

Matched:

```text
"ace"
```

Number of matched characters:

```text
3
```

Remaining:

```text
"fg"
```

Number:

```text
2
```

Formula:

```text
5 - 3 = 2
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int appendCharacters(string s, string t) {
        int i = 0;
        int j = 0;

        while(i < s.size() && j < t.size()) {

            if(s[i] == t[j]) {
                j++;
            }

            i++;
        }

        return t.size() - j;
    }
};
```

---

# 🧠 Code Explanation

## 1. First Pointer

```cpp
int i = 0;
```

`i` string `s` ko traverse karega.

Meaning:

```text
s me kaha search kar rahe hain
```

---

## 2. Second Pointer

```cpp
int j = 0;
```

`j` string `t` ke current required character ko point karega.

Saath hi `j` ye bhi batata hai ki:

```text
t ke kitne characters successfully match ho chuke hain
```

---

## 3. Main Loop

```cpp
while(i < s.size() && j < t.size())
```

Jab tak dono strings me characters available hain, matching continue karenge.

---

## 4. Characters Compare

```cpp
if(s[i] == t[j])
```

Agar current characters same hain:

```text
t ka ek required character mil gaya
```

So:

```cpp
j++;
```

---

## 5. `i++`

```cpp
i++;
```

Ye `if` ke bahar hai.

Isliye `i` har iteration me move karega.

Reason:

```text
s ka current character check ho chuka hai.
```

Chahe match hua ho ya nahi, ab next character check karna hai.

---

# ❓ `j++` Har Baar Kyu Nahi?

Because `j` sirf tab move karega jab required character mil jaye.

Example:

```text
s = "abcde"
t = "acefg"
```

At:

```text
s[i] = 'b'
t[j] = 'c'
```

Match nahi hua.

Agar hum `j++` kar dete, to next required character:

```text
'e'
```

ho jata.

Lekin `'c'` to abhi mila hi nahi.

Isliye:

```text
No match → j same
```

---

# ❓ `i++` If Ke Bahar Kyu?

Ye bhi important hai.

Code:

```cpp
if(s[i] == t[j]) {
    j++;
}

i++;
```

Suppose:

```text
s[i] = 'b'
t[j] = 'c'
```

Match nahi hua.

Ab `'b'` ko dobara check karne ka koi fayda nahi.

Hume `s` me aage jaake `'c'` search karna hai.

So:

```cpp
i++;
```

har case me hona chahiye.

---

# 🔥 Pointer Movement Summary

### Match

```text
s[i] == t[j]
```

Then:

```text
j++
i++
```

---

### No Match

```text
s[i] != t[j]
```

Then:

```text
sirf i++
```

`j` wahi rahega.

---

# 🔹 Another Case - Agar Pura `t` Match Ho Jaye

Suppose:

```text
s = "abcdef"
t = "ace"
```

Matching:

```text
a ✅
c ✅
e ✅
```

Then:

```text
j = 3
```

And:

```text
t.size() = 3
```

Final:

```cpp
return t.size() - j;
```

```text
= 3 - 3
= 0
```

So:

```text
Answer = 0
```

Kuch append karne ki zarurat nahi.

Because `t` already `s` ki subsequence hai.

---

# 🔹 Agar Kuch Bhi Match Na Ho

Suppose:

```text
s = "abc"
t = "xyz"
```

Koi character match nahi karega.

So:

```text
j = 0
```

Final:

```text
t.size() - j

= 3 - 0

= 3
```

Hume complete:

```text
"xyz"
```

append karna padega.

So answer:

```text
3
```

---

# ❓ End Me Hi Characters Append Kyu Kar Sakte Hain?

Problem specifically bolti hai ki characters:

```text
s ke END me
```

append karne hain.

Isliye hume pehle check karna hai ki `t` ka maximum starting portion `s` me subsequence ke form me already present hai.

Example:

```text
t = "acefg"
```

Agar:

```text
"ace"
```

already mil gaya, to remaining:

```text
"fg"
```

end me append kar denge.

---

# 🎯 Main Observation

Hume actual new string banane ki zarurat nahi hai.

Hume sirf ye find karna hai:

```text
t ka kitna prefix s me already subsequence hai?
```

Agar `j` characters match hue:

```text
Remaining = t.length - j
```

Bas wahi answer hai.

---

# ⏱️ Time Complexity

Pointer `i` string `s` ko maximum ek baar traverse karta hai.

So:

```text
O(s.length())
```

Ya simple notation me:

```text
O(n)
```

---

# 💾 Space Complexity

Hum sirf:

```text
i
j
```

variables use kar rahe hain.

Koi extra string/vector nahi bana rahe.

So:

```text
O(1)
```

---

# ⭐ Important Points

```text
i → s ko traverse karta hai

j → t ke current required character ko track karta hai
```

Match:

```cpp
if(s[i] == t[j]) {
    j++;
}
```

Har baar:

```cpp
i++;
```

Finally:

```cpp
return t.size() - j;
```

---

# 🔥 Quick Revision

```text
s = "abcde"
t = "acefg"

        ↓

i → s
j → t

        ↓

a == a
→ j++
→ i++

        ↓

b != c
→ only i++

        ↓

c == c
→ j++
→ i++

        ↓

d != e
→ only i++

        ↓

e == e
→ j++
→ i++

        ↓

j = 3

Matched = "ace"

Remaining = "fg"

        ↓

t.size() - j

= 5 - 3

= 2
```

---

# 🎯 Pattern To Remember

Is question ka main pattern:

```text
Subsequence Matching Using Two Pointers
```

Simple rule:

```text
i → source string s ko scan karo

j → target string t ka required character
```

Agar match:

```text
s[i] == t[j]

→ j++
```

Aur:

```text
i har baar ++
```

Finally:

```text
Total Required - Already Matched
```

Matlab:

```cpp
t.size() - j
```

---

# 📊 Complexity Summary

```text
Time Complexity  → O(s.length())

Space Complexity → O(1)
```

---

# 🧠 One-Line Revision

```text
s ko scan karo → t ke characters order me match karo → j matched characters batayega → remaining = t.size() - j.
```
