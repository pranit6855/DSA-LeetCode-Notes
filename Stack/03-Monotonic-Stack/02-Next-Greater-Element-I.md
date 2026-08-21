# LeetCode 496 — Next Greater Element I

**Pattern:** Stack — Monotonic Stack

---

# Problem

Hume do arrays diye gaye hain:

```text
nums1 = [4,1,2]
nums2 = [1,3,4,2]
```

`nums1` ka har element `nums2` mein present hoga.

Hume `nums2` mein har element ka:

> **Next Greater Element**

find karna hai.

Next Greater Element ka matlab:

> Current element ke **right side mein pehla aisa element jo current se bada ho**.

Agar aisa element nahi milta:

```text
-1
```

return karna hai.

---

# Example

Input:

```text
nums1 = [4,1,2]
nums2 = [1,3,4,2]
```

Output:

```text
[-1,3,-1]
```

---

# Example Explanation

## Element `4`

`nums2`:

```text
[1,3,4,2]
     ↑
```

`4` ke right mein:

```text
[2]
```

Hai.

Lekin:

```text
2 > 4
```

false hai.

Isliye:

```text
4 → -1
```

---

## Element `1`

Right side:

```text
[3,4,2]
```

Sabse pehla greater:

```text
3
```

So:

```text
1 → 3
```

---

## Element `2`

`2` ke right mein kuch nahi hai.

So:

```text
2 → -1
```

Final:

```text
[-1,3,-1]
```

---

# Pattern Identification

Agar question mein words aayein:

```text
Next Greater Element
Next Larger Element
First Greater Element
Greater Element on Right
```

to:

```text
MONOTONIC STACK
```

ka thought aana chahiye.

Ye Pattern #3 ka classic problem hai.

---

# Connection With LC 739

Humne previous question:

```text
LC 739 — Daily Temperatures
```

mein bhi same Monotonic Stack logic use kiya tha.

### LC 739

Current element greater mila:

```text
current > stack.top()
```

to old element ka answer calculate kiya:

```text
currentIndex - oldIndex
```

### LC 496

Current element greater mila:

```text
current > stack.top()
```

to old element ka answer:

```text
oldElement → currentElement
```

So **Stack logic same hai**.

Difference sirf answer calculate karne ka hai.

---

# Main Idea

Hum `nums2` ko left se right traverse karenge.

Stack mein woh elements rahenge:

> jinka Next Greater Element abhi tak nahi mila.

Jab current element Stack ke top se bada hota hai:

```text
current > stack.top()
```

to current element hi Stack ke top ka Next Greater Element hai.

Isliye:

```text
stack.top() → current
```

Then:

```text
pop()
```

Current element ko bhi future ke liye Stack mein push karenge.

---

# Data Structures

Hum 3 cheezein use karenge:

### 1. Stack

```cpp
stack<int> st;
```

Isme unresolved elements store karenge.

---

### 2. Hash Map

```cpp
unordered_map<int,int> mp;
```

Isme:

```text
element → next greater element
```

store karenge.

Example:

```text
1 → 3
3 → 4
4 → -1
2 → -1
```

---

### 3. Answer Vector

```cpp
vector<int> ans;
```

End mein `nums1` ke according answer fill karenge.

---

# Why Map?

`nums2` mein hum sab elements ka Next Greater Element nikal lenge.

Lekin question answer `nums1` ke order mein maang raha hai.

Example:

```text
nums2 = [1,3,4,2]
nums1 = [4,1,2]
```

Map:

```text
1 → 3
3 → 4
4 → -1
2 → -1
```

Ab `nums1` ko simply lookup kar sakte hain:

```text
4 → -1
1 → 3
2 → -1
```

---

# Algorithm

```text
Start
  ↓
Empty Stack banao
  ↓
Empty Map banao
  ↓
nums2 ko left → right traverse karo
  ↓
Current element > Stack Top?
  ├── YES
  │    ↓
  │   Stack Top ka answer = Current
  │    ↓
  │   Pop
  │    ↓
  │   Again check
  │
  └── NO
       ↓
Current ko Stack mein push karo

  ↓
nums2 traversal complete
  ↓
Stack mein jo elements bache
unka answer = -1

  ↓
nums1 traverse karo
  ↓
Map se answer nikalo
  ↓
Return
```

---

# Step 1 — Stack Create

```cpp
stack<int> st;
```

Starting:

```text
stack = []
```

---

# Step 2 — Map Create

```cpp
unordered_map<int,int> mp;
```

Initially:

```text
mp = {}
```

---

# Step 3 — nums2 Traverse

```cpp
for(int i = 0; i < nums2.size(); i++)
```

Hum:

```text
1 → 3 → 4 → 2
```

ko process karenge.

---

# Detailed Dry Run

Input:

```text
nums2 = [1,3,4,2]
```

Initial:

```text
stack = []
mp = {}
```

---

# i = 0

Current:

```text
1
```

Stack empty hai.

While loop nahi chalega.

Push:

```cpp
st.push(1);
```

Stack:

```text
[1]
```

Meaning:

> `1` ka Next Greater abhi nahi mila.

---

# i = 1

Current:

```text
3
```

Stack:

```text
[1]
```

Check:

```text
3 > 1
```

True.

Matlab:

> `1` ka Next Greater Element `3` hai.

So:

```cpp
mp[st.top()] = nums2[i];
```

becomes:

```cpp
mp[1] = 3;
```

Map:

```text
1 → 3
```

Then:

```cpp
st.pop();
```

Stack:

```text
[]
```

Ab current `3` ko Stack mein push karo:

```cpp
st.push(3);
```

Stack:

```text
[3]
```

---

# i = 2

Current:

```text
4
```

Stack:

```text
[3]
```

Check:

```text
4 > 3
```

True.

So:

```text
3 → 4
```

Code:

```cpp
mp[3] = 4;
```

Map:

```text
1 → 3
3 → 4
```

Pop:

```cpp
st.pop();
```

Stack:

```text
[]
```

Push current:

```cpp
st.push(4);
```

Stack:

```text
[4]
```

---

# i = 3

Current:

```text
2
```

Stack:

```text
[4]
```

Check:

```text
2 > 4
```

False.

Matlab:

> `4` ka Next Greater abhi nahi mila.

So current `2` ko push karo:

```cpp
st.push(2);
```

Stack:

```text
[4,2]
```

---

# Important Observation

Stack mein temperatures:

```text
4
2
```

decreasing order mein hain.

Ye hi Monotonic Stack ka concept hai.

---

# End of nums2

Ab:

```text
stack = [4,2]
```

Dono ke Next Greater Element nahi mila.

Isliye:

```cpp
while(!st.empty()) {
    mp[st.top()] = -1;
    st.pop();
}
```

---

## Top = 2

```cpp
mp[2] = -1;
```

Then:

```cpp
st.pop();
```

Stack:

```text
[4]
```

---

## Top = 4

```cpp
mp[4] = -1;
```

Then:

```cpp
st.pop();
```

Stack:

```text
[]
```

Final map:

```text
1 → 3
3 → 4
4 → -1
2 → -1
```

---

# Step 4 — nums1 Traverse

`nums1`:

```text
[4,1,2]
```

Code:

```cpp
for(int num : nums1) {
    ans.push_back(mp[num]);
}
```

---

## `num = 4`

```text
mp[4] = -1
```

So:

```text
ans = [-1]
```

---

## `num = 1`

```text
mp[1] = 3
```

So:

```text
ans = [-1,3]
```

---

## `num = 2`

```text
mp[2] = -1
```

So:

```text
ans = [-1,3,-1]
```

Final:

```text
[-1,3,-1]
```

---

# Complete C++ Code

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {

        int n = nums2.size();

        unordered_map<int, int> mp;
        vector<int> ans;
        stack<int> st;

        for (int i = 0; i < n; i++) {

            while (!st.empty() && st.top() < nums2[i]) {

                mp[st.top()] = nums2[i];
                st.pop();
            }

            st.push(nums2[i]);
        }

        while (!st.empty()) {

            mp[st.top()] = -1;
            st.pop();
        }

        for (int num : nums1) {
            ans.push_back(mp[num]);
        }

        return ans;
    }
};
```

---

# Code Explanation

## `n`

```cpp
int n = nums2.size();
```

`nums2` ki length.

---

## Stack

```cpp
stack<int> st;
```

Unresolved elements store karta hai.

---

## Map

```cpp
unordered_map<int,int> mp;
```

Next Greater Element store karta hai.

---

## Main While Loop

```cpp
while (!st.empty() && st.top() < nums2[i])
```

Ye sabse important part hai.

Matlab:

```text
Current element
       ↓
Stack Top se bada?
       ↓
    YES
       ↓
Stack Top ka answer = Current
       ↓
      POP
```

Example:

```text
stack top = 3
current = 4
```

Then:

```text
3 → 4
```

---

## Push

```cpp
st.push(nums2[i]);
```

Current element ko future ke greater element ka wait karne do.

---

# Why `while`, Not `if`?

Suppose:

```text
stack = [1,2,3]
current = 5
```

Check:

```text
5 > 3
```

So:

```text
3 → 5
```

Pop.

Ab:

```text
5 > 2
```

So:

```text
2 → 5
```

Pop.

Ab:

```text
5 > 1
```

So:

```text
1 → 5
```

Pop.

Isliye ek current element multiple previous elements ko resolve kar sakta hai.

Therefore:

```cpp
while(...)
```

use karte hain.

---

# Why Stack Stores Values Here?

739 mein humne **indexes** store kiye the:

```cpp
stack<int> st;
st.push(i);
```

because hume:

```text
current index - old index
```

calculate karna tha.

496 mein hume sirf:

```text
old value → current value
```

chahiye.

Isliye directly values store karna simpler hai:

```cpp
st.push(nums2[i]);
```

---

# Why `st.top()` Directly Use Kar Rahe Hain?

Kyuki Stack mein already values stored hain.

Example:

```text
stack = [4,2]
```

Then:

```cpp
st.top()
```

gives:

```text
2
```

So:

```cpp
st.top() < nums2[i]
```

directly compare kar sakte hain.

Ye **galat** hoga:

```cpp
nums2[st.top()]
```

because `st.top()` yahan index nahi, value hai.

---

# Common Mistake

Agar stack mein:

```cpp
st.push(nums2[i]);
```

kar rahe ho, to:

```cpp
st.top()
```

already value hai.

Correct:

```cpp
st.top() < nums2[i]
```

Wrong:

```cpp
nums2[st.top()] < nums2[i]
```

---

# Common Mistake 2

Ye code:

```cpp
while(!st.empty()) {
    mp[st.top()] = -1;
}
```

wrong hai.

Kyunki `st.pop()` nahi hai.

Stack kabhi empty nahi hoga.

Correct:

```cpp
while(!st.empty()) {
    mp[st.top()] = -1;
    st.pop();
}
```

---

# 739 vs 496

## LC 739 — Daily Temperatures

```text
Current > Stack Top
↓
Pop
↓
ans[oldIndex] = currentIndex - oldIndex
```

## LC 496 — Next Greater Element I

```text
Current > Stack Top
↓
Pop
↓
mp[oldValue] = currentValue
```

Same Monotonic Stack.

Bas output different hai.

---

# Complexity

Let:

```text
n = nums2.size()
m = nums1.size()
```

### Time

`nums2` ka har element:

```text
push → at most once
pop  → at most once
```

So:

```text
O(n)
```

`nums1` traverse:

```text
O(m)
```

Overall:

```text
O(n + m)
```

### Space

Stack:

```text
O(n)
```

Map:

```text
O(n)
```

Answer:

```text
O(m)
```

Overall:

```text
O(n + m)
```

---

# Interview Explanation

> I process `nums2` using a decreasing monotonic stack. The stack contains elements whose next greater element has not been found yet. When the current element is greater than the stack top, the current element is the next greater element for that stack top, so I store this relation in a hash map and pop the stack. I continue until the current element is no longer greater than the stack top. After processing `nums2`, all remaining stack elements have no greater element, so their answer is `-1`. Finally, I use the map to generate the answers for `nums1`.

---

# Pattern Recognition

Aage agar question mein:

```text
Next Greater
Next Larger
First Greater on Right
Nearest Greater
```

dikhe:

```text
MONOTONIC STACK
```

socho.

Core template:

```cpp
while (!st.empty() && current > st.top()) {

    mp[st.top()] = current;
    st.pop();
}

st.push(current);
```

---

# One-Line Revision

> **Current element agar Stack ke top se bada hai, to Stack top ka Next Greater current element hai; usko map karo, pop karo, aur current ko Stack mein push karo.**

---

# Pattern #3 Progress

* [x] LC 739 — Daily Temperatures
* [x] LC 496 — Next Greater Element I
* [ ] LC 503 — Next Greater Element II
* [ ] LC 901 — Online Stock Span
* [ ] LC 1475 — Final Prices With a Special Discount in a Shop
* [ ] LC 1673 — Find the Most Competitive Subsequence

---

# Folder Structure

```text
Stack/
│
├── 01-Matching-Pairing/
│   ├── 01-Valid-Parentheses.md
│   ├── 02-Minimum-Add-to-Make-Parentheses-Valid.md
│   └── 03-Minimum-Remove-to-Make-Valid-Parentheses.md
│
├── 02-Stack-Simulation/
│
├── 03-Monotonic-Stack/
│   ├── 01-Daily-Temperatures.md
│   └── 02-Next-Greater-Element-I.md
│
├── 04-Next-Previous-Greater-Smaller/
│
├── 05-Boundary-Contribution/
│
└── 06-Stack-Design/
```
