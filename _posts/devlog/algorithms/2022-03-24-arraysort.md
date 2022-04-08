---
layout: post
title: Array Sort
category: devlog
tags: algorithms
image: 
accent_image: 
  background: url('/assets/img/blog/jj-ying.jpg') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  정렬 방식 정리하기
invert_sidebar: true
comments: true
---

# 각 언어별 sort에 대한 정리

* toc
{:toc .large-only}

sort는 자주 사용하면서도 나중에 사용 할때마다 조금씩 헷갈리는 부분이라 정리해둘까 한다.

## JAVA
자바에 여러가지 방식이 있다.
- Comparator
- Comparable
- Lambda

### Comparator
Comparator 방식으로 내가 자주 사용하는 방식이다.
이 방식을 사용하면 개인적으로 내가 원하는 정렬 방식으로 변경하기도 간편하다고 생각하기 때문이다.

아래 코드를 보면

1차원 배열
```java
import java.util.Arrays;
import java.util.Comparator;

int N = 10;
int[] arr = new int[N];

Arrays.sort(arr, new Comparator<Integer>) {
    @Override
    public int compare(Object o1, Object o2) {
        return o1 - o2;     // 오름차순
    }
}
```

2차원 배열
```java
import java.util.Arrays;
import java.util.Comparator;

int N = 10;
int[][] arr = new int[N][N];

Arrays.sort(arr, new Comparator<int[]>) {
    @Override
    public int compare(int[] o1, int[] o2) {
        if(o1[0] == o2[0]) {
            return o1[1] - o2[1];   // 오름차순
        } else {
            return o1[0] - o2[0];
        }
    }
}
```

위 소스는 기본적으로 오름차순으로 되어 있고, 내림차순으로 정렬을 원한다면 반대로 리턴해준다.
```java
    return o2 - o1;     // 내림차순
```

기본적으로 리턴의 값이 음수이면 오름차순, 양수이면 내림차순으로 정렬이 된다고 생각하면 이해하기 쉽다.


### Comparable
Comparable 인터페이스에는 comepareTo() 추상메서드 하나만 존재한다.

주어진 객체보다 작으면 음수, 같으면0, 크면 양수를 리턴한다.

```java
public class NameCard implements Comparable<NameCard> {
    public String name;
    public int age;

    public NameCard(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public int compareTo(NameCard o) {
        if(this.age < o.age) {
            return -1;
        } else (this.age > o.age) {
            return 1;
        } else
            return 0;
    }
}

public class Main {
    public static void main(String[] args) {
        List<NameCard> list = new ArrayList<>();
        list.add(new NameCard("김자바", 9));
        list.add(new NameCard("박파이썬", 10));
        list.add(new NameCard("신씨", 1));
        list.add(new NameCard("양자스", 7));

        Collections.sort(list);

        for(NameCard n : list) {
            System.out.println(n.name + " " + n.age);
        }
    }
}
```

> Result : <br>
신씨 1 <br>
양자스 7 <br>
김자바 9 <br>
박파이썬 10 <br>


### Lambda
람다 방식으로도 간결하게 표현하여 정렬할 수 있다.

1차원 배열
```java
import java.util.Arrays;

int N = 10;
int[] arr = new int[N];

Arrays.sort(arrays, (o1, o2) -> o1 - o2);
```

2차원 배열
```java
import java.util.Arrays;

int N = 10;
int[] arr = new int[N];

Arrays.sort(arrays, (o1, o2) -> o1[0] == o2[0] ? o1[1] - o2[1] : o1[0] - o2[0]);
```

같은 방식이지만 사용하면서 더 편한 방법으로 사용하면 될 것 같다.

--------

## JavaScript
자바스크립트 방식은 최근에 웹 프론트 개발자로 이직하기 위해서 알고리즘을 하면서 사용해 보았다.

```javascript
let arr = [];

arr.sort([compareFunction]);
```
**파라미터** <br>
**compareFunction** <br>
> 정렬 순서를 정의하는 함수 <br>
*이 값이 생략되면, 배열의 element들은 문자열로 취급되어 유니코드 값 순서대로 정렬된다.* <br>
이 함수는 두 개의 배열 element를 파라미터로 입력 받는다. <br>
이 함수가 a, b 두개의 element를 파라미터로 입력받을 경우, <br>
이 함수가 리턴하는 값이 0보다 작을 경우,  a가 b보다 앞에 오도록 정렬하고, <br>
이 함수가 리턴하는 값이 0보다 클 경우, b가 a보다 앞에 오도록 정렬한다. <br>
만약 0을 리턴하면, a와 b의 순서를 변경하지 않는다. <br>


**리턴값** <br>
*compareFunction* 규칙에 따라서 정렬된 배열을 리턴한다.<br>
이때, 원본 배열인 arr가 정렬이 되고, 리턴하는 값 또한 원본 배열인 arr을 가리키고 있다.

```javascript
const arr1 = [2, 1, 3];
const arr2 = ['banana', 'apple', 'orange'];

arr1.sort();
document.writeln(arr1 + '<br>');

arr2.sort();
document.writeln(arr2 + '<br>');
```
> Result : <br>
1, 2, 3 <br>
apple,banana,orange <br>

```javascript
const arr = [2, 1, 3, 10];

arr.sort(function(a, b) {
    return a - b;
});
document.writeln(arr + '<br>');
```

> Result : <br>
1, 2, 3, 10

이 경우에도 a - b가 음수면 오름차순, 양수면 내림차순이 된다.

또한 객체로 구분하여 정렬하는 방법도 생각해 볼 수 있다.

```javascript
const arr = [
    {name: 'banana', price: 3000},
    {name: 'apple', price: 1000},
    {name: 'orange', price: 500}
];

arr.sort(function(a, b) {
    return a.price - b.price;
});
document.writeln(JSON.stringify(arr[0]) + '<br>');
```

> Result <br>
{name: 'orange', price: 500},
{name: 'apple', price: 1000},
{name: 'banana', price: 3000},


틀리거나 이상한 내용이 있다면 알려주세요!