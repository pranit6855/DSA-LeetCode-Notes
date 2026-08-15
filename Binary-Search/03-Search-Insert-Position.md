# LeetCode 35 - Search Insert Position

## 📌 Problem

Hume ek **sorted array** `nums` diya hai aur ek integer `target` diya hai.

Hume target ka index return karna hai.

### Case 1

Agar target already array me present hai:

```text
index return karo
```

### Case 2

Agar target array me present nahi hai:

```text
target ko jis index par insert karna chahiye
taaki array sorted rahe,
woh index return karo.
```

### Important Condition

Array:

```text
ascending order me sorted
```

hai.

Aur hume:

```text
O(log n)
```

time complexity me solve karna hai.

Isliye:

```text
Binary Search
```

use karenge.

---

# 🔹 Example 1

```text
nums = [1,3,5,6]
target = 5
```

Array:

```text
Index:  0  1  2  3
Value:  1  3  5  6
             ↑
           target
```

Target already present hai.

Target ka index:

```text
2
```

So:

```text
Output = 2
```

---

# 🔹 Example 2

```text
nums = [1,3,5,6]
target = 2
```

`2` array me present nahi hai.

Agar `2` ko insert kare:

```text
[1,2,3,5,6]
```

Array sorted rahega.

`2` ka index:

```text
1
```

So:

```text
Output = 1
```

---

# 🔹 Example 3

```text
nums = [1,3,5,6]
target = 7
```

`7` sabse bada hai.

Insert:

```text
[1,3,5,6,7]
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

# 🔹 Example 4

```text
nums = [1,3,5,6]
target = 0
```

`0` sabse chhota hai.

Insert:

```text
[0,1,3,5,6]
```

Index:

```text
0
```

So:

```text
Output = 0
```

---

# 🧠 Main Observation

Question ko dhyan se dekho.

Hume simply target search nahi karna.

Hume:

```text
target ke liye correct insertion position
```

find karni hai.

Actually question ko hum is form me convert kar sakte hain:

```text
First index where nums[index] >= target
```

Ye exactly:

```text
lower_bound
```

ki definition hai.

---

# 🔥 Lower Bound

### Lower Bound ka meaning

```text
First element >= target
```

Example:

```text
nums = [1,3,5,6]
target = 2
```

Check:

```text
1 >= 2 ? No

3 >= 2 ? Yes
```

Pehla element jo `>= 2` hai:

```text
3
```

Uska index:

```text
1
```

So lower bound:

```text
1
```

Aur wahi insertion position hai.

---

# 🔥 Important Connection

Is question ko yaad rakho:

```text
Search Insert Position
        ↓
First index where nums[i] >= target
        ↓
Lower Bound
        ↓
Binary Search
```

---

# 🧠 Binary Search Approach

Hum 3 variables lenge:

```cpp
int low = 0;
int high = nums.size() - 1;
int ans = nums.size();
```

Meaning:

```text
low  → current search range ka left
high → current search range ka right
ans  → best insertion position
```

---

# 🔥 Main Binary Search Logic

Har iteration me:

```cpp
int mid = low + (high - low) / 2;
```

Ab:

```text
nums[mid] >= target
```

check karenge.

---

# Case 1 - `nums[mid] >= target`

Example:

```text
nums[mid] = 5
target = 4
```

Since:

```text
5 >= 4
```

`mid` ek valid answer ho sakta hai.

So:

```cpp
ans = mid;
```

Lekin hume:

```text
FIRST
```

aisa index chahiye.

Ho sakta hai left side me bhi koi element `>= target` ho.

Therefore:

```cpp
high = mid - 1;
```

Meaning:

```text
valid candidate mil gaya
        ↓
aur left me better answer search karo
```

---

# Case 2 - `nums[mid] < target`

Example:

```text
nums[mid] = 3
target = 5
```

Since:

```text
3 < 5
```

Ye index answer nahi ho sakta.

Aur sorted array hone ki wajah se left side ke saare elements bhi:

```text
<= 3
```

honge.

Isliye target ko right side me hi dhoondhna padega.

So:

```cpp
low = mid + 1;
```

---

# 📊 Main Pattern

```text
nums[mid] >= target
        ↓
Possible answer
        ↓
ans = mid
        ↓
LEFT JAO
        ↓
high = mid - 1
```

```text
nums[mid] < target
        ↓
Answer nahi ho sakta
        ↓
RIGHT JAO
        ↓
low = mid + 1
```

Ye hi **lower_bound pattern** hai.

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {

        int low = 0;
        int high = nums.size() - 1;

        int ans = nums.size();

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(nums[mid] >= target) {

                ans = mid;

                // Aur left me first valid position dhundho
                high = mid - 1;
            }

            else {

                // Target right side me hoga
                low = mid + 1;
            }
        }

        return ans;
    }
};
```

---

# 🧠 Code Line By Line

## Step 1

```cpp
int low = 0;
```

Search first index se start hogi.

---

## Step 2

```cpp
int high = nums.size() - 1;
```

Search last index tak hogi.

---

## Step 3

```cpp
int ans = nums.size();
```

Ye important hai.

Example:

```text
nums = [1,3,5,6]
target = 7
```

Agar target sabse bada hai, insertion position:

```text
4
```

hai.

Aur:

```text
nums.size() = 4
```

So initially:

```text
ans = 4
```

rakh sakte hain.

Agar search me koi valid position nahi mili, answer automatically:

```text
n
```

rahega.

---

# 🔥 Why `ans = nums.size()`?

Example:

```text
nums = [1,3,5,6]
target = 7
```

Target har element se bada hai.

So:

```text
nums[mid] >= target
```

kabhi true nahi hoga.

Then `ans` update nahi hoga.

Initial:

```text
ans = 4
```

already correct hai.

Therefore:

```text
Answer = nums.size()
```

---

# 🔥 Dry Run 1

```text
nums = [1,3,5,6]
target = 5
```

Initial:

```text
low = 0
high = 3
ans = 4
```

---

## Iteration 1

```text
mid = 0 + (3-0)/2
    = 1
```

So:

```text
nums[1] = 3
```

Check:

```text
3 >= 5 ? No
```

Therefore:

```text
low = mid + 1
    = 2
```

---

## Iteration 2

Now:

```text
low = 2
high = 3
```

Calculate:

```text
mid = 2 + (3-2)/2
    = 2
```

So:

```text
nums[2] = 5
```

Check:

```text
5 >= 5 ? Yes
```

So:

```text
ans = 2
```

But first position chahiye.

Therefore:

```text
high = mid - 1
      = 1
```

Now:

```text
low = 2
high = 1
```

Loop stop.

Final:

```text
ans = 2
```

Output:

```text
2
```

---

# 🔥 Dry Run 2

```text
nums = [1,3,5,6]
target = 2
```

Initial:

```text
low = 0
high = 3
ans = 4
```

---

## Iteration 1

```text
mid = 1
nums[1] = 3
```

Check:

```text
3 >= 2
```

Yes.

So:

```text
ans = 1
```

Now left jao:

```text
high = 0
```

---

## Iteration 2

```text
low = 0
high = 0
mid = 0
```

```text
nums[0] = 1
```

Check:

```text
1 >= 2
```

No.

So:

```text
low = 1
```

Now:

```text
low = 1
high = 0
```

Loop stop.

Final:

```text
ans = 1
```

Output:

```text
1
```

Correct.

---

# 🔥 Dry Run 3

```text
nums = [1,3,5,6]
target = 7
```

Initial:

```text
low = 0
high = 3
ans = 4
```

---

## Iteration 1

```text
mid = 1
nums[mid] = 3
```

```text
3 >= 7 ? No
```

So:

```text
low = 2
```

---

## Iteration 2

```text
mid = 2
nums[mid] = 5
```

```text
5 >= 7 ? No
```

So:

```text
low = 3
```

---

## Iteration 3

```text
mid = 3
nums[mid] = 6
```

```text
6 >= 7 ? No
```

So:

```text
low = 4
```

Now:

```text
low = 4
high = 3
```

Loop stop.

`ans` kabhi update nahi hua:

```text
ans = 4
```

Final answer:

```text
4
```

---

# 🔥 Dry Run 4

```text
nums = [1,3,5,6]
target = 0
```

Initial:

```text
low = 0
high = 3
ans = 4
```

---

## Iteration 1

```text
mid = 1
nums[1] = 3
```

```text
3 >= 0
```

Yes.

So:

```text
ans = 1
high = 0
```

---

## Iteration 2

```text
low = 0
high = 0
mid = 0
nums[0] = 1
```

```text
1 >= 0
```

Yes.

So:

```text
ans = 0
high = -1
```

Loop stop.

Final:

```text
0
```

Correct insertion position.

---

# 🔥 Why Left Jaa Rahe Hain?

Suppose:

```text
nums = [1,3,5,6]
target = 4
```

Middle:

```text
nums[mid] = 5
```

Since:

```text
5 >= 4
```

`5` valid answer ho sakta hai.

But:

```text
[1,3,5,6]
   ↑
```

`3` target se chhota hai.

So `5` hi first valid value ho sakta hai.

Still, general rule ke according:

```text
high = mid - 1
```

because hume **first** valid index chahiye.

---

# 🔥 Lower Bound Ka Exact Meaning

```text
lower_bound
```

means:

```text
First position where nums[i] >= target
```

Example:

```text
nums = [1,3,3,5,7]
target = 3
```

Positions:

```text
Index:  0 1 2 3 4
Value:  1 3 3 5 7
        ↑
```

First `>= 3`:

```text
index = 1
```

So:

```text
lower_bound(3) = 1
```

---

# 🔥 LC 35 = Lower Bound

Ye connection bahut important hai:

```text
LC 35
    ↓
Search Insert Position
    ↓
First index where nums[i] >= target
    ↓
Lower Bound
```

Therefore:

```text
Search Insert Position
=
Lower Bound Pattern
```

---

# 🧠 Lower Bound vs Normal Binary Search

## Normal Binary Search

Target mil gaya:

```text
return mid;
```

## Lower Bound

Condition:

```text
nums[mid] >= target
```

Target mile ya target se bada mile:

```text
ans = mid;
```

Then:

```text
high = mid - 1;
```

because:

```text
aur left me first valid position ho sakti hai.
```

---

# 📊 Difference

```text
Normal Binary Search

nums[mid] == target
        ↓
    return mid
```

```text
Lower Bound

nums[mid] >= target
        ↓
   ans = mid
        ↓
 high = mid - 1
        ↓
Find FIRST valid position
```

---

# 🔥 Lower Bound Pattern

Is pattern ko ratne ki jagah samajh:

```text
"Valid mila?"
      ↓
    YES
      ↓
answer save karo
      ↓
kya aur left me better/earlier valid mil sakta hai?
      ↓
    YES
      ↓
left jao
```

Code:

```cpp
if(nums[mid] >= target) {
    ans = mid;
    high = mid - 1;
}
```

---

# ⚠️ Common Mistakes

## 1. Target Milte Hi Return Karna

Wrong:

```cpp
if(nums[mid] == target) {
    return mid;
}
```

Ye normal binary search ke liye theek hai.

Lekin yahan hume **first valid position** chahiye.

Example:

```text
nums = [1,3,3,3,5]
target = 3
```

Answer:

```text
1
```

Agar middle par index `2` mila aur return kar diya:

```text
2
```

Wrong.

---

# 2. `>=` Ki Jagah `>`

Wrong:

```cpp
if(nums[mid] > target)
```

Lower bound ke liye:

```cpp
if(nums[mid] >= target)
```

hona chahiye.

Because target khud bhi valid hai.

Example:

```text
nums = [1,3,5]
target = 3
```

Lower bound:

```text
1
```

because:

```text
3 >= 3
```

---

# 3. Valid Milne Par Right Jana

Wrong:

```cpp
if(nums[mid] >= target) {
    ans = mid;
    low = mid + 1;
}
```

Isse tum later valid position ki taraf chale jaoge.

Lower bound me:

```cpp
high = mid - 1;
```

because first valid position left side me ho sakti hai.

---

# 4. `ans = -1` Rakhna

Possible hai, but is question me:

```text
target > all elements
```

hone par answer `n` hona chahiye.

Example:

```text
nums = [1,3,5,6]
target = 7
```

Correct answer:

```text
4
```

Isliye:

```cpp
int ans = nums.size();
```

easy approach hai.

---

# ⏱️ Time Complexity

Har iteration me search space approximately half hota hai.

So:

```text
Time Complexity = O(log n)
```

---

# 💾 Space Complexity

Sirf variables use ho rahe hain:

```text
low
high
mid
ans
```

No extra data structure.

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

# 🔥 STL `lower_bound`

C++ me lower bound already available hai:

```cpp
lower_bound(nums.begin(), nums.end(), target)
```

Ye iterator return karta hai.

Index chahiye:

```cpp
int ans =
    lower_bound(nums.begin(), nums.end(), target)
    - nums.begin();
```

For:

```text
nums = [1,3,5,6]
target = 2
```

returns:

```text
1
```

---

# 💻 STL Solution

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {

        return lower_bound(nums.begin(), nums.end(), target)
               - nums.begin();
    }
};
```

Ye shortest solution hai.

But DSA learning ke liye **manual binary search samajhna zyada important hai**.

---

# 🧠 Manual vs STL

## Manual

```text
low
high
mid
ans
```

Use karke khud lower bound implement karna.

### Best for:

```text
DSA learning
Interviews
Pattern understanding
```

---

## STL

```cpp
lower_bound()
```

Use karna.

### Best for:

```text
Competitive programming
Short implementation
```

---

# 🔥 Important Connection: Ceil

Ceil ka definition:

```text
Smallest element >= target
```

Lower bound:

```text
First element >= target
```

Sorted array me dono same direction use karte hain.

Example:

```text
nums = [1,3,5,6]
target = 4
```

Ceil:

```text
5
```

Lower bound:

```text
index 2
```

So:

```text
Ceil value = nums[lower_bound_index]
```

---

# 🔥 Lower Bound Template

Ye future problems ke liye bahut important hai:

```cpp
int low = 0;
int high = nums.size() - 1;

int ans = nums.size();

while(low <= high) {

    int mid = low + (high - low) / 2;

    if(nums[mid] >= target) {

        ans = mid;
        high = mid - 1;
    }
    else {

        low = mid + 1;
    }
}

return ans;
```

---

# 🧠 Pattern Recognition

Question me agar dikhe:

```text
Sorted Array
+
First position
+
>= target
+
Insertion position
```

Immediately:

```text
LOWER BOUND
```

Think karo.

---

# 🔄 Quick Revision

```text
Sorted Array
      ↓
Need insertion position
      ↓
Find first index where nums[i] >= target
      ↓
LOWER BOUND
      ↓
Binary Search
```

Main condition:

```cpp
if(nums[mid] >= target)
```

Then:

```text
ans = mid
high = mid - 1
```

Otherwise:

```text
low = mid + 1
```

---

# 🧠 One-Line Revision

```text
Search Insert Position me hume first index chahiye jahan nums[index] >= target ho, isliye lower-bound binary search use hota hai.
```

---

# ⭐ Interview Explanation

Interviewer puche:

**"How do you solve Search Insert Position?"**

Bolna:

```text
The array is sorted, so I can use binary search.

The required answer is the first index where nums[index] is
greater than or equal to the target.

This is exactly the lower-bound condition.

Whenever nums[mid] is greater than or equal to target, I store
mid as a possible answer and move left to find an earlier valid
position.

Otherwise, I move right.

The time complexity is O(log n) and the space complexity is O(1).
```

---

# ⭐ Interview Revision Code

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {

        int low = 0;
        int high = nums.size() - 1;

        int ans = nums.size();

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(nums[mid] >= target) {

                ans = mid;
                high = mid - 1;
            }

            else {

                low = mid + 1;
            }
        }

        return ans;
    }
};
```

---

# 🔥 Main Formula

```text
Search Insert Position
        ↓
First index with nums[i] >= target
        ↓
Lower Bound
        ↓
Valid candidate → LEFT
Invalid candidate → RIGHT
        ↓
O(log n)
```

### Remember:

```text
nums[mid] >= target
        ↓
ans = mid
        ↓
high = mid - 1
```

```text
nums[mid] < target
        ↓
low = mid + 1
```

---

# 📌 Pattern

```text
Binary Search
    ↓
Lower Bound
    ↓
Search Insert Position
    ↓
LC 35
```
