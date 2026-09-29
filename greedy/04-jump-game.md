maxReach = Math.max(maxReach, i + nums[i]);
Core failure condition:
    return false;

Core success condition:

    return true;
```
```java
if(maxReach >= nums.length - 1)
```

```java
if(i > maxReach)
```

**Greedy rule:**

```java

> At every reachable index, keep extending the farthest reachable position.

Core update:


## Pattern Summary

Otherwise, we are pretending we can stand on an index that is actually unreachable.

---
i <= maxReach
```

```java
Before using `nums[i]`, verify:
### Mistake 2: Processing unreachable indices

We cannot blindly process every index.

```

is the correct condition.

```java
maxReach >= nums.length - 1
# Jump Game

## Problem

You are given an integer array `nums`.

You start at index `0`.

Therefore:


`nums[i]` represents the **maximum jump length** from index `i`.

Determine whether you can reach the last index.


```java
nums.length - 1
```
---

## Link

LeetCode: Jump Game
The last valid index is:

## Difficulty

Medium

## Pattern

**Greedy — Farthest Reach / Reachability**

---

## Understanding the Problem

### Mistake 1: Checking against `nums.length`

The value at each index tells us the maximum number of positions we can move forward.

## Mistakes / Lessons

For:

```text
nums = [2, 3, 1, 1, 4]
```

---

At index `0`:

```text
nums[0] = 2
```

So we can jump to:

```text
index 1
index 2
```

We do **not** have to jump exactly `nums[i]` positions.

It means:
> **How far can I reach from everything I've seen so far?**


> We can jump anywhere from `1` to `nums[i]` positions forward.

Key question:

The goal is simply to determine whether the last index is reachable.

---

## First Thought

At every index there can be multiple possible jumps.

Instead of trying every possible path, ask:

> **What is the farthest index I can reach with everything I have seen so far?**

* The problem asks whether the end can be reached rather than which exact path is required.

This removes the need to track every possible path.

---

## Greedy Observation

Maintain:

```text
maxReach
```

where:

> `maxReach` = the farthest index that is currently reachable.

At index `i`, if `i` is reachable, then we can potentially extend our reach to:
* There are many possible paths, but you only care about the farthest reachable boundary.
* Trying every jump would create unnecessary branching.


## Example 1


Track `maxReach`:

```text
i = 1
maxReach = max(2, 1 + 3) = 4


* You need to determine whether a destination is reachable.
* Each position gives you a maximum reach.
The last index is:

```text
4
```

Since:

```text
Think of this greedy pattern when:
maxReach >= 4
```

answer = `true`.

One possible path is:

```text
0 → 1 → 4
```

## Complexity

* Time: `O(n)`
* Auxiliary Space: `O(1)`

---

## Recognition Clues

---

## Example 2

```text
nums = [3, 2, 1, 0, 4]

```

Track the reachable boundary:

```text
---
i = 0
maxReach = 3

i = 1
maxReach = 3


i = 2
maxReach = 3

```
i = 3
maxReach = 3
```

```text
false

At this point:

```text
maxReach = 3
Answer:
```

but the last index is:

Index `4` cannot be reached.

```text
4
```

```

i = 0
maxReach = max(0, 0 + 2) = 2
```text
nums = [2, 3, 1, 1, 4]
```
        }
```

---

        return false;
    }
}
```text
i + nums[i]

            if(maxReach >= nums.length - 1) {
                return true;
            }
            maxReach = Math.max(maxReach, i + nums[i]);

```

Therefore:

```java
                return false;
            }
maxReach = Math.max(maxReach, i + nums[i]);
```


            if(i > maxReach) {
---

## The Critical Insight

We only process an index if it is reachable.

        for(int i = 0; i < nums.length; i++) {

Suppose:

class Solution {
    public boolean canJump(int[] nums) {
        int maxReach = 0;
```text
## Code

```java
i > maxReach
```

Then we cannot even reach index `i`.

Therefore, there is no point processing it.

The answer is immediately:

```text

---

false
```
return `true`.
6. If the loop finishes without reaching the last index, return `false`.

This is what happens in:

```text
[3, 2, 1, 0, 4]
```

We can reach index `3`, but:

```text
```text
maxReach >= nums.length - 1
```

nums[3] = 0
```

So our farthest reach remains `3`.

Index `4` is beyond our reach.

Therefore the answer is `false`.

---

## Greedy Choice

At every reachable index:


5. If:

```text
extend maxReach as far as possible
```
```

We don't care which exact jump we take.

We only care about the maximum reachable boundary.

---
```java
maxReach = Math.max(maxReach, i + nums[i]);

## Why Is This Safe?

If an index `i` is reachable, then any index between the current position and `i + nums[i]` is also reachable.

Therefore, we don't need to remember every possible path.

We only need the farthest boundary reached so far.

If the last index falls inside that boundary:

```text
maxReach >= lastIndex
```

then index `i` is unreachable, so return `false`.
4. Otherwise, extend the reachable range:

we know the end is reachable.

---


## Algorithm

1. Initialize:

i > maxReach
```
```java
maxReach = 0;
```

