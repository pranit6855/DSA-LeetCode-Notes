# LeetCode 2824 - Count Pairs Whose Sum is Less than Target

## 📌 Problem

Hume ek integer array `nums` aur ek integer `target` diya gaya hai.

Hume count karna hai ki kitne pairs `(i, j)` aise hain jinke liye:

```text
i < j
```

aur:

```text
nums[i] + nums[j] < target
```

Matlab hume aise **2 different elements** choose karne hain jinka sum `target` se strictly smaller ho.

Finally hume total valid pairs ka **count** return karna hai.

---

# 🔹 Example

```text
nums = [-1,1,2,3,1]
target = 2
```

Valid pairs check karne par total answer:

```text
3
```

So:

```text
Output = 3
```

---

# 💡 Approach - Sorting + Two Pointers

Is problem ko efficiently solve karne ke liye:

```text
1. Array ko sort karenge

2. Ek left pointer starting par rakhenge

3. Ek right pointer ending par rakhenge

4. nums[left] + nums[right] calculate karenge

5. Sum target se chhota hai to multiple pairs ek saath count karenge

6. Sum bada/equal hai to right pointer ko peeche karenge
```

Sabse important trick:

```cpp
count += right-left;
```

hai.

Isko detail me neeche samjhenge.

---

# 🔹 Step 1 - Array Sort Karna

Code:

```cpp
sort(nums.begin(),nums.end());
```

Suppose:

```text
nums = [-1,1,2,3,1]
```

Sort karne ke baad:

```text
[-1,1,1,2,3]
```

Sorting bahut important hai because ab values increasing order me hain:

```text
small --------------------> large
```

Isliye hume pata hai:

```text
left++  → bigger value milegi

right-- → smaller value milegi
```

Isi property ki wajah se Two Pointer approach work karti hai.

---

# 🔹 Step 2 - Two Pointers

Code:

```cpp
int left=0;
int right=n-1;
```

`left` array ke starting element ko point karega.

`right` array ke last element ko point karega.

Example:

```text
[-1,1,1,2,3]
 ↑         ↑
left     right
```

---

# 🔹 Step 3 - Count Variable

```cpp
int count=0;
```

Ye total valid pairs ko count karega.

Starting me:

```text
count = 0
```

because abhi tak koi pair check nahi kiya.

---

# 🔹 Step 4 - Loop

```cpp
while(left<right)
```

Jab tak:

```text
left < right
```

hai, tab tak do different elements available hain.

Har iteration me:

```cpp
int sum=nums[left]+nums[right];
```

calculate karenge.

---

# ❓ `left < right` Kyu?

Pair me hume **2 different indices** chahiye.

Agar:

```text
left == right
```

to dono pointers same element ko point karenge.

Example:

```text
[1,2,3]
   ↑
left,right
```

Ab hum same element ko twice use kar rahe honge.

Isliye:

```cpp
while(left<right)
```

use karte hain.

---

# 🔍 Main Logic

Ab do main cases hain:

```text
Case 1:

sum < target
```

ya:

```text
Case 2:

sum >= target
```

---

# ✅ Case 1 - Sum Target Se Chhota Hai

Condition:

```cpp
if(sum<target)
```

Agar:

```text
nums[left] + nums[right] < target
```

hai, to current pair valid hai.

Lekin yahan sirf **ek pair count nahi karna**.

Sorted array ki wajah se hum ek saath multiple pairs count kar sakte hain.

Isi ke liye:

```cpp
count += right-left;
```

use karte hain.

---

# 🔥 Sabse Important Logic - `count += right-left`

Is question ka main concept yahi hai.

Suppose sorted array:

```text
nums = [-2,0,1,3]
target = 5
```

Pointers:

```text
[-2,0,1,3]
 ↑       ↑
left   right
```

Current:

```text
nums[left]  = -2
nums[right] = 3
```

Sum:

```text
-2 + 3 = 1
```

And:

```text
1 < 5
```

So pair:

```text
(-2,3)
```

valid hai.

---

# 🤔 Lekin Sirf `(-2,3)` Hi Valid Nahi Hai

Array sorted hai:

```text
[-2,0,1,3]
```

`3` current range ka sabse bada element hai.

Agar:

```text
-2 + 3 < 5
```

hai, to `-2` ke saath `3` se chhote elements ka sum bhi definitely target se chhota hoga.

So:

```text
-2 + 0 = -2 < 5 ✅

-2 + 1 = -1 < 5 ✅

-2 + 3 = 1 < 5 ✅
```

Total:

```text
3 pairs
```

---

# 🔢 Ye `3` Kaise Mila?

Current indices:

```text
left = 0
right = 3
```

Left ke saath possible partners:

```text
left+1
left+2
...
right
```

Matlab:

```text
index 1
index 2
index 3
```

Total:

```text
3
```

Formula:

```text
right - left

= 3 - 0

= 3
```

Isliye:

```cpp
count += right-left;
```

---

# 🔥 General Formula

Suppose:

```text
left = 2
right = 6
```

`nums[left]` ke saath valid partners honge:

```text
3
4
5
6
```

Total:

```text
4 pairs
```

Formula:

```text
right-left

= 6-2

= 4
```

So:

```cpp
count += right-left;
```

---

# ❓ `right-left+1` Kyu Nahi?

Because `left` khud ke saath pair nahi bana sakta.

Suppose:

```text
left = 0
right = 3
```

Elements available:

```text
index 0
index 1
index 2
index 3
```

Lekin index `0` ko khud ke saath pair nahi karna.

Valid partners:

```text
1,2,3
```

Total:

```text
3
```

And:

```text
right-left = 3
```

Isliye:

```cpp
count += right-left;
```

correct hai.

---

# 🔹 Valid Pairs Count Karne Ke Baad `left++`

Code:

```cpp
left++;
```

Current `left` ke saath saare possible valid pairs already count ho gaye.

Example:

```text
[-2,0,1,3]
 ↑       ↑
 L       R
```

Humne already count kar liya:

```text
(-2,0)
(-2,1)
(-2,3)
```

Ab `-2` ke liye kuch aur check karne ki zarurat nahi.

Isliye:

```cpp
left++;
```

Ab next element ko process karenge.

---

# ❌ Case 2 - Sum Target Se Chhota Nahi Hai

Agar:

```cpp
sum<target
```

false hai, matlab:

```text
sum >= target
```

Example:

```text
nums[left] + nums[right] >= target
```

Current sum bahut bada hai.

Hume sum ko **chhota** karna hai.

Array sorted hai.

So:

```cpp
right--;
```

---

# ❓ `right--` Kyu?

Sorted array:

```text
small --------------------> large
```

`right` currently large value ko point kar raha hai.

Agar sum:

```text
target se bada/equal
```

hai, to hume smaller value chahiye.

So right pointer ko left side move karte hain:

```cpp
right--;
```

---

# 🔄 Complete Dry Run

Consider:

```text
nums = [-1,1,2,3,1]
target = 2
```

---

## Step 1 - Sort

After sorting:

```text
[-1,1,1,2,3]
```

Starting:

```text
left = 0
right = 4
count = 0
```

Pointers:

```text
[-1,1,1,2,3]
 ↑         ↑
 L         R
```

---

## Step 2

Calculate:

```text
sum = nums[left] + nums[right]

    = -1 + 3

    = 2
```

Target:

```text
2
```

Condition:

```text
sum < target

2 < 2 ❌
```

Important:

Problem me condition hai:

```text
sum < target
```

not:

```text
sum <= target
```

So `2` valid nahi hai.

Execute:

```cpp
right--;
```

Now:

```text
right = 3
```

---

## Step 3

Pointers:

```text
[-1,1,1,2,3]
 ↑       ↑
 L       R
```

Values:

```text
nums[left] = -1
nums[right] = 2
```

Sum:

```text
-1 + 2 = 1
```

Check:

```text
1 < 2 ✅
```

So current `left = -1` ke saath `right` tak saare pairs valid hain.

Possible pairs:

```text
(-1,1)
(-1,1)
(-1,2)
```

Total:

```text
right-left

= 3-0

= 3
```

So:

```cpp
count += right-left;
```

Now:

```text
count = 0 + 3
      = 3
```

Then:

```cpp
left++;
```

Now:

```text
left = 1
```

---

## Step 4

Pointers:

```text
[-1,1,1,2,3]
    ↑    ↑
    L    R
```

Current values:

```text
nums[left] = 1
nums[right] = 2
```

Sum:

```text
1 + 2 = 3
```

Check:

```text
3 < 2 ❌
```

Sum bada hai.

So:

```cpp
right--;
```

Now:

```text
right = 2
```

---

## Step 5

Pointers:

```text
[-1,1,1,2,3]
    ↑ ↑
    L R
```

Sum:

```text
1 + 1 = 2
```

Check:

```text
2 < 2 ❌
```

Again valid nahi hai because condition strictly `<` hai.

So:

```cpp
right--;
```

Now:

```text
right = 1
```

---

# 🛑 Loop Stop

Now:

```text
left = 1
right = 1
```

Condition:

```cpp
left<right
```

becomes:

```text
1 < 1 ❌
```

Loop stop.

Final:

```text
count = 3
```

Return:

```cpp
return count;
```

Output:

```text
3
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int countPairs(vector<int>& nums, int target) {
        int n=nums.size();
        sort(nums.begin(),nums.end());

        int left=0;
        int right=n-1;
        int count=0;

        while(left<right){

            int sum=nums[left]+nums[right];

            if(sum<target){
                count+=right-left;
                left++;
            }

            else{
                right--;
            }
        }

        return count;
    }
};
```

---

# 🧠 Code Explanation

## 1. Array Size

```cpp
int n=nums.size();
```

Array ka total size `n` me store kiya.

---

## 2. Sort Array

```cpp
sort(nums.begin(),nums.end());
```

Array ko ascending order me sort kiya.

Sorting Two Pointer logic ke liye important hai.

---

## 3. Left Pointer

```cpp
int left=0;
```

Starting/smallest element ko point karega.

---

## 4. Right Pointer

```cpp
int right=n-1;
```

Last/largest element ko point karega.

---

## 5. Count

```cpp
int count=0;
```

Total valid pairs store karega.

---

## 6. Main Loop

```cpp
while(left<right)
```

Jab tak 2 different elements available hain, loop chalega.

---

## 7. Current Sum

```cpp
int sum=nums[left]+nums[right];
```

Current left aur right elements ka sum calculate kiya.

---

## 8. Sum Target Se Chhota

```cpp
if(sum<target)
```

Agar largest available partner `right` ke saath bhi sum target se chhota hai, to:

```text
left ke saath left+1 se right tak
saare pairs valid hain.
```

So:

```cpp
count+=right-left;
```

---

## 9. Left Move

```cpp
left++;
```

Current `left` ke saare valid pairs count ho gaye.

Ab next left element check karenge.

---

## 10. Sum Bada Ya Equal

```cpp
else{
    right--;
}
```

Agar:

```text
sum >= target
```

to sum ko chhota karna hai.

Isliye right pointer ko smaller value ki taraf move karte hain.

---

# ⭐ Most Important Concept

Is problem me ye line sabse important hai:

```cpp
count += right-left;
```

Reason:

Agar sorted array me:

```text
nums[left] + nums[right] < target
```

hai, aur `right` current range ka largest element hai, to:

```text
nums[left] + nums[left+1]
nums[left] + nums[left+2]
nums[left] + nums[left+3]
...
nums[left] + nums[right]
```

**sab target se chhote honge.**

Total partners:

```text
right-left
```

Isliye saare pairs ek saath count kar sakte hain.

---

# ❓ Why Not Just `count++`?

Agar hum:

```cpp
count++;
```

karte, to sirf current:

```text
(left,right)
```

pair count hota.

Lekin hume already pata hai ki current `left` ke saath beech ke saare elements bhi valid hain.

Example:

```text
[-2,0,1,3]
 ↑       ↑
 L       R
```

Agar:

```text
-2 + 3 < target
```

hai, to:

```text
(-2,0)
(-2,1)
(-2,3)
```

teeno valid hain.

So:

```cpp
count++;
```

sirf `1` add karega ❌

Whereas:

```cpp
count += right-left;
```

`3` add karega ✅

---

# ❓ `left++` vs `right--` Kaise Yaad Rakhein?

Simple rule:

### Sum target se chhota hai

```text
sum < target
```

Current `left` ke saare pairs count ho gaye.

So:

```text
left++
```

---

### Sum target se bada/equal hai

```text
sum >= target
```

Sum chhota karna hai.

So:

```text
right--
```

---

# 🔥 Quick Revision

```text
nums
 ↓
SORT
 ↓

left = 0
right = n-1
count = 0

 ↓

while(left < right)

 ↓

sum = nums[left] + nums[right]

 ↓
          ┌─────────────────────┐
          │                     │
    sum < target          sum >= target
          │                     │
          ↓                     ↓
count += right-left          right--
          ↓
        left++
```

---

# 🎯 Pattern To Remember

Is problem ka pattern:

```text
Sorted Array
+
Two Pointers
+
Count Multiple Pairs At Once
```

Main observation:

```text
Agar largest remaining element ke saath sum
target se chhota hai,

to usse chhote saare elements ke saath bhi
sum target se chhota hi hoga.
```

Isliye:

```cpp
if(sum<target){
    count+=right-left;
    left++;
}
else{
    right--;
}
```

---

# 🆚 Two Sum II vs This Problem

## Two Sum II

Goal:

```text
sum == target
```

Movement:

```text
sum < target → left++

sum > target → right--
```

---

## Count Pairs Less Than Target

Goal:

```text
sum < target
```

Difference ye hai ki valid sum milne par sirf ek answer nahi milta.

Ek saath:

```text
right-left
```

valid pairs mil jaate hain.

So:

```cpp
count += right-left;
```

---

# ⏱️ Time Complexity

Sorting:

```text
O(n log n)
```

Two Pointer traversal:

```text
O(n)
```

Total:

```text
O(n log n) + O(n)
```

Dominant term:

```text
O(n log n)
```

So final:

```text
Time Complexity = O(n log n)
```

---

# 💾 Space Complexity

Two Pointer logic me hum sirf:

```text
n
left
right
count
sum
```

variables use kar rahe hain.

So Two Pointer logic:

```text
O(1)
```

extra space use karta hai.

> Note: `std::sort()` internally implementation-dependent stack space use kar sakta hai.

---

# 📊 Complexity Summary

```text
Time Complexity  → O(n log n)

Two Pointer Pass → O(n)

Extra Space      → O(1) for our logic
```

---

# 🧠 One-Line Revision

```text
Sort → left/right → sum < target hua to right-left pairs count karo → warna right ko peeche karo.
```

Sabse important line:

```cpp
count += right-left;
```

Aur iska reason:

```text
nums[left] + nums[right] < target

→ right sabse bada available element hai

→ left ke saath usse chhote saare elements bhi valid

→ total pairs = right-left
```
