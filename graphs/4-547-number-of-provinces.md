# LeetCode 547 — Number of Provinces

## 1. Problem Statement

Hume `n` cities di gayi hain aur ek matrix `isConnected` diya hai.

- `isConnected[i][j] == 1` means city `i` aur city `j` directly connected hain.
- `isConnected[i][j] == 0` means dono directly connected nahi hain.

Ek province un cities ka group hota hai jo directly ya indirectly connected ho sakti hain.

Hume total provinces count karne hain.

### Example

Input:

    isConnected = [
        [1,1,0],
        [1,1,0],
        [0,0,1]
    ]

Graph:

    City 0 ----- City 1

    City 2

Cities `0` aur `1` connected hain, jabki city `2` alag hai.

Output:

    2

Explanation:

    Province 1 = Cities 0, 1
    Province 2 = City 2


---

## 2. Pattern

**Graph Pattern 2 — Connected Components**

Sub-pattern:

    DFS + Connected Components

Is problem mein hume graph ke alag-alag connected groups count karne hain.

Ye Number of Islands jaisa hi concept hai.

Number of Islands mein grid thi, aur yahan adjacency matrix hai.


---

## 3. Important Concepts

### Adjacency Matrix

Example:

    1 1 0
    1 1 0
    0 0 1

Iska matlab:

- City 0, city 1 se connected hai.
- City 1, city 0 se connected hai.
- City 2 kisi doosri city se connected nahi hai.

Matrix mein row current city ko represent karti hai aur column doosri city ko.

Example:

    isConnected[0][1] == 1

Matlab city 0 aur city 1 connected hain.


### Visited Array

    vector<bool> visited(n, false);

Initially koi city visit nahi hui.

Example:

    visited = [false, false, false]

Jab city visit hoti hai:

    visited[city] = true;

Isse same city baar-baar visit nahi hoti.


---

## 4. Approach

Hum DFS use karenge.

### Step 1: Visited Array Banao

    vector<bool> visited(n, false);

Har city initially unvisited hai.


### Step 2: Provinces Count Karo

    int provinces = 0;

Initially koi province count nahi hua.


### Step 3: Har City Check Karo

    for (int i = 0; i < n; i++)

Har city ko check karenge.

Agar city already visited hai, toh skip kar do.

Agar city unvisited hai, iska matlab ek naya province mila.

    provinces++;

Phir DFS chalao:

    dfs(i, isConnected, visited);


### Step 4: DFS Se Connected Cities Visit Karo

DFS current city ko visited mark karega.

Phir uske saare connections check karega.

Agar koi city connected hai aur visited nahi hai, toh us city par DFS call karega.

Isse ek province ki saari connected cities visit ho jayengi.


---

## 5. Complete C++ Code

    class Solution {
    public:

        void dfs(int city, vector<vector<int>>& isConnected,
                 vector<bool>& visited) {

            visited[city] = true;

            for (int i = 0; i < isConnected.size(); i++) {

                if (isConnected[city][i] == 1 &&
                    !visited[i]) {

                    dfs(i, isConnected, visited);
                }
            }
        }

        int findCircleNum(vector<vector<int>>& isConnected) {

            int n = isConnected.size();

            int provinces = 0;

            vector<bool> visited(n, false);

            for (int i = 0; i < n; i++) {

                if (!visited[i]) {

                    provinces++;

                    dfs(i, isConnected, visited);
                }
            }

            return provinces;
        }
    };


---

## 6. Code Explanation

### A. DFS Function

    void dfs(int city, vector<vector<int>>& isConnected,
             vector<bool>& visited)

`city` current city hai jahan DFS chal raha hai.

`isConnected` matrix connections store karti hai.

`visited` batata hai ki kaunsi cities already visit ho chuki hain.


### B. Current City Ko Visited Mark Karna

    visited[city] = true;

Maan lo:

    dfs(0, isConnected, visited);

Ab city 0 visited ho jayegi.

    visited = [true, false, false]

Ye line important hai kyunki graph mein cycles ho sakti hain.


### C. Connections Check Karna

    for (int i = 0; i < isConnected.size(); i++)

Agar 3 cities hain, toh `i` ki values hongi:

    i = 0
    i = 1
    i = 2

Matlab current city ke connections sabhi cities ke saath check karo.


### D. Connection Aur Visited Check

    if (isConnected[city][i] == 1 && !visited[i])

Ismein do conditions hain:

1. `isConnected[city][i] == 1`
   - Current city aur city `i` connected honi chahiye.

2. `!visited[i]`
   - City `i` abhi visit nahi hui honi chahiye.

Dono conditions true hongi tabhi DFS aage chalega.


### E. Recursive Call

    dfs(i, isConnected, visited);

Yahan `i` use karna important hai.

`city` current city hai.

`i` connected neighbour city hai.

Isliye neighbour unvisited milne par usi par DFS call karenge.


### F. Outer Loop

    for (int i = 0; i < n; i++) {

        if (!visited[i]) {

            provinces++;

            dfs(i, isConnected, visited);
        }
    }

Ye poore graph ki saari cities scan karta hai.

Agar koi city unvisited hai, toh ek new province mila.

Isliye:

    provinces++;

Phir DFS us province ki saari connected cities visit karta hai.


---

## 7. Detailed Dry Run

Input:

    isConnected = [
        [1,1,0],
        [1,1,0],
        [0,0,1]
    ]

Graph:

    City 0 ----- City 1

    City 2


### Initial State

    visited = [false, false, false]

    provinces = 0


### Step 1: City 0

Outer loop:

    i = 0

City 0 unvisited hai.

    provinces++;

Ab:

    provinces = 1

Call:

    dfs(0, isConnected, visited);


### Step 2: DFS City 0

Current city:

    city = 0

Mark visited:

    visited = [true, false, false]

Ab city 0 ki connections check hongi.


First:

    i = 0

    isConnected[0][0] = 1

Lekin city 0 already visited hai.

Isliye skip.


Next:

    i = 1

    isConnected[0][1] = 1

City 1 connected hai aur unvisited hai.

Isliye:

    dfs(1, isConnected, visited);


### Step 3: DFS City 1

City 1 visited mark hogi.

    visited = [true, true, false]

City 1 ke connections check honge.

City 0 already visited hai, isliye skip.

City 1 khud bhi visited hai, isliye skip.

City 2 se connection nahi hai.

DFS city 1 complete ho jayegi.

Control DFS city 0 par return karega.

Ab city 0 ka DFS bhi complete ho jayega.


### Step 4: Outer Loop City 1

Ab outer loop:

    i = 1

City 1 already visited hai.

Isliye skip.

`provinces++` nahi hoga.

Abhi:

    provinces = 1


### Step 5: Outer Loop City 2

Ab:

    i = 2

City 2 unvisited hai.

Isliye ek naya province mila.

    provinces++;

Ab:

    provinces = 2

Call:

    dfs(2, isConnected, visited);


City 2 visited ho jayegi.

    visited = [true, true, true]

City 2 kisi aur city se connected nahi hai.

Isliye DFS complete.


### Final Answer

Outer loop complete hone ke baad:

    return provinces;

Output:

    2


---

## 8. Most Important Logic

Pura question mainly is code par based hai:

    for (int i = 0; i < n; i++) {

        if (!visited[i]) {

            provinces++;

            dfs(i, isConnected, visited);
        }
    }

Iska meaning:

1. Har city ko check karo.
2. Unvisited city mili toh new province count karo.
3. DFS se uski saari connected cities visit karo.
4. Agli unvisited city par jao.

Ek province mein multiple cities ho sakti hain, lekin count sirf ek baar increase hota hai.


---

## 9. Common Mistakes

### Mistake 1: Wrong Recursive Call

Wrong:

    dfs(city, isConnected, visited);

Correct:

    dfs(i, isConnected, visited);

Neighbour city `i` par aage jaana hai, same city par nahi.


### Mistake 2: DFS Ke Andar Provinces Count Karna

`provinces++` outer loop mein hona chahiye.

Kyunki ek province mein multiple connected cities ho sakti hain.


### Mistake 3: Visited Mark Karna Bhoolna

    visited[city] = true;

Ye repeated traversal aur infinite recursion se bachata hai.


### Mistake 4: Already Visited City Ko Dobara Count Karna

    if (!visited[i])

Isi condition se pata chalta hai ki city already visit hui hai ya nahi.


---

## 10. Complexity Analysis

Let:

    n = number of cities

### Time Complexity

    O(n²)

Adjacency matrix mein har city ke liye sabhi cities ke connections check kar sakte hain.

### Space Complexity

    O(n)

Visited array aur maximum DFS recursion stack ke liye.


---

## 11. Interview Explanation

"I will use DFS to find connected components. First, I create a visited array. Then I iterate through every city. Whenever I find an unvisited city, I increment the province count and perform DFS from that city to visit all cities connected to it, directly or indirectly. Already visited cities are skipped, so each province is counted exactly once."


---

## 12. Key Takeaways

1. Province ka matlab connected cities ka group hai.
2. Adjacency matrix connections represent karti hai.
3. Visited array repeated traversal prevent karti hai.
4. Outer loop new provinces count karta hai.
5. DFS ek province ki saari connected cities visit karta hai.
6. `dfs(i, ...)` neighbour city par recursion karta hai.
7. Time complexity `O(n²)` hai.

Core pattern:

    Find unvisited city
            |
            v
       provinces++
            |
            v
        Run DFS
            |
            v
    Visit connected cities
            |
            v
      Continue scanning


---

## 13. Graph Roadmap Progress

### Pattern 1 — Foundation / Traversal

- LC 1971 — Find if Path Exists in Graph ✅
- LC 200 — Number of Islands ✅
- LC 994 — Rotting Oranges ✅

### Pattern 2 — Connected Components / Cycle Detection

- LC 547 — Number of Provinces ✅
- LC 785 — Is Graph Bipartite? ⏳
- LC 684 — Redundant Connection ⏳

Next question: **LC 785 — Is Graph Bipartite?**
