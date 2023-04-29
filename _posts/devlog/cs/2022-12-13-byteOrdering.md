---
layout: post
title: 바이트오더링 정리
image:
subtitle: 바이트오더링을 이해하고 정리
category: devlog
tags: cs
accent_image: 
  background: url('/assets/img/web_dev/webdev_sidebar.jpeg') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  바이트 오더링에 대해 자세히 알아보자.
invert_sidebar: true
sitemap: false
comments: true
---

# 바이트오더링(Byte Ordering)

* toc
{:toc .large-only}

## 바이트 오더링 이란?
바이트 오더링(Byte Ordering)은 데이터가 바이트(Byte) 단위로 메모리에 저장되는 순서를 말한다. <br />

크게 **Big-Endian** 방식과 **Little-Endian** 방식이 존재하며 각 CPU 벤더에 의존적인 특징을 가지고 있다. <br />

2 바이트 이상의 크기를 가진 자료형을 저장할 때부터 차이가 난다. <br />

```
BYTE    b       =   0x12;

WORD    w       =   0x1234;

DWORD   dw      =   0x12345678;

char    str[]   =   "abcde"; 

```

| Type | Name | Size | Big-Endian | Little-Endian |
| --- | --- | --- | ----- | ----- |
| BYTE | b | 1 | [12] | [12] |
| WORD | w | 2 | [12][34] | [34][12] |
| DWORD | dw | 4 | [12][34][56][78] | [78][56][34][12] |
| char[] | str | 6 | [61][62][63][64][65][00] | [61][62][63][64][65][00] |

## Big-Endian 방식
> 상위 바이트의 값이 메모리에 먼저 표시되는 방법 (번지수가 작은 위치)

사용되는 계열 : 대형 UNIX 서버에 사용되는 RISC 계열의 CPU

![Bit-Endian](https://1.bp.blogspot.com/-ytOXeypzCPQ/X2C2qWsL-AI/AAAAAAAADhQ/Nx4996U3jCIYZUA9UcNLaKA9iuLL0LjKACLcBGAsYHQ/s16000/Big-Endian%2B%25EB%25B0%25A9%25EC%258B%259D.png)

네트워크 데이터 통신에서는 네트워크 바이트 순서(Network Byte Order, 즉 Big-Endian)를 따르도록 데이터의 바이트 순서를 변경해야 한다. <br />
(TCP/IP, XNS, SNA 규약은 16비트와 32비트 정수에서 Big-Endian 방식을 사용함) <br />

따라서 시스템이 Little-Endian 방식을 사용할 경우 네트워크를 통해 데이터를 전송하기 전에 Big-Endian 방식으로 데이터를 변경해서 보내야만 하고 받을 때는 Little-Endian 시스템은 전송되어 오는 데이터를 역순으로 조합해야 함.

## Little-Endian 방식
> 하위 바이트의 값이 메모리상에 먼저 표시되는 방법 (번지수가 작은 위치)

사용되는 계열 : Intel 계열의 CPU

![Little-Endian](https://1.bp.blogspot.com/-fPp44c-lT9I/X2C2yA917RI/AAAAAAAADhU/zWrWf6SDTM0fG791PzYT9iBlNkiEPEB-ACLcBGAsYHQ/s0/Little-Endian%2B%25EB%25B0%25A9%25EC%258B%259D.png)

## 두 Endian 방식의 장단점
Big-Endian 방식은 소프트웨어의 디버그를 편하게 해 주는 경향이 있다. <br />
사람이 숫자를 읽고 쓰는 방법과 같기 때문에 디버깅 과정에서 메모리의 값을 보기 편하다. <br />

예를 들어 0x59654148 은 빅 엔디언으로 59 65 41 48 로 표현한다. <br />

반대로 Little-Endian 방식 은 메모리에 저장된 값의 하위 바이트들만 사용할 때 별도의 계산이 필요 없다는 장점이 있다. <br />

예를 들어 32비트 숫자인 0x2A 는 Little-Endian 방식으로 표현하면 2A 00 00 00 이 되는데 이 표현에서 앞의 두 바이트 또는 한 바이트만 떼어 내면 하위 16 비트 또는 8 비트를 바로 얻을 수 있다. <br />

반면 32bit Big-Endian 방식 환경에서는 하위 16 비트나 8 비트 값을 얻기 위해서는 변수 주소에 2바이트 또는 3바이트를 더해야 한다. <br />

보통 변수의 첫 바이트를 그 변수의 주소로 삼기 때문에 이런 성질은 종종 프로그래밍을 편하게 하는 반면 Little-Endian 방식 환경의 프로그래머가 Big-Endian 방식 환경에서 종종 실수를 일으키는 한 이유이기도 하다. <br />

또한, 가산기가 덧셈을 하는 과정은 LSB 로부터 시작하여 자리 올림을 계산해야 하므로 첫 번째 바이트가 LSB 인 Little-Endian 방식에서는 가산기 설계가 조금 더 단순해진다. <br />

Big-Endian 방식에서는 가산기가 덧셈을 할때 마지막 바이트로부터 시작하여 첫 번째 바이트까지 역방향으로 진행해야 한다. <br />

그러나 오늘날의 프로세서는 여러개의 바이트를 동시에 읽어들여 동시에 덧셈을 수행하는 구조를 갖고 있어 두 Endian 방식 사이에 사실상 차이가 없다. <br />

[참고 사이트] : 
https://ko.wikipedia.org/wiki/%EC%97%94%EB%94%94%EC%96%B8