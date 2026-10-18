# Greedy Extra #4 — LeetCode 2410: Maximum Matching of Players With Trainers

## 1. Problem Statement

Humein do integer arrays diye gaye hain:

- `players[i]` — ith player ki ability.
- `trainers[j]` — jth trainer ki capacity.

Ek trainer kisi player ko tabhi train kar sakta hai jab trainer ki capacity player ki ability ke equal ya greater ho.

Rules:
- Ek player ko maximum ek trainer mil sakta hai.
- Ek trainer maximum ek player ko train kar sakta hai.
- Humein maximum possible player-trainer matches find karne hain.

### Example

Input:

```cpp
players = [4, 7, 9];
trainers = [8, 2, 5, 8];
```

Output:

```cpp
2
```

Explanation:

Pehle dono arrays ko ascending order mein sort karenge.

```text
Players:  [4, 7, 9]
Trainers: [2, 5, 8, 8]
```

Matching:

- Player 4 ko Trainer 2 nahi mil sakta, kyunki capacity insufficient hai.
- Player 4 ko Trainer 5 mil sakta hai.
- Player 7 ko Trainer 8 mil sakta hai.
- Player 9 ko remaining Trainer 8 nahi mil sakta.

Total matches = 2.

---

## 2. Pattern

**Greedy + Sorting + Two Pointers**

Is problem mein hum sorted arrays par do pointers use karte hain.

- `i` current player ko point karta hai.
- `j` current trainer ko point karta hai.
- `count` successful matches store karta hai.

## 3. Approach

### Step 1: Dono arrays sort karo

```cpp
sort(players.begin(), players.end());
sort(trainers.begin(), trainers.end());
```

Ascending order mein sort karne se players aur trainers dono smallest se largest order mein aa jaate hain.

Example:

```text
Players before sorting:  [9, 4, 7]
Players after sorting:   [4, 7, 9]

Trainers before sorting: [8, 2, 5, 8]
Trainers after sorting:  [2, 5, 8, 8]
```

### Step 2: Pointers initialize karo

```cpp
int i = 0;
int j = 0;
int count = 0;
```

- `i = 0` — first player.
- `j = 0` — first trainer.
- `count = 0` — abhi koi match nahi hua.

### Step 3: While loop chalao

```cpp
while (i < players.size() && j < trainers.size())
```

Loop tab tak chalega jab tak dono arrays mein unprocessed elements available hain.

### Step 4: Match check karo

```cpp
if (players[i] <= trainers[j])
```

Agar player ki ability trainer ki capacity se kam ya equal hai, toh match possible hai.

```cpp
i++;
j++;
count++;
```

- `i++` — next player par jao.
- `j++` — next trainer par jao.
- `count++` — ek successful match record karo.

### Step 5: Agar trainer insufficient hai

```cpp
else {
    j++;
}
```

Agar current player ki ability trainer ki capacity se zyada hai, toh current trainer us player ko train nahi kar sakta.

Isliye sirf `j++` karenge aur next trainer check karenge.

Player ko change nahi karenge, kyunki use abhi tak trainer nahi mila hai.

### Step 6: Final answer

```cpp
return count;
```

Maximum successful matches ka count return hoga.

---

## 4. C++ Code

```cpp
class Solution {
public:
    int matchPlayersAndTrainers(vector<int>& players, vector<int>& trainers) {
        sort(players.begin(), players.end());
        sort(trainers.begin(), trainers.end());

        int i = 0;
        int j = 0;
        int count = 0;

        while (i < players.size() && j < trainers.size()) {
            if (players[i] <= trainers[j]) {
                i++;
                j++;
                count++;
            }
            else {
                j++;
            }
        }

        return count;
    }
};
```

---

## 5. Code Explanation

### A. Sorting

```cpp
sort(players.begin(), players.end());
sort(trainers.begin(), trainers.end());
```

Dono arrays ko ascending order mein arrange karte hain.

Sorting se hum smallest available trainer ko pehle check kar sakte hain.

### B. Variables

```cpp
int i = 0;
int j = 0;
int count = 0;
```

`i` player ki position track karta hai.

`j` trainer ki position track karta hai.

`count` total matches calculate karta hai.

### C. While loop

```cpp
while (i < players.size() && j < trainers.size())
```

Jab tak player aur trainer dono available hain, matching continue rahegi.

### D. Successful match

```cpp
if (players[i] <= trainers[j]) {
    i++;
    j++;
    count++;
}
```

Agar trainer ki capacity sufficient hai, toh player ko trainer assign kar dete hain.

Dono ko aage badhate hain kyunki ek player aur ek trainer sirf ek baar use ho sakte hain.

### E. Insufficient trainer

```cpp
else {
    j++;
}
```

Current trainer ki capacity kam hai.

Sorted order ki wajah se woh trainer current player ko train nahi kar sakta, isliye next trainer check karte hain.

Player pointer `i` same rehta hai.

### F. Return

```cpp
return count;
```

Loop khatam hone par maximum matches return hote hain.

---

## 6. Detailed Dry Run

Input:

```cpp
players = [4, 7, 9];
trainers = [8, 2, 5, 8];
```

### Step 1: Sorting

```text
Players:
[4, 7, 9]

Trainers:
[2, 5, 8, 8]
```

Initially:

```text
i = 0
j = 0
count = 0
```

### Iteration 1

Current player:

```text
players[i] = players[0] = 4
```

Current trainer:

```text
trainers[j] = trainers[0] = 2
```

Condition:

```cpp
players[i] <= trainers[j]
```

Matlab:

```text
4 <= 2 → False
```

Trainer ki capacity insufficient hai.

Isliye `else` execute hoga:

```cpp
j++;
```

Ab:

```text
i = 0
j = 1
count = 0
```

Player 4 ko abhi trainer nahi mila.

### Iteration 2

Current player:

```text
players[0] = 4
```

Current trainer:

```text
trainers[1] = 5
```

Condition:

```text
4 <= 5 → True
```

Match possible hai.

```cpp
i++;
j++;
count++;
```

Ab:

```text
i = 1
j = 2
count = 1
```

Player 4 aur Trainer 5 ka match complete ho gaya.

### Iteration 3

Current player:

```text
players[1] = 7
```

Current trainer:

```text
trainers[2] = 8
```

Condition:

```text
7 <= 8 → True
```

Match possible hai.

```cpp
i++;
j++;
count++;
```

Ab:

```text
i = 2
j = 3
count = 2
```

Player 7 aur Trainer 8 ka match complete ho gaya.

### Iteration 4

Current player:

```text
players[2] = 9
```

Current trainer:

```text
trainers[3] = 8
```

Condition:

```text
9 <= 8 → False
```

Trainer ki capacity insufficient hai.

Isliye:

```cpp
j++;
```

Ab:

```text
i = 2
j = 4
count = 2
```

Ab `j = 4`, lekin trainers ka size 4 hai.

Condition:

```cpp
j < trainers.size()
```

Matlab:

```text
4 < 4 → False
```

Loop stop ho jayega.

### Final Output

```cpp
return 2;
```

Output:

```text
2
```

### Dry Run Table

| Step | Player | Trainer | Decision | i | j | Count |
|---:|---:|---:|---|---:|---:|---:|
| 1 | 4 | 2 | Trainer insufficient | 0 | 1 | 0 |
| 2 | 4 | 5 | Match | 1 | 2 | 1 |
| 3 | 7 | 8 | Match | 2 | 3 | 2 |
| 4 | 9 | 8 | Trainer insufficient | 2 | 4 | 2 |

Final answer = `2`.

---

## 7. Greedy Logic — Why Does It Work?

Hum sorted order mein har player ke liye sabse chhota sufficient trainer use karte hain.

Agar player ki ability 4 hai aur available trainers ki capacity 5 aur 10 hai, toh capacity 5 wala trainer choose karna better hai.

Capacity 10 wala trainer future mein kisi stronger player ko kaam aa sakta hai.

Isi tarah:
- Insufficient trainer ko skip karte hain.
- Match milne par dono ko assign karke aage badhte hain.
- Isse available trainers ka efficient use hota hai aur maximum matching milti hai.

---

## 8. Time and Space Complexity

Let:
- `n` = number of players.
- `m` = number of trainers.

### Time Complexity: O(n log n + m log m)

Players ko sort karne mein `O(n log n)` time lagta hai.

Trainers ko sort karne mein `O(m log m)` time lagta hai.

Two-pointer loop maximum `O(n + m)` time leta hai.

Overall:

```text
O(n log n + m log m + n + m)
```

Sorting dominant hone ki wajah se:

```text
O(n log n + m log m)
```

### Auxiliary Space Complexity

Dono arrays ke alawa humne koi extra vector ya data structure nahi banaya.

Typical `std::sort` implementation recursion stack use kar sakti hai, isliye sorting ki auxiliary space implementation par depend karti hai. Typical implementations mein yeh logarithmic stack space hoti hai.

---

## 9. Important Interview Points

1. **Pattern:** Greedy + Sorting + Two Pointers.
2. Dono arrays ko ascending order mein sort karte hain.
3. `i` player aur `j` trainer ko track karta hai.
4. Player ki ability `<=` trainer capacity ho, toh match possible hai.
5. Match hone par `i++`, `j++` aur `count++`.
6. Trainer insufficient ho, toh sirf `j++`.
7. `while` loop tab tak chalta hai jab tak players aur trainers dono available hain.
8. Har player aur trainer maximum ek matching mein use hota hai.

## 10. Common Mistakes

- Dono arrays ko sort karna bhool jaana.
- Trainer insufficient hone par `i++` bhi kar dena.
- Successful match par kisi ek pointer ko hi move karna.
- Equality ko reject karna. Agar player ability aur trainer capacity equal hain, toh match valid hai.
- While loop ki condition mein `&&` ki jagah `||` use karna.

## 11. Final Takeaway

Is question ka core logic:

**Sort both arrays → Two pointers → Trainer insufficient toh sirf trainer pointer aage → Match possible toh dono pointers aage aur count increase.**

Yeh pattern tab use hota hai jab do sorted groups ke elements ko constraints ke basis par efficiently match karna ho.
