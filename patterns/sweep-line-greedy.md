# Sweep Line Pattern

## What Is Sweep Line?

The **Sweep Line** technique is used when a problem involves things happening over a timeline or coordinate axis.

Imagine a vertical line moving from left to right:

```text
time →
──────────────────────────────→
       ↑
    sweep line
```

As the sweep line moves, it encounters events such as:

* Something starts
* Something ends
* Something enters
* Something leaves

We process these events in order and maintain the current state.

---

# The Core Idea

Suppose we have intervals:

```text
[1, 5]
[2, 4]
[6, 8]
```

Instead of thinking about the entire intervals, convert them into events:

```text
1 → start
5 → end

2 → start
4 → end

6 → start
8 → end
```

Sort the events by time:

```text
1(start)
2(start)
4(end)
5(end)
6(start)
8(end)
```

Now sweep from left to right.

Maintain something like:

```text
active = number of currently active intervals
```

When something starts:

```text
active++
```


```text
active--
```


---
# The General Approach

When you see an interval/timeline problem, ask these questions.

## Step 1 — What are the intervals?


```text
(start, end)
(arrival, departure)
(begin, finish)
```
These usually indicate a possible sweep-line problem.

---


Usually there are two fundamental events:
```text
START → something becomes active
END   → something becomes inactive

For example:

```text
Meeting starts → room needed
```
---

## Step 3 — What state should I maintain?

Ask:
> "What information do I need while sweeping?"

Common examples:

```text
current platforms
current rooms
number of overlapping events
current resource usage
```


Example:

```java
active++;
```

when something starts, and:

```java
active--;
```

when something ends.

---

## Step 4 — What answer am I looking for?

Different problems ask for different things.

### Maximum overlap

Track:

```java
max = Math.max(max, active);
```

Example:

**Minimum Platforms**

```text
maximum simultaneous trains
=
minimum platforms required
```

---

### Number of groups/resources

If the maximum number of simultaneous intervals is `k`, you often need:

```text
k rooms
k platforms
k resources
```

---

### Detect an overlap

You may simply check whether:

```text
active > 1
```

---

### Track something about the active intervals

Instead of a counter, you may maintain:

* minimum/maximum value
* heap
* set
* map
* other state

The sweep-line idea remains the same.

---

# Example: Minimum Platforms

Given:

```text
arr = [900, 940, 950]
dep = [910, 1120, 1200]
```

Sort both:

```text
arrivals:   900  940  950
departures: 910 1120 1200
```

Maintain:

```text
platform = 0
```

Compare the next arrival and departure.

### 900 vs 910

Arrival happens first:

```text
platform++
```

```text
platform = 1
```

### 940 vs 910

Departure happens first:

```text
platform--
```

```text
platform = 0
```

### 940 vs 1120

Arrival happens first:

```text
platform++
```

```text
platform = 1
```

### 950 vs 1120

Arrival happens first:

```text
platform++
```

```text
platform = 2
```

Therefore:

```text
maximum platforms = 2
```

---

# Two Ways to Implement Sweep Line

## Method 1 — Sort Events Together

Create events containing:

```text
(time, type)
```

For example:

```text
(900, arrival)
(910, departure)
(940, arrival)
```

Sort them by time and process them.

This requires remembering the event type.

---

## Method 2 — Sort Start and End Arrays Separately

This is often cleaner when there are exactly two event types.

```java
Arrays.sort(start);
Arrays.sort(end);
```

Then use two pointers:

```java
i → next start
j → next end
```

Compare:

```java
if(start[i] <= end[j])
```

Start happens first:

```java
active++;
i++;
```

Otherwise:

```java
active--;
j++;
```

This is the approach used in **Minimum Platforms**.

---

# The Critical Tie Rule

This is extremely important.

Suppose:

```text
start = 10
end   = 10
```

What happens when start and end occur at the same time?

It depends on the problem's definition.

For example, in the standard GFG **Minimum Platforms** problem:

```java
if(arr[i] <= dep[j])
```

is used.

So arrival is processed before departure when:

```text
arrival == departure
```

This means an additional platform is required.

But another problem might define:

> "An interval ending at time 10 does not overlap with one starting at time 10."

Then the condition would be different.

### Always ask:

> **Which event should happen first when two events have the same coordinate/time?**

This can completely change the answer.

---

# How to Recognize Sweep Line

Look for these clues:

### 1. Intervals

```text
[start, end]
```

### 2. Time or coordinate

```text
time
position
x-coordinate
```

### 3. Multiple things happening simultaneously

Examples:

```text
trains
meetings
customers
events
buildings
servers
```

### 4. Questions about overlap

Words like:

```text
maximum overlap
simultaneously
at the same time
minimum rooms
minimum platforms
resources required
active intervals
```

are strong signals.

---

# Mental Template

When you see an interval problem, think:

```text
INTERVAL PROBLEM
       ↓
Can I represent things as START / END events?
       ↓
      YES
       ↓
Sort events
       ↓
Sweep from left → right
       ↓
Maintain current state
       ↓
Update answer
```

---

# Sweep Line vs Activity Selection

These two can look very similar but solve different problems.

## Activity Selection

Example:

**N Meetings in One Room**

Goal:

```text
Choose maximum number of non-overlapping intervals
```

Greedy choice:

```text
Choose earliest finishing interval
```

---

## Sweep Line

Example:

**Minimum Platforms**

Goal:

```text
Find maximum number of simultaneously active intervals
```

Strategy:

```text
Process starts and ends chronologically
```

---

# Common Mistakes

## Mistake 1 — Sorting only one side

For example:

```java
Arrays.sort(arr);
```

but not departures.

Both timelines need to be ordered.

---

## Mistake 2 — Losing event type

If you combine everything:

```text
900, 910, 940, 1200
```

you need some way to know:

```text
900 → arrival
910 → departure
```

Otherwise you cannot know whether to increment or decrement.

---

## Mistake 3 — Ignoring equal times

Always determine:

```text
START == END
```

Which event comes first?

This is one of the most common sources of wrong answers.

---

## Mistake 4 — Tracking the wrong thing

Ask what the problem actually wants.

For maximum overlap:

```java
max = Math.max(max, active);
```

For something else, the maintained state may be different.

---

# Complexity

If there are `n` intervals:

Sorting generally costs:

```text
O(n log n)
```

The sweep itself costs:

```text
O(n)
```

Therefore:

```text
Overall = O(n log n)
```

---

# Problems to Practice

Once you understand the basic pattern, practice:

1. **Minimum Platforms**
2. **Meeting Rooms**
3. **Meeting Rooms II**
4. **Maximum Number of Events**
5. **Car Pooling**
6. **My Calendar**
7. **Employee Free Time**

Start with the first few before moving to more complicated versions.

---

# One-Line Mental Model

When you see intervals and simultaneous activity:

> **Don't simulate every moment in time. Turn starts and ends into events, sort them, sweep through them, and maintain what is currently active.**

That is the **Sweep Line pattern**.
Usually this is a counter.
active intervals


Meeting ends   → room freed
```

## Step 2 — What are the events?

(open, close)
Look for things like:

The maximum value of `active` tells us the maximum number of overlapping intervals.

