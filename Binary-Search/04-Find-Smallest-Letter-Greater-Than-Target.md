# LeetCode 744 - Find Smallest Letter Greater Than Target

## 📌 Problem

Hume ek sorted character array `letters` diya hai aur ek character `target` diya hai.

Hume:

```text
target se strictly greater
sabse chhota character
```

return karna hai.

### Important

Condition:

```text
letters[i] > target
```

hona chahiye.

`>=` nahi.

Agar target ke baad koi greater character nahi milta, to:

```text
first character
```

return karna hai.

Isko **wrap around** kehte hain.

---

# 🔹 Example 1

```text
letters = ['c','f','j']
target = 'a'
```

Target:

```text
'a'
```

Greater letters:

```text
'c'
'f'
'j'
```

Inme smallest:

```text
'c'
```

So:

```text
Output = 'c'
```

---

# 🔹 Example 2

```text
letters = ['c','f','j']
target = 'c'
```

Hume strictly greater chahiye:

```text
'c' > 'c' ❌
```

Next:

```text
'f' > 'c' ✅
```

So:

```text
Output = 'f'
```

---

# 🔹 Example 3

```text
letters = ['c','f','j']
target = 'f'
```

`f` khud greater nahi hai:

```text
'f' > 'f' ❌
```

Next:

```text
'j' > 'f' ✅
```

So:

```text
Output = 'j'
```

---

# 🔹 Example 4 - Wrap Around

```text
letters = ['c','f','j']
target = 'j'
```

Check:

```text
'c' > 'j' ❌
'f' > 'j' ❌
'j' > 'j' ❌
```

Koi greater letter nahi mila.

Question ke according first letter return karna hai:

```text
Output = 'c'
```

So concept:

```text
[c, f, j]
       ↑
    target

No greater element
        ↓
   wrap around
        ↓
      'c'
```

---

# 🧠 Main Observation

Array sorted hai:

```text
['c','f','j']
```

Aur hume:

```text
smallest character > target
```

chahiye.

Ye exactly:

```text
Upper Bound
```

pattern hai.

---

# 🔥 Upper Bound

Upper bound ka meaning:

```text
First element > target
```

Notice:

### Lower Bound

```text
First element >= target
```

### Upper Bound

```text
First element > target
```

LC 35:

```text
Search Insert Position
        ↓
Lower Bound
```

LC 744:

```text
Next Greatest Letter
        ↓
Upper Bound
```

---

# 🔥 Main Binary Search Pattern

Har iteration me:

```cpp
if(letters[mid] > target)
```

check karenge.

Agar true:

```text
mid ek valid answer hai
```

So:

```cpp
ans = letters[mid];
```

But hume:

```text
smallest greater element
```

chahiye.

Isliye aur left me search karenge:

```cpp
high = mid - 1;
```

---

# 🔹 Case 1 - `letters[mid] > target`

Example:

```text
letters[mid] = 'j'
target = 'f'
```

Since:

```text
'j' > 'f'
```

`j` valid answer hai.

But ho sakta hai left me:

```text
'g'
```

jaisa smaller greater character ho.

So:

```cpp
ans = letters[mid];
high = mid - 1;
```

Meaning:

```text
Valid mila
   ↓
answer save karo
   ↓
left jao
   ↓
better/smaller valid answer dhundho
```

---

# 🔹 Case 2 - `letters[mid] <= target`

Example:

```text
letters[mid] = 'f'
target = 'f'
```

Check:

```text
'f' > 'f' ❌
```

So ye valid answer nahi hai.

Target ko right side me search karna padega:

```cpp
low = mid + 1;
```

Same:

```text
letters[mid] < target
```

me bhi right side jana hai.

So complete condition:

```text
letters[mid] <= target
        ↓
RIGHT
```

---

# 📊 Main Logic

```text
letters[mid] > target
        ↓
   Valid candidate
        ↓
     ans = mid
        ↓
   high = mid - 1
```

```text
letters[mid] <= target
        ↓
  Invalid candidate
        ↓
   low = mid + 1
```

Ye exact **upper bound** pattern hai.

---

# 💻 C++ Code

```cpp
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {

        int low = 0;
        int high = letters.size() - 1;

        char ans = letters[0];

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(letters[mid] > target) {

                ans = letters[mid];

                // Aur chhota greater character dhundho
                high = mid - 1;
            }

            else {

                // letters[mid] <= target
                low = mid + 1;
            }
        }

        return ans;
    }
};
```

---

# 🧠 Code Line By Line

## Step 1 - `low`

```cpp
int low = 0;
```

Search first character se start hogi.

---

## Step 2 - `high`

```cpp
int high = letters.size() - 1;
```

Last valid index:

```text
n - 1
```

hota hai.

Important:

```cpp
int high = letters.size();
```

nahi karna, because:

```text
last valid index = size - 1
```

---

## Step 3 - Initial Answer

```cpp
char ans = letters[0];
```

Ye wrap-around handle karta hai.

Agar greater character nahi mila:

```text
answer = first character
```

Example:

```text
letters = ['c','f','j']
target = 'j'
```

No greater element.

So:

```text
ans = 'c'
```

already correct hai.

---

# 🔥 Why `char ans = letters[0]`?

Without wrap-around:

```text
target = 'j'
```

ke liye koi valid candidate nahi milega.

Agar:

```cpp
char ans;
```

rakha aur kabhi update nahi hua, to answer wrong/uninitialized ho sakta hai.

So:

```cpp
char ans = letters[0];
```

best hai.

---

# 🔥 Character Comparison Kaise Ho Raha Hai?

C++ me `char` comparison character ki underlying numeric/ASCII value ke basis par hota hai.

For lowercase English letters:

```text
'a' < 'b' < 'c' < ... < 'z'
```

For example:

```text
'a' = 97
'c' = 99
'f' = 102
'j' = 106
```

Isliye:

```cpp
'c' < 'f'
```

true hai.

Hume manually ASCII value nikalne ki zarurat nahi.

Direct:

```cpp
letters[mid] > target
```

likhna enough hai.

---

# 🔥 Dry Run 1

Given:

```text
letters = ['c','f','j']
target = 'a'
```

Initial:

```text
low = 0
high = 2
ans = 'c'
```

Array:

```text
Index:    0    1    2
Value:   'c'  'f'  'j'
```

---

## Iteration 1

```text
mid = 0 + (2-0)/2
    = 1
```

So:

```text
letters[mid] = 'f'
```

Check:

```text
'f' > 'a'
```

Yes.

So:

```text
ans = 'f'
```

But `f` smallest greater nahi ho sakta because left me `c` hai.

So:

```text
high = mid - 1
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
letters[mid] = 'c'
```

Check:

```text
'c' > 'a'
```

Yes.

So:

```text
ans = 'c'
```

Now:

```text
high = -1
```

Loop stop.

Final:

```text
Output = 'c'
```

---

# 🔥 Dry Run 2

```text
letters = ['c','f','j']
target = 'f'
```

Initial:

```text
low = 0
high = 2
ans = 'c'
```

---

## Iteration 1

```text
mid = 1
letters[mid] = 'f'
```

Check:

```text
'f' > 'f'
```

False.

Because target ke equal hai.

So:

```text
low = mid + 1
low = 2
```

---

## Iteration 2

```text
mid = 2
letters[mid] = 'j'
```

Check:

```text
'j' > 'f'
```

True.

So:

```text
ans = 'j'
```

Then:

```text
high = 1
```

Loop stop.

Final:

```text
Output = 'j'
```

---

# 🔥 Dry Run 3 - Wrap Around

```text
letters = ['c','f','j']
target = 'j'
```

Initial:

```text
ans = 'c'
```

---

## Iteration 1

```text
mid = 1
letters[mid] = 'f'
```

Check:

```text
'f' > 'j'
```

False.

So:

```text
low = 2
```

---

## Iteration 2

```text
mid = 2
letters[mid] = 'j'
```

Check:

```text
'j' > 'j'
```

False.

So:

```text
low = 3
```

Loop stop.

Kabhi bhi:

```text
ans
```

update nahi hua.

Initial value:

```text
ans = 'c'
```

return hogi.

Therefore:

```text
Output = 'c'
```

---

# 🔥 Why Strictly Greater?

Question kehta hai:

```text
smallest letter greater than target
```

So:

```text
>
```

use hoga.

Not:

```text
>=
```

---

# ⚠️ `>` vs `>=`

### Upper Bound

```cpp
letters[mid] > target
```

Means:

```text
First element strictly greater than target
```

### Lower Bound

```cpp
letters[mid] >= target
```

Means:

```text
First element greater than OR equal to target
```

---

# 📊 Lower Bound vs Upper Bound

| Pattern     | Condition             | Direction After Valid |
| ----------- | --------------------- | --------------------- |
| Lower Bound | `nums[mid] >= target` | Left                  |
| Upper Bound | `nums[mid] > target`  | Left                  |

Difference sirf equality ka hai.

```text
Lower Bound:
>=

Upper Bound:
>
```

---

# 🔥 LC 35 vs LC 744

## LC 35 - Search Insert Position

Need:

```text
First index where nums[i] >= target
```

Pattern:

```cpp
if(nums[mid] >= target)
```

So:

```text
Lower Bound
```

---

## LC 744 - Next Greatest Letter

Need:

```text
First character where letters[i] > target
```

Pattern:

```cpp
if(letters[mid] > target)
```

So:

```text
Upper Bound
```

---

# 🧠 Most Important Difference

```text
LC 35
target equal allowed
        ↓
>=
        ↓
Lower Bound
```

```text
LC 744
target equal NOT allowed
        ↓
>
        ↓
Upper Bound
```

---

# 🔥 Why Left Search After Finding Candidate?

Suppose:

```text
letters = ['c','f','j']
target = 'a'
```

Mid:

```text
'f'
```

`f > a`, so valid.

But hume **smallest** greater chahiye.

Left me:

```text
'c'
```

bhi greater hai.

So:

```text
high = mid - 1;
```

karke left search karte hain.

Same idea:

```text
Valid candidate mila
        ↓
aur better candidate possible?
        ↓
YES
        ↓
LEFT
```

---

# 🔥 Pattern Recognition

Question me agar ye dikhe:

```text
Sorted Array
+
Smallest element
+
Strictly greater than target
```

Immediately think:

```text
Upper Bound
```

Binary Search pattern:

```text
letters[mid] > target
        ↓
ans = mid
        ↓
high = mid - 1
```

---

# ❌ Common Mistakes

## 1. `>=` Use Karna

Wrong:

```cpp
if(letters[mid] >= target)
```

Example:

```text
letters = ['c','f','j']
target = 'f'
```

Isse `f` answer ban jayega.

But question says:

```text
strictly greater
```

Correct answer:

```text
j
```

So correct condition:

```cpp
letters[mid] > target
```

---

## 2. `high = letters.size()`

Wrong:

```cpp
int high = letters.size();
```

Correct:

```cpp
int high = letters.size() - 1;
```

Because last valid index:

```text
size - 1
```

---

## 3. `'target'` Likhnа

Wrong:

```cpp
letters[mid] > 'target'
```

Correct:

```cpp
letters[mid] > target
```

`target` already ek `char` variable hai.

Quotes character literals ke liye hoti hain:

```cpp
'a'
'f'
'z'
```

---

## 4. Valid Milte Hi Return Karna

Wrong:

```cpp
if(letters[mid] > target) {
    return letters[mid];
}
```

Kyunki ho sakta hai left side me smaller valid character ho.

Example:

```text
['c','f','j']
target = 'a'
```

Agar `f` mila aur return kar diya:

```text
f
```

Wrong.

Correct:

```text
f valid
↓
save
↓
left search
↓
c mil sakta hai
```

---

## 5. Wrap Around Bhoolna

Example:

```text
letters = ['c','f','j']
target = 'j'
```

No greater character.

Answer:

```text
'c'
```

Isliye:

```cpp
char ans = letters[0];
```

useful hai.

---

# 🔥 Alternative Thinking

Is question ko mathematically:

```text
Find first index i such that:

letters[i] > target
```

samajh sakte ho.

Agar aisa index nahi mila:

```text
return letters[0]
```

So:

```text
LC 744
=
Upper Bound
+
Wrap Around
```

---

# 💻 STL `upper_bound`

C++ me directly:

```cpp
upper_bound()
```

available hai.

Definition:

```text
First element > target
```

So conceptually:

```cpp
int index =
    upper_bound(letters.begin(), letters.end(), target)
    - letters.begin();
```

Problem:

Agar:

```text
index == letters.size()
```

to greater character exist nahi karta.

Then:

```cpp
return letters[0];
```

Otherwise:

```cpp
return letters[index];
```

---

# 💻 STL Solution

```cpp
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {

        int index =
            upper_bound(letters.begin(), letters.end(), target)
            - letters.begin();

        if(index == letters.size()) {
            return letters[0];
        }

        return letters[index];
    }
};
```

---

# 🧠 Manual Binary Search vs STL

## Manual

```text
low
high
mid
ans
```

use karke upper bound khud implement karte hain.

### Learning ke liye:

```text
BEST
```

because pattern clearly samajh aata hai.

---

## STL

```cpp
upper_bound()
```

directly use karte hain.

### Competitive programming ke liye:

```text
Short and convenient
```

---

# ⏱️ Time Complexity

Har iteration me search space half hota hai.

Therefore:

```text
Time Complexity = O(log n)
```

---

# 💾 Space Complexity

Extra data structure nahi hai.

Sirf:

```text
low
high
mid
ans
```

variables hain.

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

# 🔄 Quick Revision

```text
Sorted Characters
       ↓
Need smallest character > target
       ↓
UPPER BOUND
       ↓
letters[mid] > target ?
       ↓
YES → ans = mid
      high = mid - 1

NO  → low = mid + 1
       ↓
No answer?
       ↓
return letters[0]
```

---

# 🧠 One-Line Revision

```text
Sorted letters me target se strictly greater smallest character find karna hai, isliye upper-bound binary search use karo; agar koi greater character na mile to first letter return karo.
```

---

# ⭐ Interview Explanation

Interviewer puche:

**"How will you solve this problem?"**

Bolna:

```text
The letters are sorted, so I can use binary search.

I need the smallest character that is strictly greater than
the target, which is an upper-bound problem.

Whenever letters[mid] is greater than the target, I store it as
a possible answer and move left to find a smaller valid character.

Otherwise, I move right.

I initialize the answer with the first character to handle the
wrap-around case when no character greater than the target exists.

The time complexity is O(log n) and the space complexity is O(1).
```

---

# ⭐ Interview Revision Code

```cpp
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {

        int low = 0;
        int high = letters.size() - 1;

        char ans = letters[0];

        while(low <= high) {

            int mid = low + (high - low) / 2;

            if(letters[mid] > target) {

                ans = letters[mid];
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
LC 744
      ↓
Sorted Characters
      ↓
Smallest element > target
      ↓
Upper Bound
      ↓
Valid → LEFT
Invalid → RIGHT
      ↓
No valid element
      ↓
Wrap Around → First Character
```

### Remember:

```text
Lower Bound → first element >= target

Upper Bound → first element > target
```

---

# 📌 Pattern

```text
Binary Search
    ↓
Boundary Search
    ↓
Upper Bound
    ↓
Next Greatest Letter
    ↓
LC 744
```
