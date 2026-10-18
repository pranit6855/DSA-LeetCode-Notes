# Greedy Extra #3 — LeetCode 2279: Maximum Bags With Full Capacity of Rocks

## 1. Problem Statement

Humein 3 cheezein di gayi hain:

- `capacity[i]` — ith bag ki maximum capacity.
- `rocks[i]` — ith bag mein currently kitne rocks hain.
- `additionalRocks` — extra rocks ki total quantity jo hum use kar sakte hain.

Humein maximum number of bags ko completely fill karna hai.

Agar koi bag already full hai, toh use fill karne ke liye extra rocks ki zaroorat nahi hai.

### Example

Input:

```cpp
capacity = [2, 3, 4, 5];
rocks = [1, 2, 4, 4];
additionalRocks = 2;
```

Output:

```cpp
3
```

Explanation:

Har bag ko full karne ke liye required extra rocks:

- Bag 0: `2 - 1 = 1`
- Bag 1: `3 - 2 = 1`
- Bag 2: `4 - 4 = 0`
- Bag 3: `5 - 4 = 1`

Required rocks:

```text
[1, 1, 0, 1]
```

Sorting ke baad:

```text
[0, 1, 1, 1]
```

Available additional rocks = 2.

- 0 rocks wala bag fill ho jayega. Count = 1
- 1 rock wala bag fill ho jayega. Count = 2
- Agla 1 rock wala bag bhi fill ho jayega. Count = 3
- Ab koi extra rock nahi bacha.

Final answer = `3`.

---

## 2. Pattern

**Greedy + Sorting**

Is problem mein hum sabse kam extra rocks maangne wale bags ko pehle fill karte hain.

Isse limited rocks mein maximum bags complete ho sakte hain.

## 3. Approach

### Step 1: Har bag ki requirement calculate karo

Formula:

```cpp
needed[i] = capacity[i] - rocks[i];
```

Example:

```text
capacity = [2, 3, 4, 5]
rocks    = [1, 2, 4, 4]

needed   = [1, 1, 0, 1]
```

`needed[i]` batata hai ki ith bag ko completely fill karne ke liye kitne additional rocks chahiye.

### Step 2: Required rocks ko sort karo

```cpp
sort(needed.begin(), needed.end());
```

Sorting ke baad:

```text
[0, 1, 1, 1]
```

Ab sabse kam requirement wala bag pehle aayega.

### Step 3: Available rocks check karo

Initially:

```text
additionalRocks = 2
count = 0
```

Har bag ke liye check karenge:

```cpp
if (needed[i] <= additionalRocks)
```

Agar available rocks sufficient hain, toh:

```cpp
additionalRocks -= needed[i];
count++;
```

Yaani required rocks subtract kar do aur filled bags ka count increase kar do.

### Step 4: Final answer return karo

```cpp
return count;
```

`count` maximum filled bags ki quantity batata hai.

---

## 4. C++ Code

```cpp
class Solution {
public:
    int maximumBags(vector<int>& capacity, vector<int>& rocks, int additionalRocks) {
        vector<int> needed;

        for (int i = 0; i < capacity.size(); i++) {
            needed.push_back(capacity[i] - rocks[i]);
        }

        sort(needed.begin(), needed.end());

        int count = 0;

        for (int i = 0; i < needed.size(); i++) {
            if (needed[i] <= additionalRocks) {
                additionalRocks -= needed[i];
                count++;
            }
        }

        return count;
    }
};
```

---

## 5. Code Explanation

### A. Required rocks store karna

```cpp
vector<int> needed;
```

Ek naya vector banaya jisme har bag ko fill karne ke liye required rocks store honge.

```cpp
for (int i = 0; i < capacity.size(); i++) {
    needed.push_back(capacity[i] - rocks[i]);
}
```

Har bag ki capacity mein se uske current rocks subtract karte hain.

Example:

```text
capacity[i] = 5
rocks[i] = 3

needed = 5 - 3 = 2
```

Is bag ko full karne ke liye 2 extra rocks chahiye.

### B. Sorting

```cpp
sort(needed.begin(), needed.end());
```

Required rocks ko ascending order mein sort karta hai.

Example:

```text
Before sorting: [1, 1, 0, 1]
After sorting:  [0, 1, 1, 1]
```

### C. Count initialize karna

```cpp
int count = 0;
```

Initially koi additional bag fill nahi kiya hai, isliye count zero hai.

### D. Har bag ko check karna

```cpp
for (int i = 0; i < needed.size(); i++) {
    if (needed[i] <= additionalRocks) {
        additionalRocks -= needed[i];
        count++;
    }
}
```

Agar bag ki requirement available rocks se kam ya equal hai, toh bag fill kar sakte hain.

Uske baad required rocks subtract karte hain aur count increase kar dete hain.

Agar requirement available rocks se zyada hai, toh us iteration mein kuch nahi karte.

Humne array ko sort kiya hai. Isliye agar kisi bag ko fill karne ke liye bhi rocks insufficient hain, toh uske baad ke bags ki requirements aur badi ya equal hongi. Koi baad wala bag fill nahi ho payega.

### E. Answer return karna

```cpp
return count;
```

Maximum completely filled bags ka count return hota hai.

---

## 6. Detailed Dry Run

Input:

```cpp
capacity = [2, 3, 4, 5];
rocks = [1, 2, 4, 4];
additionalRocks = 2;
```

### Step 1: Required rocks calculate karo

```text
Bag 0: 2 - 1 = 1
Bag 1: 3 - 2 = 1
Bag 2: 4 - 4 = 0
Bag 3: 5 - 4 = 1
```

Required rocks vector:

```text
needed = [1, 1, 0, 1]
```

### Step 2: Sort karo

```text
needed = [0, 1, 1, 1]
```

Initially:

```text
additionalRocks = 2
count = 0
```

### Step 3: Loop chalao

#### Iteration 1

```text
i = 0
needed[0] = 0
additionalRocks = 2
count = 0
```

Condition:

```cpp
if (needed[i] <= additionalRocks)
```

Matlab:

```text
0 <= 2 → True
```

Bag already full hai. Koi extra rock nahi chahiye.

```text
additionalRocks = 2 - 0 = 2
count = 1
```

#### Iteration 2

```text
i = 1
needed[1] = 1
additionalRocks = 2
count = 1
```

Condition:

```text
1 <= 2 → True
```

Bag fill ho jayega.

```text
additionalRocks = 2 - 1 = 1
count = 2
```

#### Iteration 3

```text
i = 2
needed[2] = 1
additionalRocks = 1
count = 2
```

Condition:

```text
1 <= 1 → True
```

Bag fill ho jayega.

```text
additionalRocks = 1 - 1 = 0
count = 3
```

#### Iteration 4

```text
i = 3
needed[3] = 1
additionalRocks = 0
count = 3
```

Condition:

```text
1 <= 0 → False
```

Bag fill nahi kar sakte, kyunki koi extra rock nahi bacha.

Is iteration mein `additionalRocks` aur `count` change nahi honge.

### Final Output

```cpp
return 3;
```

Output:

```text
3
```

### Dry Run Table

| i | Needed rocks | Rocks before | Fill possible? | Rocks after | Count |
|---:|---:|---:|---|---:|---:|
| 0 | 0 | 2 | Yes | 2 | 1 |
| 1 | 1 | 2 | Yes | 1 | 2 |
| 2 | 1 | 1 | Yes | 0 | 3 |
| 3 | 1 | 0 | No | 0 | 3 |

---

## 7. Time and Space Complexity

### Time Complexity: O(n log n)

- Requirements calculate karne mein `O(n)`.
- Sorting mein `O(n log n)`.
- Bags process karne mein `O(n)`.

Total:

```text
O(n) + O(n log n) + O(n) = O(n log n)
```

### Space Complexity: O(n)

Hum `needed` naam ka extra vector bana rahe hain, jisme n elements store honge.

Isliye auxiliary space complexity `O(n)` hai.

---

## 8. Important Interview Points

1. **Pattern:** Greedy + Sorting.
2. Har bag ki requirement `capacity[i] - rocks[i]` se nikalti hai.
3. Required rocks ko ascending order mein sort karte hain.
4. Cheapest requirement wale bags pehle fill karte hain.
5. Bag fill tabhi hoga jab `needed[i] <= additionalRocks`.
6. Bag fill hone ke baad required rocks subtract karte hain aur count increase karte hain.
7. Already full bag ki requirement zero hoti hai, aur woh bhi answer mein count hota hai.
8. Is solution mein `break` ki zaroorat nahi hai.

## 9. Common Mistakes

- Capacity mein se current rocks subtract karna bhool jaana.
- Requirements ko sort nahi karna.
- Rocks subtract karne se pehle count increase karna.
- Condition mein `needed[i] < additionalRocks` use karna. Equal hone par bhi bag fill ho sakta hai, isliye `<=` use karna hai.
- Already full bags ko ignore kar dena.

## 10. Final Takeaway

Is question ka main logic:

**Required rocks calculate karo → Sort karo → Cheapest bags fill karo → Remaining rocks update karo → Count return karo.**

Greedy strategy limited resources ko is tarah use karti hai ki maximum number of bags completely fill ho sakein.
