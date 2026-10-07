# LeetCode 1971 — Find if Path Exists in Graph

## Problem

We are given:

- `n` → number of nodes
- `edges` → graph ki connections
- `source` → jahan se start karna hai
- `destination` → jahan pahunchna hai

Hume check karna hai ki kya `source` se `destination` tak koi path exist karta hai.

Example:

n = 3

edges = [[0,1], [1,2]]

source = 0
destination = 2

Graph:

    0
    |
    1
    |
    2

0 → 1 → 2

So answer = `true`


---

# Core Concept

Ye ek basic **Graph Traversal** problem hai.

Hume kisi node se start karke graph ke nodes explore karne hain.

Iske liye:

- DFS use kar sakte hain
- BFS bhi use kar sakte hain

Yahan hum DFS use karenge.


---

# Graph Representation

Edges diye hue hain:

`[u, v]`

Graph undirected hai.

Iska matlab:

`u → v`

and

`v → u`

Dono directions mein connection hai.

Isliye adjacency list banate waqt:

    adj[u].push_back(v);
    adj[v].push_back(u);

Example:

edges:

    [[0,1], [0,2], [1,3]]

Adjacency List:

    0 → 1, 2
    1 → 0, 3
    2 → 0
    3 → 1


---

# DFS Approach

Source se DFS start karenge.

Har node par:

1. Check karo kya current node destination hai.
2. Current node ko visited mark karo.
3. Uske saare neighbours dekho.
4. Jo neighbour visited nahi hai us par DFS call karo.
5. Agar kisi recursive call se `true` mila, turant `true` return karo.
6. Agar saare possible paths explore ho gaye aur destination nahi mila, `false` return karo.


---

# DFS Template

    void dfs(int node, int destination,
             vector<vector<int>>& adj,
             vector<bool>& visited) {

        if (node == destination) {
            return;
        }

        visited[node] = true;

        for (int neighbour : adj[node]) {

            if (!visited[neighbour]) {

                dfs(neighbour, destination,
                    adj, visited);
            }
        }
    }


---

# Important Logic

Is problem mein DFS ko `bool` return karwayenge.

Reason:

Hume sirf traversal nahi karna.

Hume ye bhi pata karna hai:

"Destination mila ya nahi?"

Isliye:

    true  → destination mil gaya
    false → destination nahi mila


---

# Final Code

    class Solution {
    public:

        bool dfs(int node, int destination,
                 vector<vector<int>>& adj,
                 vector<bool>& visited) {

            // Destination found
            if (node == destination) {
                return true;
            }

            // Mark current node as visited
            visited[node] = true;

            // Explore neighbours
            for (int neighbour : adj[node]) {

                if (!visited[neighbour]) {

                    if (dfs(neighbour, destination,
                            adj, visited)) {

                        return true;
                    }
                }
            }

            // Destination not found from this path
            return false;
        }


        bool validPath(int n, vector<vector<int>>& edges,
                       int source, int destination) {

            // Create adjacency list
            vector<vector<int>> adj(n);

            // Build undirected graph
            for (auto edge : edges) {

                int u = edge[0];
                int v = edge[1];

                adj[u].push_back(v);
                adj[v].push_back(u);
            }

            // Initially no node is visited
            vector<bool> visited(n, false);

            // Start DFS from source
            return dfs(source, destination,
                       adj, visited);
        }
    };


---

# Code Explanation

## 1. Adjacency List

    vector<vector<int>> adj(n);

`n` nodes hain.

Isliye `n` size ka adjacency list banaya.

Example:

n = 4

    adj[0]
    adj[1]
    adj[2]
    adj[3]


---

## 2. Add Edges

    for (auto edge : edges) {

        int u = edge[0];
        int v = edge[1];

        adj[u].push_back(v);
        adj[v].push_back(u);
    }

Graph undirected hai.

Agar:

    [0,1]

To:

    0 → 1
    1 → 0

Dono add karne padenge.


---

## 3. Visited Array

    vector<bool> visited(n, false);

Initially:

    visited = [false, false, false, false]

Jaise hi koi node visit hoti hai:

    visited[node] = true;


---

## 4. DFS Start

    return dfs(source, destination, adj, visited);

Agar source = 0:

    dfs(0, destination, ...)


---

# Most Important DFS Part

    for (int neighbour : adj[node]) {

        if (!visited[neighbour]) {

            if (dfs(neighbour, destination,
                    adj, visited)) {

                return true;
            }
        }
    }

Yahan actual recursion ho rahi hai.

Suppose current node `0` hai.

Neighbour `1` mila.

To:

    dfs(1, destination, ...)

Agar node `1` se destination mil gaya:

    dfs(1) → true

Then:

    if (dfs(1)) {
        return true;
    }

Source wala DFS bhi `true` return karega.


---

# Why `dfs(neighbour)` and NOT `dfs(node)`?

Ye bahut important hai.

Wrong:

    dfs(node, destination, adj, visited);

Isse same node ko dobara call karoge.

Correct:

    dfs(neighbour, destination, adj, visited);

Matlab:

"Current node ke neighbour ke paas jao aur wahan DFS continue karo."


---

# Why `visited` is Required?

Graph mein cycle ho sakti hai.

Example:

    0
   / \
  1---2

Connections:

    0 ↔ 1
    0 ↔ 2
    1 ↔ 2

Agar visited array na ho to:

    0 → 1 → 2 → 0 → 1 → 2 → ...

DFS repeatedly same nodes explore kar sakta hai.

Visited array is problem ko prevent karta hai.


---

# Detailed Dry Run

Example:

    n = 4

    edges = [[0,1], [0,2], [1,3]]

    source = 0
    destination = 3

Graph:

        0
       / \
      1   2
      |
      3


Adjacency List:

    0 → 1, 2
    1 → 0, 3
    2 → 0
    3 → 1


Initial:

    visited = [F, F, F, F]


## Step 1

Call:

    dfs(0, 3)

Check:

    0 == 3 ?

No.

Mark:

    visited[0] = true

Now:

    visited = [T, F, F, F]


Neighbours of 0:

    1, 2


---

## Step 2

Neighbour = 1

Check:

    visited[1] == false

Yes.

Call:

    dfs(1, 3)


---

## Step 3

Current node = 1

Check:

    1 == 3 ?

No.

Mark:

    visited[1] = true

Now:

    visited = [T, T, F, F]


Neighbours of 1:

    0, 3


Neighbour 0:

    visited[0] == true

So skip.

Next neighbour = 3.

    visited[3] == false

So call:

    dfs(3, 3)


---

## Step 4

Current node = 3

Check:

    3 == 3

Yes.

Therefore:

    return true;


Now recursion returns back:

    dfs(3) → true

Then:

    if (dfs(3)) {
        return true;
    }

So:

    dfs(1) → true


Then:

    dfs(0) → true


Finally:

    validPath() → true


Answer:

    true


---

# Important Recursion Flow

Actual calls:

    dfs(0)
       ↓
    dfs(1)
       ↓
    dfs(3)
       ↓
    true

Then true reverse direction mein return hota hai:

    dfs(3) → true
    dfs(1) → true
    dfs(0) → true


---

# Example Where Answer is False

    n = 6

    edges = [[0,1], [1,2], [3,4]]

    source = 0
    destination = 5


Graph:

    0 --- 1 --- 2

    3 --- 4

    5


Source component:

    0, 1, 2

Destination:

    5

Node 5 tak pahunchne ka koi edge nahi hai.

DFS:

    0 → 1 → 2

All possible neighbours explore ho gaye.

Destination nahi mila.

So:

    return false;


Answer:

    false


---

# Time Complexity

Adjacency list banane mein:

    O(E)

DFS mein har node aur edge maximum ek baar explore hota hai:

    O(V + E)

Total:

    O(V + E)


Where:

    V = number of vertices / nodes
    E = number of edges


---

# Space Complexity

Adjacency list:

    O(V + E)

Visited array:

    O(V)

DFS recursion stack:

    O(V)

Overall:

    O(V + E)


---

# Interview Explanation

Interviewer agar puche:

"How would you solve this problem?"

You can say:

"I will represent the graph using an adjacency list because the graph is undirected. Then I will run DFS from the source node while maintaining a visited array to avoid revisiting nodes. During DFS, if the current node becomes the destination, I return true. Otherwise, I recursively explore every unvisited neighbour. If all reachable nodes are explored and the destination is not found, I return false."


---

# Key Takeaways

1. Graph ko adjacency list se represent kar sakte hain.

2. Undirected graph mein edge dono directions mein add hoti hai.

3. DFS ka basic pattern:

       visit current node
              ↓
       mark visited
              ↓
       iterate neighbours
              ↓
       recurse on unvisited neighbour

4. `dfs(neighbour, ...)` karna hai, `dfs(node, ...)` nahi.

5. `visited` graph cycles aur repeated traversal ko prevent karta hai.

6. Boolean DFS useful hai jab hume answer `true / false` chahiye.

7. `if (dfs(...)) return true;` deeper recursion se aayi success ko propagate karta hai.

8. Graph traversal ka fundamental template:

       DFS = recursion / stack
       BFS = queue


---

# Pattern

## Graph Foundation / Traversal

Question:

    LC 1971 — Find if Path Exists in Graph

Pattern:

    DFS / BFS Reachability

Main idea:

    "Source se destination reachable hai ya nahi?"
