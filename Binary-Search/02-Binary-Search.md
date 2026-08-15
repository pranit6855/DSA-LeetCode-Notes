# LeetCode 704 - Binary Search

## 📌 Problem

Hume ek **sorted integer array** `nums` diya hai aur ek `target` value di gayi hai.

Hume target ka index find karna hai.

Agar target array me present hai:

```text
index return karo
```

Agar target present nahi hai:

```text
-1
```

return karo.

### Important Condition

Array **ascending order me sorted** hai.

Aur question specifically:

```text
O(log n)
```

time complexity expect karta hai.

Isliye hume:

```text
Binary Search
```

use karna hai.

---

# 🔹 Example

```text
nums = [-1,0,3,5,9,12]
target = 9
```

Array with index:

```text
Index:  0   1  2  3  4   5
Value: -1   0  3  5  9  12
```

Target:

```text
9
```

Index:

```text
4
```

So:

```text
Output = 4
```

---

# 🧠 What Is Binary Search?

Binary Search ek searching technique hai jisme hum:

```text
search space ko repeatedly half karte hain.
```

Normal Linear Search me:

```text
1 element
2 element
3 element
4 element
...
```

check karte hain.

But Binary Search me:

```text
Middle element check karo
        ↓
Aadha search space eliminate karo
        ↓
Again middle check karo
        ↓
Again half eliminate karo
```

Isi wajah se iska time:

```text
O(log n)
```

hota hai.

---

# 🔥 Why Binary Search Works?

Binary Search tab properly work karta hai jab array:

```text
Sorted
```

ho.

Example:

```text
[1,3,5,7,9,11]
```

Agar target:

```text
9
```

hai aur middle value:

```text
7
```

aayi.

Because:

```text
7 < 9
```

hume pata hai ki target `7` ke left me nahi ho sakta.

So hum pura left half eliminate kar sakte hain.

```text
[1,3,5,7] [9,11]
            ↑
        possible side
```

Yehi Binary Search ka main advantage hai.

---

# 🔥 Main Idea

Har step me 3 variables honge:

```text
low
mid
high
```

Meaning:

```text
low  → search space ka starting index
high → search space ka ending index
mid  → beech ka index
```

Initially:

```cpp
int low = 0;
int high = nums.size() - 1;
```

---

# 🔹 `mid` Kaise Calculate Kare?

Hum usually likhte hain:

```cpp
int mid = (low + high) / 2;
```

Ye logically correct hai.

But safer/general form:

```cpp
int mid = low + (high - low) / 2;
```

Use karte hain.

### Why?

Large values ke case me:

```text
low + high
```

integer overflow cause kar sakta hai.

Isliye:

```cpp
low + (high - low)/2
```

better practice hai.

---

# 🔥 Three Main Cases

Binary Search me har iteration me `nums[mid]` ko target se compare karenge.

---

## Case 1 - Target Mil Gaya

```cpp
if(nums[mid] == target)
```

Meaning:

```text
Current middle element hi target hai.
```

So directly:

```cpp
return mid;
```

---

## Case 2 - `nums[mid] < target`

Example:

```text
nums[mid] = 5
target = 9
```

Since:

```text
5 < 9
```

target right side me hoga.

So:

```cpp
low = mid + 1;
```

Meaning:

```text
mid aur uske left ke elements eliminate.
```

---

## Case 3 - `nums[mid] > target`

Example:

```text
nums[mid] = 9
target = 5
```

Since:

```text
9 > 5
```

target left side me hoga.

So:

```cpp
high = mid - 1;
```

Meaning:

```text
mid aur uske right ke elements eliminate.
```

---

# 📊 Main Logic

```text
nums[mid] == target
        ↓
      return mid

nums[mid] < target
        ↓
   target RIGHT
        ↓
   low = mid + 1

nums[mid] > target
        ↓
   target LEFT
        ↓
  high = mid - 1
```

Ye Binary Search ka sabse important part hai.

---

# 💻 Basic C++ Code

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {

        int low = 0;
        int high = nums.size() - 1;

        while(low <= high){

            int mid = low + (high - low) / 2;

            if(nums[mid] == target){
                return mid;
            }

            else if(nums[mid] < target){
                low = mid + 1;
            }

            else{
                high = mid - 1;
            }
        }

        return -1;
    }
};
```

---

# 🧠 Code Line By Line

## Step 1 - `low`

```cpp
int low = 0;
```

Search array ke first index se start hogi.

---

## Step 2 - `high`

```cpp
int high = nums.size() - 1;
```

Search array ke last index tak hogi.

Example:

```text
nums = [1,3,5,7,9]

size = 5
last index = 4
```

So:

```text
high = 4
```

---

## Step 3 - Loop

```cpp
while(low <= high)
```

Jab tak valid search space exist karta hai tab tak binary search chalegi.

Valid search space:

```text
low <= high
```

Jab:

```text
low > high
```

ho gaya:

```text
Search space empty
```

So target present nahi hai.

---

## Step 4 - Middle

```cpp
int mid = low + (high - low) / 2;
```

Current search range ka middle index mil gaya.

---

## Step 5 - Compare

```cpp
if(nums[mid] == target)
```

Target mil gaya.

So:

```cpp
return mid;
```

---

## Step 6 - Target Right Me

```cpp
else if(nums[mid] < target)
```

Middle value target se chhoti hai.

Because array sorted hai:

```text
Target right side me hi ho sakta hai.
```

So:

```cpp
low = mid + 1;
```

---

## Step 7 - Target Left Me

```cpp
else
```

Means:

```text
nums[mid] > target
```

So target left side me hoga.

```cpp
high = mid - 1;
```

---

# 🔥 Dry Run

Given:

```text
nums = [-1,0,3,5,9,12]
target = 9
```

Array:

```text
Index:  0   1  2  3  4   5
Value: -1   0  3  5  9  12
```

---

## Step 1

Initially:

```text
low = 0
high = 5
```

Calculate:

```text
mid = 0 + (5-0)/2
    = 2
```

So:

```text
nums[mid] = nums[2] = 3
```

Compare:

```text
3 == 9 ? No

3 < 9 ? Yes
```

So target right side me hai.

Update:

```text
low = mid + 1
low = 3
```

Now:

```text
low = 3
high = 5
```

---

## Step 2

Calculate:

```text
mid = 3 + (5-3)/2
    = 4
```

So:

```text
nums[4] = 9
```

Compare:

```text
9 == 9
```

Yes.

Therefore:

```text
return 4
```

Final:

```text
Output = 4
```

---

# 🔥 Search Space Visualization

Initial:

```text
[-1, 0, 3, 5, 9, 12]
  ↑           ↑
 low         high
```

Middle:

```text
[-1, 0, 3, 5, 9, 12]
          ↑
         mid
```

`3 < 9`

So left part eliminate:

```text
[-1, 0, 3] [5, 9, 12]
             ↑       ↑
            low     high
```

Now middle:

```text
[5, 9, 12]
    ↑
   mid
```

`9 == 9`

Answer:

```text
4
```

---

# 🔹 Example 2 - Target Smaller Than Mid

```text
nums = [1,3,5,7,9]
target = 3
```

Initially:

```text
low = 0
high = 4
mid = 2
```

```text
nums[mid] = 5
```

Compare:

```text
5 > 3
```

So target left me hai.

```text
high = mid - 1
high = 1
```

Now search only:

```text
[1,3]
```

Next:

```text
mid = 0
```

`1 < 3`

So:

```text
low = 1
```

Now:

```text
mid = 1
nums[1] = 3
```

Target found.

Output:

```text
1
```

---

# 🔹 Example 3 - Target Not Present

```text
nums = [1,3,5,7,9]
target = 6
```

Initial:

```text
low = 0
high = 4
mid = 2
```

```text
nums[mid] = 5
```

Since:

```text
5 < 6
```

Move right:

```text
low = 3
```

Now:

```text
low = 3
high = 4
mid = 3
```

```text
nums[3] = 7
```

Since:

```text
7 > 6
```

Move left:

```text
high = 2
```

Now:

```text
low = 3
high = 2
```

Condition:

```text
low <= high
```

false.

So loop stops.

Target not found.

Return:

```cpp
return -1;
```

---

# 🧠 Why `while(low <= high)`?

Ye important hai.

Suppose:

```text
low = 3
high = 3
```

There is still:

```text
ONE ELEMENT
```

left.

So usko check karna zaroori hai.

Agar hum likhte:

```cpp
while(low < high)
```

to `low == high` wali single element search miss ho sakti hai.

Therefore standard Binary Search:

```cpp
while(low <= high)
```

---

# 🔥 Why `mid + 1` and `mid - 1`?

Suppose:

```text
nums[mid] < target
```

Hum already check kar chuke hain ki:

```text
nums[mid] != target
```

So `mid` ko dobara check karne ki zarurat nahi.

Therefore:

```cpp
low = mid + 1;
```

Similarly:

```text
nums[mid] > target
```

to `mid` definitely answer nahi hai.

So:

```cpp
high = mid - 1;
```

---

# ❌ Common Mistakes

## 1. `low = mid`

Wrong:

```cpp
low = mid;
```

Correct:

```cpp
low = mid + 1;
```

Because `mid` already check ho chuka hai.

---

## 2. `high = mid`

Wrong:

```cpp
high = mid;
```

Correct:

```cpp
high = mid - 1;
```

Again, `mid` already check ho chuka hai.

---

## 3. Wrong `mid`

Possible:

```cpp
int mid = (low + high) / 2;
```

Ye normal cases me kaam karega.

But safer version:

```cpp
int mid = low + (high - low) / 2;
```

---

## 4. Sorted Condition Ignore Karna

Binary Search directly kisi bhi unsorted array par nahi laga sakte.

Example:

```text
[7,2,9,1,5]
```

Yahan agar:

```text
mid = 9
```

aur target `5` hai, to sirf comparison se ye decide nahi kar sakte ki target kis side hai.

Array sorted hona important hai.

---

# 🔥 Binary Search Ka Core Pattern

```text
low
 ↓
[ search space ]
             ↑
            high
```

Middle find karo:

```text
mid
```

Then:

```text
target == nums[mid]
        ↓
      found
```

Otherwise:

```text
target > nums[mid]
        ↓
      RIGHT
        ↓
low = mid + 1
```

or:

```text
target < nums[mid]
        ↓
       LEFT
        ↓
high = mid - 1
```

---

# 🔥 Linear Search vs Binary Search

## Linear Search

```text
Array:
[1,3,5,7,9,11]

Target = 11

Check:
1
3
5
7
9
11
```

Worst case:

```text
O(n)
```

---

## Binary Search

```text
[1,3,5,7,9,11]
        ↓
      middle
        ↓
   eliminate half
        ↓
   eliminate half
        ↓
      target
```

Worst case:

```text
O(log n)
```

---

# 📊 Complexity

### Time Complexity

Har iteration me approximately half elements eliminate hote hain.

So:

```text
Time Complexity = O(log n)
```

### Space Complexity

Extra data structure use nahi ho rahi.

Sirf:

```text
low
high
mid
```

variables hain.

So:

```text
Space Complexity = O(1)
```

---

# 📌 Complexity

```text
Time Complexity  → O(log n)

Space Complexity → O(1)
```

---

# 🧠 Pattern Recognition

Question dekhte hi ye check karo:

```text
1. Array sorted hai?
        ↓
       YES

2. Search / target find karna hai?
        ↓
       YES

3. O(log n) expected hai?
        ↓
       YES
```

Then:

```text
BINARY SEARCH
```

---

# 🔥 Binary Search Template

Ye template future questions ke liye yaad rakho:

```cpp
int low = 0;
int high = nums.size() - 1;

while(low <= high){

    int mid = low + (high - low) / 2;

    if(nums[mid] == target){
        return mid;
    }

    else if(nums[mid] < target){
        low = mid + 1;
    }

    else{
        high = mid - 1;
    }
}

return -1;
```

Is template ko samajhna important hai rather than sirf ratna.

---

# 🔥 Important Mental Model

Binary Search ko aise socho:

```text
Mujhe target search karna hai.

Middle dekha.

        ↓

Agar target middle hai:
DONE

        ↓

Agar target middle se bada hai:
LEFT HALF impossible

        ↓

Agar target middle se chhota hai:
RIGHT HALF impossible
```

Har step me:

```text
Half search space delete
```

hota hai.

---

# 🔄 Quick Revision

```text
Sorted Array
      ↓
low = 0
high = n - 1
      ↓
while(low <= high)
      ↓
mid calculate
      ↓
nums[mid] == target ?
   /             \
 YES              NO
  ↓                ↓
return         Compare
                  ↓
       ┌──────────┴──────────┐
       ↓                     ↓
nums[mid] < target    nums[mid] > target
       ↓                     ↓
low = mid + 1         high = mid - 1
```

---

# 🧠 One-Line Revision

```text
Sorted array me middle element ko target se compare karke har step me aadha search space eliminate karna Binary Search hai.
```

---

# ⭐ Interview Explanation

Interviewer puche:

**"How does binary search work?"**

Bolna:

```text
Binary search works on a sorted array.

I maintain a search range using low and high.
For every iteration, I calculate the middle index and compare
the middle element with the target.

If the middle element equals the target, I return the index.

If the middle element is smaller than the target, I search the
right half.

If the middle element is greater than the target, I search the
left half.

Since half of the search space is eliminated in every iteration,
the time complexity is O(log n) and the space complexity is O(1).
```

---

# 🔥 Main Formula

```text
Sorted Array
      +
Middle Element
      +
Compare With Target
      +
Eliminate Half
      ↓
Binary Search
      ↓
O(log n)
```

---

# 📌 Pattern

```text
Binary Search
    ↓
Basic Binary Search
    ↓
LC 704
```

---

# ⭐ Final Code

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {

        int low = 0;
        int high = nums.size() - 1;

        while(low <= high){

            int mid = low + (high - low) / 2;

            if(nums[mid] == target){
                return mid;
            }

            else if(nums[mid] < target){
                low = mid + 1;
            }

            else{
                high = mid - 1;
            }
        }

        return -1;
    }
};
```
