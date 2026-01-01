# LeetCode 26. Remove Duplicates from Sorted Array

## Problem Summary

Given a **sorted** array `nums`, remove duplicates **in-place** such that each unique element appears once.
Return `k`, the number of unique elements.
The first `k` elements of `nums` must be the unique elements **in sorted order**.

---

## Key Observations

* Array is **sorted** → duplicates are **adjacent**
* Order **must be preserved**
* Extra space **not allowed**

---

## Approach 1: Use Extra Data Structure (Brute Force)

### Intuition

* Use a data structure that automatically removes duplicates.
* Since array is sorted, the result will also be sorted.
* Write the unique elements back into the array.

### Steps

1. Insert all elements into an ordered set.
2. Set `k = size of set`.
3. Iterate over the set and overwrite `nums[0..k-1]`.
4. Return `k`.

### Time Complexity

* **O(n log n)**

### Space Complexity

* **O(n)**

### Code

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        set<int> s(nums.begin(), nums.end());
        int i = 0;
        for (int x : s) {
            nums[i++] = x;
        }
        return s.size();
    }
};
```

### Verdict

* Correct
* Sub-optimal
* Violates in-place constraint

---

## Approach 2: Two Pointers (Optimal)

### Intuition (Core Idea)

* Since duplicates are adjacent, we only need to compare **neighbors**.
* Maintain:

  * One pointer for **last unique element**
  * One pointer to **scan forward**
* Grow the unique prefix gradually.

### Mental Model

* Think of `i` as the **end of the unique array**
* Think of `j` as the **scanner**
* Whenever `nums[j]` is different, it belongs to the unique array

---

### Steps

1. If array is empty, return `0`.
2. Initialize:

   * `i = 0` (last unique element)
   * `j = 1` (scanner)
3. While `j < n`:

   * If `nums[j] != nums[i]`:

     * Increment `i`
     * Set `nums[i] = nums[j]`
   * Increment `j`
4. Return `i + 1`

### Time Complexity

* **O(n)**

### Space Complexity

* **O(1)**

---

### Code

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.size() == 0) return 0;

        int i = 0;
        int j = 0;
        while(j < nums.size()){
            if (i < 1 || nums[i - 1] != nums[j]) {
                nums[i] = nums[j];
                i++;
            }
        }
        return i;
    }
};
```

---

## Invariant Maintained

* At any point:

  * `nums[0..i]` contains **unique elements**
  * Elements are in **sorted order**
  * `j` scans unexplored elements

---

## Interviewer Questions (One-Liners)

**Q1. Why does this work only for sorted arrays?**
A. Because duplicates are guaranteed to be adjacent.

**Q2. Why do we return `i + 1`?**
A. `i` is the index of the last unique element.

**Q3. What happens if the array is empty?**
A. Return `0`.

**Q4. Why is no extra space needed?**
A. We overwrite duplicates within the same array.

**Q5. What is the invariant of the algorithm?**
A. The prefix `nums[0..i]` always contains unique elements.

**Q6. Can this be done in less than O(n)?**
A. No, every element must be inspected at least once.

