# LeetCode 540 - Single Element in a Sorted Array

## 📌 Problem

Hume ek **sorted array** `nums` diya hai.

Array me:

```text
- Har element exactly 2 baar aata hai
- Sirf ek element exactly 1 baar aata hai
```

Hume woh **single element** find karna hai.

Required complexity:

```text
O(log n)
```

Isliye hume **Binary Search** use karna hai.

---

# 🔹 Example

```text
nums = [1,1,2,3,3,4,4,8,8]
```

Pairs:

```text
1 → 2 times
2 → 1 time
3 → 2 times
4 → 2 times
8 → 2 times
```

Single element:

```text
2
```

Output:

```text
2
```

---

# 🧠 Brute Force Approach

Sabse simple approach me har element ko check kar sakte hain.

Agar current element:

```text
left wale se different
AND
right wale se different
```

hai, to wahi single element hoga.

Example:

```text
[1,1,2,3,3]
      ↑
```

`2` ke left me `1` aur right me `3` hai, dono different.

So answer:

```text
2
```

### Complexity

```text
Time  = O(n)
Space = O(1)
```

Lekin question `O(log n)` maang raha hai.

Isliye Binary Search use karenge.

---

# 🔥 Binary Search Ka Main Observation

Array sorted hai aur pairs bane hue hain.

Single element se pehle pairs ka pattern:

```text
Index:  0 1 | 2 3 | 4 5 | 6 7
```

Matlab:

```text
0,1  → pair
2,3  → pair
4,5  → pair
6,7  → pair
```

Pair ka first index hamesha:

```text
0,2,4,6...
```

yaani **even index** hota hai.

---

# 🔥 Single Element Ki Wajah Se Pairing Break Hoti Hai

Example:

```text
nums = [1,1,2,3,3,4,4]
```

Index:

```text
0 1 | 2 | 3 4 | 5 6
1 1 | 2 | 3 3 | 4 4
```

Single element `2` ki wajah se pairs ka normal alignment toot gaya.

Single se pehle:

```text
0,1
```

aur:

```text
```

expected pair `2,3` hota, lekin `2` single hai.

Uske baad:

```text
3,4
5,6
```

pairs shift ho gaye.

---

# 🔥 `mid` Ko Even Kyu Banate Hain?

Binary Search se `mid` odd bhi aa sakta hai.

Suppose:

```text
mid = 5
```

Lekin pair ka first index hume chahiye.

Pair ke first indexes:

```text
0,2,4,6...
```

hote hain.

So agar:

```cpp
if(mid % 2 == 1)
    mid--;
```

to:

```text
5 → 4
```

Ab `mid` even ho gaya.

Ab hum:

```cpp
nums[mid]
```

aur:

```cpp
nums[mid + 1]
```

ko ek pair ke form me check kar sakte hain.

Example:

```text
mid = 4

nums[4]
nums[5]
```

Ye expected pair hai.

---

# 🧠 Main Pair Check

Ab:

```cpp
nums[mid] == nums[mid + 1]
```

check karenge.

Do cases hain.

---

# ✅ Case 1 - Proper Pair

Agar:

```cpp
nums[mid] == nums[mid + 1]
```

hai, to pair complete hai.

Example:

```text
Index: 2 3
Value: 3 3
```

Dono same hain.

Matlab:

```text
mid tak pairs properly arranged hain.
```

Single element is pair ke baad hoga.

Therefore:

```cpp
low = mid + 2;
```

Hum poore pair ko skip kar dete hain.

---

# 🔥 Why `mid + 2`?

Because:

```text
mid
mid + 1
```

dono ek complete pair hain.

In dono ke baad hi next possible single ho sakta hai.

So:

```cpp
low = mid + 2;
```

---

# ❌ Case 2 - Pair Broken

Agar:

```cpp
nums[mid] != nums[mid + 1]
```

hai, to pair complete nahi hai.

Example:

```text
Index: 2 3
Value: 2 3
```

Normally `2` aur `3` same pair nahi hain.

Matlab single element:

```text
mid
```

par ho sakta hai, ya left side me.

Therefore:

```cpp
high = mid;
```

---

# ⚠️ `high = mid` Kyu, `mid - 1` Kyu Nahi?

Because `mid` khud single element ho sakta hai.

Example:

```text
[1,1,2,3,3]
      ↑
     mid
```

`2` hi single hai.

Agar:

```cpp
high = mid - 1;
```

karoge, to `2` ko hi remove kar doge.

Isliye:

```cpp
high = mid;
```

correct hai.

---

# 🔥 Complete Logic

```text
1. mid find karo

2. Agar mid odd hai:
       mid--

3. nums[mid] == nums[mid+1] ?

   YES:
       Pair complete hai
       Single RIGHT me
       low = mid + 2

   NO:
       Pair broken hai
       Single LEFT ya MID me
       high = mid

4. low == high
       ↓
   Answer = nums[low]
```

---

# 🔄 Dry Run

Given:

```text
nums = [1,1,2,3,3,4,4]
```

Initial:

```text
low = 0
high = 6
```

---

## Iteration 1

```text
mid = 0 + (6-0)/2
    = 3
```

`mid = 3` odd hai.

So:

```text
mid--
mid = 2
```

Compare:

```text
nums[2] = 2
nums[3] = 3
```

Same?

```text
No
```

So pair broken hai.

Single:

```text
mid ya left
```

me hai.

Therefore:

```text
high = mid
high = 2
```

Now:

```text
low = 0
high = 2
```

---

## Iteration 2

```text
mid = 0 + (2-0)/2
    = 1
```

`mid = 1` odd hai.

So:

```text
mid--
mid = 0
```

Compare:

```text
nums[0] = 1
nums[1] = 1
```

Same hain.

Ye proper pair hai.

So single right me hai:

```text
low = mid + 2
low = 2
```

Now:

```text
low = 2
high = 2
```

Loop stop.

Final:

```text
nums[2] = 2
```

Answer:

```text
2
```

---

# 🔥 Second Dry Run

```text
nums = [1,1,2,2,3,4,4,5,5]
```

Single:

```text
3
```

Initial:

```text
low = 0
high = 8
```

### Iteration 1

```text
mid = 4
```

Compare:

```text
nums[4] = 3
nums[5] = 4
```

Different.

Pair broken.

So:

```text
high = 4
```

---

### Iteration 2

```text
low = 0
high = 4
mid = 2
```

Compare:

```text
nums[2] = 2
nums[3] = 2
```

Same.

Proper pair.

So:

```text
low = mid + 2
low = 4
```

Now:

```text
low = 4
high = 4
```

Answer:

```text
nums[4] = 3
```

---

# 🧠 Why Proper Pair Means Single Right?

Suppose:

```text
nums[mid] == nums[mid+1]
```

and `mid` even hai.

Matlab:

```text
mid, mid+1
```

ek complete pair hai.

Aur kyunki array sorted hai, us pair se pehle jo pairs hain wo bhi correctly aligned hain.

Agar single left me hota, to pairing pehle hi break ho gayi hoti.

So:

```text
Proper pair
    ↓
Left side correctly paired
    ↓
Single must be RIGHT
```

---

# 🧠 Why Broken Pair Means Single Left/Mid?

Suppose:

```text
nums[mid] != nums[mid+1]
```

`mid` even hai.

Normally yahan pair complete hona chahiye tha.

Lekin pair complete nahi hua.

Matlab:

```text
Single yahin hai
```

ya:

```text
Single isse pehle hai
```

So:

```cpp
high = mid;
```

---

# 💻 Binary Search Code

```cpp
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {

        int low = 0;
        int high = nums.size() - 1;

        while(low < high) {

            int mid = low + (high - low) / 2;

            // Pair ka first index even hona chahiye
            if(mid % 2 == 1) {
                mid--;
            }

            // Proper pair
            if(nums[mid] == nums[mid + 1]) {

                // Single right side me hai
                low = mid + 2;
            }

            else {

                // Pair broken hai
                // Single mid ya left side me hai
                high = mid;
            }
        }

        return nums[low];
    }
};
```

---

# 🧠 Code Ka Main Part

```cpp
if(mid % 2 == 1) {
    mid--;
}
```

Meaning:

```text
mid odd hai
→ 1 kam karo
→ mid even banao
```

Then:

```cpp
if(nums[mid] == nums[mid + 1])
```

Meaning:

```text
Kya expected pair complete hai?
```

---

# ⚠️ Common Mistakes

## 1. `high = nums.size()`

Wrong:

```cpp
int high = nums.size();
```

Correct:

```cpp
int high = nums.size() - 1;
```

Last valid index:

```text
size - 1
```

hota hai.

---

## 2. `mid` Odd Hone Par Kuch Na Karna

Galat:

```text
mid = 5
```

and directly:

```text
nums[5], nums[6]
```

compare karna.

Hume pair ke first index se start karna hai.

Correct:

```cpp
if(mid % 2 == 1)
    mid--;
```

---

## 3. Proper Pair Milne Par `low = mid + 1`

Wrong:

```cpp
low = mid + 1;
```

Correct:

```cpp
low = mid + 2;
```

Because:

```text
mid
mid+1
```

dono complete pair hain.

---

## 4. Broken Pair Par `high = mid - 1`

Wrong:

```cpp
high = mid - 1;
```

Correct:

```cpp
high = mid;
```

Because `mid` khud single ho sakta hai.

---

## 5. `while(low <= high)`

Is approach me:

```cpp
while(low < high)
```

use karte hain.

Kyunki goal hai search space ko ek single position tak narrow karna.

End:

```text
low == high
```

par answer mil jata hai.

---

# 📊 Complexity

### Time Complexity

Har iteration me roughly half search space remove hota hai.

```text
O(log n)
```

### Space Complexity

Extra data structure nahi:

```text
O(1)
```

---

# 🔥 Pattern Recognition

Question me:

```text
Sorted Array
+
Every element appears twice
+
One element appears once
+
O(log n)
```

dikhe to:

```text
Binary Search + Pair Pattern
```

think karo.

---

# 🧠 Quick Revision

```text
mid nikalo
   ↓
mid odd?
   ↓
mid--
   ↓
nums[mid] == nums[mid+1] ?
   ↓
YES
→ proper pair
→ single RIGHT
→ low = mid+2

NO
→ pair broken
→ single LEFT/MID
→ high = mid

low == high
→ nums[low]
```

---

# ⭐ One-Line Revision

```text
Mid ko even index par lao, phir mid aur mid+1 ko compare karo; pair complete hai to single right me hai, warna single left ya mid par hai.
```

---

# ⭐ Final Interview Code

```cpp
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {

        int low = 0;
        int high = nums.size() - 1;

        while(low < high) {

            int mid = low + (high - low) / 2;

            if(mid % 2 == 1) {
                mid--;
            }

            if(nums[mid] == nums[mid + 1]) {
                low = mid + 2;
            }
            else {
                high = mid;
            }
        }

        return nums[low];
    }
};
```

---

# 📌 Pattern

```text
Binary Search
    ↓
Sorted Array
    ↓
Pair Pattern
    ↓
Even / Odd Index
    ↓
Single Element
    ↓
LC 540
```
# LeetCode 540 - Single Element in a Sorted Array

## 📌 Problem

Hume ek **sorted array** `nums` diya hai.

Array me:

```text
- Har element exactly 2 baar aata hai
- Sirf ek element exactly 1 baar aata hai
```

Hume woh **single element** find karna hai.

Required complexity:

```text
O(log n)
```

Isliye hume **Binary Search** use karna hai.

---

# 🔹 Example

```text
nums = [1,1,2,3,3,4,4,8,8]
```

Pairs:

```text
1 → 2 times
2 → 1 time
3 → 2 times
4 → 2 times
8 → 2 times
```

Single element:

```text
2
```

Output:

```text
2
```

---

# 🧠 Brute Force Approach

Sabse simple approach me har element ko check kar sakte hain.

Agar current element:

```text
left wale se different
AND
right wale se different
```

hai, to wahi single element hoga.

Example:

```text
[1,1,2,3,3]
      ↑
```

`2` ke left me `1` aur right me `3` hai, dono different.

So answer:

```text
2
```

### Complexity

```text
Time  = O(n)
Space = O(1)
```

Lekin question `O(log n)` maang raha hai.

Isliye Binary Search use karenge.

---

# 🔥 Binary Search Ka Main Observation

Array sorted hai aur pairs bane hue hain.

Single element se pehle pairs ka pattern:

```text
Index:  0 1 | 2 3 | 4 5 | 6 7
```

Matlab:

```text
0,1  → pair
2,3  → pair
4,5  → pair
6,7  → pair
```

Pair ka first index hamesha:

```text
0,2,4,6...
```

yaani **even index** hota hai.

---

# 🔥 Single Element Ki Wajah Se Pairing Break Hoti Hai

Example:

```text
nums = [1,1,2,3,3,4,4]
```

Index:

```text
0 1 | 2 | 3 4 | 5 6
1 1 | 2 | 3 3 | 4 4
```

Single element `2` ki wajah se pairs ka normal alignment toot gaya.

Single se pehle:

```text
0,1
```

aur:

```text
```

expected pair `2,3` hota, lekin `2` single hai.

Uske baad:

```text
3,4
5,6
```

pairs shift ho gaye.

---

# 🔥 `mid` Ko Even Kyu Banate Hain?

Binary Search se `mid` odd bhi aa sakta hai.

Suppose:

```text
mid = 5
```

Lekin pair ka first index hume chahiye.

Pair ke first indexes:

```text
0,2,4,6...
```

hote hain.

So agar:

```cpp
if(mid % 2 == 1)
    mid--;
```

to:

```text
5 → 4
```

Ab `mid` even ho gaya.

Ab hum:

```cpp
nums[mid]
```

aur:

```cpp
nums[mid + 1]
```

ko ek pair ke form me check kar sakte hain.

Example:

```text
mid = 4

nums[4]
nums[5]
```

Ye expected pair hai.

---

# 🧠 Main Pair Check

Ab:

```cpp
nums[mid] == nums[mid + 1]
```

check karenge.

Do cases hain.

---

# ✅ Case 1 - Proper Pair

Agar:

```cpp
nums[mid] == nums[mid + 1]
```

hai, to pair complete hai.

Example:

```text
Index: 2 3
Value: 3 3
```

Dono same hain.

Matlab:

```text
mid tak pairs properly arranged hain.
```

Single element is pair ke baad hoga.

Therefore:

```cpp
low = mid + 2;
```

Hum poore pair ko skip kar dete hain.

---

# 🔥 Why `mid + 2`?

Because:

```text
mid
mid + 1
```

dono ek complete pair hain.

In dono ke baad hi next possible single ho sakta hai.

So:

```cpp
low = mid + 2;
```

---

# ❌ Case 2 - Pair Broken

Agar:

```cpp
nums[mid] != nums[mid + 1]
```

hai, to pair complete nahi hai.

Example:

```text
Index: 2 3
Value: 2 3
```

Normally `2` aur `3` same pair nahi hain.

Matlab single element:

```text
mid
```

par ho sakta hai, ya left side me.

Therefore:

```cpp
high = mid;
```

---

# ⚠️ `high = mid` Kyu, `mid - 1` Kyu Nahi?

Because `mid` khud single element ho sakta hai.

Example:

```text
[1,1,2,3,3]
      ↑
     mid
```

`2` hi single hai.

Agar:

```cpp
high = mid - 1;
```

karoge, to `2` ko hi remove kar doge.

Isliye:

```cpp
high = mid;
```

correct hai.

---

# 🔥 Complete Logic

```text
1. mid find karo

2. Agar mid odd hai:
       mid--

3. nums[mid] == nums[mid+1] ?

   YES:
       Pair complete hai
       Single RIGHT me
       low = mid + 2

   NO:
       Pair broken hai
       Single LEFT ya MID me
       high = mid

4. low == high
       ↓
   Answer = nums[low]
```

---

# 🔄 Dry Run

Given:

```text
nums = [1,1,2,3,3,4,4]
```

Initial:

```text
low = 0
high = 6
```

---

## Iteration 1

```text
mid = 0 + (6-0)/2
    = 3
```

`mid = 3` odd hai.

So:

```text
mid--
mid = 2
```

Compare:

```text
nums[2] = 2
nums[3] = 3
```

Same?

```text
No
```

So pair broken hai.

Single:

```text
mid ya left
```

me hai.

Therefore:

```text
high = mid
high = 2
```

Now:

```text
low = 0
high = 2
```

---

## Iteration 2

```text
mid = 0 + (2-0)/2
    = 1
```

`mid = 1` odd hai.

So:

```text
mid--
mid = 0
```

Compare:

```text
nums[0] = 1
nums[1] = 1
```

Same hain.

Ye proper pair hai.

So single right me hai:

```text
low = mid + 2
low = 2
```

Now:

```text
low = 2
high = 2
```

Loop stop.

Final:

```text
nums[2] = 2
```

Answer:

```text
2
```

---

# 🔥 Second Dry Run

```text
nums = [1,1,2,2,3,4,4,5,5]
```

Single:

```text
3
```

Initial:

```text
low = 0
high = 8
```

### Iteration 1

```text
mid = 4
```

Compare:

```text
nums[4] = 3
nums[5] = 4
```

Different.

Pair broken.

So:

```text
high = 4
```

---

### Iteration 2

```text
low = 0
high = 4
mid = 2
```

Compare:

```text
nums[2] = 2
nums[3] = 2
```

Same.

Proper pair.

So:

```text
low = mid + 2
low = 4
```

Now:

```text
low = 4
high = 4
```

Answer:

```text
nums[4] = 3
```

---

# 🧠 Why Proper Pair Means Single Right?

Suppose:

```text
nums[mid] == nums[mid+1]
```

and `mid` even hai.

Matlab:

```text
mid, mid+1
```

ek complete pair hai.

Aur kyunki array sorted hai, us pair se pehle jo pairs hain wo bhi correctly aligned hain.

Agar single left me hota, to pairing pehle hi break ho gayi hoti.

So:

```text
Proper pair
    ↓
Left side correctly paired
    ↓
Single must be RIGHT
```

---

# 🧠 Why Broken Pair Means Single Left/Mid?

Suppose:

```text
nums[mid] != nums[mid+1]
```

`mid` even hai.

Normally yahan pair complete hona chahiye tha.

Lekin pair complete nahi hua.

Matlab:

```text
Single yahin hai
```

ya:

```text
Single isse pehle hai
```

So:

```cpp
high = mid;
```

---

# 💻 Binary Search Code

```cpp
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {

        int low = 0;
        int high = nums.size() - 1;

        while(low < high) {

            int mid = low + (high - low) / 2;

            // Pair ka first index even hona chahiye
            if(mid % 2 == 1) {
                mid--;
            }

            // Proper pair
            if(nums[mid] == nums[mid + 1]) {

                // Single right side me hai
                low = mid + 2;
            }

            else {

                // Pair broken hai
                // Single mid ya left side me hai
                high = mid;
            }
        }

        return nums[low];
    }
};
```

---

# 🧠 Code Ka Main Part

```cpp
if(mid % 2 == 1) {
    mid--;
}
```

Meaning:

```text
mid odd hai
→ 1 kam karo
→ mid even banao
```

Then:

```cpp
if(nums[mid] == nums[mid + 1])
```

Meaning:

```text
Kya expected pair complete hai?
```

---

# ⚠️ Common Mistakes

## 1. `high = nums.size()`

Wrong:

```cpp
int high = nums.size();
```

Correct:

```cpp
int high = nums.size() - 1;
```

Last valid index:

```text
size - 1
```

hota hai.

---

## 2. `mid` Odd Hone Par Kuch Na Karna

Galat:

```text
mid = 5
```

and directly:

```text
nums[5], nums[6]
```

compare karna.

Hume pair ke first index se start karna hai.

Correct:

```cpp
if(mid % 2 == 1)
    mid--;
```

---

## 3. Proper Pair Milne Par `low = mid + 1`

Wrong:

```cpp
low = mid + 1;
```

Correct:

```cpp
low = mid + 2;
```

Because:

```text
mid
mid+1
```

dono complete pair hain.

---

## 4. Broken Pair Par `high = mid - 1`

Wrong:

```cpp
high = mid - 1;
```

Correct:

```cpp
high = mid;
```

Because `mid` khud single ho sakta hai.

---

## 5. `while(low <= high)`

Is approach me:

```cpp
while(low < high)
```

use karte hain.

Kyunki goal hai search space ko ek single position tak narrow karna.

End:

```text
low == high
```

par answer mil jata hai.

---

# 📊 Complexity

### Time Complexity

Har iteration me roughly half search space remove hota hai.

```text
O(log n)
```

### Space Complexity

Extra data structure nahi:

```text
O(1)
```

---

# 🔥 Pattern Recognition

Question me:

```text
Sorted Array
+
Every element appears twice
+
One element appears once
+
O(log n)
```

dikhe to:

```text
Binary Search + Pair Pattern
```

think karo.

---

# 🧠 Quick Revision

```text
mid nikalo
   ↓
mid odd?
   ↓
mid--
   ↓
nums[mid] == nums[mid+1] ?
   ↓
YES
→ proper pair
→ single RIGHT
→ low = mid+2

NO
→ pair broken
→ single LEFT/MID
→ high = mid

low == high
→ nums[low]
```

---

# ⭐ One-Line Revision

```text
Mid ko even index par lao, phir mid aur mid+1 ko compare karo; pair complete hai to single right me hai, warna single left ya mid par hai.
```

---

# ⭐ Final Interview Code

```cpp
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {

        int low = 0;
        int high = nums.size() - 1;

        while(low < high) {

            int mid = low + (high - low) / 2;

            if(mid % 2 == 1) {
                mid--;
            }

            if(nums[mid] == nums[mid + 1]) {
                low = mid + 2;
            }
            else {
                high = mid;
            }
        }

        return nums[low];
    }
};
```

---

# 📌 Pattern

```text
Binary Search
    ↓
Sorted Array
    ↓
Pair Pattern
    ↓
Even / Odd Index
    ↓
Single Element
    ↓
LC 540
```
