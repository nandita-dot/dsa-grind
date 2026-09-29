# Best Time to Buy and Sell Stock II

## Problem

You are given an array `prices` where `prices[i]` represents the price of a stock on day `i`.

You may buy and sell the stock multiple times, but you must sell before buying again.

Return the maximum profit you can achieve.

---

## Link

LeetCode: Best Time to Buy and Sell Stock II

## Difficulty

Easy

## Pattern

**Greedy — Accumulate Every Positive Gain**

---

## First Thought

Initially, the idea was:

> Find the lowest price and sell when the price becomes greater.

This sounds reasonable, but we don't actually need to find one global buying point.

The important observation is that we can collect **every profitable upward movement**.

---

## Observation

If:

```text
prices[i] > prices[i - 1]
```

then there is a profit available between those two days:

```text
prices[i] - prices[i - 1]
```

Since multiple transactions are allowed, we can add every positive adjacent difference.

For example:

```text
[1, 2, 3, 4, 5]
```

Instead of thinking:

```text
Buy at 1 → Sell at 5
Profit = 4
```

we can think:

```text
1 → 2 = +1
2 → 3 = +1
3 → 4 = +1
4 → 5 = +1

Total = 4
```

Both produce the same maximum profit.

---

## Greedy Choice

Whenever today's price is greater than yesterday's price:

```java
prices[i] > prices[i - 1]
```

take that gain:

```java
profit += prices[i] - prices[i - 1];
```

If the price decreases, there is nothing to gain from that movement, so ignore it.

---

## Why Is This Safe?

Every upward movement contributes positively to the total possible profit.

For a rising sequence:

```text
1 → 2 → 3 → 4
```

the total gain is:

```text
(2 - 1) + (3 - 2) + (4 - 3)
= 3
```

which is exactly the same as:

```text
4 - 1 = 3
```

Therefore, collecting every positive adjacent difference captures the entire available upward profit.

---

## Algorithm

1. Initialize `profit = 0`.
2. Start from index `1`.
3. If `prices[i] > prices[i - 1]`, add the difference to `profit`.
4. Continue through the array.
5. Return `profit`.

---

## Code

```java
class Solution {
    public int maxProfit(int[] prices) {
        int profit = 0;

        for(int i = 1; i < prices.length; i++) {
            if(prices[i] > prices[i - 1]) {
                profit += prices[i] - prices[i - 1];
            }
        }

        return profit;
    }
}
```

---

## Example

```text
prices = [7, 1, 5, 3, 6, 4]
```

Process:

```text
7 → 1     decrease → +0
1 → 5     increase → +4
5 → 3     decrease → +0
3 → 6     increase → +3
6 → 4     decrease → +0
```

Total:

```text
4 + 3 = 7
```

Answer:

```text
7
```

---

## Complexity

* Time: `O(n)`
* Auxiliary Space: `O(1)`

---

## Recognition Clues

Think of this greedy approach when:

* Multiple transactions are allowed.
* You can buy/sell repeatedly.
* There is no transaction fee or cooldown.
* Every positive movement can independently contribute to the answer.
* You see a sequence where local gains can be accumulated.

---

## Mistakes / Lessons

### Mistake 1: Thinking only about the global minimum

Finding the lowest price and selling later is not the right general mental model.

Instead ask:

> Can I accumulate every profitable upward movement?

### Lesson

For this problem:

```text
positive adjacent difference = profit worth taking
```

---

## Pattern Summary

**Greedy rule:**

> Take every positive adjacent gain.

Implementation:

```java
if(prices[i] > prices[i - 1]) {
    profit += prices[i] - prices[i - 1];
}
```
