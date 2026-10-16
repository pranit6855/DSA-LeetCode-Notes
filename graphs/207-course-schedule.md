\# LeetCode 207 — Course Schedule

## 1. Problem Statement

Hume `numCourses` courses aur `prerequisites` naam ka 2D array diya gaya hai.

Kuch courses complete karne se pehle doosre courses complete karna zaroori hai.

Example:

    numCourses = 2
    prerequisites = [[1,0]]

Iska matlab:

Course 1 karne se pehle Course 0 complete karna zaroori hai.

Valid order:

    Course 0 → Course 1

Dono courses complete ho sakte hain.

Output:

    true

### Cycle Wala Example

    numCourses = 2
    prerequisites = [[1,0], [0,1]]

Iska matlab:

- Course 1 se pehle Course 0 chahiye.
- Course 0 se pehle Course 1 chahiye.

Graph:

    0 → 1
    ↑   |
    └───┘

Dono courses ek doosre par depend karte hain.

Isliye saare courses complete karna impossible hai.

Output:

    false


---

## 2. Pattern

Graph Pattern 3 — Topological Sort

Sub-pattern:

    Directed Graph
    Cycle Detection
    BFS
    Kahn's Algorithm

Is question mein hume sirf ye check karna hai ki saare courses complete ho sakte hain ya nahi.

Hum **BFS + Kahn's Algorithm** use karenge.


---

## 3. Important Concepts

### A. Directed Graph

Prerequisite ek direction represent karti hai.

Example:

    prerequisites = [[1,0]]

Matlab Course 0 pehle complete hoga, phir Course 1.

Graph:

    0 → 1

Isliye edge `0 → 1` banegi.


### B. Indegree Kya Hai?

Indegree kisi node par aane wali incoming edges ki count hoti hai.

Course Schedule mein indegree batati hai ki kisi course ki kitni prerequisites pending hain.

Example:

    0 → 1 → 2

Indegree:

    Course 0 = 0
    Course 1 = 1
    Course 2 = 1

Course 0 ki koi prerequisite nahi hai.

Course 1 ke liye Course 0 complete karna hoga.

Course 2 ke liye Course 1 complete karna hoga.


### C. Queue

Queue mein un courses ko store karenge jinki indegree `0` hai.

Indegree `0` ka matlab:

    Is course ki koi prerequisite pending nahi hai.

Aise courses ko abhi complete kar sakte hain.


### D. Kahn's Algorithm

Kahn's Algorithm Topological Sort ke liye BFS-based approach hai.

Basic flow:

    Indegree calculate karo
            |
            v
    Zero-indegree courses queue mein daalo
            |
            v
    Queue se course nikalo
            |
            v
    Course complete count karo
            |
            v
    Dependent courses ki indegree decrease karo
            |
            v
    Indegree 0 hui → queue mein daalo
            |
            v
    Saare courses process hue?
            |
       Yes / No
            |
            v
       true / false


---

## 4. Approach

### Step 1: Graph aur Indegree Banao

    vector<vector<int>> adj(numCourses);
    vector<int> indegree(numCourses, 0);

`adj` batata hai ki ek course complete karne ke baad kaunse courses ki prerequisite satisfy ho sakti hai.

`indegree` batata hai ki har course ki kitni incoming dependency edges hain.


### Step 2: Prerequisites Process Karo

Har prerequisite:

    [course, prerequisite]

Example:

    [1,0]

Matlab Course 1 ke liye Course 0 zaroori hai.

Isliye:

    adj[0].push_back(1);
    indegree[1]++;

Graph:

    0 → 1

Indegree:

    [0,1]


### Step 3: Queue Fill Karo

Har course ki indegree check karo.

Agar:

    indegree[i] == 0

Toh course queue mein daalo.

Example:

    indegree = [0,1]

Queue:

    [0]

Course 0 ready hai.


### Step 4: BFS Chalao

Queue se course nikalo.

    count++;

Matlab ek course complete ho gaya.

Ab uske outgoing neighbours check karo.

    indegree[next]--;

Prerequisite complete hone ke baad dependent course ki pending prerequisites ek kam ho jaati hain.

Agar:

    indegree[next] == 0

Toh us course ko queue mein daal do.


### Step 5: Final Answer

Agar:

    count == numCourses

Matlab saare courses process ho gaye.

Return:

    true

Otherwise:

    false

Kyunki cycle ki wajah se kuch courses process nahi ho paaye.


---

## 5. Complete C++ Code

    class Solution {
    public:
        bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {

            vector<vector<int>> adj(numCourses);
            vector<int> indegree(numCourses, 0);
            queue<int> q;

            // Build graph and calculate indegree
            for (auto p : prerequisites) {
                int course = p[0];
                int pre = p[1];

                adj[pre].push_back(course);
                indegree[course]++;
            }

            // Add courses with no prerequisites
            for (int i = 0; i < numCourses; i++) {
                if (indegree[i] == 0) {
                    q.push(i);
                }
            }

            int count = 0;

            // BFS - Kahn's Algorithm
            while (!q.empty()) {

                int course = q.front();
                q.pop();

                count++;

                for (int next : adj[course]) {

                    indegree[next]--;

                    if (indegree[next] == 0) {
                        q.push(next);
                    }
                }
            }

            return count == numCourses;
        }
    };


---

## 6. Code Explanation

### A. Graph Banana

    vector<vector<int>> adj(numCourses);

Agar:

    numCourses = 3

Toh nodes honge:

    0, 1, 2

Har node ki adjacency list mein uske baad kiye ja sakne wale courses store honge.


### B. Indegree Initialize Karna

    vector<int> indegree(numCourses, 0);

Initially har course ki indegree `0` set hoti hai.

Prerequisites process karte waqt indegree update karenge.


### C. Prerequisites Loop

    for (auto p : prerequisites) {

        int course = p[0];
        int pre = p[1];

        adj[pre].push_back(course);
        indegree[course]++;
    }

Yahan `p` ek prerequisite pair hai.

Example:

    p = [1,0]

Toh:

    course = 1
    pre = 0

Course 1 ke liye Course 0 prerequisite hai.

Isliye:

    adj[0].push_back(1);

Directed edge:

    0 → 1

Aur:

    indegree[1]++;

Course 1 ki indegree ek badh jaayegi.


### D. Queue Mein Zero-Indegree Courses

    for (int i = 0; i < numCourses; i++) {
        if (indegree[i] == 0) {
            q.push(i);
        }
    }

Koi pending prerequisite nahi hai toh course ready hai.

Usko queue mein daal do.


### E. Count Variable

    int count = 0;

Ye batata hai ki ab tak kitne courses process ho chuke hain.

Jab bhi queue se ek course nikaalenge:

    count++;


### F. Queue Se Course Nikaalna

    int course = q.front();
    q.pop();

`q.front()` queue ka first course deta hai.

`q.pop()` us course ko queue se remove karta hai.

Ab hum maan rahe hain ki current course complete ho gaya.


### G. Dependent Courses Process Karna

    for (int next : adj[course]) {

Har outgoing neighbour ko check karenge.

Example:

    adj[0] = [1,2]

Matlab Course 0 complete hone ke baad Course 1 aur Course 2 ki ek prerequisite satisfy ho sakti hai.

Isliye:

    indegree[next]--;


### H. Course Ready Hone Par Queue Mein Daalna

    if (indegree[next] == 0) {
        q.push(next);
    }

Agar kisi course ki saari prerequisites complete ho gayi hain, toh uski indegree `0` ho jaati hai.

Ab woh course bhi process hone ke liye ready hai.


### I. Final Condition

    return count == numCourses;

Agar processed courses ki count total courses ke equal hai, toh saare courses complete ho sakte hain.

Agar count kam hai, toh kuch courses cycle ki wajah se process nahi ho paaye.

Isliye:

    count == numCourses → true
    count < numCourses  → false


---

## 7. Detailed Dry Run — True Case

Input:

    numCourses = 3

    prerequisites = [
        [1,0],
        [2,0],
        [2,1]
    ]

Graph:

        0
       / \
      v   v
      1 → 2

Edges:

    0 → 1
    0 → 2
    1 → 2


### Step 1: Graph Aur Indegree

Initially:

    indegree = [0,0,0]

Edge `[1,0]`:

    0 → 1

Update:

    indegree = [0,1,0]

Edge `[2,0]`:

    0 → 2

Update:

    indegree = [0,1,1]

Edge `[2,1]`:

    1 → 2

Update:

    indegree = [0,1,2]


### Step 2: Queue Initialize

Indegree `0` wala course sirf Course 0 hai.

    Queue = [0]

    count = 0


### Step 3: Course 0 Process Karo

Queue se Course 0 nikalo.

    count = 1

Course 0 ke neighbours:

    1, 2

Course 1 ki indegree:

    1 → 0

Course 1 queue mein jaayega.

Course 2 ki indegree:

    2 → 1

Course 2 abhi ready nahi hai.

Ab:

    Queue = [1]
    indegree = [0,0,1]
    count = 1


### Step 4: Course 1 Process Karo

Queue se Course 1 nikalo.

    count = 2

Course 1 ka neighbour Course 2 hai.

Course 2 ki indegree:

    1 → 0

Ab Course 2 queue mein jaayega.

State:

    Queue = [2]
    indegree = [0,0,0]
    count = 2


### Step 5: Course 2 Process Karo

Queue se Course 2 nikalo.

    count = 3

Course 2 ka koi outgoing neighbour nahi hai.

Queue empty ho jaayegi.

Final:

    count = 3
    numCourses = 3

Condition:

    count == numCourses

Answer:

    true


---

## 8. Dry Run — Cycle Wala Case

Input:

    numCourses = 2

    prerequisites = [
        [1,0],
        [0,1]
    ]

Graph:

    0 → 1
    ↑   |
    └───┘

### Step 1: Indegree

Edge `[1,0]` se:

    0 → 1

Edge `[0,1]` se:

    1 → 0

Final:

    indegree = [1,1]


### Step 2: Queue Initialize

Koi course indegree `0` wala nahi hai.

    Queue = []

BFS start hi nahi hoga.

Final:

    count = 0
    numCourses = 2

Condition:

    count == numCourses

    0 == 2 → false

Answer:

    false

Dono courses ek doosre par depend karte hain, isliye koi bhi pehle complete nahi ho sakta.


---

## 9. Important Difference — Course Schedule I vs II

### LC 207 — Course Schedule

Question:

    Kya saare courses complete ho sakte hain?

Return type:

    bool

Answer:

    true / false


### LC 210 — Course Schedule II

Question:

    Courses complete karne ka valid order kya hai?

Return type:

    vector<int>

Dono mein Kahn's Algorithm use kar sakte hain.

Main difference:

LC 207 mein sirf processed courses count karte hain.

LC 210 mein processed courses ko answer vector mein store bhi karte hain.


---

## 10. Common Mistakes

### Mistake 1: Edge Direction Galat Banana

Input:

    [1,0]

Meaning:

    Course 1 requires Course 0

Correct edge:

    0 → 1

Code:

    adj[pre].push_back(course);


### Mistake 2: Indegree Galat Update Karna

Correct:

    indegree[course]++;

Incoming dependency course par aati hai.


### Mistake 3: Zero Indegree Courses Queue Mein Nahi Daalna

Correct:

    if (indegree[i] == 0) {
        q.push(i);
    }

Ye starting courses hain jo bina kisi prerequisite ke complete ho sakte hain.


### Mistake 4: Indegree Decrease Karna Bhoolna

Correct:

    indegree[next]--;

Jab prerequisite course complete hota hai, dependent course ki pending prerequisite ek kam hoti hai.


### Mistake 5: Cycle Detection Galat Karna

Sirf queue empty hone se cycle decide nahi hoti.

Correct final check:

    return count == numCourses;

Agar saare courses process ho gaye toh true, otherwise false.


---

## 11. Complexity Analysis

Let:

    V = number of courses
    E = number of prerequisite relationships

### Time Complexity

    O(V + E)

- Graph aur indegree banana: O(V + E)
- Har course queue se maximum ek baar nikalta hai.
- Har edge maximum ek baar process hoti hai.

### Space Complexity

    O(V + E)

- Adjacency list: O(V + E)
- Indegree array: O(V)
- Queue: O(V)


---

## 12. Interview Explanation

"I will use Kahn's Algorithm, which is a BFS-based topological sorting technique. First, I build a directed graph from the prerequisite relationships and calculate the indegree of every course. Then I add all zero-indegree courses to a queue. For every course removed from the queue, I increase the completed-course count and decrease the indegree of its dependent courses. Whenever a course's indegree becomes zero, I add it to the queue. If the total number of processed courses equals the number of courses, I return true; otherwise, a cycle prevents some courses from being completed, so I return false."


---

## 13. Key Takeaways

1. Course Schedule is a directed graph problem.
2. `[course, prerequisite]` means prerequisite → course.
3. Indegree pending incoming dependencies ki count hai.
4. Zero indegree means course ready hai.
5. Queue mein ready courses store hote hain.
6. Har processed course ke neighbours ki indegree decrease hoti hai.
7. Saare courses process hue toh true.
8. Kuch courses process nahi hue toh false.
9. Kahn's Algorithm BFS use karta hai.
10. Ye Topological Sort aur directed cycle detection ka important pattern hai.

Core logic:

    Calculate indegree
           |
           v
    Queue mein zero-indegree courses
           |
           v
    Process course
           |
           v
    Count++
           |
           v
    Neighbours ki indegree--
           |
           v
    Indegree 0 → Queue mein daalo
           |
           v
    count == numCourses?
           |
       Yes / No
           |
           v
       true / false


---

## 14. Graph Roadmap Progress

### Pattern 1 — Graph Traversal & Grid

- LC 1971 — Find if Path Exists ✅
- LC 200 — Number of Islands ✅
- LC 994 — Rotting Oranges ✅

### Pattern 2 — Connected Components & Cycle Detection

- LC 547 — Number of Provinces ✅
- LC 785 — Is Graph Bipartite? ✅
- LC 684 — Redundant Connection ✅

### Pattern 3 — Topological Sort

- LC 207 — Course Schedule ✅
- LC 210 — Course Schedule II ⏳

Next Question:

**LC 210 — Course Schedule II**

Same Kahn's Algorithm use karenge, bas is baar valid course order return karna hai.
