---
title: "[Lecture Note] 객체지향 프로그래밍 - 09 배열과 ArrayList (1)"
toc: true
use_math: true
categories:
  - OOP
tags:
  - [JAVA]
---

객체지향프로그래밍 입문 [강의](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%9E%85%EB%AC%B8/dashboard)의 강의 내용을 참고하여 정리한 글입니다.


배열의 기능을 JDK에서 제공하는 클래스가 ArrayList

# 배열
자료구조에서 **동일한 자료형**의 데이터가 연속적으로, **순차적**으로 나타나있는 구조를 말한다.<br>
결국 이 또한 **코드의 반복성**을 줄여준다는 점에서 큰 역할을 한다.<br>

예) <br>
여러 참조 변수들이 있다고 할 때 참조변수들마다 서로 다른 변수명으로 선언해야 하는 것을 한번에 정리할 수 있다.<br>
int type으로 length=10이라면 총 40byte의 메모리를 할당되게 된다.

## 특징
실제로 메모리에 연속적으로 값을 가지게 된다. 정말 **물리적**으로도 연속되어 있으며, **논리적**으로도 연속적이라고 할 수 있다.<br>

- 특성 :아래의 표현들은 상황에 따라 지칭하는 대상이 엇갈릴 수 도 있긴 하다.<br>
  - length : 전체 개수
  - size : 실제로 데이터가 할당되어 있는 개수

- index : 배열의 length(n)인 경우 index는 [0, n-1]이 된다.

- 사이에 있는 데이터를 지우게 되는 경우 나머지 데이터들의 index가 당겨져서 비우지 않게 한다. 즉 **연속된 자료구조**가 된다.

- 처음에 length를 정해 선언해서 사용해야 한다. 즉 fixed length를 가지게 된다. 
  - 배열을 처음에 정한 length를 다 채우면 자동으로 늘어나지 않기 때문에 더 큰 배열을 따로 선언해서 옮겨서 사용해야 한다.

## 선언 방식

- 기본 구조

```
<자료형>[] <변수명> = new <자료형>[length];
<자료형> <변수명>[] = new <자료형>[length];
```

- 초기화 방식 중 가능한 것

```
int[] numbers = {1,2,3};		
int[] numbers = new int[] {0,1,2};
int[] numbers = new int[3]; // 초기화하지 않으면 length를 알려주어야 한다.
numbers[0] = 1;
numbers[1] = 2;
numbers[2] = 3;
int[] numbers;
numbers = new int[3]; // 나중에 생성 가능. 단 이 경우 정수는 0, double은 0.0/ 객체는 null로 초기화 된다.
numbers = new int[] {1,2,3}; // 나중에 생성 가능
```
		
- 초기화 방식 중 불가능한 것

```
int[] numbers;
numbers = {1,2,3};
```


## Implementation

> 초기화 값

별도의 값을 알려주지 않은 경우 int는 0, double은 0.0, 객체는 null로 초기화 된다.






