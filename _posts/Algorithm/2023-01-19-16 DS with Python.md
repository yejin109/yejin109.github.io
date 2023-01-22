---
title: "[Lecture Summary] 16 Algorithm and Data Structure : DS with Python"
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

# Overview

This post describes how we can implement well-known data structure manually in Python. Surely there exists some structure which is hard to get implemented in Python lang and most of data structure are already implemented and optimized to be used. Therefore, it is not necessary and urgent task but useful experience to implement data structure by myself. Additionally, making up environment to test data strucutre quality, such as time to run a specific algorithm for several time, will be useful for further development progress. So I'd like to introduce some implementation codes and test results.

# Test Environment

To test whether implementation of an algorithm, we need to check 

- I/O : For a given input, it need to return expected output
- Runtime : how many time it takes for several times. 

So, I devised to test a given function by iteratively run the same function. To do so, I used following code 


```
...

def partition(func):
    def wrap(*args, **kwargs):
        print('='*50)
        print('Function Name : ', func.__name__)
        print("Result are -->")
        res = func(*args, **kwargs)
        print('='*50)
        return res
    new_func = wrap
    new_func.__name__ = func.__name__
    return new_func

def check_time(func):
  def wrap(*args, **kwargs):
      start = time.time()
      res = func(*args, **kwargs)
      print(f"Runtime : {time.time()-start: .9f}s")
      return res
  new_func = wrap
  new_func.__name__ = func.__name__
  return new_func


@partition
@check_time
def test_func(func, *args, **kwargs, ):
    print('Target Function : ', func.__name__)
    print(f"Inputs : \n Argument : {args} \n Keyword Argument : {kwargs}")
    for i in range(env.TEST_ITERATION):
        res = func(*args, **kwargs)
        # if i == 0:
    print(f"Outputs : \n {res}, Iteration : {env.TEST_ITERATION}")
...
```

As you can see the above codes, we can easily use function "test_func". Once we set target function and arguements of target function as inputs of "test_func", it prints results like

```
==================================================
Function Name :  test_func
Result are -->
Target Function :  get_at
Inputs : 
 Argument : (0,) 
 Keyword Argument : {}
Outputs : 
 -1, Iteration : 1000000
Runtime :  0.105023623s
==================================================
```

See more details at :<br>
Github : [source code](https://github.com/yejin109/Data-Structure-and-Algorithm/blob/main/utils/log.py)


# Interface: Sequence

Operations to be used : <br>

![API](/assets/images/algorithm/1-0.PNG){: width="70%" height="55%"}{: .align-center}

## Array

Some already know that there are "array" python package and all we need to implement is **Static** array. Because python offers Python list which is **dynamic** array and it is hard to handle index and allocation of memory, I used Python list to circumvent and this can degrade the performance of following performance

Github : [source code](https://github.com/yejin109/Data-Structure-and-Algorithm/blob/main/structure/array.py)


## Linked List

To implement Linked List, we need node and node has **item** and **pointer** of next(child) node as attributes(features). So we can define Node class first and use it. 

The thing is that I used "get at" operation for implementing other operations. This does affect the overall performance if I cannot properly implement "get at" operation. 

For example,

```
...
    def get_at(self, i):
        pointer = self.head.next
        for i_pointer in range(i):
            next_node = ctypes.cast(pointer, ctypes.py_object).value
            pointer = next_node.next
        return ctypes.cast(pointer, ctypes.py_object).value
...
    def insert_at(self, i, x):
        assert i >= 0, f"Wrong index : {i}"
        if i > 0:
            past_node = self.get_at(i-1)
            next_node = self.get_at(i)
            new_node = Node(x, id(next_node))
            past_node.next = id(new_node)
        else:
            self.insert_first(x)
```

Because get_at operation of Linked List take linear time(it uses for-loop!), insert_at operation also takes linear time. On the other hand, if i improve performance of get_at, then it will enhance other operation either.


Github : [source code](https://github.com/yejin109/Data-Structure-and-Algorithm/blob/main/structure/linked_list.py)

