# LeetCode 125 - Valid Palindrome

## 📌 Problem

Hume ek string `s` di gayi hai.

Hume check karna hai ki string **palindrome** hai ya nahi.

Palindrome ka matlab:

```text
Jo string left se right aur right se left same read ho.
```

Example:

```text
"madam"   → Palindrome ✅
"racecar" → Palindrome ✅
"hello"   → Palindrome ❌
```

Lekin is question me kuch extra conditions hain:

- Uppercase aur lowercase ko same consider karna hai.
- Spaces ignore karne hain.
- Special characters ignore karne hain.
- Sirf letters aur numbers consider karne hain.

---

# 🔹 Example

Input:

```text
"A man, a plan, a canal: Panama"
```

Agar spaces aur special characters remove kare aur sabko lowercase kare:

```text
"amanaplanacanalpanama"
```

Ab ye string left aur right dono taraf se same hai.

So:

```text
Output = true
```

---

# 💡 Approach

Hum problem ko **2 simple steps** me solve karenge.

```text
Step 1:
Original string ko clean karenge

Step 2:
Clean string par Two Pointers laga kar palindrome check karenge
```

---

# 🧹 Step 1 - String Ko Clean Karna

Hum ek new string banayenge:

```cpp
string temp = "";
```

`temp` ke andar sirf:

```text
letters
+
numbers
```

store karenge.

Aur saare letters ko lowercase me convert kar denge.

---

# 🔹 Original String

Example:

```text
"A man, a plan, a canal: Panama"
```

Hum har character ko check karenge:

```cpp
for(int i=0; i<s.size(); i++)
```

---

# 🔍 `isalnum()` Kya Karta Hai?

Hum condition use karte hain:

```cpp
if(isalnum(s[i]))
```

`isalnum()` ka meaning hai:

```text
is alphanumeric?
```

Alphanumeric matlab:

```text
Alphabet + Numeric
```

Yaani:

```text
A-Z
a-z
0-9
```

---

## Examples

```text
isalnum('A') → true

isalnum('a') → true

isalnum('7') → true
```

Lekin:

```text
isalnum(' ') → false

isalnum(',') → false

isalnum(':') → false

isalnum('@') → false
```

So spaces aur special characters automatically ignore ho jayenge.

---

# 🔡 `tolower()` Kyu Use Kiya?

Agar character alphanumeric hai, hum likhte hain:

```cpp
temp += tolower(s[i]);
```

`tolower()` uppercase alphabet ko lowercase me convert karta hai.

Example:

```text
'A' → 'a'

'P' → 'p'

'M' → 'm'
```

Agar already lowercase hai:

```text
'a' → 'a'
```

same rahega.

Numbers bhi same rahenge.

---

# ❓ Lowercase Karna Important Kyu Hai?

Suppose string hai:

```text
"RaceCar"
```

Agar directly compare kare:

```text
'R' != 'r'
```

C++ ke according uppercase `R` aur lowercase `r` different characters hain.

Lekin palindrome question me hume uppercase/lowercase ignore karna hai.

Isliye pehle sabko lowercase bana dete hain:

```text
"RaceCar"

     ↓

"racecar"
```

Ab comparison simple ho gaya.

---

# 🔄 Cleaning Example

Original:

```text
"A man, a plan, a canal: Panama"
```

Characters ko one-by-one check karenge.

---

### `'A'`

```text
isalnum('A') → true
```

Lowercase:

```text
'a'
```

So:

```text
temp = "a"
```

---

### `' '`

Space hai.

```text
isalnum(' ') → false
```

Ignore.

```text
temp = "a"
```

---

### `'m'`

```text
isalnum('m') → true
```

So:

```text
temp = "am"
```

---

### `'a'`

```text
temp = "ama"
```

---

### `'n'`

```text
temp = "aman"
```

Aise poori string process hogi.

Comma:

```text
,
```

ignore hoga.

Colon:

```text
:
```

ignore hoga.

Spaces bhi ignore honge.

Finally:

```text
temp = "amanaplanacanalpanama"
```

Ab humare paas clean lowercase string hai.

---

# 🔥 Step 2 - Two Pointer Palindrome Check

Ab palindrome check karna bahut simple hai.

Hum do pointers lenge:

```cpp
int i = 0;
int j = temp.size() - 1;
```

Meaning:

```text
i → first character

j → last character
```

Example:

```text
temp = "racecar"

        r a c e c a r
        ↑           ↑
        i           j
```

---

# 🔍 Main Condition

Hum loop chalate hain:

```cpp
while(i < j)
```

Jab tak `i` aur `j` ek dusre ko cross nahi karte, characters compare karenge.

---

# 🔹 Characters Compare Karna

```cpp
if(temp[i] != temp[j])
```

Agar left aur right characters different hain:

```text
Palindrome nahi hai
```

So immediately:

```cpp
return false;
```

---

# 🔹 Characters Same Hain

Agar:

```text
temp[i] == temp[j]
```

to ye pair correct hai.

Ab dono pointers ko andar move karenge:

```cpp
i++;
j--;
```

---

# 🔄 Dry Run - `"racecar"`

Clean string:

```text
racecar
```

Starting:

```text
i = 0
j = 6
```

---

## Step 1

```text
r a c e c a r
↑           ↑
i           j
```

Compare:

```text
'r' == 'r'
```

True ✅

Move:

```cpp
i++;
j--;
```

Now:

```text
i = 1
j = 5
```

---

## Step 2

```text
r a c e c a r
  ↑       ↑
  i       j
```

Compare:

```text
'a' == 'a'
```

True ✅

Move:

```text
i = 2
j = 4
```

---

## Step 3

```text
r a c e c a r
    ↑   ↑
    i   j
```

Compare:

```text
'c' == 'c'
```

True ✅

Move:

```text
i = 3
j = 3
```

---

# 🛑 Loop Stop

Condition:

```cpp
while(i < j)
```

Now:

```text
i = 3
j = 3
```

Check:

```text
3 < 3 → false
```

Loop stop.

Koi mismatch nahi mila.

So:

```cpp
return true;
```

`"racecar"` palindrome hai.

---

# ❌ Non-Palindrome Example

Consider:

```text
temp = "hello"
```

Starting:

```text
h e l l o
↑       ↑
i       j
```

Compare:

```text
'h' != 'o'
```

Different hain.

So:

```cpp
return false;
```

Hume remaining string check karne ki zarurat hi nahi hai.

Ek bhi mismatch mil gaya matlab string palindrome nahi hai.

---

# 💻 C++ Code

```cpp
class Solution {
public:
    bool isPalindrome(string s) {

        string temp = "";

        // Step 1: Clean string + lowercase
        for(int i=0; i<s.size(); i++) {

            if(isalnum(s[i])) {
                temp += tolower(s[i]);
            }
        }

        // Step 2: Two pointers
        int i = 0;
        int j = temp.size() - 1;

        while(i < j) {

            if(temp[i] != temp[j]) {
                return false;
            }

            i++;
            j--;
        }

        return true;
    }
};
```

---

# 🧠 Code Explanation

## 1. Temporary String

```cpp
string temp = "";
```

Ek empty string banayi.

Isme cleaned string store hogi.

---

## 2. Original String Traverse Karna

```cpp
for(int i=0; i<s.size(); i++)
```

Original string ke har character ko one-by-one check karte hain.

---

## 3. Alphanumeric Check

```cpp
if(isalnum(s[i]))
```

Check karta hai current character:

```text
letter ya number hai?
```

Agar yes:

```text
use karo
```

Agar space/special character hai:

```text
ignore karo
```

---

## 4. Lowercase + Add To Temp

```cpp
temp += tolower(s[i]);
```

Do kaam ek saath ho rahe hain:

```text
1. Character ko lowercase karo

2. temp ke end me add karo
```

Example:

```text
temp = "ama"

current = 'N'
```

`tolower()`:

```text
'N' → 'n'
```

Then:

```text
temp = "aman"
```

---

# 🔹 Cleaning Ke Baad

Original:

```text
"A man, a plan, a canal: Panama"
```

Becomes:

```text
"amanaplanacanalpanama"
```

Ab special characters aur uppercase ki tension khatam.

Sirf palindrome check karna hai.

---

# 🧠 Two Pointers

```cpp
int i = 0;
int j = temp.size() - 1;
```

Meaning:

```text
i = left pointer

j = right pointer
```

Visual:

```text
a m a n a p l a n a c a n a l p a n a m a
↑                                         ↑
i                                         j
```

---

# 🔹 While Loop

```cpp
while(i < j)
```

Jab tak pointers meet/cross nahi karte, comparison chalta rahega.

---

# 🔹 Mismatch

```cpp
if(temp[i] != temp[j]) {
    return false;
}
```

Agar kisi bhi position par characters different mile:

```text
Palindrome impossible
```

So immediately:

```text
false
```

return.

---

# 🔹 Pointer Movement

Agar characters same hain:

```cpp
i++;
j--;
```

Left pointer right ki taraf:

```text
→
```

Right pointer left ki taraf:

```text
←
```

Dono andar ki taraf move karte hain.

---

# ❓ `while(i < j)` Kyu?

Palindrome me hume pairs compare karne hain.

Example:

```text
racecar
```

Pairs:

```text
r ↔ r
a ↔ a
c ↔ c
```

Middle:

```text
e
```

ko khud se compare karne ki zarurat nahi.

Jab:

```text
i == j
```

matlab middle character aa gaya.

Isliye:

```cpp
while(i < j)
```

enough hai.

---

# ❓ `return false` Loop Ke Andar Aur `return true` Bahar Kyu?

Mismatch kabhi bhi mil sakta hai.

Isliye:

```cpp
if(temp[i] != temp[j]) {
    return false;
}
```

immediately return kar sakte hain.

Lekin `true` tabhi bol sakte hain jab **saare pairs successfully check ho jayein**.

Isliye:

```cpp
return true;
```

loop ke bahar hai.

---

# ⏱️ Time Complexity

## Cleaning

Original string ko ek baar traverse karte hain:

```text
O(n)
```

## Palindrome Check

Clean string ko Two Pointer se maximum ek baar traverse karte hain:

```text
O(n)
```

Total:

```text
O(n) + O(n)
```

Big-O me:

```text
O(n)
```

---

# 💾 Space Complexity

Hum extra string bana rahe hain:

```cpp
string temp = "";
```

Worst case me original string ke saare characters alphanumeric ho sakte hain.

Example:

```text
"abcdef12345"
```

Then `temp` ka size bhi approximately `n` hoga.

So:

```text
Space Complexity = O(n)
```

---

# ⭐ Important Functions

## `isalnum()`

```cpp
isalnum(s[i])
```

Check:

```text
Alphabet or Number?
```

Examples:

```text
'A' → true
'z' → true
'5' → true

' ' → false
',' → false
'@' → false
':' → false
```

---

## `tolower()`

```cpp
tolower(s[i])
```

Uppercase alphabet ko lowercase me convert karta hai.

```text
'A' → 'a'
'B' → 'b'
'Z' → 'z'
```

---

# 🔥 Quick Revision

```text
Original String

"A man, a plan, a canal: Panama"

             ↓

      Traverse String

             ↓

       isalnum() ?

       /         \
     YES          NO
      ↓            ↓
 lowercase       ignore
      ↓
 add to temp

             ↓

temp = "amanaplanacanalpanama"

             ↓

       Two Pointers

i = 0                j = n-1
 ↓                     ↓

a m a n ......... m a
↑                     ↑

             ↓

       temp[i] == temp[j] ?

          /          \
        YES           NO
         ↓             ↓
       i++           false
       j--

             ↓

       pointers meet

             ↓

           true
```

---

# 🎯 Main Pattern To Remember

Is solution ko 2 parts me yaad rakho:

```text
PART 1 → CLEAN

isalnum()
    ↓
tolower()
    ↓
temp me add
```

Then:

```text
PART 2 → CHECK

i = 0
j = temp.size()-1

while(i < j)

temp[i] != temp[j]
→ false

otherwise
→ i++
→ j--
```

Short form:

```text
Clean String → Lowercase → Two Pointers → Compare Both Ends
```

---

# 📊 Complexity Summary

```text
Time Complexity  → O(n)

Space Complexity → O(n)
```

Extra space `O(n)` isliye hai kyunki hum cleaned string:

```cpp
string temp
```

bana rahe hain.
