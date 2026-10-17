# Greedy Pattern — Question 2
# LC 860 — Lemonade Change

## 1. Problem Details

- **LeetCode Number:** 860
- **Pattern:** Greedy
- **Difficulty:** Easy
- **Topics:** Greedy, Array

## 2. Problem Statement

We are selling lemonade, and each lemonade costs $5.

Customers stand in a queue. Each customer pays using one of
the following bills:

- $5
- $10
- $20

Initially, we have no money to provide change.

For every customer, we must provide the correct change so that
the customer effectively pays exactly $5.

If we can provide change to every customer, return true.
Otherwise, return false.

Important:
- Customers arrive in the given order.
- We cannot change the order of customers.
- We can only use the bills collected from previous customers
  to provide change.

## 3. Example

Input:

    bills = [5, 5, 5, 10, 20]

Output:

    true

Explanation:

Customer 1 pays $5:
- No change is required.
- We have one $5 bill.

Customer 2 pays $5:
- No change is required.
- We have two $5 bills.

Customer 3 pays $5:
- No change is required.
- We have three $5 bills.

Customer 4 pays $10:
- Return one $5 bill as change.
- We now have two $5 bills and one $10 bill.

Customer 5 pays $20:
- Return one $10 bill and one $5 bill as change.
- The transaction succeeds.

Every customer receives the correct change.

Therefore, the answer is true.

## 4. Greedy Concept

Greedy means making a suitable choice at every step to reach
the overall goal.

In this problem, we need to give correct change to every
customer using the bills currently available.

We maintain two counters:

    five = Number of $5 bills
    ten  = Number of $10 bills

We do not need to track $20 bills because we never use a $20
bill to provide change.

### Main Greedy Decision

When a customer pays $20, we need to return $15.

There are two possible combinations:

1. One $10 bill + one $5 bill
2. Three $5 bills

We prefer the first combination whenever possible.

Why?

Because $5 bills are more useful for future customers who
pay $10 and require $5 as change.

Therefore, we try to preserve $5 bills whenever we can.

## 5. Approach — Step by Step

### Step 1: Initialize counters

    int five = 0;
    int ten = 0;

Initially, we have no bills.

### Step 2: Process customers in order

Traverse the `bills` array using a loop.

For each customer, check which bill they have paid.

### Case 1: Customer pays $5

No change is required.

    five++;

We keep the $5 bill.

### Case 2: Customer pays $10

We must return $5 as change.

If no $5 bill is available:

    return false;

Otherwise:

    five--;
    ten++;

We use one $5 bill as change and keep the received $10 bill.

### Case 3: Customer pays $20

We must return $15 as change.

First, check whether we have both a $10 and a $5 bill.

    if (ten > 0 && five > 0)

If yes:

    ten--;
    five--;

Otherwise, check if we have at least three $5 bills.

    else if (five >= 3)

If yes:

    five -= 3;

If neither combination is possible:

    return false;

### Step 3: Finish processing

If every customer receives the correct change, return:

    true

## 6. C++ Code

```cpp
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int ten = 0, five = 0;

        for (int bill : bills) {
            if (bill == 5) {
                five++;
            }
            else if (bill == 10) {
                if (five == 0) {
                    return false;
                }

                five--;
                ten++;
            }
            else {
                if (ten > 0 && five > 0) {
                    ten--;
                    five--;
                }
                else if (five >= 3) {
                    five -= 3;
                }
                else {
                    return false;
                }
            }
        }

        return true;
    }
};
```

## 7. Code Explanation

### A. Initialize the counters

```cpp
int ten = 0, five = 0;
```

`five` stores the number of $5 bills.

`ten` stores the number of $10 bills.

Initially, both are zero because we start without any money.

### B. Process every customer

```cpp
for (int bill : bills)
```

This loop processes each customer's bill in the original order.

For example, if:

    bills = [5, 10, 20]

Then the loop processes $5 first, then $10, and finally $20.

### C. Customer pays $5

```cpp
if (bill == 5) {
    five++;
}
```

Lemonade costs $5, so no change is required.

We simply increase the number of $5 bills.

### D. Customer pays $10

```cpp
else if (bill == 10) {
    if (five == 0) {
        return false;
    }

    five--;
    ten++;
}
```

The customer needs $5 in change.

If `five == 0`, we cannot provide change, so we return false.

Otherwise:
- Decrease `five` because we give away one $5 bill.
- Increase `ten` because we receive one $10 bill.

### E. Customer pays $20

```cpp
else {
    if (ten > 0 && five > 0) {
        ten--;
        five--;
    }
    else if (five >= 3) {
        five -= 3;
    }
    else {
        return false;
    }
}
```

We need to return $15 in change.

Preferred option:

    $10 + $5 = $15

If this combination is available, use it.

Otherwise, try:

    $5 + $5 + $5 = $15

If neither option is available, we cannot give the correct
change, so return false.

### F. Return true

```cpp
return true;
```

This line executes only when every customer has received
the correct change.

## 8. Detailed Dry Run

Input:

    bills = [5, 5, 10, 20]

Initially:

    five = 0
    ten = 0

### Step 1: Customer pays $5

Current bill:

    bill = 5

The condition `bill == 5` is true.

Update:

    five = 1
    ten = 0

No change is required.

### Step 2: Customer pays $5

Current bill:

    bill = 5

Again, no change is required.

Update:

    five = 2
    ten = 0

### Step 3: Customer pays $10

Current bill:

    bill = 10

We need to provide $5 as change.

Check:

    five == 0

This is false because `five = 2`.

So we execute:

    five--;
    ten++;

New values:

    five = 1
    ten = 1

We gave one $5 bill as change and received one $10 bill.

### Step 4: Customer pays $20

Current bill:

    bill = 20

We need to provide $15 as change.

First, check:

    ten > 0 && five > 0

Both conditions are true:

    ten = 1
    five = 1

Therefore, we use one $10 bill and one $5 bill.

Update:

    ten = 0
    five = 0

The customer receives the correct $15 change.

### Final Result

Every customer received the correct change.

    return true;

Output:

    true

## 9. Another Important Example

Input:

    bills = [5, 5, 10, 10, 20]

Process:

Customer 1 pays $5:
- five = 1
- ten = 0

Customer 2 pays $5:
- five = 2
- ten = 0

Customer 3 pays $10:
- Give one $5 as change.
- five = 1
- ten = 1

Customer 4 pays $10:
- Give one $5 as change.
- five = 0
- ten = 2

Customer 5 pays $20:
- Need $15 as change.
- No $5 bill is available.
- Three $5 bills are also unavailable.

We cannot provide change.

Output:

    false

## 10. Dry Run Table

Input:

    bills = [5, 5, 10, 20]

| Step | Bill | Action | five | ten |
|---|---:|---|---:|---:|
| Initial | - | No bills | 0 | 0 |
| 1 | 5 | Keep $5 | 1 | 0 |
| 2 | 5 | Keep $5 | 2 | 0 |
| 3 | 10 | Give $5 change | 1 | 1 |
| 4 | 20 | Give $10 + $5 | 0 | 0 |

Final Output:

    true

## 11. Complexity Analysis

Let `n` be the number of customers.

### Time Complexity: O(n)

We process every bill exactly once.

Each customer requires only a constant number of operations.

Therefore:

    O(n)

### Space Complexity: O(1)

We only maintain two counters:

    five
    ten

No additional data structure is required.

Therefore:

    O(1)

## 12. Important Interview Points

1. Process customers in the given order.
2. A $5 payment requires no change.
3. A $10 payment requires one $5 bill.
4. A $20 payment requires $15 in change.
5. For a $20 payment, prefer one $10 + one $5 when available.
6. If neither valid change combination is possible, return false.
7. Use `if -> else if -> else` because each customer pays using
   exactly one bill denomination.
8. Only track $5 and $10 bills.
9. Return true only if every customer receives correct change.

## 13. Key Takeaway

The main Greedy decision is:

**For a $20 payment, prefer giving one $10 bill and one $5 bill
instead of three $5 bills.**

This preserves smaller bills for future customers and helps
us provide change successfully.

## Filename

02-860-lemonade-change.md
