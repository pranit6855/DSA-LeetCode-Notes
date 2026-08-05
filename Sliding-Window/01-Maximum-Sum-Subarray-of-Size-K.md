# Maximum Sum Subarray of Size K

## 📌 Problem

Hume ek array `arr` aur integer `k` diya gaya hai.

Hume **exactly `k` size ke continuous subarray** ka maximum sum find karna hai.

---

## 🔹 Example

```text
arr = [1, 4, 2, 10, 2]
k = 3
```

Size `3` ke possible subarrays:

```text
[1, 4, 2]  → Sum = 7

[4, 2, 10] → Sum = 16

[2, 10, 2] → Sum = 14
```

Maximum:

```text
16
```

So:

```text
Output = 16
```

---

# 🧠 What is Sliding Window?

Sliding Window technique tab useful hoti hai jab hume array/string ke:

```text
continuous / contiguous
```

part par kaam karna ho.

Instead of har subarray ka sum starting se calculate karne ke, hum ek **window** maintain karte hain.

Example:

```text
arr = [1, 4, 2, 10, 2]
k = 3
```

First window:

```text
[1, 4, 2] 10 2
 ↑      ↑
low    high
```

Then window ko ek step right slide:

```text
1 [4, 2, 10] 2
   ↑       ↑
  low     high
```

Then:

```text
1 4 [2, 10, 2]
     ↑        ↑
    low      high
```

Window ka size hamesha:

```text
3
```

rahega.

Isliye ise:

```text
Fixed Size Sliding Window
```

kehte hain.

---

# 💡 Approach

Hum do pointers lenge:

```text
low
high
```

Window size `k` hai.

Starting:

```cpp
int low = 0;
int high = k-1;
```

Example:

```text
k = 3

index:  0  1  2  3   4
arr  = [1, 4, 2, 10, 2]
        ↑     ↑
       low   high
```

Window:

```text
index 0 se index 2
```

Size:

```text
high - low + 1

= 2 - 0 + 1

= 3
```

Exactly `k`.

---

# 🔥 Step 1 - First Window Ka Sum

Sabse pehle first `k` elements ka sum calculate karenge.

```cpp
int sum = 0;

for(int i=0; i<k; i++){
    sum += arr[i];
}
```

Example:

```text
arr = [1,4,2,10,2]
k = 3
```

First `3` elements:

```text
1 + 4 + 2
```

So:

```text
sum = 7
```

---

# 🔹 Maximum Initialize Karna

First window already calculate ho chuki hai.

Isliye:

```cpp
int res = sum;
```

Example:

```text
sum = 7
```

So:

```text
res = 7
```

`res` ab tak ka maximum window sum store karega.

---

# ❓ `res = 0` Kyu Nahi?

Suppose array me negative numbers hain:

```text
[-5, -2, -8]
```

Agar:

```text
res = 0
```

rakha, to maximum incorrectly `0` aa sakta hai jabki `0` kisi valid window ka sum hi nahi hai.

Isliye first valid window ka sum:

```cpp
int res = sum;
```

rakhna better hai.

---

# 🔥 Step 2 - Window Slide Karna

First window:

```text
[1,4,2] 10 2
 ↑     ↑
low   high
```

Ab next window chahiye:

```text
1 [4,2,10] 2
   ↑      ↑
  low    high
```

Isliye pointers move:

```cpp
low++;
high++;
```

---

# 🔥 Step 3 - Purana Element Remove + Naya Element Add

Ye Sliding Window ka sabse important concept hai.

First window:

```text
[1,4,2]
```

Sum:

```text
7
```

Next window:

```text
[4,2,10]
```

Hum dobara:

```text
4 + 2 + 10
```

calculate nahi karenge.

Hum previous sum ko reuse karenge.

---

## Purana Element Remove

Old window:

```text
[1,4,2]
 ↑
remove
```

`low++` karne ke baad `low` index `1` par aa gaya.

Old element ka index:

```text
low - 1
```

So remove:

```cpp
arr[low-1]
```

Example:

```text
arr[0] = 1
```

Old sum:

```text
7
```

Remove:

```text
7 - 1 = 6
```

---

## New Element Add

`high++` karne ke baad:

```text
high = 3
```

New element:

```text
arr[3] = 10
```

Add:

```text
6 + 10 = 16
```

So new window sum:

```text
16
```

---

# ⭐ Sliding Window Formula

Tumhare pointer movement ke according:

```cpp
sum = sum - arr[low-1] + arr[high];
```

Meaning:

```text
New Sum
=
Old Sum
-
Outgoing Element
+
Incoming Element
```

Ye fixed sliding window ka main formula hai.

---

# 🔄 Complete Dry Run

Consider:

```text
arr = [1,4,2,10,2]
k = 3
```

Starting:

```text
low = 0
high = 2
```

---

## Window 1

```text
[1,4,2] 10 2
 ↑     ↑
low   high
```

Sum:

```text
1 + 4 + 2 = 7
```

So:

```text
sum = 7
res = 7
```

---

## Window 2

Move pointers:

```cpp
low++;
high++;
```

Now:

```text
low = 1
high = 3
```

Window:

```text
1 [4,2,10] 2
   ↑      ↑
  low    high
```

Update sum:

```cpp
sum = sum - arr[low-1] + arr[high];
```

Values:

```text
sum = 7 - arr[0] + arr[3]

     = 7 - 1 + 10

     = 16
```

Maximum:

```cpp
res = max(sum,res);
```

So:

```text
res = max(16,7)

    = 16
```

---

## Window 3

Again:

```cpp
low++;
high++;
```

Now:

```text
low = 2
high = 4
```

Window:

```text
1 4 [2,10,2]
     ↑      ↑
    low    high
```

Update:

```text
sum = 16 - arr[1] + arr[4]

     = 16 - 4 + 2

     = 14
```

Maximum:

```text
res = max(14,16)

    = 16
```

---

# 🛑 Loop Stop

Now:

```text
high = 4
```

Array size:

```text
n = 5
```

Last valid index:

```text
n-1 = 4
```

Our condition:

```cpp
while(high < n-1)
```

becomes:

```text
4 < 4
```

False.

So loop stop.

Final:

```text
res = 16
```

---

# ❓ `while(high < n-1)` Kyu?

Ye initially confusing lag sakta hai.

Condition ka meaning ye nahi hai ki hum last element ko ignore kar rahe hain.

Iska meaning hai:

```text
Kya high pointer ko ek aur position right move karne ki jagah hai?
```

Suppose:

```text
n = 5
```

Last valid index:

```text
4
```

Agar:

```text
high = 3
```

then:

```text
3 < 4 ✅
```

Hum:

```cpp
high++;
```

karke:

```text
high = 4
```

kar sakte hain.

So last element include ho jayega.

Lekin agar already:

```text
high = 4
```

hai:

```text
4 < 4 ❌
```

Ab aage move nahi kar sakte because next hota:

```text
high = 5
```

jo out of bounds hai.

---

# ❓ `res = max()` Window Update Ke Baad Kyu?

First window ka maximum hum already store kar chuke:

```cpp
int res = sum;
```

So loop ke andar hum **next window** banate hain.

```cpp
low++;
high++;

sum = sum - arr[low-1] + arr[high];
```

Ab new window ready hai.

Isliye uske baad:

```cpp
res = max(sum,res);
```

karte hain.

---

## Flow

```text
First Window
     ↓
res = sum

     ↓

Next Window Banao

     ↓

sum update karo

     ↓

new window ready

     ↓

res = max(sum,res)
```

---

# ❓ Agar `max()` Pehle Check Kare To?

Suppose:

```cpp
while(high<n-1){

    res=max(sum,res);

    low++;
    high++;

    sum=sum-arr[low-1]+arr[high];
}
```

Problem ye hai ki last iteration me:

```text
new last window
```

banegi, lekin loop uske baad terminate ho jayega.

Us last window par:

```cpp
res=max(...)
```

run nahi hoga.

Isliye hum:

```text
pehle new window banate hain
```

then:

```text
maximum update karte hain
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int maxSubarraySum(vector<int>& arr, int k) {

        int low=0;
        int high=k-1;
        int n=arr.size();
        int sum=0;

        // First window ka sum
        for(int i=0;i<k;i++){
            sum+=arr[i];
        }

        int res=sum;

        // Remaining windows
        while(high<n-1){

            low++;
            high++;

            sum=sum-arr[low-1]+arr[high];

            res=max(sum,res);
        }

        return res;
    }
};
```

---

# 🧠 Code Explanation

## Window Start

```cpp
int low=0;
```

Window ka left boundary.

---

## Window End

```cpp
int high=k-1;
```

Window ka right boundary.

Example:

```text
k = 3

low = 0
high = 2

[ x x x ]
  3 elements
```

---

## Array Size

```cpp
int n=arr.size();
```

Array ka total size.

---

## Current Window Sum

```cpp
int sum=0;
```

Current window ka sum store karega.

---

## First Window

```cpp
for(int i=0;i<k;i++){
    sum+=arr[i];
}
```

First `k` elements ka sum calculate karta hai.

---

## Maximum

```cpp
int res=sum;
```

First window ko initial maximum maan liya.

---

## Sliding

```cpp
while(high<n-1)
```

Jab tak next window ban sakti hai, slide karenge.

---

## Move Pointers

```cpp
low++;
high++;
```

Window ko ek position right shift kiya.

---

## Update Sum

```cpp
sum=sum-arr[low-1]+arr[high];
```

Meaning:

```text
Old Sum
-
Old Left Element
+
New Right Element
```

---

## Maximum Update

```cpp
res=max(sum,res);
```

Current window aur previous maximum compare kiya.

---

## Return

```cpp
return res;
```

Maximum sum return.

---

# 🐢 Brute Force vs Sliding Window

Brute force me har window ka sum separately calculate karenge.

Example:

```text
[1,4,2]      → 1+4+2

[4,2,10]     → 4+2+10

[2,10,2]     → 2+10+2
```

Same elements baar-baar add ho rahe hain.

Sliding Window me:

```text
Previous Sum
-
Outgoing
+
Incoming
```

use karte hain.

Example:

```text
7
↓
7 - 1 + 10
↓
16
↓
16 - 4 + 2
↓
14
```

Isliye efficient hai.

---

# ⏱️ Time Complexity

First window calculate:

```text
O(k)
```

Remaining array traverse:

```text
O(n-k)
```

Total:

```text
O(k) + O(n-k)
```

Simplify:

```text
O(n)
```

---

# 💾 Space Complexity

Extra variables:

```text
low
high
n
sum
res
```

Koi extra array/vector nahi.

So:

```text
O(1)
```

---

# ⭐ Fixed Size Sliding Window Pattern

Is question se ye general pattern yaad rakho:

```cpp
int low=0;
int high=k-1;

int sum=0;

for(int i=0;i<k;i++){
    sum+=arr[i];
}

int res=sum;

while(high<n-1){

    low++;
    high++;

    sum=sum-arr[low-1]+arr[high];

    // current window par required operation
}
```

Question ke according last operation change ho sakta hai.

For example:

```text
Maximum Sum
→ max()

Minimum Sum
→ min()

Average
→ sum/k

Count / condition
→ condition check
```

Lekin **window slide karne ka core idea same rahega**.

---

# 🔥 Quick Revision

```text
FIXED SIZE SLIDING WINDOW

k = window size

        ↓

low = 0
high = k-1

        ↓

First k elements ka sum

        ↓

res = sum

        ↓

Window ko right slide karo

low++
high++

        ↓

OLD element remove

arr[low-1]

        ↓

NEW element add

arr[high]

        ↓

sum = sum - arr[low-1] + arr[high]

        ↓

res = max(res,sum)

        ↓

Repeat
```

---

# 🎯 Main Formula

Sabse important:

```text
NEW WINDOW SUM
=
OLD WINDOW SUM
-
OUTGOING ELEMENT
+
INCOMING ELEMENT
```

Tumhare code me:

```cpp
sum = sum - arr[low-1] + arr[high];
```

---

# 🧠 One-Line Revision

```text
First window ka sum nikalo → window slide karo → purana left hatao → naya right add karo → maximum update karo.
```

---

# 📊 Complexity

```text
Time Complexity  → O(n)

Space Complexity → O(1)
```
