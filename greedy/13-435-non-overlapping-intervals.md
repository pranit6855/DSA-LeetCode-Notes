# Greedy #13 — LeetCode 435: Non-overlapping Intervals

## 1. Problem Statement

Humein intervals ki ek array di gayi hai. Har interval mein starting aur ending point hota hai.

Humein minimum number of intervals remove karne hain, taaki remaining intervals overlap na karein.

### Example

Input:

```cpp
intervals = {{1,2}, {2,3}, {3,4}, {1,3}};
```

Output:

```cpp
1
```

Explanation:

Intervals `[1,2]`, `[2,3]` aur `[3,4]` ek saath reh sakte hain.

Interval `[1,3]` inmein se kuch intervals ke saath overlap karta hai, isliye ise remove kar denge.

Minimum removals = `1`.

**Important:** Agar ek interval `2` par end hota hai aur next interval `2` par start hota hai, toh woh overlapping nahi maana jaata.

Example:

```text
[1,2] aur [2,3]
```

Dono intervals valid hain.

---

## 2. Pattern

**Greedy + Sorting + Intervals**

Is question mein humein minimum intervals remove karne hain.

Greedy strategy:

**Ending point ke basis par ascending order mein sort karo aur jaldi khatam hone wale intervals ko priority do.**

Isse future intervals ke liye zyada space bachti hai.

---

## 3. Approach

### Step 1: Ending point ke basis par sorting

Normal sorting starting point ke basis par sort karti hai. Lekin is question mein humein ending point ke basis par sort karna hai.

Iske liye ek simple comparator function banayenge.

```cpp
static bool cmp(vector<int>& a, vector<int>& b) {
    return a[1] < b[1];
}
```

Yahan:

- `a[1]` = first interval ka ending point.
- `b[1]` = second interval ka ending point.
- `a[1] < b[1]` = ending point ke ascending order mein sorting.

Sorting call:

```cpp
sort(intervals.begin(), intervals.end(), cmp);
```

### Step 2: Initial values set karo

```cpp
int end = intervals[0][1];
int count = 0;
```

- `end` = last kept interval ka ending point.
- `count` = kitne intervals remove kiye.

Pehla interval keep karenge, kyunki uska ending point sabse chhota hai.

### Step 3: Baaki intervals check karo

```cpp
if (intervals[i][0] < end) {
    count++;
}
else {
    end = intervals[i][1];
}
```

**Case 1: Overlap hai**

Agar current interval ka starting point, `end` se chhota hai, toh overlap hai.

Current interval ko remove kar denge:

```cpp
count++;
```

`end` update nahi karenge, kyunki current interval remove hua hai.

**Case 2: Overlap nahi hai**

Agar current interval ka starting point, `end` ke equal ya greater hai, toh interval keep kar sakte hain.

```cpp
end = intervals[i][1];
```

Ab `end` ko update kar denge.

### Step 4: Answer return karo

```cpp
return count;
```

`count` minimum intervals removed ko represent karta hai.

---

## 4. C++ Code

```cpp
class Solution {
public:
    static bool cmp(vector<int>& a, vector<int>& b) {
        return a[1] < b[1];
    }

    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), cmp);

        int end = intervals[0][1];
        int count = 0;

        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] < end) {
                count++;
            }
            else {
                end = intervals[i][1];
            }
        }

        return count;
    }
};
```

---

## 5. Code Explanation

### A. Comparator function

```cpp
static bool cmp(vector<int>& a, vector<int>& b) {
    return a[1] < b[1];
}
```

Ye function sorting ko batata hai ki intervals kis order mein arrange karne hain.

Agar `a` ka ending point `b` ke ending point se chhota hai, toh `a` pehle aayega.

Example:

```text
[1,3]
[2,4]
[1,2]
```

Ending points:

```text
3, 4, 2
```

Sorting ke baad:

```text
[1,2]
[1,3]
[2,4]
```

### B. Sorting call

```cpp
sort(intervals.begin(), intervals.end(), cmp);
```

Saare intervals ko ending point ke ascending order mein sort karta hai.

### C. Initial ending point

```cpp
int end = intervals[0][1];
```

Sorted array ke first interval ka ending point store karte hain.

Ye interval sabse jaldi khatam hota hai, isliye ise keep karna useful hai.

### D. Removal counter

```cpp
int count = 0;
```

Initially koi interval remove nahi kiya hai.

### E. Loop

```cpp
for (int i = 1; i < intervals.size(); i++)
```

First interval already keep kar liya hai, isliye second interval, yani index `1`, se start karte hain.

### F. Overlap condition

```cpp
if (intervals[i][0] < end) {
    count++;
}
```

Agar current interval ka start, previous kept interval ke end se chhota hai, toh overlap hai.

Current interval ko remove kar denge aur `end` same rakhenge.

### G. Non-overlapping interval

```cpp
else {
    end = intervals[i][1];
}
```

Agar overlap nahi hai, toh current interval keep karenge aur uska ending point update kar denge.

### H. Return

```cpp
return count;
```

Minimum removals return ho jaate hain.

---

## 6. Detailed Dry Run

Input:

```cpp
intervals = {{1,2}, {2,3}, {3,4}, {1,3}};
```

### Step 1: Sorting

Ending point ke ascending order mein sorted array:

```text
Index 0: [1,2]
Index 1: [2,3]
Index 2: [1,3]
Index 3: [3,4]
```

Initially:

```text
end = 2
count = 0
```

First interval `[1,2]` keep kiya.

### Iteration 1: i = 1

Current interval:

```text
[2,3]
```

Condition:

```cpp
intervals[i][0] < end
```

Values put karo:

```text
2 < 2 → False
```

Overlap nahi hai.

`else` execute hoga:

```cpp
end = intervals[i][1];
```

Ab:

```text
end = 3
count = 0
```

Interval `[2,3]` keep ho gaya.

### Iteration 2: i = 2

Current interval:

```text
[1,3]
```

Condition:

```text
1 < 3 → True
```

Overlap hai.

Isliye:

```cpp
count++;
```

Ab:

```text
end = 3
count = 1
```

`end` update nahi hoga, kyunki current interval remove hua hai.

### Iteration 3: i = 3

Current interval:

```text
[3,4]
```

Condition:

```text
3 < 3 → False
```

Overlap nahi hai.

Isliye:

```cpp
end = intervals[i][1];
```

Ab:

```text
end = 4
count = 1
```

Interval `[3,4]` keep ho gaya.

### Final Output

```cpp
return count;
```

Output:

```text
1
```

### Complete Dry Run Table

| i | Current interval | end before check | Condition | Decision | end after | count |
|---:|---|---:|---|---|---:|---:|
| 1 | `[2,3]` | 2 | `2 < 2` → False | Keep | 3 | 0 |
| 2 | `[1,3]` | 3 | `1 < 3` → True | Remove | 3 | 1 |
| 3 | `[3,4]` | 3 | `3 < 3` → False | Keep | 4 | 1 |

Final answer = `1`.

---

## 7. Why Does the Greedy Approach Work?

Hum ending point ke basis par sorting karte hain.

Jis interval ka ending point sabse chhota hota hai, use keep karne se future intervals ke liye maximum space milti hai.

Example:

```text
[1,5]
[2,3]
```

Dono overlap karte hain.

Agar `[1,5]` keep karenge, toh current ending point `5` hoga.

Agar `[2,3]` keep karenge, toh ending point `3` hoga.

`[2,3]` jaldi khatam hota hai, isliye future intervals ko keep karne ke zyada chances milte hain.

Isi wajah se ending-point sorting useful hai.

---

## 8. Time and Space Complexity

### Time Complexity: O(n log n)

Sorting mein `O(n log n)` time lagta hai.

Loop mein har interval ko ek baar check karte hain, isliye `O(n)` time lagta hai.

Overall:

```text
O(n log n) + O(n) = O(n log n)
```

### Auxiliary Space Complexity

Humne koi extra vector nahi banaya.

Typical `sort()` implementation recursion stack use kar sakti hai, isliye auxiliary space implementation par depend karti hai. Typical implementations mein logarithmic stack space hoti hai.

---

## 9. Important Interview Points

1. **Pattern:** Greedy + Sorting + Intervals.
2. Is question mein intervals merge nahi karne; minimum intervals remove karne hain.
3. Ending point ke basis par ascending sorting karte hain.
4. `end` last kept interval ka ending point hai.
5. Agar `intervals[i][0] < end`, toh overlap hai aur `count++` karte hain.
6. Overlap par `end` update nahi hota.
7. Overlap nahi hone par `end` current interval ke ending point se update hota hai.
8. Equal boundary valid hai: `[1,2]` aur `[2,3]` overlapping nahi hain.

---

## 10. Common Mistakes

- Starting point ke basis par sort karke ending-point approach apply karna.
- Overlap hone par `end` ko update kar dena.
- Overlap condition mein `<=` use karna. Isse `[1,2]` aur `[2,3]` galti se overlapping count honge.
- Initial `end` ko first interval ke ending point se initialize na karna.
- `count` ke badle kept intervals ka count return karna.

---

## 11. Final Takeaway

Is question ka core logic:

**Ending point ke basis par sort karo → Pehla interval keep karo → Overlap hone par current interval remove karo → Overlap na ho toh `end` update karo → Count return karo.**

LC 56 mein overlapping intervals ko merge karte the. LC 435 mein minimum intervals remove karte hain. Dono mein sorting aur intervals ka logic use hota hai, lekin objective alag hai.
