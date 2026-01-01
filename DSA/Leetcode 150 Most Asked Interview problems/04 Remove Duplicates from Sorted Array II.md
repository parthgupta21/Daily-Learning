# Remove Duplicates from Sorted Array II

## Problem Summary

Given a **sorted** array `nums`, remove duplicates **in-place** such that **each unique element appears at most twice**.
Preserve relative order.
Return `k`, the length of the valid prefix.

---

## Key Observations

* Array is **sorted** → duplicates are **contiguous**
* Order **must be preserved**
* Extra space **not allowed**
* Constraint is not “remove duplicates”, but **limit frequency to 2**

---

## Core Idea (Very Important)

Instead of **counting duplicates explicitly**, think this way:

> While building the result array, **never allow the same number to occupy more than two positions**.

This leads to a critical insight:

> A number is valid **if it is different from the element that is two positions behind in the result array**.

Why?

* If the last two written elements are the same as the current number, adding it would create **three occurrences**, which violates the constraint.

---

## Mental Model (Interview-Friendly)

* You are building a **valid prefix** of the array.
* `k` = length of the valid prefix so far.
* For every new element:

  * If it would become the **third copy**, reject it.
  * Otherwise, accept it.

---

## Optimal Approach: Two Pointers (Frequency via Index)

### Intuition

* First two elements are always allowed.
* From the third position onward:

  * Only allow the element if it is **not equal to `nums[k - 2]`**

---

### Steps

1. Initialize `k = 0` (size of valid result).
2. Iterate through the array using index `i`.
3. If `k < 2`, always write `nums[i]`.
4. Else:

   * Write `nums[i]` **only if** `nums[i] != nums[k - 2]`
5. Increment `k` whenever you write.
6. Return `k`.

---

## Code (C++)

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int k = 0;

        for (int i = 0; i < nums.size(); i++) {
            if (k < 2 || nums[i] != nums[k - 2]) {
                nums[k] = nums[i];
                k++;
            }
        }
        return k;
    }
};
```

---

## Invariant Maintained

* `nums[0 .. k-1]` is always valid:

  * Sorted
  * No element appears more than twice

---

## Why This Works (Explain in Interviews)

* Sorted order guarantees duplicates are adjacent.
* Checking `k - 2` ensures frequency never exceeds 2.
* No explicit counter needed.
* Single pass, in-place.

---

## Interviewer Questions (One-Liners)

**Q1. Why compare with `k - 2`?**
A. To ensure an element does not appear more than twice.

**Q2. Why are the first two elements always allowed?**
A. Because at most two duplicates are permitted.

**Q3. What invariant does the algorithm maintain?**
A. The prefix `nums[0..k-1]` is always valid.

**Q4. Why does this require a sorted array?**
A. Only sorted arrays guarantee duplicates are adjacent.

**Q5. Can this be generalized to “at most K duplicates”?**
A. Yes, compare with `nums[k - K]`.

---

## Generalization Pattern (Very Important)

* Allow **at most K duplicates**
  → Condition becomes:

```
nums[i] != nums[k - K]
```

