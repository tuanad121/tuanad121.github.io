---
layout: post
title:  Array Problems
categories: [programming]
tags: [array, interview, Python]
---

## Pretty Print

Print concentric rectangular pattern in a 2d matrix. 
Let us show you some examples to clarify what we mean.

Example 1:

Input: A = 4.
Output:

```
4 4 4 4 4 4 4 
4 3 3 3 3 3 4 
4 3 2 2 2 3 4 
4 3 2 1 2 3 4 
4 3 2 2 2 3 4 
4 3 3 3 3 3 4 
4 4 4 4 4 4 4 
```

Example 2:

Input: A = 3.
Output:

```
3 3 3 3 3 
3 2 2 2 3 
3 2 1 2 3 
3 2 2 2 3 
3 3 3 3 3
``` 
The outermost rectangle is formed by A, then the next outermost is formed by A-1 and so on.

You will be given A as an argument to the function you need to implement, and you need to return a __2D array__.

```python
class Solution:
    # @param A : integer
    # @return a list of list of integers
    def prettyPrint(self, A):
        if A == 1:
            return [[1]]
        n = A + (A-1)
        res = [[0 for i in range(n)] for j in range(n)]
        r1, r2, c1, c2 = 0, n-1, 0, n-1
        while r1!= r2 or c1!=c2 or r1!=c1:
            for i in range(r1, r2+1):
                res[i][c1] = A
                res[i][c2] = A
            for j in range(c1, c2+1):
                res[r1][j] = A
                res[r2][j] = A
            r1 += 1
            r2 -= 1
            c1 += 1
            c2 -= 1
            A -= 1
        res[r1][c1] = 1
        return res
```

## Max Non Negative SubArray

Find out the maximum sub-array of non negative numbers from an array.
The sub-array should be continuous. That is, a sub-array created by choosing the second and fourth element and skipping the third element is invalid.

Maximum sub-array is defined in terms of the sum of the elements in the sub-array. Sub-array A is greater than sub-array B if sum(A) > sum(B).

Example:

```
A : [1, 2, 5, -7, 2, 3]
The two sub-arrays are [1, 2, 5] [2, 3].
The answer is [1, 2, 5] as its sum is larger than [2, 3]
```

NOTE: If there is a tie, then compare with segment's length and return segment which has maximum length

NOTE 2: If there is still a tie, then return the segment with minimum starting index

__Hint:__ Traverse the array, 

* if we meet a non-negative number, add it to our current sum. Compare our current sum to maximum sum. Use pointers to keep track the starting and ending of current sum, the starting and ending of maxium sum. 


```python
class Solution:
    # @param A : list of integers
    # @return a list of integers
    def maxset(self, A):
        
        i = 0
        n = len(A)
        maxTilNow = -10**6
        sumTilNow = -10**6
        maxLen = -10**6
        maxStart = 0
        maxEnd = 0
        start = 0
        end = 0
        while (i<n):
            if (A[i]>=0):
                
                start = i
                sumTilNow = 0
                while (i<n) and (A[i] >= 0):
                    sumTilNow += A[i]
                    i += 1
                end = i-1
            
            if (maxTilNow < sumTilNow) or ((maxTilNow==sumTilNow) and ((end-start+1)>maxLen) and maxTilNow >= 0):
                
                maxStart = start
                maxEnd = end
                maxLen = end-start+1
                maxTilNow = sumTilNow
                
            i+=1
        
        if maxLen >= 0:
            return A[maxStart:maxEnd+1]
        else:
            return []
```

## Wave Array

Given an array of integers, sort the array into a wave like array and return it, 
In other words, arrange the elements into a sequence such that a1 >= a2 <= a3 >= a4 <= a5.....

Example

```
Given [1, 2, 3, 4]

One possible answer : [2, 1, 4, 3]
Another possible answer : [4, 1, 3, 2]
```

```
 NOTE : If there are multiple answers possible, return the one thats lexicographically smallest. So, in example case, you will return [2, 1, 4, 3] 
```
 
```python
 class Solution:
    # @param A : list of integers
    # @return a list of integers
    def wave(self, A):
        A = sorted(A)
        n = len(A)
        for i in range(0,n-1,2): 
        # now swap
            A[i], A[i+1] = A[i+1], A[i] 
        return A
```

##Max Distance

Given an array A of integers, find the maximum of j - i subjected to the constraint of A[i] <= A[j].

If there is no solution possible, return -1.

Example :

```
A : [3 5 4 2]

Output : 2 
for the pair (3, 4)
```

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

## Kth Smallest Element in the Array
Find the kth smallest element in an unsorted array of non-negative integers.

Definition of kth smallest element

```
 kth smallest element is the minimum possible n such that there are at least k elements in the array <= n.
In other words, if the array A was sorted, then ```A[k - 1]``` ( k is 1 based, while the arrays are 0 based ) 
```
NOTE
You are not allowed to modify the array ( The array is read only ). 
Try to do it using constant extra space.

Example:

```
A : [2 1 4 3 2]
k : 3

answer : 2
```
__HINT:__ binary search in the range of [0,max(A)]

Find the minimum of the array. Then, find the another minimum of the array ignoring all elements equal or smaller than the last minimum. Repeat until find the K smallest element.

```python
class Solution:
    
    # @param A : tuple of integers
    # @param B : integer
    # @return an integer
    def kthsmallest(self, A, B):
        last_min = - 10**6
        while(B>0):
            curr_min = 10**6
            for a in A:
                if a <= last_min: continue
                curr_min = min(curr_min, a)
            last_min = curr_min
            for a in A:
                if a == last_min:
                    B -= 1
        return last_min
```

### NUM RANGE

Given an array of non negative integers A, and a range (B, C), 
find the number of continuous subsequences in the array which have sum S in the range ```[B, C]``` or ```B <= S <= C```

Continuous subsequence is defined as all the numbers A[i], A[i + 1], .... A[j]
where 0 <= i <= j < size(A)

Example :

```
A : [10, 5, 1, 0, 2]
(B, C) : (6, 8)
ans = 3 
as [5, 1], [5, 1, 0], [5, 1, 0, 2] are the only 3 continuous subsequence with their sum in the range [6, 8]
```
 NOTE : The answer is guranteed to fit in a 32 bit signed integer. 
 
 __HINT:__ use 2 pointers lo and hi such that ```sum(A[i:lo]) > B``` and ```sum(A[i:hi]) < C```
 
 ```python
 class Solution:
    # @param A : list of integers
    # @param B : integer
    # @param C : integer
    # @return an integer
    def numRange(self, A, B, C):
        n = len(A)
        lo, hi = 0, 0
        count = 0
        for i in range(n):
            # print(i)
            if A[i] > C: continue
            if lo < i: lo = i
            if hi < i: hi = i
            
            while lo < n+1 and sum(A[i:lo]) < B:
                lo += 1
            if sum(A[i:lo]) > C: continue
        
            if hi < lo: hi=lo 
            while hi < n+1 and sum(A[i:hi]) <= C:
                hi += 1
            count += hi-lo
        return count
 ```

## Rotate Matrix

You are given an n x n 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

You need to do this in place.

Note that if you end up using an additional array, you will only receive partial score.

Example:

```
If the array is

[
    [1, 2],
    [3, 4]
]
Then the rotated array becomes:

[
    [3, 1],
    [4, 2]
]
```

```python
class Solution:
    # @param A : list of list of integers
    # @return the same list modified
    def rotate(self, A):
        
        n = len(A)
        # transpose
        for i in range(n):
            for j in range(i+1, n):
                A[i][j], A[j][i] = A[j][i], A[i][j]
        # reverse row
        for i in range(n):
            j = 0
            k = n-1
            while(k>j):
                A[i][j], A[i][k] = A[i][k], A[i][j]
                j += 1
                k -= 1
        return A
```
## N/3 Repeat Number

You’re given a read only array of n integers. Find out if any integer occurs more than n/3 times in the array in linear time and constant additional space.

If so, return the integer. If not, return -1.

If there are multiple solutions, return any one.

Example :

```
Input : [1 2 3 1 1]
Output : 1 
1 occurs 3 times which is more than 5/3 times. 
```
Use Karp-Papadimitriou-Shanker algorithm. The main idea of the algorithm is to notice that the removal of K distinct elements from the array will not change the answer.
K here is equal to 3, and we are trying to find any element with more than n/3 occurrences in the array.

```python
class Solution:
    # @param A : tuple of integers
    # @return an integer
    def repeatedNumber(self, A):
        if len(A) == 1: return A[0]
        candidate1 = 10*8
        count1 = 0
        candidate2 = 10*8
        count2 = 0
        for i in range(len(A)):
            if A[i] == candidate1:# and count1!=0:
                count1 += 1
            elif A[i] == candidate2:# and count2!=0:
                count2 += 1
            elif count1 == 0:
                candidate1 = A[i]
                count1 = 1
            elif count2 == 0:
                candidate2 = A[i]
                count2 = 1
            else:
                count1 -= 1
                count2 -= 1
        
        count1 = 0
        count2 = 0
        
        for i in range(len(A)):
            if A[i] == candidate1:
                count1 += 1
            if A[i] == candidate2:
                count2 += 1
        
        if count1 > (len(A) / 3):
            return candidate1
        if count2 > (len(A) / 3):
            return candidate2
        
        return -1
```
