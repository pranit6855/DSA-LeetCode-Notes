# Greedy Pattern — Question 4
# LC 1710 — Maximum Units on a Truck

## 1. Problem Details

- **LeetCode Number:** 1710
- **Pattern:** Greedy
- **Difficulty:** Easy
- **Topics:** Greedy, Sorting, Array

## 2. Problem Statement

We are given a 2D array `boxTypes`.

Each entry contains two values:

    boxTypes[i] = [numberOfBoxes, numberOfUnitsPerBox]

- `boxTypes[i][0]` represents the number of available boxes.
- `boxTypes[i][1]` represents the units inside each box.

We are also given `truckSize`, which represents the maximum
number of boxes that the truck can carry.

Our task is to load boxes into the truck so that the total
number of units is maximized.

Return the maximum possible number of units.

Important:
- The truck capacity is measured in boxes, not units.
- We can take only the available number of boxes of each type.
- We can take fewer boxes than are available if the truck
  does not have enough capacity.

## 3. Example

Input:

    boxTypes = [[3,1], [1,3], [2,2]]
    truckSize = 4

Output:

    8

Explanation:

There are three box types:

| Box Type | Available Boxes | Units Per Box |
|---|---:|---:|
| Type 1 | 3 | 1 |
| Type 2 | 1 | 3 |
| Type 3 | 2 | 2 |

The truck can carry only 4 boxes.

The best selection is:

- 1 box containing 3 units = 3 units
- 2 boxes containing 2 units each = 4 units
- 1 box containing 1 unit = 1 unit

Total boxes loaded:

    1 + 2 + 1 = 4

Total units:

    3 + 4 + 1 = 8

Therefore, the answer is 8.

## 4. Greedy Concept

Greedy means making a suitable choice at each step to
maximize the overall result.

In this problem, we want to maximize the number of units
inside a truck with limited capacity.

The main idea is:

**Load boxes with the highest units per box first.**

Why?

Suppose one box contains 5 units and another contains
only 2 units.

If the truck has space for only one box, choosing the
first box gives us more units.

Therefore, we sort box types in descending order of
units per box.

Descending order means:

    Highest units -> Lowest units

## 5. Approach — Step by Step

### Step 1: Get the number of box types

    int n = boxTypes.size();

This tells us how many different box types are available.

### Step 2: Sort using two for loops

We compare the units per box of each pair of box types.

    if (boxTypes[i][1] < boxTypes[j][1])

If the current box type has fewer units per box than the
later box type, swap them.

    swap(boxTypes[i], boxTypes[j]);

After sorting, the box type with the highest units per
box comes first.

We are using simple nested loops to understand sorting.

### Step 3: Initialize total units

    int totalUnits = 0;

This stores the total units loaded into the truck.

### Step 4: Traverse the sorted box types

For every box type, check how many boxes we can load.

If all available boxes fit:

    totalUnits += boxTypes[i][0] * boxTypes[i][1];
    truckSize -= boxTypes[i][0];

If all available boxes do not fit, load only the remaining
truck capacity:

    totalUnits += truckSize * boxTypes[i][1];
    truckSize = 0;

### Step 5: Stop when the truck is full

    if (truckSize == 0) {
        break;
    }

There is no space left, so we stop loading.

### Step 6: Return the answer

    return totalUnits;

The final value contains the maximum number of units loaded.

## 6. Beginner-Friendly C++ Code

```cpp
class Solution {
public:
    int maximumUnits(vector<vector<int>>& boxTypes, int truckSize) {
        int n = boxTypes.size();

        // Step 1: Sort boxes by units per box
        // Highest units first
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (boxTypes[i][1] < boxTypes[j][1]) {
                    swap(boxTypes[i], boxTypes[j]);
                }
            }
        }

        // Step 2: Load boxes into the truck
        int totalUnits = 0;

        for (int i = 0; i < n; i++) {
            if (boxTypes[i][0] <= truckSize) {
                totalUnits += boxTypes[i][0] * boxTypes[i][1];
                truckSize -= boxTypes[i][0];
            }
            else {
                totalUnits += truckSize * boxTypes[i][1];
                truckSize = 0;
            }

            if (truckSize == 0) {
                break;
            }
        }

        return totalUnits;
    }
};
```

## 7. Code Explanation

### A. Get the number of box types

```cpp
int n = boxTypes.size();
```

Stores the number of rows in `boxTypes`.

For example:

    boxTypes = [[3,1], [1,3], [2,2]]

Here, `n = 3` because there are three box types.

### B. Sort using nested loops

```cpp
for (int i = 0; i < n; i++) {
    for (int j = i + 1; j < n; j++) {
        if (boxTypes[i][1] < boxTypes[j][1]) {
            swap(boxTypes[i], boxTypes[j]);
        }
    }
}
```

We compare the second value of each row.

Remember:

    boxTypes[i][0] = Available boxes
    boxTypes[i][1] = Units per box

If the current row has fewer units per box, we swap the
rows so the higher-value box type comes first.

Example:

    Before: [[3,1], [1,3], [2,2]]
    After:  [[1,3], [2,2], [3,1]]

Now the most valuable boxes are at the beginning.

### C. Initialize total units

```cpp
int totalUnits = 0;
```

Initially, we have not loaded any boxes.

Therefore, the total units are zero.

### D. Check whether all boxes fit

```cpp
if (boxTypes[i][0] <= truckSize) {
    totalUnits += boxTypes[i][0] * boxTypes[i][1];
    truckSize -= boxTypes[i][0];
}
```

If all available boxes of the current type fit, load all of
them.

The total units increase by:

    Number of boxes * Units per box

The truck capacity decreases by the number of boxes loaded.

### E. Handle boxes that do not all fit

```cpp
else {
    totalUnits += truckSize * boxTypes[i][1];
    truckSize = 0;
}
```

If the available boxes exceed the remaining capacity, load
only as many boxes as the truck can carry.

For example:

    Available boxes = 3
    Remaining capacity = 1
    Units per box = 2

Only one box can be loaded, adding 2 units.

The truck becomes full.

### F. Stop when the truck is full

```cpp
if (truckSize == 0) {
    break;
}
```

Once the remaining capacity is zero, no additional boxes
can be loaded.

Therefore, we stop the loop.

### G. Return the maximum units

```cpp
return totalUnits;
```

Returns the total number of units loaded into the truck.

## 8. Detailed Dry Run

Input:

    boxTypes = [[3,1], [1,3], [2,2]]
    truckSize = 4

### Step 1: Initial State

    boxTypes = [[3,1], [1,3], [2,2]]
    truckSize = 4
    totalUnits = 0

### Step 2: Sort the box types

Compare `[3,1]` and `[1,3]`.

Their units per box are 1 and 3.

Since:

    1 < 3

We swap them.

    [[1,3], [3,1], [2,2]]

Next, compare `[1,3]` and `[2,2]`.

Since:

    3 < 2

This is false, so no swap is needed.

Finally, compare `[3,1]` and `[2,2]`.

Since:

    1 < 2

We swap them.

Sorted result:

    [[1,3], [2,2], [3,1]]

### Step 3: Load the first box type [1,3]

Available boxes:

    1

Units per box:

    3

Remaining truck capacity:

    4

Check:

    1 <= 4

True, so we load all 1 box.

Calculation:

    totalUnits = 0 + (1 * 3) = 3
    truckSize = 4 - 1 = 3

Current state:

    totalUnits = 3
    truckSize = 3

### Step 4: Load the second box type [2,2]

Available boxes:

    2

Units per box:

    2

Remaining truck capacity:

    3

Check:

    2 <= 3

True, so we load both boxes.

Calculation:

    totalUnits = 3 + (2 * 2) = 7
    truckSize = 3 - 2 = 1

Current state:

    totalUnits = 7
    truckSize = 1

### Step 5: Load the third box type [3,1]

Available boxes:

    3

Units per box:

    1

Remaining truck capacity:

    1

Check:

    3 <= 1

False.

We cannot load all three boxes, so we load only one box.

Calculation:

    totalUnits = 7 + (1 * 1) = 8
    truckSize = 0

The truck is full, so the loop stops.

### Final Answer

    return totalUnits;

Output:

    8

## 9. Dry Run Table

| Step | Box Type | Boxes Loaded | Units Added | Total Units | Capacity Left |
|---|---|---:|---:|---:|---:|
| Initial | - | 0 | 0 | 0 | 4 |
| 1 | [1,3] | 1 | 3 | 3 | 3 |
| 2 | [2,2] | 2 | 4 | 7 | 1 |
| 3 | [3,1] | 1 | 1 | 8 | 0 |

Final output:

    8

## 10. Complexity Analysis

Let `n` be the number of box types.

### Time Complexity: O(n²)

The sorting section contains two nested loops.

In the worst case, each loop compares box types, resulting
in quadratic time.
