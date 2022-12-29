---
title: "[Lecture Summary] 09 Algorithm and Data Structure : Graph Problems"
toc: true
toc_sticky: true
toc_lable: "Main Contents"
use_math: true
categories:
  - Algorithm
tags:
  - [Algorithm, DataStructure]
---

This contents is based on Lecture of [link](https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-spring-2020/pages/syllabus/)


# Problem to be solved

There are lots of problems in graph structured data. The thing is that we need to know about I/O for each problem and corresponding algorithm! When it comes to understanding algorithm, we need to prove correctness from the point of view of target problem and I/O.

## Topological Sort

- Topological Order

ordering $f$ of vertices such that every edge $(u,v)$ satisfies $f(u)<f(v)$

- Finishing Order

order completed Full-DFS vertices.<br> completed vertex comes first 

KEY! **If G is Directed Acyclic Graph(DAG), reverse of a finishing order is topological order**.

Personally, we can interprete as follows:<br>
topologically far vertex is the latest recursion! Therefore, reverse of a finishing order is order of topoligically distance.


## Cycle Detection

For directed graph, we can detect cycles in graph. This is available because for every DAG, the above statement satisfies. That is, if graph is not DAG, then every directed graph but not an acyclic graphs which contains cycles cannot satisfy that statement.

To return cycles, trace ancestor(or parent) in **Full-DFS**. In sumamry, use topological sort to check whether there exist cycles or not and Full-DFS to specify cycles.

Input : Graph<br>
Output : cycles<br>
Algorithm : **Full-DFS**

## Connected Component

parition of $V$ into subsets which subgraph for corresponding subset is connected. 

Input: Graph<br>
Output: Component<br>
Algorithm : Full-BFS or Full-DFS<br>

## Singel Pair Reachability

Input : Graph, source node $s$ and target node $t$<br>
Output : bool

## Singel Source Reachability

Input : Graph, source node $s$<br>
Output : bool for all $v \in V$ <br>
Algorithm : [DFS](https://yejin109.github.io/algorithm/09-BFS/#depth-first-search) or BFS

## Single Pair Shortest Path

Input : Graph, source node $s$ and the other node $t$<br>
Output : distance $\delta(s,t)$ and shortest path in G

## Single Soursce Shortest Paths

Input : Graph, source node $s$<br>
Output : distances for all $v \in V$ and shortest path tree which contains shortest path from $s$ to every $v \in V$<br>
Algorithm : 

![제목](/assets/images/algorithm/9-0.PNG){: width="70%" height="55%"}{: .align-center}



