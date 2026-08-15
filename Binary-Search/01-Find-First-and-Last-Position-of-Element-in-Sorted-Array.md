# LeetCode 34 - Find First and Last Position of Element in Sorted Array

## 📌 Problem

Hume ek **sorted array** `nums` diya hai aur ek `target` value di gayi hai.

Hume target element ki:

1. **First position**
2. **Last position**

find karni hai.

Agar target array me nahi hai, to:

```text
[-1, -1]
```

return karna hai.

### Important Condition

Array sorted hai aur hume:

```text
O(log n)
```

time complexity me answer find karna hai.

Isliye yahan **Binary Search** use hoga.

---

# 🔹 Example

```text
nums = [5,7,7,8,8,10]
target = 8
```

Array:

```text
Index:  0 1 2 3 4 5
Value:  5 7 7 8 8 10
```

Target:

```text
8
```

First occurrence:

```text
index = 3
```

Last occurrence:

```text
index = 4
```

Therefore:

```text
Output = [3,4]
```

---

# 🧠 Main Concept

Normally agar hum simple loop lagaye:

```cpp
for(int i=0;i<nums.size();i++)
```

to worst case:

```text
O(n)
```

lagega.

But question clearly bol raha hai:

```text
O(log n)
```

So hume **Binary Search** use karna padega.

Yahan ek normal binary search se directly answer nahi milega.

Kyun?

Because target multiple times aa sakta hai.

Example:

```text
[5,7,7,8,8,8,10]
       ↑ ↑ ↑
       3 4 5
```

Hume specifically:

```text
First 8 → 3
Last 8  → 5
```

chahiye.

Isliye hume **2 binary searches** karni hongi:

```text
1. First Position
2. Last Position
```

---

# 🔥 Binary Search Pattern

Is question ka main pattern:

```text
Sorted Array
      ↓
Binary Search
      ↓
Find First Occurrence
      +
Find Last Occurrence
```

Isko hum:

```text
Boundary Binary Search
```

bhi samajh sakte hain.

---

# 🔹 First Position Kaise Find Kare?

Suppose:

```text
nums = [5,7,7,8,8,10]
target = 8
```

Target `8` hume index:

```text
3
4
```

par mil raha hai.

Agar normal binary search me `8` mil gaya, to hum turant return nahi karenge.

Kyunki ho sakta hai left side me bhi `8` ho.

Example:

```text
[5,7,7,8,8,10]
       ↑
      mid
```

Target mil gaya.

But hume check karna hai:

```text
Kya isse pehle bhi target hai?
```

Agar hai:

```text
high = mid - 1
```

kar ke left side search karenge.

---

# 🔥 First Position Logic

```cpp
int first = -1;

int low = 0;
int high = nums.size() - 1;

while(low <= high){

    int mid = low + (high-low)/2;

    if(nums[mid] == target){

        first = mid;

        // Aur left me target ho sakta hai
        high = mid - 1;
    }

    else if(nums[mid] < target){

        low = mid + 1;
    }

    else{

        high = mid - 1;
    }
}
```

---

# 🧠 First Position Me `high = mid - 1` Kyu?

Suppose:

```text
nums = [5,7,7,8,8,10]
```

Aur:

```text
mid = 4
nums[mid] = 8
```

Target mil gaya.

But:

```text
index 3 par bhi 8 hai
```

So index `4` first position nahi hai.

Hume left side me jaana hai.

Therefore:

```cpp
high = mid - 1;
```

kar denge.

Important:

```text
Target mila
     ↓
Answer store karo
     ↓
Left side search karo
     ↓
Maybe aur pehle target mile
```

---

# 🔹 First Position Example

```text
nums = [5,7,7,8,8,10]
target = 8
```

Initial:

```text
low = 0
high = 5
```

Middle:

```text
mid = 2
nums[2] = 7
```

7 < 8

So:

```text
low = mid + 1
low = 3
```

Now:

```text
low = 3
high = 5
```

Middle:

```text
mid = 4
nums[4] = 8
```

Target found.

So:

```text
first = 4
```

But hume first occurrence chahiye.

Therefore:

```text
high = 3
```

Now:

```text
low = 3
high = 3
```

Middle:

```text
mid = 3
nums[3] = 8
```

Target found again.

So:

```text
first = 3
```

Now:

```text
high = 2
```

Loop stop.

Final:

```text
first = 3
```

---

# 🔥 Last Position Kaise Find Kare?

Last position ke liye bhi same binary search karenge.

Difference sirf ek hai.

Target milne par:

```text
right side search
```

karna hai.

Because target ke baad bhi target aa sakta hai.

So:

```cpp
low = mid + 1;
```

---

# 🔥 Last Position Logic

```cpp
int last = -1;

int low = 0;
int high = nums.size() - 1;

while(low <= high){

    int mid = low + (high-low)/2;

    if(nums[mid] == target){

        last = mid;

        // Aur right me target ho sakta hai
        low = mid + 1;
    }

    else if(nums[mid] < target){

        low = mid + 1;
    }

    else{

        high = mid - 1;
    }
}
```

---

# 🧠 First vs Last Position

Ye difference sabse important hai.

## First Position

Target mila:

```cpp
first = mid;
high = mid - 1;
```

Meaning:

```text
Answer save karo
     ↓
Left side jao
```

---

## Last Position

Target mila:

```cpp
last = mid;
low = mid + 1;
```

Meaning:

```text
Answer save karo
     ↓
Right side jao
```

---

# 📊 Difference Yaad Rakho

```text
FIRST OCCURRENCE

target found
     ↓
answer = mid
     ↓
high = mid - 1
     ↓
LEFT


LAST OCCURRENCE

target found
     ↓
answer = mid
     ↓
low = mid + 1
     ↓
RIGHT
```

---

# 🔥 Complete Approach

Hume total 2 Binary Searches karni hain.

```text
Array
  ↓
Binary Search #1
  ↓
First Position
  ↓
Binary Search #2
  ↓
Last Position
  ↓
Return [first,last]
```

---

# 💻 C++ Code - Basic Binary Search Approach

```cpp
class Solution {
public:

    vector<int> searchRange(vector<int>& nums, int target) {

        int n = nums.size();

        // -------------------------
        // Find First Position
        // -------------------------

        int first = -1;

        int low = 0;
        int high = n - 1;

        while(low <= high){

            int mid = low + (high - low) / 2;

            if(nums[mid] == target){

                first = mid;

                // Search more on left side
                high = mid - 1;
            }

            else if(nums[mid] < target){

                low = mid + 1;
            }

            else{

                high = mid - 1;
            }
        }


        // -------------------------
        // Find Last Position
        // -------------------------

        int last = -1;

        low = 0;
        high = n - 1;

        while(low <= high){

            int mid = low + (high - low) / 2;

            if(nums[mid] == target){

                last = mid;

                // Search more on right side
                low = mid + 1;
            }

            else if(nums[mid] < target){

                low = mid + 1;
            }

            else{

                high = mid - 1;
            }
        }


        return {first, last};
    }
};
```

---

# 🔥 Dry Run

Given:

```text
nums = [5,7,7,8,8,10]
target = 8
```

---

## Step 1 - Find First Position

Initial:

```text
low = 0
high = 5
```

### Iteration 1

```text
mid = 2
nums[2] = 7
```

Since:

```text
7 < 8
```

Move right:

```text
low = 3
```

---

### Iteration 2

```text
low = 3
high = 5

mid = 4
nums[4] = 8
```

Target found.

```text
first = 4
```

But maybe target left me hai.

So:

```text
high = 3
```

---

### Iteration 3

```text
low = 3
high = 3

mid = 3
nums[3] = 8
```

Target found.

```text
first = 3
```

Search further left:

```text
high = 2
```

Loop stop.

Therefore:

```text
First = 3
```

---

# Step 2 - Find Last Position

Again:

```text
low = 0
high = 5
```

### Iteration 1

```text
mid = 2
nums[2] = 7
```

7 < 8

So:

```text
low = 3
```

---

### Iteration 2

```text
mid = 4
nums[4] = 8
```

Target found.

```text
last = 4
```

But maybe right side me bhi `8` hai.

So:

```text
low = 5
```

---

### Iteration 3

```text
low = 5
high = 5

mid = 5
nums[5] = 10
```

10 > 8

So:

```text
high = 4
```

Loop stop.

Therefore:

```text
Last = 4
```

---

# ✅ Final Answer

```text
[first,last]

[3,4]
```

---

# 🔥 `lower_bound` Approach

C++ STL me already ek useful function available hai:

```cpp
lower_bound()
```

Ye first position find karne me directly help karta hai.

---

# 🧠 `lower_bound()` Kya Karta Hai?

Syntax:

```cpp
lower_bound(nums.begin(), nums.end(), target)
```

Ye iterator return karta hai pointing to:

```text
First element >= target
```

Meaning:

```text
target se greater ya equal first position
```

---

# 🔹 Example

```text
nums = [5,7,7,8,8,10]
target = 8
```

```cpp
lower_bound(nums.begin(), nums.end(), 8)
```

returns iterator pointing to:

```text
index 3
```

Because:

```text
nums[3] = 8
```

aur ye first element hai jo:

```text
>= 8
```

hai.

Therefore:

```text
First Position = 3
```

---

# 🔥 Iterator Ko Index Me Kaise Convert Kare?

`lower_bound()` iterator return karta hai.

Hume index chahiye.

So:

```cpp
lower_bound(nums.begin(), nums.end(), target) - nums.begin()
```

Example:

```text
iterator - begin iterator
```

gives:

```text
index
```

Therefore:

```cpp
int first =
    lower_bound(nums.begin(), nums.end(), target)
    - nums.begin();
```

---

# ⚠️ Target Exist Karta Hai Ya Nahi?

Important point:

`lower_bound()` target na hone par bhi koi position return karta hai.

Example:

```text
nums = [5,7,7,8,8,10]
target = 6
```

`lower_bound(6)` index:

```text
1
```

return karega.

Because:

```text
nums[1] = 7
```

and 7 is the first element:

```text
>= 6
```

But target `6` actually array me hai hi nahi.

Therefore pehle check:

```cpp
if(first == nums.size() || nums[first] != target)
```

Agar true hai:

```text
Target present nahi hai.
```

So:

```cpp
return {-1,-1};
```

---

# 🔥 `upper_bound()` Kya Karta Hai?

Syntax:

```cpp
upper_bound(nums.begin(), nums.end(), target)
```

Ye return karta hai:

```text
First element > target
```

Difference:

```text
lower_bound → first element >= target

upper_bound → first element > target
```

---

# 🔹 Example

```text
nums = [5,7,7,8,8,10]
target = 8
```

`lower_bound(8)`:

```text
index 3
```

Because:

```text
8 >= 8
```

first time index 3 par mila.

`upper_bound(8)`:

```text
index 5
```

Because index 5 par:

```text
10 > 8
```

first element hai.

So:

```text
lower_bound → 3
upper_bound → 5
```

---

# 🔥 Last Position Ka Formula

`upper_bound()` first element `> target` deta hai.

Hume last element `== target` chahiye.

Therefore:

```cpp
last = upper_bound(nums.begin(), nums.end(), target)
       - nums.begin() - 1;
```

Example:

```text
upper_bound = 5
```

Therefore:

```text
last = 5 - 1

last = 4
```

---

# 💻 C++ Code Using `lower_bound` and `upper_bound`

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {

        int first =
            lower_bound(nums.begin(), nums.end(), target)
            - nums.begin();

        // Target not present
        if(first == nums.size() || nums[first] != target){
            return {-1, -1};
        }

        int last =
            upper_bound(nums.begin(), nums.end(), target)
            - nums.begin() - 1;

        return {first, last};
    }
};
```

---

# 🔥 Code Ko Line By Line Samjho

## Step 1

```cpp
int first =
    lower_bound(nums.begin(), nums.end(), target)
    - nums.begin();
```

`lower_bound`:

```text
First element >= target
```

return karta hai.

Target present hai to ye target ki:

```text
FIRST POSITION
```

dega.

---

## Step 2

```cpp
if(first == nums.size() || nums[first] != target)
```

Do cases check ho rahe hain.

### Case 1

```cpp
first == nums.size()
```

Meaning:

```text
lower_bound ko koi element mila hi nahi.
```

---

### Case 2

```cpp
nums[first] != target
```

Meaning:

```text
lower_bound ne koi greater element find kiya,
but target present nahi hai.
```

Example:

```text
nums = [5,7,7,8,8,10]
target = 6
```

lower_bound:

```text
index = 1
nums[1] = 7
```

But:

```text
7 != 6
```

So target absent.

Return:

```cpp
{-1,-1}
```

---

# Step 3

```cpp
int last =
    upper_bound(nums.begin(), nums.end(), target)
    - nums.begin() - 1;
```

`upper_bound`:

```text
First element > target
```

return karta hai.

So usse ek position peeche:

```text
last occurrence
```

hogi.

---

# 📊 Lower Bound vs Upper Bound

| Function      | Meaning                   |
| ------------- | ------------------------- |
| `lower_bound` | First element `>= target` |
| `upper_bound` | First element `> target`  |

Example:

```text
nums = [5,7,7,8,8,10]
target = 8
```

```text
lower_bound → index 3
upper_bound → index 5
```

Therefore:

```text
first = 3
last = 5 - 1 = 4
```

Answer:

```text
[3,4]
```

---

# 🔥 Most Important Concept

Is question ko yaad rakhne ka easiest way:

```text
lower_bound
     ↓
First >= target
     ↓
FIRST POSITION


upper_bound
     ↓
First > target
     ↓
One step back
     ↓
LAST POSITION
```

---

# 🔄 Complete Dry Run Using STL

Given:

```text
nums = [5,7,7,8,8,10]
target = 8
```

### `lower_bound`

```text
First element >= 8
```

Array:

```text
5  7  7  8  8  10
0  1  2  3  4  5
         ↑
```

Result:

```text
first = 3
```

---

### `upper_bound`

```text
First element > 8
```

Array:

```text
5  7  7  8  8  10
0  1  2  3  4  5
                  ↑
```

Result:

```text
upper_bound = 5
```

Last occurrence:

```text
last = 5 - 1

last = 4
```

Final:

```text
[3,4]
```

---

# ❌ Example - Target Not Found

```text
nums = [5,7,7,8,8,10]
target = 6
```

`lower_bound(6)`:

```text
index = 1
```

Because:

```text
nums[1] = 7
```

But:

```text
7 != 6
```

Therefore:

```cpp
return {-1,-1};
```

Output:

```text
[-1,-1]
```

---

# ❌ Example - Empty Array

```text
nums = []
target = 0
```

```cpp
lower_bound(nums.begin(), nums.end(), target)
```

returns:

```text
nums.end()
```

Subtracting:

```cpp
- nums.begin()
```

gives:

```text
0
```

And:

```cpp
first == nums.size()
```

is true.

So:

```cpp
return {-1,-1};
```

---

# 🔥 Pattern Recognition

Question dekhte hi ye cheezein identify karo:

```text
1. Array sorted hai?
        ↓
       YES

2. O(log n) bola hai?
        ↓
       YES

3. Same target multiple times aa sakta hai?
        ↓
       YES

4. First / Last position chahiye?
        ↓
       YES

5. Boundary Binary Search use karo.
```

---

# 🧠 Binary Search Pattern

Normal Binary Search:

```text
Target find karo
     ↓
Target mil gaya
     ↓
Return
```

But Boundary Binary Search:

```text
Target find karo
     ↓
Answer save karo
     ↓
Search continue karo
     ↓
Required boundary find karo
```

---

# 🔥 First vs Last Boundary

```text
FIRST

target found
     ↓
ans = mid
     ↓
high = mid - 1
     ↓
LEFT


LAST

target found
     ↓
ans = mid
     ↓
low = mid + 1
     ↓
RIGHT
```

Ye pattern future binary search questions me bahut important hai.

---

# ⚠️ Common Mistakes

## 1. Target milte hi return kar dena

Wrong:

```cpp
if(nums[mid] == target){
    return mid;
}
```

Kyunki:

```text
target multiple times ho sakta hai.
```

Hume first ya last occurrence chahiye.

---

## 2. First ke liye right jaana

Wrong:

```cpp
if(nums[mid] == target){
    first = mid;
    low = mid + 1;
}
```

Ye last position ki direction hai.

First ke liye:

```cpp
high = mid - 1;
```

---

## 3. Last ke liye left jaana

Wrong:

```cpp
if(nums[mid] == target){
    last = mid;
    high = mid - 1;
}
```

Ye first position ki direction hai.

Last ke liye:

```cpp
low = mid + 1;
```

---

## 4. `upper_bound()` ko directly last position samajhna

Wrong:

```cpp
last = upper_bound(...) - nums.begin();
```

`upper_bound`:

```text
First element > target
```

deta hai.

So:

```cpp
last = upper_bound(...) - nums.begin() - 1;
```

---

## 5. Target Present Hai Ya Nahi Check Na Karna

Sirf:

```cpp
int first = lower_bound(...) - nums.begin();
```

enough nahi hai.

Check:

```cpp
if(first == nums.size() || nums[first] != target)
```

---

# ⏱️ Time Complexity

### First Binary Search

```text
O(log n)
```

### Second Binary Search

```text
O(log n)
```

Total:

```text
O(log n) + O(log n)
```

Therefore:

```text
O(log n)
```

because constants ignore karte hain.

---

# 💾 Space Complexity

Hum koi extra array/map nahi bana rahe.

Only variables:

```text
first
last
low
high
mid
```

Use ho rahe hain.

Therefore:

```text
Space Complexity = O(1)
```

---

# 📊 Complexity

```text
Time Complexity  → O(log n)

Space Complexity → O(1)
```

---

# 🔥 `lower_bound` / `upper_bound` Shortcut

Interview ya competitive programming me agar STL allowed hai:

```cpp
int first =
    lower_bound(nums.begin(), nums.end(), target)
    - nums.begin();

int last =
    upper_bound(nums.begin(), nums.end(), target)
    - nums.begin() - 1;
```

But target existence check zaroor karo:

```cpp
if(first == nums.size() || nums[first] != target){
    return {-1,-1};
}
```

---

# 🧠 Manual Binary Search vs STL

## Manual Binary Search

```text
Pros:
→ Binary Search ka actual logic clear hota hai
→ Interviews me explain kar sakte ho
→ Pattern samajhne ke liye best

Cons:
→ Code thoda lengthy
```

## STL

```text
Pros:
→ Short code
→ Fast implementation
→ Competitive programming me useful

Cons:
→ lower_bound / upper_bound ka concept clearly pata hona chahiye
```

---

# 🔥 Interview Me Kaise Explain Karna Hai?

Agar interviewer puche:

**"How will you solve this problem?"**

Bolna:

```text
Since the array is sorted and the required complexity is O(log n),
I will use binary search.

Because the target can occur multiple times, one normal binary
search is not enough.

I will perform two boundary binary searches.

The first search finds the first occurrence by moving to the
left whenever the target is found.

The second search finds the last occurrence by moving to the
right whenever the target is found.

Therefore the overall time complexity is O(log n) and space
complexity is O(1).
```

---

# 🔥 Quick Revision

```text
Question:
Find First and Last Position

        ↓

Array Sorted?
        ↓
       YES

        ↓

Binary Search

        ↓

First Position
target found
→ answer = mid
→ high = mid - 1

        +

Last Position
target found
→ answer = mid
→ low = mid + 1

        ↓

Return {first,last}
```

---

# 🧠 One-Line Revision

```text
Sorted array me first occurrence ke liye target milne par left search karo, aur last occurrence ke liye target milne par right search karo.
```

---

# ⭐ STL One-Line Revision

```text
lower_bound → first >= target

upper_bound → first > target

first = lower_bound index
last  = upper_bound index - 1
```

---

# ⭐ Interview Revision Code

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {

        int first =
            lower_bound(nums.begin(), nums.end(), target)
            - nums.begin();

        if(first == nums.size() || nums[first] != target){
            return {-1, -1};
        }

        int last =
            upper_bound(nums.begin(), nums.end(), target)
            - nums.begin() - 1;

        return {first, last};
    }
};
```

---

# 🔥 Main Formula

```text
Sorted Array
     +
Binary Search
     +
lower_bound
     +
upper_bound
     ↓
First + Last Position
```

### Remember:

```text
lower_bound → FIRST >= target

upper_bound → FIRST > target

upper_bound - 1 → LAST target
```

---

# 📌 Pattern

```text
Binary Search
    ↓
Boundary Search
    ↓
First / Last Occurrence
    ↓
LC 34
```
