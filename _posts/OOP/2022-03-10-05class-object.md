---
title: "[Lecture Note] 객체지향 프로그래밍 - 02 클래스와 객체 2 (1)"
toc: true
use_math: true
categories:
  - OOP
tags:
  - [JAVA]
---

객체지향프로그래밍 입문 [강의](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%9E%85%EB%AC%B8/dashboard)의 강의 내용을 참고하여 정리한 글입니다.

# this

- 자신(인스턴스)의 메모리를 가리킴(point)

인스턴스를 생성하게 되면 class가 참조 자료형으로 참조변수에 Heap 메모리 내 주소값(16진수)인 참조값으로 할당하게 되었다.

이제 이 인스턴스 **내부**에서 어떤 작업들이 진행될 때 인스턴스 자신의 attribute 등을 지칭해야하는 일이 생기는데, 이 때 사용하는 것이 this.<br>
그래서 같은 class로 생성된 서로 다른 인스턴스에서 this는 서로 다른 메모리를 가리키게 된다. 


- 생성자에서 다른 생성자 호출

멤버변수(attribute)들을 초기화하는 작업이 주로 생성자 내부에서 진행되고, 생성자가 여러가지 존재할 수 있고 parameter만 다른 경우에는 **constructor overloading**이라고 했다. 이 때 한 생성자 냉부에서 또 다른 생성자를 호출하고 싶을 때 this를 사용하게 된다. 

사용하는 예시(이유)로 default 생성자에서 초기값을 작성할 때 똑같은 방식으로 생성하는 생성자가 있다면 그 생성자를 사용할 수 있게 하는 것.
 - 뭔가 생성과정이 복잡해지고 기능에 따라 달라질 때 사용하는 것 같다.

이렇게 사용할 때엔 this 이전에 새로운 statement는 올 수 없다. 왜냐하면 this가 호출되어 생성이 되기 전에는 메모리에 올라간 인스턴스가 없으니 statement에 사용된 변수들은 결국 메모리가 없는 상황에서 가리킬 대상이 없으므로 에러가 생기는 것.
 - 이건 어떻게 생각해보면 Heap 메모리 상에 주소가 없는 상황에서 멤버변수든 지역변수든 할당을 할 수 없는 것으로 생각하면 되지 않을까

- 자신의 주소 반환

# Key

항상 코드는 가장 가까운 값을 참조하게 된다. 