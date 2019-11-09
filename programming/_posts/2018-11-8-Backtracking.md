---
layout: post
title:  Backtracking Problems
categories: [programming]
tags: [backtracking, interview, Python]
---

## Generate all Parentheses II

Check the detail explaination [here](https://www.youtube.com/watch?v=LxwiwlUDOk4)

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses of length 2*n.

For example, given n = 3, a solution set is:

```
"((()))", "(()())", "(())()", "()(())", "()()()"
```

Make sure the returned list of strings are sorted.

We keep track number of avalaible parenthesis and number of unclosed parenthesis.

Hints:

* If there is no available parenthesis, we have to close every unclosed ones.
* If there is no unclosed parenthesis, we can open a new parenthesis.
* Otherwise, we can open a new parenthesis or close an unclosed one.

```python
class Solution:
    def recursive_gen(self, curr_str, num_available, num_unclosed):
        if num_available == 0:
            return [curr_str + ')' * num_unclosed]
        if num_unclosed == 0:
            return self.recursive_gen(curr_str+'(', num_available-1, num_unclosed+1)
        return self.recursive_gen(curr_str+'(', num_available-1, num_unclosed+1) \
        + self.recursive_gen(curr_str+')', num_available, num_unclosed-1)
    # @param A : integer
    # @return a list of strings
    def generateParenthesis(self, A):
        if A==0:
            return []
        return self.recursive_gen('', A, 0)
```

## Combination Sum II

Given a collection of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

Each number in C may only be used once in the combination.

Note:
All numbers (including target) will be positive integers.
Elements in a combination (a1, a2, … , ak) must be in non-descending order. (ie, a1 ≤ a2 ≤ … ≤ ak).
The solution set must not contain duplicate combinations.
Example :

```
Given candidate set 10,1,2,7,6,1,5 and target 8,

A solution set is:

[1, 7]
[1, 2, 5]
[2, 6]
[1, 1, 6]
```


```python
class Solution:
    def recursive_comb(self, A, start, path, result, curr_B):
        if not curr_B:
            result.append(path)
            return
        for i in range(start, len(A)):
            if i > start and A[i] == A[i-1]:
                continue
            if A[i] > curr_B:
                break
            self.recursive_comb(A, i+1, path+ [A[i]], result, curr_B-A[i])
    # @param A : list of integers
    # @param B : integer
    # @return a list of list of integers
    def combinationSum(self, A, B):
        A.sort() # sort the array
        result = []
        self.recursive_comb(A, 0, [], result, B)
        return result
```

## Kth Permutation Sequence

The set [1,2,3,…,n] contains a total of n! unique permutations.

By listing and labeling all of the permutations in order,
We get the following sequence (ie, for n = 3 ) :

```
1. "123"
2. "132"
3. "213"
4. "231"
5. "312"
6. "321"
```

Given n and k, return the kth permutation sequence.

For example, given n = 3, k = 4, ans = "231"

```

Good questions to ask the interviewer :
What if n is greater than 10. How should multiple digit numbers be represented in string?
In this case, just concatenate the number to the answer.
so if n = 11, k = 1, ans = "1234567891011" 
Whats the maximum value of n and k?
In this case, k will be a positive integer thats less than INT_MAX.
n is reasonable enough to make sure the answer does not bloat up a lot. 

```

The idea is as follow:

For permutations of n, the first (n-1)! permutations start with 1, next (n-1)! ones start with 2, ... and so on. And in each group of (n-1)! permutations, the first (n-2)! permutations start with the smallest remaining number, ...

take n = 3 as an example, the first 2 (that is, (3-1)! ) permutations start with 1, next 2 start with 2 and last 2 start with 3. For the first 2 permutations (123 and 132), the 1st one (1!) starts with 2, which is the smallest remaining number (2 and 3). So we can use a loop to check the region that the sequence number falls in and get the starting digit. Then we adjust the sequence number and continue.

```python

import math
class Solution:
    # @param A : integer
    # @param B : integer
    # @return a strings
    def getPermutation(self, A, B):
        result = ''
        nums = range(1, A+1)
        B -= 1
        while A > 0:
            A -= 1
            index, B = divmod(B, math.factorial(A))
            result += str(nums[index])
            nums.remove(nums[index])
        return result
```
