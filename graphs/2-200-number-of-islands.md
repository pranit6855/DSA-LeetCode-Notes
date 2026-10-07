# LeetCode 200 — Number of Islands

## Problem

Hume ek `m x n` grid di gayi hai.

- `'1'` = Land
- `'0'` = Water

Connected land cells milkar ek island banate hain.

Sirf 4 directions mein connection count hota hai:

    Up
     ↑
Left ← → Right
     ↓
    Down

Diagonal connection count nahi hota.

Example:

    1 1 0 0 0
    1 1 0 0 0
    0 0 1 0 0
    0 0 0 1 1

Isme total 3 islands hain.

Answer:

    3


---

# Pattern

## Grid DFS / Connected Components

Is problem ka main pattern hai:

    Har unvisited '1' = ek new island

Phir DFS/BFS se us island ke saare connected `1` cells ko visit kar do.

---

# Approach

Hum 2 cheezein karenge:

1. Puri grid ko outer loop se scan karenge.
2. Jab bhi `'1'` milega:
   - `count++`
   - DFS call karenge.
   - DFS us poore connected island ko visit karega.

Visited ke liye separate array use nahi karenge.

Instead:

    grid[r][c] = '0';

karke current land ko visited mark kar denge.

Isliye grid mein:

    '0' = water OR already visited


---

# DFS Logic

DFS ko current cell `(r,c)` diya jayega.

Sabse pehle boundary check:

    r < 0
    r >= row
    c < 0
    c >= col

Agar cell grid ke bahar hai:

    return;

Phir check:

    if(grid[r][c] == '0')
        return;

Matlab water hai ya already visited hai.

Agar cell `'1'` hai:

    grid[r][c] = '0';

Isko visited mark karo.

Phir 4 directions explore karo:

    Up
    Down
    Left
    Right


---

# Code

    class Solution {
    public:

        void dfs(int r, int c, vector<vector<char>>& grid) {

            int row = grid.size();
            int col = grid[0].size();

            // Boundary check
            if (r < 0 || r >= row ||
                c < 0 || c >= col) {
                return;
            }

            // Water or already visited
            if (grid[r][c] == '0') {
                return;
            }

            // Mark visited
            grid[r][c] = '0';

            // Up
            dfs(r - 1, c, grid);

            // Down
            dfs(r + 1, c, grid);

            // Left
            dfs(r, c - 1, grid);

            // Right
            dfs(r, c + 1, grid);
        }

        int numIslands(vector<vector<char>>& grid) {

            int row = grid.size();
            int col = grid[0].size();

            int count = 0;

            for (int i = 0; i < row; i++) {

                for (int j = 0; j < col; j++) {

                    if (grid[i][j] == '1') {

                        // New island found
                        count++;

                        // Visit entire island
                        dfs(i, j, grid);
                    }
                }
            }

            return count;
        }
    };


---

# Code Explanation

## 1. Grid dimensions

    int row = grid.size();
    int col = grid[0].size();

`row` = total rows

`col` = total columns


---

## 2. Count variable

    int count = 0;

Initially koi island nahi mila.

Jab bhi koi new unvisited land milega:

    count++;


---

## 3. Outer loop

    for (int i = 0; i < row; i++) {
        for (int j = 0; j < col; j++) {

Hum har cell ko one by one check karte hain.


---

## 4. New island detect karna

    if (grid[i][j] == '1') {

Agar current cell `1` hai aur humne usko pehle DFS se visit nahi kiya hai, toh ye ek new island hai.

    count++;

Phir:

    dfs(i, j, grid);


---

# Why DFS is called only when we find `1`

Suppose:

    1 1
    1 1

Ye 4 land cells hain but only 1 island hai.

Pehle cell par:

    count++;

DFS poore connected area ko visit karke sabko `0` kar dega.

Grid eventually:

    0 0
    0 0

Isliye baaki cells par dobara `count++` nahi hoga.

---

# DFS Boundary Check

    if (r < 0 || r >= row ||
        c < 0 || c >= col) {
        return;
    }

Example:

Agar current cell first row mein hai:

    r = 0

Up jaane par:

    r - 1 = -1

Ye grid ke bahar hai.

So DFS simply return karega.

Similarly:

    r >= row
    c < 0
    c >= col

grid ke bahar ke cases hain.


---

# Why mark `grid[r][c] = '0'`?

Ye bahut important hai.

    grid[r][c] = '0';

Matlab:

    "Is cell ko already visit kar chuka hoon."

Agar hum visited mark nahi karenge toh graph/grid cycle mein fas sakta hai.

Example:

    1 1
    1 0

(0,0) se (0,1) ja sakte hain.

Phir (0,1) se left jaakar dobara (0,0) aa sakte hain.

Without visited marking:

    (0,0)
       ↓
    (0,1)
       ↓
    (0,0)
       ↓
    (0,1)
       ↓
      ...

Infinite recursion ho sakti hai.

---

# Four Directions

Current cell:

        Up
         ↑
         |
Left ← (r,c) → Right
         |
         ↓
       Down

Code:

    dfs(r - 1, c, grid);
    dfs(r + 1, c, grid);
    dfs(r, c - 1, grid);
    dfs(r, c + 1, grid);


---

# Detailed Dry Run

Grid:

    1 1 0
    1 0 0
    0 0 1

Initial:

    count = 0


## Step 1

Outer loop reaches `(0,0)`.

Cell:

    1

So:

    count++;

Now:

    count = 1

Call:

    dfs(0,0)


---

## Step 2

At `(0,0)`:

    grid[0][0] = '1'

Mark visited:

    grid[0][0] = '0'

Grid:

    0 1 0
    1 0 0
    0 0 1

Then DFS checks:

    Up
    Down
    Left
    Right


---

## Step 3 — Down

From `(0,0)`:

    dfs(1,0)

`grid[1][0] = '1'`

Mark visited:

    grid[1][0] = '0'

Grid:

    0 1 0
    0 0 0
    0 0 1

Its neighbours are then checked.

Any `0` or out-of-bound cell simply returns.


---

## Step 4 — Right from `(0,0)`

Eventually:

    dfs(0,1)

`grid[0][1] = '1'`

Mark:

    grid[0][1] = '0'

Grid:

    0 0 0
    0 0 0
    0 0 1

Now first island is completely visited.

Important:

    count = 1

There were 3 land cells, but they formed only 1 island.


---

## Step 5

Outer loop continues.

Only remaining `1` is:

    (2,2)

So:

    count++;

Now:

    count = 2

Call:

    dfs(2,2)


DFS usko bhi `0` kar dega.

Final grid:

    0 0 0
    0 0 0
    0 0 0

Final answer:

    2


---

# Most Important Logic

Pura problem sirf is flow par based hai:

    FOR every cell
          ↓
    Is it '1'?
       /     \
     No       Yes
     ↓         ↓
   ignore    count++
                ↓
               DFS
                ↓
       complete island visit
                ↓
       connected '1' → '0'
                ↓
          continue scanning


---

# Why count is outside DFS

Wrong idea:

    void dfs(...) {
        count++;
    }

Aisa karoge toh island ke har cell ko separate island count kar doge.

Example:

    1 1
    1 1

4 cells hain.

Correct answer:

    1

Isliye:

    count++

sirf tab karna hai jab outer loop ko ek fresh `1` mile.

Then DFS entire connected island ko mark karega.


---

# Common Mistakes

## Mistake 1

Wrong:

    r > row
    c > col

Correct:

    r >= row
    c >= col

Because last valid index is:

    row - 1
    col - 1


## Mistake 2

Visited mark na karna.

Correct:

    grid[r][c] = '0';


## Mistake 3

Har cell par DFS call karna.

Wrong:

    dfs(i, j, grid);

Correct:

    if (grid[i][j] == '1') {
        count++;
        dfs(i, j, grid);
    }


## Mistake 4

Diagonal ko connected maan lena.

Wrong:

    1 0
    0 1

Ye 1 island nahi hai.

Correct:

    2 islands


---

# Complexity

Let:

    R = number of rows
    C = number of columns

Har cell maximum ek baar visit hota hai.

Time Complexity:

    O(R × C)

Extra Space:

    O(R × C)

Worst case mein DFS recursion stack poori grid tak ja sakti hai.


---

# Interview Explanation

"I will scan the entire grid. Whenever I find an unvisited land cell, I know that a new island has been found, so I increment the count and run DFS from that cell. DFS visits all connected land cells in the four possible directions and marks them as visited by changing them from '1' to '0'. This ensures that the same island is not counted again."


---

# Key Takeaways

1. Grid ko graph ki tarah treat kar sakte hain.

2. `'1'` = land node.

3. `'0'` = water / visited.

4. Har fresh `1` = new connected component = new island.

5. DFS poore connected component ko visit karta hai.

6. Four directions:
   
       Up
       Down
       Left
       Right

7. Visited mark karna mandatory hai.

8. `count++` DFS ke andar nahi, outer loop mein hoga.

9. Boundary condition:

       r < 0
       r >= row
       c < 0
       c >= col

10. Core pattern:

       Find unvisited land
              ↓
           count++
              ↓
             DFS
              ↓
       visit entire island


---

# Pattern Progress

## Graph Foundation / Traversal

- LC 1971 — Find if Path Exists in Graph ✅
- LC 200 — Number of Islands ✅
- LC 994 — Rotting Oranges ⏳

LC 133 — Clone Graph → Optional / skipped from main roadmap.
