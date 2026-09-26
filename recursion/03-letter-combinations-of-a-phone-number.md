# Recursion Pattern

# 03. Letter Combinations of a Phone Number

## LeetCode 17

## Pattern
Recursion + Multiple Choices + Backtracking

---

# 1. Problem Statement

Given a string containing digits from `2` to `9`, return all possible letter combinations that the digits could represent.

Phone keypad:

    2 → abc
    3 → def
    4 → ghi
    5 → jkl
    6 → mno
    7 → pqrs
    8 → tuv
    9 → wxyz

Example:

    digits = "23"

Output:

    ["ad","ae","af","bd","be","bf","cd","ce","cf"]

Order does not matter.

---

# 2. Simple Language Mein Problem

Phone keypad mein har digit ke multiple letters hote hain.

For example:

    2 → a b c
    3 → d e f

Agar input:

    "23"

hai, toh:

2 ke 3 choices hain:

    a
    b
    c

3 ke 3 choices hain:

    d
    e
    f

Ab har possible combination banana hai:

    a + d = ad
    a + e = ae
    a + f = af

    b + d = bd
    b + e = be
    b + f = bf

    c + d = cd
    c + e = ce
    c + f = cf

Total:

    9 combinations

---

# 3. Main Concept

Yahan recursion ka main idea hai:

    Har digit par uske saare possible letters try karo.

Example:

    2 → a,b,c

Pehle:

    a choose karo

Phir next digit `3` ke letters try karo:

    ad
    ae
    af

Phir wapas aao.

Next:

    b choose karo

Then:

    bd
    be
    bf

Then:

    c choose karo

Then:

    cd
    ce
    cf

---

# 4. Recursion Tree

For:

    digits = "23"

Tree:

                    ""
                 /   |   \
                a    b    c
              /|\  /|\  /|\
             ad ae af bd be bf cd ce cf

Har level ek digit represent karta hai.

Level 0:

    Digit 2

Level 1:

    Digit 3

Har level par available letters choose karte hain.

---

# 5. Recursion Mein Kya Track Karna Hai?

Hum 3 cheezein maintain karenge:

## 1. digits

Original input.

Example:

    "23"

## 2. index

Kaunsi digit process kar rahe hain.

    index = 0 → digit 2
    index = 1 → digit 3

## 3. current

Ab tak jo combination bana hai.

Example:

    ""
    "a"
    "ad"

---

# 6. Base Case

Base case:

    index == digits.length()

Matlab saari digits process ho gayi.

Example:

    digits = "23"

Length:

    2

Agar:

    index = 2

Toh dono digits process ho chuki hain.

Suppose:

    current = "ad"

Ab combination complete hai.

So:

    ans.push_back(current);

Then:

    return;

---

# 7. Current Digit Ke Letters Kaise Milenge?

Current digit ko check karenge.

Example:

    digits[index] = '2'

Then:

    letters = "abc"

Agar:

    digits[index] = '3'

Then:

    letters = "def"

Simple version mein hum conditions use kar sakte hain:

    if(digits[index] == '2') letters = "abc";
    if(digits[index] == '3') letters = "def";

and so on.

---

# 8. For Loop Ka Role

Suppose:

    letters = "abc"

Loop:

    for(char ch : letters)

Ye teen baar chalega:

    ch = 'a'
    ch = 'b'
    ch = 'c'

Har baar ek choice select hogi.

---

# 9. Recursive Call

Main recursion line:

    solve(digits, index + 1, current + ch);

Iska meaning:

    Current letter choose karo
        ↓
    current mein add karo
        ↓
    next digit par jao

Example:

    current = ""
    ch = 'a'

Then:

    current + ch = "a"

And:

    index + 1

means next digit.

So call:

    solve("23", 1, "a")

---

# 10. Detailed Dry Run

Input:

    digits = "23"

Start:

    solve("23", 0, "")

---

## Step 1

    index = 0

Current digit:

    2

Letters:

    abc

First choice:

    a

So:

    current = "a"

Next recursive call:

    solve("23", 1, "a")

---

## Step 2

Now:

    index = 1
    current = "a"

Current digit:

    3

Letters:

    def

First choice:

    d

So:

    current = "ad"

Next call:

    solve("23", 2, "ad")

---

## Step 3

Now:

    index = 2

And:

    digits.length() = 2

So:

    index == digits.length()

Base case.

Current:

    "ad"

Add to answer:

    ans = ["ad"]

Return.

---

## Step 4

Recursion returns to digit `3`.

Next choice:

    e

Current:

    "ae"

Add:

    "ae"

Then:

    af

So after processing `a`:

    ad
    ae
    af

---

## Step 5

Now recursion goes back to digit `2`.

Next choice:

    b

Then digit `3` ke choices:

    bd
    be
    bf

---

## Step 6

Next choice:

    c

Then:

    cd
    ce
    cf

Final:

    ad
    ae
    af
    bd
    be
    bf
    cd
    ce
    cf

---

# 11. Important Recursion Flow

For:

    "23"

Actual flow:

    ""
     ↓
     a
     ↓
    ad
     ↓
    return
     ↓
    ae
     ↓
    return
     ↓
    af
     ↓
    return
     ↓
     b
     ↓
    bd
    ...
    
This process continues until all choices are tried.

---

# 12. Why `index + 1`?

Because next recursive call mein next digit process karni hai.

Example:

    digits = "23"

Start:

    index = 0
    digit = 2

After choosing one letter from `2`:

    index = 1
    digit = 3

So:

    index + 1

means:

    next digit par jao

---

# 13. Why `current + ch`?

Suppose:

    current = "a"
    ch = 'd'

Then:

    current + ch

becomes:

    "ad"

So every recursion call mein current combination grow hota hai.

Example:

    ""
    ↓
    "a"
    ↓
    "ad"

---

# 14. Why `ans.push_back(current)`?

Jab saari digits process ho jaati hain, tab `current` ek complete combination hota hai.

Example:

    current = "ad"

Then:

    ans.push_back("ad");

Similarly:

    ae
    af
    bd
    ...

sab answer mein add honge.

---

# 15. Code

class Solution {
public:
    vector<string> ans;

    void solve(string digits, int index, string current) {

        if(index == digits.length()) {
            ans.push_back(current);
            return;
        }

        string letters;

        if(digits[index] == '2') letters = "abc";
        if(digits[index] == '3') letters = "def";
        if(digits[index] == '4') letters = "ghi";
        if(digits[index] == '5') letters = "jkl";
        if(digits[index] == '6') letters = "mno";
        if(digits[index] == '7') letters = "pqrs";
        if(digits[index] == '8') letters = "tuv";
        if(digits[index] == '9') letters = "wxyz";

        for(char ch : letters) {
            solve(digits, index + 1, current + ch);
        }
    }

    vector<string> letterCombinations(string digits) {

        if(digits.empty()) {
            return {};
        }

        solve(digits, 0, "");

        return ans;
    }
};

---

# 16. Code Ka Flow

Code ko 4 main parts mein samjho.

## Part 1

    ans

All completed combinations store karega.

---

## Part 2

    solve(digits, index, current)

Recursion function.

It tracks:

    current digit
    +
    current combination

---

## Part 3

    if(index == digits.length())

Base case.

All digits process ho gayi:

    current → answer

---

## Part 4

    for(char ch : letters)

Current digit ke saare possible letters try karo.

Then:

    solve(index + 1, current + ch)

Next digit par jao.

---

# 17. Full Mental Example

Input:

    "23"

Start:

    ""

Digit 2:

    a
    b
    c

For `a`:

    ad
    ae
    af

For `b`:

    bd
    be
    bf

For `c`:

    cd
    ce
    cf

Final:

    9 combinations

---

# 18. Common Mistakes

## Mistake 1

Base case bhool jaana.

Correct:

    if(index == digits.length()) {
        ans.push_back(current);
        return;
    }

---

## Mistake 2

`index + 1` na karna.

Agar index same rakha, toh same digit repeatedly process hogi.

Correct:

    index + 1

---

## Mistake 3

Current letter add na karna.

Correct:

    current + ch

---

## Mistake 4

Complete combination se pehle answer mein push karna.

Answer tab push karo jab:

    index == digits.length()

---

## Mistake 5

Empty string handle na karna.

Correct:

    if(digits.empty()) {
        return {};
    }

---

# 19. Important Concept: Multiple Choices

Normal Fibonacci recursion:

    fib(n)
      ↓
    2 choices

    fib(n-1)
    fib(n-2)

LC 17:

    Current digit
         ↓
    Multiple choices

Example:

    2
    ↓
    a,b,c

So recursion tree mein har level par multiple branches hoti hain.

---

# 20. Recursion vs Backtracking

Is problem mein hum choices try kar rahe hain.

General idea:

    Choice
      ↓
    Recursion
      ↓
    Next Choice

Ye backtracking ka basic idea hai.

Yahan humne:

    current + ch

use kiya hai, isliye explicit `pop_back()` ki zarurat nahi padi.

Har recursive call ko new string mil rahi hai.

---

# 21. Complexity

Agar har digit ke maximum 4 letters hain:

    Number of combinations ≈ 4^n

Har combination ki length `n` hoti hai.

So output generation itself takes:

    O(4^n × n)

Space depends on output size.

Recursion stack:

    O(n)

---

# 22. Interview Explanation

"I will use recursion to generate all possible combinations.

For every digit, I will get all the letters mapped to that digit.

I will try each letter one by one, append it to the current string, and recursively process the next digit.

When all digits are processed, the current string is a complete combination, so I add it to the answer.

The recursion tree represents all possible choices for every digit."

---

# 23. Pattern Recognition

Agar problem mein:

- Multiple choices per position
- Generate all combinations
- Every character/digit has multiple options
- Need all possible answers

Toh think:

    Recursion / Backtracking

Basic structure:

    Choose
      ↓
    Recurse
      ↓
    Next choice

---

# 24. Mental Model

Always think:

    Current digit
         ↓
    Available choices
         ↓
    Ek choice choose
         ↓
    Next digit
         ↓
    Combination complete?
         ↓
    Answer mein add

---

# 25. One-Line Trick

"Har digit ke saare letters try karo, ek letter current mein add karo, next digit par recursion karo, aur saari digits khatam hone par combination answer mein daal do."

---

# 26. Final Takeaway

LC 17 ka main recursion pattern:

    Digit
      ↓
    Choices
      ↓
    Choose one
      ↓
    Next digit
      ↓
    Base Case
      ↓
    Answer

For:

    "23"

Tree:

              ""
           /   |   \
          a    b    c
        /|\  /|\  /|\
       ad ae af bd be bf cd ce cf

Core concepts:

    1. Multiple choices
    2. Recursive call
    3. index + 1
    4. current + ch
    5. Base case
    6. Store completed combination

Complexity:

    Time = O(4^n × n)
    Recursion Space = O(n)
