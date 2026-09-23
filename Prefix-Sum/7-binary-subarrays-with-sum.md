# Prefix Sum Pattern

# 06. Binary Subarrays With Sum

## LeetCode 930

## Pattern
Prefix Sum + HashMap

---

# 1. Problem Statement

Given a binary array `nums` containing only `0` and `1`, and an integer `goal`.

Hume count karna hai ki kitne non-empty subarrays ka sum exactly `goal` ke equal hai.

Example:

nums = [1,0,1,0,1]
goal = 2

Answer = 4

---

# 2. Simple Language Mein Problem

Array mein sirf `0` aur `1` hain.

Hume aise saare continuous subarrays count karne hain jinka sum exactly `goal` ho.

Example:

nums = [1,0,1,0,1]
goal = 2

Valid subarrays:

[1,0,1]
[1,0,1]
[1,0,1]
[1,0,1]

Total = 4

---

# 3. Brute Force Approach

Har possible subarray banayenge.

Har subarray ka sum calculate karenge.

Agar sum == goal:

    ans++

Lekin isme bahut saare subarrays check karne padenge.

Time Complexity:

O(n²)

Hum isko O(n) mein kar sakte hain using:

Prefix Sum + HashMap

---

# 4. Main Idea

Hum ek `sum` variable maintain karenge.

Ye current index tak ka prefix sum store karega.

Example:

nums = [1,0,1,0,1]

Prefix sums:

Index:       0  1  2  3  4
nums:        1  0  1  0  1
prefix sum:  1  1  2  2  3

Ab maan lo:

current sum = 3
goal = 2

Hume previous prefix sum chahiye:

previous sum = current sum - goal

previous sum = 3 - 2

previous sum = 1

Agar prefix sum `1` pehle aa chuka hai, toh us point ke baad se current index tak ka subarray sum `2` hoga.

---

# 5. Core Formula

Sabse important formula:

current sum - previous sum = goal

Isko rearrange karo:

previous sum = current sum - goal

Isliye code mein:

int required = sum - goal;

---

# 6. HashMap Kya Store Karega?

HashMap mein:

prefix sum -> kitni baar aaya

store karenge.

Example:

sum = 1 do baar aaya.

Toh:

mp[1] = 2

Ye important hai kyunki agar same prefix sum multiple times aaya hai, toh multiple valid subarrays ban sakte hain.

---

# 7. `mp[0] = 1` Kyu?

Ye bahut important hai.

Array start hone se pehle hum assume karte hain:

prefix sum = 0

aur ye ek baar already exist karta hai.

Isliye:

mp[0] = 1;

Example:

nums = [1,1]
goal = 2

Start:

mp[0] = 1

First element:

sum = 1

required = 1 - 2 = -1

-1 map mein nahi hai.

Store:

mp[1] = 1

Second element:

sum = 2

required = 2 - 2 = 0

Ab `0` map mein already hai.

mp[0] = 1

Isliye:

ans = ans + 1

Ye `[1,1]` wala complete subarray represent karta hai.

---

# 8. Detailed Dry Run

nums = [1,0,1,0,1]

goal = 2

Initially:

mp[0] = 1

sum = 0
ans = 0

--------------------------------

Index 0

nums[0] = 1

sum = sum + nums[0]

sum = 1

required = sum - goal

required = 1 - 2

required = -1

-1 map mein nahi hai.

Ab current sum store karo:

mp[1]++

Map:

0 -> 1
1 -> 1

ans = 0

--------------------------------

Index 1

nums[1] = 0

sum = 1 + 0

sum = 1

required = 1 - 2

required = -1

-1 map mein nahi hai.

Ab:

mp[1]++

Map:

0 -> 1
1 -> 2

ans = 0

--------------------------------

Index 2

nums[2] = 1

sum = 1 + 1

sum = 2

required = 2 - 2

required = 0

0 map mein hai.

mp[0] = 1

Isliye:

ans = 0 + 1

ans = 1

Ab:

mp[2]++

Map:

0 -> 1
1 -> 2
2 -> 1

--------------------------------

Index 3

nums[3] = 0

sum = 2 + 0

sum = 2

required = 2 - 2

required = 0

0 map mein hai.

mp[0] = 1

Isliye:

ans = 1 + 1

ans = 2

Ab:

mp[2]++

Map:

0 -> 1
1 -> 2
2 -> 2

--------------------------------

Index 4

nums[4] = 1

sum = 2 + 1

sum = 3

required = 3 - 2

required = 1

Ab:

mp[1] = 2

Matlab prefix sum `1` do baar pehle aa chuka hai.

Isliye:

ans = 2 + 2

ans = 4

Ab:

mp[3]++

Map:

0 -> 1
1 -> 2
2 -> 2
3 -> 1

--------------------------------

Final Answer:

4

---

# 9. Code

class Solution {
public:
    int numSubarraysWithSum(vector<int>& nums, int goal) {

        unordered_map<int, int> mp;

        // Prefix sum 0 initially ek baar maana
        mp[0] = 1;

        int sum = 0;
        int ans = 0;

        for(int i = 0; i < nums.size(); i++) {

            // Current prefix sum
            sum = sum + nums[i];

            // Hume ye previous sum chahiye
            int required = sum - goal;

            // Agar required pehle aaya hai
            if(mp.find(required) != mp.end()) {
                ans = ans + mp[required];
            }

            // Current sum ko store karo
            mp[sum]++;
        }

        return ans;
    }
};

---

# 10. Code Explanation

## Step 1

unordered_map banaya:

unordered_map<int, int> mp;

Ye store karega:

prefix sum -> frequency

---

## Step 2

mp[0] = 1;

Array start hone se pehle prefix sum `0` ek baar exist karta hai.

---

## Step 3

sum = 0;

Current prefix sum maintain karne ke liye.

---

## Step 4

ans = 0;

Valid subarrays count karne ke liye.

---

## Step 5

Array traverse karenge:

for(int i = 0; i < nums.size(); i++)

---

## Step 6

Current prefix sum update:

sum = sum + nums[i];

---

## Step 7

Previous required sum nikalo:

int required = sum - goal;

Reason:

current sum - previous sum = goal

So:

previous sum = current sum - goal

---

## Step 8

Check karo required sum pehle aaya hai ya nahi:

if(mp.find(required) != mp.end())

Agar mila:

ans = ans + mp[required];

Frequency jitni hogi, utne valid subarrays milenge.

---

## Step 9

Current sum ko map mein store karo:

mp[sum]++;

Frequency increase kar do.

---

# 11. Sabse Important Logic

Is question ka actual logic sirf ye hai:

sum = sum + nums[i];

required = sum - goal;

if(required map mein hai) {
    ans += mp[required];
}

mp[sum]++;

---

# 12. `mp[required]` Kyu Add Karte Hain?

Suppose:

current sum = 3
goal = 2

required = 3 - 2
required = 1

Agar:

mp[1] = 2

iska matlab prefix sum `1` do different positions par pehle aa chuka hai.

Dono positions se current index tak valid subarray banega.

Isliye:

ans += 2;

---

# 13. LC 560 Se Connection

LeetCode 560:

Subarray Sum Equals K

Usme bhi:

required = sum - k

LC 930:

Binary Subarrays With Sum

Usme:

required = sum - goal

Dono ka main pattern same hai:

Prefix Sum + HashMap Frequency

Difference:

LC 560 -> general integer array

LC 930 -> binary array (0 and 1)

---

# 14. LC 525 Se Difference

LC 525:

Contiguous Array

Usme:

0 ko -1 treat karte hain.

1 ko +1 treat karte hain.

Same prefix sum milne par longest subarray find karte hain.

Isliye map mein:

prefix sum -> first index

store hota hai.

LC 930 mein:

Hume longest nahi chahiye.

Hume total number of subarrays count karne hain.

Isliye:

prefix sum -> frequency

store hota hai.

---

# 15. Common Mistakes

## Mistake 1

`mp[0] = 1` bhool jaana.

Wrong:

unordered_map<int, int> mp;

Correct:

unordered_map<int, int> mp;
mp[0] = 1;

---

## Mistake 2

`required = sum - goal` ki jagah:

required = goal - sum

likh dena.

Correct formula:

required = sum - goal

---

## Mistake 3

Map mein frequency store na karna.

Hume:

mp[sum]++;

karna hai.

Kyuki same prefix sum multiple times aa sakta hai.

---

## Mistake 4

`ans++` karna instead of:

ans += mp[required];

Agar required sum 3 baar aa chuka hai, toh 3 valid subarrays mil sakte hain.

---

# 16. Complexity

Time Complexity:

O(n)

Har element ko ek baar process kar rahe hain.

HashMap lookup average O(1) hota hai.

Space Complexity:

O(n)

Worst case mein map mein O(n) different prefix sums ho sakte hain.

---

# 17. Interview Mein Kaise Explain Karna Hai

"Sir, I will use Prefix Sum with a HashMap.

While traversing the array, I maintain the current prefix sum.

For every current sum, I need a previous prefix sum such that:

currentSum - previousSum = goal

Therefore:

previousSum = currentSum - goal

I store the frequency of every prefix sum in a HashMap.

If the required prefix sum already exists, its frequency tells me how many subarrays ending at the current index have sum equal to goal.

I initialize mp[0] = 1 to handle subarrays starting from index 0."

---

# 18. Pattern Recognition

Jab question mein ye words dikhein:

- Count subarrays
- Sum exactly K / goal
- Number of subarrays
- Continuous subarray
- Given target sum

Toh immediately socho:

Prefix Sum + HashMap Frequency

Core formula:

required = currentSum - target

---

# 19. Full Mental Model

Array:

[1, 0, 1, 0, 1]

Goal:

2

Hum current prefix sum nikalte hain.

Example:

current sum = 3

Hume previous sum chahiye:

3 - 2 = 1

Agar previous sum `1` do baar aaya hai:

2 valid subarrays milenge.

Isliye:

ans += mp[1];

Phir current sum ko future ke liye store kar do:

mp[3]++;

---

# 20. One-Line Trick

"Current prefix sum mein se goal minus karo; jo previous prefix sum mile, wo jitni baar pehle aaya hai utne valid subarrays current index par end ho rahe hain."

---

# 21. Prefix Sum Roadmap Progress

1. LC 1480 - Running Sum of 1d Array
   - Basic Prefix Sum

2. LC 303 - Range Sum Query - Immutable
   - Prefix Sum for Range Queries

3. LC 560 - Subarray Sum Equals K
   - Prefix Sum + HashMap Frequency

4. LC 974 - Subarray Sums Divisible by K
   - Prefix Sum + Remainder

5. LC 525 - Contiguous Array
   - Prefix Sum + First Index

6. LC 930 - Binary Subarrays With Sum
   - Prefix Sum + HashMap Frequency

---

# Final Takeaway

Prefix Sum ka sabse important idea:

current sum - previous sum = subarray sum

Agar hume target `goal` chahiye:

current sum - previous sum = goal

Therefore:

previous sum = current sum - goal

Aur HashMap mein previous prefix sums ki frequency store karke hum O(n) mein answer nikal sakte hain.

LC 930 ko yaad rakhne ka simple formula:

sum -> required = sum - goal -> map mein check -> frequency answer mein add -> current sum store
