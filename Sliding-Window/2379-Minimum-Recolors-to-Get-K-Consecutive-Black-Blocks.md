# LeetCode 2379 - Minimum Recolors to Get K Consecutive Black Blocks

## 📌 Problem

Hume ek string `blocks` aur integer `k` diya gaya hai.

String me sirf do characters hain:

```text
'W' → White Block
'B' → Black Block
```

Ek operation me hum:

```text
W → B
```

kar sakte hain.

Hume **minimum number of recolors** find karne hain jisse kahin bhi `k` consecutive Black blocks mil jayein.

---

# 🔹 Example

```text
blocks = "WBBWWBBWBW"
k = 7
```

Hume exactly `7` consecutive positions ki window dekhni hai.

Main observation:

```text
Window me jitne W
=
utne recolors required
```

Kyunki `B` already black hai.

Sirf `W` ko `B` banana padega.

Isliye hume:

```text
Size k ki window me minimum number of W
```

find karne hain.

---

# 🧠 Main Idea

Suppose:

```text
blocks = "WBBWB"
k = 3
```

Possible windows:

```text
WBB → 1 W → 1 recolor
BBW → 1 W → 1 recolor
BWB → 1 W → 1 recolor
```

Minimum:

```text
1
```

So answer:

```text
1
```

---

# 🔥 Approach

Ye **Fixed Size Sliding Window** problem hai.

Kyunki har window ka size exactly:

```text
k
```

rahega.

Hum maintain karenge:

```text
low
high
count
min_count
```

Where:

```text
count = current window me number of W

min_count = ab tak kisi k-size window me minimum W
```

---

# 🔹 Initial Variables

```cpp
int n=blocks.size();
int low=0;
int high=k-1;
int count=0;
```

Meaning:

```text
n     → string size

low   → window ka left index

high  → window ka right index

count → current window me W ki count
```

---

# ❓ `high = k-1` Kyu?

Window size formula:

```text
high - low + 1
```

Initially:

```text
low = 0
high = k-1
```

So:

```text
(k-1) - 0 + 1

= k
```

Therefore first window exactly `k` size ki hai.

Example:

```text
k = 3
```

Then:

```text
low = 0
high = 2
```

Window:

```text
[W B B] W B
 ↑   ↑
low high
```

Size:

```text
2 - 0 + 1 = 3
```

---

# 🔥 Step 1 - First Window Me W Count

```cpp
for(int i=0;i<k;i++){

    if(blocks[i]=='W'){
        count++;
    }
}
```

Example:

```text
Window = "WBB"
```

Check:

```text
W → count = 1
B → no change
B → no change
```

So:

```text
count = 1
```

Meaning:

```text
"WBB"
```

ko:

```text
"BBB"
```

banane ke liye exactly:

```text
1 recolor
```

required hai.

---

# 🔥 Step 2 - First Window Ko Answer Me Include Karo

```cpp
int min_count=count;
```

First window bhi final answer ho sakti hai.

Isliye uski `W` count ko initial minimum maan lete hain.

Example:

```text
count = 1
```

Then:

```text
min_count = 1
```

---

# ❓ `min_count = count` Kyu, `0` Kyu Nahi?

Agar:

```cpp
int min_count=0;
```

rakh diya to:

```cpp
min(count,0)
```

hamesha `0` hi de sakta hai jab count positive ho.

Example:

```text
count = 2

min(2,0) = 0 ❌
```

Jabki ho sakta hai answer actually `2` ho.

Isliye first valid window se initialize karte hain:

```cpp
int min_count=count;
```

---

# 🔥 Step 3 - Window Slide

```cpp
while(high<n-1)
```

Jab tak next window possible hai, slide karte rahenge.

Window ko right move karne ke liye:

```cpp
low++;
high++;
```

Example:

Before:

```text
[W B B] W B
 ↑   ↑
low high
```

After:

```text
W [B B W] B
   ↑   ↑
  low high
```

Notice:

```text
low  : 0 → 1
high : 2 → 3
```

Dono:

```cpp
++
```

hue.

---

# ⚠️ `high--` Kyu Galat Hai?

Wrong:

```cpp
low++;
high--;
```

Suppose:

```text
low = 0
high = 2
```

Then:

```text
low  = 1
high = 1
```

Window size:

```text
1 - 1 + 1 = 1
```

But hume size:

```text
k = 3
```

maintain karna tha.

Isliye fixed window ko right slide karte waqt:

```cpp
low++;
high++;
```

hoga.

---

# 🔥 Step 4 - Outgoing Character Remove

Window slide hone ke baad ek old character bahar gaya.

Example:

```text
[W B B] W
```

to:

```text
W [B B W]
```

Outgoing:

```text
W
```

Pointers increment karne ke baad outgoing character ka index:

```text
low - 1
```

So:

```cpp
if(blocks[low-1]=='W'){
    count--;
}
```

Agar outgoing character `W` tha, current window me ek `W` kam ho gaya.

---

# 🔥 Step 5 - Incoming Character Add

Window me ek new character enter karta hai.

New `high` incoming character ko point karta hai.

So:

```cpp
if(blocks[high]=='W'){
    count++;
}
```

Agar incoming character `W` hai:

```text
count++
```

---

# 🔄 Example of Sliding

Current window:

```text
[W B B]
```

Current:

```text
count = 1
```

Next window:

```text
W [B B W]
```

Outgoing:

```text
W
```

So:

```text
count--

1 → 0
```

Incoming:

```text
W
```

So:

```text
count++

0 → 1
```

New window:

```text
BBW
```

Actually bhi:

```text
1 W
```

Correct.

---

# 🔥 Step 6 - Minimum Update

Har new window ke baad:

```cpp
min_count=min(count,min_count);
```

Example:

```text
previous min_count = 2
current count = 1
```

Then:

```text
min_count = min(1,2)

          = 1
```

Matlab ab tak hume ek aisi window mili hai jisme sirf:

```text
1 W
```

hai.

So sirf `1` recolor required hai.

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int minimumRecolors(string blocks, int k) {

        int n=blocks.size();
        int low=0;
        int high=k-1;
        int count=0;

        // First window
        for(int i=0;i<k;i++){

            if(blocks[i]=='W'){
                count++;
            }
        }

        int min_count=count;

        // Remaining windows
        while(high<n-1){

            low++;
            high++;

            // Outgoing character
            if(blocks[low-1]=='W'){
                count--;
            }

            // Incoming character
            if(blocks[high]=='W'){
                count++;
            }

            min_count=min(count,min_count);
        }

        return min_count;
    }
};
```

---

# 🧠 Code Line-by-Line

## String Size

```cpp
int n=blocks.size();
```

Total blocks.

---

## Left Pointer

```cpp
int low=0;
```

Window ka starting index.

---

## Right Pointer

```cpp
int high=k-1;
```

Window ka ending index.

---

## Current White Count

```cpp
int count=0;
```

Current `k` size window me kitne `W` hain.

Yehi current window ke:

```text
required recolors
```

hain.

---

## First Window

```cpp
for(int i=0;i<k;i++){

    if(blocks[i]=='W'){
        count++;
    }
}
```

First `k` characters me `W` count.

---

## Initial Minimum

```cpp
int min_count=count;
```

First window ko bhi answer me include kiya.

---

## Move Window

```cpp
low++;
high++;
```

Window ko ek position right slide kiya.

---

## Remove Outgoing W

```cpp
if(blocks[low-1]=='W'){
    count--;
}
```

Outgoing block White tha to current White count decrease.

---

## Add Incoming W

```cpp
if(blocks[high]=='W'){
    count++;
}
```

Incoming block White hai to count increase.

---

## Minimum Update

```cpp
min_count=min(count,min_count);
```

Minimum number of recolors store.

---

## Final Answer

```cpp
return min_count;
```

Minimum recolors return.

---

# 🔥 LC 1456 Se Connection

LC 1456 me:

```text
Size k ki window me maximum vowels
```

Hum kar rahe the:

```cpp
if(outgoing is vowel){
    count--;
}

if(incoming is vowel){
    count++;
}

max_count=max(count,max_count);
```

LC 2379 me:

```text
Size k ki window me minimum W
```

Hum kar rahe hain:

```cpp
if(outgoing == 'W'){
    count--;
}

if(incoming == 'W'){
    count++;
}

min_count=min(count,min_count);
```

Almost exact same pattern.

Difference:

```text
LC 1456 → MAX vowels

LC 2379 → MIN white blocks
```

---

# 🔥 LC 643, 1456, 1343, 2379 Pattern

### LC 643

```text
Fixed k
→ sum maintain
→ maximum average
```

### LC 1456

```text
Fixed k
→ vowel count maintain
→ maximum count
```

### LC 1343

```text
Fixed k
→ sum maintain
→ condition satisfy karne wali windows count
```

### LC 2379

```text
Fixed k
→ W count maintain
→ minimum count
```

In sabka sliding concept same hai:

```text
FIRST WINDOW
     ↓
OLD REMOVE
     ↓
NEW ADD
     ↓
ANSWER UPDATE
```

---

# ⏱️ Time Complexity

First window:

```text
O(k)
```

Remaining windows:

```text
O(n-k)
```

Total:

```text
O(k + n-k)

= O(n)
```

So:

```text
Time Complexity = O(n)
```

---

# 💾 Space Complexity

Sirf variables use ho rahe hain:

```text
low
high
count
min_count
```

No extra array/map.

So:

```text
Space Complexity = O(1)
```

---

# ⚠️ Common Mistakes

## 1. Lowercase `w`

Wrong:

```cpp
blocks[i]=='w'
```

Problem me uppercase characters hain.

Correct:

```cpp
blocks[i]=='W'
```

C++ case-sensitive hai:

```text
'w' != 'W'
```

---

## 2. `high--`

Wrong:

```cpp
low++;
high--;
```

Correct:

```cpp
low++;
high++;
```

Fixed window ko right slide karne ke liye dono pointers right move karte hain.

---

## 3. `min_count = 0`

Wrong:

```cpp
int min_count=0;
```

Correct:

```cpp
int min_count=count;
```

First window ko initial answer banao.

---

# 🔥 Quick Revision

```text
low = 0
high = k-1
      ↓
First k blocks me W count
      ↓
min_count = count
      ↓
low++, high++
      ↓
Outgoing W?
YES → count--
      ↓
Incoming W?
YES → count++
      ↓
min_count update
      ↓
repeat
```

---

# 🧠 One-Line Revision

```text
Har k-size window me W ki count maintain karo; outgoing W ko minus aur incoming W ko plus karo, aur sab windows me minimum W count return karo.
```

---

# ⭐ Most Important Code

```cpp
low++;
high++;

if(blocks[low-1]=='W'){
    count--;
}

if(blocks[high]=='W'){
    count++;
}

min_count=min(count,min_count);
```

Because:

```text
Number of W in window
=
Number of recolors required
```
