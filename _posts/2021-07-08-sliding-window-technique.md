---
title: Sliding Window Technique
date: 2021-07-08T11:20:22-08:00
layout: single
permalink: /sliding-window-technique/
category: 'programming'
tags: ['technical interviews', 'algorithms']
---

The sliding window technique is used to eliminate travel waste (nested loop) within an array whenever you need to operate on subarrays of length K.

This technique allows you to convert a time complexity of `O(N * K)` to a single linear pass of `O(N)`.

With a brute force solution, you'd use two loops, where the outer loop increments the subarray starting point, and the inner loop sequentially adds K elements to build the subarray. All of the inner loop's work is lost/reset when the outer loop increments to the next starting element. 

This results in unnecessary repetition: re-iterating over all of the prior subarray's elements between the first and last; these are common between adjacent subarrays.

Instead of building a new subarray from each starting element, you can create the next subarray by performing two modifications to the current one:
1. remove prior subarray's first element from current subarray.
2. add the next array element to current subarray.

This operation is a bit like an inchworm, inching along the parent array.

## The basic sliding window

**Goal**: Given an array, get a list of averages for each subarray of length K.

1. Take an array `arr` and a subarray length `k`.
2. Init a container for your `results`, to store the averages of each subarray.
3. Init an intermediary accumulator value `sum`, to store the sum of elements in the current subarray.
4. Init a counter `windowStart`, representing the index of the start of the sliding window.
5. For each element at index `windowEnd` in the array,
    1. Add the element to the accumulator.
    2. If we have a complete subarray (`windowEnd` is greater than `k-1`),
        1. Calculate and append result for this subarray.
        2. Remove the element at `windowStart` from `sum`.
        3. Increment `windowStart` to next index.
6. Return `results`.