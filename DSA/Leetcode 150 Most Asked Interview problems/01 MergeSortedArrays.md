# LeetCode 88. Merge Sorted Array

## Problem Summary

Merge two sorted arrays `nums1` and `nums2` into `nums1` in non-decreasing order.
`nums1` has extra space at the end to accommodate elements from `nums2`.

---

## Approach 1: Insert Using Upper Bound (Sub-optimal)

### Steps

1. Iterate through each element of `nums2`.
2. Find the **upper bound** position in the valid portion of `nums1` `[0 .. m-1]`.
3. Shift elements of `nums1` one position to the right.
4. Insert the element at the computed position.
5. Increment `m`.

### Time Complexity

* Worst Case: **O(n · m)**

### Space Complexity

* **O(1)**

### Code

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        for (int i = 0; i < n; i++) {
            int val = nums2[i];

            int left = 0, right = m;
            while (left < right) {
                int mid = left + (right - left) / 2;
                if (nums1[mid] <= val)
                    left = mid + 1;
                else
                    right = mid;
            }

            for (int j = m; j > left; j--) {
                nums1[j] = nums1[j - 1];
            }

            nums1[left] = val;
            m++;
        }
    }
};
```

---

## Approach 2: Three Pointer Backward Merge (Optimal)

### Steps

1. Initialize three pointers:

   * `i = m - 1` (last valid element in `nums1`)
   * `j = n - 1` (last element in `nums2`)
   * `k = m + n - 1` (last index of `nums1`)
2. Compare `nums1[i]` and `nums2[j]`.
3. Place the larger element at `nums1[k]`.
4. Move corresponding pointer backward.
5. Repeat until one array is exhausted.
6. Copy remaining elements of `nums2` if any.

### Time Complexity

* **O(m + n)**

### Space Complexity

* **O(1)**

### Code

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m - 1, j = n - 1, k = m + n - 1;

        while (i >= 0 && j >= 0) {
            if (nums1[i] > nums2[j]) {
                nums1[k] = nums1[i];
                i--;
            } else {
                nums1[k] = nums2[j];
                j--;
            }
            k--;
        }

        while (j >= 0) {
            nums1[k] = nums2[j];
            j--;
            k--;
        }
    }
};
```

---

## Interviewer Follow-up Questions (With One-Liners)

**Q1. Why do we merge from the back?**
A. To avoid overwriting unprocessed elements in `nums1`.

**Q2. Why is the time complexity O(m + n)?**
A. Each element is processed exactly once.

**Q3. Why can we skip copying remaining elements of nums1?**
A. They are already in correct sorted positions.

**Q4. Why must remaining elements of nums2 be copied?**
A. They have no guaranteed placement unless explicitly written.

**Q5. Is sorting the combined array acceptable?**
A. Correct but sub-optimal due to O((m+n) log(m+n)) time.

**Q6. What invariant is maintained during merging?**
A. Elements from index `k+1` onward are always correctly placed.

**Q7. What happens if `m = 0`?**
A. All elements of `nums2` are copied into `nums1`.

