# Recursion Pattern

# 02. Pow(x, n)

## LeetCode 50

## Pattern
Recursion + Binary Exponentiation

---

# 1. Problem Statement

Given two values:

    x
    n

Return:

    x^n

Example:

    x = 2
    n = 5

Then:

    2^5 = 32

Another example:

    x = 3
    n = 4

Then:

    3^4 = 81

---

# 2. Simple Language Mein Problem

Hume `x` ki power `n` calculate karni hai.

Example:

    x = 2
    n = 3

So:

    2^3
    = 2 × 2 × 2
    = 8

Yahan:

    x = base number
    n = power/exponent

---

# 3. Normal Approach

Normal way mein:

    x × x × x × ... n times

multiply kar sakte hain.

Example:

    2^5

    2 × 2 × 2 × 2 × 2

Agar `n` bahut bada ho, toh bahut saari multiplications karni padengi.

Time Complexity:

    O(n)

Hum recursion se isko faster kar sakte hain.

---

# 4. Main Recursion Idea

Power ko har baar half karenge.

Example:

    2^8

Instead of 8 multiplications:

    2^8
    ↓
    2^4
    ↓
    2^2
    ↓
    2^1
    ↓
    2^0

Har step mein exponent half ho raha hai.

Isliye:

    O(log n)

---

# 5. Even Power

Agar `n` even hai:

    x^n = x^(n/2) × x^(n/2)

Example:

    2^8

Half power:

    2^4

Therefore:

    2^8
    = 2^4 × 2^4

    = 16 × 16
    = 256

Code:

    half = myPow(x, n/2)

Then:

    return half * half;

---

# 6. Odd Power

Agar `n` odd hai:

    x^n = x^(n/2) × x^(n/2) × x

Example:

    2^5

Integer division:

    5/2 = 2

So:

    2^5
    = 2^2 × 2^2 × 2

    = 4 × 4 × 2
    = 32

Isliye odd case mein:

    half * half * x

---

# 7. `x` Kya Hai?

Question mein:

    x^n

calculate karna hai.

Yahan:

    x = base

Example:

    2^5

Toh:

    x = 2
    n = 5

Another example:

    3^4

Toh:

    x = 3
    n = 4

---

# 8. Odd Case Mein Extra `x` Kyu?

Suppose:

    x^5

Power half karne par:

    5/2 = 2

So:

    half = x^2

Ab:

    half × half
    = x^2 × x^2
    = x^4

Lekin hume:

    x^5

chahiye.

Ek `x` abhi bhi missing hai.

So:

    x^5 = x^2 × x^2 × x

Therefore:

    half * half * x

---

# 9. Even vs Odd

## Even

    x^8

    x^4 × x^4

So:

    half × half

---

## Odd

    x^5

    x^2 × x^2 × x

So:

    half × half × x

---

# 10. Base Case

Jab:

    n = 0

Toh:

    x^0 = 1

Example:

    5^0 = 1
    10^0 = 1
    2^0 = 1

So recursion ka base case:

    if(n == 0)
        return 1;

Ye recursion ko stop karta hai.

---

# 11. Negative Power

Question mein `n` negative bhi ho sakta hai.

Example:

    2^-4

Rule:

    x^-n = 1 / x^n

Therefore:

    2^-4
    = 1 / 2^4

    = 1 / 16

So negative exponent ko positive banane ke liye:

    x = 1 / x

and:

    n = -n

---

# 12. Why `long long num`?

Code mein:

    long long num = n;

liye hai.

Reason:

`int` ka smallest value:

    -2147483648

Iska positive version directly `int` mein represent nahi ho sakta.

Isliye safe handling ke liye `long long` use karte hain.

---

# 13. Detailed Dry Run

Example:

    x = 2
    n = 5

Start:

    myPow(2,5)

`n` 0 nahi hai.

`n` positive hai.

Now:

    half = myPow(2, 5/2)

    half = myPow(2,2)

Again:

    half = myPow(2, 2/2)

    half = myPow(2,1)

Again:

    half = myPow(2, 1/2)

    half = myPow(2,0)

Base case:

    myPow(2,0) = 1

Now recursion return karna start karega.

---

## Returning from `2^1`

    half = 1

`1` odd hai.

    1 × 1 × 2

    = 2

So:

    2^1 = 2

---

## Returning from `2^2`

    half = 2

`2` even hai.

    2 × 2

    = 4

So:

    2^2 = 4

---

## Returning from `2^5`

    half = 4

`5` odd hai.

    4 × 4 × 2

    = 32

Final:

    2^5 = 32

---

# 14. Recursion Flow

For:

    2^5

Calls:

    myPow(2,5)
         ↓
    myPow(2,2)
         ↓
    myPow(2,1)
         ↓
    myPow(2,0)

Then return:

    1
    ↓
    2
    ↓
    4
    ↓
    32

Important:

Recursion mein pehle calls neeche jaate hain.

Phir answers return hote waqt calculate hote hain.

---

# 15. Code

class Solution {
public:
    double myPow(double x, int n) {

        // Base case
        if(n == 0) {
            return 1;
        }

        long long num = n;

        // Negative power
        if(num < 0) {
            num = -num;
            x = 1 / x;
        }

        // Half power
        double half = myPow(x, num / 2);

        // Even power
        if(num % 2 == 0) {
            return half * half;
        }

        // Odd power
        return half * half * x;
    }
};

---

# 16. Code Ka Flow

Code ko 4 parts mein samjho.

## Part 1 — Base Case

    if(n == 0) {
        return 1;
    }

Meaning:

    x^0 = 1

Yahin recursion stop hota hai.

---

## Part 2 — Negative Power

    if(num < 0) {
        num = -num;
        x = 1 / x;
    }

Negative exponent ko positive mein convert kar rahe hain.

Example:

    2^-4

becomes:

    (1/2)^4

---

## Part 3 — Half Power

    double half = myPow(x, num / 2);

Exponent ko half karte hain.

Example:

    2^8

becomes:

    2^4

Then:

    2^2

Then:

    2^1

Then:

    2^0

---

## Part 4 — Even / Odd

Even:

    return half * half;

Odd:

    return half * half * x;

---

# 17. Most Important Formula

## Even

    x^n = x^(n/2) × x^(n/2)

## Odd

    x^n = x^(n/2) × x^(n/2) × x

## Base

    x^0 = 1

---

# 18. Common Mistakes

## Mistake 1

Base case bhool jaana.

Correct:

    if(n == 0)
        return 1;

---

## Mistake 2

Odd case mein `x` bhool jaana.

Wrong:

    return half * half;

Correct:

    return half * half * x;

for odd `n`.

---

## Mistake 3

Negative exponent handle na karna.

Correct:

    x = 1 / x;
    n = -n;

---

## Mistake 4

Wrong formula for odd power.

Example:

    x^5

Correct:

    x^2 × x^2 × x

---

## Mistake 5

Median-style integer division issue jaisa kuch samajhna.

Yahan:

    5/2 = 2

because `num` is integer.

Ye expected hai.

---

# 19. Complexity

Har recursive call mein exponent half ho raha hai.

So:

    Time Complexity = O(log n)

Recursion depth:

    O(log n)

Therefore:

    Space Complexity = O(log n)

---

# 20. Interview Explanation

"I will use recursive binary exponentiation.

Instead of multiplying `x` n times, I will calculate the power for `n/2`.

For even `n`, the answer is:

    x^(n/2) × x^(n/2)

For odd `n`, one extra `x` remains:

    x^(n/2) × x^(n/2) × x

The base case is `n = 0`, where the answer is 1.

For negative powers, I use:

    x^-n = 1 / x^n

This reduces the exponent by half at every step, giving O(log n) time."

---

# 21. Pattern Recognition

Agar question mein exponent/power ho aur large `n` ho:

    x^n

Aur exponent ko repeatedly half kiya ja sakta ho:

    n/2

Then think:

    Recursion + Binary Exponentiation

---

# 22. Mental Model

    x^n
      ↓
    n/2
      ↓
    recursive answer
      ↓
    square answer
      ↓
    if odd → multiply x
      ↓
    final answer

---

# 23. One-Line Trick

"Power ko half karte jao, half ka answer recursively nikalo, even mein square karo aur odd mein ek extra x multiply karo."

---

# 24. Final Takeaway

LC 50 ka main concept:

    x^n
     ↓
    n/2
     ↓
    Recursive Call
     ↓
    half answer
     ↓
    Even → half × half
    Odd  → half × half × x

Base:

    n = 0 → 1

Negative:

    x^-n = 1/x^n

Complexity:

    Time = O(log n)
    Space = O(log n)

Core Recursion Pattern:

    Base Case
        +
    Smaller Problem
        +
    Result Combine
    
