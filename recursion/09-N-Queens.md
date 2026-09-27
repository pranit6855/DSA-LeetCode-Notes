# 09 - N Queens

## LeetCode 51 - N-Queens

### Problem

Given an integer `n`, place `n` queens on an `n x n` chessboard so that **no two queens attack each other**.

A queen can attack another queen if they are in the:

- Same row
- Same column
- Same diagonal

Return all distinct solutions.

---

# Simple Understanding

Hume `n x n` chessboard par `n` queens place karni hain.

Condition:

**Koi bhi do queens ek dusre ko attack nahi karni chahiye.**

Example for `n = 4`:

    . Q . .
    . . . Q
    Q . . .
    . . Q .

Yahan 4 queens hain aur:

- koi same row mein nahi
- koi same column mein nahi
- koi same diagonal mein nahi

Isliye ye valid solution hai.

---

# Main Idea - Backtracking

N-Queens ko solve karne ke liye hum:

**Row by Row Queen Place** karenge.

Har row mein:

- Har column try karo
- Check karo ki queen safely place ho sakti hai ya nahi
- Agar safe hai to queen place karo
- Next row par jao
- Agar aage solution nahi banta, queen remove karo
- Next column try karo

Ye exactly **Backtracking** hai.

---

# Why Row By Row?

Hum ek time par sirf ek row handle karenge.

Example:

    Row 0 → Queen place karo
    Row 1 → Queen place karo
    Row 2 → Queen place karo
    Row 3 → Queen place karo

Isse ek important condition automatically satisfy ho jaati hai:

**Ek row mein sirf ek queen hogi.**

Hume sirf check karna hai:

- Column safe hai?
- Diagonal safe hai?

---

# Board Representation

Hum board ko:

`vector<string> board;`

se represent karenge.

For `n = 4`:

    ....
    ....
    ....
    ....

Queen place karne par:

    .Q..
    ....
    ....
    ....

Queen ko:

`board[row][col] = 'Q';`

se place karenge.

Aur backtracking ke time:

`board[row][col] = '.';`

karke remove kar denge.

---

# row Ka Meaning

`row` batata hai:

**Abhi kis row mein queen place karni hai.**

Example:

`row = 0`

Matlab first row mein queen place karni hai.

`row = 1`

Matlab second row mein queen place karni hai.

---

# Base Case

Sabse important:

`if(row == n)`

Agar:

`row == n`

iska matlab hai:

**Saari n rows mein queen successfully place ho chuki hain.**

Board ek valid solution hai.

So:

`ans.push_back(board);`

kar denge.

---

# Safe Position

Queen ko kisi position `(row, col)` par place karne se pehle check karna hai ki woh safe hai ya nahi.

Hume 3 cheeze check karni hain:

### 1. Same Column

Koi queen already same column mein nahi honi chahiye.

Example:

    Q . . .
    . . . .
    Q . . .

Ye invalid hai kyunki dono queens same column mein hain.

---

### 2. Upper Left Diagonal

Check karo ki upper-left diagonal par queen hai ya nahi.

Example:

    Q . . .
    . Q . .

Ye invalid hai.

---

### 3. Upper Right Diagonal

Check karo ki upper-right diagonal par queen hai ya nahi.

Example:

    . . Q .
    . Q . .

Ye bhi invalid hai.

---

# Why Only Upper Side Check Karna Hai?

Hum board ko:

**Top → Bottom**

fill kar rahe hain.

Isliye current row ke neeche abhi koi queen hai hi nahi.

So current queen ke liye sirf:

- same column
- upper-left diagonal
- upper-right diagonal

check karna enough hai.

---

# isSafe Function

Basic function:

`bool isSafe(vector<string>& board, int row, int col, int n)`

Ye check karega ki `(row, col)` par queen place kar sakte hain ya nahi.

---

# Same Column Check

Code:

    for(int i = 0; i < row; i++) {
        if(board[i][col] == 'Q')
            return false;
    }

Hum `0` se `row - 1` tak check karenge.

Agar same column mein queen mil gayi:

`return false;`

Matlab position unsafe hai.

---

# Upper Left Diagonal Check

Starting position:

`i = row - 1`

`j = col - 1`

Har step:

    i--
    j--

Code:

    int i = row - 1;
    int j = col - 1;

    while(i >= 0 && j >= 0) {
        if(board[i][j] == 'Q')
            return false;

        i--;
        j--;
    }

---

# Upper Right Diagonal Check

Starting position:

`i = row - 1`

`j = col + 1`

Har step:

    i--
    j++

Code:

    int i = row - 1;
    int j = col + 1;

    while(i >= 0 && j < n) {
        if(board[i][j] == 'Q')
            return false;

        i--;
        j++;
    }

---

# Complete isSafe Function

    bool isSafe(vector<string>& board, int row, int col, int n) {

        // Same column
        for(int i = 0; i < row; i++) {
            if(board[i][col] == 'Q')
                return false;
        }

        // Upper-left diagonal
        int i = row - 1;
        int j = col - 1;

        while(i >= 0 && j >= 0) {
            if(board[i][j] == 'Q')
                return false;

            i--;
            j--;
        }

        // Upper-right diagonal
        i = row - 1;
        j = col + 1;

        while(i >= 0 && j < n) {
            if(board[i][j] == 'Q')
                return false;

            i--;
            j++;
        }

        return true;
    }

---

# Backtracking Function

Ab main recursion:

`solve(board, row)`

`row` batayega ki kaunsi row fill karni hai.

Har row ke liye:

`for(int col = 0; col < n; col++)`

chalayenge.

Har column ko check karenge.

Agar safe hai:

`board[row][col] = 'Q';`

Queen place karo.

Then:

`solve(board, row + 1);`

Next row par jao.

Finally:

`board[row][col] = '.';`

Queen remove karo.

---

# Choose → Explore → Undo

N-Queens ka core:

    board[row][col] = 'Q';

    solve(board, row + 1);

    board[row][col] = '.';

Meaning:

1. Queen place karo
2. Next row solve karo
3. Queen remove karo

Ye hi **Backtracking** hai.

---

# Complete Code

    class Solution {
    public:

        vector<vector<string>> ans;

        bool isSafe(vector<string>& board, int row, int col, int n) {

            // Same column
            for(int i = 0; i < row; i++) {
                if(board[i][col] == 'Q')
                    return false;
            }

            // Upper-left diagonal
            int i = row - 1;
            int j = col - 1;

            while(i >= 0 && j >= 0) {
                if(board[i][j] == 'Q')
                    return false;

                i--;
                j--;
            }

            // Upper-right diagonal
            i = row - 1;
            j = col + 1;

            while(i >= 0 && j < n) {
                if(board[i][j] == 'Q')
                    return false;

                i--;
                j++;
            }

            return true;
        }

        void solve(vector<string>& board, int row, int n) {

            // All queens placed
            if(row == n) {
                ans.push_back(board);
                return;
            }

            // Try every column
            for(int col = 0; col < n; col++) {

                if(isSafe(board, row, col, n)) {

                    // Choose
                    board[row][col] = 'Q';

                    // Explore
                    solve(board, row + 1, n);

                    // Undo
                    board[row][col] = '.';
                }
            }
        }

        vector<vector<string>> solveNQueens(int n) {

            vector<string> board(n, string(n, '.'));

            solve(board, 0, n);

            return ans;
        }
    };

---

# Detailed Dry Run - n = 4

Initial board:

    ....
    ....
    ....
    ....

Start:

`row = 0`

---

## Step 1 - Row 0

Try:

`col = 0`

Board:

    Q...
    ....
    ....
    ....

Safe.

Now:

`solve(row = 1)`

---

# Step 2 - Row 1

Try column 0.

    Q...
    Q...
    ....
    ....

Invalid.

Same column.

So reject.

---

Try column 1.

    Q...
    .Q..
    ....
    ....

Invalid.

Diagonal conflict.

So reject.

---

Try column 2.

    Q...
    ..Q.
    ....
    ....

Invalid.

Diagonal conflict.

So reject.

---

Try column 3.

    Q...
    ...Q
    ....
    ....

Safe.

Go to:

`row = 2`

---

# Step 3 - Row 2

Try column 0.

    Q...
    ...Q
    Q...
    ....

Invalid.

Same column.

---

Try column 1.

    Q...
    ...Q
    .Q..
    ....

Invalid.

Diagonal conflict.

---

Try column 2.

    Q...
    ...Q
    ..Q.
    ....

Invalid.

Diagonal conflict.

---

Try column 3.

    Q...
    ...Q
    ...Q
    ....

Invalid.

Same column.

So:

**No valid column for row 2.**

This branch fails.

---

# Backtrack

Row 1 queen remove karo:

    Q...
    ....
    ....
    ....

Now row 1 ka next option try karo.

Since all columns failed:

Row 0 queen bhi remove karo.

    ....
    ....
    ....
    ....

Now row 0 ka next column try karenge.

---

# Step 4 - Row 0, Column 1

Board:

    .Q..
    ....
    ....
    ....

Next row:

`row = 1`

Possible positions check karenge.

Invalid positions eliminate karte hue eventually ek valid branch milegi:

    .Q..
    ...Q
    Q...
    ..Q.

This is a valid solution.

---

# One Valid Solution

    .Q..
    ...Q
    Q...
    ..Q.

Check:

- Row 0 → one queen
- Row 1 → one queen
- Row 2 → one queen
- Row 3 → one queen

Columns:

    1
    3
    0
    2

All different.

Diagonals bhi conflict nahi karte.

So solution valid hai.

---

# All Solutions for n = 4

There are **2 solutions**.

### Solution 1

    .Q..
    ...Q
    Q...
    ..Q.

### Solution 2

    ..Q.
    Q...
    ...Q
    .Q..

---

# Recursion Tree Concept

For each row:

    Row 0
    │
    ├── Column 0
    │   ├── Row 1
    │   ├── Row 1
    │   └── Dead End
    │
    ├── Column 1
    │   └── Valid Branch
    │
    ├── Column 2
    │   └── Valid Branch
    │
    └── Column 3
        └── Dead End

Har branch ek possible board configuration represent karti hai.

Agar branch dead end par pahunchti hai:

**Backtrack**

aur previous choice undo karke next choice try karte hain.

---

# Why Backtracking Needed?

Suppose:

    Q...
    ...Q
    ....
    ....

Ab next row mein koi valid position nahi milti.

Agar hum backtrack nahi karenge, toh ye branch wahi stuck ho jayegi.

Backtracking:

    Queen remove
    ↓
    Previous row
    ↓
    Next column try

---

# Most Important 3 Lines

N-Queens mein:

    board[row][col] = 'Q';

    solve(board, row + 1, n);

    board[row][col] = '.';

Meaning:

**Place → Recurse → Remove**

Ye hi backtracking ka core pattern hai.

---

# Why One Queen Per Row?

Hum row by row solve kar rahe hain.

Har recursion call current row mein exactly ek queen place karne ki try karti hai.

So automatically:

    One row → One queen

Isliye same-row conflict check karne ki zarurat nahi padti.

---

# Why Column Check?

Queen vertically attack kar sakti hai.

Example:

    Q...
    ....
    Q...

Same column.

So invalid.

---

# Why Diagonal Check?

Queen diagonal direction mein bhi attack kar sakti hai.

### Diagonal 1

    Q...
    .Q..

### Diagonal 2

    ..Q.
    .Q..

Isliye dono diagonals check karni zaroori hain.

---

# Alternative Optimized Approach

Basic solution mein har position ke liye board scan karte hain.

Hum `set` / arrays use karke checks faster bhi bana sakte hain.

Three structures:

    vector<bool> col(n);
    vector<bool> diag1(2*n - 1);
    vector<bool> diag2(2*n - 1);

Meaning:

    col     → columns
    diag1   → one diagonal direction
    diag2   → other diagonal direction

---

# Optimized Code

    class Solution {
    public:

        vector<vector<string>> ans;

        void solve(
            vector<string>& board,
            vector<bool>& col,
            vector<bool>& diag1,
            vector<bool>& diag2,
            int row,
            int n
        ) {

            if(row == n) {
                ans.push_back(board);
                return;
            }

            for(int c = 0; c < n; c++) {

                int d1 = row - c + n - 1;
                int d2 = row + c;

                if(col[c] || diag1[d1] || diag2[d2])
                    continue;

                // Place
                board[row][c] = 'Q';

                col[c] = true;
                diag1[d1] = true;
                diag2[d2] = true;

                // Next row
                solve(
                    board,
                    col,
                    diag1,
                    diag2,
                    row + 1,
                    n
                );

                // Backtrack
                board[row][c] = '.';

                col[c] = false;
                diag1[d1] = false;
                diag2[d2] = false;
            }
        }

        vector<vector<string>> solveNQueens(int n) {

            vector<string> board(n, string(n, '.'));

            vector<bool> col(n, false);
            vector<bool> diag1(2 * n - 1, false);
            vector<bool> diag2(2 * n - 1, false);

            solve(
                board,
                col,
                diag1,
                diag2,
                0,
                n
            );

            return ans;
        }
    };

---

# Diagonal Formula

Optimized approach mein diagonal ko identify karne ke liye:

`row + col`

aur

`row - col + n - 1`

use karte hain.

### `row + col`

Same `/` diagonal ke liye same value milti hai.

### `row - col`

Same `\` diagonal ke liye same value milti hai.

`row - col` negative ho sakta hai, isliye:

`row - col + n - 1`

use karte hain.

---

# Time Complexity

N-Queens mein worst case approximately:

**O(N!)**

states explore karne pad sakte hain.

Basic implementation mein har position ko safe check karne ke liye `O(N)` lag sakta hai.

So commonly:

**Time Complexity = O(N × N!)**

Optimized column + diagonal arrays ke saath safety check:

**O(1)**

ho jaata hai.

---

# Space Complexity

Board:

**O(N²)**

Recursion depth:

**O(N)**

Column array:

**O(N)**

Diagonal arrays:

**O(N)**

So auxiliary space approximately:

**O(N²)**

board ke saath.

Answer storage alag se count hoga.

---

# Common Mistakes

### 1. Queen place karna but remove na karna

Wrong:

    board[row][col] = 'Q';
    solve(board, row + 1, n);

Correct:

    board[row][col] = 'Q';
    solve(board, row + 1, n);
    board[row][col] = '.';

---

### 2. Same column check bhool jaana

Queen vertically attack karti hai.

Always check:

`board[i][col] == 'Q'`

---

### 3. Diagonal check galat karna

Current row se diagonal move:

Upper-left:

    row--
    col--

Upper-right:

    row--
    col++

---

### 4. Base Case galat likhna

Correct:

`if(row == n)`

Tabhi saari rows successfully complete hui hain.

---

### 5. Next row par move na karna

Correct:

`solve(board, row + 1, n);`

---

### 6. Backtracking nahi karna

Correct order:

    Place
    ↓
    Recurse
    ↓
    Undo

---

# N-Queens vs Permutations

Permutation mein:

**Position fill karni hoti hai using swaps.**

N-Queens mein:

**Row fill karni hoti hai with safe positions.**

Permutation:

`Choose → Recurse → Swap Back`

N-Queens:

`Place → Recurse → Remove`

Dono mein common concept:

**Backtracking**

---

# Pattern Recognition

Question mein agar:

- Place queens on a chessboard
- No two queens should attack each other
- Same row / column / diagonal avoid karna hai
- Generate all valid boards

aisa likha ho,

to:

**Backtracking + Constraint Checking**

think karo.

---

# Interview Explanation

Interview mein simple language mein aise explain kar sakte ho:

"We solve the board row by row using backtracking. For each row, we try every column and check whether placing a queen there is safe. We check the column and both upper diagonals because previous rows are already filled. If the position is safe, we place the queen and recursively solve the next row. After returning from recursion, we remove the queen so we can try the next column."

---

# One-Line Mental Model

**Har row mein safe column choose karo → next row solve karo → fail hone par queen hatao → next column try karo.**

---

# Final Takeaway

N-Queens ka pura concept:

Row choose karo
→ safe column check karo
→ queen place karo
→ next row par recursion
→ solution complete ho to store karo
→ queen remove karo
→ next choice try karo

N-Queens = **Backtracking + Constraint Checking**

Sabse important:

**row = Current row**

**col = Current column**

**isSafe() = Kya queen yahan place ho sakti hai?**

**Place = Queen lagao**

**Recurse = Next row solve karo**

**Remove = Choice undo karo**

**Core Pattern = Place → Recurse → Remove**
