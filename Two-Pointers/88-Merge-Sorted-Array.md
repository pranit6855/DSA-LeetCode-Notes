# LeetCode 88 - Merge Sorted Array

## 📌 Problem

Hume do **sorted arrays** diye gaye hain:

```text
nums1
nums2
```

`nums1` ke andar starting ke `m` elements actual elements hain.

`nums2` ke andar `n` elements hain.

`nums1` ka total size:

```text
m + n
```

hota hai, kyunki uske end me extra space already diya hota hai.

Hume `nums1` aur `nums2` ko merge karke final sorted array **nums1 ke andar hi store karna hai**.

---

## 🔹 Example

```text
nums1 = [1,2,3,0,0,0]
m = 3

nums2 = [2,5,6]
n = 3
```

Yahan `nums1` ke actual elements:

```text
[1,2,3]
```

Aur `nums2`:

```text
[2,5,6]
```

Dono ko merge karne ke baad:

```text
[1,2,2,3,5,6]
```

So final:

```text
nums1 = [1,2,2,3,5,6]
```

---

# 💡 Approach - Two Pointers + Extra Array

Hum ek temporary array banayenge:

```cpp
vector<int> nums3(m+n);
```

Is `nums3` ke andar `nums1` aur `nums2` ko sorted order me merge karenge.

Uske baad `nums3` ke saare elements ko wapas `nums1` me copy kar denge.

---

# 🧠 Pointers

Hum 3 pointers use karenge:

```text
i → nums1 ke actual elements ko traverse karega

j → nums2 ko traverse karega

k → nums3 me elements insert karne ke liye use hoga
```

Starting:

```cpp
int i=0,j=0;
int k=0;
```

Example:

```text
nums1 = [1,2,3,0,0,0]
         ↑
         i

nums2 = [2,5,6]
         ↑
         j

nums3 = [_,_,_,_,_,_]
         ↑
         k
```

---

# 🔍 Main Logic

Jab tak dono arrays ke andar elements available hain:

```cpp
while(i<m && j<n)
```

hum `nums1[i]` aur `nums2[j]` ko compare karenge.

Jo element **smaller** hoga, usko `nums3[k]` me daal denge.

---

# 🔹 Case 1 - nums1 ka element smaller hai

Agar:

```cpp
nums1[i] < nums2[j]
```

to `nums1[i]` ko `nums3` me daalenge:

```cpp
nums3[k]=nums1[i];
```

Uske baad:

```cpp
i++;
k++;
```

`i++` because `nums1` ka current element use ho gaya.

`k++` because `nums3` ki current position fill ho gayi.

---

# 🔹 Case 2 - nums2 ka element smaller ya equal hai

Agar:

```cpp
nums1[i] < nums2[j]
```

false hai, to `else` chalega:

```cpp
nums3[k]=nums2[j];
```

Then:

```cpp
j++;
k++;
```

`j++` because `nums2` ka current element use ho gaya.

`k++` because `nums3` ki current position fill ho gayi.

---

# 🔄 Complete Dry Run

Consider:

```text
nums1 = [1,2,3,0,0,0]
m = 3

nums2 = [2,5,6]
n = 3
```

Temporary array:

```text
nums3 = [_,_,_,_,_,_]
```

Starting:

```text
i = 0
j = 0
k = 0
```

---

## Step 1

Compare:

```text
nums1[i] = 1
nums2[j] = 2
```

Check:

```text
1 < 2
```

True.

So:

```cpp
nums3[k]=nums1[i];
```

Now:

```text
nums3 = [1,_,_,_,_,_]
```

Move:

```text
i = 1
k = 1
```

---

## Step 2

Now:

```text
nums1[i] = 2
nums2[j] = 2
```

Condition:

```text
2 < 2
```

False.

So `else` chalega:

```cpp
nums3[k]=nums2[j];
```

Now:

```text
nums3 = [1,2,_,_,_,_]
```

Move:

```text
j = 1
k = 2
```

---

## Step 3

Now compare:

```text
nums1[i] = 2
nums2[j] = 5
```

Condition:

```text
2 < 5
```

True.

So:

```text
nums3[k]=nums1[i];
```

Result:

```text
nums3 = [1,2,2,_,_,_]
```

Move:

```text
i = 2
k = 3
```

---

## Step 4

Compare:

```text
nums1[i] = 3
nums2[j] = 5
```

Condition:

```text
3 < 5
```

True.

So:

```text
nums3 = [1,2,2,3,_,_]
```

Move:

```text
i = 3
k = 4
```

---

# ⚠️ Main While Loop Ab Ruk Jayega

Condition thi:

```cpp
while(i<m && j<n)
```

Ab:

```text
i = 3
m = 3
```

So:

```text
i < m

3 < 3 → False
```

Main loop stop ho jayega.

Lekin `nums2` me abhi elements bache hue hain:

```text
5, 6
```

Isliye hume remaining elements separately copy karne padenge.

---

# 🔹 Remaining nums1 Elements

Agar `nums1` ke elements bach gaye:

```cpp
while(i<m){
    nums3[k]=nums1[i];
    i++;
    k++;
}
```

Ye loop `nums1` ke remaining elements ko `nums3` me copy karega.

Example:

```text
nums1 = [1,5,6]
nums2 = [2,3,4]
```

Agar `nums2` pehle finish ho gaya, to `nums1` ke remaining elements ko copy karna padega.

---

# 🔹 Remaining nums2 Elements

Agar `nums2` ke elements bach gaye:

```cpp
while(j<n){
    nums3[k]=nums2[j];
    j++;
    k++;
}
```

Hamare example me:

```text
nums2 = [2,5,6]
           ↑
           j
```

`5` aur `6` remaining hain.

First:

```text
nums3 = [1,2,2,3,5,_]
```

Then:

```text
nums3 = [1,2,2,3,5,6]
```

Ab merge complete hai.

---

# 🔹 nums3 Ko nums1 Me Copy Karna

Problem me final answer `nums1` ke andar chahiye.

Abhi answer:

```text
nums3 = [1,2,2,3,5,6]
```

me pada hai.

Isliye:

```cpp
for(int x=0;x<m+n;x++){
    nums1[x]=nums3[x];
}
```

use karke `nums3` ko `nums1` me copy kar denge.

Final:

```text
nums1 = [1,2,2,3,5,6]
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i=0,j=0;
        vector<int>nums3(m+n);
        int k=0;

        while(i<m && j<n){
            if(nums1[i]<nums2[j]){
                nums3[k]=nums1[i];
                i++;
                k++;
            }
            else {
                nums3[k]=nums2[j];
                j++;
                k++;
            }
        }

        while(i<m){
            nums3[k]=nums1[i];
            i++;
            k++;
        }

        while(j<n){
            nums3[k]=nums2[j];
            j++;
            k++;
        }

        for(int x=0;x<m+n;x++){
            nums1[x]=nums3[x];
        }
    }
};
```

---

# 🧠 Code Explanation

## 1. Pointers Initialize Karna

```cpp
int i=0,j=0;
```

`i`:

```text
nums1 ko traverse karega
```

`j`:

```text
nums2 ko traverse karega
```

---

## 2. Temporary Array

```cpp
vector<int>nums3(m+n);
```

Dono arrays me total:

```text
m + n
```

elements hain.

Isliye `nums3` ka size:

```text
m+n
```

rakha.

---

## 3. Third Pointer

```cpp
int k=0;
```

`k` `nums3` ki current empty position ko point karega.

---

## 4. Main While Loop

```cpp
while(i<m && j<n)
```

Jab tak **dono arrays me elements available hain**, comparison karte rahenge.

---

## 5. Smaller Element Choose Karna

```cpp
if(nums1[i]<nums2[j])
```

Agar `nums1` ka element smaller hai:

```cpp
nums3[k]=nums1[i];
i++;
k++;
```

Otherwise:

```cpp
nums3[k]=nums2[j];
j++;
k++;
```

---

# ❓ `i`, `j`, `k` Ko Kaise Yaad Rakhein?

Simple:

```text
i → nums1

j → nums2

k → nums3
```

Isliye assignments bhi generally:

```cpp
nums3[k] = nums1[i];
```

ya:

```cpp
nums3[k] = nums2[j];
```

honge.

---

# ❓ Extra While Loops Kyu Chahiye?

Main loop:

```cpp
while(i<m && j<n)
```

tab tak hi chalta hai jab tak **dono arrays me elements hain**.

Jaise hi ek array finish ho gaya, loop ruk jayega.

Lekin dusre array me elements abhi bhi bach sakte hain.

Isliye:

```cpp
while(i<m)
```

remaining `nums1` elements ke liye.

Aur:

```cpp
while(j<n)
```

remaining `nums2` elements ke liye.

---

# ❓ Final For Loop Kyu Chahiye?

Humne merge:

```text
nums3
```

me kiya hai.

Lekin LeetCode ko final answer:

```text
nums1
```

ke andar chahiye.

Isliye:

```cpp
for(int x=0;x<m+n;x++){
    nums1[x]=nums3[x];
}
```

se complete merged array wapas `nums1` me copy karte hain.

---

# ⏱️ Time Complexity

Main merging process:

```text
O(m+n)
```

Uske baad `nums3` ko `nums1` me copy karne me bhi:

```text
O(m+n)
```

Overall:

```text
O(m+n)
```

Constants ignore hote hain.

---

# 💾 Space Complexity

Humne extra array banaya:

```cpp
vector<int>nums3(m+n);
```

Isliye:

```text
Space Complexity = O(m+n)
```

---

# ⭐ Important Points

```text
i → nums1 ka pointer

j → nums2 ka pointer

k → nums3 ka pointer
```

Compare:

```text
nums1[i] < nums2[j]
```

Agar true:

```text
nums1 ka element nums3 me
i++
k++
```

Otherwise:

```text
nums2 ka element nums3 me
j++
k++
```

Ek array finish hone ke baad:

```text
remaining nums1 copy karo

remaining nums2 copy karo
```

Finally:

```text
nums3 → nums1
```

---

# 🔥 Quick Revision

```text
nums1 sorted        nums2 sorted
      ↓                   ↓
      i                   j
       \                 /
        \               /
          Compare
             ↓
      smaller element
             ↓
          nums3[k]
             ↓
            k++
```

Main idea:

```text
1. i = nums1
2. j = nums2
3. k = nums3

4. Dono elements compare karo

5. Smaller ko nums3 me daalo

6. Jis array se element liya
   uska pointer move karo

7. Remaining elements copy karo

8. nums3 ko nums1 me copy karo
```

---

# 🎯 Pattern To Remember

Do sorted arrays ko merge karte waqt:

```text
Compare → Smaller Pick → Pointer Move
```

Because arrays already sorted hain, hume baar-baar sorting karne ki zarurat nahi padti.

Ye **Two Pointer Merge Pattern** hai.
