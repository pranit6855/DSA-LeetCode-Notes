# Greedy #12 — LeetCode 56: Merge Intervals

## 1. Problem Statement

Humein intervals ki ek array di gayi hai. Har interval mein starting aur ending point hota hai.

Agar do intervals overlap karte hain, toh unhe merge karna hai. Agar overlap nahi karte, toh unhe alag rakhna hai.

Humein final merged intervals return karne hain.

### Example

Input:

```cpp
intervals = {{1,3}, {2,6}, {8,10}, {15,18}};
```

Output:

```cpp
{{1,6}, {8,10}, {15,18}}
```

Explanation:

- `[1,3]` aur `[2,6]` overlap karte hain, isliye merge hokar `[1,6]` banenge.
- `[8,10]` ka `[1,6]` se overlap nahi hai.
- `[15,18]` ka `[8,10]` se overlap nahi hai.

Final output:

```text
[[1,6], [8,10], [15,18]]
```

---

## 2. Pattern

**Sorting + Intervals + Greedy**

Is question mein hum intervals ko starting point ke basis par sort karte hain.

Phir ek-ek interval ko check karte hain:

- Overlap nahi hai → interval ko answer mein add karo.
- Overlap hai → last interval ka ending point update karke merge karo.

---

## 3. Approach

### Step 1: Intervals ko sort karo

```cpp
sort(intervals.begin(), intervals.end());
```

Normal sorting se intervals starting point ke basis par ascending order mein aa jaate hain.

Example:

```text
Before sorting:
[[2,6], [1,3], [15,18], [8,10]]

After sorting:
[[1,3], [2,6], [8,10], [15,18]]
```

### Step 2: Answer vector banao

```cpp
vector<vector<int>> ans;
```

Is vector mein final merged intervals store honge.

Initially:

```text
ans = []
```

### Step 3: Har interval ko process karo

```cpp
for (int i = 0; i < intervals.size(); i++)
```

Har interval ko answer ke last interval ke saath compare karenge.

### Step 4: Check karo ki overlap hai ya nahi

```cpp
if (ans.empty() || ans.back()[1] < intervals[i][0])
```

Is condition ke do parts hain:

**Part 1: `ans.empty()`**

Agar answer empty hai, toh pehla interval directly add kar do.

**Part 2: `ans.back()[1] < intervals[i][0]`**

- `ans.back()[1]` = answer ke last interval ka ending point.
- `intervals[i][0]` = current interval ka starting point.

Agar last interval ka end, current interval ke start se chhota hai, toh overlap nahi hai.

Isliye current interval ko answer mein add karenge.

```cpp
ans.push_back(intervals[i]);
```

### Step 5: Agar overlap hai, toh merge karo

```cpp
else {
    ans.back()[1] = max(ans.back()[1], intervals[i][1]);
}
```

Dono intervals overlap karte hain, isliye unhe merge karenge.

Merged interval ka starting point same rahega, lekin ending point maximum ending point hoga.

`max()` ensure karta hai ki merged interval ka ending point dono intervals mein se sabse bada ho.

### Step 6: Final answer return karo

```cpp
return ans;
```

---

## 4. C++ Code

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());

        vector<vector<int>> ans;

        for (int i = 0; i < intervals.size(); i++) {
            if (ans.empty() || ans.back()[1] < intervals[i][0]) {
                ans.push_back(intervals[i]);
            }
            else {
                ans.back()[1] = max(ans.back()[1], intervals[i][1]);
            }
        }

        return ans;
    }
};
```

---

## 5. Code Explanation

### A. Sorting

```cpp
sort(intervals.begin(), intervals.end());
```

Intervals ko starting point ke ascending order mein arrange karta hai.

Isse humein pata rehta hai ki next interval ka starting point current interval ke starting point se chhota nahi hoga.

### B. Answer vector

```cpp
vector<vector<int>> ans;
```

Yahan merged intervals store honge.

### C. For loop

```cpp
for (int i = 0; i < intervals.size(); i++)
```

Har interval ko ek-ek karke process karte hain.

### D. New interval add karna

```cpp
if (ans.empty() || ans.back()[1] < intervals[i][0]) {
    ans.push_back(intervals[i]);
}
```

Agar answer empty hai ya current interval last saved interval se overlap nahi karta, toh current interval ko directly add kar dete hain.

### E. Overlapping intervals merge karna

```cpp
ans.back()[1] = max(ans.back()[1], intervals[i][1]);
```

`ans.back()` answer ke last interval ko access karta hai.

`ans.back()[1]` us interval ka ending point hai.

`max()` se dono ending points mein se bada ending point select karte hain.

Example:

```text
Last interval = [1,3]
Current interval = [2,6]
```

Calculation:

```cpp
max(3, 6) = 6;
```

Merged interval:

```text
[1,6]
```

### F. Return

```cpp
return ans;
```

Final merged intervals return ho jaate hain.

---

## 6. Detailed Dry Run

Input:

```cpp
intervals = {{1,3}, {2,6}, {8,10}, {15,18}};
```

### Step 1: Sorting

Array already sorted hai.

```text
Index 0: [1,3]
Index 1: [2,6]
Index 2: [8,10]
Index 3: [15,18]
```

Initially:

```text
ans = []
```

### Iteration 1: i = 0

Current interval:

```text
[1,3]
```

Condition:

```cpp
ans.empty() || ans.back()[1] < intervals[i][0]
```

`ans` empty hai, isliye condition true hai.

Current interval add karenge:

```cpp
ans.push_back(intervals[i]);
```

Ab:

```text
ans = [[1,3]]
```

### Iteration 2: i = 1

Current interval:

```text
[2,6]
```

Answer ka last interval:

```text
[1,3]
```

Values:

```text
Last interval ka end = 3
Current interval ka start = 2
```

Condition:

```text
3 < 2 → False
```

Overlap hai, isliye `else` execute hoga.

```cpp
ans.back()[1] = max(ans.back()[1], intervals[i][1]);
```

Calculation:

```text
max(3,6) = 6
```

Ab answer:

```text
ans = [[1,6]]
```

Dono intervals merge ho gaye.

### Iteration 3: i = 2

Current interval:

```text
[8,10]
```

Answer ka last interval:

```text
[1,6]
```

Values:

```text
Last interval ka end = 6
Current interval ka start = 8
```

Condition:

```text
6 < 8 → True
```

Overlap nahi hai.

Current interval add kar denge:

```cpp
ans.push_back(intervals[i]);
```

Ab:

```text
ans = [[1,6], [8,10]]
```

### Iteration 4: i = 3

Current interval:

```text
[15,18]
```

Answer ka last interval:

```text
[8,10]
```

Values:

```text
Last interval ka end = 10
Current interval ka start = 15
```

Condition:

```text
10 < 15 → True
```

Overlap nahi hai.

Current interval add karenge.

Ab:

```text
ans = [[1,6], [8,10], [15,18]]
```

### Final Output

```cpp
return ans;
```

Output:

```text
[[1,6], [8,10], [15,18]]
```

### Complete Dry Run Table

| i | Current interval | Last interval | Overlap? | Decision | Answer |
|---:|---|---|---|---|---|
| 0 | `[1,3]` | None | N/A | Add | `[[1,3]]` |
| 1 | `[2,6]` | `[1,3]` | Yes | Merge | `[[1,6]]` |
| 2 | `[8,10]` | `[1,6]` | No | Add | `[[1,6],[8,10]]` |
| 3 | `[15,18]` | `[8,10]` | No | Add | `[[1,6],[8,10],[15,18]]` |

---

## 7. Important Concept: `ans.back()`

```cpp
ans.back()
```

Ye answer vector ke last element ko access karta hai.

Example:

```text
ans = [[1,6], [8,10]]
```

Toh:

```cpp
ans.back()
```

Last interval `[8,10]` return karega.

Aur:

```cpp
ans.back()[1]
```

Last interval ka ending point `10` return karega.

Isliye hum har new interval ko answer ke last interval ke saath compare kar sakte hain.

---

## 8. Time and Space Complexity

### Time Complexity: O(n log n)

Sorting mein `O(n log n)` time lagta hai.

For loop mein har interval ek baar process hota hai, isliye `O(n)` time lagta hai.

Overall:

```text
O(n log n) + O(n) = O(n log n)
```

### Auxiliary Space Complexity

Answer vector mein maximum `n` intervals store ho sakte hain.

Isliye output space `O(n)` hai.

Sorting ki additional space typical implementation mein logarithmic recursion stack ho sakti hai.

---

## 9. Important Interview Points

1. **Pattern:** Sorting + Intervals + Greedy.
2. Intervals ko starting point ke basis par sort karte hain.
3. `ans.back()` last merged interval ko access karta hai.
4. Overlap nahi hai toh `push_back()` use karte hain.
5. Overlap hai toh `max()` se ending point update karte hain.
6. Starting point sorted order ki wajah se merge karte waqt same rehta hai.
7. Is problem mein intervals remove nahi karne; overlapping intervals ko merge karna hai.

---

## 10. Common Mistakes

- Intervals ko sort karna bhool jaana.
- Overlap check mein current interval ka ending point compare karna.
- Merge karte waqt `max()` ki jagah `min()` use karna.
- `ans.back()[1]` ka meaning samajhne bina use karna.
- Overlap na hone par current interval ko answer mein add na karna.
- Equality ko galat handle karna. Is solution mein agar last end aur current start equal hon, toh intervals merge honge.

---

## 11. Final Takeaway

Is question ka main logic:

**Sort → Last saved interval se compare → Overlap nahi hai toh add → Overlap hai toh ending point merge karo → Answer return karo.**

LC 56 ka objective overlapping intervals ko combine karna hai.

LC 435 ka objective minimum intervals remove karna hai.

Dono questions mein intervals aur sorting ka concept common hai, lekin unka main greedy decision alag hai.
