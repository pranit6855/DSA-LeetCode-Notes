# LeetCode 684 — Redundant Connection

## 1. Problem Statement

Hume ek undirected graph diya hai jo initially ek tree tha.

Tree mein koi cycle nahi hoti. Lekin ek extra edge add kar di gayi, jiski wajah se graph mein cycle ban gayi.

Hume woh extra edge return karni hai jise remove karne se cycle khatam ho jaaye.

Agar multiple valid edges possible hon, toh input mein sabse last aane wali valid edge return karni hai.

### Example

Input:

    edges = [[1,2], [1,3], [2,3]]

Graph:

        1
       / \
      2---3

Yahan edges hain:

    [1,2]
    [1,3]
    [2,3]

Last edge [2,3] add hone se cycle complete hoti hai.

Output:

    [2,3]


---

## 2. Pattern

Graph Pattern 2 — Connected Components / Cycle Detection

Sub-pattern:

    DSU — Disjoint Set Union
    Union-Find
    Cycle Detection

DSU ka use karke hum check kar sakte hain ki do nodes pehle se ek hi group mein connected hain ya nahi.

Agar dono nodes already same group mein hain, toh unke beech ek aur edge add karne se cycle banegi.


---

## 3. DSU Kya Hai?

DSU ka full form hai:

    Disjoint Set Union

Iska kaam hai nodes ke groups ko manage karna.

Initially har node apne separate group mein hota hai.

Example:

    1       2       3

Yahan 3 separate groups hain.

Ab edge [1,2] add hoti hai:

    1 ----- 2       3

Ab nodes 1 aur 2 ek group mein hain.

Phir edge [1,3] add hoti hai:

        1
       / \
      2   3

Ab teeno nodes ek group mein hain.


---

## 4. DSU Ke 2 Important Operations

### Operation 1 — Find

`find(node)` node ke group ka root return karta hai.

Root group ka representative node hota hai.

Example:

    1 → 2 → 3

Yahan:

    find(1) = 3
    find(2) = 3
    find(3) = 3

Teeno ka root 3 hai, matlab teeno same group mein hain.

### Operation 2 — Union

Union do different groups ko merge karta hai.

Example:

    Group 1        Group 2

       1              2

Agar union(1,2) karenge:

    1 ----- 2

Ab dono ek group mein hain.


---

## 5. Approach

Hum `parent` array maintain karenge.

### Step 1: Parent Array Initialize Karo

    vector<int> parent(n + 1);

Initially har node khud ka parent hoga.

    parent[1] = 1
    parent[2] = 2
    parent[3] = 3

Matlab teeno alag groups mein hain.

### Step 2: Har Edge Process Karo

Har edge mein do nodes honge:

    edge = [u,v]

Example:

    edge = [1,2]

Toh:

    u = 1
    v = 2

### Step 3: Dono Nodes Ke Roots Find Karo

    int pu = find(u, parent);
    int pv = find(v, parent);

`pu` = node u ka root.

`pv` = node v ka root.

### Step 4: Roots Compare Karo

Agar:

    pu == pv

Matlab dono nodes already same group mein hain.

Isliye current edge cycle banati hai.

    return edge;

Agar roots different hain, toh dono groups ko merge karo:

    parent[pu] = pv;


---

## 6. Complete C++ Code

    class Solution {
    public:

        int find(int node, vector<int>& parent) {

            if (parent[node] == node) {
                return node;
            }

            return find(parent[node], parent);
        }

        vector<int> findRedundantConnection(vector<vector<int>>& edges) {

            int n = edges.size();

            vector<int> parent(n + 1);

            // Initially every node is its own parent
            for (int i = 1; i <= n; i++) {
                parent[i] = i;
            }

            // Process every edge
            for (auto edge : edges) {

                int u = edge[0];
                int v = edge[1];

                int pu = find(u, parent);
                int pv = find(v, parent);

                // Same root means cycle
                if (pu == pv) {
                    return edge;
                }

                // Merge the two groups
                parent[pu] = pv;
            }

            return {};
        }
    };


---

## 7. Code Explanation

### A. Parent Array

    vector<int> parent(n + 1);

Agar n = 3 hai, toh array ka size 4 hoga.

Index 0 se 3 tak available honge.

Graph ke nodes 1, 2, 3 hain.

    for (int i = 1; i <= n; i++) {
        parent[i] = i;
    }

Initially:

    parent[1] = 1
    parent[2] = 2
    parent[3] = 3

Har node apne separate group mein hai.


### B. Find Function

    int find(int node, vector<int>& parent) {

        if (parent[node] == node) {
            return node;
        }

        return find(parent[node], parent);
    }

Is function ka kaam node ka root find karna hai.

Agar node khud apna parent hai:

    parent[node] == node

Toh woh root hai.

Otherwise, uske parent ke root ko recursively find karo.

Example:

    parent[1] = 2
    parent[2] = 3
    parent[3] = 3

Parent links:

    1 → 2 → 3

Node 1 ka root:

    find(1) = 3

Node 2 ka root:

    find(2) = 3

Node 3 ka root:

    find(3) = 3

Isliye teeno same group mein hain.


### C. Har Edge Ko Process Karna

    for (auto edge : edges)

Ye loop har edge ko one by one uthata hai.

Example:

    edges = [[1,2], [1,3], [2,3]]

Pehli baar:

    edge = [1,2]

Doosri baar:

    edge = [1,3]

Teesri baar:

    edge = [2,3]


### D. U Aur V

    int u = edge[0];
    int v = edge[1];

Current edge ke dono endpoints store karte hain.

Example:

    edge = [1,2]

Toh:

    u = 1
    v = 2


### E. Dono Roots Find Karna

    int pu = find(u, parent);
    int pv = find(v, parent);

`pu` node u ka root hai.

`pv` node v ka root hai.

Ab hum check karenge ki dono same group mein hain ya alag groups mein.


### F. Cycle Detection

    if (pu == pv) {
        return edge;
    }

Agar roots same hain, toh dono nodes pehle se connected hain.

Nayi edge add karne se cycle banegi.

Isliye current edge answer hai.

Example:

    pu = 3
    pv = 3

Dono roots equal hain.

Therefore:

    return edge;


### G. Groups Merge Karna

    parent[pu] = pv;

Ye tab execute hota hai jab roots different hote hain.

Matlab dono nodes alag groups mein hain.

Unke groups ko merge kar do.

Example:

    pu = 1
    pv = 2

Then:

    parent[1] = 2

Ab node 1, node 2 ke group mein aa gaya.


---

## 8. Detailed Dry Run

Input:

    edges = [[1,2], [1,3], [2,3]]

Initially:

    parent = [_, 1, 2, 3]

Yahan `_` index 0 ko represent karta hai, jo is graph mein use nahi ho raha.


### Step 1 — Edge [1,2]

Current edge:

    [1,2]

So:

    u = 1
    v = 2

Roots:

    pu = find(1) = 1
    pv = find(2) = 2

Check:

    pu == pv ?

    1 == 2 → False

Cycle nahi hai.

Merge:

    parent[1] = 2

Now:

    parent = [_, 2, 2, 3]

Nodes 1 aur 2 ek group mein hain.


### Step 2 — Edge [1,3]

Current edge:

    [1,3]

So:

    u = 1
    v = 3

Roots:

    pu = find(1) = 2
    pv = find(3) = 3

Check:

    2 == 3 → False

Cycle nahi hai.

Merge:

    parent[2] = 3

Now:

    parent = [_, 2, 3, 3]

Parent links:

    1 → 2 → 3

Teeno nodes ka root 3 hai.

Graph ke groups:

    Group 1 = {1,2,3}


### Step 3 — Edge [2,3]

Current edge:

    [2,3]

So:

    u = 2
    v = 3

Roots:

    pu = find(2) = 3
    pv = find(3) = 3

Check:

    pu == pv ?

    3 == 3 → True

Dono nodes already same group mein hain.

Isliye ye extra edge cycle banayegi.

Code:

    if (pu == pv) {
        return edge;
    }

Return:

    [2,3]

Final answer:

    [2,3]


---

## 9. Pura Logic Simple Words Mein

Har edge ke liye:

    Edge [u,v]
         |
         v
    Dono roots find karo
         |
         v
    Roots same hain?
       /       \
     Yes        No
      |          |
  Cycle hai   Groups merge
      |          |
  Return edge  Next edge

Agar roots same hain toh edge redundant hai.

Agar roots different hain toh edge do groups ko connect karti hai, aur hume process continue karna hai.


---

## 10. Common Mistakes

### Mistake 1: Wrong Find Call

Correct:

    int pu = find(u, parent);
    int pv = find(v, parent);

Dono endpoints ka root find karna hai.


### Mistake 2: Root Compare Karne Se Pehle Merge Karna

Pehle roots compare karo.

    if (pu == pv) {
        return edge;
    }

Uske baad hi merge karo:

    parent[pu] = pv;


### Mistake 3: Same Root Ko Ignore Karna

Agar roots same hain aur hum edge ko accept kar lete hain, toh cycle detect nahi hogi.

### Mistake 4: Parent Array Galat Initialize Karna

Nodes 1 se n tak hain.

Correct:

    for (int i = 1; i <= n; i++) {
        parent[i] = i;
    }


---

## 11. Complexity Analysis

Let:

    n = number of nodes
    E = number of edges

Is beginner-friendly implementation mein path compression aur union by rank use nahi kiya gaya.

### Time Complexity

Worst case:

    O(n²)

Parent chain lambi ho sakti hai, isliye repeated `find()` calls multiple parents traverse kar sakti hain.

### Space Complexity

    O(n)

Parent array aur recursion stack ke liye.


---

## 12. Interview Explanation

"I will use Disjoint Set Union to detect the redundant edge. Initially, every node belongs to its own set. For each edge, I find the roots of its two endpoints. If both roots are the same, the endpoints are already connected, so adding this edge creates a cycle and I return it. Otherwise, I merge the two sets and continue processing the remaining edges."


---

## 13. Key Takeaways

1. DSU ka full form Disjoint Set Union hai.
2. `parent` array groups ko represent karta hai.
3. `find(node)` node ke group ka root return karta hai.
4. Same roots ka matlab nodes already connected hain.
5. Different roots ka matlab groups alag hain.
6. `parent[pu] = pv` groups ko merge karta hai.
7. LC 684 mein same root milne par current edge answer hoti hai.

Core rule:

    find(u) == find(v)
             |
             v
       Cycle detected
             |
             v
       Return current edge

    find(u) != find(v)
             |
             v
         Merge groups


---

## 14. Graph Roadmap Progress

### Pattern 1 — Foundation / Traversal

- LC 1971 — Find if Path Exists ✅
- LC 200 — Number of Islands ✅
- LC 994 — Rotting Oranges ✅

### Pattern 2 — Connected Components / Cycle Detection

- LC 547 — Number of Provinces ✅
- LC 785 — Is Graph Bipartite? ✅
- LC 684 — Redundant Connection ✅

Pattern 2 complete.

Next: **Pattern 3 — Topological Sort**

First question: **LC 207 — Course Schedule**
