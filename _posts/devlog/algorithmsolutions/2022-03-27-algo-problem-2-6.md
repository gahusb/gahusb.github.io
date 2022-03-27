---
layout: post
title: Algorithm 강의 02-06.뒤집은 소수
image: 
subtitle: 02-06.뒤집은 소수
category: devlog
tags: algorithmsolutions
accent_image: 
  background: url('/assets/img/blog/jj-ying.jpg') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  알고리즘 강의 문제 풀이
invert_sidebar: true
---

# 02-06.뒤집은 소수
알고리즘 강의를 듣고 푸는 중에 기록할 만한 내용을 기록해 봅니다.

---

## 1. 문제
<details>
<summary>문제 보기</summary>
<div markdown="1">

N개의 자연수가 입력되면 각 자연수를 뒤집은 후 그 뒤집은 수가 소수이면 그 소수를 출력하 는 프로그램을 작성하세요. 예를 들어 32를 뒤집으면 23이고, 23은 소수이다. 그러면 23을 출 력한다. 단 910를 뒤집으면 19로 숫자화 해야 한다. 첫 자리부터의 연속된 0은 무시한다.

### 입력(Input)

첫 줄에 자연수의 개수 N(3<=N<=100)이 주어지고, 그 다음 줄에 N개의 자연수가 주어진다. 각 자연수의 크기는 100,000를 넘지 않는다.

### 출력(Output)

첫 줄에 뒤집은 소수를 출력합니다. 출력순서는 입력된 순서대로 출력합니다.

</div>
</details>

---

## 2. 문제 풀이

```java
import java.util.ArrayList;
import java.util.Scanner;

class problem_02_06 {
    public boolean isPrime(int num) {
        if(num == 1) return false;
        for(int i = 2; i < num; i++) {
            if(num % i == 0) return false;
        }
        return true;
    }

    public ArrayList<Integer> solution(int N, int[] arr) {
        ArrayList<Integer> list = new ArrayList<>();
        
        for(int i = 0; i < N; i++) {
            int tmp = arr[i];
            int res = 0;
            while(tmp > 0) {
                int t = tmp % 10;
                res = res * 10 + t;
                tmp = tmp / 10;
            }

            if(isPrime(res)) list.add(res);
        }

        return list;
    }
    public static void main(String[] args) {
        problem_02_06 mc = new problem_02_06();
        Scanner sc = new Scanner(System.in);
        int N = sc.nextInt();
        int[] arr = new int[N];
        for(int i = 0; i < N; i++) {
            arr[i] = sc.nextInt();
        }
        for(int x : mc.solution(N, arr)) {
            System.out.print(x + " ");
        }
    }
}

```