# Heap Pattern

# 04. K Closest Points to Origin

## LeetCode 973

## Pattern
K Closest Elements + Max Heap

---

# 1. Problem Statement

Given an array of points where:

points[i] = [x, y]

and an integer `k`.

We have to return the `k` points that are closest to the origin `(0,0)`.

The answer can be returned in any order.

Example:

points = [[1,3],[-2,2],[5,8]]
k = 2

Answer:

[[1,3],[-2,2]]

Because these two points are closest to `(0,0)`.

---

# 2. Simple Language Mein Problem

Hume kuch points diye hain.

Example:

[1,3]
[-2,2]
[5,8]

Aur:

k = 2

Matlab:

Origin `(0,0)` ke sabse paas wale 2 points return karo.

---

# 3. Origin Kya Hai?

Origin:

(0,0)

Har point ki distance origin se compare karni hai.

Example:

Point = [1,3]

Distance:

1² + 3² = 10

Point = [-2,2]

Distance:

(-2)² + 2² = 8

Point = [5,8]

Distance:

5² + 8² = 89

So:

[-2,2] → 8
[1,3] → 10
[5,8] → 89

Smaller distance means point is closer.

Therefore closest 2 points:

[-2,2]
[1,3]

---

# 4. Distance Formula

Normal distance formula:

sqrt(x² + y²)

But hume actual distance nahi chahiye.

Sirf comparison karna hai.

Isliye square root ignore kar sakte hain.

Hum:

distance = x² + y²

use karenge.

Example:

[1,3]

distance = 1*1 + 3*3
         = 1 + 9
         = 10

---

# 5. Brute Force Approach

Sabhi points ki distance calculate karo.

Phir distances ko sort karo.

Uske baad first `k` closest points le lo.

Lekin sorting ki wajah se:

O(n log n)

time lag sakta hai.

Heap se hum sirf `k` closest points maintain kar sakte hain.

---

# 6. Main Heap Approach

Yahan hum:

Max Heap of size K

use karenge.

Ye thoda opposite lag sakta hai.

Agar hume closest points chahiye toh Max Heap kyun?

Reason:

Hume Heap mein sirf `k` closest points rakhne hain.

Agar ek extra point aa gaya, toh hume usme se sabse farthest point remove karna hai.

Max Heap ka top:

largest distance

hota hai.

Matlab:

Max Heap ka top = farthest point

Isliye extra point aane par farthest point ko easily remove kar sakte hain.

---

# 7. Core Idea

Har point ke liye:

1. `x` aur `y` nikalo.
2. Distance calculate karo.
3. `{distance, {x,y}}` Heap mein push karo.
4. Agar Heap ka size `k` se bada ho:
   - Farthest point pop karo.
5. End mein Heap mein `k` closest points bachenge.

---

# 8. Why Max Heap?

Suppose:

k = 2

Current points ke distances:

10
8

Ye 2 closest points hain.

Ab new point ka distance:

89

Heap:

89
10
8

Ab hume sirf 2 closest rakhne hain.

Kaunsa remove karna hai?

89

Kyunki 89 sabse far hai.

Max Heap ka top:

89

So:

pq.pop()

aur 89 remove ho jayega.

Bachenge:

10
8

Ye 2 closest hain.

---

# 9. Detailed Dry Run

points = [[1,3],[-2,2],[5,8]]

k = 2

---

## Point 1

Point:

[1,3]

x = 1
y = 3

Distance:

1*1 + 3*3 = 10

Heap:

{10,[1,3]}

Size = 1

K = 2

Nothing remove.

---

## Point 2

Point:

[-2,2]

x = -2
y = 2

Distance:

(-2)*(-2) + 2*2 = 8

Heap:

{10,[1,3]}
{8,[-2,2]}

Size = 2

K = 2

Nothing remove.

---

## Point 3

Point:

[5,8]

x = 5
y = 8

Distance:

5*5 + 8*8

= 25 + 64

= 89

Push into Heap.

Now:

{89,[5,8]}
{10,[1,3]}
{8,[-2,2]}

Size = 3

But K = 2.

So:

size > k

Therefore pop the top.

Max Heap ka top:

{89,[5,8]}

Ye farthest point hai.

Remove it.

Final Heap:

{10,[1,3]}
{8,[-2,2]}

Ab Heap mein sirf 2 closest points hain.

---

# 10. Code

class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {

        priority_queue<pair<int, pair<int, int>>> pq;

        for(int i = 0; i < points.size(); i++) {

            int x = points[i][0];
            int y = points[i][1];

            int distance = x * x + y * y;

            pq.push({distance, {x, y}});

            if(pq.size() > k) {
                pq.pop();
            }
        }

        vector<vector<int>> res;

        while(!pq.empty()) {

            res.push_back({
                pq.top().second.first,
                pq.top().second.second
            });

            pq.pop();
        }

        return res;
    }
};

---

# 11. Code Explanation

## Max Heap

priority_queue<pair<int, pair<int, int>>> pq;

Ye Max Heap hai.

Heap mein:

{distance, {x,y}}

store kar rahe hain.

Example:

{10,{1,3}}

Matlab:

distance = 10
point = [1,3]

---

## Current Point

int x = points[i][0];
int y = points[i][1];

Har point:

[x,y]

format mein hai.

Example:

points[i] = [1,3]

So:

x = 1
y = 3

---

## Distance

int distance = x * x + y * y;

Distance ka squared value calculate kar rahe hain.

Square root ki zarurat nahi.

---

## Heap Mein Push

pq.push({distance, {x, y}});

Example:

distance = 10
point = [1,3]

Heap mein:

{10,{1,3}}

---

## Heap Size Check

if(pq.size() > k) {
    pq.pop();
}

Agar k se zyada points ho gaye:

sabse bada distance remove karo.

Max Heap mein sabse bada distance top par hota hai.

Isliye `pop()` farthest point ko remove karta hai.

---

# 12. Answer Kaise Niklega?

Final Heap mein:

{10,{1,3}}
{8,{-2,2}}

Ab hume point chahiye, distance nahi.

Heap ka format:

{distance,{x,y}}

So:

pq.top().second

gives:

{x,y}

Then:

pq.top().second.first

gives `x`

and:

pq.top().second.second

gives `y`

Isliye:

res.push_back({
    pq.top().second.first,
    pq.top().second.second
});

Point answer mein add ho jayega.

---

# 13. `points[i][0]` and `points[i][1]`

Har point:

[x,y]

Example:

points[i] = [1,3]

Then:

points[i][0] = 1
points[i][1] = 3

Therefore:

int x = points[i][0];
int y = points[i][1];

Yaad rakho:

0 = first value
1 = second value

---

# 14. Most Important Logic

LC 973 ka actual main logic:

distance = x*x + y*y

then:

{distance, point}

Max Heap mein push karo.

Agar:

heap size > k

toh:

farthest point pop karo.

End mein:

k closest points bachenge.

---

# 15. LC 215 Se Connection

LC 215:

Kth Largest Element

    Min Heap
    Size K
    Smallest pop

LC 973:

K Closest Points

    Max Heap
    Size K
    Largest/Farthest pop

Dono mein important pattern:

    Heap Size = K

Difference:

LC 215 mein largest elements maintain karte hain.

LC 973 mein closest elements maintain karte hain.

---

# 16. Why Not Min Heap?

Agar Min Heap use karenge:

Top par smallest distance rahega.

Lekin jab heap mein extra point aayega, hume:

farthest point

remove karna hai.

Min Heap mein farthest point easily top par nahi milega.

Isliye Max Heap better hai.

---

# 17. Common Mistakes

## Mistake 1

K Closest ke liye Min Heap blindly use karna.

Correct:

K Closest → Max Heap

---

## Mistake 2

Actual square root calculate karna.

Need nahi hai.

Use:

distance = x*x + y*y

---

## Mistake 3

Heap mein sirf distance store karna.

Hume end mein original point bhi chahiye.

Isliye:

{distance, {x,y}}

store karte hain.

---

## Mistake 4

Heap size K se bada hone dena.

Correct:

if(pq.size() > k) {
    pq.pop();
}

---

## Mistake 5

Answer mein distance daal dena.

Hume point chahiye:

[x,y]

Isliye:

pq.top().second

se point nikalna hai.

---

# 18. Complexity

Har point ko Heap mein insert karte hain.

Heap ka maximum size:

K

Push:

O(log K)

Pop:

O(log K)

For `n` points:

Time Complexity = O(n log K)

Space Complexity = O(K)

---

# 19. Interview Explanation

"I will use a Max Heap of size K.

For every point, I calculate its squared distance from the origin using x² + y².

I insert the distance along with the point into the Max Heap.

Whenever the heap size becomes greater than K, I remove the top element.

Since it is a Max Heap, the top element has the largest distance, which means it is the farthest point.

This keeps only the K closest points in the heap.

Finally, I extract those K points and return them."

---

# 20. Pattern Recognition

Agar question mein aaye:

- K closest points
- K nearest points
- K smallest distances
- Closest to origin
- Closest K elements

Think:

Max Heap of size K

Because:

    Extra element
        ↓
    Farthest remove

---

# 21. Mental Model

K Closest:

        Point
          ↓
      Distance
          ↓
      Max Heap
          ↓
      Size > K?
          ↓
      Farthest pop
          ↓
      K closest remain

---

# 22. One-Line Trick

"K closest points ke liye Max Heap of size K rakho; jab extra point aaye toh largest distance yani farthest point hata do."

---

# 23. Final Takeaway

LC 973 ka core pattern:

K Closest
    ↓
Distance calculate
    ↓
Max Heap
    ↓
Size = K
    ↓
Extra aaye
    ↓
Farthest pop
    ↓
K closest remain

Remember:

    Kth Largest → Min Heap of size K

    K Closest → Max Heap of size K

Complexity:

Time = O(n log K)

Space = O(K)
