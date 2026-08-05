# LeetCode 209 - Minimum Size Subarray Sum

## 📌 Problem

Hume:

```text
target
```

aur ek positive integers ka array:

```text
nums
```

diya gaya hai.

Hume **minimum length ka continuous subarray** find karna hai jiska sum:

```text
>= target
```

ho.

Agar aisa koi subarray exist nahi karta, to:

```text
0
```

return karna hai.

---

# 🔹 Example

```text
target = 7

nums = [2,3,1,2,4,3]
```

Possible valid subarrays:

```text
[2,3,1,2] → sum = 8 → length = 4

[3,1,2,4] → sum = 10 → length = 4

[4,3] → sum = 7 → length = 2
```

Minimum length:

```text
2
```

So:

```text
Output = 2
```

---

# 🧠 Sliding Window Type

Pichle question me humne **Fixed Size Sliding Window** dekha tha.

Usme window size fixed tha:

```text
k
```

Example:

```text
size = 3

[1,2,3]
  [2,3,4]
    [3,4,5]
```

Har window ka size same tha.

---

## Is Question Me Window Size Fixed Nahi Hai

LC 209 me hume minimum length find karni hai.

Window kabhi:

```text
4 elements
```

ki ho sakti hai,

kabhi:

```text
3 elements
```

ki,

aur kabhi:

```text
2 elements
```

ki.

Isliye ye:

```text
Variable Size Sliding Window
```

hai.

---

# 💡 Main Idea

Hum 2 pointers use karenge:

```text
low
high
```

`high` ka kaam:

```text
window ko expand karna
```

`low` ka kaam:

```text
window ko shrink karna
```

Simple pattern:

```text
high++
→ EXPAND

low++
→ SHRINK
```

---

# 🔹 Initial Variables

```cpp
int low=0;
int n=nums.size();
int high=0;
int sum=0;
int min_len=INT_MAX;
```

Meaning:

```text
low
→ window ka starting point

high
→ window ka ending point

sum
→ current window ka sum

min_len
→ ab tak mili minimum valid length
```

---

# 🔹 Starting Position

Example:

```text
nums = [2,3,1,2,4,3]
```

Initially:

```text
        high
         ↓
        [2,3,1,2,4,3]
         ↑
        low
```

So:

```text
low = 0
high = 0
sum = 0
```

---

# 🔥 Step 1 - Window Expand Karna

Outer loop:

```cpp
while(high<n)
```

Har iteration me current `high` element ko window me add karenge:

```cpp
sum+=nums[high];
```

Example:

```text
nums = [2,3,1,2,4,3]
```

Initially:

```text
sum = 0
```

Add `2`:

```text
sum = 2
```

Then `high++`.

Next:

```text
sum = 2 + 3
    = 5
```

Then:

```text
sum = 5 + 1
    = 6
```

Then:

```text
sum = 6 + 2
    = 8
```

Ab:

```text
sum >= target
```

because:

```text
8 >= 7
```

Ab window valid ho gayi.

---

# 🔥 Step 2 - Valid Window Milne Par Shrink Karo

Condition:

```cpp
while(sum>=target)
```

Jaise hi current window ka sum target ke equal ya greater hota hai, hume ek valid subarray mil gaya.

Lekin question sirf valid subarray nahi maang raha.

Question maang raha hai:

```text
MINIMUM LENGTH
```

Isliye valid window milte hi rukenge nahi.

Window ko left side se shrink karke dekhenge:

```text
kya aur chhoti valid window mil sakti hai?
```

---

# 🔹 Current Length Calculate Karna

Window ka start:

```text
low
```

Window ka end:

```text
high
```

Length:

```cpp
int len=high-low+1;
```

Formula:

```text
Window Length = high - low + 1
```

---

# ❓ `+1` Kyu?

Suppose:

```text
low = 0
high = 3
```

Indices:

```text
0 1 2 3
```

Total elements:

```text
4
```

Lekin:

```text
high-low

= 3-0

= 3
```

Isliye:

```text
high-low+1

= 3-0+1

= 4
```

Correct length.

---

# 🔹 Minimum Length Update

Current window ki length:

```cpp
int len=high-low+1;
```

Ab previous minimum se compare:

```cpp
min_len=min(len,min_len);
```

Example:

```text
min_len = 4
len = 3
```

Then:

```text
min(3,4) = 3
```

So:

```text
min_len = 3
```

---

# 🔥 Step 3 - Window Shrink Karna

Ab leftmost element ko remove karenge:

```cpp
sum=sum-nums[low];
```

Then:

```cpp
low++;
```

Matlab:

```text
left element remove
        ↓
low ko right move karo
```

Window chhoti ho jayegi.

---

# 🔄 Complete Dry Run

Consider:

```text
target = 7

nums = [2,3,1,2,4,3]
```

Starting:

```text
low = 0
high = 0
sum = 0

min_len = INT_MAX
```

---

# Step 1

Add:

```text
nums[0] = 2
```

So:

```text
sum = 2
```

Window:

```text
[2] 3 1 2 4 3
 ↑
L,H
```

Check:

```text
2 >= 7 ❌
```

Cannot shrink.

So:

```text
high++
```

---

# Step 2

Now:

```text
high = 1
```

Add:

```text
nums[1] = 3
```

Sum:

```text
2 + 3 = 5
```

Window:

```text
[2,3] 1 2 4 3
 ↑ ↑
 L H
```

Check:

```text
5 >= 7 ❌
```

So:

```text
high++
```

---

# Step 3

Now:

```text
high = 2
```

Add:

```text
nums[2] = 1
```

Sum:

```text
5 + 1 = 6
```

Window:

```text
[2,3,1] 2 4 3
 ↑   ↑
 L   H
```

Check:

```text
6 >= 7 ❌
```

So:

```text
high++
```

---

# Step 4

Now:

```text
high = 3
```

Add:

```text
nums[3] = 2
```

Sum:

```text
6 + 2 = 8
```

Window:

```text
[2,3,1,2] 4 3
 ↑     ↑
 L     H
```

Check:

```text
8 >= 7 ✅
```

Ab inner `while` start hoga.

---

# 🔥 First Valid Window

Current window:

```text
[2,3,1,2]
```

Sum:

```text
8
```

Length:

```text
high-low+1

= 3-0+1

= 4
```

So:

```text
len = 4
```

Update:

```text
min_len = min(4, INT_MAX)
```

So:

```text
min_len = 4
```

---

# 🔹 Shrink

Remove:

```text
nums[low] = 2
```

So:

```text
sum = 8 - 2
    = 6
```

Then:

```text
low = 1
```

Window:

```text
2 [3,1,2] 4 3
   ↑   ↑
   L   H
```

Check:

```text
6 >= 7 ❌
```

Inner while stop.

---

# Step 5 - Expand Again

Outer loop continues.

```text
high++
```

Now:

```text
high = 4
```

Add:

```text
nums[4] = 4
```

Current sum:

```text
6 + 4 = 10
```

Window:

```text
2 [3,1,2,4] 3
   ↑     ↑
   L     H
```

Check:

```text
10 >= 7 ✅
```

Shrink start.

---

## Current Window

```text
[3,1,2,4]
```

Length:

```text
4-1+1 = 4
```

So:

```text
len = 4
```

Update:

```text
min_len = min(4,4)
        = 4
```

Remove `3`:

```text
sum = 10 - 3
    = 7
```

Move:

```text
low = 2
```

---

# 🔥 Still Valid!

Now window:

```text
2 3 [1,2,4] 3
     ↑   ↑
     L   H
```

Sum:

```text
7
```

Condition:

```text
7 >= 7 ✅
```

Still valid.

Isliye inner `while` dobara chalega.

Length:

```text
4-2+1 = 3
```

So:

```text
len = 3
```

Update:

```text
min_len = min(3,4)
        = 3
```

Ab remove:

```text
nums[2] = 1
```

Sum:

```text
7 - 1 = 6
```

Move:

```text
low = 3
```

Now:

```text
6 >= 7 ❌
```

Shrinking stop.

---

# Step 6 - Expand Again

`high++`:

```text
high = 5
```

Add:

```text
nums[5] = 3
```

Current sum:

```text
6 + 3 = 9
```

Window:

```text
2 3 1 [2,4,3]
       ↑   ↑
       L   H
```

Valid:

```text
9 >= 7 ✅
```

Length:

```text
5-3+1 = 3
```

Update:

```text
min_len = min(3,3)
        = 3
```

Remove:

```text
nums[3] = 2
```

Sum:

```text
9 - 2 = 7
```

Move:

```text
low = 4
```

---

# 🔥 Still Valid

Window:

```text
2 3 1 2 [4,3]
         ↑ ↑
         L H
```

Sum:

```text
7
```

Valid:

```text
7 >= 7 ✅
```

Length:

```text
5-4+1 = 2
```

Update:

```text
min_len = min(2,3)
        = 2
```

Remove:

```text
nums[4] = 4
```

Sum:

```text
7 - 4 = 3
```

Move:

```text
low = 5
```

Now:

```text
3 >= 7 ❌
```

Stop shrinking.

---

# ✅ Final Answer

Minimum length found:

```text
2
```

Subarray:

```text
[4,3]
```

So:

```text
Output = 2
```

---

# ❓ Inner `while` Kyu? `if` Kyu Nahi?

Ye is question ka **sabse important concept** hai.

Hum likhte hain:

```cpp
while(sum>=target)
```

not:

```cpp
if(sum>=target)
```

Reason:

Hume valid window milne ke baad usko **baar-baar shrink** karna hai jab tak wo valid hai.

Example:

```text
[3,1,2,4]

sum = 10
```

Target:

```text
7
```

Ye valid hai.

Shrink:

```text
[1,2,4]

sum = 7
```

Ye bhi valid hai!

Agar `if` use karte, sirf ek baar shrink hota.

Lekin `while` ki wajah se hum dobara check karte hain aur smaller window find kar lete hain.

---

# 🧠 `while` Ka Meaning

```cpp
while(sum>=target)
```

ka simple meaning:

```text
"Jab tak meri window valid hai,
use aur chhota karne ki try karo."
```

---

# 🔥 Expand vs Shrink

Is question ka pura concept:

```text
sum < target
     ↓
window chhoti / insufficient
     ↓
high++ se EXPAND
```

Aur:

```text
sum >= target
     ↓
valid window mil gayi
     ↓
answer update
     ↓
low++ se SHRINK
     ↓
smaller valid window find karo
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {

        int low=0;
        int n=nums.size();
        int high=0;
        int sum=0;
        int min_len=INT_MAX;

        while(high<n){

            sum+=nums[high];

            while(sum>=target){

                int len=high-low+1;

                min_len=min(len,min_len);

                sum=sum-nums[low];

                low++;
            }

            high++;
        }

        if(min_len==INT_MAX){
            return 0;
        }

        return min_len;
    }
};
```

---

# 🧠 Code Explanation

## Low Pointer

```cpp
int low=0;
```

Window ka starting point.

`low++` karne ka matlab:

```text
window shrink
```

---

## High Pointer

```cpp
int high=0;
```

Window ka ending point.

`high++` karne ka matlab:

```text
window expand
```

---

## Current Sum

```cpp
int sum=0;
```

Current window ke elements ka sum store karta hai.

---

## Minimum Length

```cpp
int min_len=INT_MAX;
```

Initially hume koi valid window nahi mili.

Isliye minimum ko bahut large value se initialize kiya:

```text
INT_MAX
```

---

# 🔹 Outer While Loop

```cpp
while(high<n)
```

`high` pointer se poora array traverse karte hain.

---

# 🔹 Current Element Add

```cpp
sum+=nums[high];
```

New element ko current window me include karta hai.

Matlab:

```text
EXPAND
```

---

# 🔹 Valid Window Check

```cpp
while(sum>=target)
```

Jab current window valid ho jaye, usko shrink karna start karte hain.

---

# 🔹 Length

```cpp
int len=high-low+1;
```

Current window ki length.

---

# 🔹 Minimum Update

```cpp
min_len=min(len,min_len);
```

Current window ki length aur previous minimum ko compare karta hai.

---

# 🔹 Left Element Remove

```cpp
sum=sum-nums[low];
```

Window ka leftmost element remove karta hai.

---

# 🔹 Shrink Window

```cpp
low++;
```

Left boundary right move hoti hai.

Window chhoti ho jati hai.

---

# 🔹 Expand Window

Inner while complete hone ke baad:

```cpp
high++;
```

Next element include karne ke liye right boundary aage badhti hai.

---

# ⚠️ Important Edge Case

Suppose:

```text
target = 20

nums = [1,2,3]
```

Total sum:

```text
6
```

Kabhi bhi:

```text
sum >= 20
```

nahi hoga.

So:

```text
min_len
```

kabhi update nahi hoga.

Wo rahega:

```text
INT_MAX
```

Lekin problem ke according agar valid subarray nahi hai:

```text
return 0
```

karna hai.

Isliye:

```cpp
if(min_len==INT_MAX){
    return 0;
}
```

Otherwise:

```cpp
return min_len;
```

---

# 🔥 Fixed vs Variable Sliding Window

## Fixed Size

Pichle question me:

```text
Window ka size = k
```

Window:

```text
[1,4,2]
  [4,2,10]
    [2,10,2]
```

Size always:

```text
3
```

Pattern:

```text
old left remove
+
new right add
```

---

## Variable Size

LC 209:

```text
Window size change hota hai
```

Example:

```text
[2,3,1,2] → size 4

[3,1,2,4] → size 4

[1,2,4] → size 3

[4,3] → size 2
```

Pattern:

```text
high → expand

low → shrink
```

---

# ⭐ Variable Size Sliding Window Template

Is question se ye basic pattern yaad rakho:

```cpp
int low=0;
int high=0;

while(high<n){

    // window me new element add
    sum += nums[high];

    // jab condition satisfy ho
    while(condition){

        // answer update

        // left element remove
        sum -= nums[low];

        // shrink
        low++;
    }

    // expand
    high++;
}
```

LC 209 me:

```text
condition = sum >= target
```

Aur answer:

```text
minimum window length
```

---

# 🔥 Quick Revision

```text
low = 0
high = 0
sum = 0

        ↓

nums[high] add karo

        ↓

sum >= target ?

     NO
      ↓
   high++
   EXPAND


     YES
      ↓
length calculate
      ↓
minimum update
      ↓
nums[low] remove
      ↓
low++
SHRINK
      ↓
phir check:
sum >= target ?
```

---

# 🎯 Pattern To Remember

### EXPAND

```cpp
sum += nums[high];
```

Then eventually:

```cpp
high++;
```

---

### CONDITION

```cpp
while(sum >= target)
```

---

### ANSWER

```cpp
int len = high-low+1;

min_len = min(len,min_len);
```

---

### SHRINK

```cpp
sum -= nums[low];

low++;
```

---

# 🧠 One-Line Revision

```text
High se window expand karo → sum target tak pahuchte hi length store karo → low se baar-baar shrink karo → minimum valid window find karo.
```

---

# ⏱️ Time Complexity

At first glance nested loops dekhkar lag sakta hai:

```text
O(n²)
```

Lekin actually:

```text
O(n)
```

hai.

Reason:

`high` pointer poore array me maximum `n` steps move karta hai.

`low` pointer bhi poore array me maximum `n` steps move karta hai.

Dono pointers kabhi peeche nahi jaate.

So total:

```text
O(n + n)
```

Which simplifies to:

```text
O(n)
```

---

# 💾 Space Complexity

Hum sirf:

```text
low
high
sum
min_len
len
```

variables use kar rahe hain.

Koi extra array/vector nahi.

So:

```text
O(1)
```

---

# 📊 Complexity Summary

```text
Time Complexity  → O(n)

Space Complexity → O(1)
```

---

# ⭐ Most Important Takeaway

Fixed Sliding Window me:

```text
Window size already given hota hai.
```

Variable Sliding Window me:

```text
Condition given hoti hai,
window size hume adjust karna hota hai.
```

LC 209 me:

```text
EXPAND until sum >= target

        ↓

SHRINK while sum >= target

        ↓

Minimum length update karte raho
```

Ye **Variable Size Sliding Window ka fundamental pattern** hai.
