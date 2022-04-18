---
layout: post
title: Programmers. 1835.단체사진 찍기
image: 
subtitle: 1835.단체사진 찍기 - 카카오
category: devlog
tags: algorithmsolutions
accent_image: 
  background: url('/assets/img/blog/jj-ying.jpg') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
    알고리즘 문제 풀이
invert_sidebar: true
comments: true
---

# [Programmers] 1835.단체사진 찍기
해당 문제는 순열로 카카오 친구들의 순서를 나열하고 조건을 걸어 풀어보았습니다.

링크 :
[![image](https://velog.velcdn.com/images/pyh8618/post/ad4f3b17-fb2b-4290-b7e1-8b953f554a6d/programmers.jpg)](https://programmers.co.kr/learn/courses/30/lessons/1835)

---
## 1. 문제
<details>
<summary>문제 보기</summary>
<div markdown="1">

![image](https://t1.kakaocdn.net/codefestival/picture.png)

가을을 맞아 카카오프렌즈는 단체로 소풍을 떠났다. 즐거운 시간을 보내고 마지막에 단체사진을 찍기 위해 카메라 앞에 일렬로 나란히 섰다. 그런데 각자가 원하는 배치가 모두 달라 어떤 순서로 설지 정하는데 시간이 오래 걸렸다. 네오는 프로도와 나란히 서기를 원했고, 튜브가 뿜은 불을 맞은 적이 있던 라이언은 튜브에게서 적어도 세 칸 이상 떨어져서 서기를 원했다. 사진을 찍고 나서 돌아오는 길에, 무지는 모두가 원하는 조건을 만족하면서도 다르게 서는 방법이 있지 않았을까 생각해보게 되었다. 각 프렌즈가 원하는 조건을 입력으로 받았을 때 모든 조건을 만족할 수 있도록 서는 경우의 수를 계산하는 프로그램을 작성해보자.


### 입력형식

입력은 조건의 개수를 나타내는 정수 n과 n개의 원소로 구성된 문자열 배열 data로 주어진다. data의 원소는 각 프렌즈가 원하는 조건이 N~F=0과 같은 형태의 문자열로 구성되어 있다. 제한조건은 아래와 같다.

 - 1 <= n <= 100
 - data의 원소는 다섯 글자로 구성된 문자열이다. 각 원소의 조건은 다음과 같다.
   - 첫 번째 글자와 세 번째 글자는 다음 8개 중 하나이다. {A, C, F, J, M, N, R, T} 각각 어피치, 콘, 프로도, 제이지, 무지, 네오, 라이언, 튜브를 의미한다. 첫 번째 글자는 조건을 제시한 프렌즈, 세 번째 글자는 상대방이다. 첫 번째 글자와 세 번째 글자는 항상 다르다.
   - 두 번째 글자는 항상 ~이다.
   - 네 번째 글자는 다음 3개 중 하나이다. {=, <, >} 각각 같음, 미만, 초과를 의미한다.
   - 다섯 번째 글자는 0 이상 6 이하의 정수의 문자형이며, 조건에 제시되는 간격을 의미한다. 이때 간격은 두 프렌즈 사이에 있는 다른 프렌즈의 수이다.


### 출력(Output)

모든 조건을 만족하는 경우의 수를 리턴한다.

### 예제입출력

| n	 | data	| answer |
| -- | ---- | ------ |
| 2	| |["N~F=0", "R~T>2"] | 3648 |
| 2	| ["M~C<2", "C~M>1"] | 0 |

### 예제에대한 설명

첫 번째 예제는 문제에 설명된 바와 같이, 네오는 프로도와의 간격이 0이기를 원하고 라이언은 튜브와의 간격이 2보다 크기를 원하는 상황이다.

두 번째 예제는 무지가 콘과의 간격이 2보다 작기를 원하고, 반대로 콘은 무지와의 간격이 1보다 크기를 원하는 상황이다. 이는 동시에 만족할 수 없는 조건이므로 경우의 수는 0이다.

</div>
</details>

---

## 2. 문제 풀이

카카오 친구들의 일렬로 나열되는 순열의 경우를 우선적으로 구해보았다.

순열 방식을 이용해서 우선적으로 카카오 친구들의 순서를 나열한다.

<sub>n</sub>P<sub>r</sub>을 아래와 같은 방식으로 정의할 수 있다.

```java
public void permutation(char[] arr, int depth, int n, int r) {
    if(depth == r) {
        print(arr);
        return;
    }

    for(int i = depth; i < n; i++) {
        swap(arr, depth, i);
        permutation(arr, depth + 1, n, r);
        swap(arr, depth, i);
    }
}
```

이렇게 모든 카카오친구들의 순열을 구했다면, 입력된 data 배열의 방식대로 조건에 따라서 카운팅을 할 것인지 그냥 return 할 것인지를 구해준다.

이때, 주의하여야 할 것은 두 친구 사이의 조건을 나타낸 것 이므로 사이 숫자는 `+1`을 시켜주어서 비교해야 한다.


그럼 이것을 활용하여 코드로 표현해보자.

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class GroupPhoto_level2 {
    // 어피치, 콘, 프로도, 제이지, 무지, 네오, 라이언, 튜브
    static char[] kakaoFriends = {'A', 'C', 'F', 'J', 'M', 'N', 'R', 'T'};
    static int totalcnt;

    public static int findFriend(char[] kakao, char pick) {
        for(int i = 0; i < kakao.length; i++) {
            if(kakao[i] == pick) return i;
        }
        return -1;
    } 

    public static void checkOrder(char[] kakao, String[] data) {
        int len = data.length;
        for(int i = 0; i < len; i++) {
            char[] tmp = data[i].toCharArray();
            int first = findFriend(kakao, tmp[0]);
            int second = findFriend(kakao, tmp[2]);
            int gap = tmp[4] - '0';
            if(tmp[3] == '=') {
                if(Math.abs(first - second) != gap + 1) return;
            } else if(tmp[3] == '<') {
                if(Math.abs(first - second) >= gap + 1) return;
            } else if(tmp[3] == '>') {
                if(Math.abs(first - second) <= gap + 1) return;
            }
        }

        totalcnt++;
    }

    public static void swap(char[] data, int depth, int i) {
        char tmp = data[depth];
        data[depth] = data[i];
        data[i] = tmp;
    }

    public static void permutation(char[] arr, int depth, int n, int r, String[] data) {
        if(depth == r) {
            checkOrder(arr, data);
            return;
        }

        for(int i = depth; i < n; i++) {
            swap(arr, depth, i);
            permutation(arr, depth + 1, n, r, data);
            swap(arr, depth, i);
        }
    }
    public static int solution(int n, String[] data) {
        totalcnt = 0;
        permutation(kakaoFriends, 0, 8, 8, data);
        return totalcnt;
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken());
        String[] data = new String[N];
        st = new StringTokenizer(br.readLine());
        for(int i = 0; i < N; i++) {
            data[i] = st.nextToken();
        }

        System.out.println(solution(N, data));
    }
}
```