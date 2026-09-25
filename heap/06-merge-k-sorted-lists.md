# Heap Pattern

# 06. Merge k Sorted Lists

## LeetCode 23

## Pattern
K-Way Merge + Min Heap

---

# 1. Problem Statement

Given `k` sorted linked lists, merge all of them into one sorted linked list.

Example:

List 1:
1 → 4 → 5

List 2:
1 → 3 → 4

List 3:
2 → 6

Final merged list:

1 → 1 → 2 → 3 → 4 → 4 → 5 → 6

---

# 2. Simple Language Mein Problem

Hume multiple linked lists di gayi hain.

Important baat:

Har linked list already sorted hai.

Example:

1 → 4 → 5
1 → 3 → 4
2 → 6

Hume in sabko combine karke ek sorted list banani hai.

Answer:

1 → 1 → 2 → 3 → 4 → 4 → 5 → 6

---

# 3. Brute Force Approach

Sabhi nodes ko ek normal array/vector mein store kar sakte hain.

Phir saare values ko sort kar sakte hain.

Uske baad sorted values se linked list bana sakte hain.

Lekin sorting ke liye:

O(N log N)

time lagega.

Yahan `N` = total number of nodes.

Hum Heap use karke directly smallest current node choose kar sakte hain.

---

# 4. Main Idea

Har linked list already sorted hai.

Isliye har list ka current first node hi us list ka smallest available node hai.

Example:

List 1:
1 → 4 → 5

List 2:
1 → 3 → 4

List 3:
2 → 6

Current smallest elements:

1
1
2

In 3 values mein se smallest = 1.

Hum isliye Min Heap use karenge.

---

# 5. Heap Mein Kya Rakhenge?

Heap mein har list ka:

    current node

rakhenge.

Hum `{value, list index}` pair use karenge.

Format:

    {node value, list index}

Example:

List 1 ka current node = 1

List 2 ka current node = 1

List 3 ka current node = 2

Heap:

    {1,0}
    {1,1}
    {2,2}

Yahan:

first = node ki value

second = kaunsi list ka node hai

---

# 6. Why Min Heap?

Hume har step par sabse chhota current node chahiye.

Min Heap ka top:

    smallest value

hota hai.

So:

    Min Heap → smallest current node

---

# 7. Core Trick

Har baar:

1. Heap se smallest node nikalo.
2. Us node ko answer mein jodo.
3. Us node ki list ko next node par move karo.
4. Agar next node exist karta hai, usko Heap mein daalo.
5. Repeat.

Main idea:

    Smallest nikalo
        ↓
    Answer mein jodo
        ↓
    Usi list ka next node Heap mein daalo
        ↓
    Repeat

---

# 8. Detailed Example

Lists:

    List 1: 1 → 4 → 5
    List 2: 1 → 3 → 4
    List 3: 2 → 6

Initially Heap:

    {1,0}
    {1,1}
    {2,2}

---

## Step 1

Smallest = 1

List 1 ka node nikala.

Answer:

    1

List 1 ab:

    4 → 5

Isliye `4` Heap mein daalenge.

Heap:

    {1,1}
    {2,2}
    {4,0}

---

## Step 2

Smallest = 1

Ye List 2 ka node hai.

Answer:

    1 → 1

List 2 ab:

    3 → 4

Isliye `3` Heap mein daalenge.

Heap:

    {2,2}
    {3,1}
    {4,0}

---

## Step 3

Smallest = 2

List 3 ka node.

Answer:

    1 → 1 → 2

List 3 ab:

    6

Heap mein `6` daalenge.

Heap:

    {3,1}
    {4,0}
    {6,2}

---

## Step 4

Smallest = 3

Answer:

    1 → 1 → 2 → 3

List 2 ka next:

    4

Heap mein `4` daalenge.

Heap:

    {4,0}
    {4,1}
    {6,2}

---

## Step 5

Smallest = 4

Answer:

    1 → 1 → 2 → 3 → 4

List 1 ka next:

    5

Heap mein `5` daalenge.

---

## Step 6

Next smallest = 4

Answer:

    1 → 1 → 2 → 3 → 4 → 4

List 2 finish ho gayi.

Isliye uska next node nahi hai.

---

## Step 7

Next = 5

Answer:

    1 → 1 → 2 → 3 → 4 → 4 → 5

List 1 finish.

---

## Step 8

Next = 6

Answer:

    1 → 1 → 2 → 3 → 4 → 4 → 5 → 6

All lists finish.

Final answer:

    1 → 1 → 2 → 3 → 4 → 4 → 5 → 6

---

# 9. Code

class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {

        priority_queue<
            pair<int,int>,
            vector<pair<int,int>>,
            greater<pair<int,int>>
        > pq;

        // Har list ka first node Heap mein
        for(int i = 0; i < lists.size(); i++) {

            if(lists[i] != NULL) {
                pq.push({lists[i]->val, i});
            }
        }

        // Answer linked list
        ListNode* dummy = new ListNode(-1);
        ListNode* tail = dummy;

        while(!pq.empty()) {

            // Smallest value aur uski list ka index
            int index = pq.top().second;
            pq.pop();

            // Us list ka current node
            ListNode* node = lists[index];

            // Answer mein node add karo
            tail->next = node;
            tail = node;

            // Us list ko next node par move karo
            lists[index] = lists[index]->next;

            // Agar next node hai toh Heap mein daalo
            if(lists[index] != NULL) {
                pq.push({lists[index]->val, index});
            }
        }

        return dummy->next;
    }
};

---

# 10. Code Ka Simple Flow

## Step 1

Har list ka first node Heap mein daalo.

Example:

    List 1 → 1
    List 2 → 1
    List 3 → 2

Heap:

    {1,0}
    {1,1}
    {2,2}

---

## Step 2

Heap ka smallest nikalo.

    int index = pq.top().second;

`index` batata hai ki node kis list se aaya.

---

## Step 3

Us list ka current node lo.

    ListNode* node = lists[index];

---

## Step 4

Node ko answer mein jodo.

    tail->next = node;
    tail = node;

---

## Step 5

Ab usi list ko next node par move karo.

    lists[index] = lists[index]->next;

Example:

    1 → 4 → 5

1 use kar liya.

Ab:

    lists[index]

4 ko point karega.

---

## Step 6

Agar next node available hai:

    pq.push({lists[index]->val, index});

Matlab:

    Next node ko Heap mein daal do.

---

# 11. `lists[index]` Kya Hai?

Ye important concept hai.

Suppose:

    lists[1]

means:

    List number 1 ka current node

Agar:

    lists[1] = 1 → 3 → 4

Aur humne `1` use kar liya:

    lists[1] = lists[1]->next;

Ab:

    lists[1] = 3 → 4

Matlab hum current pointer ko next node par move kar rahe hain.

---

# 12. Pair `{value, index}` Kyu?

Hum Heap mein:

    {node value, list index}

store kar rahe hain.

Example:

    {1,0}

means:

    value = 1
    list = 0

Agar Heap se `{1,0}` nikla:

    List 0 ka current node use karo.

Isse hume pata chal jaata hai ki next node kis list se lena hai.

---

# 13. `greater<pair<int,int>>` Kya Kar Raha Hai?

Ye:

    greater<pair<int,int>>

Heap ko Min Heap banata hai.

Matlab smallest pair top par aayega.

Hum mainly first value:

    node value

ke basis par smallest node chahte hain.

---

# 14. Dummy Node Kyu?

Hum:

    dummy = new ListNode(-1);

use karte hain.

Ye ek temporary/fake starting node hai.

Example:

    dummy → 1 → 2 → 3

Actual answer:

    dummy->next

So:

    return dummy->next;

returns:

    1 → 2 → 3

Dummy khud answer ka part nahi hai.

---

# 15. Tail Kya Kar Raha Hai?

`tail` answer linked list ke last node ko point karta hai.

Start:

    dummy
      ↑
     tail

Agar `1` add kiya:

    dummy → 1
             ↑
            tail

Next `2`:

    dummy → 1 → 2
                  ↑
                 tail

So:

    tail

hamesha answer ke end par hai.

---

# 16. Main Logic

Code ka actual important part:

    int index = pq.top().second;
    pq.pop();

    ListNode* node = lists[index];

    tail->next = node;
    tail = node;

    lists[index] = lists[index]->next;

    if(lists[index] != NULL) {
        pq.push({lists[index]->val, index});
    }

Meaning:

    Smallest node nikalo
        ↓
    Answer mein jodo
        ↓
    Usi list ko next node par move karo
        ↓
    Next node Heap mein daalo

---

# 17. Why We Don't Put Every Node Into Heap?

Ye important optimization hai.

Agar:

List 1:
1 → 4 → 5

Toh starting mein Heap mein:

    1

hi daalenge.

`4` aur `5` ko abhi Heap mein daalne ki zarurat nahi.

Kyunki `1` ke use hone ke baad hi `4` available hoga.

Isliye Heap mein **har list ka sirf current node** rakha jaata hai.

Ye Heap ko small rakhta hai.

---

# 18. Connection With Previous Heap Questions

LC 215:

    Kth Largest
        ↓
    Min Heap
        ↓
    Size K

LC 347:

    Top K Frequent
        ↓
    Frequency Map + Min Heap

LC 973:

    K Closest
        ↓
    Max Heap
        ↓
    Size K

LC 295:

    Median
        ↓
    Two Heaps

LC 23:

    K Sorted Lists
        ↓
    Min Heap
        ↓
    Smallest current node
        ↓
    Next node add

---

# 19. Common Mistakes

## Mistake 1

Har list ke saare nodes Heap mein daal dena.

Zarurat nahi.

Sirf current node rakho.

---

## Mistake 2

`lists[index]` ko next par move karna bhool jaana.

Correct:

    lists[index] = lists[index]->next;

---

## Mistake 3

Next node ko Heap mein add na karna.

Correct:

    if(lists[index] != NULL) {
        pq.push({lists[index]->val, index});
    }

---

## Mistake 4

Max Heap use karna.

Hume smallest node chahiye.

Correct:

    Min Heap

---

## Mistake 5

`return dummy` kar dena.

Correct:

    return dummy->next;

---

# 20. Complexity

Let:

`N` = total number of nodes

`K` = number of linked lists

Heap mein maximum `K` nodes rahenge.

Har node Heap mein ek baar insert hota hai.

Har node Heap se ek baar remove hota hai.

Therefore:

Time Complexity:

    O(N log K)

Space Complexity:

    O(K)

---

# 21. Interview Explanation

"I will use a Min Heap for the k-way merge.

Initially, I will insert the first node of every non-empty linked list into the Min Heap.

The Heap always contains the current smallest available node from each list.

I will repeatedly remove the smallest node and attach it to the result list.

Then I will move that node's list to its next node and insert the next node into the Heap if it exists.

This process continues until the Heap becomes empty.

The time complexity is O(N log K), where N is the total number of nodes and K is the number of lists."

---

# 22. Pattern Recognition

Agar question mein aaye:

- Merge K sorted lists
- Merge multiple sorted arrays/lists
- K sorted sequences
- Find smallest among multiple sorted sequences
- Multiple sorted sources ko merge karna

Think:

    K-Way Merge + Min Heap

---

# 23. Mental Model

Har list ka ek current candidate:

    List 1 → 1
    List 2 → 1
    List 3 → 2

Heap:

    1
    1
    2

Smallest:

    1

Usko answer mein daalo.

Phir usi list ka next:

    4

Heap mein daalo.

Again smallest choose karo.

Repeat.

---

# 24. One-Line Trick

"Har sorted list ka current first node Min Heap mein rakho; smallest node nikalo, answer mein lagao, aur usi list ka next node Heap mein daal do."

---

# 25. Final Takeaway

LC 23 ka complete flow:

    K Sorted Lists
          ↓
    Har list ka current node
          ↓
       Min Heap
          ↓
    Smallest node nikalo
          ↓
      Answer mein jodo
          ↓
    Usi list ka next node
          ↓
       Heap mein daalo
          ↓
         Repeat
          ↓
    Sorted merged list

Core Pattern:

    K-Way Merge
        +
    Min Heap

Complexity:

    Time = O(N log K)
    Space = O(K)
