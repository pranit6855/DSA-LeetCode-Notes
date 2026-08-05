# LeetCode 26 - Remove Duplicates from Sorted Array

## 📌 Problem

Hume ek **sorted array `nums`** diya gaya hai jisme duplicate elements ho sakte hain.

Hume duplicates ko remove karke saare **unique elements array ke starting me** rakhne hain.

Hume return karna hai:

```text
Total number of unique elements
```

Important baat: Hume koi extra array nahi banana hai. Original `nums` array ko hi modify karna hai.

---

## 🔹 Example

```text
Input:

nums = [1,1,2,2,3]
```

Duplicates remove karne ke baad starting portion:

```text
[1,2,3,_,_]
```

Unique elements:

```text
1, 2, 3
```

Isliye:

```text
Output = 3
```

---

# 💡 Approach - Two Pointers

Is problem ko solve karne ke liye hum **Two Pointer Approach** use karenge.

Hum do pointers lenge:

```text
i → last unique element ko point karega

j → array me aage jaake elements check karega
```

Saath me:

```text
unique → unique elements ka count rakhega
```

---

## 🔹 Initialization

```cpp
int i = 0;
int j = 1;
int unique = 1;
```

### `i = 0` kyu?

Array ka first element hamesha unique maana ja sakta hai.

Isliye `i` first element ko point karega.

### `j = 1` kyu?

First element already consider kar liya hai, isliye checking second element se start karenge.

### `unique = 1` kyu?

First element already unique hai.

Isliye starting unique count:

```text
unique = 1
```

---

# 🔍 Main Logic

Hum compare karenge:

```cpp
nums[j] != nums[i]
```

Do cases ho sakte hain.

---

## Case 1: Duplicate Element

Agar:

```cpp
nums[j] == nums[i]
```

matlab current element duplicate hai.

Example:

```text
nums = [1,1,2,2,3]

        i j
        ↓ ↓
       [1,1,2,2,3]
```

Yahan:

```text
nums[i] = 1
nums[j] = 1
```

Dono same hain.

Isliye second `1` duplicate hai.

Hume kuch bhi store nahi karna.

Bas `j` ko aage move kar denge:

```cpp
j++;
```

---

## Case 2: New Unique Element

Agar:

```cpp
nums[j] != nums[i]
```

matlab hume ek **new unique element** mil gaya.

Example:

```text
[1,1,2,2,3]
 ↑   ↑
 i   j
```

Yahan:

```text
nums[i] = 1
nums[j] = 2
```

Different hain.

Matlab `2` ek new unique element hai.

Ab `2` ko last unique element ke next position par rakhna hai.

```cpp
nums[i+1] = nums[j];
```

Array:

```text
Before:

[1,1,2,2,3]

After:

[1,2,2,2,3]
```

Ab `2` last unique element ban gaya.

Isliye:

```cpp
i++;
```

Aur ek new unique element mila hai, so:

```cpp
unique++;
```

---

# 🔄 Complete Dry Run

Consider:

```text
nums = [1,1,2,2,3]
```

Starting:

```text
i = 0
j = 1
unique = 1
```

Array:

```text
[1,1,2,2,3]
 ↑ ↑
 i j
```

---

### Step 1

Compare:

```text
nums[i] = 1
nums[j] = 1
```

Condition:

```text
1 != 1 → False
```

Duplicate mila.

Kuch change nahi karenge.

Bas:

```cpp
j++;
```

Now:

```text
i = 0
j = 2
unique = 1
```

Array:

```text
[1,1,2,2,3]
 ↑   ↑
 i   j
```

---

### Step 2

Compare:

```text
nums[i] = 1
nums[j] = 2
```

Condition:

```text
1 != 2 → True
```

New unique element mila.

Execute:

```cpp
nums[i+1] = nums[j];
```

Means:

```text
nums[1] = nums[2]
```

So array becomes:

```text
[1,2,2,2,3]
```

Then:

```cpp
i++;
unique++;
```

Now:

```text
i = 1
j = 3
unique = 2
```

---

### Step 3

Now:

```text
nums[i] = 2
nums[j] = 2
```

Same hain.

```text
2 != 2 → False
```

Duplicate hai.

Sirf:

```cpp
j++;
```

Now:

```text
j = 4
```

---

### Step 4

Now compare:

```text
nums[i] = 2
nums[j] = 3
```

Different hain.

```text
2 != 3 → True
```

New unique element mila.

Execute:

```cpp
nums[i+1] = nums[j];
```

Means:

```text
nums[2] = nums[4]
```

Array becomes:

```text
[1,2,3,2,3]
```

Then:

```cpp
i++;
unique++;
```

Now:

```text
unique = 3
```

---

## ✅ Final Result

Array ka important starting portion:

```text
[1,2,3]
```

Unique elements:

```text
1
2
3
```

So:

```text
unique = 3
```

Return:

```cpp
return unique;
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int i=0;
        int j=1;
        int unique=1;
        int n=nums.size();

        while(j<n){
            if(nums[j]!=nums[i]){
                nums[i+1]=nums[j];
                i++;
                unique++;
            }
            j++;
        }

        return unique;
    }
};
```

---

# 🧠 Code Explanation

## 1. Array ka size

```cpp
int n=nums.size();
```

`n` me array ka total size store kar liya.

---

## 2. First Pointer

```cpp
int i=0;
```

`i` last unique element ko point karta hai.

Starting me first element unique hai, isliye:

```text
i = 0
```

---

## 3. Second Pointer

```cpp
int j=1;
```

`j` naye elements check karta hai.

First element already consider ho gaya, isliye checking index `1` se start hoti hai.

---

## 4. Unique Counter

```cpp
int unique=1;
```

First element already unique hai.

Isliye count initially `1` hai.

---

## 5. Array Traverse Karna

```cpp
while(j<n)
```

Jab tak `j` array ke andar hai, hum elements check karte rahenge.

---

## 6. New Unique Element Check

```cpp
if(nums[j]!=nums[i])
```

Agar `j` wala element `i` wale element se different hai, iska matlab hume new unique element mil gaya.

---

## 7. Unique Element Ko Correct Position Par Rakhna

```cpp
nums[i+1]=nums[j];
```

New unique element ko last unique element ke just next position par rakh dete hain.

---

## 8. `i` Ko Move Karna

```cpp
i++;
```

Ab new element last unique element ban gaya.

Isliye `i` ko bhi aage move kar dete hain.

---

## 9. Unique Count Increase

```cpp
unique++;
```

Ek new unique element mila, isliye count increase karte hain.

---

## 10. `j` Ko Move Karna

```cpp
j++;
```

`j` ko har iteration me next element check karna hai.

Isliye ye `if` ke bahar hai.

Chahe duplicate mile ya unique:

```text
j always moves forward
```

---

# ❓ Sorted Array Important Kyu Hai?

Ye approach isliye work karta hai kyunki array **sorted** hai.

Sorted array me duplicates ek saath hote hain.

Example:

```text
[1,1,1,2,2,3,3,4]
```

Isliye agar current element last unique element ke equal hai, to hume pata hai ki wo duplicate hai.

Example:

```text
i
↓
2,2,2
  ↑
  j
```

Jab tak `j` ko `2` mil raha hai, hum skip karte rahenge.

Jaise hi:

```text
2 != 3
```

milega, hume pata chal jayega ki `3` new unique element hai.

---

# ⏱️ Time Complexity

```text
O(n)
```

Reason:

`j` array ke har element ko maximum ek baar check karta hai.

---

# 💾 Space Complexity

```text
O(1)
```

Hum koi extra array use nahi kar rahe.

Sirf kuch variables use ho rahe hain:

```text
i
j
unique
n
```

Isliye extra space constant hai.

---

# ⭐ Important Points

```text
i → last unique element

j → array ko traverse karta hai

unique → unique elements count karta hai
```

### Duplicate mila:

```text
nums[j] == nums[i]

→ kuch mat karo
→ j++
```

### Unique mila:

```text
nums[j] != nums[i]

→ nums[i+1] = nums[j]
→ i++
→ unique++
→ j++
```

---

# 🔥 Quick Revision

```text
Sorted Array
      ↓
Two Pointers
      ↓

i = last unique element
j = checking pointer

      ↓

nums[j] == nums[i]
      ↓
Duplicate
      ↓
j++

---------------------

nums[j] != nums[i]
      ↓
New Unique Element
      ↓
nums[i+1] = nums[j]
i++
unique++
j++
```

---

# 🎯 Pattern To Remember

Is problem ka main Two Pointer pattern:

```text
i → processed / unique part maintain karta hai

j → unexplored elements ko scan karta hai
```

`j` ko new valid element milta hai to us element ko `i` ke next position par place kar dete hain.

Ye Two Pointer pattern dusre array problems me bhi kaafi useful hota hai.
