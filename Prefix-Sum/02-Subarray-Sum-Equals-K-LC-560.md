# LC 560 — Subarray Sum Equals K

## Pattern
Prefix Sum + HashMap

---

# 1. Problem

Humein ek integer array `nums` diya gaya hai aur ek integer `k` diya gaya hai.

Humein count karna hai ki kitne **continuous subarrays** ka sum exactly `k` ke equal hai.

Example:

    nums = [1, 2, 3]
    k = 3

Valid subarrays:

    [1, 2] = 3
    [3] = 3

Therefore:

    Answer = 2

---

# 2. Subarray Kya Hota Hai?

Subarray matlab array ka **continuous part**.

For:

    nums = [1, 2, 3]

Valid subarrays:

    [1]
    [2]
    [3]
    [1, 2]
    [2, 3]
    [1, 2, 3]

Lekin:

    [1, 3]

subarray nahi hai because `1` aur `3` ke beech `2` bhi hai aur humne usse skip kar diya.

---

# 3. Question Ko Simple Language Mein Samjho

Humein bas ye find karna hai:

    Kitne continuous subarrays ka sum = k?

Example:

    nums = [1, 2, 3]
    k = 3

Check:

    [1]       → 1   ❌
    [2]       → 2   ❌
    [3]       → 3   ✅

    [1, 2]     → 3   ✅
    [2, 3]     → 5   ❌
    [1, 2, 3]  → 6   ❌

So:

    Answer = 2

---

# 4. Brute Force Approach

Sabse simple method:

Har starting index se subarray start karo.

Phir aage elements add karte jao.

Example:

    nums = [1, 2, 3]

Start from index `0`:

    [1]       → 1
    [1, 2]    → 3   ✅
    [1, 2, 3] → 6

Start from index `1`:

    [2]       → 2
    [2, 3]    → 5

Start from index `2`:

    [3]       → 3   ✅

Answer = 2

Problem:

Har possible subarray ko check karna padega.

Time Complexity:

    O(n²)

Hum isko `O(n)` tak optimize kar sakte hain.

---

# 5. Prefix Sum Ka Main Concept

Yahan sabse important idea hai:

    Current Prefix Sum - Previous Prefix Sum
                    =
             Subarray Sum

Example:

    nums = [1, 2, 3]

Prefix sums:

    1
    1 + 2 = 3
    1 + 2 + 3 = 6

So:

    prefix = [1, 3, 6]

Ab dekho:

    6 - 3 = 3

Yani:

    Current Prefix = 6
    Previous Prefix = 3

Difference:

    6 - 3 = 3

Aur array mein jo part bachta hai wo:

    [3]

Iska sum:

    3

---

# 6. Prefix Sum Difference Ko Samjho

Maan lo:

    Prefix till current = 10

Aur kisi previous position par:

    Prefix = 7

Toh:

    10 - 7 = 3

Matlab un dono positions ke beech ke elements ka sum `3` hai.

General formula:

    Current Prefix - Previous Prefix = Subarray Sum

---

# 7. Humein Sum = K Chahiye

Hum chahte hain:

    Subarray Sum = k

Aur:

    Current Prefix - Previous Prefix = k

So:

    Current Prefix - Previous Prefix = k

Rearrange:

    Previous Prefix = Current Prefix - k

Ye LC 560 ka **MAIN FORMULA** hai.

Important:

    required = prefixSum - k

Matlab har index par hum check karenge:

    Kya `prefixSum - k`
    pehle aa chuka hai?

Agar haan, toh valid subarray mila.

---

# 8. HashMap Kyun Use Karna Hai?

Ab problem ye hai:

Agar current prefix sum `10` hai aur `k = 3` hai, toh humein check karna hai:

    required = 10 - 3
             = 7

Kya previous prefix sum `7` tha?

Agar hum baar-baar peeche search karenge toh time badh jayega.

Isliye HashMap mein hum prefix sums store karenge.

Map structure:

    prefixSum → frequency

Example:

    0 → 1
    1 → 1
    3 → 2

Matlab:

- prefix sum `0` ek baar aaya
- prefix sum `1` ek baar aaya
- prefix sum `3` do baar aaya

---

# 9. Frequency Kyun Store Karni Hai?

Sirf ye check karna enough nahi hai:

    "Kya required prefix exist karta hai?"

Humein ye bhi jaana hai:

    "Required prefix kitni baar exist karta hai?"

Example:

    required = 5

Agar map mein:

    5 → 3

hai, toh iska matlab prefix sum `5` teen baar pehle aa chuka hai.

Toh current position ke saath **3 different valid subarrays** ban sakte hain.

Isliye:

    ans += mp[required]

---

# 10. Sabse Important Initialization

Code mein:

    mp[0] = 1;

Ye line bahut important hai.

Question:

    `0` ko map mein pehle se `1` kyun rakha?

Answer:

Array start hone se pehle prefix sum `0` hota hai.

Example:

    nums = [2]
    k = 2

Current prefix:

    2

Required:

    2 - 2 = 0

Agar map mein:

    0 → 1

already hoga, toh `[2]` ko count kar sakte hain.

Agar `mp[0] = 1` nahi karoge, toh index `0` se start hone wale valid subarrays miss ho sakte hain.

---

# 11. `mp[0] = 1` Ka Simple Meaning

Isko aise yaad rakho:

    Array start hone se pehle
    prefix sum = 0
    aur ye 1 baar exist karta hai.

Visual:

    Start
      ↓
    prefix = 0
      ↓
    first element
      ↓
    next prefix

So:

    mp[0] = 1

---

# 12. Detailed Example

Input:

    nums = [1, 2, 3]
    k = 3

Initial:

    prefixSum = 0
    ans = 0

Map:

    {0 : 1}

Ab ek-ek element dekhte hain.

---

# 13. Dry Run — i = 0

Current element:

    nums[0] = 1

Current prefix:

    prefixSum = 0 + 1
              = 1

Now required previous prefix:

    required = prefixSum - k
             = 1 - 3
             = -2

Map:

    {0 : 1}

Kya `-2` map mein hai?

    NO

Isliye:

    ans = 0

Ab kya karna hai?

Current prefix `1` ko future ke liye store karna hai.

    mp[1]++

Map:

    {0 : 1, 1 : 1}

IMPORTANT:

    -2 ko store nahi kiya.

Hum `-2` ko SEARCH kar rahe the.

Humne current prefix `1` ko STORE kiya.

---

# 14. Ye Confusion Yaad Rakhna

Har iteration mein do alag cheezein hoti hain:

## Search

    required = prefixSum - k

Map mein required search karo.

## Store

    mp[prefixSum]++

Current prefix ko future ke liye store karo.

So:

    required → SEARCH

    current prefix → STORE

Ye LC 560 ka very important distinction hai.

---

# 15. Dry Run — i = 1

Current element:

    nums[1] = 2

Current prefix:

    prefixSum = 1 + 2
              = 3

Required:

    required = 3 - 3
             = 0

Map:

    {0 : 1, 1 : 1}

`0` map mein hai.

Frequency:

    mp[0] = 1

So:

    ans += 1

Therefore:

    ans = 1

Kaunsa subarray mila?

    [1, 2]

Sum:

    1 + 2 = 3

Ab current prefix `3` store karo:

    mp[3]++

Map:

    {0 : 1, 1 : 1, 3 : 1}

---

# 16. Dry Run — i = 2

Current element:

    nums[2] = 3

Current prefix:

    prefixSum = 3 + 3
              = 6

Required:

    required = 6 - 3
             = 3

Map:

    {0 : 1, 1 : 1, 3 : 1}

`3` map mein hai.

Frequency:

    mp[3] = 1

So:

    ans += 1

Current answer:

    ans = 2

Kaunsa subarray mila?

    [3]

Sum:

    3

Ab current prefix `6` store:

    mp[6]++

Map:

    {0 : 1, 1 : 1, 3 : 1, 6 : 1}

Final:

    ans = 2

---

# 17. Complete Dry Run Table

| i | nums[i] | prefixSum | required = prefix-k | Required Map Mein? | Frequency | ans |
|---|---:|---:|---:|---|---:|---:|
| 0 | 1 | 1 | -2 | No | 0 | 0 |
| 1 | 2 | 3 | 0 | Yes | 1 | 1 |
| 2 | 3 | 6 | 3 | Yes | 1 | 2 |

Final answer:

    2

---

# 18. Actual Subarrays

For:

    nums = [1, 2, 3]
    k = 3

Valid subarrays:

    [1, 2]
    [3]

Count:

    2

Therefore:

    answer = 2

---

# 19. Code

    class Solution {
    public:
        int subarraySum(vector<int>& nums, int k) {

            unordered_map<int, int> mp;

            // Prefix sum 0 exists once before the array starts
            mp[0] = 1;

            int prefixSum = 0;
            int ans = 0;

            for(int x : nums) {

                // Add current element to prefix sum
                prefixSum += x;

                // We need this previous prefix
                int required = prefixSum - k;

                // If previous prefix exists
                if(mp.find(required) != mp.end()) {
                    ans += mp[required];
                }

                // Store current prefix for future elements
                mp[prefixSum]++;
            }

            return ans;
        }
    };

---

# 20. Code Ko Line By Line Samjho

## Step 1

    unordered_map<int, int> mp;

HashMap banaya.

Ismein:

    key   = prefix sum
    value = frequency

Example:

    mp[3] = 2

Matlab prefix sum `3` do baar aaya hai.

---

## Step 2

    mp[0] = 1;

Starting mein prefix sum `0` ek baar exist karta hai.

Ye index `0` se start hone wale subarrays ko handle karta hai.

---

## Step 3

    int prefixSum = 0;

Running prefix sum maintain karne ke liye.

---

## Step 4

    int ans = 0;

Valid subarrays ka count.

---

## Step 5

    for(int x : nums)

Array ke har element ko process karenge.

---

## Step 6

    prefixSum += x;

Current prefix sum update.

---

## Step 7

    int required = prefixSum - k;

Previous prefix sum chahiye.

Formula:

    Previous Prefix = Current Prefix - K

---

## Step 8

    if(mp.find(required) != mp.end())

Check kar raha hai:

    Kya required prefix pehle aaya hai?

---

## Step 9

    ans += mp[required];

Agar required prefix `n` times aaya hai:

    n valid subarrays current position par end ho sakte hain.

Isliye frequency ko answer mein add karte hain.

---

## Step 10

    mp[prefixSum]++;

Current prefix sum ko future ke liye store kar rahe hain.

---

## Step 11

    return ans;

Final number of valid subarrays return karte hain.

---

# 21. Complete Flow

Har element ke liye exact flow:

    Current Element
          ↓
    Prefix Sum Update
          ↓
    required = prefixSum - k
          ↓
    HashMap mein required search karo
          ↓
    Agar mila:
          ↓
    ans += frequency
          ↓
    Current prefix map mein store karo
          ↓
    Next element

---

# 22. Main Concept Ek Example Se

Maan lo:

    current prefix = 10
    k = 3

Humein chahiye:

    previous prefix = 10 - 3
                    = 7

Agar map mein:

    7 → 2

hai, toh iska matlab:

    Prefix sum 7 do baar aa chuka hai.

Toh:

    2 valid subarrays
    current position par end ho sakte hain.

So:

    ans += 2

---

# 23. Prefix Difference Ka Real Meaning

Suppose:

    Prefix till A = 5

Aur:

    Prefix till B = 12

Toh:

    12 - 5 = 7

Matlab:

    A ke baad se B tak
    subarray ka sum = 7

Exactly isi idea ka use LC 560 mein hai.

---

# 24. Negative Numbers Kyun Important Hain?

LC 560 mein negative numbers aa sakte hain.

Example:

    nums = [1, -1, 0]
    k = 0

Aise cases mein sliding window directly reliable nahi hota.

Kyunki sum continuously increase nahi karta.

For example:

    1
    1 + (-1) = 0
    0 + 0 = 0

Sum kabhi increase, kabhi decrease kar raha hai.

Prefix Sum + HashMap negative numbers ke saath bhi properly kaam karta hai.

---

# 25. Another Important Example

Input:

    nums = [1, -1, 0]
    k = 0

Prefix sums:

    0
    1
    0
    0

Notice:

    prefix 0 multiple times aa raha hai.

Isliye frequency store karna important hai.

Map kuch aisa banega:

    0 → frequency
    1 → frequency

Agar required prefix multiple times aaya hai, multiple valid subarrays mil sakte hain.

---

# 26. Why HashMap Frequency Instead of Set?

Agar hum Set use karenge:

    set<int>

toh humein sirf ye pata chalega:

    Prefix existed or not?

Lekin humein actual count chahiye:

    Prefix kitni baar exist hua?

Isliye HashMap use karte hain:

    prefixSum → frequency

Not:

    prefixSum → true/false

---

# 27. Why We Store Current Prefix AFTER Searching?

Code:

    int required = prefixSum - k;

    if(mp.find(required) != mp.end()) {
        ans += mp[required];
    }

    mp[prefixSum]++;

Search pehle hota hai.

Store baad mein.

Kyun?

Kyuki current prefix ko khud ke saath compare karke same element ka fake subarray nahi banana chahiye.

Humein sirf **previous prefix sums** use karne hain.

So order:

    SEARCH FIRST
    STORE AFTER

---

# 28. Golden Rule

LC 560 mein do golden rules:

    1. mp[0] = 1

    2. Search required first,
       then store current prefix

And:

    required = prefixSum - k

---

# 29. Brute Force vs Optimized

## Brute Force

Har possible subarray ko check karna.

Time:

    O(n²)

Space:

    O(1)

---

## Prefix Sum + HashMap

Har element ko ek baar process karna.

Time:

    O(n)

Space:

    O(n)

---

# 30. Complexity

## Time Complexity

Loop `n` elements par chalega.

HashMap operations average `O(1)` hain.

So:

    Time = O(n)

---

## Space Complexity

Worst case mein `n` different prefix sums ho sakte hain.

So:

    Space = O(n)

---

# 31. Common Mistakes

## Mistake 1 — `mp[0] = 1` Bhool Jana

Wrong:

    unordered_map<int, int> mp;

Then directly loop.

Problem:

    Index 0 se start hone wale subarrays miss ho sakte hain.

Correct:

    mp[0] = 1;

---

## Mistake 2 — Required Ko Store Karna

Wrong thinking:

    required = prefixSum - k

aur phir:

    mp[required]++

Ye galat hai.

`required` ko **search** karna hai.

Current prefix ko **store** karna hai.

Correct:

    if(mp.find(required) != mp.end()) {
        ans += mp[required];
    }

    mp[prefixSum]++;

---

## Mistake 3 — Set Use Karna

Set sirf existence batayega.

Humein frequency chahiye.

Isliye:

    unordered_map<int, int>

---

## Mistake 4 — Sliding Window Use Karna

Negative numbers ki wajah se sliding window reliable nahi hota.

Example:

    [1, -1, 2]

Sum continuously increase nahi kar raha.

Isliye:

    Prefix Sum + HashMap

use karte hain.

---

## Mistake 5 — Search Ke Baad Update Ki Jagah Pehle Update Karna

Correct order:

    Prefix update
        ↓
    required calculate
        ↓
    required search
        ↓
    answer update
        ↓
    current prefix store

---

# 32. Pattern Recognition

Jab question bole:

    "Count number of subarrays with sum equal to K"

Directly think:

    Prefix Sum + HashMap

Aur formula:

    previousPrefix = currentPrefix - K

So:

    required = prefixSum - k

---

# 33. LC 560 Ka Most Important Formula

Ye line yaad kar lo:

    required = prefixSum - k

Why?

Because:

    currentPrefix - previousPrefix = k

Therefore:

    previousPrefix = currentPrefix - k

Bas.

---

# 34. LC 724 vs LC 560

## LC 724

Find pivot index.

Main idea:

    Total Sum
       +
    Left Sum

Formula:

    rightSum = totalSum - leftSum - nums[i]

---

## LC 560

Count subarrays with sum `k`.

Main idea:

    Prefix Sum
       +
    HashMap

Formula:

    required = prefixSum - k

---

# 35. Difference Between Them

LC 724:

    Left == Right

LC 560:

    Current Prefix - Previous Prefix == K

So Prefix Sum pattern ka next level yahan start hota hai.

---

# 36. Interview Explanation

Interview mein simple language mein bol sakte ho:

> "Main running prefix sum maintain karta hoon aur har prefix sum ki frequency HashMap mein store karta hoon. Current prefix sum `S` par agar mujhe subarray sum `K` chahiye, toh mujhe pehle `S-K` prefix sum chahiye, because `S - (S-K) = K`. Agar `S-K` map mein multiple times aaya hai, toh utne valid subarrays milte hain. Starting mein `mp[0] = 1` rakhta hoon taaki index 0 se start hone wale subarrays count ho sakein."

---

# 37. One-Line Interview Explanation

    Current Prefix - Previous Prefix = K

Therefore:

    Previous Prefix = Current Prefix - K

HashMap mein previous prefix ki frequency rakho.

---

# 38. 10-Second Revision

Agar interview se just pehle dekhna ho:

    mp[0] = 1
    prefixSum = 0
    ans = 0

    for each x:

        prefixSum += x

        required = prefixSum - k

        if required exists:
            ans += mp[required]

        mp[prefixSum]++

    return ans

---

# 39. Memory Trick

Isko ek sentence mein yaad rakho:

    "Current prefix se K hatao,
     jo prefix bachega usse map mein dhoondo."

Matlab:

    prefixSum - K
           ↓
       HashMap
           ↓
    Frequency
           ↓
      Answer mein add

---

# 40. Final Code

    class Solution {
    public:
        int subarraySum(vector<int>& nums, int k) {

            unordered_map<int, int> mp;

            mp[0] = 1;

            int prefixSum = 0;
            int ans = 0;

            for(int x : nums) {

                prefixSum += x;

                int required = prefixSum - k;

                if(mp.find(required) != mp.end()) {
                    ans += mp[required];
                }

                mp[prefixSum]++;
            }

            return ans;
        }
    };

---

# 41. Final Revision Notes

Problem:

    LC 560 — Subarray Sum Equals K

Pattern:

    Prefix Sum + HashMap

Goal:

    Count continuous subarrays
    whose sum equals K.

Main Formula:

    currentPrefix - previousPrefix = K

Rearranged:

    previousPrefix = currentPrefix - K

Code:

    required = prefixSum - k;

HashMap:

    prefixSum → frequency

Initialization:

    mp[0] = 1

Flow:

    prefixSum update
        ↓
    required = prefixSum - k
        ↓
    required search
        ↓
    ans += frequency
        ↓
    current prefix store

Complexity:

    Time  = O(n)
    Space = O(n)

---

# 42. Most Important Things to Remember

1. `mp[0] = 1` is necessary.
2. `required = prefixSum - k`.
3. Required prefix ko search karna hai.
4. Current prefix ko store karna hai.
5. Frequency store karni hai, sirf existence nahi.
6. Search first, store after.
7. Negative numbers ke saath bhi ye approach kaam karti hai.
8. Prefix difference = subarray sum.

---

# Status

✅ LC 560 — Subarray Sum Equals K

Pattern:

    Prefix Sum + HashMap

Difficulty:

    Medium

Core Formula:

    previousPrefix = currentPrefix - K

Most Important Code Line:

    int required = prefixSum - k;

Core Data Structure:

    HashMap / unordered_map

Complexity:

    O(n) Time
    O(n) Space

Prefix Sum Progress:

    ✅ LC 724 — Find Pivot Index
    ✅ LC 560 — Subarray Sum Equals K
