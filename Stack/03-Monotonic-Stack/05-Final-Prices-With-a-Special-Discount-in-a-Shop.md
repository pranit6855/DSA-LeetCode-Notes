# LeetCode 1475 — Final Prices With a Special Discount in a Shop

**Pattern:** Stack — Monotonic Stack

---

# Problem

Hume ek integer array `prices` diya gaya hai.

Har item ke liye ek special discount mil sakta hai.

Discount ka rule:

> Current price ke right side mein jo **first price less than or equal to current price** milega, wahi discount hoga.

Final price:

```text
final price = current price - discount
```

Agar right side mein koi price `<= current price` nahi milta, to koi discount nahi milega.

---

# Example 1

```text
Input:
prices = [8,4,6,2,3]

Output:
[4,2,4,2,3]
```

### Explanation

For `8`:

First price `<= 8`:

```text
4
```

So:

```text
8 - 4 = 4
```

For `4`:

First price `<= 4`:

```text
2
```

So:

```text
4 - 2 = 2
```

For `6`:

First price `<= 6`:

```text
2
```

So:

```text
6 - 2 = 4
```

For `2`:

Right side:

```text
3
```

But:

```text
3 <= 2
```

false.

So no discount:

```text
2
```

For `3`:

No element after it.

So:

```text
3
```

Final:

```text
[4,2,4,2,3]
```

---

# Pattern Identification

Agar question mein:

```text
Next smaller
Next smaller or equal
First smaller
Discount
Less than or equal on right
```

jaise words dikhein, to:

```text
MONOTONIC STACK
```

ka thought aana chahiye.

---

# Connection With Previous Monotonic Stack Questions

## LC 739 — Daily Temperatures

```text
current > stack top
→ next greater
```

## LC 496 — Next Greater Element I

```text
current > stack top
→ next greater
```

## LC 503 — Next Greater Element II

```text
current > stack top
→ next greater
```

## LC 901 — Online Stock Span

```text
current >= stack top
→ previous smaller/equal spans
```

## LC 1475 — Final Prices

```text
current <= stack top
→ next smaller/equal
→ use as discount
```

So yahan main difference hai:

```text
Greater problems → current > stack top
Smaller problems → current <= stack top
```

---

# Main Idea

Hum left se right traverse karenge.

Stack mein:

```text
unresolved indexes
```

store karenge.

Meaning:

> In indexes ko abhi tak unka discount nahi mila.

Jab current price kisi Stack top price se:

```text
current <= stack top
```

hoti hai, to current price hi Stack top ka discount hai.

So:

```text
stack top → current
```

Then us item ka final price update karenge aur index ko pop kar denge.

---

# Why Store Index?

Hum Stack mein:

```cpp
stack<int> st;
```

use karenge.

Stack ke andar indexes honge.

Example:

```text
prices = [8,4,6,2,3]

index:   0 1 2 3 4
price:   8 4 6 2 3
```

Agar Stack:

```text
[0,2]
```

hai, iska matlab:

```text
index 0 → price 8
index 2 → price 6
```

Actual price:

```cpp
prices[st.top()]
```

se milegi.

Index store karne se hum original array mein directly answer update kar sakte hain:

```cpp
prices[index] -= prices[i];
```

---

# Main Rule

Current price:

```text
prices[i]
```

Stack top price:

```text
prices[st.top()]
```

Agar:

```text
prices[i] <= prices[st.top()]
```

to:

```text
current price = discount
```

Therefore:

```cpp
prices[index] -= prices[i];
```

Then:

```cpp
st.pop();
```

Ye repeat hota rahega.

---

# Algorithm

```text
Start
  ↓
Empty Stack banao
  ↓
prices ko left → right traverse karo
  ↓
Current price <= Stack Top price?
  ├── YES
  │    ↓
  │   Stack Top ko current price ka discount do
  │    ↓
  │   prices[index] -= prices[i]
  │    ↓
  │   pop
  │    ↓
  │   Again check
  │
  └── NO
       ↓
Current index push
  ↓
Next element
  ↓
Return prices
```

---

# Step-by-Step Approach

## Step 1 — Stack

```cpp
stack<int> st;
```

Stack mein unresolved item ke indexes store honge.

---

## Step 2 — Traverse

```cpp
for (int i = 0; i < prices.size(); i++)
```

Har price ko left se right process karenge.

---

## Step 3 — While Condition

```cpp
while (!st.empty() &&
       prices[i] <= prices[st.top()])
```

Do conditions hain.

### Condition 1

```cpp
!st.empty()
```

Stack mein koi unresolved item hona chahiye.

### Condition 2

```cpp
prices[i] <= prices[st.top()]
```

Current price Stack top se less than or equal honi chahiye.

Agar true:

> Current price Stack top ka discount hai.

---

# Step 4 — Index Save Karo

```cpp
int index = st.top();
```

Example:

```text
st.top() = 0
```

Matlab current discount index `0` wale item ko milega.

---

# Step 5 — Pop

```cpp
st.pop();
```

Ab us item ka discount mil chuka hai.

Therefore usko Stack mein rakhne ki need nahi.

---

# Step 6 — Discount Apply

```cpp
prices[index] -= prices[i];
```

Example:

```text
index = 0
prices[0] = 8
prices[i] = 4
```

Then:

```text
8 - 4 = 4
```

So:

```text
prices[0] = 4
```

---

# Step 7 — Current Index Push

While loop ke baad:

```cpp
st.push(i);
```

Current price ko Stack mein store karo because future mein iska discount mil sakta hai.

---

# Complete C++ Code

```cpp
class Solution {
public:
    vector<int> finalPrices(vector<int>& prices) {

        stack<int> st;

        for (int i = 0; i < prices.size(); i++) {

            while (!st.empty() &&
                   prices[i] <= prices[st.top()]) {

                int index = st.top();
                st.pop();

                prices[index] -= prices[i];
            }

            st.push(i);
        }

        return prices;
    }
};
```

---

# Detailed Dry Run

Input:

```text
prices = [8,4,6,2,3]
```

Indexes:

```text
index:  0 1 2 3 4
price:  8 4 6 2 3
```

Initially:

```text
stack = []
prices = [8,4,6,2,3]
```

---

# i = 0

Current:

```text
prices[0] = 8
```

Stack empty.

While nahi chalega.

Push:

```cpp
st.push(0);
```

Stack:

```text
[0]
```

Meaning:

```text
index 0 → price 8
```

---

# i = 1

Current:

```text
prices[1] = 4
```

Stack:

```text
[0]
```

Top index:

```text
0
```

Top price:

```text
prices[0] = 8
```

Check:

```text
4 <= 8
```

True.

So current `4` is the first valid discount for price `8`.

---

## Apply Discount

```cpp
int index = st.top();
```

```text
index = 0
```

Pop:

```cpp
st.pop();
```

Stack:

```text
[]
```

Apply:

```cpp
prices[index] -= prices[i];
```

So:

```text
prices[0] = 8 - 4
           = 4
```

Array:

```text
[4,4,6,2,3]
```

Now push current index:

```cpp
st.push(1);
```

Stack:

```text
[1]
```

---

# i = 2

Current:

```text
prices[2] = 6
```

Stack:

```text
[1]
```

Top price:

```text
prices[1] = 4
```

Check:

```text
6 <= 4
```

False.

No discount can be given to price `4` from current `6`.

Push:

```cpp
st.push(2);
```

Stack:

```text
[1,2]
```

Prices:

```text
[4,4,6,2,3]
```

---

# i = 3

Current:

```text
prices[3] = 2
```

Stack:

```text
[1,2]
```

Top:

```text
index = 2
price = 6
```

Check:

```text
2 <= 6
```

True.

So `2` is discount for `6`.

Apply:

```text
prices[2] = 6 - 2
           = 4
```

Prices:

```text
[4,4,4,2,3]
```

Pop index `2`.

Stack:

```text
[1]
```

---

## While Again

Top:

```text
index = 1
price = 4
```

Current:

```text
2
```

Check:

```text
2 <= 4
```

True.

So `2` is also the first valid discount for this `4`.

Apply:

```text
prices[1] = 4 - 2
           = 2
```

Prices:

```text
[4,2,4,2,3]
```

Pop:

```text
stack = []
```

---

## Push Current

Current index:

```text
3
```

Push:

```cpp
st.push(3);
```

Stack:

```text
[3]
```

---

# i = 4

Current:

```text
prices[4] = 3
```

Stack:

```text
[3]
```

Top price:

```text
prices[3] = 2
```

Check:

```text
3 <= 2
```

False.

So no discount for price `2`.

Push current:

```cpp
st.push(4);
```

Stack:

```text
[3,4]
```

---

# End

Final prices:

```text
[4,2,4,2,3]
```

Return:

```text
[4,2,4,2,3]
```

---

# Why While, Not If?

Ye important hai.

At `i = 3`, current price `2` aayi.

Stack:

```text
[1,2]
```

Current `2`:

```text
2 <= 6
```

So index `2` resolve.

Pop.

Ab Stack:

```text
[1]
```

Current `2` again check:

```text
2 <= 4
```

Again true.

So index `1` bhi resolve.

Isliye:

```cpp
while(...)
```

use karte hain.

Agar `if` use karte:

```text
sirf 6 ko discount milta
4 ko nahi milta
```

Answer wrong ho jata.

---

# Monotonic Stack Property

Stack indexes:

```text
[1,2]
```

Corresponding prices:

```text
4,6
```

Increasing order mein hain.

Then current `2` aaya:

```text
2 <= 6
```

6 pop.

Then:

```text
2 <= 4
```

4 pop.

So smaller current value aane par bigger values resolve hoti hain.

Ye stack ko:

```text
Monotonic Increasing Stack
```

jaisa maintain karta hai.

---

# Why Current Price Is the First Valid Discount?

Suppose:

```text
prices = [8, 6, 4, 2]
```

When `2` arrives:

Stack contains unresolved candidates.

Top `4` hai.

Since:

```text
2 <= 4
```

current `2` is the **first** valid price to the right of `4`.

Isliye usko immediately resolve kar dete hain.

Then next candidate `6`:

```text
2 <= 6
```

Again current `2` is its first valid discount among unresolved candidates.

---

# Why Index Store Karna Better Hai?

Stack:

```text
[0,1,2]
```

means:

```text
0 → price 8
1 → price 4
2 → price 6
```

Current index:

```text
3
```

price:

```text
2
```

Then:

```cpp
prices[st.top()]
```

se original price mil jaati hai.

Aur:

```cpp
prices[index] -= prices[i];
```

se directly final answer update kar sakte hain.

---

# Common Mistake

### Mistake 1 — Value push karna

Wrong:

```cpp
st.push(prices[i]);
```

Better:

```cpp
st.push(i);
```

Because later hume actual array index par discount apply karna hai.

---

# Common Mistake 2 — `<` Instead of `<=`

Wrong:

```cpp
prices[i] < prices[st.top()]
```

Correct:

```cpp
prices[i] <= prices[st.top()]
```

Because problem mein **equal price bhi valid discount** hai.

Example:

```text
prices = [5,5]
```

Second `5` first `5` ka valid discount hai.

Final:

```text
[0,5]
```

---

# Common Mistake 3 — Discount Wrong Direction

Wrong:

```cpp
prices[i] -= prices[index];
```

Correct:

```cpp
prices[index] -= prices[i];
```

Current price:

```text
discount
```

hoti hai.

Old item's price:

```text
old price - discount
```

---

# Common Mistake 4 — `if` Instead of `while`

Wrong:

```cpp
if (!st.empty() &&
    prices[i] <= prices[st.top()])
```

Correct:

```cpp
while (!st.empty() &&
       prices[i] <= prices[st.top()])
```

Because one current price multiple previous prices ko resolve kar sakti hai.

---

# LC 496 vs LC 1475

## LC 496

Find:

```text
Next Greater
```

Condition:

```text
current > stack.top
```

---

## LC 1475

Find:

```text
Next Smaller or Equal
```

Condition:

```text
current <= stack.top
```

---

# LC 739 vs LC 1475

## LC 739

```text
current > top
→ warmer temperature
→ calculate waiting days
```

## LC 1475

```text
current <= top
→ discount found
→ subtract discount
```

Same Stack idea:

```text
Unresolved indexes
      ↓
Current resolves them
      ↓
Pop
```

---

# Complexity

Let:

```text
n = prices.size()
```

### Time Complexity

```text
O(n)
```

Each index:

```text
push → maximum once
pop  → maximum once
```

So total operations linear hain.

### Space Complexity

```text
O(n)
```

Worst case Stack mein all indexes store ho sakte hain.

Example:

```text
[1,2,3,4,5]
```

---

# Interview Explanation

> I use a monotonic stack of indices to find the first price to the right that is less than or equal to the current price. For each new price, while the current price is less than or equal to the price at the stack top, the current price becomes the discount for that item. I subtract the discount from the original price and pop that index. Then I push the current index for future elements. This gives O(n) time and O(n) space.

---

# One-Line Revision

> **Current price agar Stack top price se chhoti ya equal hai, to current price hi us Stack top ka discount hai — discount apply karo, pop karo, aur repeat karo.**

---

# Pattern #3 Progress

* [x] LC 739 — Daily Temperatures
* [x] LC 496 — Next Greater Element I
* [x] LC 503 — Next Greater Element II
* [x] LC 901 — Online Stock Span
* [x] LC 1475 — Final Prices With a Special Discount in a Shop
* [ ] LC 1673 — Find the Most Competitive Subsequence

**Pattern #3 complete hone ke liye ab sirf 1 question remaining hai.**

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
│   ├── 02-Next-Greater-Element-I.md
│   ├── 03-Next-Greater-Element-II.md
│   ├── 04-Online-Stock-Span.md
│   └── 05-Final-Prices-With-a-Special-Discount-in-a-Shop.md
│
├── 04-Next-Previous-Greater-Smaller/
│
├── 05-Boundary-Contribution/
│
└── 06-Stack-Design/
```
