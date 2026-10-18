# Greedy Extra #1 — LeetCode 2144: Minimum Cost of Buying Candies With Discount

## 1. Problem Statement

Humein `cost` naam ka integer array diya gaya hai, jisme har candy ki price hai.

Har 3 candies khareedne par unmein se sabse sasti candy free milti hai.

Humein saari candies khareedni hain aur minimum total cost return karni hai.

### Example

Input:
cost = [1, 2, 3, 4, 5, 6]

Output:
16

Explanation:

Sorted prices descending order mein:
[6, 5, 4, 3, 2, 1]

Pehle 3 candies:
- Price 6: Pay
- Price 5: Pay
- Price 4: Free

Agli 3 candies:
- Price 3: Pay
- Price 2: Pay
- Price 1: Free

Total cost = 6 + 5 + 3 + 2 = 16.

---

## 2. Pattern

**Greedy + Sorting**

Is question mein hum expensive candies pehle arrange karte hain, taaki har 3 candies ke group mein sabse sasti candy free ho sake.

## 3. Approach

Hum is problem ko 3 simple steps mein solve karenge.

### Step 1: Descending order mein sort karo

```cpp
sort(cost.begin(), cost.end(), greater<int>());
```

Isse prices highest se lowest order mein aa jayengi.

Example:
[1, 5, 2, 6, 3, 4]

Sorting ke baad:
[6, 5, 4, 3, 2, 1]

`greater<int>()` descending order mein sort karne ke liye use hota hai.

### Step 2: Saari prices ka total nikalo

Ek variable `total = 0` se start karenge.

Har candy ki price ko `total` mein add kar denge.

Example:
6 + 5 + 4 + 3 + 2 + 1 = 21

Abhi total cost 21 hai, lekin humein free candies ka paisa pay nahi karna hai.

### Step 3: Har third candy ki price minus karo

Descending sorted array mein har 3 candies ke group ka third element free hoga.

Iske liye loop:

```cpp
for (int i = 2; i < cost.size(); i += 3)
```

Index `2` se start karenge, kyunki C++ mein indexing zero se hoti hai.

Isliye free candies ke indices honge:

2, 5, 8, 11...

Har baar index mein `3` add karenge.

Example:
- Index 2 → Price 4 free
- Index 5 → Price 1 free

Final cost = 21 - 4 - 1 = 16.

---

## 4. C++ Code

```cpp
class Solution {
public:
    int minimumCost(vector<int>& cost) {
        sort(cost.begin(), cost.end(), greater<int>());

        int total = 0;

        for (int i = 0; i < cost.size(); i++) {
            total += cost[i];
        }

        for (int i = 2; i < cost.size(); i += 3) {
            total -= cost[i];
        }

        return total;
    }
};
```

---

## 5. Code Explanation

### Sorting

```cpp
sort(cost.begin(), cost.end(), greater<int>());
```

Saari candy prices ko descending order mein arrange karta hai.

`greater<int>()` ka matlab hai bade numbers ko pehle rakhna.

Example:
[1, 3, 2, 6, 5, 4]

Sorting ke baad:
[6, 5, 4, 3, 2, 1]

### Total initialize karna

```cpp
int total = 0;
```

Is variable mein saari candies ki prices ka sum store hoga.

### Saari prices add karna

```cpp
for (int i = 0; i < cost.size(); i++) {
    total += cost[i];
}
```

Loop har candy ki price ko total mein add karta hai.

### Free candies ki prices minus karna

```cpp
for (int i = 2; i < cost.size(); i += 3) {
    total -= cost[i];
}
```

Index `2` se har third element ko select karte hain.

Ye candies free hain, isliye unki prices total se subtract kar dete hain.

### Answer return karna

```cpp
return total;
```

Final minimum cost return ho jaati hai.

---

## 6. Detailed Dry Run

Input:

```cpp
cost = [1, 2, 3, 4, 5, 6];
```

### Step 1: Sorting

Descending order:

```text
Index:   0  1  2  3  4  5
Price:   6  5  4  3  2  1
```

### Step 2: Total calculate karna

Initially:

```text
total = 0
```

| Index | Price | Calculation | Total |
|---:|---:|---|---:|
| 0 | 6 | 0 + 6 | 6 |
| 1 | 5 | 6 + 5 | 11 |
| 2 | 4 | 11 + 4 | 15 |
| 3 | 3 | 15 + 3 | 18 |
| 4 | 2 | 18 + 2 | 20 |
| 5 | 1 | 20 + 1 | 21 |

Ab:

```text
total = 21
```

### Step 3: Free candies minus karna

Loop ki starting value:

```text
i = 2
```

**First iteration:**

```text
cost[2] = 4

total = 21 - 4
total = 17
```

Next index:

```text
i = 2 + 3 = 5
```

**Second iteration:**

```text
cost[5] = 1

total = 17 - 1
total = 16
```

Next index:

```text
i = 5 + 3 = 8
```

Ab `8 < cost.size()` false hai, kyunki array ka size 6 hai. Loop stop ho jayega.

### Final Answer

```text
return 16;
```

---

## 7. Time and Space Complexity

### Time Complexity: O(n log n)

Sorting mein O(n log n) time lagta hai.

Total calculate karne aur free prices subtract karne mein O(n) time lagta hai.

Overall complexity:

O(n log n) + O(n) + O(n) = O(n log n)

### Auxiliary Space Complexity: O(log n)

Typical sorting implementation mein recursion stack ke liye O(log n) auxiliary space lag sakti hai. Sorting implementation ke according actual space usage vary kar sakti hai.

---

## 8. Important Interview Points

1. **Pattern:** Greedy + Sorting.
2. Descending sorting se expensive candies pehle aati hain.
3. Har group ki third candy free hoti hai.
4. Total sum calculate karke free candies ki prices subtract kar sakte hain.
5. Loop `i = 2` se start hota hai kyunki indexing zero-based hai.
6. `i += 3` har third candy tak directly pahunchata hai.
7. `greater<int>()` descending sorting ke liye use hota hai.

## 9. Common Mistakes

- Normal ascending sorting ke baad har third price subtract kar dena.
- Loop ko `i = 0` se start karna.
- `i++` use karna, jabki humein har teesre index par jaana hai.
- Free candy ko galat group se choose karna.

## 10. Final Takeaway

Is problem ka main idea hai:

**Descending sort → Total sum → Har third candy ki price subtract → Answer return.**

Ye approach tab useful hai jab discount ya free-item rule ki wajah se sorted items ke specific positions ko skip karna ho.
