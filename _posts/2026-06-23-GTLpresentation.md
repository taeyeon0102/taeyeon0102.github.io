---
title: "[GTL 발표] C++의 핵심 원리: 포인터로 연결하는 상속과 다형성"
date: 2026-06-23 18:30:00 +0900
categories: [활동 및 프로젝트, GOGDC]  # [대분류, 소분류] 구조!
tags: [C++, 상속, 다형성, 포인터, 객체지향프로그래밍]    # 소문자 해시태그들
---

이번 GTL에서 제가 발표했던 PPT 자료입니다. 아래 화면에서 한 장씩 넘겨보실 수 있습니다!
* GTL이란? GDG on Campus HUFS Tuesday Live!

<iframe src="https://docs.google.com/presentation/d/e/2PACX-1vQW6H2lPba-JdPTgdtLh1-2K1RgkFN-dZy4kh0D4uxDOFfU6UyrAvlC1PjeoLfOqx5t1ek6qh6isctB/pubembed?start=false&loop=false&delayms=10000" frameborder="0" style="width: 100%; aspect-ratio: 16 / 9;" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

## 주제 선정 이유
이 주제를 선택한 이유는 당시 1-2에 **컴퓨터프로그래밍및실습**라는 과목을 수강하고 있었는데, C++을 배우면서 해당 개념이 어렵게 느껴졌기 때문에 깔끔하게 공부 겸 정리하고 다른 분들께도 쉽게 설명하면서 중요 개념을 알려드리고 싶었기 때문에 이 주제를 선정하였습니다.

사실 GOGDC에는 컴공 뿐만 아니라 (비중이 가장 크긴 하지만) 다양한 전공을 가진 사람이 많습니다. 그리고 C언어나 C++을 모르는 분들도 있기에 해당 개념을 준비했습니다.

 ~~사실 당시 아직 1학년이어서 경험이 별로 없어서 이런 기술 주제를 삼은 것은 안비밀... 더 최신 기술을 소개하는 재미있는 주제로 하지 못한게 조금 아쉽다 ㅠㅠ 좀 노잼 주제였던 것 같다~~
 

## 📌 주요 개념 정리

### 포인터 (Pointer)
- 메모리 주소를 담는 변수

### 객체 지향 프로그래밍 (Object-oriented Programming, OOP)
- 데이터(상태)와 함수(동작)을 하나로 묶어서 생각하여, 내부의 구체적인 코드나 원리를 몰라도 해당 객체를 이용하여 프로그래밍을 할 수 있다.
- C++에서는 클래스를 통해서 만들 수 있다.

### 상속 (inheritance)
- 한 클래스가 다른 클래스의 상태, 동작법을 물려받아 재사용함
- 기존 코드를 재사용하고, 클래스 간의 계층적 구조를 구축하여 클래스의 확장성과 유지보수성을 높임.

### 다형성 (Polymorphism)
- 같은 연산이 객체에 따라 다르게 동작할 수 있게 함
- 가상 함수와 `virtual` 키워드를 사용하고, 포인터를 통해 구현됨