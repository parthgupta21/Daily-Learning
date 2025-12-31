# LeetCode 27. Remove Element

## Problem Summary

Remove all occurrences of `val` from array `nums` **in-place** and return the count `k` of remaining elements.
Order of elements **does not matter**.

---

## Approach 1: Overwrite Forward (Stable Order)

### Steps

1. Initialize pointer `p = 0`.
2. Traverse the array from left to right.
3. If `nums[i] != val`, write `nums[i]` to `nums[p]`.
4. Increment `p`.
5. Return `p`.

### Time Complexity

* **O(n)**

### Space Complexity

* **O(1)**

### Code

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int p = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != val) {
                nums[p] = nums[i];
                p++;
            }
        }
        return p;
    }
};
```

---

## Approach 2: Swap With End (Order Not Preserved)

### Steps

1. Initialize:

   * `i = 0`
   * `n = nums.size()`
2. While `i < n`:

   * If `nums[i] == val`:

     * Replace `nums[i]` with `nums[n - 1]`
     * Decrement `n`
   * Else:

     * Increment `i`
3. Return `n`.

### Time Complexity

* **O(n)**

### Space Complexity

* **O(1)**

### Code

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int i = 0;
        int n = nums.size();

        while (i < n) {
            if (nums[i] == val) {
                nums[i] = nums[n - 1];
                n--;
            } else {
                i++;
            }
        }
        return n;
    }
};
```

---

## Interviewer Follow-Up Questions (One-Liners)

**Q1. Why does the overwrite approach work?**
A. It compacts all valid elements into the front of the array.

**Q2. What is returned by the function?**
A. The count of elements not equal to `val`.

**Q3. Why is order allowed to change?**
A. The problem explicitly states order does not matter.

**Q4. Why don’t we increment `i` after swapping with the end?**
A. The swapped element is unprocessed and must be checked.

**Q5. What happens if `nums[n-1]` is also `val`?**
A. `n` shrinks and the loop continues safely.

**Q6. Which approach minimizes write operations?**
A. The swap-with-end approach.

**Q7. Which approach preserves order?**
A. The overwrite-forward approach.

**Q8. What invariant is maintained in swap-with-end?**
A. All indices `[0, n)` are valid and under consideration.

