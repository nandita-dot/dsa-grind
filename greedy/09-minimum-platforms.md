# Minimum Platforms

## Problem

Given the arrival and departure times of trains at a railway station, find the **minimum number of platforms required** so that no train has to wait.

A platform can be reused once the train occupying it has departed.

### Example

```text
arr = [900, 940, 950, 1100, 1500, 1800]
dep = [910, 1200, 1120, 1130, 1900, 2000]
```

The answer is:

```text
3
```

because at one point, 3 trains are simultaneously present.

---

## Pattern

**Greedy — Event Sweep / Two Pointers**

The problem is similar to interval problems, but we are **not selecting intervals**.

Instead, we track how many trains are present at every point in time.

---

## Key Observation

At any arrival time:

> If the earliest departure has not happened yet, the arriving train needs another platform.

So we compare:

```text
next arrival
vs
next departure
```

### Arrival comes first

```text
arr[i] <= dep[j]
```

A train arrives before the earliest current departure.

Therefore:

```text
platform++
```

### Departure comes first

```text
arr[i] > dep[j]
```

A train has already left.

Therefore:

```text
platform--
```

---

## Why Sort Both Arrays?

Sorting lets us process events chronologically.

Example:

```text
arrivals:    900   940   950   1100
              ↑

departures:  910   1120  1130  1200
              ↑
```

At every step, compare the earliest unprocessed arrival and departure.

This tells us which event happens next.

---

## Greedy Choice

At every step:

```text
If arrival <= departure:
    A train arrives first
    → allocate a platform

Else:
    A train departs first
    → free a platform
```

Keep track of the **maximum number of platforms occupied simultaneously**.

That maximum is the answer.

---

## Algorithm

1. Sort `arr`.
2. Sort `dep`.
3. Maintain two pointers:

   * `i` → next arrival
   * `j` → next departure
4. Maintain:

   * `platform` → currently occupied platforms
   * `count` → maximum platforms required so far
5. While both pointers are valid:

   * If `arr[i] <= dep[j]`:

     * increment `platform`
     * increment `i`
   * Otherwise:

     ```java
     ```
---

## Code
```java
class Solution {
    public int minPlatform(int arr[], int dep[]) {
        Arrays.sort(arr);
        Arrays.sort(dep);


        int i = 0;
        int j = 0;

        while(i < arr.length && j < dep.length) {
                platform++;
                i++;
            } else {
                j++;
            }

            count = Math.max(platform, count);
        }

    }
}
```

---

## Example Walkthrough

```text
arr = [900, 940, 950]
dep = [910, 1120, 1200]
```

### Step 1

```text
900 <= 910
```

Arrival happens first:

```text
platform = 1
```

### Step 2

```text
940 <= 910 → false
```

Departure happens first:

```text
platform = 0
```

### Step 3

```text
940 <= 1120
```

Arrival:

```text
platform = 1
```

### Step 4

```text
950 <= 1120
```

Another arrival:

```text
platform = 2
```

Therefore:

```text
answer = 2
```

---

## Important Boundary Condition

The code uses:

```java
arr[i] <= dep[j]
```

rather than:

```java
arr[i] < dep[j]
```

For the standard GFG version, if one train arrives at exactly the same time another train departs, the arriving train requires another platform.

        return count;
                platform--;

            if(arr[i] <= dep[j]) {
        int count = 1;
So:
        int platform = 0;


```text
arrival == departure
        ↓


arrival is processed first
6. Return `count`.
     count = Math.max(platform, count);
        ↓

     * decrement `platform`
platform++
```


Always check the exact overlap condition in the problem statement.


---


## Mistake / Failed Approach


An initial approach was to combine all arrivals and departures into one array and sort it.

The problem with that approach:

```text

[900, 910, 940, 1200]
```


After sorting, we no longer know whether a particular time represents:

```text

arrival
```

or:


```text

departure
```

Therefore, simply comparing adjacent times cannot tell us whether to increment or decrement the platform count.


### Lesson


> When processing events, you must preserve the **type of each event**.

Sorting arrivals and departures separately solves this cleanly.

---


## Complexity

Sorting arrivals:

```text
O(n log n)
```

Sorting departures:

```text

O(n log n)
```

Two-pointer traversal:

```text
O(n)
```

Overall:

```text
O(n log n)
```

Auxiliary space:

```text
O(1)
```

excluding the space used internally by sorting.

---

## Recognition Clues

Think of this pattern when:

* You have **start/end events**.
* You need the maximum number of overlapping intervals.
* You need to know how many resources are required simultaneously.
* Examples include:

  * railway platforms
  * meeting rooms
  * CPU/resource allocation
  * concurrent processes

The key question is:

> **How many intervals are active at the same time?**

Think:

```text
Sort starts
Sort ends
      ↓
Two pointers
      ↓
Start → resource++
End   → resource--
      ↓
Track maximum
```

---

## Pattern Summary

### N Meetings

```text
Goal: maximize activities
Strategy: choose earliest ending activity
Pattern: Activity Selection
```

### Minimum Platforms

```text
Goal: minimize resources required
Strategy: process arrivals/departures chronologically
Pattern: Event Sweep + Two Pointers
```

The core insight:

> **The answer is the maximum number of trains simultaneously present at the station.**

