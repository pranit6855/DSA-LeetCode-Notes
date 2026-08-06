# LeetCode 904 - Fruit Into Baskets

## 📌 Problem

Hume ek integer array `fruits` diya gaya hai.

Example:

```text
fruits = [1,2,1]
```

Har number ek **fruit type** represent karta hai.

Humare paas sirf:

```text
2 baskets
```

hain.

Har basket me sirf **ek type ka fruit** aa sakta hai.

Iska simple meaning:

```text
Hum maximum 2 different fruit types rakh sakte hain.
```

Hume longest continuous subarray find karna hai jisme:

```text
At Most 2 Unique Fruit Types
```

hon.

Return karna hai:

```text
Maximum number of fruits
```

---

# 🔹 Example 1

```text
fruits = [1,2,1]
```

Unique fruit types:

```text
1, 2
```

Sirf 2 types hain.

Isliye poora array valid hai:

```text
[1,2,1]
```

Length:

```text
3
```

Output:

```text
3
```

---

# 🔹 Example 2

```text
fruits = [1,2,3,2,2]
```

Poora array:

```text
[1,2,3,2,2]
```

Unique types:

```text
1, 2, 3
```

Total:

```text
3 types ❌
```

Hum maximum 2 types hi rakh sakte hain.

Longest valid part:

```text
[2,3,2,2]
```

Unique types:

```text
2, 3
```

Length:

```text
4
```

So:

```text
Output = 4
```

---

# 🧠 Approach

Ye problem:

```text
Variable Size Sliding Window
+
Frequency Map
```

ka question hai.

Hum use karenge:

```text
low
high
unordered_map
```

### `high`

Window ko expand karega:

```text
high++ → EXPAND
```

### `low`

Window ko shrink karega:

```text
low++ → SHRINK
```

### `unordered_map`

Current window me har fruit type ki frequency store karega.

---

# 🔥 Frequency Map

Hum banayenge:

```cpp
unordered_map<int,int> mp;
```

Suppose current window:

```text
[1,2,1,2]
```

Map:

```text
1 → 2
2 → 2
```

Meaning:

```text
fruit type 1 → 2 fruits
fruit type 2 → 2 fruits
```

And:

```cpp
mp.size()
```

hoga:

```text
2
```

So:

```text
mp.size() = number of unique fruit types
```

---

# ⭐ Main Condition

Humare paas 2 baskets hain.

So maximum:

```text
2 unique fruit types
```

allowed hain.

Valid:

```text
1 type  ✅
2 types ✅
```

Invalid:

```text
3 types ❌
4 types ❌
```

Isliye:

```cpp
while(mp.size()>2)
```

hote hi window shrink karenge.

---

# 🔥 Initial Variables

```cpp
int n=fruits.size();
int low=0;
int high=0;
int max_len=0;

unordered_map<int,int> mp;
```

Meaning:

```text
n       → array ka size

low     → window ka left boundary

high    → window ka right boundary

max_len → longest valid window

mp      → fruit frequencies
```

---

# ❓ `max_len = 0` Kyu?

Yahan hum maximum valid window ki **length** calculate kar rahe hain.

Length ki natural minimum value:

```text
0
```

hoti hai.

Isliye:

```cpp
int max_len=0;
```

Pichle `Longest Substring with K Unique` question me humne:

```cpp
int max_len=-1;
```

liya tha.

Kyunki wahan problem explicitly bolti thi:

```text
Exactly K unique nahi mile
→ return -1
```

Fruit Into Baskets me aisa nahi hai.

Isliye:

```text
max_len = 0
```

---

# 🔥 Step 1 - Fruit Add Karo

Outer loop:

```cpp
while(high<n)
```

Har iteration me `high` wala fruit current window me add karenge:

```cpp
mp[fruits[high]]++;
```

Example:

```text
fruits = [1,2,1]
```

Initially:

```text
high = 0
```

Fruit:

```text
fruits[0] = 1
```

Add:

```cpp
mp[1]++;
```

Map:

```text
1 → 1
```

---

Next fruit `2`:

```text
1 → 1
2 → 1
```

Now:

```text
mp.size() = 2
```

Still valid.

---

Next fruit `1`:

```text
1 → 2
2 → 1
```

Still:

```text
mp.size() = 2
```

Valid.

---

# 🔥 Step 2 - More Than 2 Types?

Condition:

```cpp
while(mp.size()>2)
```

Suppose current map:

```text
1 → 1
2 → 2
3 → 1
```

Then:

```text
mp.size() = 3
```

But allowed:

```text
maximum 2
```

So current window invalid hai.

Ab window ko left side se shrink karenge.

---

# 🔥 Step 3 - Left Fruit Remove

Code:

```cpp
mp[fruits[low]]--;
```

Suppose:

```text
fruits[low] = 1
```

and map:

```text
1 → 2
2 → 2
3 → 1
```

Remove one `1`:

```text
1 → 1
2 → 2
3 → 1
```

Notice:

```text
mp.size() = 3
```

abhi bhi hai.

Kyunki fruit type `1` abhi window me present hai.

Isliye shrink dobara hoga.

---

# 🔥 Frequency 0 Hone Par Erase

Suppose map:

```text
1 → 1
2 → 2
3 → 1
```

Aur `low` par:

```text
1
```

hai.

Decrease:

```cpp
mp[fruits[low]]--;
```

Now:

```text
1 → 0
2 → 2
3 → 1
```

Fruit type `1` ki frequency:

```text
0
```

ho gayi.

Matlab current window me fruit `1` ab exist nahi karta.

Isliye:

```cpp
if(mp[fruits[low]]==0){
    mp.erase(fruits[low]);
}
```

Map becomes:

```text
2 → 2
3 → 1
```

Now:

```text
mp.size() = 2
```

Window valid again.

---

# ⚠️ Important Common Mistake

Galat:

```cpp
if(mp.size()==0){
    mp.erase(fruits[low]);
}
```

Ye map ka total size check kar raha hai.

Hume ye nahi dekhna ki:

```text
map empty hua ya nahi
```

Hume dekhna hai:

```text
current fruit ki frequency 0 hui ya nahi
```

Correct:

```cpp
if(mp[fruits[low]]==0){
    mp.erase(fruits[low]);
}
```

### Rule

```cpp
mp[element]--;

if(mp[element]==0){
    mp.erase(element);
}
```

---

# 🔥 Step 4 - `low++`

Fruit remove karne ke baad:

```cpp
low++;
```

Matlab window ka left boundary right side move karega.

```text
low++ → SHRINK
```

---

# 🔥 Step 5 - Current Window Length

Jab:

```cpp
while(mp.size()>2)
```

finish ho gaya, iska matlab:

```text
mp.size() <= 2
```

guaranteed hai.

So current window valid hai.

Length:

```cpp
int res=high-low+1;
```

---

# 🔥 Step 6 - Maximum Update

```cpp
max_len=max(res,max_len);
```

Example:

```text
previous max_len = 3

current res = 4
```

Then:

```text
max(4,3) = 4
```

So:

```text
max_len = 4
```

---

# ❓ Yahan `if(mp.size()==2)` Kyu Nahi?

Ye is question ka **sabse important concept** hai.

Pichle question me:

```text
Longest Substring with K Unique
```

condition thi:

```text
EXACTLY K unique
```

Suppose:

```text
k = 3
```

Then:

```text
1 unique ❌
2 unique ❌
3 unique ✅
4 unique ❌
```

Isliye wahan:

```cpp
if(mp.size()==k){
    // calculate answer
}
```

zaroori tha.

---

# 🍎 Fruit Into Baskets Me Difference

Yahan condition hai:

```text
AT MOST 2 unique fruit types
```

Meaning:

```text
1 unique ✅
2 unique ✅
3 unique ❌
```

So both:

```text
mp.size() == 1
```

and:

```text
mp.size() == 2
```

valid hain.

Isliye:

```cpp
if(mp.size()==2)
```

nahi lagayenge.

---

# ⭐ Exactly K vs At Most K

## Exactly K

Valid only when:

```text
size == k
```

Code:

```cpp
while(mp.size()>k){
    // shrink
}

if(mp.size()==k){
    // calculate answer
}
```

---

## At Most K

Valid when:

```text
size <= k
```

Hum already:

```cpp
while(mp.size()>k)
```

se saare invalid cases remove kar chuke hain.

Jab `while` khatam:

```text
size <= k
```

automatically guaranteed hai.

So direct:

```cpp
int res=high-low+1;
max_len=max(res,max_len);
```

---

# 🔥 Example - Why `if(size==2)` Would Be Wrong

Consider:

```text
fruits = [1,1,1]
```

Current map:

```text
1 → 3
```

So:

```text
mp.size() = 1
```

Kya ye valid hai?

Yes ✅

Humare paas 2 baskets hain, iska matlab ye nahi ki dono baskets use karna compulsory hai.

Hum sirf ek basket use karke:

```text
1,1,1
```

collect kar sakte hain.

Answer:

```text
3
```

Agar hum:

```cpp
if(mp.size()==2)
```

lagate, condition kabhi true hi nahi hoti.

Aur correct answer miss ho jata.

---

# 🔄 Complete Dry Run

Consider:

```text
fruits = [1,2,3,2,2]
```

Initially:

```text
low = 0
high = 0
max_len = 0

mp = {}
```

---

## Window 1

Add:

```text
1
```

Map:

```text
1 → 1
```

Unique types:

```text
1
```

Valid because:

```text
1 <= 2
```

Length:

```text
high-low+1

= 0-0+1

= 1
```

So:

```text
max_len = 1
```

---

## Window 2

`high++`

Add:

```text
2
```

Map:

```text
1 → 1
2 → 1
```

Unique:

```text
2
```

Valid.

Window:

```text
[1,2]
```

Length:

```text
2
```

So:

```text
max_len = 2
```

---

## Window 3

Add:

```text
3
```

Map:

```text
1 → 1
2 → 1
3 → 1
```

Now:

```text
mp.size() = 3
```

Invalid ❌

Because:

```text
3 > 2
```

---

## Shrink

Current `low` fruit:

```text
1
```

Decrease:

```text
1 → 0
```

Since:

```text
frequency == 0
```

erase:

```text
1
```

Map:

```text
2 → 1
3 → 1
```

Now:

```text
mp.size() = 2
```

Valid.

Move:

```text
low++
```

Current window:

```text
[2,3]
```

Length:

```text
2
```

Maximum still:

```text
2
```

---

## Next Fruit

Add:

```text
2
```

Map:

```text
2 → 2
3 → 1
```

Window:

```text
[2,3,2]
```

Unique:

```text
2
```

Valid.

Length:

```text
3
```

So:

```text
max_len = 3
```

---

## Next Fruit

Add:

```text
2
```

Map:

```text
2 → 3
3 → 1
```

Window:

```text
[2,3,2,2]
```

Unique:

```text
2
```

Valid.

Length:

```text
4
```

So:

```text
max_len = 4
```

---

# ✅ Final Answer

```text
4
```

Longest valid subarray:

```text
[2,3,2,2]
```

---

# 💻 C++ Code

```cpp
class Solution {
public:
    int totalFruit(vector<int>& fruits) {

        int n=fruits.size();
        int low=0;
        int high=0;
        int max_len=0;

        unordered_map<int,int> mp;

        while(high<n){

            mp[fruits[high]]++;

            while(mp.size()>2){

                mp[fruits[low]]--;

                if(mp[fruits[low]]==0){
                    mp.erase(fruits[low]);
                }

                low++;
            }

            int res=high-low+1;

            max_len=max(res,max_len);

            high++;
        }

        return max_len;
    }
};
```

---

# 🧠 Code Line By Line

### Array Size

```cpp
int n=fruits.size();
```

Total fruits.

---

### Left Pointer

```cpp
int low=0;
```

Window ka starting point.

```text
low++ → SHRINK
```

---

### Right Pointer

```cpp
int high=0;
```

Window ka ending point.

```text
high++ → EXPAND
```

---

### Maximum Length

```cpp
int max_len=0;
```

Longest valid window ki length.

---

### Frequency Map

```cpp
unordered_map<int,int> mp;
```

Fruit type aur uski frequency store karta hai.

---

### Expand

```cpp
mp[fruits[high]]++;
```

Current fruit ko window me add karo.

---

### Invalid Window

```cpp
while(mp.size()>2)
```

2 se jyada fruit types aaye to window invalid hai.

---

### Remove Fruit

```cpp
mp[fruits[low]]--;
```

Leftmost fruit ki frequency decrease.

---

### Erase

```cpp
if(mp[fruits[low]]==0){
    mp.erase(fruits[low]);
}
```

Frequency `0` hui to fruit type current window me nahi hai.

Map se erase.

---

### Shrink

```cpp
low++;
```

Window left side se chhoti hoti hai.

---

### Length

```cpp
int res=high-low+1;
```

Current valid window ki length.

---

### Maximum

```cpp
max_len=max(res,max_len);
```

Longest valid window update.

---

### Expand

```cpp
high++;
```

Next fruit par move.

---

# ⭐ Main Pattern

```cpp
while(high<n){

    // EXPAND
    mp[fruits[high]]++;

    // INVALID → SHRINK
    while(mp.size()>2){

        mp[fruits[low]]--;

        if(mp[fruits[low]]==0){
            mp.erase(fruits[low]);
        }

        low++;
    }

    // Current window is valid
    int res=high-low+1;

    max_len=max(res,max_len);

    high++;
}
```

---

# 🔥 Visual Flow

```text
Fruit add karo using high
          ↓
mp[fruits[high]]++
          ↓
Unique types > 2 ?
          ↓
       YES
          ↓
Left fruit remove
          ↓
frequency--
          ↓
frequency == 0 ?
          ↓
        erase
          ↓
        low++
          ↓
Repeat until size <= 2
          ↓
Current window valid
          ↓
len = high-low+1
          ↓
max_len update
          ↓
high++
```

---

# 🆚 Longest K Unique vs Fruit Into Baskets

| Feature | Longest K Unique | Fruit Into Baskets |
|---|---|---|
| Requirement | Exactly `k` unique | At most `2` unique |
| Invalid | `size > k` | `size > 2` |
| Answer condition | `size == k` | `size <= 2` |
| Need `if(size==k)`? | Yes | No |
| Initial answer | `-1` | `0` |
| Data | Characters | Fruit types |

---

# ⏱️ Time Complexity

```text
O(n)
```

Why?

`high` pointer:

```text
0 → n-1
```

maximum `n` times move karta hai.

`low` pointer bhi:

```text
0 → n-1
```

maximum `n` times move karta hai.

Dono pointers kabhi backward nahi jaate.

Total movement roughly:

```text
n + n = 2n
```

Big-O:

```text
O(2n) = O(n)
```

Nested `while` hone ke bawajood `O(n²)` nahi hai because `low` har outer iteration me starting se dobara traverse nahi karta.

---

# 💾 Space Complexity

Map current window ke fruit types ki frequency maintain karta hai.

Valid window me maximum:

```text
2 fruit types
```

aur new invalid fruit add hone ke moment par temporarily `3` types ho sakte hain.

So map ka active size bounded hai.

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

# 🔥 Quick Revision

```text
2 baskets
   ↓
At most 2 fruit types
   ↓
high se fruit add
   ↓
unique > 2 ?
   ↓ YES
low se remove
   ↓
frequency 0?
   ↓
erase
   ↓
low++
   ↓
unique <= 2
   ↓
window valid
   ↓
length calculate
   ↓
maximum update
```

---

# 🧠 One-Line Revision

```text
High se fruits add karo → 2 se jyada types ho to low se remove karo → size <= 2 hote hi current window valid hai → maximum length update karo.
```

---

# ⭐ Most Important Difference

```text
EXACTLY K
→ answer only when size == k

AT MOST K
→ shrink while size > k
→ uske baad current window automatically valid
```

Fruit Into Baskets:

```text
AT MOST 2
```

isliye:

```cpp
while(mp.size()>2){
    // shrink
}

// no if needed
int res=high-low+1;
max_len=max(res,max_len);
```
