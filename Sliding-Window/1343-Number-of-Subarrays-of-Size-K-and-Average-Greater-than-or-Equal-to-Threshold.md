# LeetCode 1343 - Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold

## 📌 Problem

Hume ek integer array `arr`, integer `k` aur integer `threshold` diya gaya hai.

Hume count karna hai ki:

```text
Exactly k size ke kitne continuous subarrays ka average >= threshold hai.
```

Matlab har `k` size ki window ko check karenge.

Agar:

```text
average >= threshold
```

hai, to:

```text
count++
```

---

# 🔹 Example

```text
arr = [2,2,2,2,5,5,5,8]
k = 3
threshold = 4
```

Since:

```text
k = 3
```

har window me exactly `3` elements honge.

Possible windows:

```text
[2,2,2] → average = 2 ❌
[2,2,2] → average = 2 ❌
[2,2,5] → average = 3 ❌
[2,5,5] → average = 4 ✅
[5,5,5] → average = 5 ✅
[5,5,8] → average = 6 ✅
```

Total valid windows:

```text
3
```

So:

```text
Output = 3
```

---

# 🧠 Approach

Ye **Fixed Size Sliding Window** problem hai.

Kyunki question already bol raha hai:

```text
Subarray of size k
```

Matlab window ka size hamesha:

```text
k
```

rahega.

Hum use karenge:

```text
low
high
sum
count
```

---

# 🔥 Variables

```cpp
int n=arr.size();
int high=k-1;
int low=0;
int sum=0;
int count=0;
```

Meaning:

```text
n     → array ka size

low   → current window ka left index

high  → current window ka right index

sum   → current window ka sum

count → valid windows ki total count
```

---

# 🔥 Why `high = k-1`?

First window ka size exactly `k` hona chahiye.

Window size formula:

```text
high - low + 1
```

Initially:

```text
low = 0
high = k-1
```

So:

```text
(k-1) - 0 + 1

= k
```

Therefore first window ka size exactly `k` hai.

Example:

```text
k = 3

low = 0
high = 2
```

Array:

```text
[2  2  2] 2  5  5  5  8
 ↑     ↑
low   high
```

Window size:

```text
2 - 0 + 1 = 3
```

---

# 🔥 Step 1 - First Window Ka Sum

First `k` elements ka sum calculate karenge:

```cpp
for(int i=0;i<k;i++){
    sum+=arr[i];
}
```

Example:

```text
arr = [2,2,2,2,5,5,5,8]
k = 3
```

First window:

```text
[2,2,2]
```

Sum:

```text
2 + 2 + 2 = 6
```

So:

```text
sum = 6
```

---

# 🔥 Step 2 - First Window Check

Question average ke baare me pooch raha hai.

Average formula:

```text
Average = Sum / Number of Elements
```

Window me exactly `k` elements hain.

So:

```text
Average = sum / k
```

Check:

```cpp
if((sum/k)>=threshold){
    count++;
}
```

Example:

```text
sum = 6
k = 3
threshold = 4
```

Average:

```text
6 / 3 = 2
```

Check:

```text
2 >= 4 ❌
```

So:

```text
count = 0
```

---

# ❓ First Window Ko While Se Pehle Check Kyu Karte Hain?

Ye important hai.

First window humne:

```cpp
for(int i=0;i<k;i++){
    sum+=arr[i];
}
```

se banayi hai.

Abhi `while` start nahi hua.

So first window:

```text
[2,2,2]
```

ko separately check karna padega:

```cpp
if((sum/k)>=threshold){
    count++;
}
```

Agar ye check nahi karenge, first window answer se miss ho jayegi.

---

# 🔥 Step 3 - Remaining Windows

Ab first window process ho chuki hai.

Remaining windows ke liye:

```cpp
while(high<n-1)
```

use karenge.

Why?

Because agar:

```text
high = n-1
```

ho gaya, to `high` already last element par hai.

Uske baad koi next window possible nahi hai.

---

# 🔥 Step 4 - Window Slide

```cpp
low++;
high++;
```

Example:

Before:

```text
[2 2 2] 2 5 5 5 8
 ↑   ↑
low high
```

After:

```text
2 [2 2 2] 5 5 5 8
   ↑   ↑
  low high
```

Dono pointers ek step move hue.

Window size still:

```text
3
```

hai.

Isi liye ye:

```text
Fixed Size Sliding Window
```

hai.

---

# 🔥 Step 5 - New Window Ka Sum

Hum har new window ka sum starting se calculate nahi karenge.

Instead:

```cpp
sum=sum-arr[low-1]+arr[high];
```

Ye Sliding Window ka main formula hai.

Meaning:

```text
New Sum
=
Old Sum
- Outgoing Element
+ Incoming Element
```

---

# 🔹 Example

Current window:

```text
[2,2,2]
```

Current:

```text
sum = 6
```

Window slide hui:

```text
2 [2,2,2]
```

Old element bahar gaya:

```text
2
```

New element andar aaya:

```text
2
```

So:

```text
new sum = 6 - 2 + 2

         = 6
```

Code:

```cpp
sum=sum-arr[low-1]+arr[high];
```

---

# ❓ `arr[low-1]` Kyu?

Hum pehle:

```cpp
low++;
high++;
```

karte hain.

Suppose initially:

```text
low = 0
high = 2
```

After increment:

```text
low = 1
high = 3
```

Window se jo element bahar gaya tha uska old index:

```text
0
```

Current `low`:

```text
1
```

Therefore:

```text
low - 1 = 0
```

So outgoing element:

```cpp
arr[low-1]
```

hai.

---

# ❓ `arr[high]` Kyu?

`high++` ke baad `high` new incoming element ko point karta hai.

Example:

```text
Before:

high = 2
```

After:

```text
high = 3
```

Index `3` wala element new window me enter hua.

Therefore:

```cpp
arr[high]
```

incoming element hai.

---

# 🔥 Step 6 - New Window Check

New window ka sum mil gaya.

Ab again:

```cpp
if((sum/k)>=threshold){
    count++;
}
```

check karenge.

Agar current window ka average threshold se greater than ya equal hai:

```text
valid window
```

So:

```text
count++
```

---

# 🔄 Complete Dry Run

Given:

```text
arr = [2,2,2,2,5,5,5,8]

k = 3
threshold = 4
```

---

## Window 1

```text
[2,2,2]
```

Sum:

```text
6
```

Average:

```text
6/3 = 2
```

Check:

```text
2 >= 4 ❌
```

Count:

```text
0
```

---

## Window 2

```text
[2,2,2]
```

Outgoing:

```text
2
```

Incoming:

```text
2
```

New sum:

```text
6 - 2 + 2 = 6
```

Average:

```text
6/3 = 2
```

Invalid ❌

```text
count = 0
```

---

## Window 3

```text
[2,2,5]
```

Old sum:

```text
6
```

Outgoing:

```text
2
```

Incoming:

```text
5
```

New sum:

```text
6 - 2 + 5 = 9
```

Average:

```text
9/3 = 3
```

Check:

```text
3 >= 4 ❌
```

Count:

```text
0
```

---

## Window 4

```text
[2,5,5]
```

Old sum:

```text
9
```

Outgoing:

```text
2
```

Incoming:

```text
5
```

New sum:

```text
9 - 2 + 5 = 12
```

Average:

```text
12/3 = 4
```

Check:

```text
4 >= 4 ✅
```

So:

```text
count = 1
```

---

## Window 5

```text
[5,5,5]
```

Sum:

```text
15
```

Average:

```text
15/3 = 5
```

Check:

```text
5 >= 4 ✅
```

So:

```text
count = 2
```

---

## Window 6

```text
[5,5,8]
```

Sum:

```text
18
```

Average:

```text
18/3 = 6
```

Check:

```text
6 >= 4 ✅
```

So:

```text
count = 3
```

Final:

```text
3
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int numOfSubarrays(vector<int>& arr, int k, int threshold) {

        int n=arr.size();
        int high=k-1;
        int low=0;
        int sum=0;
        int count=0;

        // First window
        for(int i=0;i<k;i++){
            sum+=arr[i];
        }

        // Check first window
        if((sum/k)>=threshold){
            count++;
        }

        // Remaining windows
        while(high<n-1){

            low++;
            high++;

            sum=sum-arr[low-1]+arr[high];

            if((sum/k)>=threshold){
                count++;
            }
        }

        return count;
    }
};
```

---

# 🧠 Code Line-by-Line

## Array Size

```cpp
int n=arr.size();
```

Array ki total length.

---

## Right Pointer

```cpp
int high=k-1;
```

First `k` size window ka last index.

---

## Left Pointer

```cpp
int low=0;
```

Window ka starting index.

---

## Current Sum

```cpp
int sum=0;
```

Current `k` size window ka sum.

---

## Valid Window Count

```cpp
int count=0;
```

Kitni windows ka:

```text
average >= threshold
```

hai, wo store karega.

---

## First Window Sum

```cpp
for(int i=0;i<k;i++){
    sum+=arr[i];
}
```

First `k` elements add.

---

## First Window Check

```cpp
if((sum/k)>=threshold){
    count++;
}
```

First window valid hui to count increase.

---

## Slide

```cpp
low++;
high++;
```

Window ko ek position right move.

---

## Sum Update

```cpp
sum=sum-arr[low-1]+arr[high];
```

```text
old sum
- outgoing
+ incoming
```

---

## Check New Window

```cpp
if((sum/k)>=threshold){
    count++;
}
```

Current window valid hui to count increase.

---

## Final Answer

```cpp
return count;
```

Total valid windows return.

---

# ⭐ Better Condition - Division Avoid Kar Sakte Hain

Hum check kar rahe hain:

```text
sum / k >= threshold
```

Mathematically:

```text
sum >= threshold * k
```

So instead of:

```cpp
if((sum/k)>=threshold){
    count++;
}
```

hum likh sakte hain:

```cpp
if(sum >= threshold*k){
    count++;
}
```

Ye better hai because integer division avoid hoti hai.

So recommended condition:

```cpp
if(sum >= threshold*k){
    count++;
}
```

---

# 🔥 LC 643 Se Connection

### LC 643

Question:

```text
Maximum average find karo
```

Pattern:

```text
First window sum
      ↓
max_sum initialize
      ↓
slide
      ↓
old remove + new add
      ↓
max_sum update
```

### LC 1343

Question:

```text
Kitni windows ka average >= threshold hai?
```

Pattern:

```text
First window sum
      ↓
valid? → count++
      ↓
slide
      ↓
old remove + new add
      ↓
valid? → count++
```

Sliding logic exactly same hai.

Bas answer maintain karne ka method different hai.

---

# 🔥 LC 1456 Se Connection

LC 1456 me:

```text
Outgoing vowel → count--
Incoming vowel → count++
```

LC 1343 me:

```text
Outgoing number → sum se minus
Incoming number → sum me plus
```

Same fixed-window concept:

```text
REMOVE OLD
+
ADD NEW
```

---

# ⭐ Fixed Size Sliding Window Template

Is type ke questions me basic pattern:

```cpp
// First window
for(int i=0;i<k;i++){

    // process
}

// first window answer/check

while(high<n-1){

    low++;
    high++;

    // outgoing remove

    // incoming add

    // answer/check
}
```

---

# ⏱️ Time Complexity

First window:

```text
O(k)
```

Remaining sliding:

```text
O(n-k)
```

Total:

```text
O(k + n-k)

= O(n)
```

So:

```text
Time Complexity = O(n)
```

---

# 💾 Space Complexity

Extra variables:

```text
low
high
sum
count
```

Koi map/array extra use nahi kiya.

So:

```text
Space Complexity = O(1)
```

---

# 📊 Complexity Summary

```text
Time Complexity  → O(n)

Space Complexity → O(1)
```

---

# ⚠️ Common Mistakes

## 1. First Window Check Bhool Jana

Wrong:

```cpp
for(int i=0;i<k;i++){
    sum+=arr[i];
}

while(...){
```

First window answer se miss ho jayegi.

Correct:

```cpp
for(int i=0;i<k;i++){
    sum+=arr[i];
}

if((sum/k)>=threshold){
    count++;
}

while(...){
```

---

## 2. `if` Parentheses

Wrong:

```cpp
if (sum/k)>=threshold){
```

Correct:

```cpp
if((sum/k)>=threshold){
```

---

## 3. Har Window Ka Sum Dobara Calculate Karna

Unnecessary:

```text
[2,2,2] → complete sum

[2,2,5] → complete sum again

[2,5,5] → complete sum again
```

Sliding Window me:

```text
old sum - outgoing + incoming
```

use karo.

---

# 🔥 Quick Revision

```text
low = 0
high = k-1
      ↓
First k elements ka sum
      ↓
average >= threshold?
      ↓
YES → count++
      ↓
low++, high++
      ↓
outgoing element minus
      ↓
incoming element plus
      ↓
average >= threshold?
      ↓
YES → count++
      ↓
repeat
```

---

# 🧠 One-Line Revision

```text
First k elements ka sum nikalo, valid ho to count karo, phir har slide me old element minus aur new element add karke har valid window ka count badhao.
```

---

# ⭐ Most Important Part

```cpp
if((sum/k)>=threshold){
    count++;
}

while(high<n-1){

    low++;
    high++;

    sum=sum-arr[low-1]+arr[high];

    if((sum/k)>=threshold){
        count++;
    }
}
```

First `if`:

```text
FIRST WINDOW ko check karta hai
```

While ke andar wala `if`:

```text
HAR NEXT WINDOW ko check karta hai
```

Ye difference yaad rakhna.
