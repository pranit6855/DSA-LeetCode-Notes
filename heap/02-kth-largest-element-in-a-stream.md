# Heap Pattern

# 02. Kth Largest Element in a Stream

## LeetCode 703

## Pattern
Kth Element + Min Heap

---

# 1. Problem Statement

Design a class that finds the kth largest element in a stream.

We are given:

- An integer `k`
- An initial array of numbers
- After that, new numbers will be added one by one

Every time a new number is added, we have to return the kth largest element.

Example:

k = 3

Initial numbers:

[4,5,8,2]

Then:

add(3)
add(5)
add(10)

Har `add()` ke baad 3rd largest element return karna hai.

---

# 2. Simple Language Mein Problem

LC 215 mein pura array ek saath diya tha.

LC 703 mein numbers continuously aate hain.

Example:

k = 3

Initial:

[4,5,8,2]

Sorted:

[2,4,5,8]

3rd largest = 5

Ab `3` aaya:

[4,5,8,2,3]

Sorted:

[2,3,4,5,8]

3rd largest = 4

Ab `5` aaya:

[4,5,8,2,3,5]

3rd largest = 5

Matlab har naye number ke baad answer update karna hai.

---

# 3. Main Idea

LC 215 mein humne:

Min Heap of size K

use kiya tha.

Yahan bhi exactly same concept use hoga.

Difference sirf itna hai:

LC 215:
Pura array process karke answer nikala.

LC 703:
Numbers one-by-one aate hain, isliye Heap ko continuously maintain karna hai.

---

# 4. Why Min Heap?

Suppose:

k = 3

Hume 3rd largest element chahiye.

Hum Heap mein sirf 3 largest elements rakhenge.

Example:

[5,8,10]

Ye 3 largest elements hain.

Ab in 3 elements mein smallest kaun hai?

5

Min Heap mein smallest element top par hota hai.

Therefore:

Min Heap ka top = 3rd largest

Ye hi main trick hai.

---

# 5. Core Logic

Har naye number ke liye:

1. Number ko Min Heap mein push karo.
2. Agar Heap ka size `k` se bada ho:
   - Smallest element remove karo.
3. Heap ka top return karo.

Short form:

New number
    ↓
Min Heap mein push
    ↓
Size > K?
    ↓
Yes → smallest pop
    ↓
pq.top()
    ↓
Kth Largest

---

# 6. Detailed Dry Run

k = 3

Initial numbers:

[4,5,8,2]

---

## Initial Setup

Start:

Heap = []

---

### Add 4

Push 4:

Heap:

[4]

Size = 1

K = 3

Size K se bada nahi hai.

---

### Add 5

Push 5:

Heap:

[4,5]

Size = 2

K = 3

K se bada nahi hai.

---

### Add 8

Push 8:

Heap:

[4,5,8]

Size = 3

K = 3

K se bada nahi hai.

Ab Heap mein 3 largest elements hain:

4,5,8

Top:

4

Therefore:

3rd largest = 4

---

### Add 2

Push 2:

Heap:

[2,4,5,8]

Size = 4

K = 3

Size > K.

Smallest element remove karo:

2 remove.

Heap:

[4,5,8]

Top:

4

3rd largest = 4

---

# 7. Ab New Stream Element Aaya

Suppose:

add(3)

Current Heap:

[4,5,8]

---

### Step 1

3 ko push karo:

Heap:

[3,4,5,8]

---

### Step 2

Size = 4

K = 3

Size > K.

Smallest:

3

Remove:

Heap:

[4,5,8]

---

### Step 3

Top:

4

Therefore:

add(3) = 4

---

# 8. Another Example

Current Heap:

[4,5,8]

K = 3

Now:

add(10)

Push:

[4,5,8,10]

Size > 3.

Smallest `4` remove:

[5,8,10]

Top:

5

Therefore:

add(10) = 5

---

# 9. Code

class KthLargest {
public:

    // Min Heap
    priority_queue<int, vector<int>, greater<int>> pq;

    int k;

    KthLargest(int k, vector<int>& nums) {

        this->k = k;

        // Initial elements ko process karo
        for(int i = 0; i < nums.size(); i++) {

            pq.push(nums[i]);

            // Sirf k largest elements rakho
            if(pq.size() > k) {
                pq.pop();
            }
        }
    }

    int add(int val) {

        // New element add karo
        pq.push(val);

        // Heap ka size k se zyada nahi hona chahiye
        if(pq.size() > k) {
            pq.pop();
        }

        // Min Heap ka top = Kth largest
        return pq.top();
    }
};

---

# 10. Code Explanation

## Step 1: Min Heap

priority_queue<int, vector<int>, greater<int>> pq;

C++ ka default `priority_queue` Max Heap hota hai.

`greater<int>` use karke Min Heap banaya.

Min Heap mein:

pq.top()

smallest element deta hai.

---

## Step 2: K Store Karna

int k;

Hume pata hona chahiye ki kth largest find karna hai.

Example:

k = 3

Matlab 3rd largest.

---

# 11. Constructor

KthLargest(int k, vector<int>& nums)

Constructor initial numbers ko process karta hai.

---

## `this->k = k`

Class ke variable `k` mein input wala `k` store kar rahe hain.

Simple:

this->k = k

means:

Class ka k = input wala k

---

# 12. Initial Numbers Process

for(int i = 0; i < nums.size(); i++)

Har initial number ko Heap mein daalenge.

Then:

pq.push(nums[i]);

Agar Heap ka size `k` se bada ho:

if(pq.size() > k)

Smallest remove:

pq.pop();

---

# 13. `add()` Function

int add(int val)

Ye function tab chalega jab stream mein new number aayega.

Example:

add(10)

---

## Step 1

New number Heap mein:

pq.push(val);

---

## Step 2

Agar Heap ka size K se bada ho:

if(pq.size() > k)

Smallest remove:

pq.pop();

---

## Step 3

Return:

return pq.top();

Min Heap ka top Kth largest hai.

---

# 14. Most Important Code

Actually pura question ka main logic sirf ye hai:

pq.push(val);

if(pq.size() > k) {
    pq.pop();
}

return pq.top();

Ye same logic constructor mein initial numbers ke liye bhi use hota hai.

---

# 15. LC 215 vs LC 703

## LC 215

Pura array ek saath:

[3,2,1,5,6,4]

Heap maintain karo.

End mein:

pq.top()

Answer.

---

## LC 703

Initial array:

[4,5,8,2]

Then:

add(3)
add(5)
add(10)
...

Har baar:

push
↓
size > k
↓
pop
↓
top return

---

# 16. Why Heap Size K?

Suppose:

k = 3

Hume sirf 3 largest elements ki zarurat hai.

Agar current largest elements hain:

[5,8,10]

Toh 3rd largest:

5

Min Heap mein:

top = 5

Isliye extra elements ko remove karte hain aur Heap ka size exactly K rakhte hain.

---

# 17. Why Smallest Remove Karte Hain?

Suppose K = 3.

Heap:

[3,8,10,15]

Hume sirf 3 largest elements chahiye.

4 elements hain.

Kaunsa remove karna hai?

Smallest:

3

Remaining:

[8,10,15]

Ab ye 3 largest elements hain.

Min Heap ka top:

8

8 = 3rd largest.

---

# 18. Common Mistakes

## Mistake 1

Kth Largest ke liye Max Heap use karna.

Correct:

Kth Largest → Min Heap of size K

---

## Mistake 2

Heap ka size K se bada hone dena.

Correct:

if(pq.size() > k) {
    pq.pop();
}

---

## Mistake 3

`pq.top()` ko largest samajhna.

Yahan Min Heap use ho raha hai.

Therefore:

pq.top() = smallest among K largest elements

Aur wahi Kth largest hai.

---

## Mistake 4

Constructor aur add() ka purpose confuse karna.

Constructor:

Initial numbers ko Heap mein setup karta hai.

add():

New stream number ko process karta hai.

---

# 19. Complexity

Let:

n = total numbers processed

Heap ka maximum size:

K

Har push/pop:

O(log K)

Therefore total:

O(n log K)

Space:

O(K)

---

# 20. Interview Explanation

"I will maintain a Min Heap of size K.

Initially, I insert all the given numbers into the heap while keeping its size at most K.

Whenever a new number comes in, I insert it into the Min Heap.

If the heap size becomes greater than K, I remove the smallest element.

This ensures that the heap always contains the K largest elements seen so far.

Since it is a Min Heap, the top element is the smallest among those K elements, which is exactly the Kth largest element.

Therefore I return the heap top after every add operation."

---

# 21. Pattern Recognition

Jab question mein aaye:

- Kth largest in a stream
- Kth largest after every insertion
- Dynamic Kth largest
- Numbers continuously add ho rahe hain
- Kth largest maintain karna hai

Think:

Min Heap of size K

---

# 22. Mental Model

Kth Largest:

    Min Heap

Heap Size:

    K

New element:

    Push

Size > K:

    Pop smallest

Final:

    Top = Kth Largest

---

# 23. One-Line Trick

"Kth largest ko continuously maintain karne ke liye Min Heap of size K rakho; har new element ke baad extra smallest element hatao, aur top Kth largest hoga."

---

# 24. Final Takeaway

LC 703 basically LC 215 ka dynamic version hai.

LC 215:

    Array → Heap → Answer

LC 703:

    Initial Array → Heap
                     ↓
                 New Element
                     ↓
                    Push
                     ↓
                  Size > K?
                     ↓
                    Pop
                     ↓
                   Top
                     ↓
               Kth Largest

Core pattern:

    Kth Largest
        ↓
    Min Heap
        ↓
    Size = K
        ↓
    Top = Kth Largest

Complexity:

Time = O(n log K)
Space = O(K)
