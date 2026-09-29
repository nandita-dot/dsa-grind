# Maximum Units on a Truck

## Problem

You are given an array `boxTypes` where:

```text
boxTypes[i] = [numberOfBoxes, unitsPerBox]
```

Each box type contains a certain number of boxes, and every box of that type contains the same number of units.

You also have a truck that can carry at most `truckSize` boxes.

Return the **maximum total number of units** that can be loaded onto the truck.

---

## Link

LeetCode: Maximum Units on a Truck

## Difficulty

Easy

## Pattern

**Greedy — Sort by Value per Unit of Capacity**

---

## First Thought

Initially, the idea was to prioritize box types with:

* More boxes
* More units per box

The important correction is that the **number of boxes does not determine priority**.

Every box consumes exactly **one slot** in the truck.

Therefore, the important factor is:

```text
unitsPerBox
```

If one truck slot can give us more units, we should use that slot for the higher-value box.

---

## Observation

Taking the second type:
```text
```

## Greedy Choice

Sort all box types by:

```text
in **descending order**.

If the current type contains more boxes than the remaining truck capacity, take only the number that fits.
---
## Why Is This Safe?

Every box consumes exactly one unit of truck capacity.

Therefore, each available truck slot should be assigned to the box that gives the maximum number of units.

If a box gives fewer units than another available box, replacing the lower-value box with the higher-value box can never decrease the total number of units.
Therefore, filling the truck with the highest `unitsPerBox` boxes first is optimal.

---

## Algorithm

1. Sort `boxTypes` in descending order of `unitsPerBox`.
2. Initialize:

   * `totalUnits = 0`
   * `truckSize` as the remaining capacity.
3. For each box type:

   * Take:

```java
Math.min(numberOfBoxes, truckSize)
```

boxes.
4. Add:

```text
boxesTaken × unitsPerBox
```

to `totalUnits`.
5. Reduce the remaining truck capacity.
6. Stop when the truck is full.
7. Return `totalUnits`.

---

## Code

```java
class Solution {
    public int maximumUnits(int[][] boxTypes, int truckSize) {

        Arrays.sort(boxTypes, (a, b) -> b[1] - a[1]);

        int totalUnits = 0;

        for(int[] box : boxTypes) {

            if(truckSize == 0) {
                break;
            }

            int boxesTaken = Math.min(box[0], truckSize);

            totalUnits += boxesTaken * box[1];

            truckSize -= boxesTaken;
        }

        return totalUnits;
    }
}
```

---

## Example

```text
boxTypes = [
    [1, 3],
    [2, 2],
    [3, 1]
]

truckSize = 4
```

### Step 1 — Sort by units per box

```text
[1, 3]
[2, 2]
[3, 1]
```

Already sorted.

### Step 2 — Take from the highest-value type

Take:

```text
1 box × 3 units = 3
```

Remaining capacity:

```text
4 - 1 = 3
```

### Step 3

Take:

```text
2 boxes × 2 units = 4
```

Remaining capacity:

```text
3 - 2 = 1
```

### Step 4

Only one slot remains.

Take:

```text
1 box × 1 unit = 1
```

Total:

```text
3 + 4 + 1 = 8
```

Answer:

```text
8
```

---

## Important Detail: Partial Selection

We don't have to take every box from a type.

For example:

```text
truckSize = 4

current box type:
[10, 5]
```

There are 10 boxes available, but only 4 truck slots.

Therefore:

```java
int boxesTaken = Math.min(10, 4);
```

gives:

```text
boxesTaken = 4
```

Contribution:

```text
4 × 5 = 20 units
```

The truck is now full.

---

## Complexity

Let `n` be the number of box types.

* Sorting: `O(n log n)`
* Iterating through box types: `O(n)`
* Auxiliary Space: `O(1)` apart from the sorting implementation.

Overall:

```text
Time:  O(n log n)
Space: O(1) auxiliary
```

---

## Recognition Clues

Look for this greedy pattern when:

* There is a limited capacity/resource.
* Each selected item consumes the same amount of that resource.
* Different items provide different values.
* You want to maximize total value.

Ask:

> **Which choice gives me the most value for each unit of my limited resource?**

Here:

```text
Resource = truck capacity
Cost = 1 box slot
Value = units per box
```

Therefore:

```text
highest units per box → take first
```

---

## Mistakes / Lessons

### Mistake 1: Prioritizing the number of boxes

Having more boxes available does not make a box type more valuable.

Example:

```text
[100, 1]
[2, 10]
```

The second type is more valuable because every occupied truck slot gives more units.

### Mistake 2: Thinking we must take an entire box type

We can take only part of a type.

Use:

```java
Math.min(numberOfBoxes, truckSize)
```

to determine how many boxes fit.

---

## Pattern Summary

**Greedy rule:**

> When every item consumes the same amount of limited capacity, prioritize the item with the highest value.

For this problem:

```text
Sort by unitsPerBox ↓
        ↓
Take as many as possible
        ↓
Move to the next highest-value type
        ↓
Stop when truck is full
```




Then take as many boxes as possible from the highest-value type before moving to the next type.
unitsPerBox
```


---

Therefore, we should prioritize the type with the greater `unitsPerBox`.
2 × 10 = 20 units

```

2 × 1 = 2 units

```text

Taking the first type:
If the truck has only 2 available slots:

Consider:

