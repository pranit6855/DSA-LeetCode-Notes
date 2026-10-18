# Greedy Extra #2 — LeetCode 2126: Destroying Asteroids

## 1. Problem Statement

Humein spaceship ki initial `mass` aur `asteroids` naam ka integer array diya gaya hai.

Har asteroid ki apni mass hoti hai.

Rules:
- Agar spaceship ki current mass asteroid ki mass ke equal ya greater hai, toh spaceship us asteroid ko destroy kar sakti hai.
- Asteroid destroy hone ke baad uski mass spaceship ki mass mein add ho jaati hai.
- Humein check karna hai ki spaceship saare asteroids destroy kar sakti hai ya nahi.

Agar saare asteroids destroy ho sakte hain, toh `true` return karo.
Agar koi asteroid destroy nahi kar sakte, toh `false` return karo.

### Example

Input:

```cpp
mass = 5;
asteroids = [4, 9, 23, 4];
```

Output:

```cpp
false
```

Explanation:

Asteroids ko ascending order mein sort karenge:

```text
[4, 4, 9, 23]
```

Ab process dekho:

- Initial mass = 5
- Asteroid 4 destroy → mass = 9
- Asteroid 4 destroy → mass = 13
- Asteroid 9 destroy → mass = 22
- Asteroid 23 destroy nahi kar sakte, kyunki mass sirf 22 hai.

Isliye answer `false` hai.

---

## 2. Pattern

**Greedy + Sorting**

Is question mein hum chhote asteroids ko pehle destroy karte hain, taaki spaceship ki mass jaldi increase ho sake.

## 3. Greedy Approach

### Step 1: Asteroids ko sort karo

```cpp
sort(asteroids.begin(), asteroids.end());
```

Isse asteroids ascending order mein aa jayenge.

Example:

```text
Before sorting: [4, 9, 23, 4]
After sorting:  [4, 4, 9, 23]
```

### Step 2: Current mass maintain karo

```cpp
long long currMass = mass;
```

`currMass` spaceship ki current mass store karega.

`long long` use kiya hai, kyunki saare asteroids ki mass add hone par total `int` ki range se bada ho sakta hai.

### Step 3: Har asteroid check karo

```cpp
if (currMass < asteroids[i]) {
    return false;
}
```

Agar spaceship ki current mass asteroid se kam hai, toh woh asteroid destroy nahi kar sakti.

Isliye turant `false` return kar do.

### Step 4: Asteroid ki mass add karo

```cpp
currMass += asteroids[i];
```

Agar spaceship asteroid destroy kar sakti hai, toh us asteroid ki mass spaceship ki mass mein add ho jayegi.

### Step 5: Saare asteroids destroy ho jaayein

Agar loop complete ho gaya, toh saare asteroids destroy ho gaye.

```cpp
return true;
```

---

## 4. C++ Code

```cpp
class Solution {
public:
    bool asteroidsDestroyed(int mass, vector<int>& asteroids) {
        sort(asteroids.begin(), asteroids.end());

        long long currMass = mass;

        for (int i = 0; i < asteroids.size(); i++) {
            if (currMass < asteroids[i]) {
                return false;
            }

            currMass += asteroids[i];
        }

        return true;
    }
};
```

---

## 5. Code Explanation

### Sorting

```cpp
sort(asteroids.begin(), asteroids.end());
```

Asteroids ko smallest se largest order mein arrange karta hai.

### Current mass

```cpp
long long currMass = mass;
```

Starting spaceship mass ko `currMass` mein store karte hain.

### Loop

```cpp
for (int i = 0; i < asteroids.size(); i++)
```

Har asteroid ko ek-ek karke process karta hai.

### Condition check

```cpp
if (currMass < asteroids[i]) {
    return false;
}
```

Agar current mass asteroid ki mass se kam hai, toh asteroid destroy nahi ho sakta.

### Mass increase

```cpp
currMass += asteroids[i];
```

Asteroid destroy hone par uski mass spaceship ki mass mein add hoti hai.

### Final answer

```cpp
return true;
```

Agar loop complete ho gaya, toh spaceship ne saare asteroids destroy kar diye.

---

## 6. Detailed Dry Run

Input:

```cpp
mass = 5;
asteroids = [4, 9, 23, 4];
```

### Step 1: Sorting

```text
Original array:
[4, 9, 23, 4]

Sorted array:
[4, 4, 9, 23]
```

Initially:

```text
currMass = 5
```

### Step 2: Iteration 1

```text
i = 0
asteroids[0] = 4
currMass = 5
```

Condition:

```cpp
if (currMass < asteroids[i])
```

Matlab:

```text
5 < 4 → False
```

Spaceship asteroid destroy kar sakti hai.

```cpp
currMass += asteroids[i];
```

Calculation:

```text
currMass = 5 + 4
currMass = 9
```

### Step 3: Iteration 2

```text
i = 1
asteroids[1] = 4
currMass = 9
```

Condition:

```text
9 < 4 → False
```

Asteroid destroy kar sakte hain.

```text
currMass = 9 + 4
currMass = 13
```

### Step 4: Iteration 3

```text
i = 2
asteroids[2] = 9
currMass = 13
```

Condition:

```text
13 < 9 → False
```

Asteroid destroy kar sakte hain.

```text
currMass = 13 + 9
currMass = 22
```

### Step 5: Iteration 4

```text
i = 3
asteroids[3] = 23
currMass = 22
```

Condition:

```text
22 < 23 → True
```

Spaceship ki mass 22 hai, lekin asteroid ki mass 23 hai.

Spaceship asteroid destroy nahi kar sakti.

```cpp
return false;
```

Function yahin stop ho jayega.

### Final Output

```text
false
```

### Dry Run Table

| i | Asteroid | Current Mass | Destroy? | Mass After Destruction |
|---:|---:|---:|---|---:|
| 0 | 4 | 5 | Yes | 9 |
| 1 | 4 | 9 | Yes | 13 |
| 2 | 9 | 13 | Yes | 22 |
| 3 | 23 | 22 | No | — |

---

## 7. Time and Space Complexity

### Time Complexity: O(n log n)

Sorting mein `O(n log n)` time lagta hai.

Asteroids par loop chalane mein `O(n)` time lagta hai.

Overall:

```text
O(n log n) + O(n) = O(n log n)
```

### Auxiliary Space Complexity: O(log n)

Typical in-place sorting implementation mein recursion stack ke liye `O(log n)` auxiliary space lag sakti hai. Actual usage sorting implementation par depend karti hai.

---

## 8. Important Interview Points

1. **Pattern:** Greedy + Sorting.
2. Chhote asteroids ko pehle destroy karte hain.
3. Asteroid destroy hone ke baad uski mass spaceship ki mass mein add hoti hai.
4. Agar current mass asteroid ki mass se kam hai, toh `false` return karte hain.
5. `long long` overflow se bachne ke liye use kiya hai.
6. Agar saare asteroids destroy ho jaate hain, toh `true` return hota hai.

## 9. Common Mistakes

- Asteroids ko sort nahi karna.
- Asteroid destroy hone ke baad uski mass add karna bhool jaana.
- Condition mein `currMass <= asteroids[i]` likhna. Equal mass hone par bhi asteroid destroy kar sakte hain.
- Final `return true` bhool jaana.
- Total mass ke liye `int` use karna, jab accumulated mass bahut badi ho sakti hai.

## 10. Final Takeaway

Is question ka main logic:

**Sort → Current mass check → Asteroid destroy karo → Mass increase karo → Result return karo.**

Greedy strategy ka idea hai chhote asteroids se mass badhana, taaki aage ke bade asteroids destroy kar sakein.
