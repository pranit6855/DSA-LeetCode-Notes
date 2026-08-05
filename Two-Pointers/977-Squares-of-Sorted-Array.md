# LeetCode 977 - Squares of a Sorted Array

## 📌 Problem

Hume ek **sorted integer array `nums`** diya gaya hai.

Array me:

- Negative numbers ho sakte hain
- `0` ho sakta hai
- Positive numbers ho sakte hain

Hume har element ka **square** nikalna hai aur final answer ko **sorted order** me return karna hai.

---

## 🔹 Example

```text
nums = [-4,-1,0,3,10]
```

Agar directly square kare:

```text
(-4)² = 16
(-1)² = 1
0²    = 0
3²    = 9
10²   = 100
```

To milega:

```text
[16,1,0,9,100]
```

Lekin hume **sorted answer** chahiye:

```text
[0,1,9,16,100]
```

---

# ❓ Normal Sorted Order Directly Kaam Kyu Nahi Karta?

Original array already sorted hai:

```text
[-4,-1,0,3,10]
```

Lekin square karne ke baad:

```text
[16,1,0,9,100]
```

sorted nahi raha.

Reason:

Negative number ka square positive ho jata hai.

Example:

```text
-4 < 3
```

Lekin square:

```text
16 > 9
```

Isliye hum sirf left se right traverse karke answer nahi bana sakte.

---

# 💡 Approach - Two Pointers

Important observation:

Sorted array me **largest square** either:

```text
left end
```

ya

```text
right end
```

se milega.

Example:

```text
[-4,-1,0,3,10]
 ↑             ↑
 i             j
```

Squares:

```text
(-4)² = 16
10²   = 100
```

Dono compare karenge.

Jo bada square hoga, usko answer ke **last position** par rakhenge.

---

# 🧠 Pointers

Hum 3 variables use karenge:

```text
i → nums ke left side ko point karega

j → nums ke right side ko point karega

k → answer array ko right se fill karega
```

Initialization:

```cpp
int i=0;
int j=n-1;
int k=n-1;
```

---

# ❓ `k = n-1` Kyu?

Ye is problem ka sabse important point hai.

Array ke dono ends compare karke hume **largest square** easily mil sakta hai.

Example:

```text
[-4,-1,0,3,10]
 ↑             ↑

16            100
```

Hume pata hai:

```text
100
```

sabse bada square hai.

Isliye answer ko **last se fill karna easiest hai**.

```text
ans = [_,_,_,_,100]
             ↑
             k
```

Phir next largest square second-last position par.

---

# ❌ `k = 0` Se Kyu Nahi?

Agar hum:

```cpp
int k=0;
```

se start kare aur dono ends me se **smaller square** choose kare, to problem hogi.

Example:

```text
nums = [-4,-1,0,3,10]
```

Ends ke squares:

```text
16 and 100
```

Smaller:

```text
16
```

To hum kar dete:

```text
ans = [16,_,_,_,_]
```

Lekin actual smallest square hai:

```text
0² = 0
```

jo array ke **middle me hai**.

So ends se:

```text
smallest square guaranteed nahi hai ❌
```

Lekin:

```text
largest square guaranteed hai ✅
```

Isliye answer ko right se fill karte hain.

---

# 🔍 Main Logic

Har iteration me:

```cpp
int left=nums[i]*nums[i];
int right=nums[j]*nums[j];
```

Dono ends ka square calculate karenge.

---

## Case 1 - Left Square Bada Hai

Agar:

```cpp
left > right
```

to:

```cpp
ans[k]=left;
```

Aur kyunki `nums[i]` use ho gaya:

```cpp
i++;
```

---

## Case 2 - Right Square Bada Hai

Otherwise:

```cpp
ans[k]=right;
```

Aur right wala element use ho gaya:

```cpp
j--;
```

---

## Har Case Ke Baad

Answer ki current position fill ho gayi.

So:

```cpp
k--;
```

---

# 🔄 Complete Dry Run

Consider:

```text
nums = [-4,-1,0,3,10]
```

Starting:

```text
i = 0
j = 4
k = 4
```

Answer:

```text
[_,_,_,_,_]
```

---

## Step 1

```text
nums[i] = -4
nums[j] = 10
```

Squares:

```text
left  = 16
right = 100
```

Compare:

```text
16 > 100 → False
```

So:

```cpp
ans[k]=right;
```

Answer:

```text
[_,_,_,_,100]
```

Then:

```text
j = 3
k = 3
```

---

## Step 2

Now:

```text
nums[i] = -4
nums[j] = 3
```

Squares:

```text
left  = 16
right = 9
```

Compare:

```text
16 > 9 → True
```

So:

```cpp
ans[k]=left;
```

Answer:

```text
[_,_,_,16,100]
```

Then:

```text
i = 1
k = 2
```

---

## Step 3

Now:

```text
nums[i] = -1
nums[j] = 3
```

Squares:

```text
left  = 1
right = 9
```

`9` bigger hai.

So:

```text
ans = [_,_,9,16,100]
```

Then:

```text
j = 2
k = 1
```

---

## Step 4

Now:

```text
nums[i] = -1
nums[j] = 0
```

Squares:

```text
left  = 1
right = 0
```

`1` bigger hai.

So:

```text
ans = [_,1,9,16,100]
```

Then:

```text
i = 2
k = 0
```

---

## Step 5

Ab:

```text
i = 2
j = 2
```

Dono same element ko point kar rahe hain:

```text
nums[2] = 0
```

Square:

```text
0
```

Answer:

```text
[0,1,9,16,100]
```

Final answer:

```text
[0,1,9,16,100]
```

---

# ❓ `while(i <= j)` Kyu?

Hum condition use karte hain:

```cpp
while(i<=j)
```

`<=` important hai.

Agar:

```text
i == j
```

to iska matlab ek element abhi bhi bacha hua hai.

Example:

```text
[-1,0,2]
    ↑
   i,j
```

Is element ko bhi answer me add karna hai.

Agar hum:

```cpp
while(i<j)
```

likhte, to `i == j` hote hi loop stop ho jata aur middle element process nahi hota.

Isliye:

```text
i <= j ✅
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int n=nums.size();
        vector<int>ans(n);
        int i=0;
        int j=n-1;
        int k=n-1;

        while(i<=j){
            int left=nums[i]*nums[i];
            int right=nums[j]*nums[j];

            if(left>right){
                ans[k]=left;
                i++;
            }
            else{
                ans[k]=right;
                j--;
            }

            k--;
        }

        return ans;
    }
};
```

---

# 🧠 Code Explanation

## 1. Size

```cpp
int n=nums.size();
```

Array ka size store kiya.

---

## 2. Answer Array

```cpp
vector<int>ans(n);
```

Final squared elements store karne ke liye `n` size ka answer array banaya.

---

## 3. Left Pointer

```cpp
int i=0;
```

Array ke first element ko point karta hai.

---

## 4. Right Pointer

```cpp
int j=n-1;
```

Array ke last element ko point karta hai.

---

## 5. Answer Pointer

```cpp
int k=n-1;
```

Answer ko **last se fill** karenge because ends compare karke largest square milta hai.

---

## 6. Squares Calculate Karna

```cpp
int left=nums[i]*nums[i];
int right=nums[j]*nums[j];
```

Left aur right elements ke squares nikale.

---

## 7. Compare

```cpp
if(left>right)
```

Agar left square bada:

```cpp
ans[k]=left;
i++;
```

Otherwise:

```cpp
ans[k]=right;
j--;
```

---

## 8. Answer Pointer Move

```cpp
k--;
```

Ek position fill ho gayi, isliye next position par move karte hain.

---

# ⏱️ Time Complexity

```text
O(n)
```

Har element ko ek baar process kar rahe hain.

Sorting dobara nahi karni pad rahi.

---

# 💾 Space Complexity

```text
O(n)
```

Because hum:

```cpp
vector<int> ans(n);
```

extra answer array use kar rahe hain.

---

# ⭐ Important Points

```text
i → left end

j → right end

k → answer ka last index
```

Dono ends ke squares compare karo:

```text
left > right
    ↓
left ko ans[k] me rakho
i++

right >= left
    ↓
right ko ans[k] me rakho
j--
```

Har baar:

```text
k--
```

---

# 🔥 Quick Revision

```text
Sorted Array
     ↓
Square karne par negative values problem create karti hain
     ↓
Largest square either LEFT ya RIGHT end par milega
     ↓
Two Pointers

i = 0
j = n-1
k = n-1

     ↓

leftSquare vs rightSquare

     ↓

Bada square → ans[k]

     ↓

Us side ka pointer move

     ↓

k--

     ↓

Final Sorted Squares
```

---

# 🎯 Pattern To Remember

Is question ki main trick:

```text
Smallest element middle me ho sakta hai,
isliye smallest ko ends se find nahi kar sakte.

Lekin largest absolute value kisi ek end par hogi,
isliye largest square ends se find kar sakte hain.
```

So:

```text
Compare Ends → Pick Larger Square → Fill Answer From Back
```

Ye **Two Pointer from Both Ends** pattern hai.
