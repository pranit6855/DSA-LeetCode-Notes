# LC 1091 — Shortest Path in Binary Matrix

## 1. Problem Statement

Given an n x n binary matrix `grid`:

- `0` represents an open cell.
- `1` represents a blocked cell.
- Starting position is `(0, 0)`.
- Destination is `(n-1, n-1)`.

We can move in 8 directions:
1. Up
2. Down
3. Left
4. Right
5. Top-left diagonal
6. Top-right diagonal
7. Bottom-left diagonal
8. Bottom-right diagonal

We need to return the length of the shortest valid path from the starting cell to the destination.

Important:
- Both starting and destination cells are counted.
- If no valid path exists, return `-1`.
- Diagonal movement is allowed.

---

## 2. Example

Input:

    grid = [
        [0, 0, 0],
        [1, 1, 0],
        [1, 1, 0]
    ]

Output:

    4

Shortest path:

    (0,0) -> (0,1) -> (1,2) -> (2,2)

Path length = 4 cells.

There are only 3 moves, but the path contains 4 cells because the starting cell is also counted.

---

## 3. Pattern

**Pattern 4 — Shortest Path**

Algorithm: Breadth-First Search (BFS)

### Why BFS?

BFS explores nodes level by level.

- Level 1: Cells reachable in 1 cell of path length.
- Level 2: Cells reachable in 2 cells of path length.
- Level 3: Cells reachable in 3 cells of path length.
- And so on.

Every move has the same cost: one step.

Therefore, BFS reaches the destination using the minimum number of cells first.

**Important rule:** BFS gives the shortest path when all moves have equal cost.

---

## 4. Approach

### Step 1: Find the grid size

Store the number of rows in `n`.

Since the grid is n x n, the destination is:

    (n-1, n-1)

### Step 2: Check the start and destination

If either `grid[0][0]` or `grid[n-1][n-1]` is `1`, return `-1`.

We cannot start from or reach a blocked cell.

### Step 3: Create a queue

Store three values in the queue:

    {row, column, distance}

Example:

    {0, 0, 1}

This means:
- Row = 0
- Column = 0
- Distance = 1

We start the distance at 1 because the starting cell is counted.

### Step 4: Mark the starting cell visited

Change:

    grid[0][0] = 1

This prevents the starting cell from being visited repeatedly.

We reuse the grid to mark visited cells.

### Step 5: Explore all 8 directions

For every current cell, calculate the coordinates of its neighbours.

    newRow = row + dr
    newCol = col + dc

A neighbour is valid when:

1. Its row is within the grid.
2. Its column is within the grid.
3. Its value is `0`, meaning it is open and unvisited.

### Step 6: Add valid neighbours to the queue

If a neighbour is valid:

1. Mark it visited.
2. Add it to the queue.
3. Increase the distance by 1.

    newDistance = distance + 1

### Step 7: Check the destination

When the destination is removed from the queue, return its distance.

Because BFS explores in increasing order of distance, the first valid arrival gives the shortest path length.

### Step 8: Return -1 if unreachable

If the queue becomes empty before reaching the destination, return `-1`.

---

## 5. C++ Code

```cpp
class Solution {
public:
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        int n = grid.size();

        // Start or destination is blocked
        if (grid[0][0] == 1 ||
            grid[n - 1][n - 1] == 1) {
            return -1;
        }

        // {row, column, distance}
        queue<tuple<int, int, int>> q;
        q.push({0, 0, 1});

        // Mark starting cell as visited
        grid[0][0] = 1;

        // All 8 possible directions
        vector<pair<int, int>> directions = {
            {-1, -1}, {-1, 0}, {-1, 1},
            { 0, -1},           { 0, 1},
            { 1, -1}, { 1, 0}, { 1, 1}
        };

        while (!q.empty()) {
            auto [r, c, distance] = q.front();
            q.pop();

            // Destination reached
            if (r == n - 1 && c == n - 1) {
                return distance;
            }

            // Explore all 8 neighbours
            for (auto [dr, dc] : directions) {
                int nr = r + dr;
                int nc = c + dc;

                // Boundary check and open-cell check
                if (nr >= 0 && nr < n &&
                    nc >= 0 && nc < n &&
                    grid[nr][nc] == 0) {

                    // Mark visited before adding to queue
                    grid[nr][nc] = 1;

                    q.push({nr, nc, distance + 1});
                }
            }
        }

        // Destination is unreachable
        return -1;
    }
};
```

---

## 6. Code Explanation

### A. Grid size

```cpp
int n = grid.size();
```

Stores the number of rows. Since the grid is square, it is also the number of columns.

### B. Start and destination check

```cpp
if (grid[0][0] == 1 ||
    grid[n - 1][n - 1] == 1) {
    return -1;
}
```

If the starting cell or destination is blocked, there is no valid path.

### C. Queue

```cpp
queue<tuple<int, int, int>> q;
q.push({0, 0, 1});
```

The queue stores:

    {row, column, distance}

We use `tuple` because each queue entry has three values.

The starting distance is 1 because the starting cell counts toward the path length.

### D. Visited marking

```cpp
grid[0][0] = 1;
```

Marks the starting cell as visited.

We can reuse the grid because we only need to know whether a cell is open and unvisited.

Originally blocked cells are also `1`, so both blocked and visited cells will be skipped.

### E. Direction array

```cpp
vector<pair<int, int>> directions = {
    {-1, -1}, {-1, 0}, {-1, 1},
    { 0, -1},           { 0, 1},
    { 1, -1}, { 1, 0}, { 1, 1}
};
```

Each pair represents:

    {row change, column change}

Examples:

- `{-1, 0}`: Move up.
- `{1, 0}`: Move down.
- `{0, 1}`: Move right.
- `{0, -1}`: Move left.
- `{1, 1}`: Move diagonally down-right.
- `{-1, -1}`: Move diagonally up-left.

### F. BFS loop

```cpp
while (!q.empty()) {
    auto [r, c, distance] = q.front();
    q.pop();
```

Take the front entry from the queue and remove it.

For example, if the entry is:

    {0, 1, 2}

Then:

    r = 0
    c = 1
    distance = 2

### G. Destination condition

```cpp
if (r == n - 1 && c == n - 1) {
    return distance;
}
```

If the current cell is the destination, return its distance.

BFS guarantees that this is the shortest path length.

### H. Calculate neighbour coordinates

```cpp
int nr = r + dr;
int nc = c + dc;
```

The current position plus a direction gives the neighbour's position.

Example:

    Current cell = (0, 1)
    Direction    = (1, 1)
    New cell     = (1, 2)

This represents a diagonal move.

### I. Boundary and cell validation

```cpp
if (nr >= 0 && nr < n &&
    nc >= 0 && nc < n &&
    grid[nr][nc] == 0)
```

The neighbour must:
- Be inside the grid.
- Be open.
- Not have been visited before.

### J. Visit and push the neighbour

```cpp
grid[nr][nc] = 1;
q.push({nr, nc, distance + 1});
```

Mark the neighbour visited and push it into the queue with its updated distance.

We mark the cell visited immediately to prevent duplicate queue entries.

### K. No valid path

```cpp
return -1;
```

If the queue becomes empty, every reachable cell has been explored. If the destination was not reached, return `-1`.

---

## 7. Detailed Dry Run

Input:

    grid = [
        [0, 0, 0],
        [1, 1, 0],
        [1, 1, 0]
    ]

Starting cell: `(0, 0)`

Destination: `(2, 2)`

### Initial State

Starting cell is placed into the queue with distance 1.

    Queue = [(0, 0, 1)]

After marking the starting cell visited:

    1 0 0
    1 1 0
    1 1 0

### Step 1: Process (0, 0, 1)

The cell `(0, 0)` is not the destination.

The only valid unvisited neighbour is `(0, 1)`.

Its distance is:

    1 + 1 = 2

Queue after processing:

    Queue = [(0, 1, 2)]

Grid:

    1 1 0
    1 1 0
    1 1 0

### Step 2: Process (0, 1, 2)

Valid unvisited neighbours:
- `(0, 2)`
- `(1, 2)`

Both receive distance 3.

Queue:

    Queue = [(0, 2, 3), (1, 2, 3)]

Grid:

    1 1 1
    1 1 1
    1 1 0

### Step 3: Process (0, 2, 3)

The valid neighbour `(1, 2)` is already visited.

No new cells are added.

Queue:

    Queue = [(1, 2, 3)]

### Step 4: Process (1, 2, 3)

The destination `(2, 2)` is open and unvisited.

Its distance is:

    3 + 1 = 4

Queue:

    Queue = [(2, 2, 4)]

Grid:

    1 1 1
    1 1 1
    1 1 1

### Step 5: Process (2, 2, 4)

The current cell is the destination.

The code returns:

    return 4;

Final output:

    4

Shortest path:

    (0,0) -> (0,1) -> (1,2) -> (2,2)

The path contains 4 cells and 3 moves.

---

## 8. Complexity Analysis

Let `n` be the number of rows and columns.

### Time Complexity: O(n²)

There are `n²` cells.

Each cell is added to the queue at most once, and for each cell we check 8 directions.

Since 8 is constant, the total time complexity is:

    O(n²)

### Space Complexity: O(n²)

The queue can store up to O(n²) cells in the worst case.

Therefore:

    O(n²)

---

## 9. Important Interview Points

1. BFS is used because each move has equal cost.
2. The path length includes both the starting and destination cells.
3. Start the distance from 1, not 0.
4. There are 8 possible directions, including diagonals.
5. Check boundaries before accessing the grid.
6. Mark a cell visited when pushing it into the queue.
7. If the destination is blocked or unreachable, return -1.
8. Dijkstra's Algorithm is generally used for shortest paths in weighted graphs with non-negative edge weights.

---

## 10. Key Takeaway

In a normal BFS traversal, the queue stores the nodes to explore.

In a shortest-path BFS, we additionally store the distance:

    {row, column, distance}

Every valid move increases the distance by 1.

The first time the destination is reached, its distance is the shortest path length.

## Filename

1091-shortest-path-in-binary-matrix.md
