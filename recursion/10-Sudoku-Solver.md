# 10 - Sudoku Solver

## LeetCode 37 - Sudoku Solver

### Problem

Given a `9 x 9` Sudoku board, solve the puzzle by filling the empty cells.

Each row must contain the digits:

`1 - 9`

Each column must contain the digits:

`1 - 9`

Each `3 x 3` box must contain the digits:

`1 - 9`

A digit cannot be repeated in:

- Same row
- Same column
- Same `3 x 3` box

The board has exactly one valid solution.

---

# Simple Understanding

Sudoku board mein kuch cells already filled hote hain aur kuch empty hote hain.

Example:

    5 3 . . 7 . . . .
    6 . . 1 9 5 . . .
    . 9 8 . . . . 6 .
    8 . . . 6 . . . 3
    4 . . 8 . 3 . . 1
    7 . . . 2 . . . 6
    . 6 . . . . 2 8 .
    . . . 4 1 9 . . 5
    . . . . 8 . . 7 9

`.` means:

**Empty cell**

Hume har empty cell mein correct number fill karna hai.

---

# Main Idea - Backtracking

Sudoku ko solve karne ke liye:

1. Empty cell find karo
2. `1` se `9` tak number try karo
3. Check karo number safe hai ya nahi
4. Agar safe hai:
   - Number place karo
   - Next empty cell solve karo
5. Agar aage solution nahi banta:
   - Number remove karo
   - Next number try karo

Ye exactly:

**Choose → Explore → Undo**

hai.

---

# Why Backtracking?

Suppose kisi empty cell mein:

`1`

place kiya.

Ab next cells ko fill karte waqt eventually pata chala ki koi valid number possible nahi hai.

Toh hum:

`1` remove karenge

aur:

`2`

try karenge.

Aise hi `9` tak try karte hain.

---

# Board Representation

Board:

    vector<vector<char>> board;

Ye `9 x 9` matrix hai.

Example:

    {
        {'5','3','.','.','7','.','.','.','.'},
        {'6','.','.','1','9','5','.','.','.'},
        ...
    }

Empty cell:

`.`

represent karta hai.

---

# Row and Column

Har cell ko:

`board[row][col]`

se access karenge.

Example:

`board[0][0]`

means first row, first column.

`board[4][4]`

means middle cell.

---

# Basic Algorithm

Hum board ko left-to-right aur top-to-bottom traverse karenge.

Har cell:

### Case 1 - Already filled

Agar:

`board[row][col] != '.'`

toh simply next cell par jao.

### Case 2 - Empty

Agar:

`board[row][col] == '.'`

toh `1` se `9` tak har digit try karo.

---

# Finding Empty Cell

Hum normally board scan kar sakte hain:

    for(int row = 0; row < 9; row++) {
        for(int col = 0; col < 9; col++) {

            if(board[row][col] == '.') {

                // Empty cell found
            }
        }
    }

Jaise hi empty cell milta hai, us cell ko fill karne ki try karenge.

---

# What Makes a Number Valid?

Suppose:

`board[row][col]`

empty hai.

Hum number `num` place karna chahte hain.

Tab 3 checks karne hain:

1. Same row
2. Same column
3. Same `3 x 3` box

Agar teeno safe hain:

**number valid hai.**

---

# 1. Row Check

Check karo ki same row mein number already present toh nahi.

Example:

    5 3 . . 7 . . . .

Agar hum `5` place karna chahte hain:

Invalid.

Kyunki row mein `5` already hai.

Code:

    for(int j = 0; j < 9; j++) {
        if(board[row][j] == num)
            return false;
    }

---

# 2. Column Check

Same column mein number already present toh nahi.

Example:

    5
    6
    .
    8
    4

Agar current cell mein `6` place karna chahte hain:

Invalid.

Code:

    for(int i = 0; i < 9; i++) {
        if(board[i][col] == num)
            return false;
    }

---

# 3. 3 x 3 Box Check

Sudoku board mein total:

**9 boxes**

hote hain.

Har box:

**3 x 3**

ka hota hai.

Example:

    5 3 .
    6 . .
    . 9 8

Ye first `3 x 3` box hai.

Agar number `5` already box mein hai:

Invalid.

---

# Finding 3 x 3 Box

Current cell:

`(row, col)`

ke corresponding box ka starting point:

    rowStart = (row / 3) * 3
    colStart = (col / 3) * 3

Example:

`row = 4`

`col = 5`

Then:

    rowStart = (4 / 3) * 3
             = 1 * 3
             = 3

    colStart = (5 / 3) * 3
             = 1 * 3
             = 3

So box starts at:

`(3,3)`

---

# Box Loop

Code:

    int startRow = (row / 3) * 3;
    int startCol = (col / 3) * 3;

    for(int i = 0; i < 3; i++) {
        for(int j = 0; j < 3; j++) {

            if(board[startRow + i][startCol + j] == num)
                return false;
        }
    }

---

# Complete isSafe Function

    bool isSafe(vector<vector<char>>& board,
                int row,
                int col,
                char num) {

        // Check row
        for(int j = 0; j < 9; j++) {
            if(board[row][j] == num)
                return false;
        }

        // Check column
        for(int i = 0; i < 9; i++) {
            if(board[i][col] == num)
                return false;
        }

        // Check 3x3 box
        int startRow = (row / 3) * 3;
        int startCol = (col / 3) * 3;

        for(int i = 0; i < 3; i++) {
            for(int j = 0; j < 3; j++) {

                if(board[startRow + i][startCol + j] == num)
                    return false;
            }
        }

        return true;
    }

---

# Backtracking Function

Ab recursive function banayenge:

`solve(board)`

Is function ka kaam:

**poora Sudoku solve karna**

Steps:

1. Empty cell find karo
2. `1-9` try karo
3. Safe number place karo
4. Recursively solve karo
5. Agar fail ho:
   - Number remove karo

---

# Base Case

Agar board mein koi empty cell hi nahi mila:

Matlab:

**Sudoku completely solved hai.**

So:

`return true;`

---

# Choose

Agar empty cell mila:

`board[row][col]`

Toh:

    board[row][col] = num;

Number place karenge.

---

# Explore

Ab:

    solve(board)

call karenge.

Matlab:

**Baaki Sudoku solve karo.**

Agar recursion `true` return kare:

Solution mil gaya.

So:

`return true;`

---

# Undo

Agar recursion fail ho:

    board[row][col] = '.';

Number remove karo.

Then next number try karo.

---

# Complete Code

    class Solution {
    public:

        bool isSafe(vector<vector<char>>& board,
                    int row,
                    int col,
                    char num) {

            // Row check
            for(int j = 0; j < 9; j++) {
                if(board[row][j] == num)
                    return false;
            }

            // Column check
            for(int i = 0; i < 9; i++) {
                if(board[i][col] == num)
                    return false;
            }

            // 3x3 box check
            int startRow = (row / 3) * 3;
            int startCol = (col / 3) * 3;

            for(int i = 0; i < 3; i++) {
                for(int j = 0; j < 3; j++) {

                    if(board[startRow + i][startCol + j] == num)
                        return false;
                }
            }

            return true;
        }

        bool solve(vector<vector<char>>& board) {

            for(int row = 0; row < 9; row++) {

                for(int col = 0; col < 9; col++) {

                    // Skip filled cells
                    if(board[row][col] != '.')
                        continue;

                    // Try 1 to 9
                    for(char num = '1'; num <= '9'; num++) {

                        if(isSafe(board, row, col, num)) {

                            // Choose
                            board[row][col] = num;

                            // Explore
                            if(solve(board))
                                return true;

                            // Undo
                            board[row][col] = '.';
                        }
                    }

                    // No number works
                    return false;
                }
            }

            // No empty cells -> solved
            return true;
        }

        void solveSudoku(vector<vector<char>>& board) {

            solve(board);
        }
    };

---

# Detailed Dry Run

Suppose current empty cell is:

`board[0][2]`

Board starts:

    5 3 . . 7 . . . .
    6 . . 1 9 5 . . .
    . 9 8 . . . . 6 .
    ...

Current:

`row = 0`

`col = 2`

---

## Try Number 1

Check:

### Row

Row 0:

    5 3 . . 7 . . . .

No `1`.

Safe.

### Column

Column 2:

    .
    .
    8
    .
    .
    .
    .
    .
    .

No `1`.

Safe.

### Box

First `3 x 3` box:

    5 3 .
    6 . .
    . 9 8

No `1`.

Safe.

So:

    board[0][2] = '1';

---

# Recursion

Now:

`solve(board)`

Next empty cell find karenge.

Suppose next cell:

`board[0][3]`

Ab again:

`1 -> 9`

try karenge.

---

# What If Wrong Choice?

Suppose kisi branch mein:

    board[0][2] = '1';

place karne ke baad eventually koi empty cell impossible ho gaya.

Then recursive call:

`solve(board)`

returns:

`false`

So:

    board[0][2] = '.';

Queen-like undo nahi, yahan **number undo** hota hai.

Then next:

`2`

try karenge.

---

# Backtracking Pattern

Sudoku mein core:

    if(isSafe(board, row, col, num)) {

        board[row][col] = num;

        if(solve(board))
            return true;

        board[row][col] = '.';
    }

Meaning:

### Step 1

**Choose**

Number place karo.

### Step 2

**Explore**

Next cells solve karo.

### Step 3

**Undo**

Agar branch fail ho toh number remove karo.

---

# Why `return false` After Trying 1-9?

Suppose current empty cell mein:

`1` to `9`

sab try kar liye.

Koi bhi number successful nahi hua.

Matlab:

**Current branch impossible hai.**

So:

`return false;`

Previous recursion level par control chala jayega.

Wahan previous choice undo hogi.

---

# Why Return True?

Agar kisi branch se:

`solve(board)`

returns `true`

toh complete Sudoku solved hai.

So:

`return true;`

karna padega.

---

# Why We Don't Need to Track Row Separately?

Kyuki row checking directly board se kar rahe hain.

Example:

    for(int j = 0; j < 9; j++)

Same for:

- column
- box

No extra `set` required in the basic solution.

---

# Optimized Approach

Basic solution har number ke liye:

- Row scan
- Column scan
- Box scan

karta hai.

Isko optimize karne ke liye hum boolean arrays / sets maintain kar sakte hain.

Example:

    rows[9][10]
    cols[9][10]
    boxes[9][10]

Meaning:

`rows[r][num]`

batayega ki row `r` mein `num` already used hai ya nahi.

Similarly:

`cols[c][num]`

and:

`boxes[b][num]`

---

# Box Number

Har `3 x 3` box ko ek ID de sakte hain:

    box = (row / 3) * 3 + (col / 3);

Example:

Top-left:

`0`

Top-middle:

`1`

Top-right:

`2`

Middle-left:

`3`

...

Bottom-right:

`8`

---

# Optimized Code

    class Solution {
    public:

        bool rows[9][10] = {};
        bool cols[9][10] = {};
        bool boxes[9][10] = {};

        bool solve(vector<vector<char>>& board) {

            for(int row = 0; row < 9; row++) {

                for(int col = 0; col < 9; col++) {

                    if(board[row][col] != '.')
                        continue;

                    int box = (row / 3) * 3 + (col / 3);

                    for(int num = 1; num <= 9; num++) {

                        if(rows[row][num] ||
                           cols[col][num] ||
                           boxes[box][num]) {
                            continue;
                        }

                        // Choose
                        board[row][col] = char('0' + num);

                        rows[row][num] = true;
                        cols[col][num] = true;
                        boxes[box][num] = true;

                        // Explore
                        if(solve(board))
                            return true;

                        // Undo
                        board[row][col] = '.';

                        rows[row][num] = false;
                        cols[col][num] = false;
                        boxes[box][num] = false;
                    }

                    return false;
                }
            }

            return true;
        }

        void solveSudoku(vector<vector<char>>& board) {

            // Initialize tracking arrays
            for(int row = 0; row < 9; row++) {

                for(int col = 0; col < 9; col++) {

                    if(board[row][col] == '.')
                        continue;

                    int num = board[row][col] - '0';
                    int box = (row / 3) * 3 + (col / 3);

                    rows[row][num] = true;
                    cols[col][num] = true;
                    boxes[box][num] = true;
                }
            }

            solve(board);
        }
    };

---

# Important Difference

Basic solution:

**Check board every time**

Optimized solution:

**Track used numbers**

So:

Basic:

    isSafe()

Optimized:

    rows[]
    cols[]
    boxes[]

---

# Recursion Flow

Sudoku recursion:

    Find empty cell
          ↓
    Try 1 to 9
          ↓
    Check validity
          ↓
    Place number
          ↓
    solve(board)
          ↓
    Solution found?
       /        \
     YES         NO
      ↓           ↓
    return     Undo number
                 ↓
            Try next number

---

# Most Important 3 Lines

Sudoku mein ye 3 lines core logic hain:

    board[row][col] = num;

    if(solve(board))
        return true;

    board[row][col] = '.';

Meaning:

1. **Place number**
2. **Go deeper**
3. **Undo if needed**

Ye hi Backtracking ka main pattern hai.

---

# Backtracking Kya Hai?

Simple meaning:

**Choice lo → aage explore karo → fail ho toh choice undo karo → next choice try karo.**

Sudoku mein:

**Number place → recurse → remove**

---

# N-Queens vs Sudoku

## N-Queens

Choose:

**Column**

Check:

- Column
- Diagonal

Then:

**Place Queen → Recurse → Remove Queen**

---

## Sudoku

Choose:

**Number**

Check:

- Row
- Column
- Box

Then:

**Place Number → Recurse → Remove Number**

---

# Common Mistakes

### 1. Undo karna bhool jaana

Wrong:

    board[row][col] = num;
    solve(board);

Correct:

    board[row][col] = num;

    if(solve(board))
        return true;

    board[row][col] = '.';

---

### 2. Same row check bhool jaana

Number same row mein duplicate nahi hona chahiye.

---

### 3. Same column check bhool jaana

Number same column mein duplicate nahi hona chahiye.

---

### 4. Box formula galat likhna

Correct:

    int startRow = (row / 3) * 3;
    int startCol = (col / 3) * 3;

---

### 5. `return false` galat jagah lagana

Sirf tab:

`1-9`

sab try ho jaayein aur kuch work na kare.

Then:

`return false;`

---

### 6. Solution milne ke baad continue karna

Agar:

    if(solve(board))
        return true;

ho gaya, toh immediately return karo.

---

# Why First Empty Cell Works?

Hum kisi bhi empty cell ko choose kar sakte hain.

Simple implementation mein:

**First empty cell**

choose karte hain.

Ye easiest approach hai.

---

# Better Optimization - Minimum Remaining Values

Ek aur optimization:

Sabse pehle woh empty cell choose karo jahan **least valid numbers** available hon.

Example:

Cell A:

Possible:

`1,2,3,4,5`

Cell B:

Possible:

`2`

Pehle Cell B solve karna better ho sakta hai.

Isse search space reduce ho sakta hai.

Lekin standard interview solution ke liye:

**First empty cell + Backtracking**

enough hai.

---

# Time Complexity

Sudoku has fixed `9 x 9` board.

Worst case mein many combinations explore ho sakte hain.

At each empty cell:

At most:

`9 choices`

So worst-case search exponential hai.

Commonly:

**Time Complexity = O(9^E)**

where `E` = number of empty cells.

Since board size is fixed to `9 x 9`, practical search space bounded hai, but backtracking worst case large ho sakta hai.

---

# Space Complexity

Recursion depth maximum:

`81`

So recursion stack:

**O(E)**

where `E` is number of empty cells.

Board itself:

**O(81)**

Since board size fixed hai, ye effectively constant space hai apart from recursion.

---

# Pattern Recognition

Question mein agar:

- Fill Sudoku board
- Fill empty cells
- Numbers `1-9`
- No duplicate in row
- No duplicate in column
- No duplicate in box
- Find valid complete board

aisa likha ho,

to immediately think:

**Backtracking + Constraint Checking**

---

# Interview Explanation

Interview mein simple language mein:

"I solve the Sudoku using backtracking. I find an empty cell and try digits from 1 to 9. Before placing a digit, I check whether it already exists in the same row, column, or 3x3 box. If the digit is valid, I place it and recursively solve the remaining board. If the recursive call fails, I remove the digit and try the next one. If there are no empty cells left, the Sudoku is solved."

---

# One-Line Mental Model

**Empty cell find karo → 1 to 9 try karo → row/column/box check karo → place karo → recurse karo → fail ho toh remove karo.**

---

# Final Takeaway

Sudoku Solver ka pura concept:

Empty cell find karo
→ valid number choose karo
→ place karo
→ next empty cell par recursion
→ solution fail ho toh number remove karo
→ next number try karo

Sudoku = **Backtracking + Constraint Checking**

Sabse important:

**`.` = Empty cell**

**row = Current row**

**col = Current column**

**num = Candidate digit**

**isSafe = Row + Column + 3x3 Box check**

**Place = Number lagao**

**Recurse = Baaki board solve karo**

**Undo = Number hatao**

**Core Pattern = Place → Recurse → Undo**
