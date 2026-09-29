# Assign Cookies

## Problem

We are given two arrays:

* `g[i]` — the greed factor of child `i`
* `s[j]` — the size of cookie `j`

A child can be satisfied if:

```text
cookie size >= child's greed factor
```

Each child can receive at most one cookie, and each cookie can be assigned to at most one child.

### Goal

Maximize the number of satisfied children.

---

## Problem Link

[LeetCode — Assign Cookies](https://leetcode.com/problems/assign-cookies/)

## Difficulty

Easy

## Pattern

**Greedy + Sorting + Two Pointers**

---

# 1. My First Approach

My initial approach was to sort both arrays and then use two loops.

For every child, I would search through the cookies to find a cookie large enough to satisfy that child. Once a cookie was used, I would mark it as `-1`.

### Code

```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        int count = 0;

        Arrays.sort(g);
        Arrays.sort(s);

        for (int i = 0; i < g.length; i++) {
            for (int j = 0; j < s.length; j++) {
                if (s[j] != -1 && g[i] <= s[j]) {
                    count++;
                    s[j] = -1;
                    break;
                }
            }
        }

        return count;
    }
}
```

### Complexity

Sorting:

```text
O(n log n + m log m)
```

Nested search:

```text
O(n × m)
```

Therefore:

```text
Time: O(nm + n log n + m log m)
In the worst case, when `n` and `m` are of similar size:

```text

O(n²)

```


---


# 2. Observation


After sorting both arrays, we can process the least greedy child first.


If the smallest available cookie is too small for the current child, it will also be too small for every remaining child because the remaining children are at least as greedy.


Therefore, that cookie can safely be discarded.


If the smallest available cookie can satisfy the current child, we should use it rather than a larger cookie.


This preserves the larger cookies for children with larger greed factors.


---


# 3. Greedy Choice

> **Give the smallest cookie that can satisfy the least greedy remaining child.**


This prevents us from wasting a large cookie on a child who could have been satisfied with a smaller one.

---


# 4. Why Is the Greedy Choice Safe?

Suppose the current child has the smallest remaining greed factor.

If the smallest available cookie cannot satisfy this child, then it cannot satisfy any remaining child, so we can discard it.


If it can satisfy the child, using this smallest sufficient cookie is safe because any larger cookie could potentially be needed by a more demanding child later.

Therefore, matching the least greedy child with the smallest sufficient cookie does not reduce the maximum number of children that can eventually be satisfied.

---

# 5. Optimal Approach

Sort both arrays.

Use two pointers:

* `i` → current child
* `j` → current cookie

At every step:

### Case 1

```text
s[j] >= g[i]
```

The cookie can satisfy the child.

So:

* count the child as satisfied
* move to the next child
* move to the next cookie

### Case 2

```text
s[j] < g[i]
```

The cookie is too small.

Since the children are sorted, this cookie cannot satisfy the current child or any later child.

So:

* discard the cookie
* move only the cookie pointer

---

# 6. Algorithm

1. Sort the greed array.
2. Sort the cookie array.
3. Initialize two pointers:

   * `i = 0`
   * `j = 0`
4. While both pointers are within their arrays:

   * If `s[j] >= g[i]`, assign the cookie:

     * increment `count`
     * increment `i`
     * increment `j`
   * Otherwise, the cookie is too small:

     * increment `j`
5. Return `count`.

---

# 7. Code

```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        int count = 0;

        Arrays.sort(g);
        Arrays.sort(s);

        int i = 0;
        int j = 0;

        while (i < g.length && j < s.length) {
            if (s[j] >= g[i]) {
                count++;
                i++;
                j++;
            } else {
                j++;
            }
        }

        return count;
    }
}
```

---

# 8. Complexity

Let:

* `n = g.length`
* `m = s.length`

Sorting:

```text
O(n log n + m log m)
```

Two-pointer traversal:

```text
O(n + m)
```

Therefore:

```text
Time: O(n log n + m log m)
Space: O(1) auxiliary
```


---


# 9. Example

```text
g = [1, 2, 3]
s = [1, 1]
```


After sorting:


```text

g = [1, 2, 3]

s = [1, 1]

```


### Step 1

```text
g[i] = 1
s[j] = 1
```


`1 >= 1`

Child is satisfied.

```text

count = 1
i = 1

j = 1
```

### Step 2


```text
g[i] = 2

s[j] = 1
```

`1 < 2`

Cookie is too small.


Move only `j`.

```text
j = 2
```

No more cookies.

Therefore:

```text
answer = 1
```

---

# 10. Recognition Clues

Think of **sorting + greedy + two pointers** when you see:

* We need to maximize the number of successful matches.
* There are two groups: demands and resources.
* A resource can satisfy a demand if it meets some threshold.
* Each resource can be used only once.
* We want to avoid wasting large resources on small requirements.
* Sorting can establish a useful ordering.

### General Pattern

```text
Sort both sides
        ↓
Process smallest requirement
        ↓
Use smallest sufficient resource
        ↓
Move pointers
```

---

# 11. Mistakes / Lessons

### Mistake

My first solution searched through the entire cookie array for every child and marked used cookies as `-1`.

### Lesson

Once both arrays are sorted, I don't need to repeatedly search for a suitable cookie.

The ordering itself lets me eliminate impossible cookies permanently and process everything using two pointers.

### Important Greedy Insight

> **Use the smallest sufficient resource for the smallest remaining requirement.**

This prevents wasting resources that may be required by more demanding elements later.

---

# 12. Pattern Summary

**Demand + Resource Matching**

```text
Sort demands
Sort resources

        ↓

smallest demand


        ↓


smallest resource that can satisfy it

        ↓

match and consume both


OR


resource too small
        ↓
discard resource

```


This is a reusable greedy pattern, not just a solution for Assign Cookies.

