# LeetCode 994 — Rotting Oranges

## 1. Problem Statement

Hume ek grid di gayi hai:

- `0` = Empty cell
- `1` = Fresh orange
- `2` = Rotten orange

Har minute, rotten orange ke up, down, left aur right mein present fresh oranges rotten ho jaate hain.

Hume minimum minutes find karne hain jisme saare fresh oranges rotten ho jayen.

Agar kuch fresh oranges tak infection nahi pahunch sakta, toh `-1` return karo.

### Example

Input:

    grid = [
        [2,1,1],
        [1,1,0],
        [0,1,1]
    ]

Output:

    4

Saare fresh oranges ko rotten hone mein 4 minutes lagenge.


---

## 2. Pattern

**Graph Pattern 1 — Foundation / Traversal**

**Sub-pattern: Grid BFS / Multi-Source BFS**

Is question mein BFS use karte hain kyunki hume minimum time find karna hai.

Har BFS level ek minute represent karta hai.

Multi-Source BFS ka matlab hai ki starting mein jitne rotten oranges hain, un sabko queue mein daalna.


---

## 3. Approach

### Step 1: Grid scan karo

- Rotten orange (`2`) mile toh uski position queue mein daalo.
- Fresh orange (`1`) mile toh `fresh++`.
- Empty cell (`0`) ko ignore karo.

### Step 2: BFS start karo

Queue se rotten orange nikalo aur uske 4 neighbours check karo.

Agar neighbour fresh hai:

    grid[nr][nc] = 2;
    fresh--;
    q.push({nr, nc});

Matlab:
1. Orange ko rotten mark karo.
2. Fresh count kam karo.
3. Orange ki position queue mein add karo.

### Step 3: Minutes count karo

Ek complete BFS level process hone ke baad:

    time++;

### Step 4: Answer return karo

- `fresh == 0` → `time` return karo.
- Fresh oranges bache hain → `-1` return karo.


---

## 4. C++ Code

    class Solution {
    public:
        int orangesRotting(vector<vector<int>>& grid) {

            int m = grid.size();
            int n = grid[0].size();

            queue<pair<int,int>> q;
            int fresh = 0, time = 0;

            // Initial scan
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < n; j++) {

                    if (grid[i][j] == 2)
                        q.push({i, j});

                    else if (grid[i][j] == 1)
                        fresh++;
                }
            }

            // Up, Down, Left, Right
            int dr[] = {-1, 1, 0, 0};
            int dc[] = {0, 0, -1, 1};

            while (!q.empty() && fresh > 0) {

                int size = q.size();

                while (size--) {

                    int r = q.front().first;
                    int c = q.front().second;
                    q.pop();

                    for (int d = 0; d < 4; d++) {

                        int nr = r + dr[d];
                        int nc = c + dc[d];

                        if (nr >= 0 && nr < m &&
                            nc >= 0 && nc < n &&
                            grid[nr][nc] == 1) {

                            grid[nr][nc] = 2;
                            fresh--;
                            q.push({nr, nc});
                        }
                    }
                }

                time++;
            }

            return fresh == 0 ? time : -1;
        }
    };


---

## 5. Important Code Explanation

### Queue

    queue<pair<int,int>> q;

Queue mein orange ki position store hoti hai.

Example:

    q.push({0, 0});

Matlab row `0`, column `0` wala orange queue mein daalo.

### Fresh Count

    int fresh = 0;

Har fresh orange ke liye `fresh++`.

Jab orange rotten hota hai, `fresh--`.

### Four Directions

    int dr[] = {-1, 1, 0, 0};
    int dc[] = {0, 0, -1, 1};

Ye arrays neighbouring cell ki position nikalte hain.

| Direction | Row change | Column change |
|---|---:|---:|
| Up | -1 | 0 |
| Down | +1 | 0 |
| Left | 0 | -1 |
| Right | 0 | +1 |

Current cell `(r,c)` ho toh:

    int nr = r + dr[d];
    int nc = c + dc[d];

`nr` aur `nc` next neighbour ki position hain.

### Boundary Check

    nr >= 0 && nr < m &&
    nc >= 0 && nc < n

Check karta hai ki neighbour grid ke andar hai.

### Fresh Orange Ko Rotten Karna

    grid[nr][nc] = 2;
    fresh--;
    q.push({nr, nc});

Fresh orange rotten hota hai, count kam hota hai aur uski position queue mein add ho jaati hai.

### BFS Level

    int size = q.size();

Ye current level mein process karne wale oranges ki count save karta hai.

    while (size--) {
        // Current level ke oranges process karo
    }

Naye rotten oranges agle level mein process honge.

**Ek complete level = ek minute.**

Isliye level complete hone ke baad:

    time++;


---

## 6. Dry Run

Input:

    2 1 1
    1 1 0
    0 1 1

Initially:

    Queue = [(0,0)]
    Fresh = 6
    Time = 0

### Minute 1

Fresh neighbours `(1,0)` aur `(0,1)` rotten hote hain.

    2 2 1
    2 1 0
    0 1 1

    Fresh = 4
    Time = 1

### Minute 2

`(1,1)` aur `(0,2)` rotten hote hain.

    2 2 2
    2 2 0
    0 1 1

    Fresh = 2
    Time = 2

### Minute 3

`(2,1)` rotten hota hai.

    2 2 2
    2 2 0
    0 2 1

    Fresh = 1
    Time = 3

### Minute 4

`(2,2)` rotten hota hai.

    2 2 2
    2 2 0
    0 2 2

    Fresh = 0
    Time = 4

Final answer:

    4


---

## 7. Common Mistakes

1. `time++` har orange ke liye nahi, har complete BFS level ke baad karna hai.

2. `size = q.size()` current level ko separate process karne ke liye hai.

3. Orange rotten hote hi `grid[nr][nc] = 2` karna hai.

4. Starting ke saare rotten oranges queue mein daalne hain.

5. Fresh oranges remaining hon aur queue empty ho jaaye, toh `-1` return karna hai.


---

## 8. Complexity

Let:

    R = number of rows
    C = number of columns

Time Complexity:

    O(R × C)

Har cell ko maximum ek baar process kiya jaata hai.

Space Complexity:

    O(R × C)

Queue worst case mein grid ke bahut saare cells store kar sakti hai.


---

## 9. Interview Explanation

"I will use Multi-Source BFS. First, I add all initially rotten oranges to a queue and count the fresh oranges. Then I process the queue level by level. For each rotten orange, I check its four neighbours. If a neighbour is fresh, I mark it rotten, decrease the fresh count, and add it to the queue. Each completed level represents one minute. If all fresh oranges become rotten, I return the time; otherwise, I return -1."


---

## 10. Key Takeaways

- BFS minimum steps/time ke liye useful hai jab har transition ka cost equal ho.
- Queue mein `(row, column)` store hote hain.
- Multi-Source BFS mein saare starting rotten oranges queue mein jaate hain.
- `fresh` remaining fresh oranges ko track karta hai.
- Har complete BFS level ek minute hai.
- Saare fresh oranges rotten ho jaayen toh time return karo; warna `-1`.

---

## Graph Roadmap — Progress

### Pattern 1: Foundation / Traversal

- LC 1971 — Find if Path Exists in Graph ✅
- LC 200 — Number of Islands ✅
- LC 994 — Rotting Oranges ✅
- LC 733 — Flood Fill ⏳
- LC 695 — Max Area of Island ⏳

Next: **LC 733 — Flood Fill**
