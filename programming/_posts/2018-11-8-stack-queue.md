---
layout: post
title:  Stack and Queue Problems
categories: [programming]
tags: [stack, queue, interview, Python]
---

## Largest rectangle from histogram
Given n non-negative integers representing the histogram’s bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

Largest Rectangle in Histogram: 

Example 1

![figure1](/assets/hist1.png)


Above is a histogram where width of each bar is 1, given height = [2,1,5,6,2,3].

Largest Rectangle in Histogram: 

Example 2

![figure1](/assets/hist2.png)

The largest rectangle is shown in the shaded area, which has area = 10 unit.

For example,
Given height = [2,1,5,6,2,3],
return 10.

```python
class Solution:
    # @param A : list of integers
    # @return an integer
    def largestRectangleArea(self, A):
        stack =[-1]
        maxArea = 0
        for i in range(len(A)):
            while (stack[-1]!=-1) and A[i] < A[stack[-1]]:
                last_idx = stack.pop()
                maxArea = max(maxArea, A[last_idx]*(i-stack[-1]-1))
            stack.append(i)
        
        while stack[-1]!=-1:
            last_idx = stack.pop()
            maxArea = max(maxArea, A[last_idx]*(len(A)-stack[-1]-1))
        return maxArea
```

## Min Stack
Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

push(x) – Push element x onto stack.
pop() – Removes the element on top of the stack.
top() – Get the top element.
getMin() – Retrieve the minimum element in the stack.
Note that all the operations have to be constant time operations.

Questions to ask the interviewer :

```
Q: What should getMin() do on empty stack? 
A: In this case, return -1.

Q: What should pop do on empty stack? 
A: In this case, nothing. 

Q: What should top() do on empty stack?
A: In this case, return -1
```

NOTE : If you are using your own declared global variables, make sure to clear them out in the constructor. 

__HINT__: 

* The retrieve the minimum element in constant time, we need to calculate it before hand, and store it.
* When you pop() an element out of the stack, or push() an element to the stack, we may change the minimum value.
* When we push() an new element and it becomes new minimum value, we update the new minimum value and store the old minimum value. Then, when we pop() out the minimum value, we can retrieve current minimum value in a constant time.
* We can store current minimum in an extra stack, or we can store a pair of (value, minimum) in a single stack.

```python
class MinStack:
    def __init__(self):
        self.stack = []
        
    # @param x, an integer
    def push(self, x):
        if not self.stack:
            self.stack.append((x,x))
            return
        min_stack = self.stack[-1][1]
        if x < min_stack:
            self.stack.append((x, x))
        else:
            self.stack.append((x, min_stack))


    # @return nothing
    def pop(self):
        if not self.stack:
            return
        x, min_stack = self.stack.pop()
        return x

    # @return an integer
    def top(self):
        if self.stack:
            return self.stack[-1][0]
        else:
            return -1

    # @return an integer
    def getMin(self):
        if self.stack:
            return self.stack[-1][1]
        else:
            return -1

```

## Max Rectangle in Binary Matrix

Given a 2D binary matrix filled with 0’s and 1’s, find the largest rectangle containing all ones and return its area.

Bonus if you can solve it in O(n^2) or less.

Example :

```
A : [  1 1 1
       0 1 1
       1 0 0 
    ]

Output : 4 
```

As the max area rectangle is created by the 2x2 rectangle created by (0,1), (0,2), (1,1) and (1,2)

__Hint:__
We can treat the problem as __Largest rectangle from histogram__. In the case, each row will be treat as a x-axis for those histogram. Ones mean there is a histogram here, zeros mean there is no histogram here. For each rows, we calculate the heights of each histogram, then we treat the problem as __Largest rectangle from histogram__ 

```python
class Solution:
    # @param A : list of list of integers
    # @return an integer
    def maximalRectangle(self, A):
        if not A or not A[0]: return 0
        n = len(A)
        m = len(A[0])
        heights = [0] * (m+1)
        ans = 0
        
        for row in A:
            # imagine at each row we build up m rectangles 
            # with width=1
            # and different heights
            # calculate heights at current row:
            for i in range(m):
                heights[i] = heights[i] + 1 if row[i] == 1 else 0
            stack = [-1]
            for i in range(m+1):
                while heights[i] < heights[stack[-1]]:
                    h = heights[stack.pop()]
                    w = i - 1 - stack[-1]
                    ans = max(ans, w*h)
                stack.append(i)
        return ans
```
