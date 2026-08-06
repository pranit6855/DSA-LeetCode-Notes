# LeetCode 1456 - Maximum Number of Vowels in a Substring of Given Length

## 📌 Problem

Hume ek string `s` aur integer `k` diya gaya hai.

Hume **exactly `k` length** ki substring find karni hai jisme maximum number of vowels ho.

Vowels:

```text
a, e, i, o, u
```

Return:

```text
Maximum number of vowels in any substring of length k
```

---

# 🔹 Example

```text
s = "abciiidef"
k = 3
```

Possible windows of size `3`:

```text
"abc" → 1 vowel
"bci" → 1 vowel
"cii" → 2 vowels
"iii" → 3 vowels ✅
"iid" → 2 vowels
"ide" → 2 vowels
"def" → 1 vowel
```

Maximum:

```text
3
```

So:

```text
Output = 3
```

---

# 🧠 Approach

Ye **Fixed Size Sliding Window** problem hai.

Kyunki question already bol raha hai:

```text
substring of length k
```

Matlab window ka size hamesha fixed rahega:

```text
k
```

Hum use karenge:

```text
low
high
count
max_count
```

---

# 🔥 Main Idea

Pehle first `k` characters me vowels count karenge.

Uske baad window ko ek-ek position aage slide karenge.

Har slide me:

```text
1 character window se BAHAR jayega
1 character window ke ANDAR aayega
```

Agar outgoing character vowel hai:

```text
count--
```

Agar incoming character vowel hai:

```text
count++
```

Phir:

```text
max_count
```

update karenge.

---

# 🔧 Helper Function

Vowel baar-baar check karna hai.

Agar har jagah ye likhen:

```cpp
ch=='a' || ch=='e' || ch=='i' || ch=='o' || ch=='u'
```

to code unnecessarily long ho jayega.

Isliye helper function:

```cpp
int isvowel(char ch){

    if(ch=='a' || ch=='e' || ch=='i' || ch=='o' || ch=='u'){
        return 1;
    }

    return 0;
}
```

Agar:

```text
ch = 'a'
```

then:

```text
return 1
```

Agar:

```text
ch = 'b'
```

then:

```text
return 0
```

So hum easily likh sakte hain:

```cpp
if(isvowel(s[i])){
    count++;
}
```

---

# ⚠️ Why Return `0`, Not `-1`?

Ye important C++ concept hai.

`if` condition me:

```text
0 → false
non-zero → true
```

Matlab:

```text
1  → true
-1 → true
5  → true
0  → false
```

Agar consonant ke liye:

```cpp
return -1;
```

karte, to:

```cpp
if(isvowel('b'))
```

bhi true ho jata ❌

Isliye:

```text
Vowel     → return 1
Consonant → return 0
```

---

# 🔥 Initial Pointers

```cpp
int low=0;
int high=k-1;
```

Suppose:

```text
s = "abciiidef"
k = 3
```

Then:

```text
low = 0
high = 2
```

First window:

```text
[a b c] i i i d e f
 ↑   ↑
low high
```

Window size:

```text
high-low+1

= 2-0+1

= 3
```

Exactly `k`.

---

# 🔥 Step 1 - First Window

Pehle first `k` characters ke vowels count:

```cpp
for(int i=0;i<k;i++){

    if(isvowel(s[i])){
        count++;
    }
}
```

For:

```text
[a b c]
```

Check:

```text
a → vowel     → count = 1
b → consonant → count = 1
c → consonant → count = 1
```

So:

```text
count = 1
```

---

# ❓ First Window Alag Se Kyu?

Fixed sliding window me pehle hume ek complete window banana hota hai.

Window size:

```text
k
```

Isliye first `k` elements ko process karte hain.

Uske baad har next window me complete calculation dobara nahi karte.

Sirf:

```text
outgoing remove
+
incoming add
```

karte hain.

Yehi Sliding Window ka main benefit hai.

---

# 🔥 Step 2 - `max_count`

First window calculate hone ke baad:

```cpp
int max_count=count;
```

Kyun?

Because first window bhi possible answer hai.

Example:

```text
s = "aeibcd"
k = 3
```

First window:

```text
"aei"
```

Vowels:

```text
3
```

Ho sakta hai yehi maximum answer ho.

Agar first window ko `max_count` me store nahi kiya to hum ise miss kar sakte hain.

---

# 🔥 Step 3 - Window Slide

Code:

```cpp
while(high<n-1)
```

Jab tak next window possible hai tab tak slide karenge.

Inside:

```cpp
low++;
high++;
```

Example:

Before:

```text
[a b c] i i i d e f
 ↑   ↑
low high
```

After:

```text
a [b c i] i i d e f
   ↑   ↑
  low high
```

Window size ab bhi:

```text
3
```

hai.

Isliye ye:

```text
FIXED SIZE WINDOW
```

hai.

---

# 🔥 Step 4 - Outgoing Character

Old window:

```text
[a b c]
```

New window:

```text
[b c i]
```

Character:

```text
a
```

window se bahar gaya.

Pointers increment karne ke baad:

```text
low = 1
```

Old character ka index:

```text
low - 1
= 0
```

Isliye outgoing character:

```cpp
s[low-1]
```

hai.

Check:

```cpp
if(isvowel(s[low-1])){
    count--;
}
```

Agar outgoing character vowel tha to vowel count decrease.

Example:

```text
a
```

vowel tha:

```text
count = 1
```

Remove:

```text
count = 0
```

---

# 🔥 Step 5 - Incoming Character

New character:

```text
i
```

window me enter hua.

New `high` usi character ko point karta hai.

So:

```cpp
if(isvowel(s[high])){
    count++;
}
```

`i` vowel hai:

```text
count = 0 + 1
      = 1
```

New window:

```text
"bci"
```

Actually bhi:

```text
b ❌
c ❌
i ✅
```

Total:

```text
1 vowel
```

Correct.

---

# 🔥 Step 6 - Maximum Update

Current window ke vowels:

```text
count
```

Previous maximum:

```text
max_count
```

Compare:

```cpp
max_count=max(count,max_count);
```

Example:

```text
count = 2
max_count = 1
```

Then:

```text
max_count = 2
```

---

# 🔄 Complete Dry Run

Given:

```text
s = "abciiidef"
k = 3
```

## Window 1

```text
[abc]
```

Vowels:

```text
a
```

So:

```text
count = 1
max_count = 1
```

---

## Slide to Window 2

```text
a [bci]
```

Outgoing:

```text
a → vowel
```

So:

```text
count--
1 → 0
```

Incoming:

```text
i → vowel
```

So:

```text
count++
0 → 1
```

Current:

```text
"bci" → 1 vowel
```

Maximum:

```text
1
```

---

## Slide to Window 3

```text
a b [cii]
```

Outgoing:

```text
b
```

Not vowel.

So:

```text
count = 1
```

Incoming:

```text
i
```

Vowel.

So:

```text
count = 2
```

Current:

```text
"cii"
```

Vowels:

```text
2
```

Update:

```text
max_count = 2
```

---

## Slide to Window 4

```text
a b c [iii]
```

Outgoing:

```text
c
```

Not vowel.

Incoming:

```text
i
```

Vowel.

So:

```text
count = 3
```

Current:

```text
"iii"
```

Update:

```text
max_count = 3
```

---

## Remaining Windows

```text
"iid" → 2
"ide" → 2
"def" → 1
```

Maximum remains:

```text
3
```

So final answer:

```text
3
```

---

# 💻 C++ Code

```cpp
class Solution {
public:

    int isvowel(char ch){

        if(ch=='a' || ch=='e' || ch=='i' || ch=='o' || ch=='u'){
            return 1;
        }

        return 0;
    }

    int maxVowels(string s, int k) {

        int n=s.size();
        int low=0;
        int high=k-1;
        int count=0;

        // First window
        for(int i=0;i<k;i++){

            if(isvowel(s[i])){
                count++;
            }
        }

        int max_count=count;

        // Slide window
        while(high<n-1){

            low++;
            high++;

            // Outgoing character
            if(isvowel(s[low-1])){
                count--;
            }

            // Incoming character
            if(isvowel(s[high])){
                count++;
            }

            max_count=max(count,max_count);
        }

        return max_count;
    }
};
```

---

# 🧠 Code Line By Line

### Helper Function

```cpp
int isvowel(char ch)
```

Check karta hai character vowel hai ya nahi.

Returns:

```text
1 → vowel
0 → not vowel
```

---

### String Size

```cpp
int n=s.size();
```

String ki total length.

---

### Left Pointer

```cpp
int low=0;
```

Window ka left boundary.

---

### Right Pointer

```cpp
int high=k-1;
```

First fixed window ka right boundary.

---

### Vowel Count

```cpp
int count=0;
```

Current window me kitne vowels hain.

---

### First Window

```cpp
for(int i=0;i<k;i++){
    if(isvowel(s[i])){
        count++;
    }
}
```

First `k` characters me vowels count.

---

### Store First Answer

```cpp
int max_count=count;
```

First window ko bhi answer me include karna hai.

---

### Move Window

```cpp
low++;
high++;
```

Dono pointers ek step move.

Window ka size same rehta hai.

---

### Remove Outgoing Vowel

```cpp
if(isvowel(s[low-1])){
    count--;
}
```

Purana character vowel tha to count decrease.

---

### Add Incoming Vowel

```cpp
if(isvowel(s[high])){
    count++;
}
```

Naya character vowel hai to count increase.

---

### Maximum Update

```cpp
max_count=max(count,max_count);
```

Maximum vowels store.

---

# 🔥 Connection With LC 643

LC 643 me humne kiya:

```cpp
sum=sum-nums[low-1]+nums[high];
```

Meaning:

```text
old number remove
+
new number add
```

LC 1456 me:

```cpp
if(isvowel(s[low-1])){
    count--;
}

if(isvowel(s[high])){
    count++;
}
```

Meaning:

```text
old vowel remove
+
new vowel add
```

Concept exactly same hai.

---

# ⭐ Fixed Sliding Window Pattern

```text
First K elements process karo
          ↓
answer initialize karo
          ↓
low++, high++
          ↓
outgoing element remove
          ↓
incoming element add
          ↓
answer update
          ↓
repeat
```

---

# ⏱️ Time Complexity

First window:

```text
O(k)
```

Remaining string sliding:

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

Hum sirf variables use kar rahe hain:

```text
low
high
count
max_count
```

Koi extra array/map nahi.

So:

```text
Space Complexity = O(1)
```

---

# 📊 Complexity

```text
Time Complexity  → O(n)

Space Complexity → O(1)
```

---

# ⚠️ Common Mistakes

### Mistake 1

```cpp
ch='i'
```

Ye assignment hai ❌

Correct:

```cpp
ch=='i'
```

Comparison ke liye `==`.

---

### Mistake 2

```cpp
return -1;
```

Consonant ke liye wrong hai agar helper ko directly `if` me use kar rahe hain.

Because C++ me:

```text
-1 → true
```

Correct:

```cpp
return 0;
```

---

### Mistake 3

First window ke baad:

```cpp
int max_count=0;
```

karna unnecessary/risky pattern hai.

Better:

```cpp
int max_count=count;
```

because first window bhi answer ho sakti hai.

---

# 🔥 Quick Revision

```text
low = 0
high = k-1
      ↓
First k chars ke vowels count
      ↓
max_count = count
      ↓
low++, high++
      ↓
s[low-1] vowel?
      ↓
count--
      ↓
s[high] vowel?
      ↓
count++
      ↓
max_count update
      ↓
repeat
```

---

# 🧠 One-Line Revision

```text
First k characters ke vowels count karo, phir har slide me outgoing vowel ko minus aur incoming vowel ko plus karke maximum count maintain karo.
```

---

# ⭐ Most Important Part

```cpp
low++;
high++;

if(isvowel(s[low-1])){
    count--;
}

if(isvowel(s[high])){
    count++;
}

max_count=max(count,max_count);
```

Yehi is question ka main **Fixed Size Sliding Window** logic hai.
