---
layout: post
title:  Two Pointers Problems
categories: [programming]
tags: [two pointers, interview, Python]
---

## Max Distance

Given an array A of integers, find the maximum of j - i subjected to the constraint of A[i] <= A[j].

If there is no solution possible, return -1.

Example :

```
A : [3 5 4 2]

Output : 2 
for the pair (3, 4)
```
For each element in the array, we keep track the minimum value from the element (inclusive) to the left side, also keep track the maximum value from the element to the right side. 

Prepare 2 pointers start from 0 to the end of the array. Pointer i is to manage the minimum value and pointer j is to manage the maximum value.

If minimum value of the left side of i is smaller than the maximum of the right side of j, we update max distance j - i and increase j. Else, we increase i in order to find a new minimum.


```python
class Solution:
    # @param A : tuple of integers
    # @return an integer
    def maximumGap(self, A):
        n = len(A)
        LMin = [0] * n
        RMax = [0] * n
        
        LMin[0] = A[0]
        for i in range(1, n):
            LMin[i] = max(LMin[i-1], A[i])
        
        RMax[-1] = A[-1]
        for i in range(n-2,-1,-1):
            RMax[i] = max(A[i], RMax[i+1])
        
        i=0
        j=0
        max_gap=-1
        while (i<n) and (j<n):
            if LMin[i] < RMax[j]:
                max_gap = max(max_gap, j - i)
                j += 1
            else:
                i += 1
        return max_gap
```

## Min Jumps Array

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

Example :
Given array A = [2,3,1,1,4]

The minimum number of jumps to reach the last index is 2. (Jump 1 step from index 0 to 1, then 3 steps to the last index.)

If it is not possible to reach the end index, return -1.

```python
class Solution:
    # @param A : list of integers
    # @return an integer
    def jump(self, A):
        n = len(A)
        if n==0:
            return -1
        if n > 1 and A[0]==0: return -1
        if n == 1: return 0
        jumps = [0] * n
        for i in range(1, n):
            jumps[i] = float('inf')
            for j in range(i):
                if (i <= j + A[j]) and jumps[j] != float('inf'):
                    jumps[i] = min(jumps[i], jumps[j]+1)
        return jumps[-1]
```

It's an O(n) solution. In the solution, we use 2 pointers l and r to keep track starting point and max reach for a current jump. We estimate nxt pointer as the maximum reach (in 1 jump) from any point betwen [l, r]. We update l, r as r, nxt respectively. It's because we wanna go further than r so we have to start from points between [r, nxt]. 


```python
class Solution:
    def jump(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) <= 1: return 0
        l, r = 0, nums[0] # l is starting point, r is max reach
        num_steps = 1
        while r < len(nums)-1: # 
            num_steps += 1
            nxt = max(i+nums[i] for i in range(l, r+1))
            # because nxt is the max reach of any points between [l,r]
            # wanna go beyond nxt, start from points between [r, nxt]
            l, r = r, nxt 
                             
        return num_steps
```
