# Lemonade Change

## Problem

We are selling lemonade for `$5`.

Customers arrive one by one and pay using:

* `$5`
* `$10`
* `$20`

Each lemonade costs `$5`.

Initially, we have no money.

For every customer, we must provide the correct change. We need to determine whether we can serve **all customers in the given order**.

### Goal

Return `true` if every customer can be given the correct change, otherwise return `false`.

---

## Problem Link

[LeetCode — Lemonade Change](https://leetcode.com/problems/lemonade-change/)

## Difficulty

Easy

## Pattern

**Greedy + Simulation**

---

# 1. My Approach

I only need to keep track of the number of `$5` and `$10` bills currently available.

I don't need to track `$20` bills because they can never be useful for giving change:

* `$10` requires `$5` change.
* `$20` requires `$15` change.

For each customer:

### If the customer pays `$5`

No change is required.

```text
five++
```

### If the customer pays `$10`

I need one `$5` bill as change.

```text
five--
ten++
```

If there is no `$5`, I cannot serve the customer.

### If the customer pays `$20`

I need `$15` in change.

There are two possible combinations:

```text
$10 + $5
```
or

```text
$5 + $5 + $5

```

I should prefer `$10 + $5` whenever possible because `$5` bills are more flexible and can be used to provide change for future `$10` or `$20` bills.


---

# 2. Greedy Choice


For a `$20` bill:

> **Use one `$10` and one `$5` before using three `$5` bills.**

This preserves `$5` bills, which are the most useful denomination for future transactions.

---

# 3. Why Is the Greedy Choice Safe?


A `$5` bill is more flexible than a `$10` bill because:

* A `$5` bill can be used as change for a `$10` bill.
* A `$5` bill can also be part of the change for a `$20` bill.


A `$10` bill can only be used as part of `$15` change for a `$20` bill.


Therefore, when handling a `$20` bill, using:

```text
$10 + $5
```


instead of:


```text
$5 + $5 + $5
```

preserves more `$5` bills for future customers.

---

# 4. Algorithm

1. Maintain:

   * `five` = number of `$5` bills
   * `ten` = number of `$10` bills
2. Process each bill in order.
3. If the bill is `$5`:

   * increment `five`.
4. If the bill is `$10`:

   * require one `$5`
   * decrement `five`
   * increment `ten`
   * if no `$5` exists, return `false`.
5. If the bill is `$20`:

   * first try `$10 + $5`
   * otherwise try `$5 + $5 + $5`
   * if neither is possible, return `false`.
6. If every customer is served, return `true`.

---

# 5. Code

```java
class Solution {
    public boolean lemonadeChange(int[] bills) {
        int five = 0;
        int ten = 0;

        for (int bill : bills) {
            if (bill == 5) {
                five++;
            } 
            else if (bill == 10) {
                if (five >= 1) {
                    five--;
                    ten++;
                } else {
                    return false;
                }
            } 
            else if (bill == 20) {
                if (ten >= 1 && five >= 1) {
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
}
```

---

# 6. Example

```text
bills = [5, 5, 5, 10, 20]

```


### Customer 1


```text
Bill = 5


five = 1
ten = 0

```


### Customer 2

```text

Bill = 5


five = 2

ten = 0
```


### Customer 3

```text

Bill = 5

five = 3
ten = 0

```

### Customer 4


```text
Bill = 10


Give $5 change.


five = 2
ten = 1
```


### Customer 5


```text
Bill = 20

Use $10 + $5 as change.


five = 1
ten = 0

```

Everyone is served.


```text
Output = true

```

---


# 7. Complexity


For every customer, we perform a constant amount of work.

Therefore:


```text
Time:  O(n)
Space: O(1)

```

---


# 8. Recognition Clues


Think about **greedy simulation** when:


* Events happen sequentially.
* A decision must be made immediately.

* The decision affects the resources available for future events.
* We don't need to explore multiple possible future states.
* We can identify which resources are more valuable/flexible for future decisions.


### Important clue from this problem


> When several valid choices exist, prefer the choice that preserves the most flexible resources for the future.


---


# 9. Mistakes / Lessons

### Lesson


For a `$20` bill, I should prefer:


```text

$10 + $5

```


over:


```text
$5 + $5 + $5

```


because `$5` bills are more flexible and useful for future customers.


### Important Greedy Insight


Greedy doesn't always mean choosing the numerically smallest or largest value.


Sometimes it means:

> **Choose the option that preserves the most useful resources for future decisions.**


---


# 10. Pattern Summary


This problem demonstrates:


```text
Sequential decisions
        ↓

Maintain current resources
        ↓

Choose the locally safest option
        ↓
Preserve flexibility for the future

        ↓
Continue

```


**Pattern: Greedy + Simulation**

