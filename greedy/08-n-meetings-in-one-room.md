# N Meetings in One Room

## Problem

You are given `n` meetings represented by two arrays:

```text
start[i] = starting time of meeting i
end[i]   = ending time of meeting i
```

Only one meeting can happen in the room at a time.

The goal is to select the **maximum number of non-overlapping meetings**.

If the problem asks for meeting numbers, return their **original 1-based indices**.

---

## Example

```text
start = [1, 3, 0, 5, 8, 5]
end   = [2, 4, 6, 7, 9, 9]
```

Possible selection:

```text
(1,2) → (3,4) → (5,7) → (8,9)
```

So the maximum number of meetings is:

```text
4
```

---

## Pattern

**Greedy — Activity Selection / Earliest Finish Time**

---

## First Thought

We want to fit as many meetings as possible.

A meeting that finishes earlier leaves more time for future meetings.

Therefore:

> **Always choose the meeting that finishes earliest.**

---

## Observation

Consider:

```text
(1,2)
(0,6)
```

If we choose `(0,6)`, we block the possibility of attending several later meetings.

If we choose `(1,2)`, the room becomes available much earlier.

Therefore, **earliest finishing time** is the greedy criterion.

---

## Greedy Choice

1. Sort all meetings by their ending time.
2. Select the first meeting.
3. For every subsequent meeting:

   * If its start time is after the previous selected meeting's end time, select it.
4. Continue until all meetings are processed.

The compatibility condition is:

```java
current.start > lastEnd
```

---

## Why Is This Safe?

Suppose two available meetings are:

```text
A → ends at 4
B → ends at 7
```

Choosing A leaves the room free earlier.

Any meeting that could be scheduled after B can also potentially be scheduled after A.

Therefore, choosing the earliest finishing available meeting cannot reduce the number of meetings we can schedule later.

---

## Algorithm

### Step 1 — Store the original index

We need the original meeting number because sorting changes the order.


### Step 2 — Sort by ending time


```java

Arrays.sort(meetings, (a, b) -> a.end - b.end);

```


### Step 3 — Select the first meeting


```java
answer.add(meetings[0].index);

lastEnd = meetings[0].end;
```


### Step 4 — Select compatible meetings


For every remaining meeting:


```java
if (meetings[i].start > lastEnd)

```

select it and update `lastEnd`.

---

## Code


```java
class Meeting {

    int start;
    int end;
    int index;


    Meeting(int start, int end, int index) {
        this.start = start;

        this.end = end;
        this.index = index;
    }
}


class Solution {
    public ArrayList<Integer> maxMeetings(int[] s, int[] f) {
        int n = s.length;

        Meeting[] meetings = new Meeting[n];

        for (int i = 0; i < n; i++) {
            meetings[i] = new Meeting(s[i], f[i], i + 1);
        }

        Arrays.sort(meetings, (a, b) -> a.end - b.end);

        ArrayList<Integer> answer = new ArrayList<>();

        answer.add(meetings[0].index);
        int lastEnd = meetings[0].end;

        for (int i = 1; i < n; i++) {

            if (meetings[i].start > lastEnd) {
                answer.add(meetings[i].index);
                lastEnd = meetings[i].end;
            }
        }

        return answer;
    }
}
```

---

## Important Mistake

### Sorted index ≠ Original index

After:

```java
Arrays.sort(meetings, ...)
```

`i` no longer represents the original meeting number.

For example:

```text
Original:
Meeting 1 → (5, 6)
Meeting 2 → (1, 2)
Meeting 3 → (3, 4)
```

After sorting:

```text
Meeting 2 → (1,2)
Meeting 3 → (3,4)
Meeting 1 → (5,6)
```

Therefore, we must store:

```java
int index;
```

inside the `Meeting` object.

---

## Important Boundary Condition

The code uses:

```java
meetings[i].start > lastEnd
```

not:

```java
meetings[i].start >= lastEnd
```

because this version of the problem requires the next meeting to start **after** the previous meeting ends.

Always check the exact overlap condition specified by the problem.

---

## Complexity

Sorting:

```text
O(n log n)
```

Traversal:

```text
O(n)
```

Overall:


```text
O(n log n)

```

Auxiliary space:

```text

O(n)
```


because we store the meeting objects.


---


## Recognition Clues


Think **Activity Selection / Earliest Finish Time** when:


* You have activities with start and end times.

* Activities cannot overlap.
* You want to maximize the number of activities selected.
* Choosing one activity affects which activities remain available.


The key question:

> **Which choice leaves the most room for future choices?**


Answer:


> **The activity that finishes earliest.**

---


## Pattern Summary

```text
Sort by end time
       ↓
Choose earliest finishing meeting
       ↓
Skip overlapping meetings

       ↓
Choose next compatible meeting
       ↓
Maximize number of meetings
```

### Core Greedy Insight

> **Finish early → leave more room → fit more activities.**

