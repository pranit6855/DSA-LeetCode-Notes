# Greedy #14 — LeetCode 452: Minimum Number of Arrows to Burst Balloons

## 1. Problem Statement

Humein balloons ki positions intervals ke form mein di gayi hain.

Har balloon ko `[start, end]` se represent kiya gaya hai.

Ek arrow ko kisi bhi `x` position par shoot kar sakte hain. Agar balloon ka interval us position ko cover karta hai, toh woh balloon burst ho jayega.

Humein saare balloons burst karne ke liye minimum number of arrows return karne hain.

### Example

Input:

```cpp
points = {{10,16}, {2,8}, {1,6}, {7,12}};
```

Output:

```cpp
2
```

Explanation:

Agar arrow `x = 6` par shoot karein, toh balloons `[1,6]` aur `[2,8]` burst ho jayenge.

Agar doosra arrow `x = 12` par shoot karein, toh balloons `[7,12]` aur `[10,16]` burst ho jayenge.

Saare balloons sirf 2 arrows mein burst ho gaye.

Final answer = `2`.

---

## 2. Pattern

**Greedy + Sorting + Intervals**

Is question mein hum balloons ko ending point ke ascending order mein sort karte hain.

Greedy strategy:

**Pehle balloon ke ending point par arrow shoot karo. Jab tak next balloon us position ko cover karta hai, same arrow use karo. Jab cover nahi karta, tab naya arrow lagao.**

---

## 3. Approach

### Step 1: Ending point ke basis par sort karo

Humein starting point ke basis par nahi, ending point ke basis par sorting karni hai.

Iske liye ek simple comparator function banayenge.

```cpp
static bool cmp(vector<int>& a, vector<int>& b) {
    return a[1] < b[1];
}
```

Yahan:

- `a[1]` = first balloon ka ending point.
- `b[1]` = second balloon ka ending point.
- `a[1] < b[1]` = ending point ke ascending order mein sorting.

Sorting call:

```cpp
sort(points.begin(), points.end(), cmp);
```

### Step 2: Pehle balloon ke liye arrow lagao

```cpp
int end = points[0][1];
int arrows = 1;
```

- `end` = current arrow ki position.
- `arrows = 1` = pehle balloon ke liye ek arrow already use kar rahe hain.

Hum arrow ko pehle balloon ke ending point par shoot karte hain, kyunki ending point ke basis par sorting ki hai.

### Step 3: Loop `i = 1` se start karo

```cpp
for (int i = 1; i < points.size(); i++)
```

Pehla balloon already process ho chuka hai.

Isliye hum second balloon, yani index `1`, se checking start karte hain.

### Step 4: Check karo ki naya arrow chahiye ya nahi

```cpp
if (points[i][0] > end)
```

- `points[i][0]` = current balloon ka starting point.
- `end` = current arrow ki position.

Agar current balloon ka start, arrow position se bada hai, toh existing arrow is balloon ko burst nahi kar sakta.

Isliye:

```cpp
arrows++;
end = points[i][1];
```

Naya arrow use karenge aur uski position current balloon ke ending point par set kar denge.

### Step 5: Agar condition false hai

Agar:

```cpp
points[i][0] <= end
```

Toh current arrow ki position current balloon ke interval ke andar hai.

Isliye balloon existing arrow se burst ho jayega.

Koi naya arrow nahi chahiye aur `end` update bhi nahi karna hai.

### Step 6: Answer return karo

```cpp
return arrows;
```

---

## 4. C++ Code

```cpp
class Solution {
public:
    static bool cmp(vector<int>& a, vector<int>& b) {
        return a[1] < b[1];
    }

    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(), points.end(), cmp);

        int end = points[0][1];
        int arrows = 1;

        for (int i = 1; i < points.size(); i++) {
            if (points[i][0] > end) {
                arrows++;
                end = points[i][1];
            }
        }

        return arrows;
    }
};
```

---

## 5. Code Explanation

### A. Comparator Function

```cpp
static bool cmp(vector<int>& a, vector<int>& b) {
    return a[1] < b[1];
}
```

Ye function sorting ko batata hai ki balloons kis order mein arrange karne hain.

`a[1]` aur `b[1]` dono intervals ke ending points hain.

Agar `a` ka ending point chhota hai, toh `a` pehle aayega.

### B. Sorting

```cpp
sort(points.begin(), points.end(), cmp);
```

Saare balloons ko ending point ke ascending order mein arrange karta hai.

Example:

```text
Before sorting:

[10,16]
[2,8]
[1,6]
[7,12]

After sorting:

[1,6]
[2,8]
[7,12]
[10,16]
```

### C. Initial Values

```cpp
int end = points[0][1];
int arrows = 1;
```

Sorted array ka pehla balloon `[1,6]` hai.

Hum arrow ko position `6` par shoot karte hain.

Initially:

```text
end = 6
arrows = 1
```

### D. For Loop

```cpp
for (int i = 1; i < points.size(); i++)
```

Pehla balloon already process ho chuka hai.

Isliye loop second balloon se start hota hai.

### E. New Arrow Condition

```cpp
if (points[i][0] > end) {
    arrows++;
    end = points[i][1];
}
```

Agar current balloon ka starting point existing arrow position se bada hai, toh existing arrow use burst nahi kar sakta.

Isliye naya arrow lagate hain aur `end` update karte hain.

### F. Condition False Hone Par

Agar `points[i][0] <= end` hai, toh existing arrow current balloon ko burst kar sakta hai.

Isliye na `arrows` badhega aur na `end` update hoga.

### G. Return

```cpp
return arrows;
```

Total arrows return ho jaate hain.

---

## 6. Detailed Dry Run

Input:

```cpp
points = {{2,9}, {1,5}, {7,12}, {10,14},
          {13,17}, {16,20}, {18,22}};
```

### Step 1: Sorting

Ending point ke ascending order mein:

```text
Index 0: [1,5]
Index 1: [2,9]
Index 2: [7,12]
Index 3: [10,14]
Index 4: [13,17]
Index 5: [16,20]
Index 6: [18,22]
```

Initially:

```text
end = points[0][1] = 5
arrows = 1
```

Pehla arrow position `5` par shoot hua.

### Iteration 1: i = 1

Current balloon:

```text
[2,9]
```

Values:

```text
points[i][0] = 2
end = 5
```

Condition:

```cpp
if (points[i][0] > end)
```

Calculation:

```text
2 > 5 → False
```

Position `5`, balloon `[2,9]` ke andar hai.

Isliye same arrow se balloon burst ho jayega.

```text
end = 5
arrows = 1
```

Koi update nahi karna.

### Iteration 2: i = 2

Current balloon:

```text
[7,12]
```

Values:

```text
points[i][0] = 7
end = 5
```

Condition:

```text
7 > 5 → True
```

Existing arrow position `5` par hai, lekin balloon `7` se start hota hai.

Isliye naya arrow lagayenge.

```cpp
arrows++;
end = points[i][1];
```

Calculation:

```text
arrows = 1 + 1 = 2
end = 12
```

Naya arrow position `12` par shoot hua.

### Iteration 3: i = 3

Current balloon:

```text
[10,14]
```

Condition:

```text
10 > 12 → False
```

Position `12`, balloon `[10,14]` ke andar hai.

Existing arrow is balloon ko burst kar dega.

```text
arrows = 2
end = 12
```

Koi update nahi karna.

### Iteration 4: i = 4

Current balloon:

```text
[13,17]
```

Condition:

```text
13 > 12 → True
```

Existing arrow position `12` par hai, lekin balloon `13` se start hota hai.

Naya arrow lagayenge.

```text
arrows = 3
end = 17
```

Naya arrow position `17` par shoot hua.

### Iteration 5: i = 5

Current balloon:

```text
[16,20]
```

Condition:

```text
16 > 17 → False
```

Position `17` is balloon ke interval ke andar hai.

Same arrow se balloon burst ho jayega.

```text
arrows = 3
end = 17
```

Koi update nahi karna.

### Iteration 6: i = 6

Current balloon:

```text
[18,22]
```

Condition:

```text
18 > 17 → True
```

Existing arrow position `17` par hai. Ye balloon `18` se start hota hai.

Isliye naya arrow lagayenge.

```text
arrows = 4
end = 22
```

### Final Output

```cpp
return arrows;
```

Output:

```text
4
```

### Complete Dry Run Table

| i | Current balloon | `start > end` | Decision | Arrows | `end` |
|---:|---|---|---|---:|---:|
| Initial | `[1,5]` | — | Pehla arrow | 1 | 5 |
| 1 | `[2,9]` | `2 > 5` → False | Same arrow | 1 | 5 |
| 2 | `[7,12]` | `7 > 5` → True | Naya arrow | 2 | 12 |
| 3 | `[10,14]` | `10 > 12` → False | Same arrow | 2 | 12 |
| 4 | `[13,17]` | `13 > 12` → True | Naya arrow | 3 | 17 |
| 5 | `[16,20]` | `16 > 17` → False | Same arrow | 3 | 17 |
| 6 | `[18,22]` | `18 > 17` → True | Naya arrow | 4 | 22 |

Final answer = `4`.

---

## 7. Important Concept: `i = 1` Se Kyun Start Karte Hain?

Code:

```cpp
int end = points[0][1];
int arrows = 1;
```

In dono lines ne pehle balloon ko already process kar diya.

- `end` mein first balloon ka ending point store ho gaya.
- `arrows = 1` se uske liye pehla arrow count ho gaya.

Isliye loop `i = 1` se start karte hain.

Agar loop `i = 0` se start karte, toh first balloon ko unnecessarily dobara check karte.

**Rule:** Jab first element ko initialization mein already process kar diya ho, toh loop usually index `1` se start kar sakta hai.

---

## 8. Important Difference: `>` Aur `>=`

Condition:

```cpp
if (points[i][0] > end)
```

Yahan strictly `>` use karte hain, `>=` nahi.

Example:

```text
Current arrow = 5
Current balloon = [5,9]
```

Condition:

```text
5 > 5 → False
```

Existing arrow position `5` balloon `[5,9]` ke andar hai, isliye balloon burst ho jayega.

Agar `>=` use karte, toh galti se naya arrow laga dete.

---

## 9. Why Does the Greedy Approach Work?

Hum ending point ke basis par sorting karte hain.

Pehle khatam hone wale balloon ke ending point par arrow shoot karne se overlapping balloons ko ek hi arrow se burst karne ka chance maximize hota hai.

Jab next balloon ka starting point current arrow position se bada ho jaata hai, tab existing arrow us balloon tak nahi pahunch sakta. Isliye naya arrow zaroori hai.

---

## 10. Time and Space Complexity

### Time Complexity: O(n log n)

Sorting mein `O(n log n)` time lagta hai.

Loop mein har balloon ek baar process hota hai, isliye `O(n)` time lagta hai.

Overall:

```text
O(n log n) + O(n) = O(n log n)
```

### Auxiliary Space Complexity

Humne koi extra vector nahi banaya.

Sorting ki additional space typical implementation mein recursion stack ki wajah se logarithmic ho sakti hai.

---

## 11. Important Interview Points

1. **Pattern:** Greedy + Sorting + Intervals.
2. Ending point ke basis par ascending sorting karte hain.
3. Pehle balloon ke liye ek arrow initialize karte hain.
4. Loop `i = 1` se start hota hai, kyunki first balloon already process ho chuka hai.
5. Agar `points[i][0] > end`, toh naya arrow chahiye.
6. Naya arrow lagane par `arrows++` aur `end = points[i][1]`.
7. Agar condition false hai, toh existing arrow current balloon ko burst kar sakta hai.
8. `>` use hota hai, `>=` nahi, kyunki balloon ke endpoints included hote hain.

---

## 12. Common Mistakes

- Comparator banakar `sort()` mein pass karna bhool jaana.
- Starting point ke basis par sort karna, jabki approach ending-point sorting par based hai.
- `end` ko har interval ke liye update kar dena, chahe naya arrow laga ho ya nahi.
- `points[i][0] >= end` condition use karna.
- Pehle balloon ke liye `arrows = 1` initialize karna bhool jaana.

---

## 13. Final Takeaway

Is question ka core logic:

**Ending point ke basis par sort karo → Pehle balloon ke liye arrow rakho → Agar next balloon ka start `end` se bada hai toh naya arrow lagao → Warna same arrow use karo → Total arrows return karo.**

LC 435 aur LC 452 dono mein ending-point sorting use hoti hai.

- **LC 435:** Overlap hone par interval remove karte hain.
- **LC 452:** Overlap hone par existing arrow se balloon burst karte hain.

Dono questions mein common idea hai ki jaldi khatam hone wale intervals ko priority deni chahiye.
