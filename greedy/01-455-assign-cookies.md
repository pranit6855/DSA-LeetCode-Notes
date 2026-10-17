# Greedy Pattern — Question 1
# LC 455 — Assign Cookies

## 1. Problem Details

- **LeetCode Number:** 455
- **Pattern:** Greedy
- **Difficulty:** Easy
- **Topics:** Sorting, Two Pointers, Greedy

## 2. Problem Statement

We are given two arrays:

- `g[i]` = Minimum cookie size required to satisfy child `i`.
- `s[j]` = Size of cookie `j`.

Each child can receive at most one cookie, and each cookie can be
assigned to at most one child.

A child is satisfied if:

    cookie size >= child's requirement

Our task is to return the maximum number of children that can be
satisfied.

## 3. Example

Input:

    g = [1, 2, 3]
    s = [1, 1, 2]

Output:

    2

Explanation:

- Child with requirement 1 receives a cookie of size 1.
- Child with requirement 2 receives a cookie of size 2.
- Child with requirement 3 cannot be satisfied because no cookie
  of size 3 or greater is available.

Therefore, the maximum number of satisfied children is 2.

## 4. Greedy Concept

Greedy means making a suitable choice at each step to reach
the overall optimal answer.

In this problem, we want to satisfy as many children as possible.

The main idea is:

**Give the smallest suitable cookie to the smallest unsatisfied
requirement.**

Why?

Suppose a child needs a cookie of size 1 and another child needs
a cookie of size 3.

If we give the first child a cookie of size 3, we might waste a
large cookie that could have satisfied the second child.

Therefore, we try to satisfy smaller requirements using smaller
cookies first.

## 5. Approach — Sorting + Two Pointers

### Step 1: Sort both arrays

Sort `g` in ascending order.

Sort `s` in ascending order.

Example:

    g = [1, 2, 3]
    s = [1, 1, 2]

Now we can process the smallest requirement and smallest cookie
first.

### Step 2: Create two pointers

    i = 0  // Current child
    j = 0  // Current cookie

### Step 3: Compare the requirement and cookie

If:

    s[j] >= g[i]

The cookie is large enough to satisfy the child.

- Move to the next child: `i++`
- Move to the next cookie: `j++`

Otherwise:

    s[j] < g[i]

The cookie is too small for the current child.

- Skip that cookie: `j++`
- Keep the child pointer unchanged.

We can try a larger cookie for the same child.

### Step 4: Stop when either array ends

Continue while:

    i < g.size() && j < s.size()

### Step 5: Return the answer

Every successful assignment increases `i` by one.

Therefore:

    return i;

The final value of `i` represents the number of satisfied children.

## 6. C++ Code

```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());

        int i = 0, j = 0;
        int n = g.size();
        int m = s.size();

        while (i < n && j < m) {
            if (s[j] >= g[i]) {
                i++;
                j++;
            } else {
                j++;
            }
        }

        return i;
    }
};
```

## 7. Code Explanation

### A. Sort both arrays

```cpp
sort(g.begin(), g.end());
sort(s.begin(), s.end());
```

Both arrays are sorted in ascending order.

This allows us to begin with the smallest requirement and
smallest available cookie.

### B. Initialize pointers

```cpp
int i = 0, j = 0;
int n = g.size();
int m = s.size();
```

- `i` points to the current child.
- `j` points to the current cookie.
- `n` is the number of children.
- `m` is the number of cookies.

### C. Loop condition

```cpp
while (i < n && j < m)
```

Continue while at least one child and one cookie remain
available for matching.

### D. Check whether the cookie is sufficient

```cpp
if (s[j] >= g[i]) {
    i++;
    j++;
}
```

If the cookie size is greater than or equal to the child's
requirement, the child is satisfied.

Both pointers move forward because neither the child nor the
cookie can be reused.

### E. Skip a cookie that is too small

```cpp
else {
    j++;
}
```

If the cookie is smaller than the current child's requirement,
it cannot satisfy that child.

Because the arrays are sorted, we move to the next, larger
cookie while keeping the same child.

### F. Return the count

```cpp
return i;
```

The child pointer increases only when a child is satisfied.

Therefore, `i` is the total number of satisfied children.

## 8. Detailed Dry Run

Input:

    g = [1, 2, 3]
    s = [1, 1, 2]

Both arrays are already sorted.

Initial values:

    i = 0
    j = 0

### Step 1

Current child requirement:

    g[i] = g[0] = 1

Current cookie size:

    s[j] = s[0] = 1

Comparison:

    1 >= 1  // True

The child is satisfied.

Update:

    i = 1
    j = 1

Arrays remain:

    g = [1, 2, 3]
    s = [1, 1, 2]

### Step 2

Current child requirement:

    g[i] = g[1] = 2

Current cookie size:

    s[j] = s[1] = 1

Comparison:

    1 >= 2  // False

The cookie is too small.

Only the cookie pointer moves:

    i = 1
    j = 2

The child with requirement 2 is still waiting.

### Step 3

Current child requirement:

    g[i] = g[1] = 2

Current cookie size:

    s[j] = s[2] = 2

Comparison:

    2 >= 2  // True

The child is satisfied.

Update:

    i = 2
    j = 3

Now all cookies have been used.

The loop stops because:

    j == m

Return:

    return i;  // 2

Final output:

    2

## 9. Dry Run Table

| Step | Child Requirement | Cookie Size | Decision | i | j |
|---|---:|---:|---|---:|---:|
| Initial | - | - | Start | 0 | 0 |
| 1 | 1 | 1 | Satisfy child | 1 | 1 |
| 2 | 2 | 1 | Skip cookie | 1 | 2 |
| 3 | 2 | 2 | Satisfy child | 2 | 3 |
| End | 3 | No cookies left | Stop | 2 | 3 |

**Final Answer: 2**

## 10. Complexity Analysis

Let:

- `n` = Number of children.
- `m` = Number of cookies.

### Time Complexity

Sorting `g` takes O(n log n).

Sorting `s` takes O(m log m).

The two-pointer traversal takes O(n + m).

Overall:

    O(n log n + m log m)

### Space Complexity

The two-pointer traversal uses O(1) extra space.

C++ sorting implementations may use O(log n + log m) auxiliary
stack space.

## 11. Important Interview Points

1. Sort both arrays before using two pointers.
2. A child is satisfied when `cookie size >= requirement`.
3. If a cookie is too small, skip the cookie, not the child.
4. Move both pointers when a child is satisfied.
5. The final value of `i` gives the number of satisfied children.
6. Greedy works here because assigning the smallest suitable
   cookie preserves larger cookies for children with larger needs.

## 12. Key Takeaway

**Smallest suitable cookie → smallest unsatisfied requirement.**

Sorting and two pointers help us find the maximum number of
children that can be satisfied without wasting larger cookies.

## Filename

01-455-assign-cookies.md
