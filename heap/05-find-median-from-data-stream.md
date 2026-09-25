# Heap Pattern

# 05. Find Median from Data Stream

## LeetCode 295

## Pattern
Two Heaps

---

# 1. Problem Statement

Design a data structure that supports two operations:

1. `addNum(num)` → stream mein ek new number add karo.
2. `findMedian()` → ab tak ke saare numbers ka median return karo.

Numbers ek-ek karke aayenge.

Example:

addNum(1)
→ median = 1

addNum(2)
→ median = 1.5

addNum(3)
→ median = 2

addNum(4)
→ median = 2.5

---

# 2. Simple Language Mein Problem

Hume numbers one by one mil rahe hain.

Har naye number ke baad hume current numbers ka median batana hai.

Example:

Numbers:

[1]

Median:

1

Next:

[1,2]

Median:

(1 + 2) / 2 = 1.5

Next:

[1,2,3]

Median:

2

Next:

[1,2,3,4]

Median:

(2 + 3) / 2 = 2.5

---

# 3. Median Kya Hota Hai?

Pehle numbers ko sorted order mein dekho.

## Odd Number of Elements

Example:

[1,2,3]

Middle element:

2

Median = 2

---

## Even Number of Elements

Example:

[1,2,3,4]

Middle ke 2 elements:

2 and 3

Median:

(2 + 3) / 2

= 2.5

---

# 4. Brute Force Approach

Har baar new number aaye:

1. Number ko list mein add karo.
2. List sort karo.
3. Median nikalo.

Example:

Numbers:

[1,2,3,4,5]

Har baar sorting karni padegi.

Sorting:

O(n log n)

Agar bahut saare numbers stream mein aaye, toh baar-baar sorting expensive ho sakti hai.

Isliye Heap use karenge.

---

# 5. Main Idea

Hum numbers ko 2 halves mein divide karenge.

Small Half:
Smaller numbers

Large Half:
Larger numbers

Small Half ke liye:

Max Heap

Large Half ke liye:

Min Heap

So:

    Small Half → Max Heap
    Large Half → Min Heap

---

# 6. Why Two Heaps?

Suppose sorted numbers:

[1,2,3,4,5]

Hum divide kar sakte hain:

Small Half:

[1,2,3]

Large Half:

[4,5]

Small half ke liye Max Heap use karenge.

So:

left.top() = 3

Large half ke liye Min Heap use karenge.

So:

right.top() = 4

Ab median easily mil gaya:

3

Kyuki total elements odd hain aur left mein ek extra element hai.

---

# 7. Heap Roles

## Left Heap

Max Heap

Isme smaller half ke numbers rahenge.

    left.top()

smaller half ka largest element deta hai.

Ye median ke left side ka last element hai.

---

## Right Heap

Min Heap

Isme larger half ke numbers rahenge.

    right.top()

larger half ka smallest element deta hai.

Ye median ke right side ka first element hai.

---

# 8. Size Balance

Dono heaps ko balanced rakhna hai.

Allowed condition:

    left.size() == right.size()

ya:

    left.size() == right.size() + 1

Matlab:

Left maximum 1 element bada ho sakta hai.

Right left se bada nahi hona chahiye.

---

# 9. Why Left Can Have One Extra?

Odd number of elements mein ek middle element hota hai.

Hum us middle element ko left heap ke top par rakh sakte hain.

Example:

[1,2,3]

Left:

[1,2]

Right:

[3]

Left ka top:

2

Median:

2

Isliye left ko one extra element allow karte hain.

---

# 10. New Number Kahan Jayega?

Suppose:

left.top() = 5

New number:

3

Because:

3 <= 5

Number left mein jayega.

New number:

8

Because:

8 > 5

Number right mein jayega.

So rule:

    num <= left.top()
        ↓
      left

    num > left.top()
        ↓
      right

---

# 11. Main Approach

Har new number ke liye:

## Step 1

Check karo number small half mein jayega ya large half mein.

Small number:

    left

Large number:

    right

---

## Step 2

Heaps ko balance karo.

Agar left bahut bada ho gaya:

    left.top() → right

Agar right bada ho gaya:

    right.top() → left

---

## Step 3

Median return karo.

Agar left ka size bada hai:

    median = left.top()

Agar dono same size hain:

    median = (left.top() + right.top()) / 2

---

# 12. Detailed Dry Run

Suppose:

addNum(1)
addNum(2)
addNum(3)
addNum(4)

---

## addNum(1)

Start:

left = []
right = []

1 ko left mein daalenge.

left:

[1]

right:

[]

Left one extra hai.

Median:

1

---

## addNum(2)

2 > left.top()

2 right mein jayega.

left:

[1]

right:

[2]

Dono same size.

Median:

(1 + 2) / 2

= 1.5

---

## addNum(3)

3 > left.top()

3 right mein jayega.

left:

[1]

right:

[2,3]

Ab right bada ho gaya.

Right ka top:

2

2 ko left mein move karo.

Final:

left:

[1,2]

right:

[3]

Left one extra.

Median:

left.top()

= 2

---

## addNum(4)

4 > left.top()

4 right mein jayega.

left:

[1,2]

right:

[3,4]

Dono same size.

Median:

(left.top() + right.top()) / 2

= (2 + 3) / 2

= 2.5

---

# 13. Code

class MedianFinder {
public:

    // Smaller half
    priority_queue<int> left;

    // Larger half
    priority_queue<int, vector<int>, greater<int>> right;

    MedianFinder() {
    }

    void addNum(int num) {

        // Number ko correct half mein daalo
        if(left.empty() || num <= left.top()) {
            left.push(num);
        }
        else {
            right.push(num);
        }

        // Left too large
        if(left.size() > right.size() + 1) {
            right.push(left.top());
            left.pop();
        }

        // Right too large
        if(right.size() > left.size()) {
            left.push(right.top());
            right.pop();
        }
    }

    double findMedian() {

        // Odd number of elements
        if(left.size() > right.size()) {
            return left.top();
        }

        // Even number of elements
        return (left.top() + right.top()) / 2.0;
    }
};

---

# 14. Code Ka Flow

Code ko 3 main blocks mein samjho.

## Block 1 — Two Heaps

    left  → Max Heap
    right → Min Heap

Left:

    smaller half

Right:

    larger half

---

## Block 2 — addNum()

New number aaya.

Pehle decide:

    small → left
    large → right

Phir balance karo.

Agar left mein extra ho gaya:

    left.top() → right

Agar right mein extra ho gaya:

    right.top() → left

---

## Block 3 — findMedian()

Odd elements:

    left.top()

Even elements:

    (left.top() + right.top()) / 2.0

---

# 15. Why Max Heap on Left?

Suppose:

[1,2,3,4,5]

Small half:

[1,2,3]

Median ke liye hume small half ka largest number chahiye.

That is:

3

Max Heap ka top:

3

Therefore left side ke liye Max Heap useful hai.

---

# 16. Why Min Heap on Right?

Large half:

[4,5]

Median ke liye hume large half ka smallest number chahiye.

That is:

4

Min Heap ka top:

4

Therefore right side ke liye Min Heap useful hai.

---

# 17. Important Concept

Actually dono heaps median ke aas-paas ke 2 important elements ko directly available rakhte hain.

For even count:

    left.top()  = left middle
    right.top() = right middle

For odd count:

    left.top() = middle element

Isliye median O(1) mein mil jata hai.

---

# 18. Important Code Conditions

## Condition 1

if(left.empty() || num <= left.top())

Agar left empty hai:

    left mein daalo

Ya number left ke top se chhota/equal hai:

    left mein daalo

Otherwise:

    right mein daalo

---

## Condition 2

if(left.size() > right.size() + 1)

Agar left mein 1 se zyada extra elements hain:

    left.top() → right

---

## Condition 3

if(right.size() > left.size())

Agar right left se bada ho gaya:

    right.top() → left

---

# 19. Median Formula

## Odd

Example:

[1,2,3]

left size > right size

Median:

    left.top()

---

## Even

Example:

[1,2,3,4]

left size == right size

Median:

    (left.top() + right.top()) / 2.0

---

# 20. Why `/ 2.0`?

Ye important C++ point hai.

Agar:

    (2 + 3) / 2

integer division use hui toh result:

    2

aa sakta hai.

Hume:

    2.5

chahiye.

Isliye:

    / 2.0

use karte hain.

---

# 21. Common Mistakes

## Mistake 1

Left aur right ko constructor ke andar declare kar dena.

Wrong:

    MedianFinder() {
        priority_queue<int> left;
    }

Ye local variables ban jaate hain.

Correct:

    class ke andar variables declare karo.

---

## Mistake 2

Balancing mein `else` use karna.

Wrong idea:

    if(left.size() > right.size() + 1) {
        ...
    }
    else {
        ...
    }

Correct:

    left ke liye separate if

    right ke liye separate if

---

## Mistake 3

Median formula mein brackets bhoolna.

Wrong:

    left.top() + right.top() / 2.0

Correct:

    (left.top() + right.top()) / 2.0

---

## Mistake 4

Right ko left se bada hone dena.

Allowed:

    left = right

or:

    left = right + 1

Not allowed:

    right > left

---

# 22. LC 295 Ka Main Pattern

This is called:

    Two Heaps Pattern

Structure:

    Smaller Half
         ↓
      Max Heap

    Larger Half
         ↓
      Min Heap

Then:

    Balance
       ↓
    Median

---

# 23. Connection With Previous Heap Questions

LC 215:

    Kth Largest
        ↓
    Min Heap of Size K

LC 347:

    Top K Frequent
        ↓
    Frequency Map + Min Heap

LC 973:

    K Closest
        ↓
    Max Heap of Size K

LC 295:

    Median
        ↓
    Two Heaps
        ↓
    Max Heap + Min Heap

---

# 24. Interview Explanation

"I will use two heaps.

A Max Heap will store the smaller half of the numbers, and a Min Heap will store the larger half.

I will keep the heaps balanced so that their sizes are either equal or the left Max Heap has one extra element.

For every new number, I first decide which half it belongs to and insert it into the appropriate heap.

Then I rebalance the heaps if necessary.

If the total number of elements is odd, the median is the top of the left Max Heap.

If it is even, the median is the average of the tops of both heaps.

This allows median retrieval in O(1) and insertion in O(log n)."

---

# 25. Complexity

`addNum()`:

    O(log n)

Because we perform Heap insertion/removal.

`findMedian()`:

    O(1)

Because median is directly available from the Heap tops.

Space:

    O(n)

Because all numbers are stored in the two heaps.

---

# 26. Pattern Recognition

Agar question mein:

- Numbers stream mein aa rahe hain
- Har insertion ke baad median chahiye
- Dynamic median
- Running median
- Median maintain karna hai

Toh:

    Two Heaps

Immediately think:

    Max Heap + Min Heap

---

# 27. Mental Model

Saare numbers ko mentally 2 parts mein divide karo:

    Smaller Half | Larger Half

        ↓             ↓

    Max Heap       Min Heap

        ↓             ↓

   largest small    smallest large
        ↓             ↓

       left.top()   right.top()

              ↓

           Median

---

# 28. One-Line Trick

"Numbers ko 2 halves mein divide karo: smaller half Max Heap mein, larger half Min Heap mein; heaps ko balance rakho, aur tops se median nikaalo."

---

# 29. Final Takeaway

LC 295 ka complete flow:

New Number
    ↓
Small hai?
    ↓
Left Max Heap
    OR
Right Min Heap
    ↓
Balance Heaps
    ↓
Odd:
left.top()
    ↓
Even:
(left.top() + right.top()) / 2.0

Core Pattern:

    Two Heaps
        ↓
    Max Heap + Min Heap
        ↓
    Balance
        ↓
    Median

Complexity:

addNum() = O(log n)

findMedian() = O(1)

Space = O(n)
