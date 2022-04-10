---
layout: post
title: Dart 언어
image:
subtitle: flutter에서 사용되는 dart에 대해 알아보기
category: devlog
tags: flutter
accent_image: 
  background: url('/assets/img/algo_img/coding_sidebar.jpg') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  Flutter를 개발하기 위한 구글 Dart 언어에 대해서 알아보자
invert_sidebar: true
sitemap: false
comments: false
---

# 플러터를 위한 필수! 다트를 알자!

* toc
{:toc .large-only}

플러터는 다트(Dart)라는 프로그래밍 언어로 개발됐습니다. 따라서 플러터를 사용하려면 다트라는 새로운 언어를 알아야 합니다.   
다트 홈페이지에는 `"자바나 C# 개발자라면 하루면 배울 수 있다."` 라고 소개하는 것처럼 다트는 생각보다 어렵지 않은 언어입니다.   

다트 언어의 철학은 "간단하게 배워서 다양한 플랫폼에 써먹자!" 입니다. 지금 다트를 익혀 두면 모바일 앱뿐만 아니라 서버와 웹 프런트엔드, 데스크톱 앱을 만들 수도 있습니다.   
다만, 플러터가 주제이므로 다트를 전문적으로 배우지는 않고 핵심 내용만 간략하게 다룰 예정입니다.

## 다트(Dart)!
다트는 구글이 Web Front-end 구현을 목적으로 개발한 프로그래밍 언어로 2011년 10월에 공개되었습니다.   
다트는 마치 카멜레온처럼 어떻게 활용하느냐에 따라 서버나 웹, 앱을 만들 때 사용할 수 있습니다.

### 다트 언어의 9가지 특징
다트는 다른 언어와 비교해 9가지 두드러진 특징이 있습니다.

1. 다트는 main() 함수로 시작합니다.
2. 다트는 어디에서나 변수를 선언하고 사용할 수 있습니다.
3. 다트에서는 모든 변수가 객체입니다. 그리고 모든 객체는 Object 클래스를 상속받습니다.
4. 다트는 자료형이 엄격한 언어입니다. 이 말은 변수에 지정한 자료형과 다른 유형의 값을 저장하면 오류가 발생한다는 의미입니다. 만약 여러 자료형을 허용하려면 dynamic 타입을 이용할 수 있습니다.
5. 다트는 제네릭 타입을 이용해 개발할 수 있습니다. 그리고 `List<int>`처럼 int형을 넣을 수도 있고, `List<dynamic>`처럼 다양한 데이터를 넣을 수도 있습니다.
6. 다트는 public, protected 같은 키워드가 없습니다. 만약 외부로 노출하고 싶지 않다면 변수나 함수 이름 앞에 언더스코어(_)를 이용해 표시할 수 있습니다.
7. 변수나 함수의 시작은 언더스코어 또는 문자열로 시작하고 그 이후에 숫자를 입력할 수 있습니다.
8. 다트는 삼항 연산자를 사용할 수 있습니다.
9. Null safety를 지원합니다. 이는 2.0에서 새롭게 추가된 기능으로, Null safety를 이용하면 컴파일 전에 널 예외(Null Exception)를 알 수 있으므로 널에 대한 오류가 발생하지 않도록 작업할 수 있습니다.

#### 8) 삼항연산자   
다음 코드에서 첫 번째 줄을 보면 isPublic이 참이면 "public", 참이 아니면 "private"를 반환하여 visibility에 지정합니다. 두 번째 줄은 매개변수로 전달받은 name이 null이면 "Guest"를 반환하고, 아니면 매개변수로 전달 받은 값을 그대로 반환합니다.
```dart
var visibility = isPublic ? 'public' : 'private';
String playerName(String name) => name ?? 'Guest';
```

### 간단한 코드로 다트의 특징 이해하기
다트로 만든 프로그램의 시작점은 자바나 C처럼 **main()** 함수입니다.   

```dart
// 함수 정의
printInteger(int aNumber) {
    print('The number is $aNumber.'); // 콘솔에 출력
}

// main() 함수에서 시작
main() {
    var number = 42;        // 동적 타입 변수 지정
    printInteger(number);   // 함수 호출
}
```
```console
결과 >
The number is 42.
```
> `var 키워드로 변수를 선언하면 해당 변수에 저장되는 값의 유형에 따라 자료형이 정해집니다. 이것을 자료형 추론(type inference)이라고 합니다.`

다트에서는 문자열을 표현할 때는 큰따옴표나 작은따옴표를 이용하는데, 이때 따옴표 안에 **`${표현식}`**과 같은 형태로 사용하면 표현식에 변수를 직접 넣을 수 있습니다.

| 구분 | 자료형 | 설명 |
|:---:|:----:|:-----:|
| 숫자 | int | 정수형 숫자 |
|     | double | 실수형 숫자 |
|     | num | 정수형 또는 실수형 숫자 |
|문자열| String | 텍스트 기반 문자 |
|논리형| bool | True나 false |
| 자료형 추론 | var | 입력받은 값에 따라 자료형 결정. 한 번 결정된 자료형은 변경 불가 |
|          | dynamic | 입력받은 값에따라 자료형 결정. 다른 변수 입력하면 자료형 변경 가능 |


### Null safety
Null safety를 사용하려면 pubspec.yaml 파일의 SDK 환경을 변경해야 합니다. 다음과 같이 2.12.0 버전 이상부터 Null safety를 지원합니다.   
```yaml
enviroment:
    sdk: ">=2.12.0 <3.0.0"
```
> [버전 표기 이해](https://gahusb.github.io/devlog/web-npmcarrot.html)

변수를 선언할 때 사용하는 것으로, 자료형 다음에 `?`를 붙이면 Null이 가능하고 붙이지 않으면 Null이 불가능합니다. 그리고 식 다음에 `!`를 붙이면 Null이 아님을 직접 표시합니다.

```dart
int? couldReturnNullButDoesnt() => -3;

void main() {
    int? couldBeNullButIsnt = 1;            // null로 변경 가능
    List<int?> listThatCouldHoldNulls = [2, null, 4];   // List의 int에 null 값 포함 가능
    List<int>? nullsList;                   // List 자체가 null일 수 있음
    int a = couldBeNullButIsnt;             // null을 넣으면 오류
    int b = listThatCouldHoldNulls.first;   // int b는 ?가 없으므로 오류
    int b = listThatCouldHoldNulls.first!;  // null이 아님을 직접 표시
    int c = couldReturnNullButDoesnt().abs();   // null일 수도 있으므로 abs()에서 오류
    int c = couldReturnNullButDoesnt()!.abs();  // null이 아님을 직접 표시

    print('a is $a.');
    print('b is $b.');
    print('c is $c.');
}
```

Null safety를 사용하는 이유는 프로그램 실행 중 널 예외가 발생하면 프로그램이 중지되는데, 이를 코드 단계에서 구분하여 작성할 수 있도록 하기 위해서입니다. 이런 이유로 요즘 나오는 언어 중에는 Null safety를 제공하는 언어가 많습니다.

### 다트가 제공하는 키워드
| 키워드 | - | - | - |
| :--: |:--:|:--:|:--:|
| abstract | dynamic | implements | show |
| as | else | import | static |
| assert | enum | in | super |
| async | export | interface | switch |
| await | extends | is | sync |
| break | external | library | this |
| case | factory | mixin | throw |
| catch | false | new | true |
| class | final | null | try |
| const | finally | on | typedef |
| continue | for | operator | var |
| convariant | Function | part | void |
| default | get | rethrow | while |
| deferred | hide | return | with |
| do | if | set | yield |

> [다트가 제공하는 키워드](https://dart.dev/guides/language/language-tour#keywords)

---
## 비동기 처리 방식
다트는 비동기 처리를 지원하는 언어입니다. 비동기(asynchronous)란 언제 끝날지 모르는 작업을 기다리지 않고 다음 작업을 처리하게 하는 것을 의미합니다. 만약 비동기를 지원하지 않고 동기(synchronous)로만 처리한다면 어떤 작업이 오래 걸릴 경우 사용자는 실행이 멈춘 것으로 생각하고 프로그램을 종료할 수 있습니다. 일반적으로 네트워크에서 데이터를 가져오거나 데이터베이스 쓰기, 파일 읽기 등의 작업은 상황에 따라 언제 끝날지 알 수 없으므로 비동기로 처리합니다.   


![synchronous](https://res.cloudinary.com/practicaldev/image/fetch/s--IB0Ikc71--/c_imagga_scale,f_auto,fl_progressive,h_900,q_auto,w_1600/https://cl.ly/3N0P302P0H2g/Image%25202018-07-19%2520at%25209.16.55%2520AM.png) <sup>[1](#footnote_1)</sup>
<a name="footnote_1">1</a>: 출처: DEV.to

### 비동기 프로세스의 작동 방식
다트는 async와 await 키워드를 이용해 비동기 처리를 구현합니다.   
1. 함수 이름 뒤, 본문이 시작하는 중괄호 `{` 앞에 async 키워드를 붙여 비동기로 만든다.
2. 비동기 함수 안에서 언제 끝날지 모르는 작업 앞에 await 키워드를 붙인다.
3. 2번 작업을 마친 결과를 받기 위해 비동기 함수 이름 앞에 Future(값이 여러 개면 Stream) 클래스를 지정한다.

다음 예시를 보겠습니다.

```dart
void main() {
    checkVersion();
    print('end process');
}
Future checkVersion() async {
    var version = await lookUpVersion();
    print(version);
}

int lookUpVersion() {
    return 12;
}
```

일반적인 생각으로 코드를 보면 main() 함수에서 제일 먼저 checkVersion() 함수를 호출했으므로 checkVersion() 함수에 있는 lookUpVersion() 함수가 호출되어 12를 전달받아 출력한 다음, 다시 main()으로 돌아와서 'end process'가 출력될 것 같습니다. 하지만 결과는 다음과 같습니다.   

```console
실행 결과 >
end process
12
```

이러한 결과가 나오는 이유는 먼저 checkVersion() 함수를 보면 이름 앞뒤로 Future와 async가 붙었습니다. 이렇게 하면 checkVersion() 함수를 비동기로 만들겠다는 의미입니다. 즉, checkVersion() 함수 안에 await가 붙은 함수를 비동기로 처리한 다음 그 결과는 Future 클래스에 저장해 둘 테니 먼저 checkVersion() 함수를 호출한 main() 함수의 나머지 코드를 모두 실행하라는 의미입니다. 그리고 main() 함수를 모두 실행했으면 그때 Future 클래스에 저장해 둔 결과를 이용해서 checkVersion() 함수의 나머지 코드를 실행합니다.

앞선 코드에서 lookUpVersion() 함수 앞에 await 키워드가 붙었습니다. await 키워드는 처리를 완료하고 결과를 반환할 때까지 이후 코드의 처리를 멈춥니다. 따라서 lookUpVersion() 함수를 호출해 version 변수에 12가 저장된 다음에야 비로소 print(version) 문으로 이를 출력합니다.   
이처럼 비동기 함수에서 어떤 결과값이 필요하다면 해당 코드를 await로 지정합니다. 그러면 네트워크 지연 등으로 제대로 된 값을 반환받지 못한 채 이후 과정이 실행되는 것을 방지할 수 있습니다.

이러한 비동기 처리를 이용하면 지연이 발생하는 동안 애플리케이션이 멈춰 있지 않고 다른 동작을 하게 할 수 있습니다.

### 비동기 함수가 반환하는 값 활용하기
비동기 함수가 반환하는 값을 처리하려면 then() 함수를 이용합니다.

```dart
void main() async {
    await getVersionName().then((value) => {
        print(value);
    });
    print('end process');
}

Future<String> getVersionName() async {
    var versionName = await lookUpVersionName();
    return versionName;
}

String lookUpVersionName() {
    return 'Android Q';
}
```
```console
실행 결과 >
Android Q
end process
```

코드를 보면 Future<String>이라는 반환값을 정해 놓은 getVersionName() 이라는 함수가 있습니다. 이 함수는 async 키워드가 붙었으므로 비동기 함수입니다. 이처럼 비동기 함수가 데이터를 성공적으로 반환하면 호출하는 쪽에서 then() 함수를 이용해 처리할 수 있습니다.   

then() 이외에 error() 함수도 이용할 수 있습니다. error() 함수는 실행 과정에서 오류가 발생했을 때 호출되므로 이를 이용해 예외를 처리할 수 있습니다.

### 다트와 스레드
다트는 **하나의 스레드(thread)로 동작**하는 프로그래밍 언어입니다. 그래서 await 키워드를 잘 사용해야 합니다.

```dart
void main() {
    printOne();
    printTwo();
    printThree();
}

void printOne() {
    print('One');
}

void printThree() {
    print('Three');
}

void printTwo async {
    Future.delayed(Duration(seconds: 1), () {
        print('Future!!');
    });
    print('Two');
}
```
```console
실행 결과 >
One
Two
Three
Future!!
```
'One' 출력 이후에 printTwo() 함수에 진입하면 Future를 1초 지연했으므로 async로 정의한 비동기 함수의 특징에 대해 'Two'가 먼저 출력됩니다. 그리고 'Three'를 출력하고 'Future!!'가 가장 늦게 출력됩니다.

이때, printTwo() 함수를 다음과 같이 수정한다면?
```dart
void printTwo() async {
    await Future.delayed(Duration(seconds: 2), () {
        print('Future Method');
    });
    print('Two');
}
```
Future.delayed() 코드 앞에 await 키워드를 붙였으므로 이후 코드의 실행이 멈춥니다.   
따라서 printTwo() 함수를 벗어나 main() 함수의 나머지 코드를 모두 실행하고, 그 다음에 await가 붙은 코드부터 차례대로 실행합니다.
```console
실행 결과 >
One
Three
Future Method
Two
```
이처럼 await 키워드를 이용하면 await가 속한 함수를 호출한 쪽의 프로세스가 끝날 때까지 기다리기 때문에 이를 잘 고려해서 프로그램을 작성해야 합니다.

---

## JSON 데이터 주고 받기
애플리케이션을 개발하다 보면 서버와의 통신이 중요하다는 것을 알게 됩니다. 대부분 앱은 서버와 데이터를 주고 받으며 상호 작용하고 화면에 필요한 데이터를 출력합니다. 이러한 데이터를 교환할 때 가장 많이 쓰는 형식이 **JSON**입니다.   
직접 문자열 형태나 XML을 이용해 데이터를 주고 받기도 하지만, 가장 편리하면서 파일 크기도 작은 JSON 형식을 주로 이용합니다. 다트에서는 이러한 JSON 통신을 간편하게 이용할 수 있습니다.   
JSON을 사용하려면 소스에 convert라는 라이브러리를 포함해야 합니다.
```dart
import 'dart:conver';

void main() {
    var jsonString = '''
        [
            {"score": 40},
            {"score": 80}
        ]
    ''';
    var scores = jsonDecode(jsonString);
    print(scores is List);              // true
    var firstScore = scores[0];
    print(firstScore is Map);           // true
    print(firstScore['score'] == 40);   // true
}
```
위 코드에서 jsonString 변수에 저장된 데이터가 JSON을 형태의 문자열입니다. 이 데이터를 convert 라이브러리에 있는 jsonDecode() 함수에 전달한 후 그 결과를 scores 변수에 저장했습니다. jsonDecode() 함수는 JSON 형태의 데이터를 dynamic 형식의 리스트로 변환해서 반환해 줍니다.   
scores 변수가 리스트인지는 True/False로 점검할 수 있습니다.

그리고 scores 리스트에서 첫 번째 값을 firstScore에 저장합니다. 이 값은 키(key)와 값(values)이 있는 Map 형태입니다. print(firstScore['score'] == 40) 코드는 firstScore 데이터의 score 키에 해당하는 값이 40이라는 것을 나타냅니다. 이처럼 jsonDecode() 함수를 이용하면 서버에서 JSON 데이터를 받아서 사용할 수 있습니다.

이제 애플리케이션에서 서버로 데이터를 보내는 예도 살펴보겠습니다. 이때는 jsonEncode() 함수를 이용해 JSON 형태로 변환한 데이터를 서버로 보낼 수 있습니다.

```dart
import 'dart:convert';

void main() {
    var scores = [
        {'score': 40},
        {'score': 80},
        {'score': 100, 'overtime': true, 'special_guest': null},
    ];

    var jsonText = jsonEncode(scores);
    print(jsonText == 
        '[{"score": 40},{"score": 80},'
        '{"score": 100, "overtime": true,'
        '"special_guest": null}]');     // true 출력
}
```

scores 데이터는 배열로 이루어졌고 각 항목은 score값으로 구성되며 마지막 항목에는 overtime과 special_guest값을 추가했습니다.   
앞의 코드에서는 {"score": 40}처럼 키에 큰따옴표를 사용해 JSON 데이터임을 표시했고, 지금 코드는 {'score': 40}처럼 작은따옴표를 이용해 변수임을 표시했습니다.

이 scores 데이터를 인자로 jsonEncode() 함수를 호출하면 키값이 큰따옴표로 묶이고 전체 데이터를 작은따옴표로 한 번 묶어서 JSON 형태의 데이터가 됩니다.   
이처럼 다트는 간단하게 JSON을 만들고 파싱하여 데이터를 주고받는 기능을 제공합니다.

---

## 스트림 통신하기
애플리케이션을 개발하다 보면 데이터를 순서대로 주고받아야 할 때가 있습니다. 데이터를 순서대로 주고받을 것으로 생각해서 화면을 구성했는데 네트워크나 와이파이 연결이 끊기거나 특정 API 호출이 늦어져 순서가 달라지면 애플리케이션이 원하는 흐름대로 작동하지 않을 수도 있습니다.

이처럼 순서를 보장받고 싶을 때 스트림(stream)을 이용합니다.   
스트림은 처음에 넣은 데이터가 꺼낼 때도 가장 먼저 나오는 데이터 구조로 생각할 수 있습니다. 따라서 스트림을 이용하면 데이터를 차례대로 주고받는 코드를 작성할 수 있습니다.

```dart
import 'dart:async';

Future<int> sumStream(Stream<int> stream) async {
    var sum = 0;
    await for (var value in stream) {
        print('sumStream : $value');
        sum += value;
    }
    return sum;
}

Stream<int> countStream(int to) async* {
    for (int i = 0; i <= to; i++) {
        print('countStream : $i');
        yield i;
    }
}

main() async {
    var stream = countStream(10);
    var sum = await sumStream(stream);
    print(sum);     // 55
}
```

main() 함수를 살펴보면 먼저 countStream(10) 함수를 호출합니다. 이 함수는 async*dhk yield 키워드를 이용해 비동기 함수로 만들었습니다. 이 함수는 for 문을 이용해 1부터 int형 매개변수 to로 전달받은 숫자까지 반복합니다.

async* 명령어는 앞으로 yield를 이용해 지속적으로 데이터를 전달하겠다는 의미입니다. 위 코드에서 yield는 int형 i를 반환하는데, return은 한 번 반환하면 함수가 끝나지만 yield는 반환 후에도 계속 함수를 유지합니다.

이렇게 받은 yield값을 인자ㅗ sumStream() 함수를 호출하면 이 값이 전달될 때마다 sum 변수에 누적해서 반환해 줍니다. 그리고 main() 함수에서 이 값을 받아서 출력하면 55가 나옵니다.   
출력 결과를 보면 함수가 어떤 흐름으로 진행되는지 알 수 있을겁니다.   

```console
실행 결과 >
countStream : 1
sumStream : 1
countStream : 2
sumStream : 2
...
countcountStream : 9
sumStream : 9
countStream : 10
sumStream : 10
55
```
이처럼 스트림을 이용하면 데이터를 차례대로 받아서 처리할 수 있습니다.

아니면 then() 함수를 이용해 스트림 코드를 작성할 수도 있습니다.
```dart
main() {
    var stream = Stream.fromIterable([1, 2, 3, 4, 5]);

    // 가장 앞의 데이터 결과: 1
    stream.first.then((value) => print('first: $value'));
    // 가장 마지막 데이터의 결과 : 5
    stream.last.then((value) => print('last: $value'));
    // 현재 스트림이 비어 있는지 확인: false
    stream.isEmpty.then((value) => print('isEmpty: $value'));
    // 전체 길이: 5
    stream.length.then((value) => print('length: $value'));
}
```
코드를 보면 Stream클래스를 이용해 배열을 하나 만든 후 함수를 이용해서 값을 가져옵니다.   
그런 다음 then() 함수로 가져다 사용합니다.   
그런데 이 코드는 그대로 실행하면 오류가 발생합니다. 일단 스트림을 통해 데이터를 사용하면 데이터는 사라지기 때문입니다. 따라서 다음처럼 한 번만 실행하도록 변경해야 합니다.

```dart
main() {
    var stream = Stream.fromIterable([1, 2, 3, 4, 5]);

    // 가장 마지막 데이터의 결과: 5
    stream.last.then((value) => print('last: $value'));
}
```

스트림은 실시간으로 서버를 살펴보다가 서버에서 데이터가 변경되면 화면을 새로 고침하지 않더라도 자동으로 변경된 데이터가 반영되어야 할 때 사용할 수 있는 유용한 클래스입니다.

---

## 다트 프로그램 만들기
### 1. 구구단 프로그램 작성하기
```dart
void main() {
    for(int i = 0; i <= 9; i++) {
        for(int j = 0; j <=9; j++) {
            print('$i * $j = ${i * j}');
        }
    }
}
```

### 2. 자동차 클래스 구현하기
다음과 같은 속성을 포함하는 클래스를 만들어 봅니다.
| 이름 | 자료형 | 의미 |
|:---:|:----:|:---:|
|maxSpeed|int|최고 속도|
|price|num|가격|
|name|String|이름|

Car 클래스 안에 saleCar()라는 이름으로 함수를 작성합니다. 이 함수는 자동차 가격을 10% 할인해서 반환합니다. 그리고 main() 함수에서 Car 클래스를 이용해 다음과 같은 속성값으로 3종류의 자동차를 선언합니다. 새로운 객체를 선언할 때 자바에선는 new라는 키워드를 사용하지만 다트에서는 생략할 수 있습니다. 물론 사용해도 무방합니다.

|maxSpeed|price|name|
|:----:|:----:|:----:|
|320|100000|BMW|
|250|70000|BENZ|
|200|80000|FORD|

BMW를 3번 할인하는 함수를 호출한 뒤에 차량 가격을 출력해봅니다.

```dart
void main() {
    Car bmw = Car(320, 100000, BMW);
    Car benz = Car(250, 70000, BENZ);
    Car ford = Car(200, 80000, FORD);

    bmw.saleCar();
    bmw.saleCar();
    bmw.saleCar();
    print(bmw.price);
}

class Car {
    int maxSpeed;
    num price;
    String name;

    Car(int maxSpeed, num price, String name) {
        this.maxSpeed = maxSpeed;
        this.price = price;
        this.name = name;
    }

    num saleCar() {
        price = price * 0.9;
        return price;
    }
}
```

### 3. 로또 번호 생성기
무작위 수를 생성하는 랜덤 함수를 이용하려면 dart:math 라이브러리를 사용해야 합니다.   
<code>import 'dart:math' as math;</code>   
as math 코드는 import한 dart:math 라이브러리를 math라는 이름으로 사용하겠다는 의미입니다. 이 math를 이용해 6개의 무작위 수를 만드는 로또 번호 생성기를 만들어 봅시다. 만약, 생성한 번호가 중복일 경우에는 다시 생성합니다.

```dart
import 'dart:collection';
import 'dart:math' as math;

void main() {
    var rand = math.Random();
    HashSet<int> lotteryNumber  HashSet();

    while(lotteryNumber.length < 6) {
        lotteyNumber.add(rand.nextInt(45) + 1);
    }
    print(lotteryNumber);
}
```
```console
실행 결과 >
{2, 9, 13, 24, 33, 42}
```

HashSet를 사용하려면 dart:collection이라는 라이브러리를 import해야 합니다.