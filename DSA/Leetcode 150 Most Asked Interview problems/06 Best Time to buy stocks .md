# Best Time to Buy and Sell Stock

## Problem Summary

Given an array `prices` where `prices[i]` is the stock price on day `i`, choose **one day to buy** and a **future day to sell** to maximize profit.
If no profit is possible, return `0`.

---

## Core Idea (Most Important)

Instead of trying all buy–sell pairs, think like this:

> If I sell **today**, what is the **best price I could have bought at before today**?

This reduces the problem to tracking:

1. The **minimum price so far**
2. The **maximum profit so far**

You never need to look ahead.

---

## Key Observation

Profit on day `i` is:

```
prices[i] - minPriceSoFar
```

So at each day:

* Update the minimum buying price
* Update the best profit if selling today is better

---

## Invariant Maintained

At every index `i`:

* `minPrice` = minimum of `prices[0..i]`
* `maxProfit` = maximum profit achievable in `prices[0..i]`

This guarantees:

* Buy always happens before sell
* Only one transaction is considered

---

## Optimal Approach: One Pass (Greedy / DP Intuition)

### Steps

1. Initialize:

   * `minPrice` to a very large value
   * `maxProfit` to `0`
2. Traverse the array:

   * Update `minPrice`
   * Compute profit if selling today
   * Update `maxProfit`
3. Return `maxProfit`

---

## Time and Space Complexity

* **Time**: O(n)
* **Space**: O(1)

---

## Code (Clean and Interview-Ready)

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int minPrice = INT_MAX;
        int maxProfit = 0;

        for (int price : prices) {
            minPrice = min(minPrice, price);
            maxProfit = max(maxProfit, price - minPrice);
        }

        return maxProfit;
    }
};
```

---

## Dry Run Example

### Input

```
prices = [7,1,5,3,6,4]
```

### Trace

```
minPrice = 7, maxProfit = 0
minPrice = 1, maxProfit = 0
minPrice = 1, maxProfit = 4
minPrice = 1, maxProfit = 4
minPrice = 1, maxProfit = 5
minPrice = 1, maxProfit = 5
```

### Output

```
5
```

---

## Why This Works (Explain in Interviews)

* The best profit on any day depends only on the **lowest price before it**
* Tracking that minimum is enough
* No need for nested loops or DP tables

---

## Interview One-Liners

**Q: Why update `minPrice` first?**
A: To ensure the buy day always comes before the sell day.

**Q: Is this DP or greedy?**
A: Greedy with DP intuition; the DP table collapses to constant space.

**Q: What happens if prices are decreasing?**
A: `maxProfit` remains `0`.

