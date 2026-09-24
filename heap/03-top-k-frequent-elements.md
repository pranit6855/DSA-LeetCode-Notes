# Heap Pattern

# 03. Top K Frequent Elements

## LeetCode 347

## Pattern
Frequency Map + Min Heap

---

# 1. Problem Statement

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements.

The answer can be returned in any order.

Example:

nums = [1,1,1,2,2,3]
k = 2

Answer:

[1,2]

Because:

1 appears 3 times
2 appears 2 times
3 appears 1 time

The 2 most frequent elements are `1` and `2`.

---

# 2. Simple Language Mein Problem

Sabse pehle har number ki frequency count karni hai.

Example:

nums = [1,1,1,2,2,3]

Frequency:

1 → 3
2 → 2
3 → 1

Ab hume top `k` frequent numbers chahiye.

Agar:

k = 2

Toh:

1 and 2

answer honge.

---

# 3. Brute Force Approach

Pehle frequency count kar sakte hain.

Uske baad frequencies ko sort kar sakte hain.

Example:

1 → 3
2 → 2
3 → 1

Frequency ke basis par sort:

3, 2, 1

Top 2:

1 and 2

Lekin sorting ki wajah se extra time lagega.

Hum Heap use karke top `k` elements maintain kar sakte hain.

---

# 4. Main Idea

Is question mein 2 important cheezein hain:

1. Frequency count karna
2. Top K frequencies maintain karna

Isliye:

Frequency Map + Min Heap

use karenge.

---

# 5. Step 1 - Frequency Count

HashMap mein:

number → frequency

store karenge.

Example:

nums = [1,1,1,2,2,3]

Map:

1 → 3
2 → 2
3 → 1

Code:

unordered_map<int, int> mp;

for(int x : nums) {
    mp[x]++;
}

---

# 6. Step 2 - Min Heap

Ab har number aur uski frequency ko Heap mein daalenge.

Heap mein pair store karenge:

{frequency, number}

Example:

{3,1}
{2,2}
{1,3}

Yahan:

first = frequency

second = number

---

# 7. Why Min Heap?

Hume Top K frequent elements chahiye.

Suppose:

k = 2

Agar Heap mein 3 elements hain:

{1,3}
{2,2}
{3,1}

Hume sirf 2 most frequent elements rakhne hain.

Sabse kam frequency wala element:

{1,3}

isko remove karenge.

Bachenge:

{2,2}
{3,1}

Ye 2 most frequent elements hain.

Min Heap mein minimum frequency top par hoti hai.

Isliye extra element remove karna easy ho jata hai.

---

# 8. Core Logic

Har unique number ke liye:

    {frequency, number} Heap mein push karo

Agar:

    heap size > k

toh:

    smallest frequency pop karo

End mein:

    Heap mein k most frequent elements bachenge

---

# 9. Detailed Dry Run

nums = [1,1,1,2,2,3]

k = 2

---

## Step 1: Frequency Count

Array:

[1,1,1,2,2,3]

Map:

1 → 3
2 → 2
3 → 1

---

## Step 2: Min Heap

Start:

Heap = []

---

## Number 1

Frequency = 3

Heap mein:

{3,1}

Heap:

{3,1}

Size = 1

K = 2

Nothing remove.

---

## Number 2

Frequency = 2

Push:

{2,2}

Heap:

{2,2}
{3,1}

Size = 2

K = 2

Nothing remove.

---

## Number 3

Frequency = 1

Push:

{1,3}

Heap:

{1,3}
{2,2}
{3,1}

Size = 3

But K = 2.

Size > K

So smallest frequency remove.

Top:

{1,3}

Remove it.

Now:

{2,2}
{3,1}

Heap mein sirf top 2 frequent elements hain.

---

# 10. Answer Kaise Niklega?

Heap mein:

{2,2}
{3,1}

Pair ka second value number hai.

For:

{2,2}

number = 2

For:

{3,1}

number = 1

So answer:

[2,1]

Order important nahi hai.

---

# 11. Code

class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {

        unordered_map<int, int> mp;

        // Frequency count
        for(int x : nums) {
            mp[x]++;
        }

        // Min Heap
        // {frequency, number}
        priority_queue<pair<int, int>,
                       vector<pair<int, int>>,
                       greater<pair<int, int>>> pq;

        for(auto x : mp) {

            pq.push({x.second, x.first});

            // Sirf k most frequent elements rakho
            if(pq.size() > k) {
                pq.pop();
            }
        }

        vector<int> ans;

        // Heap se answer nikalo
        while(!pq.empty()) {
            ans.push_back(pq.top().second);
            pq.pop();
        }

        return ans;
    }
};

---

# 12. Code Explanation

## Frequency Map

unordered_map<int, int> mp;

Ye store karega:

number → frequency

Example:

1 → 3
2 → 2
3 → 1

---

## Frequency Count

for(int x : nums) {
    mp[x]++;
}

Har baar number milne par uski frequency increase kar denge.

---

## Min Heap

priority_queue<pair<int, int>,
               vector<pair<int, int>>,
               greater<pair<int, int>>> pq;

Ye Min Heap hai.

Heap mein pair:

{frequency, number}

store kar rahe hain.

---

# 13. `for(auto x : mp)` Ka Matlab

Map ke har pair ko ek-ek karke `x` mein le rahe hain.

Suppose:

x = {1,3}

Matlab:

x.first = 1
x.second = 3

Yahan:

x.first = number
x.second = frequency

---

# 14. Heap Mein Ulta Kyu Dala?

Code:

pq.push({x.second, x.first});

Map mein:

{number, frequency}

Example:

{1,3}

Heap mein:

{3,1}

Kyun?

Kyuki Heap ko frequency ke basis par compare karna hai.

Isliye:

first = frequency

second = number

---

# 15. `pq.size() > k` Kyu?

Suppose:

k = 2

Hum sirf 2 most frequent elements rakhna chahte hain.

Agar Heap size 3 ho gayi:

pq.size() > k

toh:

pq.pop();

Min Heap smallest frequency wale pair ko top par rakhega.

Isliye lowest frequency remove ho jayegi.

---

# 16. `pq.top().second` Kyu?

Heap mein:

{frequency, number}

store hai.

Example:

{3,1}

Toh:

pq.top().first = 3

pq.top().second = 1

Hume answer mein number chahiye.

Isliye:

pq.top().second

use karenge.

---

# 17. Final Answer Nikalna

vector<int> ans;

while(!pq.empty()) {

    ans.push_back(pq.top().second);

    pq.pop();
}

Har Heap element ka number answer mein daal denge.

---

# 18. Most Important Logic

Is question ka actual main logic:

    Frequency count karo

    {frequency, number} Heap mein push karo

    Agar size > k:
        pop smallest frequency

    Remaining elements = Top K Frequent

---

# 19. Why Min Heap of Size K?

Suppose:

k = 2

Frequencies:

1 → 3
2 → 2
3 → 1
4 → 5

Hume top 2 chahiye:

4 → 5
1 → 3

Min Heap use karenge.

Jab bhi 3 elements ho jayenge:

smallest frequency remove kar denge.

End mein sirf 2 highest-frequency elements bachenge.

---

# 20. Connection With LC 215

LC 215:

Kth Largest Element

    Min Heap
    Size = K
    Top = Kth Largest

LC 347:

Top K Frequent Elements

    Frequency Count
    Min Heap
    Size = K
    Lowest frequency remove
    Remaining = Top K Frequent

Dono mein same important idea:

    Min Heap of Size K

Difference:

LC 215 mein direct numbers compare karte hain.

LC 347 mein frequency compare karte hain.

---

# 21. Common Mistakes

## Mistake 1: Frequency count na karna

Direct Heap mein numbers daalne se Top K Frequent solve nahi hoga.

Pehle frequency count karni hai.

---

## Mistake 2: Heap mein `{number, frequency}` daal dena

Agar frequency ke basis par Min Heap chahiye, toh:

{frequency, number}

store karna better hai.

---

## Mistake 3: Heap size K se bada hone dena

Correct:

if(pq.size() > k) {
    pq.pop();
}

---

## Mistake 4: `pq.top().first` answer samajhna

Pair:

{frequency, number}

So:

first = frequency
second = number

Answer ke liye:

pq.top().second

---

# 22. Complexity

Frequency Map:

O(n)

Unique elements ko Heap mein insert karna:

O(m log k)

where `m` = number of unique elements.

Overall:

O(n + m log k)

Worst case:

O(n log k)

Space:

O(n)

Map aur Heap dono extra space le sakte hain.

---

# 23. Interview Explanation

"I will first count the frequency of every number using a HashMap.

Then I will use a Min Heap of size K.

For every unique number, I will insert a pair of {frequency, number} into the heap.

Whenever the heap size becomes greater than K, I remove the element with the smallest frequency.

This way, the heap always contains the K most frequent elements.

Finally, I extract the numbers from the heap and return them.

The main idea is Frequency Map + Min Heap of size K."

---

# 24. Pattern Recognition

Question mein agar aaye:

- Top K frequent elements
- K most frequent numbers
- Most frequent elements
- Frequency + Top K

Immediately think:

    HashMap + Min Heap

---

# 25. Mental Model

Example:

nums = [1,1,1,2,2,3]

Frequency:

1 → 3
2 → 2
3 → 1

Then:

{3,1}
{2,2}
{1,3}

Min Heap:

smallest frequency top par

K = 2

Extra element:

{1,3}

remove.

Remaining:

{2,2}
{3,1}

Answer:

[2,1]

---

# 26. One-Line Trick

"Frequency count karo, frequency-number pair ko Min Heap mein rakho, size K se bada hote hi lowest frequency hatao; remaining K elements hi Top K Frequent hain."

---

# 27. Final Takeaway

LC 347 ka complete flow:

Array
    ↓
Frequency Map
    ↓
Number ki frequency
    ↓
{frequency, number}
    ↓
Min Heap
    ↓
Size > K
    ↓
Lowest frequency pop
    ↓
K elements remain
    ↓
Answer

Core Pattern:

    Top K Frequent
        ↓
    Frequency Map
        ↓
    Min Heap
        ↓
    Size = K

Complexity:

Time = O(n + m log k)

Space = O(n)

Where `m` = number of unique elements.
