# Heap Pattern

# 01. Kth Largest Element in an Array

## LeetCode 215

## Pattern
Kth Element + Min Heap

---

# 1. Problem Statement

Given an integer array `nums` and an integer `k`, find the kth largest element in the array.

Example:

nums = [3,2,1,5,6,4]
k = 2

Answer = 5

Because sorted order is:

[1,2,3,4,5,6]

Largest element = 6
2nd largest element = 5

Therefore answer = 5.

---

# 2. Simple Language Mein Problem

Hume array mein kth largest element find karna hai.

Example:

[3,2,1,5,6,4]

Agar:

k = 2

Toh hume second largest element chahiye.

Largest:
6

Second largest:
5

Answer:
5

---

# 3. Brute Force Approach

Sabse simple approach:

Array ko sort kar do.

Example:

[3,2,1,5,6,4]

Sort:

[1,2,3,4,5,6]

Kth largest nikal sakte hain.

Agar array ka size `n` hai, toh kth largest ka index:

n - k

Example:

n = 6
k = 2

index:

6 - 2 = 4

nums[4] = 5

Answer = 5

Sorting ki complexity:

O(n log n)

Lekin Heap se hum sorting ke bina answer nikal sakte hain.

---

# 4. Main Heap Approach

Hume Kth largest element chahiye.

Iske liye:

Min Heap use karenge.

Lekin ek important rule:

Heap ka size maximum `k` rakhenge.

Matlab:

Heap mein sirf `k` largest elements maintain karenge.

---

# 5. Why Min Heap?

Suppose:

nums = [3,2,1,5,6,4]

k = 2

Hume sirf 2 largest elements chahiye.

Final mein 2 largest elements honge:

[5,6]

Ab in dono mein smallest kaun hai?

5

Aur 5 hi array ka 2nd largest element hai.

Min Heap mein smallest element top par hota hai.

Therefore:

Min Heap ka top = Kth largest element

Ye is question ka main trick hai.

---

# 6. Core Logic

Har element ko Min Heap mein push karenge.

Agar heap ka size `k` se bada ho jaye:

    heap.pop()

Kyun?

Kyuki hume sirf `k` largest elements rakhne hain.

Min Heap automatically smallest element ko top par rakhega.

Isliye extra smallest element ko remove kar denge.

End mein:

    heap.top()

Kth largest element dega.

---

# 7. Detailed Dry Run

nums = [3,2,1,5,6,4]

k = 2

Initially:

heap = []

---

## Step 1: 3

3 ko heap mein push:

heap:

[3]

Size = 1

K = 2

Size <= K

Kuch remove nahi karna.

---

## Step 2: 2

2 ko push:

heap:

[2,3]

Size = 2

Size <= K

Kuch remove nahi karna.

Ab heap mein:

[2,3]

---

## Step 3: 1

1 ko push:

heap:

[1,2,3]

Size = 3

Lekin K = 2.

Size > K.

Isliye top remove:

1 remove ho jayega.

Heap:

[2,3]

Ab heap mein 2 largest elements hain jo abhi tak mile:

2 and 3

---

## Step 4: 5

5 ko push:

heap:

[2,3,5]

Size = 3

Size > K.

Smallest remove:

2 remove.

Heap:

[3,5]

Ab current 2 largest:

3 and 5

---

## Step 5: 6

6 ko push:

heap:

[3,5,6]

Size = 3

Size > K.

Smallest remove:

3 remove.

Heap:

[5,6]

Ab current 2 largest:

5 and 6

---

## Step 6: 4

4 ko push:

heap:

[4,5,6]

Size = 3

Size > K.

Smallest remove:

4 remove.

Heap:

[5,6]

Final heap:

[5,6]

Min Heap ka top:

5

Therefore:

Answer = 5

---

# 8. Code

class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {

        // Min Heap
        priority_queue<int, vector<int>, greater<int>> pq;

        for(int i = 0; i < nums.size(); i++) {

            // Current element heap mein add karo
            pq.push(nums[i]);

            // Sirf k largest elements maintain karne hain
            if(pq.size() > k) {
                pq.pop();
            }
        }

        // Min Heap ka top = kth largest
        return pq.top();
    }
};

---

# 9. Code Explanation

## Step 1: Min Heap Create

priority_queue<int, vector<int>, greater<int>> pq;

C++ mein default priority_queue Max Heap hota hai.

Lekin hume Min Heap chahiye.

Isliye:

greater<int>

use kiya.

---

## Step 2: Array Traverse

for(int i = 0; i < nums.size(); i++)

Har element ko ek-ek karke process karenge.

---

## Step 3: Element Push

pq.push(nums[i]);

Current element ko Min Heap mein add kar diya.

---

## Step 4: Heap Size Check

if(pq.size() > k)

Agar heap mein `k` se zyada elements aa gaye:

pq.pop();

Min Heap mein smallest element top par hota hai.

Isliye smallest element remove ho jayega.

---

# 10. Sabse Important Logic

Suppose:

k = 3

Hum heap mein hamesha sirf 3 largest elements rakhenge.

Example final:

[7,8,10]

Ye 3 largest elements hain.

Min Heap mein:

top = 7

7 inmein smallest hai.

Lekin original array mein:

7 = 3rd largest

Therefore:

Heap Top = Kth Largest

---

# 11. Why Heap Size K?

Ye sabse important point hai.

Agar hum poore array ke elements heap mein rakh dein, toh hume Kth largest directly nahi milega.

Hum sirf `k` largest elements maintain karna chahte hain.

Isliye:

Heap Size = K

Example:

k = 2

Heap:

[5,6]

Top:

5

5 = 2nd largest.

---

# 12. Why Not Max Heap?

Agar Max Heap use karenge:

Top = largest element

Example:

[1,2,3,4,5,6]

Max Heap top:

6

Lekin hume 2nd largest chahiye.

Isliye Max Heap directly useful nahi hai.

Min Heap of size K use karte hain.

---

# 13. Important Pattern

Kth Largest:

    Min Heap

Kth Smallest:

    Max Heap

Reason:

For Kth Largest:

Min Heap ke top par K largest elements mein se smallest element hota hai.

For Kth Smallest:

Max Heap ke top par K smallest elements mein se largest element hota hai.

---

# 14. Time Complexity

Har element ko Heap mein push karte hain.

Heap ka maximum size `k` hai.

Push:

O(log k)

Pop:

O(log k)

Total elements:

n

Therefore:

Time Complexity = O(n log k)

Space Complexity = O(k)

---

# 15. Sorting vs Heap

Sorting Approach:

    Sort array
    O(n log n)

Heap Approach:

    Maintain Min Heap of size k
    O(n log k)

Heap approach especially useful hoti hai jab `k` relatively small ho aur hume sirf Kth element chahiye.

---

# 16. Common Mistakes

## Mistake 1: Max Heap use karna

Kth largest ke liye blindly Max Heap mat lagao.

Correct:

Kth Largest -> Min Heap

---

## Mistake 2: Heap ka size K se bada hone dena

Agar:

pq.size() > k

toh:

pq.pop();

karna zaroori hai.

---

## Mistake 3: Final answer ko pq.size() se nikalna

Answer:

pq.top()

hoga.

---

## Mistake 4: Min Heap aur Max Heap confuse karna

C++ default:

priority_queue<int>

Max Heap hota hai.

Min Heap:

priority_queue<int, vector<int>, greater<int>>

---

# 17. Interview Explanation

Agar interviewer puche:

"How will you find the kth largest element?"

Simple answer:

"I will use a Min Heap of size K.

I will traverse the array and insert every element into the Min Heap.

Whenever the heap size becomes greater than K, I remove the smallest element.

This way, the heap always contains the K largest elements seen so far.

At the end, the smallest element among these K elements, which is at the top of the Min Heap, is the Kth largest element.

The time complexity is O(n log k) and space complexity is O(k)."

---

# 18. Pattern Recognition

Agar question mein aaye:

- Kth largest
- Kth smallest
- Top K
- K largest elements
- K smallest elements

Immediately Heap ke baare mein socho.

For Kth Largest:

    Min Heap of size K

For Kth Smallest:

    Max Heap of size K

---

# 19. Mental Model

Kth Largest ke liye:

Array:

[3,2,1,5,6,4]

K = 2

Hume poora array sort nahi karna.

Bas 2 largest elements maintain karne hain.

Final:

[5,6]

In dono mein smallest:

5

Therefore:

5 = 2nd largest

So:

Min Heap + Size K

---

# 20. One-Line Trick

"Kth Largest ke liye Min Heap of size K rakho; heap mein hamesha K largest elements rahenge aur top unmein sabse chhota, yani Kth largest hoga."

---

# 21. Final Takeaway

LC 215 ka main concept:

Kth Largest
    ↓
Min Heap
    ↓
Heap Size = K
    ↓
Extra element aaye
    ↓
Smallest pop karo
    ↓
End mein pq.top()
    ↓
Kth Largest

Remember:

    Kth Largest  -> Min Heap of size K
    Kth Smallest -> Max Heap of size K

Complexity:

Time = O(n log k)
Space = O(k)
