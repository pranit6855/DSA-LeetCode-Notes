# LeetCode 7 - Reverse Integer

## 📌 Problem

Hume ek **32-bit signed integer `x`** diya gaya hai.

Hume us integer ke digits ko reverse karke return karna hai.

---

## 🔹 Example 1

```text
Input:

x = 123
```

Reverse:

```text
123 → 321
```

Output:

```text
321
```

---

## 🔹 Example 2

```text
Input:

x = -123
```

Reverse:

```text
-123 → -321
```

Output:

```text
-321
```

---

## 🔹 Example 3

```text
Input:

x = 120
```

Reverse:

```text
120 → 021
```

Integer me starting zero ki koi value nahi hoti.

So:

```text
021 → 21
```

Output:

```text
21
```

---

# 💡 Approach - Digit Extraction

Is problem me hume number ke digits ko **right side se ek-ek karke nikalna hai**.

Example:

```text
123
```

Digits hume is order me milenge:

```text
3
2
1
```

Phir in digits ko use karke:

```text
321
```

banana hai.

Iske liye mainly 3 operations yaad rakhni hain:

```text
1. Last digit nikalo

2. Last digit ko answer me add karo

3. Original number se last digit remove karo
```

---

# 🔥 Step 1 - Last Digit Kaise Nikale?

Kisi integer ka last digit nikalne ke liye:

```cpp
x % 10
```

use karte hain.

Example:

```text
123 % 10 = 3
```

So:

```cpp
int digit = x % 10;
```

gives:

```text
digit = 3
```

---

## More Examples

```text
456 % 10 = 6

92 % 10 = 2

7 % 10 = 7
```

So formula:

```text
Last Digit = Number % 10
```

---

# 🔥 Step 2 - Answer Me Digit Kaise Add Kare?

Suppose:

```text
ans = 32
```

Aur new digit:

```text
digit = 1
```

Hume banana hai:

```text
321
```

Sabse pehle:

```cpp
ans * 10
```

karenge.

```text
32 * 10 = 320
```

Phir digit add:

```text
320 + 1 = 321
```

So formula:

```cpp
ans = ans * 10 + digit;
```

---

# ❓ `ans * 10` Kyu?

Multiplication by `10` existing digits ko ek position left shift karta hai.

Example:

```text
3
```

Multiply by 10:

```text
30
```

Then:

```text
30 + 2 = 32
```

Again:

```text
32 * 10 = 320
```

Then:

```text
320 + 1 = 321
```

So:

```cpp
ans = ans * 10 + digit;
```

reversed number gradually build karta hai.

---

# 🔥 Step 3 - Original Number Se Last Digit Remove Kaise Kare?

Integer division by `10` use karenge:

```cpp
x = x / 10;
```

Example:

```text
123 / 10 = 12
```

C++ me integer division decimal part remove kar deta hai.

So:

```text
123 → 12
```

Next:

```text
12 / 10 = 1
```

Then:

```text
1 / 10 = 0
```

So formula:

```text
Last Digit Remove = Number / 10
```

---

# 🧠 Complete Main Logic

Bas ye 3 lines main logic hain:

```cpp
int digit = x % 10;

ans = ans * 10 + digit;

x = x / 10;
```

Yaad rakho:

```text
% 10
→ last digit NIKALO

* 10 + digit
→ answer BANAO

/ 10
→ last digit HATAO
```

---

# 🔄 Complete Dry Run

Consider:

```text
x = 123
```

Starting:

```text
ans = 0
```

---

## Step 1

Current:

```text
x = 123
ans = 0
```

Last digit:

```cpp
digit = x % 10;
```

So:

```text
digit = 123 % 10
      = 3
```

Now answer:

```cpp
ans = ans * 10 + digit;
```

```text
ans = 0 * 10 + 3
    = 3
```

Remove last digit:

```cpp
x = x / 10;
```

```text
x = 123 / 10
  = 12
```

Now:

```text
x = 12
ans = 3
```

---

## Step 2

Last digit:

```text
digit = 12 % 10
      = 2
```

Answer:

```text
ans = 3 * 10 + 2
    = 32
```

Remove digit:

```text
x = 12 / 10
  = 1
```

Now:

```text
x = 1
ans = 32
```

---

## Step 3

Last digit:

```text
digit = 1 % 10
      = 1
```

Answer:

```text
ans = 32 * 10 + 1
    = 321
```

Remove digit:

```text
x = 1 / 10
  = 0
```

Now:

```text
x = 0
ans = 321
```

---

# 🛑 Loop Stop

Condition hai:

```cpp
while(x != 0)
```

Ab:

```text
x = 0
```

So loop stop.

Final:

```text
ans = 321
```

Return:

```text
321
```

---

# 🔄 Negative Number Kaise Handle Hoga?

Consider:

```text
x = -123
```

C++ me:

```text
-123 % 10 = -3
```

So:

```text
digit = -3
```

Answer:

```text
ans = 0 * 10 + (-3)
    = -3
```

Then:

```text
x = -123 / 10
  = -12
```

Next:

```text
-12 % 10 = -2
```

Answer:

```text
-3 * 10 + (-2)
= -32
```

Next:

```text
-1
```

add hoga.

Final:

```text
-321
```

Isliye negative numbers ke liye alag se sign handle karne ki zarurat nahi padti.

---

# 🔹 Trailing Zero Example

Consider:

```text
x = 120
```

### Step 1

```text
digit = 120 % 10
      = 0
```

Answer:

```text
ans = 0 * 10 + 0
    = 0
```

x:

```text
120 / 10 = 12
```

---

### Step 2

```text
digit = 2
```

Answer:

```text
0 * 10 + 2 = 2
```

---

### Step 3

```text
digit = 1
```

Answer:

```text
2 * 10 + 1
= 21
```

Final:

```text
21
```

Isliye:

```text
120 → 21
```

automatically handle ho jata hai.

---

# ⚠️ Integer Overflow

Ye question ka important part hai.

C++ ka normal:

```cpp
int
```

sirf ek fixed range tak values store kar sakta hai.

32-bit signed integer range:

```text
INT_MIN = -2147483648

INT_MAX =  2147483647
```

Suppose kisi number ko reverse karne ke baad answer:

```text
9646324351
```

aa gaya.

Ye `int` ki maximum range:

```text
2147483647
```

se bada hai.

Problem ke according agar reversed number 32-bit integer range se bahar chala jaye:

```text
return 0
```

karna hai.

---

# 💡 Overflow Handle Karne Ke Liye

Hum answer ko:

```cpp
long long
```

me store karte hain:

```cpp
long long ans = 0;
```

`long long` normal `int` se larger values store kar sakta hai.

Pehle safely reverse bana lenge.

Uske baad check:

```cpp
if(ans > INT_MAX || ans < INT_MIN) {
    return 0;
}
```

---

# 🔍 Overflow Condition

```cpp
ans > INT_MAX
```

means:

```text
answer maximum int value se bada hai
```

OR:

```cpp
ans < INT_MIN
```

means:

```text
answer minimum int value se bhi chhota hai
```

Dono cases me:

```cpp
return 0;
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int reverse(int x) {

        long long ans = 0;

        while(x != 0) {

            int digit = x % 10;

            ans = ans * 10 + digit;

            x = x / 10;
        }

        if(ans > INT_MAX || ans < INT_MIN) {
            return 0;
        }

        return ans;
    }
};
```

---

# 🧠 Code Explanation

## 1. Answer Variable

```cpp
long long ans = 0;
```

Reversed number ko store karega.

`long long` overflow safely check karne ke liye use kiya hai.

---

## 2. Loop

```cpp
while(x != 0)
```

Jab tak original number ke digits bache hain, loop chalega.

---

## 3. Last Digit

```cpp
int digit = x % 10;
```

Current number ka last digit nikalta hai.

Example:

```text
123 % 10 = 3
```

---

## 4. Reverse Build Karna

```cpp
ans = ans * 10 + digit;
```

Existing answer ko ek digit left shift karke new digit add karta hai.

Example:

```text
ans = 32
digit = 1

32 * 10 + 1
= 321
```

---

## 5. Last Digit Remove

```cpp
x = x / 10;
```

Original number se last digit remove karta hai.

Example:

```text
123 / 10 = 12
```

---

## 6. Overflow Check

```cpp
if(ans > INT_MAX || ans < INT_MIN) {
    return 0;
}
```

Agar reversed number 32-bit signed integer range ke bahar hai:

```text
return 0
```

---

## 7. Final Answer

```cpp
return ans;
```

Agar overflow nahi hua to reversed integer return kar do.

---

# ⏱️ Time Complexity

Agar number me `d` digits hain, hum har digit ko ek baar process karte hain.

So:

```text
O(d)
```

Integer `x` ke terms me ise:

```text
O(log₁₀ |x|)
```

bhi likh sakte hain.

Simple revision ke liye:

```text
Time = O(number of digits)
```

---

# 💾 Space Complexity

Hum sirf:

```text
ans
digit
```

jaise variables use kar rahe hain.

Koi array/string/vector nahi bana rahe.

So:

```text
O(1)
```

---

# ⭐ Most Important Formulas

## Last Digit Nikalo

```cpp
digit = x % 10;
```

---

## Reverse Me Add Karo

```cpp
ans = ans * 10 + digit;
```

---

## Last Digit Remove Karo

```cpp
x = x / 10;
```

---

# 🔥 Quick Revision

```text
x = 123
ans = 0

        ↓

digit = x % 10
      = 3

        ↓

ans = ans*10 + digit
    = 3

        ↓

x = x/10
  = 12

        ↓

Repeat
```

Complete:

```text
123

 ↓ %10

3

 ↓

ans = 3

 ↓ /10

12

 ↓ %10

2

 ↓

ans = 32

 ↓ /10

1

 ↓ %10

1

 ↓

ans = 321

 ↓

x = 0

 ↓

return 321
```

---

# 🎯 Pattern To Remember

Reverse Integer ka main pattern:

```text
EXTRACT → BUILD → REMOVE
```

### Extract

```cpp
digit = x % 10;
```

### Build

```cpp
ans = ans * 10 + digit;
```

### Remove

```cpp
x = x / 10;
```

Bas ye teen operations yaad rahe to integer reverse easily likh sakte ho:

```text
% 10        → last digit nikalo

* 10 + digit → reverse build karo

/ 10        → last digit hatao
```

---

# 📊 Complexity Summary

```text
Time Complexity  → O(number of digits)

Space Complexity → O(1)
```
