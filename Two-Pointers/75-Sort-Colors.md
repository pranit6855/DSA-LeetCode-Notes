# LeetCode 75 - Sort Colors

## 📌 Problem

Hume ek array `nums` diya gaya hai jisme sirf 3 types ke elements hain:

```text
0 → Red
1 → White
2 → Blue
```

Hume array ko aise sort karna hai:

```text
saare 0 pehle
saare 1 beech me
saare 2 last me
```

Built-in `sort()` use nahi karna.

---

## 🔹 Example

```text
nums = [2,0,2,1,1,0]
```

Sorted array:

```text
[0,0,1,1,2,2]
```

---

# 💡 Approach - Counting

Kyuki array me sirf:

```text
0
1
2
```

hi ho sakte hain, hum pehle count kar sakte hain ki:

```text
kitne 0 hain
kitne 1 hain
kitne 2 hain
```

Uske baad array ko counts ke according overwrite kar denge.

---

# 🔹 Step 1 - Counters Banao

```cpp
int zero=0,one=0,two=0;
```

Meaning:

```text
zero → total 0 count karega

one → total 1 count karega

two → total 2 count karega
```

---

# 🔹 Step 2 - Array Traverse Karke Count Karo

```cpp
for(int i=0;i<n;i++)
```

Har element check karenge.

Agar:

```cpp
nums[i]==0
```

to:

```cpp
zero++;
```

Agar:

```cpp
nums[i]==1
```

to:

```cpp
one++;
```

Agar:

```cpp
nums[i]==2
```

to:

```cpp
two++;
```

---

# 🔄 Example Counting

Input:

```text
nums = [2,0,2,1,1,0]
```

Starting:

```text
zero = 0
one = 0
two = 0
```

---

## Element 1

```text
nums[0] = 2
```

So:

```text
two = 1
```

---

## Element 2

```text
nums[1] = 0
```

So:

```text
zero = 1
```

---

## Element 3

```text
nums[2] = 2
```

So:

```text
two = 2
```

---

## Element 4

```text
nums[3] = 1
```

So:

```text
one = 1
```

---

## Element 5

```text
nums[4] = 1
```

So:

```text
one = 2
```

---

## Element 6

```text
nums[5] = 0
```

So:

```text
zero = 2
```

---

# ✅ Final Counts

```text
zero = 2
one  = 2
two  = 2
```

Matlab final array me:

```text
2 zeros
2 ones
2 twos
```

hone chahiye.

---

# 🔹 Step 3 - Zeros Fill Karo

Total:

```text
zero = 2
```

So starting ke `2` positions par `0` fill karenge:

```cpp
for(int i=0;i<zero;i++){
    nums[i]=0;
}
```

Array:

```text
[0,0,_,_,_,_]
```

---

# 🔹 Step 4 - Ones Fill Karo

Zeros ne positions occupy ki:

```text
0 se zero-1
```

So ones start honge:

```text
zero
```

se.

Aur total `one` elements fill karne hain.

Isliye ending position:

```text
zero + one
```

tak hogi.

Code:

```cpp
for(int i=zero;i<zero+one;i++){
    nums[i]=1;
}
```

Example:

```text
zero = 2
one = 2
```

Loop:

```text
i = 2
i = 3
```

Array:

```text
[0,0,1,1,_,_]
```

---

# 🔹 Step 5 - Twos Fill Karo

Zeros aur ones milakar:

```text
zero + one
```

positions occupy kar chuke hain.

So twos start honge:

```text
zero + one
```

se.

Code:

```cpp
for(int i=zero+one;i<n;i++){
    nums[i]=2;
}
```

Array:

```text
[0,0,1,1,2,2]
```

Sorted.

---

# ❓ Ones Ke Loop Me `i < one` Kyu Nahi?

Ye important hai.

Suppose:

```text
zero = 2
one = 3
```

Array me hona chahiye:

```text
[0,0,1,1,1,...]
```

Zeros indices:

```text
0,1
```

Ones indices:

```text
2,3,4
```

Agar hum likhen:

```cpp
for(int i=zero;i<one;i++)
```

to:

```text
i = 2
```

sirf ek baar chalega because:

```text
one = 3
```

Lekin hume **3 ones** fill karne the.

Correct ending:

```text
zero + one
```

So:

```cpp
for(int i=zero;i<zero+one;i++)
```

---

# ❓ Twos `one` Se Start Kyu Nahi Honge?

Suppose:

```text
zero = 2
one = 3
two = 2
```

Final positions:

```text
index:

0 1 | 2 3 4 | 5 6
```

Values:

```text
0 0 | 1 1 1 | 2 2
```

Twos ka starting index:

```text
5
```

Aur:

```text
zero + one
= 2 + 3
= 5
```

Isliye twos start honge:

```cpp
i=zero+one
```

se.

---

# 💻 C++ Code

```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int zero=0,one=0,two=0;
        int n=nums.size();

        for(int i=0;i<n;i++){
            if(nums[i]==0){
                zero++;
            }

            if(nums[i]==1){
                one++;
            }

            if(nums[i]==2){
                two++;
            }
        }

        for(int i=0;i<zero;i++){
            nums[i]=0;
        }

        for(int i=zero;i<zero+one;i++){
            nums[i]=1;
        }

        for(int i=zero+one;i<n;i++){
            nums[i]=2;
        }
    }
};
```

---

# 🧠 Code Explanation

## Counters

```cpp
int zero=0,one=0,two=0;
```

Teen values ki frequency count karenge.

---

## Array Size

```cpp
int n=nums.size();
```

Array ka total size.

---

## Counting Loop

```cpp
for(int i=0;i<n;i++)
```

Poora array traverse karta hai.

---

### Zero Count

```cpp
if(nums[i]==0){
    zero++;
}
```

Har `0` ke liye `zero` increase.

---

### One Count

```cpp
if(nums[i]==1){
    one++;
}
```

Har `1` ke liye `one` increase.

---

### Two Count

```cpp
if(nums[i]==2){
    two++;
}
```

Har `2` ke liye `two` increase.

---

# 🔹 Final Array Ki Range

Agar:

```text
zero = Z
one = O
two = T
```

to final array:

```text
0 ........ Z-1
→ zeros
```

```text
Z ........ Z+O-1
→ ones
```

```text
Z+O ...... n-1
→ twos
```

Short form:

```text
[ Zeros ][ Ones ][ Twos ]

0       zero     zero+one      n
```

---

# 🔄 Full Dry Run

Input:

```text
[2,0,2,1,1,0]
```

Count:

```text
zero = 2
one = 2
two = 2
```

First loop:

```text
[0,0,_,_,_,_]
```

Second loop:

```text
[0,0,1,1,_,_]
```

Third loop:

```text
[0,0,1,1,2,2]
```

Final:

```text
[0,0,1,1,2,2]
```

---

# ⏱️ Time Complexity

First traversal:

```text
O(n)
```

Then array ko values se fill karne me total:

```text
O(n)
```

So:

```text
O(n) + O(n)
```

Constants ignore karne ke baad:

```text
O(n)
```

---

# 💾 Space Complexity

Humne koi extra array nahi banaya.

Sirf:

```text
zero
one
two
n
```

variables use kiye.

So:

```text
O(1)
```

---

# ⚠️ Important

Ye solution:

```text
Time  = O(n)
Space = O(1)
```

hai.

Lekin array ko roughly **2 passes** me process karta hai:

```text
Pass 1 → 0,1,2 count karo

Pass 2 → values wapas fill karo
```

Is question ka ek aur famous solution hai:

```text
Dutch National Flag Algorithm
```

jo:

```text
low
mid
high
```

3 pointers use karke **single pass** me array sort karta hai.

---

# ⭐ Important Points

```text
Count 0
Count 1
Count 2
```

Then:

```text
0 se zero-1
→ fill 0
```

```text
zero se zero+one-1
→ fill 1
```

```text
zero+one se n-1
→ fill 2
```

---

# 🔥 Quick Revision

```text
nums = [2,0,2,1,1,0]

        ↓

Count elements

zero = 2
one  = 2
two  = 2

        ↓

Fill zeros

[0,0,_,_,_,_]

        ↓

Fill ones

[0,0,1,1,_,_]

        ↓

Fill twos

[0,0,1,1,2,2]
```

---

# 🎯 Pattern To Remember

Jab array me limited known values ho:

```text
0, 1, 2
```

to unki **frequency count** karke array ko reconstruct kar sakte hain.

Main idea:

```text
Count → Overwrite
```

For this problem:

```text
0 → [0, zero)

1 → [zero, zero+one)

2 → [zero+one, n)
```
