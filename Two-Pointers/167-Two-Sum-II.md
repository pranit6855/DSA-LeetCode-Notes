# LeetCode 167 - Two Sum II: Input Array Is Sorted

## 📌 Problem

Hume ek **sorted array `numbers`** aur ek integer `target` diya gaya hai.

Hume array ke **2 numbers** find karne hain jinka sum exactly `target` ke equal ho.

Matlab:

```text
numbers[i] + numbers[j] = target
```

Important:

- Array already sorted hai.
- Exactly ek valid answer guaranteed hai.
- Same element ko twice use nahi kar sakte.
- Answer me **1-based indices** return karne hain.

---

## 🔹 Example

```text
numbers = [2,7,11,15]
target = 9
```

Hume:

```text
2 + 7 = 9
```

mil gaya.

Indices normally:

```text
2 → index 0
7 → index 1
```

Lekin problem **1-indexed answer** maang rahi hai.

So:

```text
0 + 1 = 1
1 + 1 = 2
```

Answer:

```text
[1,2]
```

---

# 💡 Approach - Two Pointers

Kyuki array already **sorted** hai, hum Two Pointer use kar sakte hain.

Ek pointer starting me:

```text
i = 0
```

Aur dusra pointer end me:

```text
j = n-1
```

Example:

```text
numbers = [2,7,11,15]

           ↑       ↑
           i       j
```

Ab dono ka sum calculate karenge:

```cpp
int sum=numbers[i]+numbers[j];
```

Uske baad sum ko target se compare karenge.

---

# 🧠 Pointer Meaning

```text
i → left pointer

j → right pointer
```

Starting:

```cpp
int i=0;
int j=numbers.size()-1;
```

---

# 🔍 Three Cases

Har iteration me:

```cpp
int sum=numbers[i]+numbers[j];
```

calculate karenge.

Uske baad 3 possibilities hain.

---

# Case 1 - `sum == target`

Agar:

```cpp
sum==target
```

to answer mil gaya.

Example:

```text
numbers[i] = 2
numbers[j] = 7

2 + 7 = 9

target = 9
```

So direct return:

```cpp
return {i+1,j+1};
```

---

# ❓ `i+1` Aur `j+1` Kyu?

C++ arrays normally **0-indexed** hote hain.

Example:

```text
numbers = [2,7,11,15]

index      0 1  2  3
```

Lekin problem answer **1-indexed** maang rahi hai:

```text
index      1 2  3  4
```

Isliye:

```cpp
return {i+1,j+1};
```

karna padta hai.

---

# Case 2 - `sum < target`

Agar:

```cpp
sum<target
```

iska matlab current sum **target se chhota** hai.

Hume sum ko bada karna hai.

Array sorted hai:

```text
small --------------------> large
```

Isliye left pointer ko right side move karenge:

```cpp
i++;
```

---

## Example

```text
numbers = [2,3,4,8,10]
target = 12
```

Starting:

```text
[2,3,4,8,10]
 ↑          ↑
 i          j
```

Sum:

```text
2 + 10 = 12
```

Ye exact answer hai.

Lekin maan lo target `13` hota:

```text
2 + 10 = 12

12 < 13
```

Sum chhota hai.

Hume bigger number chahiye.

So:

```cpp
i++;
```

Now:

```text
[2,3,4,8,10]
   ↑        ↑
   i        j
```

Left value:

```text
2 → 3
```

ho gayi.

Isse sum increase hoga.

---

# Case 3 - `sum > target`

Agar:

```cpp
sum>target
```

matlab current sum target se **bada** hai.

Hume sum ko chhota karna hai.

Array sorted hai, isliye right pointer ko left move karenge:

```cpp
j--;
```

---

## Example

```text
numbers = [2,3,4,8,15]
target = 10
```

Starting:

```text
[2,3,4,8,15]
 ↑          ↑
 i          j
```

Sum:

```text
2 + 15 = 17
```

Target:

```text
10
```

Since:

```text
17 > 10
```

sum bahut bada hai.

So:

```cpp
j--;
```

Ab:

```text
[2,3,4,8,15]
 ↑       ↑
 i       j
```

Right value:

```text
15 → 8
```

ho gayi.

Sum decrease ho gaya.

---

# 🔄 Complete Dry Run

Consider:

```text
numbers = [2,7,11,15]
target = 9
```

Starting:

```text
i = 0
j = 3
```

Pointers:

```text
[2,7,11,15]
 ↑        ↑
 i        j
```

---

## Step 1

Current sum:

```text
2 + 15 = 17
```

Compare:

```text
17 > 9
```

Sum bada hai.

So:

```cpp
j--;
```

Now:

```text
j = 2
```

Pointers:

```text
[2,7,11,15]
 ↑     ↑
 i     j
```

---

## Step 2

Sum:

```text
2 + 11 = 13
```

Compare:

```text
13 > 9
```

Again sum bada hai.

So:

```cpp
j--;
```

Now:

```text
j = 1
```

Pointers:

```text
[2,7,11,15]
 ↑ ↑
 i j
```

---

## Step 3

Sum:

```text
2 + 7 = 9
```

Target:

```text
9
```

So:

```text
sum == target
```

Answer mil gaya.

Current indices:

```text
i = 0
j = 1
```

But problem 1-indexed answer maang rahi hai.

So:

```text
i + 1 = 1
j + 1 = 2
```

Return:

```text
[1,2]
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int i=0;
        int j=numbers.size()-1;

        while(i<j){

            int sum=numbers[i]+numbers[j];

            if(sum==target){
                return {i+1,j+1};
            }

            else if(sum<target){
                i++;
            }

            else{
                j--;
            }
        }

        return {};
    }
};
```

---

# 🧠 Code Explanation

## Left Pointer

```cpp
int i=0;
```

Array ke smallest element se start karta hai.

---

## Right Pointer

```cpp
int j=numbers.size()-1;
```

Array ke largest element se start karta hai.

---

## Main Loop

```cpp
while(i<j)
```

Jab tak left pointer right pointer se pehle hai, tab tak search karenge.

---

# ❓ `i < j` Kyu?

Same element ko twice use nahi kar sakte.

Agar:

```text
i == j
```

to dono pointers same element ko point karenge.

Example:

```text
[2,5,8]
   ↑
  i,j
```

Tab hum:

```text
5 + 5
```

kar rahe honge.

Lekin problem bolti hai:

```text
You may not use the same element twice.
```

Isliye:

```cpp
while(i<j)
```

use karte hain.

---

# 🔹 Sum Calculate

```cpp
int sum=numbers[i]+numbers[j];
```

Current left aur right element ka sum.

---

# 🔹 Exact Answer

```cpp
if(sum==target){
    return {i+1,j+1};
}
```

Target mil gaya to direct answer return.

---

# 🔹 Sum Chhota

```cpp
else if(sum<target){
    i++;
}
```

Sum ko bada karna hai.

Sorted array me `i++` karne se bigger value milti hai.

---

# 🔹 Sum Bada

```cpp
else{
    j--;
}
```

Sum ko chhota karna hai.

Sorted array me `j--` karne se smaller value milti hai.

---

# ❓ Sorting Kyu Important Hai?

Two Pointer movement ka pura logic sorting par depend karta hai.

Sorted array:

```text
[2,7,11,15]
```

Hume pata hai:

```text
left → smaller values

right → larger values
```

Isliye:

```text
sum chhota
→ left ko aage karo
→ sum increase hoga
```

Aur:

```text
sum bada
→ right ko peeche karo
→ sum decrease hoga
```

Agar array sorted nahi hota:

```text
[11,2,15,7]
```

to `i++` karne se value increase hogi, ye guarantee nahi hoti.

---

# ⏱️ Time Complexity

Two pointers array ko maximum ek baar traverse karte hain.

```text
O(n)
```

---

# 💾 Space Complexity

Hum sirf:

```text
i
j
sum
```

use kar rahe hain.

Koi extra array/hash map nahi.

So:

```text
O(1)
```

---

# ⭐ Important Points

```text
Array already sorted hai
```

Start:

```text
i = 0
j = n-1
```

Then:

```text
sum = numbers[i] + numbers[j]
```

Movement:

```text
sum == target
→ return {i+1,j+1}
```

```text
sum < target
→ sum bada chahiye
→ i++
```

```text
sum > target
→ sum chhota chahiye
→ j--
```

---

# 🔥 Quick Revision

```text
Sorted Array

[2,7,11,15]
 ↑        ↑
 i        j

      ↓

numbers[i] + numbers[j]

      ↓

--------------------------------

sum == target
→ answer mil gaya
→ return {i+1,j+1}

--------------------------------

sum < target
→ sum chhota hai
→ bigger value chahiye
→ i++

--------------------------------

sum > target
→ sum bada hai
→ smaller value chahiye
→ j--

--------------------------------
```

---

# 🎯 Pattern To Remember

Is problem ka classic Two Pointer pattern:

```text
Sorted Array + Pair Sum
        ↓
Left + Right Pointer
```

Rule:

```text
Too Small → Move Left

Too Large → Move Right

Equal → Answer
```

Aur ek important difference normal Two Sum se:

```text
Two Sum I
→ array necessarily sorted nahi hota
→ Hash Map useful

Two Sum II
→ array already sorted hai
→ Two Pointers best
```
