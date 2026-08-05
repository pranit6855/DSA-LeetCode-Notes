# LeetCode 16 - 3Sum Closest

## 📌 Problem

Hume ek integer array `nums` aur ek `target` diya gaya hai.

Hume array ke **3 numbers** choose karne hain jinka sum `target` ke **sabse close** ho.

Exact target milna compulsory nahi hai.

Hume final me **closest sum return** karna hai.

---

## 🔹 Example

```text
nums = [-1,2,1,-4]
target = 1
```

Array ko sort karne ke baad:

```text
[-4,-1,1,2]
```

Ek possible triplet:

```text
-1 + 1 + 2 = 2
```

Target:

```text
1
```

Difference:

```text
|1 - 2| = 1
```

Ye target ke sabse close hai.

So:

```text
Output = 2
```

---

# 💡 Approach - Sorting + Two Pointers

Ye question **3Sum jaisa hi hai**.

3Sum me hum find kar rahe the:

```text
sum == 0
```

Yahan hum find kar rahe hain:

```text
sum target ke sabse close ho
```

Approach:

```text
1. Array sort karo

2. Ek element i se fix karo

3. left = i+1

4. right = n-1

5. Current sum calculate karo

6. Check karo current sum target ke previous closest se
   zyada close hai ya nahi

7. Pointers move karo
```

---

# 🔹 Step 1 - Sort Array

```cpp
sort(nums.begin(),nums.end());
```

Example:

```text
Before:

[-1,2,1,-4]

After:

[-4,-1,1,2]
```

Sorting important hai because isi ki wajah se hum decide kar sakte hain:

```text
sum chhota hai → left++

sum bada hai → right--
```

---

# 🔹 Step 2 - Initial Closest Sum

Hume current best answer store karne ke liye ek variable chahiye:

```cpp
int closest=nums[0]+nums[1]+nums[2];
```

Starting me first 3 elements ka sum maan lete hain.

Example:

```text
nums = [-4,-1,1,2]
```

So:

```text
closest = -4 + -1 + 1
        = -4
```

Abhi ke liye:

```text
closest = -4
```

Aage agar better sum milega to `closest` update kar denge.

---

# ❓ First 3 Elements Hi Kyu?

Hume starting me bas **koi valid triplet sum** chahiye jisse future sums compare kar sakein.

Array me minimum 3 elements guaranteed hote hain.

Isliye simply:

```cpp
nums[0]+nums[1]+nums[2]
```

use kar sakte hain.

Ye final answer hona zaroori nahi hai.

Ye sirf starting value hai.

---

# 🔹 Step 3 - Ek Element Fix Karna

```cpp
for(int i=0;i<n-2;i++)
```

Har iteration me `nums[i]` ko fix karenge.

Then:

```cpp
int left=i+1;
int right=n-1;
```

Example:

```text
[-4,-1,1,2]
 ↑   ↑     ↑
 i  left  right
```

---

# 🔹 Step 4 - Current Sum

```cpp
int sum=nums[i]+nums[left]+nums[right];
```

Example:

```text
-4 + -1 + 2
= -3
```

Ab check karna hai:

```text
Kya -3 target ke closer hai
previous closest -4 se?
```

---

# ⭐ Sabse Important Logic - Closest Kaise Check Kare?

Hum target aur sum ke beech ka **difference/distance** check karenge.

Code:

```cpp
if(abs(target-sum)<abs(target-closest)){
    closest=sum;
}
```

Is line ko properly samjho.

---

## Example

Maan:

```text
target = 10

closest = 6

sum = 8
```

Old closest ki target se distance:

```text
|10 - 6|
= 4
```

Current sum ki target se distance:

```text
|10 - 8|
= 2
```

Compare:

```text
2 < 4
```

True.

Matlab `8`, `6` se target ke zyada paas hai.

So:

```cpp
closest=sum;
```

Now:

```text
closest = 8
```

---

# ❓ `abs()` Kyu Use Karte Hain?

`abs()` absolute value deta hai.

Example:

```text
target = 10
sum = 12
```

Normal subtraction:

```text
10 - 12 = -2
```

Lekin distance negative nahi hoti.

Actual distance:

```text
2
```

So:

```cpp
abs(10-12)
```

gives:

```text
2
```

Isliye comparison:

```cpp
abs(target-sum)
```

se karte hain.

---

# 🔍 Pointer Movement

Current sum calculate karne ke baad 3 cases hain.

---

## Case 1 - `sum < target`

Agar:

```cpp
sum<target
```

matlab current sum **target se chhota** hai.

Hume sum bada karna hai.

Array sorted hai.

So:

```cpp
left++;
```

Example:

```text
[-4,-1,1,2]
 ↑   ↑     ↑
 i   L     R
```

Current:

```text
-4 + -1 + 2 = -3
```

Target:

```text
1
```

`-3` chhota hai.

So bigger value chahiye.

`left++`:

```text
[-4,-1,1,2]
 ↑      ↑ ↑
 i      L R
```

Ab `-1` ki jagah `1` aa gaya.

Sum increase ho gaya.

---

# Case 2 - `sum > target`

Agar:

```cpp
sum>target
```

matlab sum target se bada hai.

Hume smaller sum chahiye.

So:

```cpp
right--;
```

Sorted array me `right` ko left move karne se smaller value milti hai.

---

# Case 3 - `sum == target`

Agar:

```cpp
sum==target
```

to exact target mil gaya.

Example:

```text
target = 5
sum = 5
```

Difference:

```text
|5-5| = 0
```

`0` se smaller difference possible hi nahi hai.

Isliye direct:

```cpp
return sum;
```

kar sakte hain.

---

# 🔄 Complete Dry Run

Consider:

```text
nums = [-1,2,1,-4]

target = 1
```

Sort:

```text
[-4,-1,1,2]
```

Initial:

```text
closest = -4 + -1 + 1
        = -4
```

---

## Step 1

```text
i = 0
left = 1
right = 3
```

Array:

```text
[-4,-1,1,2]
 ↑   ↑     ↑
 i   L     R
```

Current sum:

```text
-4 + -1 + 2
= -3
```

Target:

```text
1
```

Current sum distance:

```text
|1 - (-3)| = 4
```

Old closest distance:

```text
|1 - (-4)| = 5
```

Compare:

```text
4 < 5
```

True.

So:

```text
closest = -3
```

---

## Pointer Movement

Current:

```text
sum = -3
target = 1
```

Since:

```text
-3 < 1
```

sum chhota hai.

So:

```cpp
left++;
```

---

## Step 2

Now:

```text
[-4,-1,1,2]
 ↑      ↑ ↑
 i      L R
```

Current sum:

```text
-4 + 1 + 2
= -1
```

Distance:

```text
|1 - (-1)|
= 2
```

Previous closest:

```text
closest = -3
```

Distance:

```text
|1 - (-3)|
= 4
```

Since:

```text
2 < 4
```

update:

```text
closest = -1
```

---

## Next `i`

Now:

```text
i = 1
```

Pointers:

```text
[-4,-1,1,2]
     ↑  ↑ ↑
     i  L R
```

Sum:

```text
-1 + 1 + 2
= 2
```

Target:

```text
1
```

Current distance:

```text
|1 - 2|
= 1
```

Previous closest:

```text
closest = -1
```

Distance:

```text
|1 - (-1)|
= 2
```

Since:

```text
1 < 2
```

update:

```text
closest = 2
```

---

# ✅ Final Answer

Closest sum:

```text
2
```

So:

```cpp
return closest;
```

Output:

```text
2
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {

        int n=nums.size();

        sort(nums.begin(),nums.end());

        int closest=nums[0]+nums[1]+nums[2];

        for(int i=0;i<n-2;i++){

            int left=i+1;
            int right=n-1;

            while(left<right){

                int sum=nums[i]+nums[left]+nums[right];

                if(abs(target-sum)<abs(target-closest)){
                    closest=sum;
                }

                if(sum<target){
                    left++;
                }

                else if(sum>target){
                    right--;
                }

                else{
                    return sum;
                }
            }
        }

        return closest;
    }
};
```

---

# 🧠 Code Explanation

## Array Size

```cpp
int n=nums.size();
```

Total elements store kiye.

---

## Sort

```cpp
sort(nums.begin(),nums.end());
```

Two Pointer movement ke liye sorting important hai.

---

## Initial Closest

```cpp
int closest=nums[0]+nums[1]+nums[2];
```

Starting ke liye koi valid triplet sum store kar liya.

---

## Fix One Element

```cpp
for(int i=0;i<n-2;i++)
```

Har iteration me `nums[i]` fix hota hai.

---

## Two Pointers

```cpp
int left=i+1;
int right=n-1;
```

Remaining portion me Two Pointer search.

---

## Current Sum

```cpp
int sum=nums[i]+nums[left]+nums[right];
```

Current 3 elements ka sum.

---

## Closest Update

```cpp
if(abs(target-sum)<abs(target-closest)){
    closest=sum;
}
```

Meaning:

```text
current sum ki distance
<
old closest ki distance
```

to current sum better hai.

---

## Sum Chhota

```cpp
if(sum<target){
    left++;
}
```

Sum increase karna hai.

---

## Sum Bada

```cpp
else if(sum>target){
    right--;
}
```

Sum decrease karna hai.

---

## Exact Target

```cpp
else{
    return sum;
}
```

Exact target mil gaya to difference `0` hai.

Isse better possible nahi.

---

# ❓ 3Sum Aur 3Sum Closest Me Difference

### 3Sum

Goal:

```text
sum == 0
```

Valid triplets store karne hain.

---

### 3Sum Closest

Goal:

```text
sum target ke sabse paas ho
```

Triplet store nahi karna.

Sirf:

```text
closest sum
```

return karna hai.

---

# ⏱️ Time Complexity

Sorting:

```text
O(n log n)
```

Outer loop + Two Pointer:

```text
O(n²)
```

Overall:

```text
O(n²)
```

---

# 💾 Space Complexity

Two Pointer logic ke liye extra array use nahi kar rahe.

```text
O(1)
```

Sorting implementation internally extra space use kar sakti hai.

---

# ⭐ Important Points

```text
Sort first
```

Then:

```text
i → fixed element

left → i+1

right → n-1
```

Closest comparison:

```cpp
abs(target-sum) < abs(target-closest)
```

Pointer movement:

```text
sum < target
→ left++
```

```text
sum > target
→ right--
```

```text
sum == target
→ direct return
```

---

# 🔥 Quick Revision

```text
3Sum Closest
      ↓
Sort Array
      ↓
Initial closest = first 3 elements ka sum
      ↓
Fix i
      ↓
left = i+1
right = n-1
      ↓
Calculate sum
      ↓
Is current sum closer?
      ↓
YES → closest = sum
      ↓
------------------------
sum < target → left++

sum > target → right--

sum == target → return
------------------------
      ↓
return closest
```

---

# 🎯 Main Pattern

```text
3Sum Closest
=
Sorting
+
Fix One Element
+
Two Pointers
+
Minimum Difference Tracking
```

Sabse important line:

```cpp
if(abs(target-sum)<abs(target-closest)){
    closest=sum;
}
```

Bas iska meaning yaad rakho:

```text
"Current sum target ke purane answer se zyada paas hai kya?"
```

Agar haan:

```text
closest update kar do.
```
