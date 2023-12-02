---
title: LeetCode 219 - Contains Duplicate II
date: 2023-12-01T08:12:22-08:00
layout: single
permalink: /leetcode-219-contains-duplicate-ii
category: 'programming'
tags: ['leetcode', 'arrays_and_hashing']
---

# LeetCode 219 - Contains Duplicate II

[LeetCode - Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/description/)

## Principles

* Use storage to track iteration progress.
* A dictionary combines a set (keys) with a relationship (values for each key).
* Use a dictionary to collect uniques + additional data. 
* Example: tracking pairs of values during iteration, when one value is static (list element) and the other is updated (last seen index). Add the static value as a key, check for the key's existence, and update dynamic value for that key as you go.
* When searching for qualifying pairs, only store the minimum you need for comparison (the most recent index, because you already have the current index).

## Prompt

Given an integer array `nums` and an integer `k`, return true if there are two distinct indices `i` and `j` in the array such that `nums[i] == nums[j]` and `abs(i - j) <= k`.

Constraints:

* `1 <= nums.length <= 105`
* `-109 <= nums[i] <= 109`
* `0 <= k <= 105`

## Discussion

We're looking for numerically identical elements in a list, but we have an additional criterium for the relationship of their indices if we find a match: that the absolute value of index 1 minus index 2 be less than or equal to some limit `k`.

What happens to this absolute value as we iterate through a list of length 3?

| i | j | abs(i-j) |
|---|---|---------|
| 0 | 0 | 0       |
| 0 | 1 | 1       |
| 0 | 2 | 2       |
| 1 | 0 | 1       |
| 1 | 1 | 0       |
| 1 | 2 | 1       |
| 2 | 0 | 2       |
| 2 | 1 | 1       |
| 2 | 2 | 0       |

We can see that the absolute value increments to a limit, then takes the value of `i`, and cycles this pattern.


## Solution 1 - O(N^2)

```python

def containsNearbyDuplicate(nums, k):
    def criteria_one(nums, idx1, idx2):
        return True if nums[idx1] == nums[idx2] else False

    def criteria_two(idx1, idx2, k):
        return True if abs(idx1-idx2) <= k else False

    for i in range(len(nums)):
        for j in range(i+1, len(nums)):		
            if criteria_one(nums, i, j) and criteria_two(i, j, k):
                return True
    return False
```

This is a brute force solution that works, but has a lot of travel waste due to the nested loops checking combinations we've already seen.

We should see if we can refactor this to a single linear pass using storage to track our progress so far.

## Solution 1 - O(N)

The termination condition contains two criteria: a duplicate value, but also a numerical relationship between the indices of the duplicates.

So we need to track values for the equality check, as well as the indices of two values.

We already have the current index from iteration, so we only need to track one index for any value we've seen before.

So we need to track the value, and the index we most recently saw that value at.

Since we have two values to track, and since one of them won't change (element value) and the other will change (last index of the value), we can use a dictionary where the key is the element value, and the property is the last seen index.

The dictionary keys also function as a set, so we can efficiently check if we've already seen an element.

We'll init a storage map, and do a single linear pass through the set of nums. 

For each num, we'll:
* check if it's in the map keys yet
* if so, we have a duplicate, so we'll check the relationship of the current index to the most recent index stored in the map.
* if the relationship of indices meets the criteria, we've found a qualifying match and can return true.
* otherwise, we update the most recent index for that key and move on
* otherwise, if the num isn't in the map, add it as a key, and store the current index there.
* if the loop terminates, no qualifying pair was found, so we return false.

```python
def containsNearbyDuplicate(nums, k):
    # Create a dictionary to store the last seen index of each element
    num_to_last_idx_map = {}
    # Iterate through the list
    for i, num in enumerate(nums):
        # Check if the element is already in the dictionary
        if num in num_to_last_idx_map:
            # If yes, check the absolute value of diff b/w current index and the last seen index
            if abs(i - num_to_last_idx_map[num]) <= k:
                return True
            else:
                # If the difference is greater than k, update the last seen index
                num_to_last_idx_map[num] = i
        else:
            # If the element is not in the dictionary, add it
            num_to_last_idx_map[num] = i
    return False
```

## Summary

For an iterative pair check, instead of neesting loops, we use a map/set that tracks two values: the element's value and it's last index. 

Then we perform checks. Have we seen this element yet? If no, add it. If yes, compare indices. Do they qualify? If yes, return true and we're done. If no, update index and continue. If no qualifying pair found, return false and we're done.