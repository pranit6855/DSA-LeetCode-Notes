# LeetCode 202 - Happy Number

## 📌 Problem

Hume ek integer `n` diya gaya hai.

Har step me:

1. Number ki har digit ka square karna hai.
2. Sab squares ko add karna hai.
3. Jo sum aaya uske saath fir wahi process repeat karna hai.

Agar finally:

```text
1
```

aa gaya

Return:

```text
true
```

Agar numbers repeat hone lage (cycle ban jaye)

Return:

```text
false
```

---

# 🔹 Example 1

```text
n = 19
```

Process

```text
19

↓

1² + 9²

↓

82

↓

8² + 2²

↓

68

↓

6² + 8²

↓

100

↓

1² + 0² + 0²

↓

1
```

Output

```text
true
```

---

# 🔹 Example 2

```text
n = 2
```

Process

```text
2

↓

4

↓

16

↓

37

↓

58

↓

89

↓

145

↓

42

↓

20

↓

4
```

Dekho

```text
4
```

dobara aa gaya.

Matlab cycle ban gayi.

Output

```text
false
```

---

# 🧠 Main Idea

Ye Linked List nahi hai.

Lekin har number ka next number fixed hai.

Example

```text
19

↓

82

↓

68

↓

100

↓

1
```

Isko hum aise soch sakte hain.

```text
19 → 82 → 68 → 100 → 1
```

Agar cycle ban gayi

```text
4 → 16 → 37 → 58
↑              ↓
← ← ← ← ← ← ← ←
```

To ye bilkul Linked List Cycle jaisa hai.

Isliye:

```text
Slow → 1 step

Fast → 2 steps
```

---

# 🔥 Step 1 - Helper Function

Sabse pehle ek function banayenge.

```cpp
int nextNumber(int n)
```

Ye function return karega

```text
Digits ke square ka sum
```

Example

```text
19

↓

1² + 9²

↓

82
```

---

# 🔥 Helper Function Code

```cpp
int nextNumber(int n){

    int sum = 0;

    while(n > 0){

        int digit = n % 10;

        sum += digit * digit;

        n = n / 10;
    }

    return sum;
}
```

---

## Line by Line

### Step 1

```cpp
int sum = 0;
```

Answer store karne ke liye.

---

### Step 2

```cpp
while(n > 0)
```

Jab tak number ke digits bache hain.

---

### Step 3

```cpp
int digit = n % 10;
```

Last digit nikalo.

Example

```text
82

↓

digit = 2
```

---

### Step 4

```cpp
sum += digit * digit;
```

Digit ka square add karo.

Example

```text
2² = 4
```

---

### Step 5

```cpp
n = n / 10;
```

Last digit hata do.

```text
82

↓

8
```

Fir loop repeat.

Finally

```cpp
return sum;
```

---

# 🔥 Step 2 - Create Two Pointers

```cpp
int slow = n;

int fast = n;
```

Initially

```text
slow = 19

fast = 19
```

---

# 🔥 Step 3 - Move Slow

```cpp
slow = nextNumber(slow);
```

Slow sirf

```text
1 step
```

move karega.

Example

```text
19

↓

82
```

So

```text
slow = 82
```

---

# 🔥 Step 4 - Move Fast

```cpp
fast = nextNumber(nextNumber(fast));
```

Fast

```text
2 steps
```

move karega.

Example

Initially

```text
fast = 19
```

First function

```text
19

↓

82
```

Second function

```text
82

↓

68
```

So

```text
fast = 68
```

---

# 🔥 Step 5 - Check Meeting

```cpp
if(slow == fast)
```

Agar dono same value par aa gaye.

Matlab

```text
Cycle detect ho gayi.
```

Loop stop.

---

# 🔄 Dry Run

Initially

```text
slow = 19

fast = 19
```

↓

Iteration 1

```text
slow = 82

fast = 68
```

↓

Iteration 2

```text
slow = 68

fast = 1
```

↓

Iteration 3

```text
slow = 100

fast = 1
```

↓

Iteration 4

```text
slow = 1

fast = 1
```

Dono mil gaye.

Loop stop.

---

# 🔥 Step 6 - Return Answer

Ab check karenge meeting point kya tha.

```cpp
if(slow == 1){
    return true;
}
else{
    return false;
}
```

Agar

```text
slow = 1
```

Matlab Happy Number.

Return

```text
true
```

Agar

```text
slow = 4
```

ya

```text
89
```

ya koi aur repeated number.

Matlab cycle me fas gaya.

Return

```text
false
```

Short form

```cpp
return slow == 1;
```

Ye internally same hai.

```cpp
if(slow == 1){
    return true;
}
else{
    return false;
}
```

---

# 💻 C++ Code

```cpp
class Solution {
public:

    int nextNumber(int n){

        int sum = 0;

        while(n > 0){

            int digit = n % 10;

            sum += digit * digit;

            n = n / 10;
        }

        return sum;
    }

    bool isHappy(int n) {

        int slow = n;
        int fast = n;

        while(true){

            slow = nextNumber(slow);

            fast = nextNumber(nextNumber(fast));

            if(slow == fast){
                break;
            }
        }

        if(slow == 1){
            return true;
        }
        else{
            return false;
        }
    }
};
```

---

# 🔥 Most Important Concept

```text
Slow = 1 step

↓

Fast = 2 steps

↓

Meeting

↓

Meeting at 1

↓

Happy Number

OR

Meeting at any other number

↓

Cycle

↓

Not Happy
```

---

# ⏱️ Time Complexity

```text
O(log n)
```

---

# 💾 Space Complexity

```text
O(1)
```

---

# ⚠️ Common Mistakes

### 1.

Fast ko sirf ek baar function call karna.

Wrong

```cpp
fast = nextNumber(fast);
```

Correct

```cpp
fast = nextNumber(nextNumber(fast));
```

---

### 2.

Meeting check move karne se pehle.

Wrong

```cpp
if(slow == fast)
```

Initially dono equal hi hote hain.

Pehle move karo.

Fir compare karo.

---

### 3.

Return direct false.

Meeting point check karna mat bhoolo.

```cpp
if(slow == 1){
    return true;
}
else{
    return false;
}
```

---

# 🔥 Quick Revision

```text
Create nextNumber()

↓

Slow = 1 step

↓

Fast = 2 steps

↓

Slow == Fast ?

↓

Meeting at 1 ?

↓

Happy

↓

Else

↓

Cycle
```

---

# 🧠 One-Line Revision

Har number ko uske digit squares ke sum me convert karo. Slow ek transformation karega, Fast do transformations. Agar dono `1` par milte hain to Happy Number hai, warna kisi aur value par milne ka matlab cycle hai.

---

# ⭐ Interview Revision

```text
Number Sequence

↓

Treat Like Linked List

↓

Slow = 1 Step

↓

Fast = 2 Steps

↓

Meeting

↓

1 → Happy

Else → Not Happy
```

Pattern

```text
Slow & Fast Pointer

↓

Floyd Cycle Detection

↓

Happy Number
```
