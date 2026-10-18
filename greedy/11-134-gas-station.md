# Greedy Pattern — Question 11
# LC 134 — Gas Station

## 1. Problem Details

- **LeetCode Number:** 134
- **Pattern:** Greedy
- **Difficulty:** Medium
- **Topics:** Greedy, Array

## 2. Problem Statement

We are given two integer arrays:

- `gas[i]`: The amount of gas available at station `i`.
- `cost[i]`: The amount of gas required to travel from station `i`
  to the next station.

The gas stations form a circular route.

We start with an empty tank and must find a starting station
from which we can complete the entire circular journey.

If completing the journey is impossible, return -1.

If a valid starting station exists, return its index.

Important:
- The tank has unlimited capacity.
- We must have enough gas to travel to the next station.
- After the last station, we return to the starting station.
- If a solution exists, the problem guarantees it is unique.

## 3. Example

Input:

    gas  = [1, 2, 3, 4, 5]
    cost = [3, 4, 5, 1, 2]

Output:

    3

Explanation:

At every station, calculate:

    diff = gas[i] - cost[i]

The differences are:

    Station 0: 1 - 3 = -2
    Station 1: 2 - 4 = -2
    Station 2: 3 - 5 = -2
    Station 3: 4 - 1 =  3
    Station 4: 5 - 2 =  3

Starting at station 3, we can complete the circular route.

Therefore, the answer is 3.

## 4. Greedy Concept

Greedy means making a suitable choice at each step to reach
the overall goal efficiently.

In this problem, we track the remaining gas while travelling
through the stations.

For each station:

    diff = gas[i] - cost[i]

If `diff` is positive, we have gas left after travelling
to the next station.

If `diff` is negative, we lose more gas than the station
provides.

We maintain two important balances:

- `tank`: The remaining gas for the current starting candidate.
- `total`: The net gas across all stations.

If `tank` becomes negative, the current starting station
cannot complete the journey through the segment we explored.

We then choose the next station as the new starting candidate
and reset `tank` to zero.

At the end:
- If `total < 0`, the journey is impossible.
- Otherwise, return the candidate starting index.

## 5. Approach — Step by Step

### Step 1: Initialize variables

    int total = 0;
    int tank = 0;
    int start = 0;

- `total` stores the net gas across all processed stations.
- `tank` tracks the current candidate's gas balance.
- `start` stores the candidate starting station index.

### Step 2: Calculate the difference at every station

    int diff = gas[i] - cost[i];

This tells us how much gas is gained or lost after visiting
the current station and travelling to the next one.

### Step 3: Update both balances

    total += diff;
    tank += diff;

`total` accumulates the net gas of the entire route.

`tank` accumulates the gas balance since the current
starting candidate was chosen.

### Step 4: Handle a negative tank

    if (tank < 0) {
        start = i + 1;
        tank = 0;
    }

If the tank becomes negative, the current starting candidate
cannot reach station `i + 1`.

We move the starting candidate to station `i + 1` and
reset the tank to zero.

### Step 5: Check whether the complete journey is possible

    if (total < 0) {
        return -1;
    }

If the total gas available is less than the total gas
required, no starting station can complete the journey.

### Step 6: Return the starting station

    return start;

If the overall gas balance is non-negative, the chosen
starting station can complete the circular route.

## 6. C++ Code

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int total = 0;
        int tank = 0;
        int start = 0;

        for (int i = 0; i < gas.size(); i++) {
            int diff = gas[i] - cost[i];

            total += diff;
            tank += diff;

            if (tank < 0) {
                start = i + 1;
                tank = 0;
            }
        }

        if (total < 0) {
            return -1;
        }

        return start;
    }
};
```

## 7. Code Explanation

### A. Initialize the variables

```cpp
int total = 0;
int tank = 0;
int start = 0;
```

Initially:
- `total = 0`: No stations have been processed.
- `tank = 0`: No gas has been accumulated.
- `start = 0`: Station 0 is our initial starting candidate.

### B. Calculate the net gas

```cpp
int diff = gas[i] - cost[i];
```

Suppose:

    gas[i] = 4
    cost[i] = 1

Then:

    diff = 4 - 1
    diff = 3

We gain a net 3 units of gas at this station after
accounting for the journey to the next station.

If:

    gas[i] = 1
    cost[i] = 3

Then:

    diff = 1 - 3
    diff = -2

We are short by 2 units if we reach this station with an
empty tank.

### C. Update total and tank

```cpp
total += diff;
tank += diff;
```

These variables have different purposes.

`total` tracks the net gas balance across all stations.

`tank` tracks the current starting candidate's gas balance.

We do not reset `total` when the starting candidate changes.
We reset only `tank`.

### D. If the tank becomes negative

```cpp
if (tank < 0) {
    start = i + 1;
    tank = 0;
}
```

A negative tank means the current candidate cannot reach
the next station from the segment we have processed.

We move the candidate to the next station.

Why can we skip all the stations between the old start
and the current station?

If starting from the old candidate leaves us with a negative
balance by station `i`, then starting from any later station
within that failed segment cannot accumulate enough gas
to overcome the same deficit.

Therefore, the next candidate is `i + 1`.

### E. Check the overall balance

```cpp
if (total < 0) {
    return -1;
}
```

The total net gas is:

    Total gas - Total cost

If it is negative, the complete circular route requires
more gas than all stations provide.

No starting station can solve this problem.

### F. Return the answer

```cpp
return start;
```

If `total >= 0`, a valid starting station exists.
Return the starting candidate we found.

## 8. Detailed Dry Run

Input:

    gas  = [1, 2, 3, 4, 5]
    cost = [3, 4, 5, 1, 2]

Initially:

    total = 0
    tank = 0
    start = 0

### Step 1: i = 0

Gas:

    gas[0] = 1

Cost:

    cost[0] = 3

Calculate the difference:

    diff = 1 - 3
    diff = -2

Update:

    total = 0 + (-2) = -2
    tank = 0 + (-2) = -2

Now:

    tank < 0

This is true.

The current candidate, station 0, cannot complete the
first segment.

Update:

    start = 0 + 1 = 1
    tank = 0

Current state:

    total = -2
    tank = 0
    start = 1

### Step 2: i = 1

Calculate:

    diff = gas[1] - cost[1]
    diff = 2 - 4
    diff = -2

Update:

    total = -2 + (-2) = -4
    tank = 0 + (-2) = -2

Since:

    tank < 0

We update:

    start = 1 + 1 = 2
    tank = 0

Current state:

    total = -4
    tank = 0
    start = 2

Station 1 also fails as a starting candidate.

### Step 3: i = 2

Calculate:

    diff = gas[2] - cost[2]
    diff = 3 - 5
    diff = -2

Update:

    total = -4 + (-2) = -6
    tank = 0 + (-2) = -2

Since the tank is negative:

    start = 2 + 1 = 3
    tank = 0

Current state:

    total = -6
    tank = 0
    start = 3

Now station 3 becomes the candidate.

### Step 4: i = 3

Calculate:

    diff = gas[3] - cost[3]
    diff = 4 - 1
    diff = 3

Update:

    total = -6 + 3 = -3
    tank = 0 + 3 = 3

Since:

    tank < 0

is false, we do
