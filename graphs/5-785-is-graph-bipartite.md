# LeetCode 785 — Is Graph Bipartite?

## 1. Problem Statement

Hume ek undirected graph diya hai.

Hume check karna hai ki graph ke saare nodes ko 2 groups ya colours mein divide kar sakte hain ya nahi.

Condition:

**Har connected pair ke nodes ka colour different hona chahiye.**

Hum 2 colours use karenge:

- `0` = Red
- `1` = Blue
- `-1` = Abhi colour assign nahi hua

Agar graph ko successfully 2 colours mein divide kar sakein, toh `true` return karo.

Agar kisi connected pair ka colour same ho jaaye, toh `false` return karo.


---

## 2. Example — True Case

Graph:

    0 ----- 1
    |       |
    |       |
    3 ----- 2

Is graph ko aise colour kar sakte hain:

    0 (Red) ----- 1 (Blue)
        |              |
        |              |
    3 (Blue) ----- 2 (Red)

Har connected pair ka colour different hai.

Output:

    true


---

## 3. Example — False Case

Graph:

        0
       / \
      1---2

Node 0 ko colour `0` denge.

Nodes 1 aur 2 ko colour `1` dena padega, kyunki dono node 0 se connected hain.

Lekin nodes 1 aur 2 bhi aapas mein connected hain.

Dono ka colour same hai, isliye conflict hai.

Output:

    false


---

## 4. Pattern

Graph Pattern 2 — Connected Components / Cycle Detection

Sub-pattern:

    Graph Coloring + BFS

Is question mein hum BFS se graph ke nodes ko alternate colours assign karenge.

Important concepts:

- BFS
- Queue
- Colour array
- Graph Coloring
- Conflict Detection
- Disconnected Components


---

## 5. Approach

### Step 1: Colour Array Banao

    vector<int> color(n, -1);

Initially sabhi nodes uncoloured hain.

Example:

    color = [-1, -1, -1, -1]

### Step 2: Har Uncoloured Node Se BFS Start Karo

    color[i] = 0;
    q.push(i);

Starting node ko colour `0` do aur queue mein daalo.

Outer loop zaroori hai kyunki graph disconnected ho sakta hai.

### Step 3: Queue Se Node Nikalo

    int node = q.front();
    q.pop();

Ab current node ke neighbours check karenge.

### Step 4: Neighbours Check Karo

Agar neighbour uncoloured hai:

    color[neighbour] = 1 - color[node];
    q.push(neighbour);

Matlab neighbour ko opposite colour do aur queue mein daalo.

### Step 5: Same Colour Conflict Check Karo

Agar connected neighbour ka colour current node ke colour ke same hai:

    return false;

Graph bipartite nahi hai.

### Step 6: Final Answer

Agar saare components process ho gaye aur koi conflict nahi mila:

    return true;


---

## 6. Complete C++ Code

    class Solution {
    public:
        bool isBipartite(vector<vector<int>>& graph) {

            int n = graph.size();

            vector<int> color(n, -1);

            queue<int> q;

            for (int i = 0; i < n; i++) {

                if (color[i] != -1) {
                    continue;
                }

                color[i] = 0;
                q.push(i);

                while (!q.empty()) {

                    int node = q.front();
                    q.pop();

                    for (int neighbour : graph[node]) {

                        if (color[neighbour] == -1) {

                            color[neighbour] = 1 - color[node];

                            q.push(neighbour);
                        }
                        else if (color[neighbour] == color[node]) {

                            return false;
                        }
                    }
                }
            }

            return true;
        }
    };


---

## 7. Code Explanation

### A. Number of Nodes

    int n = graph.size();

`n` total nodes ki count hai.

### B. Colour Array

    vector<int> color(n, -1);

Har node ka colour store hota hai.

    -1 = Uncoloured
     0 = Red
     1 = Blue

Initially:

    color = [-1, -1, -1, -1]

### C. Queue

    queue<int> q;

BFS mein current nodes ki positions store hoti hain.

Example:

    q.push(0);

Node 0 ko queue mein daalo.

### D. Outer Loop

    for (int i = 0; i < n; i++)

Har node ko check karta hai.

    if (color[i] != -1) {
        continue;
    }

Agar node already coloured hai, toh use dobara process start karne ki zarurat nahi.

Agar uncoloured hai, toh naya BFS start karenge.

Ye disconnected graphs ko handle karta hai.

### E. Starting Node Ko Colour Do

    color[i] = 0;
    q.push(i);

Starting node ko colour 0 diya aur queue mein daala.

### F. Queue Se Node Nikaalo

    int node = q.front();
    q.pop();

`q.front()` queue ka pehla node deta hai.

`q.pop()` us node ko queue se hata deta hai.

### G. Neighbours Check Karo

    for (int neighbour : graph[node])

Current node ke saare connected neighbours ko one by one check karte hain.

### H. Opposite Colour Assign Karna

    if (color[neighbour] == -1) {

        color[neighbour] = 1 - color[node];

        q.push(neighbour);
    }

Agar neighbour uncoloured hai, toh opposite colour assign karo.

Formula:

    Current colour = 0
    1 - 0 = 1

    Current colour = 1
    1 - 1 = 0

Isliye `1 - color[node]` opposite colour return karta hai.

Phir neighbour ko queue mein daal dete hain.

### I. Same Colour Check

    else if (color[neighbour] == color[node]) {

        return false;
    }

Agar connected neighbour ka colour same hai, toh graph ko 2 valid groups mein divide nahi kar sakte.

Isliye false return kar dete hain.

### J. Final Return

    return true;

Agar saare nodes process ho gaye aur koi colour conflict nahi mila, toh graph bipartite hai.


---

## 8. Detailed Dry Run — True Case

Input:

    graph = [
        [1,3],
        [0,2],
        [1,3],
        [0,2]
    ]

Graph:

    0 ----- 1
    |       |
    |       |
    3 ----- 2

Initially:

    color = [-1, -1, -1, -1]

    queue = []


### Step 1: Node 0

Node 0 uncoloured hai.

    color[0] = 0;
    q.push(0);

Ab:

    color = [0, -1, -1, -1]
    queue = [0]


### Step 2: Node 0 Process Karo

Queue se node 0 niklega.

Uske neighbours:

    1, 3

Dono uncoloured hain, toh dono ko opposite colour 1 milega.

    color = [0, 1, -1, 1]
    queue = [1, 3]


### Step 3: Node 1 Process Karo

Queue:

    [1, 3]

Node 1 ke neighbours:

    0, 2

Node 0 already coloured hai aur uska colour 0 hai.

Node 2 uncoloured hai.

Isliye:

    color[2] = 1 - color[1]
             = 1 - 1
             = 0

Ab:

    color = [0, 1, 0, 1]
    queue = [3, 2]


### Step 4: Node 3 Process Karo

Node 3 ke neighbours:

    0, 2

Dono ka colour 0 hai, aur node 3 ka colour 1 hai.

Koi conflict nahi hai.


### Step 5: Node 2 Process Karo

Node 2 ke neighbours:

    1, 3

Dono ka colour 1 hai, aur node 2 ka colour 0 hai.

Koi conflict nahi hai.

Queue empty ho jaati hai.

Final:

    color = [0, 1, 0, 1]

    return true;

Output:

    true


---

## 9. False Case — Triangle Graph

Input:

    graph = [
        [1,2],
        [0,2],
        [0,1]
    ]

Graph:

        0
       / \
      1---2

Initially:

    color = [-1, -1, -1]


### Step 1

Node 0 ko colour 0 do.

    color = [0, -1, -1]

### Step 2

Node 0 ke neighbours 1 aur 2 hain.

Dono ko colour 1 milega.

    color = [0, 1, 1]

### Step 3

Ab node 1 process hoga.

Uska neighbour node 2 bhi colour 1 rakhta hai.

Check:

    color[2] == color[1]

Dono equal hain, toh conflict mil gaya.

    return false;

Output:

    false


---

## 10. Important Difference — BFS vs DFS

Dono approaches is problem ko solve kar sakti hain.

### BFS

    Queue use hoti hai.

    Nodes level-by-level process hote hain.

### DFS

    Recursion ya stack use hota hai.

    Ek path ko depth mein explore karta hai.

Is question mein humne BFS choose kiya hai, kyunki queue-based colouring approach beginner ke liye clear ho sakti hai.


---

## 11. Common Mistakes

### Mistake 1: Opposite Colour Galat Assign Karna

Correct:

    color[neighbour] = 1 - color[node];

### Mistake 2: Same Colour Conflict Check Bhoolna

Correct:

    else if (color[neighbour] == color[node]) {
        return false;
    }

### Mistake 3: Disconnected Graph Ko Ignore Karna

Outer loop sabhi nodes check karta hai.

    for (int i = 0; i < n; i++)

Har uncoloured component se BFS start karna zaroori hai.

### Mistake 4: Queue Mein Neighbour Add Nahi Karna

Colour assign karne ke baad:

    q.push(neighbour);

Ye zaroori hai, taaki neighbour ke connections bhi process hon.


---

## 12. Complexity Analysis

Let:

    V = number of vertices
    E = number of edges

### Time Complexity

    O(V + E)

BFS mein har node aur edge maximum constant number of times process hota hai.

### Space Complexity

    O(V)

Colour array aur queue ke liye.


---

## 13. Interview Explanation

"I will use BFS with a color array. Initially, all nodes are uncolored. For every uncolored node, I assign color 0 and start BFS. Whenever I find an uncolored neighbour, I assign it the opposite color and add it to the queue. If an already-colored neighbour has the same color as the current node, I return false. If all components are processed without any conflict, I return true."


---

## 14. Key Takeaways

1. Bipartite graph ko 2 groups mein divide kiya ja sakta hai.
2. Connected nodes ka colour different hona chahiye.
3. `-1` means uncoloured.
4. `1 - color[node]` opposite colour deta hai.
5. Same-colour connected nodes milne par answer false hai.
6. Disconnected graph ke liye outer loop zaroori hai.
7. BFS aur DFS dono se solve kar sakte hain.

Core pattern:

    Assign colour
         |
         v
    Process neighbours
         |
         v
    Uncoloured?
      /     \
    Yes      No
     |        |
    Give     Same colour?
   opposite     |
   colour     Yes -> false
     |
     v
  Push queue
     |
     v
 Continue BFS
     |
     v
 No conflict -> true


---

## 15. Graph Roadmap Progress

### Pattern 1 — Foundation / Traversal

- LC 1971 — Find if Path Exists ✅
- LC 200 — Number of Islands ✅
- LC 994 — Rotting Oranges ✅

### Pattern 2 — Connected Components / Cycle Detection

- LC 547 — Number of Provinces ✅
- LC 785 — Is Graph Bipartite? ✅

Next question:

**LC 684 — Redundant Connection**

Pattern: Cycle Detection + DSU (Disjoint Set Union)
