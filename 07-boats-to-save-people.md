# Boats to Save People

## Problem

You are given an array `people`, where `people[i]` represents the weight of a person.

Each boat can carry **at most two people**, and the total weight of people in a boat cannot exceed `limit`.

Return the **minimum number of boats** required to rescue everyone.

### Example

```text
Input:
people = [3, 2, 2, 1]
limit = 3

Output:
3
```

One valid arrangement:

```text
3
2 + 1
2
```

---

## Pattern

**Greedy — Two Pointers / Pair Heaviest with Lightest**

The key idea is to sort the people by weight and use two pointers:

* `left` → lightest person
* `right` → heaviest person


## First Thought


The heaviest person is the most difficult person to place.


So instead of deciding which person goes first, ask:


> **Can the heaviest person share a boat with the lightest person?**


If yes, pair them.


---


If no, the heaviest person must go alone.

## Observation

After sorting:


```text
[1, 2, 2, 3]

 ↑       ↑
left    right

```

The heaviest person is `3`.

### Case 1: They can be paired


If:


```text
people[left] + people[right] <= limit
```


then the lightest person can safely share the boat with the heaviest person.

Example:


```text

1 + 3 <= 3
```

So:

```text

[1, 3]

```

can use one boat.


Move both pointers.

---


### Case 2: They cannot be paired

Suppose:

```text

[2, 3]
limit = 4

```

Even:


```text

2 + 3 > 4

```


The lightest person cannot fit with the heaviest person.


Therefore, **nobody else can fit with the heaviest person either**, because everyone else is at least as heavy as the lightest person.


So the heaviest person must take a boat alone.


Move only `right`.

---


## Greedy Choice


For every iteration:

> **The heaviest remaining person gets a boat.**


Then:


* If they can fit with the lightest remaining person → pair them.
* Otherwise → the heaviest person goes alone.


Either way, the heaviest person is removed from consideration.


---

## Why Is This Safe?

Consider the heaviest person.

### If the lightest person cannot fit


```text
lightest + heaviest > limit

```

Then no other person can fit either.

Therefore, the heaviest person **must** use a boat alone.

### If the lightest person can fit

```text
lightest + heaviest <= limit
```


Pairing them is safe because the lightest person is the easiest person to pair with the heaviest person.


There is no benefit to leaving the lightest person for a later boat while trying to pair the heaviest person with someone heavier.


Thus, pairing the extremes is a safe greedy choice.


---


## Algorithm

1. Sort `people` in ascending order.
2. Set:

   ```text
   left = 0
---
   right = people.length - 1
   ```
3. While `left <= right`:

   * Consider the lightest and heaviest remaining people.
   * If their combined weight is within the limit:

     * increment `left`.
   * Regardless of whether they were paired:

     * increment the boat count.
     * decrement `right`.

4. Return the boat count.



## Code


```java
class Solution {
    public int numRescueBoats(int[] people, int limit) {


        int boatCount = 0;


        Arrays.sort(people);


        int left = 0;
        int right = people.length - 1;


        while(left <= right) {


            int sum = people[left] + people[right];


            if(sum <= limit) {
                left++;

            }


            boatCount++;

```text
            right--;

        }


        return boatCount;
    }

}
```

---

## Example Walkthrough


```text

people = [3, 2, 2, 1]

limit = 3
[1, 2, 2, 3]
 ↑       ↑
 L       R

```


After sorting:

```


### Step 1


```text
1 + 3 = 4 > 3

```


Cannot pair.


```text
3

```


One boat.


```text
L = 0

R = 2

boats = 1

```


---


### Step 2


```text
1 + 2 = 3 <= 3

```


Pair them:


```text
1 + 2

```


Second boat.


```text
L = 1

R = 1
boats = 2
```

---


### Step 3


Remaining person:


```text
2

```

They need their own boat.

```text
boats = 3
```

Answer:

```text
3
```

---

## Important Detail

Notice that `right--` happens **every iteration**, regardless of whether a pair was formed.

Why?

Because the heaviest person always gets a boat during that iteration.

The only question is whether that boat contains:

```text
heaviest
```

or:

```text
heaviest + lightest
```

---

## Complexity

### Sorting

```text
O(n log n)
```

### Two-pointer traversal


```text

O(n)
```


### Overall


```text

excluding the space used internally by the sorting implementation.
O(n log n)
```


### Auxiliary Space

```text
O(1)
```

---


## Recognition Clues


Think of this pattern when:


* You need to minimize the number of groups/boats.
* Each group has a maximum capacity.
* A group can contain at most two elements.

* You need to pair elements under a constraint.
* There is a natural **smallest vs largest** relationship.

* Sorting makes the constraint easier to reason about.
* The largest element is the most restrictive element.


Common mental trigger:

> **"Sort → take the hardest/largest element → try pairing it with the easiest/smallest element."**

---


## Mistakes / Lessons

### 1. Don't greedily pair arbitrary people

The goal isn't to find any valid pair.

The goal is to minimize the total number of boats.

The heaviest person is the most restrictive, so they should drive the decision.

### 2. Don't move `left` when pairing fails

If:

```text
people[left] + people[right] > limit
```

the lightest person wasn't used.

Only the heaviest person must leave:

```text
right--
```

### 3. `right` always moves

Every iteration consumes the heaviest remaining person because that person definitely occupies one boat.

---

## Pattern Summary

This problem introduces a useful variation of greedy:

```text
Sort
  ↓
Identify the most restrictive element
  ↓
Try pairing it with the least restrictive element
  ↓
If possible → use one group for both
If impossible → restrictive element goes alone
```

The core insight is:

> **When the heaviest person cannot pair with the lightest person, they cannot pair with anyone.**

That makes the greedy decision provably safe.

