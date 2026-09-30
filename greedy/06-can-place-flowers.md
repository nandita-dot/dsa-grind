# Can Place Flowers

## Problem

You are given a flowerbed represented by an integer array.

* `1` means a flower is already planted.
* `0` means the plot is empty.

You cannot plant flowers in adjacent plots.

Given an integer `n`, determine whether it is possible to plant `n` new flowers without violating the rule.

---

## Link

LeetCode: Can Place Flowers

## Difficulty

Easy

## Pattern

**Greedy — Take the Earliest Safe Position**

---

## First Thought

When encountering an empty position (`0`), check its neighboring positions.

A flower can be planted if:

* The current position is empty.
* The left position is empty or does not exist.
* The right position is empty or does not exist.

If all conditions are satisfied, plant immediately.

---

## Observation

For every empty position `i`, check:

```java
leftEmpty  = (i == 0 || flowerbed[i - 1] == 0);
rightEmpty = (i == flowerbed.length - 1 || flowerbed[i + 1] == 0);
```

If both are empty:

```text
leftEmpty = true
rightEmpty = true
```

then the current position is safe.

---

## Greedy Choice
> **Whenever the current position is safe, plant a flower immediately.**


Why?


---

## Boundary Conditions

The first position has no left neighbor.

Because planting at the earliest available safe position cannot reduce the number of flowers that can ultimately be planted.

Therefore:

```java

i == 0
```

is treated as having an empty left side.
We are scanning from left to right, and every planted flower prevents its immediate neighbors from being used. Taking a safe position immediately is therefore sufficient and avoids unnecessary future choices.

Similarly, the last position has no right neighbor:

```java
i == flowerbed.length - 1
```

is treated as having an empty right side.

This allows the same condition to handle both boundaries:

```java
boolean leftEmpty =
    (i == 0 || flowerbed[i - 1] == 0);

boolean rightEmpty =
    (i == flowerbed.length - 1 ||
     flowerbed[i + 1] == 0);
```

---

## Algorithm

1. If `n == 0`, immediately return `true`.
2. Iterate through the flowerbed from left to right.
3. If the current position is already occupied, skip it.
4. For an empty position:

   * Check whether the left side is empty.
   * Check whether the right side is empty.
5. If both sides are empty:

   * Plant a flower.
   * Decrease `n`.
6. If `n` becomes `0`, return `true`.
7. If the loop finishes and `n > 0`, return `false`.

---

## Code

```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        if(n == 0) return true;

        for(int i = 0; i < flowerbed.length; i++){
            if(flowerbed[i] == 0){

                boolean leftEmpty =
                    (i == 0 || flowerbed[i - 1] == 0);

                boolean rightEmpty =
                    (i == flowerbed.length - 1 ||
                     flowerbed[i + 1] == 0);

                if(leftEmpty == true && rightEmpty == true){
                    flowerbed[i] = 1;
                    n--;
                }

                if(n == 0) return true;
            }
        }

        return false;
    }
}
```

---

## Example 1

```text
flowerbed = [1,0,0,0,1]
n = 1
```

Start:

```text
[1,0,0,0,1]
```

Index `1`:

```text
left = 1
right = 0
```

Cannot plant.

Index `2`:

```text
left = 0
right = 0
```

Safe.

Plant:

```text
[1,0,1,0,1]
```

Now:

```text
n = 0
```

Return:

```text
true
```


---


## Example 2


```text

flowerbed = [1,0,0,0,1]

n = 2

```text
[1,0,1,0,1]
```


Only index `2` can safely receive a flower.

After planting:
```


One flower is still required.


Therefore:

```text
false

```


---


## Example 3

```text
flowerbed = [0,0,0,0,0]

n = 2
```

Index `0` is safe:


```text

[1,0,0,0,0]
```


Index `1` is no longer safe.


Index `2` is safe:

```text

[1,0,1,0,0]
```

Therefore two flowers can be planted.

Answer:


```text
true

```

---


## Complexity


Let `n` be the length of the flowerbed.

* Time: `O(n)`
* Auxiliary Space: `O(1)`

The input array is modified in place.

---

## Recognition Clues

Think of this greedy pattern when:

* You process positions from left to right.
* A position is either valid or invalid based on its local neighbors.
* Choosing a valid position changes the availability of nearby positions.
* You want to maximize the number of valid placements.

Ask:

> **If this position is safe right now, is there any reason to save it for later?**

If the answer is no, take the position immediately.

---

## Mistakes / Lessons

### Mistake 1: Using `||` instead of `&&`

Initially, the condition was interpreted as:

```java
if(leftEmpty || rightEmpty)
```

But a flower can only be planted when **both** sides are empty.

Correct:

```java
if(leftEmpty && rightEmpty)
```

### Lesson

Translate the English condition carefully:

> "The left **and** right sides must be empty."

means:

```java
&&
```

not:

```java
||
```

### Mistake 2: Forgetting boundaries

The first and last positions do not have two neighbors.

Treat the missing neighbor as empty:

```java
i == 0
```

and:

```java
i == flowerbed.length - 1
```

---

## Pattern Summary

**Greedy rule:**

> Scan from left to right and immediately take every position that is locally safe.

Core condition:

```java
if(leftEmpty && rightEmpty)
```

Core action:

```java
flowerbed[i] = 1;
n--;
```

Core recognition question:

> **Is the current position safe, and can taking it immediately ever hurt the optimal answer?**

