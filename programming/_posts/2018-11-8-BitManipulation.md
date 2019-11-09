---
layout: post
title:  Bit Manipulation Problems
categories: [programming]
tags: [bit manipulation, interview, Python]
---

##Single Number III

Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.

Example:

```
Input:  [1,2,1,3,2,5]
Output: [3,5]
```
Note:

The order of the result is not important. So in the above example, [5, 3] is also correct.
Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?

```python

class Solution:
    def singleNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        xor = nums[0]
        for num in nums[1:]:
            xor ^= num
        bit = xor & ~(xor-1) # find the last bit = 1
        num1 = 0 
        num2 = 0
        for num in nums:
            if (num&bit) != 0:
                num1 ^= num
            else:
                num2 ^= num
        return num1, num2
```


## Insertion
You are given 2 32-bit numbers, N and M, and two bit positions, i and j. Write a method to insert M into N such that M starts at bit j and ends at bit i. You can assume that the bits j through i have enough space to fit all of M. That is, if M = 10011, you can assume that there are at lest 5 bits between j and i. You would not, for exampe, have j=3 and i=2 because M could not fully fit between between bit 3 and bit 2.

Example: 
Input: N = 10000000000, M = 10011, i=2, j=6
Output: N = 10001001100

```python
def insert(N, M, i, j): # e.g. i=2, j=4
    M = M << i
    allOnes = ~0
    left = allOnes << j+1 # left = 11100000
    right = (1<<i) - 1 # right = 00000011
    mask = left or right # 11100011
    N = N and mask # clear bit j through i
    return M or N
```

### Different Bits Sum Pairwise
We define f(X, Y) as number of different corresponding bits in binary representation of X and Y. For example, f(2, 7) = 2, since binary representation of 2 and 7 are 010 and 111, respectively. The first and the third bit differ, so f(2, 7) = 2.

You are given an array of N positive integers, A1, A2 ,…, AN. Find sum of f(Ai, Aj) for all pairs (i, j) such that 1 ≤ i, j ≤ N. Return the answer modulo 109+7.

For example,

```
A=[1, 3, 5]

We return

f(1, 1) + f(1, 3) + f(1, 5) + 
f(3, 1) + f(3, 3) + f(3, 5) +
f(5, 1) + f(5, 3) + f(5, 5) =

0 + 1 + 1 +
1 + 0 + 2 +
1 + 2 + 0 = 8
```

```python
class Solution:
    # @param A : list of integers
    # @return an integer
    def cntBits(self, A):
        ans = 0
        for i in range(32):
            count = 0
            for a in A:
                if a & (1<<i):
                    count += 1
            ans += count * (len(A)-count) * 2 
        return ans
```
