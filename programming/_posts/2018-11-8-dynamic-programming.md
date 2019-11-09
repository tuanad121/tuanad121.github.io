---
layout: post
title:  Dynamic Programming Problems
categories: [programming]
tags: [dynamic programming, interview, Python]
---

## Allocate Books

N number of books are given. 
The ith book has Pi number of pages. 
You have to allocate books to M number of students so that maximum number of pages alloted to a student is minimum. A book will be allocated to exactly one student. Each student has to be allocated at least one book. Allotment should be in contiguous order, for example: A student cannot be allocated book 1 and book 3, skipping book 2.

NOTE: Return -1 if a valid assignment is not possible

Input:

```
List of Books
M number of students
Your function should return an integer corresponding to the minimum number.
```
Example:

```
P : [12, 34, 67, 90]
M : 2

Output : 113
```

```
There are 2 number of students. Books can be distributed in following fashion : 
  1) [12] and [34, 67, 90]
      Max number of pages is allocated to student 2 with 34 + 67 + 90 = 191 pages
  2) [12, 34] and [67, 90]
      Max number of pages is allocated to student 2 with 67 + 90 = 157 pages 
  3) [12, 34, 67] and [90]
      Max number of pages is allocated to student 1 with 12 + 34 + 67 = 113 pages

Of the 3 cases, Option 3 has the minimum pages = 113. 
```

__HINT:__

* Firstly, check if the number of books is enough for all students.
* We use a dynamic programming approach, the DP matrix has count\_pages[i][j] as the maximum pages assigned to one student when i students read j books. 
* count\_pages[i][j] = max(count_pages[i-1][k], sum( [ pages of all books from k to j ] )) with 0 <= k < j
* we try to minimize count\_pages[i][j]

```python
class Solution:
    # @param A : list of integers
    # @param B : integer
    # @return an integer
    def books(self, A, B):
        n = len(A)
        if n < B:
            return -1
        cumsum = [0] * n
        cumsum[0] = A[0]
        # calculate cummulative sum
        for i in range(1,n):
            cumsum[i] = A[i] + cumsum[i-1]
        count_pages = [[0 for i in range(n)] for j in range(B)]
        
        for i in range(n):
            count_pages[0][i] = cumsum[i]
        
        for j in range(B):
            count_pages[j][0] = cumsum[0]
        
        for j in range(1, n):    
            for i in range(1, B):
                best = 10 ** 6
                for p in range(j):
                    
                    best = min(best, max(count_pages[i-1][p], cumsum[j]-cumsum[p]))
                
                count_pages[i][j] = best
        return count_pages[-1][-1]
```

##Painters's partition problem

You have to paint N boards of length {A0, A1, A2, A3 … AN-1}. There are K painters available and you are also given how much time a painter takes to paint 1 unit of board. You have to get this job done as soon as possible under the constraints that any painter will only paint contiguous sections of board.

```
2 painters cannot share a board to paint. That is to say, a board
cannot be painted partially by one painter, and partially by another.
A painter will only paint contiguous boards. Which means a
configuration where painter 1 paints board 1 and 3 but not 2 is
invalid.
```

Return the ans % 10000003

Input :
K : Number of painters
T : Time taken by painter to paint 1 unit of board
L : A List which will represent length of each board

Output:
     return minimum time to paint all boards % 10000003
Example

```
Input : 
  K : 2
  T : 5
  L : [1, 10]
Output : 50
```

```python
class Solution:
    def cumsum(self, C):
        n = len(C)
        cum = [0 for i in range(n)]
        cum[0] = C[0]
        for i in range(1,n):
            cum[i] = cum[i-1] + C[i]
        return cum
    # @param A : num of painters
    # @param B : time taken by painter to pain 1 unit
    # @param C : list of integers
    # @return an integer
    def paint(self, A, B, C):
        n = len(C) # number of jobs
        
        # cummulative jobs
        cum = self.cumsum(C)
        
        # intialize M; (A x n) array
        M=[[0 for i in range(n)] for j in range(A)]
        
        for i in range(n):
            M[0][i] = cum[i]
        
        for i in range(A):
            M[i][0] = C[0]
        
        for i in range(1, n):
            for j in range(1, A):
                best = 10**10
                for p in range(i):
                    best = min(best, max(M[j-1][p], cum[i]-cum[p]))
                M[j][i] = best
        
        return M[A-1][n-1] * B %  10000003
```

## Distinct Subsequences

Given two sequences S, T, count number of unique ways in sequence S, to form a subsequence that is identical to the sequence T.

Example :

```
S = "rabbbit" 
T = "rabbit"
```

Return 3. And the formations as follows:

```
S1= "ra_bbit" 
S2= "rab_bit" 
S3="rabb_it"
```

"_" marks the removed character.

```python

class Solution:
    # @param A : S string
    # @param B : T string
    # @return an integer
    def numDistinct(self, A, B):
        n = len(A)
        m = len(B)
        if m > n:
            return None
        count = [[0 for i in range(n+1)] for j in range(m+1)]
        for i in range(n+1):
            count[0][i] = 1
#         for i in range(1, m+1):
#             count[i][0] = 0
            
        for i in range(1,m+1):
            for j in range(1,n+1):
           
                if B[i-1] != A[j-1]:
                    count[i][j] = count[i][j-1]
                else:
                    count[i][j] = count[i][j-1] + count[i-1][j-1]
        return count[m][n]

```
