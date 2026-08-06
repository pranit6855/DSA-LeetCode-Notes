# LeetCode 2461 - Maximum Sum of Distinct Subarrays With Length K

## 📌 Problem

Hume ek integer array `nums` aur integer `k` diya gaya hai.

Hume **exactly `k` length** ka aisa continuous subarray find karna hai jisme:

1. Saare elements **distinct** hone chahiye.
2. Us subarray ka **sum maximum** hona chahiye.

Agar koi valid subarray nahi milta, answer `0` return karna hai.

---

# 🔹 Example

```text
nums = [1,5,4,2,9,9,9]
k = 3
```

Possible windows:

```text
[1,5,4] → distinct ✅ → sum = 10

[5,4,2] → distinct ✅ → sum = 11

[4,2,9] → distinct ✅ → sum = 15 ⭐

[2,9,9] → duplicate 9 ❌

[9,9,9] → duplicates ❌
```

Maximum valid sum:

```text
15
```

So:

```text
Output = 15
```

---

# 🧠 Approach

Ye **Fixed Size Sliding Window + Hash Map** problem hai.

Fixed Sliding Window isliye:

```text
Window size = k
```

hamesha fixed rahega.

Hash Map isliye:

```text
Current window me har number ki frequency track karni hai.
```

Hum simultaneously do cheezein maintain karenge:

```text
1. Current window ka sum
2. Current window ke elements ki frequency
```

---

# 🔥 Main Observation

Window ka size already:

```text
k
```

hai.

Agar map me unique elements ki count bhi:

```text
k
```

hai, then saare elements distinct hain.

So:

```cpp
if(mp.size()==k)
```

means:

```text
Current k-size window ke saare elements distinct hain.
```

---

# 🔹 Example of `mp.size() == k`

Window:

```text
[4,2,9]
```

Map:

```text
4 → 1
2 → 1
9 → 1
```

Number of elements:

```text
3
```

Number of unique elements:

```text
mp.size() = 3
```

And:

```text
k = 3
```

So:

```text
mp.size() == k
```

Therefore:

```text
All elements are distinct ✅
```

---

# ❌ Duplicate Example

Window:

```text
[2,9,9]
```

Map:

```text
2 → 1
9 → 2
```

Window size:

```text
3
```

But unique elements:

```text
2
```

So:

```text
mp.size() = 2
k = 3
```

Therefore:

```text
mp.size() != k
```

Window invalid ❌

---

# 🔥 Variables

```cpp
int low=0;
int high=k-1;
long long sum=0;
long long max_sum=0;
int n=nums.size();

unordered_map<int,int> mp;
```

Meaning:

```text
low     → window ka left index

high    → window ka right index

sum     → current window ka sum

max_sum → maximum valid window sum

mp      → current window ke elements ki frequency

n       → array size
```

---

# 🔥 Step 1 - First Window

First `k` elements process karenge:

```cpp
for(int i=0;i<k;i++){
    sum+=nums[i];
    mp[nums[i]]++;
}
```

Yahan ek hi loop me do kaam ho rahe hain.

### Sum calculate

```cpp
sum+=nums[i];
```

### Frequency store

```cpp
mp[nums[i]]++;
```

---

# 🔹 First Window Example

```text
nums = [1,5,4,2,9,9,9]
k = 3
```

First window:

```text
[1,5,4]
```

Sum:

```text
1 + 5 + 4 = 10
```

So:

```text
sum = 10
```

Map:

```text
1 → 1
5 → 1
4 → 1
```

So:

```text
mp.size() = 3
```

Since:

```text
k = 3
```

Window valid hai.

---

# 🔥 Step 2 - First Window Check

```cpp
if(mp.size()==k){
    max_sum=sum;
}
```

First window ko separately check karna zaroori hai.

Because first window `for` loop se bani hai.

`while` loop remaining windows ko process karega.

Example:

```text
mp.size() = 3
k = 3
```

So:

```text
max_sum = 10
```

---

# ❓ `max_sum = 0` Initially Kyu?

```cpp
long long max_sum=0;
```

Agar koi bhi valid distinct window nahi milti, answer:

```text
0
```

return hona chahiye.

Isliye initial:

```text
max_sum = 0
```

rakha hai.

---

# 🔥 Step 3 - Window Slide

```cpp
while(high<n-1)
```

Har iteration me:

```cpp
low++;
high++;
```

Example:

Before:

```text
[1 5 4] 2 9 9 9
 ↑   ↑
low high
```

After:

```text
1 [5 4 2] 9 9 9
   ↑   ↑
  low high
```

Window size ab bhi:

```text
k = 3
```

hai.

---

# 🔥 Step 4 - Outgoing Element

Window slide karne par old left element bahar jayega.

Example:

```text
[1,5,4]
```

se:

```text
[5,4,2]
```

`1` bahar gaya.

Hume `1` ko **do jagah se remove/update** karna hai:

```text
1. Sum
2. Frequency Map
```

---

# 🔥 Outgoing Element - Sum

```cpp
sum=sum-nums[low-1];
```

Suppose:

```text
sum = 10
```

Outgoing:

```text
1
```

Then:

```text
sum = 10 - 1
    = 9
```

---

# 🔥 Outgoing Element - Map

Outgoing element current window me ek baar kam ho gaya.

So:

```cpp
mp[nums[low-1]]--;
```

Before:

```text
1 → 1
5 → 1
4 → 1
```

After removing `1`:

```text
1 → 0
5 → 1
4 → 1
```

---

# ❓ Frequency 0 Hone Par `erase()` Kyu?

Ab:

```text
1 → 0
```

ka meaning hai:

```text
Current window me 1 present nahi hai.
```

Lekin agar map me:

```text
1 → 0
```

key padi rahegi, `mp.size()` usko bhi count karega.

Example:

```text
1 → 0
5 → 1
4 → 1
```

Map bolega:

```text
mp.size() = 3
```

But current window me actually unique elements sirf:

```text
5, 4
```

hain.

Isliye frequency `0` hone par key erase karna zaroori hai:

```cpp
if(mp[nums[low-1]]==0){
    mp.erase(nums[low-1]);
}
```

Now:

```text
5 → 1
4 → 1
```

Correct:

```text
mp.size() = 2
```

---

# ⚠️ Important - `erase()` Me Key Dete Hain

Correct:

```cpp
mp.erase(nums[low-1]);
```

Wrong:

```cpp
mp.erase(mp[nums[low-1]]);
```

Kyun?

Suppose:

```text
nums[low-1] = 5
```

and:

```text
mp[5] = 0
```

Then:

```cpp
mp[nums[low-1]]
```

returns:

```text
0
```

Agar likhen:

```cpp
mp.erase(mp[nums[low-1]]);
```

to effectively:

```cpp
mp.erase(0);
```

ho jayega ❌

Hume key:

```text
5
```

erase karni thi.

Therefore:

```cpp
mp.erase(nums[low-1]);
```

Correct ✅

---

# 🔥 Step 5 - Incoming Element

Window:

```text
[1,5,4]
```

se:

```text
[5,4,2]
```

hui.

Incoming element:

```text
2
```

Ab `2` ko:

```text
1. Sum me add
2. Map me add
```

karna hai.

---

# 🔥 Incoming Element - Map

```cpp
mp[nums[high]]++;
```

Map:

```text
5 → 1
4 → 1
```

Incoming:

```text
2
```

New map:

```text
5 → 1
4 → 1
2 → 1
```

---

# 🔥 Incoming Element - Sum

```cpp
sum=sum+nums[high];
```

Current:

```text
sum = 9
```

Incoming:

```text
2
```

So:

```text
sum = 9 + 2
    = 11
```

Current window:

```text
[5,4,2]
```

Actual sum:

```text
5 + 4 + 2 = 11
```

Correct.

---

# 🔥 Step 6 - Distinct Check

Ab check:

```cpp
if(mp.size()==k){
    max_sum=max(sum,max_sum);
}
```

Current map:

```text
5 → 1
4 → 1
2 → 1
```

So:

```text
mp.size() = 3
k = 3
```

All distinct ✅

Current:

```text
sum = 11
max_sum = 10
```

Update:

```text
max_sum = max(11,10)

        = 11
```

---

# 🔄 Complete Dry Run

Given:

```text
nums = [1,5,4,2,9,9,9]
k = 3
```

## Window 1

```text
[1,5,4]
```

Map:

```text
1 → 1
5 → 1
4 → 1
```

```text
mp.size() = 3
```

Distinct ✅

Sum:

```text
10
```

So:

```text
max_sum = 10
```

---

## Window 2

```text
[5,4,2]
```

Outgoing:

```text
1
```

Remove from sum:

```text
10 - 1 = 9
```

Remove from map:

```text
1 → 0
```

Erase `1`.

Incoming:

```text
2
```

Add to sum:

```text
9 + 2 = 11
```

Map:

```text
5 → 1
4 → 1
2 → 1
```

Distinct ✅

```text
max_sum = 11
```

---

## Window 3

```text
[4,2,9]
```

Sum:

```text
15
```

Map:

```text
4 → 1
2 → 1
9 → 1
```

Distinct ✅

So:

```text
max_sum = 15
```

---

## Window 4

```text
[2,9,9]
```

Map:

```text
2 → 1
9 → 2
```

Unique elements:

```text
2
```

So:

```text
mp.size() = 2
```

But:

```text
k = 3
```

Therefore:

```text
mp.size()!=k
```

Invalid ❌

`max_sum` update nahi hoga.

---

## Window 5

```text
[9,9,9]
```

Map:

```text
9 → 3
```

So:

```text
mp.size() = 1
```

Invalid ❌

Final:

```text
max_sum = 15
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    long long maximumSubarraySum(vector<int>& nums, int k) {

        int low=0;
        int high=k-1;
        long long sum=0;
        long long max_sum=0;
        int n=nums.size();

        unordered_map<int,int> mp;

        // First window
        for(int i=0;i<k;i++){
            sum+=nums[i];
            mp[nums[i]]++;
        }

        // First window check
        if(mp.size()==k){
            max_sum=sum;
        }

        // Remaining windows
        while(high<n-1){

            low++;
            high++;

            // Outgoing element
            mp[nums[low-1]]--;
            sum=sum-nums[low-1];

            if(mp[nums[low-1]]==0){
                mp.erase(nums[low-1]);
            }

            // Incoming element
            mp[nums[high]]++;
            sum=sum+nums[high];

            // Distinct check
            if(mp.size()==k){
                max_sum=max(sum,max_sum);
            }
        }

        return max_sum;
    }
};
```

---

# 🔥 Most Important Part

```cpp
mp[nums[low-1]]--;
sum=sum-nums[low-1];

if(mp[nums[low-1]]==0){
    mp.erase(nums[low-1]);
}

mp[nums[high]]++;
sum=sum+nums[high];
```

Meaning:

```text
OLD element
→ frequency decrease
→ sum se remove
→ frequency 0 ho gayi to map se erase

NEW element
→ frequency increase
→ sum me add
```

---

# 🧠 Why `mp.size()==k`?

Ye question ka sabse important concept hai.

Window ka size already:

```text
k
```

hai.

Agar unique elements bhi:

```text
k
```

hain:

```text
Total elements  = k
Unique elements = k
```

then:

```text
No duplicates ✅
```

Example:

```text
[4,2,9]

Total = 3
Unique = 3
```

Distinct.

But:

```text
[2,9,9]

Total = 3
Unique = 2
```

Duplicate present.

---

# ⏱️ Time Complexity

First window:

```text
O(k)
```

Remaining windows:

```text
O(n-k)
```

Hash map operations average:

```text
O(1)
```

Total:

```text
O(n)
```

So:

```text
Time Complexity = O(n)
```

---

# 💾 Space Complexity

Map current window ke elements store karta hai.

Maximum `k` distinct elements ho sakte hain.

So:

```text
Space Complexity = O(k)
```

---

# 📊 Complexity

```text
Time Complexity  → O(n)

Space Complexity → O(k)
```

---

# ⚠️ Common Mistakes

## 1. Wrong `erase`

Wrong:

```cpp
mp.erase(mp[nums[low-1]]);
```

Correct:

```cpp
mp.erase(nums[low-1]);
```

`erase()` ko **key** deni hai, frequency nahi.

---

## 2. Frequency 0 Hone Par Erase Na Karna

Sirf:

```cpp
mp[nums[low-1]]--;
```

karna enough nahi hai.

Agar frequency `0` hai:

```cpp
if(mp[nums[low-1]]==0){
    mp.erase(nums[low-1]);
}
```

karna zaroori hai.

Otherwise `mp.size()` incorrect ho sakta hai.

---

## 3. Sirf Sum Check Karna

Question maximum sum nahi, balki:

```text
Maximum sum of DISTINCT k-size subarray
```

pooch raha hai.

So:

```cpp
max_sum=max(sum,max_sum);
```

directly nahi karna.

Pehle:

```cpp
if(mp.size()==k)
```

check karna hai.

---

## 4. `count` Variable Ki Zarurat Nahi

Is question me:

```cpp
int count=0;
```

ki need nahi hai.

Hum directly:

```cpp
mp.size()
```

se unique elements check kar rahe hain.

---

# 🔥 Previous Questions Se Connection

### LC 643

```text
Fixed Window
+
Sum
+
Maximum
```

### LC 1456

```text
Fixed Window
+
Vowel Count
+
Maximum
```

### LC 1343

```text
Fixed Window
+
Sum
+
Condition
+
Valid Windows Count
```

### LC 2379

```text
Fixed Window
+
W Count
+
Minimum
```

### LC 2461

```text
Fixed Window
+
Sum
+
Hash Map
+
Distinct Check
+
Maximum
```

So LC 2461 me humne fixed sliding window ke saath **Hash Map** bhi combine kar diya.

---

# 🔥 Quick Revision

```text
First k elements
      ↓
sum + frequency map
      ↓
mp.size() == k ?
      ↓
YES → max_sum update
      ↓
low++, high++
      ↓
Outgoing:
frequency--
sum se minus
      ↓
frequency == 0 ?
YES → erase
      ↓
Incoming:
frequency++
sum me add
      ↓
mp.size() == k ?
      ↓
YES → max_sum update
      ↓
repeat
```

---

# 🧠 One-Line Revision

```text
Har k-size window ka sum aur frequency map maintain karo; agar map me k unique elements hain to window distinct hai aur uska sum maximum answer ke liye consider karo.
```

---

# ⭐ Interview Revision Code

```cpp
for(int i=0;i<k;i++){
    sum+=nums[i];
    mp[nums[i]]++;
}

if(mp.size()==k){
    max_sum=sum;
}

while(high<n-1){

    low++;
    high++;

    mp[nums[low-1]]--;
    sum-=nums[low-1];

    if(mp[nums[low-1]]==0){
        mp.erase(nums[low-1]);
    }

    mp[nums[high]]++;
    sum+=nums[high];

    if(mp.size()==k){
        max_sum=max(sum,max_sum);
    }
}
```

**Main formula:**

```text
Fixed Window
+ Sum
+ Frequency Map
+ mp.size()==k
= Distinct K-Size Maximum Sum
```
