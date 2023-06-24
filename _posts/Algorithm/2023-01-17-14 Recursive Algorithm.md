---
title: "[Lecture Summary] 14 Algorithm and Data Structure : Recursive Algorithm"
toc: true
toc_sticky: true
toc_lable: "Main Contents"
use_math: true
categories:
  - Study
tags:
  - [Algorithm, DataStructure]
---

This contents is based on Lecture of [link](https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-spring-2020/pages/syllabus/)

# Overview

Algorithm is devised to be called repeatedly! In that point, we should design algorithm keeping in mind that it can be used recursively. That's why we need to be able to handle recursive property.

These are features to be considered
- For arbitrary size of input, there exists recursion.
- To analyze it, use induction!
- We can formulate recursive function call(or total problem) as a graph.
  - That's why (dependency) graph of recursive calls must be acyclic
- All we need to do is decompose problems into subproblems(subgraph)!

The thing is that we can formulate problem as a graph and we can solve it according to the "type" of graph

|Algorithm|Graph Type|
|:--:|:--:|
|Brute Force|Star|
|Decrease & Conquer|Chain|
|Divide & Conquer|Tree|
|Dynamic Programming|DAG|
|Greedy/Incremental|Subgraph|

What if we can formulate problem as a DAG? <br>
(The reason why we focus on DAG is its theoretical background)

# Dynamic Programming(DP)

Key idea
- To use DP, it should be decomposable subproblems
- Useful when dependencies(edges) overlap(more than 1 in-degree)
- Re-use it recursively called problems rather than iteratively compute!(memorization)

It is useful for optimization problems! It gradually.

(Sometimes, we call DP as "local brute force" algorithm)

## SRTBOT

All we need to do is SRTBOT! Once we formulate subproblems as a graph and their dependency as an edge in that graph, we can solve original problems just the way DFS works.

- **S** : Subproblem definition
  - Approach : Prefix($x[:i]$)/suffix($x[i:]$)/substring($x[i:j]$)
  - except substring, other approaches take linear time!
- **R** : Relate subproblems recursively
  - When it comes to relate them, define a recursive equation!
- **T** : Topological order on subproblems
  - Relation between subproblems are acyclic. This implies that child node needs parents node to solve it.
- **B** : Base case solution (First step of Induction)
- **O** : Original problem solution via subproblems
- **T** : Time analysis
  - (the number of elements) times (nonrecursive work or computation)

Because we can solve subproblems(DAG) with Dynamic Programming, we can say that
> Dynamic Programming = recursion + memorization

cf)<br>
subsequence : subset of sequence which is not necessarily continuous<br>
substring : continuous range or interval of it<br>


## Memorization

Idea : remember & re-use solution of sub-problems!

Features
- maintain dictionary which maps subproblems into solutions
- recursive function can return the already stored solution or compute solution
- No need to use any loops! 

Of course, we can solve it iteratively suing topological order.(Just the way we used before!)

# Examples

## Merge Sort

|SRTBOT|Contents|
|:--:|:--:|
|Subproblems|$S(i,j)$ : Sorted sub-array|
|Relation|$S(i,j)=$merge$(S(i,m),S(m,j))$|
|Topological Order|increasing length of sub-array|
|Base Case|$S(i,i+1)$|
|Original Problem|$S(0,n)$|
|Time|$T(n)=2T(n/2)+O(n)=\Theta(n\log n)$|

As you can see the definition of subproblems, the number of sub problem is quadratic. However, run time is $n\log n$ which is less than quadratic time.<Br>
Especially, Subprobem is tree structure so that we can solve it with divide and conquer algorithm.

## Fibonacci Numbers

|SRTBOT|Contents|
|:--:|:--:|
|Subproblems|$F(i)$ : $i$th Fibonacci number|
|Relation|$F(i)=F(i-1)+F(i-2)$|
|Topological Order|increasing $i$|
|Base Case|$F(0)=0, F(1)=1$|
|Original Problem|$F(n)$|
|Time|$T(n)=T(n-1)+T(n-2) + O(1)>2T(n-2), T(n)=\Omega(2^{n/2})$|

If we naively implement algorithm, it takes exponential time!<br>
By the way, we can formulate this problem into graph as follows:

||||||
|:--:|:--:|:--:|:--:|:--:|
|||F(n)|||
||F(n-1)||F(n-2)||
|F(n-2)|F(n-3)||F(n-3)|F(n-4)|

As you can see the above figure, structure is DAG and there are duplicates! That's why **memorization** is needed!

## Bowling pin

- Problem Definition

![제목](/assets/images/algorithm/14-0.PNG){: width="70%" height="55%"}{: .align-center}

### Suffixes approach

|SRTBOT|Contents|
|:--:|:--:|
|Subproblems|$B(i)$ : max score using from $i$th pins to $n-1$th pins|
|Relation|$B(i)=\max \lbrace B(i+1), B(i+1)+v_i, B(i+2)+v_i\cdot v_{i+1}\rbrace$|
|Topological Order|decreasing $i$|
|Base Case|$B(n)=0$|
|Original Problem|$B(0)$|
|Time|$\theta(n)\times\theta(1)$|

cf) divide and conquer algorithm

![제목](/assets/images/algorithm/14-1.PNG){: width="70%" height="55%"}{: .align-center}
