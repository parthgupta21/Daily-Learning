

# Majority Element

## Problem Summary

Given an array `nums` of size `n`, return the element that appears **more than ⌊n / 2⌋ times**.
The majority element is **guaranteed to exist**.

---

## Key Guarantee

* One element appears **strictly more than half the time**
* This guarantee enables an **O(1) space** solution

---

## Approach 1: Frequency Map (Brute Force)

### Idea

* Count frequency of each element.
* Return the element with maximum frequency.

### Steps

1. Use an unordered map to store frequencies.
2. Traverse the map and track the maximum frequency.
3. Return the element with frequency > ⌊n / 2⌋.

### Time Complexity

* **O(n)**

### Space Complexity

* **O(n)**

---

## Approach 2: Sorting

### Idea

* After sorting, the majority element must occupy index `n / 2`.

### Steps

1. Sort the array.
2. Return `nums[n / 2]`.

### Time Complexity

* **O(n log n)**

### Space Complexity

* **O(log n)** (sorting stack)

---

## Approach 3: Candidate–Counter (Optimal)

### Core Idea

* Pairwise cancel different elements.
* The element appearing more than ⌊n / 2⌋ times **cannot be fully canceled**.

---

## Mental Model (Very Important)

* Treat elements as votes.
* Same element → supports the candidate.
* Different element → cancels one vote.
* Majority element always survives cancellation.

---

## Rules of the Algorithm

1. If `count == 0`, pick current element as candidate.
2. If current element == candidate → `count++`
3. Else → `count--`
4. Return the final candidate.

---

## Code (Your Implementation)

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int candidate = 0, count = 0;

        for(auto it : nums){
            if(count == 0){
                candidate = it;
                count = 1;
            }else if(it == candidate){
                count++;
            }else{
                count--;
            }
        }

        return candidate;
    }
};
```

---

## Dry Run (Your Example)

### Input

```
[1, 2, 2, 2, 1] → n = 5
```

### Step-by-Step

```
count = 0, candidate = 0

i = 0 → 1
count = 1, candidate = 1

i = 1 → 2
count = 0

i = 2 → 2
count = 1, candidate = 2

i = 3 → 2
count = 2, candidate = 2

i = 4 → 1
count = 1, candidate = 2
```

### Result

```
Majority Element = 2
```

---

## Why This Works (Final Explanation)

* The majority element appears **more than all other elements combined**.
* Every time a different element appears, it cancels one occurrence.
* Non-majority elements can be fully canceled.
* The majority element **cannot** be fully canceled.
* Hence, it must remain as the final candidate.

---

## Invariant Maintained

* `count` represents the dominance of the current candidate over other elements seen so far.

---

## Time and Space Complexity

* **Time**: O(n)
* **Space**: O(1)

---

## Interview One-Liners

**Q: Why does this work without counting frequencies?**
A: Because the majority element cannot be canceled completely.

**Q: Why is a second pass not needed here?**
A: The problem guarantees a majority element exists.

**Q: When would this fail?**
A: If no majority element is guaranteed.


