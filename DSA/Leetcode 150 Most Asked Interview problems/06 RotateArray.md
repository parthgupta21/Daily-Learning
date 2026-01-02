

# LeetCode 189. Rotate Array

## Problem Summary

Given an array `nums` of size `n`, rotate the array to the **right** by `k` steps, in-place if possible.

---

## Key Observations

* Rotation is **index remapping**, not repeated shifting
* `k` can be larger than `n` → must normalize
* In-place solutions require careful rearrangement

---

## Preprocessing Step (Mandatory)

### Normalize `k`

```
k = k % n
```

### Why?

* Rotating by `n` steps returns the array to its original state
* Prevents unnecessary work and index overflow

---

## Approach 1: Rotate One Step at a Time (Brute Force)

### Intuition

* Rotate the array right by **1 step**
* Repeat this process `k` times

### Steps

1. Save the last element
2. Shift all elements one position to the right
3. Place saved element at index `0`
4. Repeat `k` times

### Time Complexity

* **O(n · k)**

### Space Complexity

* **O(1)**

### Code

```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k %= n;

        while (k--) {
            int last = nums[n - 1];
            for (int i = n - 1; i > 0; i--) {
                nums[i] = nums[i - 1];
            }
            nums[0] = last;
        }
    }
};
```

### When to Use

* Only as a brute-force baseline
* Not acceptable for large `k`

---

## Approach 2: Extra Array (Index Mapping)

### Intuition

* Each element at index `i` moves to `(i + k) % n`
* Use a temporary array to place elements directly

### Steps

1. Create a temp array of size `n`
2. For each index `i`, place `nums[i]` at `(i + k) % n`
3. Copy temp array back to `nums`

### Time Complexity

* **O(n)**

### Space Complexity

* **O(n)**

### Code

```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k %= n;
        vector<int> temp(n);

        for (int i = 0; i < n; i++) {
            temp[(i + k) % n] = nums[i];
        }

        nums = temp;
    }
};
```

### When to Use

* Simple and intuitive
* Rejected if interviewer enforces **O(1) space**

---

## Approach 3: Reversal Algorithm (Optimal)

### Core Intuition

Right rotation by `k` means:

* Last `k` elements move to the front
* Order within segments must be preserved

Reversal allows segment rearrangement **in-place**.

---

### Steps

1. Reverse the entire array
2. Reverse the first `k` elements
3. Reverse the remaining `n - k` elements

---

### Example

```
nums = [1,2,3,4,5,6,7], k = 3

Reverse all      → [7,6,5,4,3,2,1]
Reverse first k  → [5,6,7,4,3,2,1]
Reverse rest     → [5,6,7,1,2,3,4]
```

---

### Time Complexity

* **O(n)**

### Space Complexity

* **O(1)**

---

### Code

```cpp
class Solution {
public:
    void reverse(vector<int>& nums, int l, int r) {
        while (l < r) {
            swap(nums[l++], nums[r--]);
        }
    }

    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k %= n;

        reverse(nums, 0, n - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, n - 1);
    }
};
```

---

## Generalized Rotation Idea (Very Important)

### Index Mapping Formula

For **right rotation**:

```
newIndex = (i + k) % n
```

For **left rotation**:

```
newIndex = (i - k + n) % n
```

---

## How to Think About Rotation Problems

Ask these questions:

1. Is rotation **left or right**?
2. Is **extra space allowed**?
3. Can indices be **remapped directly**?
4. Can segments be **reversed or cycled**?

---

## Reusable Rotation Patterns

### Pattern 1: Extra Space Allowed

* Use direct index mapping

### Pattern 2: In-Place Required

* Use **reversal**
* Or **cyclic replacement** (advanced)

---

## Interview One-Liners

**Q: Why do we reverse the entire array first?**
A: To bring the last `k` elements to the front.

**Q: Why does reversal preserve order?**
A: Double reversal restores relative ordering within segments.

**Q: Why is reversal optimal?**
A: Linear time, constant space.
