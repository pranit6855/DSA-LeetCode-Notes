# Greedy #15 — LeetCode 763: Partition Labels

## 1. Problem Statement

Humein ek string `s` di gayi hai.

Humein string ko maximum possible parts mein divide karna hai, taaki har character string ke sirf ek hi part mein appear ho.

Matlab kisi character ki saari occurrences ek hi partition mein honi chahiye.

Finally, humein har partition ki length return karni hai.

### Example

Input:

```cpp
s = "ababcbacadefegdehijhklij";
```

Output:

```cpp
[9, 7, 8]
```

Explanation:

String ke partitions honge:

```text
ababcbaca | defegde | hijhklij
```

Inki lengths:

- `ababcbaca` → 9
- `defegde` → 7
- `hijhklij` → 8

Har character ki saari occurrences uske respective partition mein hain.

Isliye answer:

```text
[9, 7, 8]
```

---

## 2. Pattern

**Greedy + String**

Is question mein hum har character ka last occurrence index store karte hain.

Phir string ko left se right traverse karke decide karte hain ki current partition kahan tak jaana chahiye.

Greedy idea:

**Jab tak current partition mein present characters ki last occurrences cover nahi hoti, tab tak partition close nahi kar sakte.**

---

## 3. Approach

### Step 1: Har character ka last index store karo

```cpp
vector<int> last(26, 0);

for (int i = 0; i < s.size(); i++) {
    last[s[i] - 'a'] = i;
}
```

`last` naam ka vector banaya hai jisme 26 lowercase English letters ke last indices store honge.

Example:

```text
s = "abac"

Index:  0  1  2  3
String: a  b  a  c
```

Last occurrences:

```text
a → 2
b → 1
c → 3
```

Kyunki `a` index 0 aur 2 par aata hai, toh uska last index 2 hai.

### Step 2: Variables initialize karo

```cpp
vector<int> ans;
int start = 0;
int end = 0;
```

- `ans` — partition lengths store karega.
- `start` — current partition ka starting index.
- `end` — current partition ka required ending index.

Initially:

```text
start = 0
end = 0
```

### Step 3: String traverse karo

```cpp
for (int i = 0; i < s.size(); i++) {
    end = max(end, last[s[i] - 'a']);
}
```

Har character ke liye uska last index check karte hain.

```cpp
end = max(end, last[s[i] - 'a']);
```

Iska matlab hai current partition ka end, ab tak dekhe gaye characters ki last occurrences ko cover karna chahiye.

`max()` isliye use karte hain kyunki koi naya character current end se bhi aage appear ho sakta hai.

### Step 4: Partition complete hone ka check

```cpp
if (i == end) {
    ans.push_back(i - start + 1);
    start = i + 1;
}
```

Agar current index `i`, `end` ke equal ho gaya, toh current partition complete hai.

Ab partition ki length calculate karenge:

```cpp
i - start + 1
```

Is length ko `ans` mein add karenge.

Uske baad next partition ka starting index:

```cpp
start = i + 1;
```

### Step 5: Answer return karo

```cpp
return ans;
```

Final partition lengths return ho jayengi.

---

## 4. C++ Code

```cpp
class Solution {
public:
    vector<int> partitionLabels(string s) {
        vector<int> last(26, 0);

        for (int i = 0; i < s.size(); i++) {
            last[s[i] - 'a'] = i;
        }

        vector<int> ans;
        int start = 0;
        int end = 0;

        for (int i = 0; i < s.size(); i++) {
            end = max(end, last[s[i] - 'a']);

            if (i == end) {
                ans.push_back(i - start + 1);
                start = i + 1;
            }
        }

        return ans;
    }
};
```

---

## 5. Code Explanation

### A. Last occurrence array

```cpp
vector<int> last(26, 0);
```

26 lowercase English letters ke last indices store karne ke liye vector banaya.

```cpp
for (int i = 0; i < s.size(); i++) {
    last[s[i] - 'a'] = i;
}
```

Har character ke liye uska current index store karte hain.

Agar character dobara milta hai, toh uski value update ho jaati hai. Loop complete hone ke baad har character ka last index mil jaata hai.

### B. `s[i] - 'a'` ka meaning

```cpp
last[s[i] - 'a']
```

Lowercase characters ko `0` se `25` tak index mein convert karta hai.

Example:

```text
'a' - 'a' = 0
'b' - 'a' = 1
'c' - 'a' = 2
```

Isliye `last` vector mein har character ka last index efficiently store kar sakte hain.

### C. Variables

```cpp
int start = 0;
int end = 0;
```

`start` current partition ka starting index hai.

`end` batata hai ki current partition ko minimum kitne index tak extend karna zaroori hai.

### D. Partition end update karna

```cpp
end = max(end, last[s[i] - 'a']);
```

Current character ka last occurrence check karke partition ka end update karte hain.

Example:

```text
Current end = 2
Current character ka last index = 5
```

Calculation:

```cpp
end = max(2, 5);
```

Ab:

```text
end = 5
```

Partition ko index 5 tak extend karna padega.

### E. Partition complete karna

```cpp
if (i == end)
```

Jab `i == end` hota hai, toh current partition ke saare characters ki last occurrences cover ho chuki hoti hain.

Isliye ab partition close kar sakte hain.

### F. Partition length calculate karna

```cpp
ans.push_back(i - start + 1);
```

Partition ki length:

```text
Ending index - Starting index + 1
```

`+1` isliye karte hain kyunki starting aur ending dono indices included hain.

### G. Next partition start karna

```cpp
start = i + 1;
```

Current partition `i` par end ho gaya, toh agla partition `i + 1` se shuru hoga.

---

## 6. Detailed Dry Run

Input:

```cpp
s = "abac";
```

Index ke saath string:

```text
Index:  0  1  2  3
String: a  b  a  c
```

### Step 1: Last indices store karo

Character `a` index 0 aur 2 par hai.

Isliye:

```text
last[a] = 2
```

Character `b` index 1 par hai.

```text
last[b] = 1
```

Character `c` index 3 par hai.

```text
last[c] = 3
```

Initially:

```text
start = 0
end = 0
ans = []
```

### Iteration 1: i = 0

Current character:

```text
s[0] = 'a'
last[a] = 2
```

Code:

```cpp
end = max(end, last[s[i] - 'a']);
```

Calculation:

```text
end = max(0, 2)
end = 2
```

Condition:

```cpp
i == end
```

Values:

```text
0 == 2 → False
```

Partition abhi complete nahi hai.

Current state:

```text
start = 0
end = 2
ans = []
```

Humein `a` ki last occurrence cover karne ke liye index 2 tak jaana padega.

### Iteration 2: i = 1

Current character:

```text
s[1] = 'b'
last[b] = 1
```

Calculation:

```text
end = max(2, 1)
end = 2
```

`end` 2 hi rahega, kyunki pehle se index 2 tak jaana zaroori hai.

Condition:

```text
1 == 2 → False
```

Partition abhi complete nahi hai.

Current state:

```text
start = 0
end = 2
ans = []
```

### Iteration 3: i = 2

Current character:

```text
s[2] = 'a'
last[a] = 2
```

Calculation:

```text
end = max(2, 2)
end = 2
```

Ab condition:

```text
i == end
2 == 2 → True
```

Partition complete ho gaya.

Code:

```cpp
ans.push_back(i - start + 1);
```

Calculation:

```text
Length = 2 - 0 + 1
Length = 3
```

Ab:

```text
ans = [3]
```

Next partition ka starting index:

```cpp
start = i + 1;
start = 2 + 1;
start = 3;
```

Current state:

```text
start = 3
end = 2
ans = [3]
```

### Iteration 4: i = 3

Current character:

```text
s[3] = 'c'
last[c] = 3
```

Calculation:

```text
end = max(2, 3)
end = 3
```

Condition:

```text
3 == 3 → True
```

Partition complete ho gaya.

Length:

```text
3 - 3 + 1 = 1
```

Answer mein add karenge:

```text
ans = [3, 1]
```

### Final Output

```cpp
return ans;
```

Output:

```text
[3, 1]
```

### Dry Run Table

| i | Character | Character ka last index | end after update | `i == end` | Action |
|---:|---|---:|---:|---|---|
| 0 | a | 2 | 2 | False | Continue |
| 1 | b | 1 | 2 | False | Continue |
| 2 | a | 2 | 2 | True | Length 3 add karo |
| 3 | c | 3 | 3 | True | Length 1 add karo |

Final answer = `[3, 1]`.

---

## 7. Important Concept: `end = max(...)`

```cpp
end = max(end, last[s[i] - 'a']);
```

Ye is question ki sabse important line hai.

Har naye character ki last occurrence check karte hain.

Agar uska last occurrence current `end` se aage hai, toh partition ko aage extend karna padta hai.

Agar uska last occurrence current `end` se pehle ya equal hai, toh `end` same rehta hai.

Isse partition tabhi close hota hai jab usmein present saare characters ki last occurrences cover ho jaati hain.

---

## 8. `i == end` Kyun?

```cpp
if (i == end)
```

- `i < end` → Abhi current partition ki required ending tak nahi pahunche.
- `i == end` → Partition ki required ending tak pahunch gaye.
- Har partition mein `end` ko update karte rehte hain, isliye jab equality milti hai toh partition complete hota hai.

Partition ki length:

```cpp
i - start + 1
```

Phir next partition:

```cpp
start = i + 1;
```

---

## 9. Time and Space Complexity

### Time Complexity: O(n)

Pehla loop har character ka last index store karta hai: `O(n)`.

Doosra loop string ko ek baar traverse karta hai: `O(n)`.

Overall:

```text
O(n) + O(n) = O(n)
```

### Auxiliary Space Complexity: O(1)

`last` vector mein sirf 26 lowercase English letters ke indices store hote hain.

Isliye `last` ki space constant hai.

`ans` output vector hai, jisme partitions ki lengths store hoti hain. Output space ko exclude karne par auxiliary space `O(1)` hai.

---

## 10. Important Interview Points

1. **Pattern:** Greedy + String.
2. Har character ka last occurrence index store karte hain.
3. `last[s[i] - 'a']` character ka last index access karta hai.
4. `end = max(end, last[s[i] - 'a'])` partition ka required end update karta hai.
5. `i == end` hone par partition complete hota hai.
6. Partition length `i - start + 1` se calculate hoti hai.
7. Next partition `i + 1` se start hota hai.
8. Time complexity `O(n)` hai.

---

## 11. Common Mistakes

- Character ka first index store karna, last index nahi.
- `end` ko `max()` se update na karna.
- `i == end` ke bajaye galat condition use karna.
- Partition ki length calculate karte waqt `+1` bhool jaana.
- Partition complete hone ke baad `start = i + 1` update na karna.
- `last` array ko sirf 26 elements ke bajaye galat size dena, jab input lowercase English letters ka ho.

---

## 12. Final Takeaway

Is question ka main logic:

**Har character ka last index store karo → String traverse karo → `end` ko maximum last index se update karo → Jab `i == end`, partition close karo → Length answer mein add karo.**

Greedy idea ye hai ki partition ko jitna zaroori hai utna extend karo, aur jaise hi saare characters ki last occurrences cover ho jaayein, partition close kar do.
