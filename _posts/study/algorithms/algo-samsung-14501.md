---
layout: post
title: Study-Algorithms
image: /assets/img/blog/jj-ying.jpg
subtitle: 14501.퇴사 - 삼성기출문제
tags: Algorithm, BOJ, JAVA, 삼성SW기출문제
accent_image: 
  background: url('/assets/img/blog/jj-ying.jpg') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  Version 9.1 provides minor design changes, new features, and closes multiple issues.
invert_sidebar: true
categories:
  - study
  - algorithms
---

# 14501. 퇴사 - 삼성기출문제 (from.백준알고리즘)

해당 문제는 DP(Dynamic Programing) 또는 완전탐색으로 풀 수 있는 문제입니다.

우선 문제를 DP를 이용해서 풀어보고, 그 다음 완전탐색을 이용해서 풀어 보았습니다.

---

### **14051\. 퇴사**

상담원으로 일하고 있는 백준이는 퇴사를 하려고 한다.

오늘부터 N+1일째 되는 날 퇴사를 하기 위해서, 남은 N일 동안 최대한 많은 상담을 하려고 한다.

백준이는 비서에게 최대한 많은 상담을 잡으라고 부탁을 했고, 비서는 하루에 하나씩 서로 다른 사람의 상담을 잡아놓았다.

각각의 상담은 상담을 완료하는데 걸리는 기간 Ti와 상담을 했을 때 받을 수 있는 금액 Pi로 이루어져 있다.

N = 7인 경우에 다음과 같은 상담 일정표를 보자.

|   | 1일 | 2일 | 3일 | 4일 | 5일 | 6일 | 7일 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Ti | 3 | 5 | 1 | 1 | 2 | 4 | 2 |
| Pi | 10 | 20 | 10 | 20 | 15 | 40 | 200 |

1일에 잡혀있는 상담은 총 3일이 걸리며, 상담했을 때 받을 수 있는 금액은 10이다. 5일에 잡혀있는 상담은 총 2일이 걸리며, 받을 수 있는 금액은 15이다.

상담을 하는데 필요한 기간은 1일보다 클 수 있기 때문에, 모든 상담을 할 수는 없다. 예를 들어서 1일에 상담을 하게 되면, 2일, 3일에 있는 상담은 할 수 없게 된다. 2일에 있는 상담을 하게 되면, 3, 4, 5, 6일에 잡혀있는 상담은 할 수 없다.

또한, N+1일째에는 회사에 없기 때문에, 6, 7일에 있는 상담을 할 수 없다.

퇴사 전에 할 수 있는 상담의 최대 이익은 1일, 4일, 5일에 있는 상담을 하는 것이며, 이때의 이익은 10+20+15=45이다.

상담을 적절히 했을 때, 백준이가 얻을 수 있는 최대 수익을 구하는 프로그램을 작성하시오.

**입력(Input)**

첫째 줄에 N (1 ≤ N ≤ 15)이 주어진다.

둘째 줄부터 N개의 줄에 Ti와 Pi가 공백으로 구분되어서 주어지며, 1일부터 N일까지 순서대로 주어진다. (1 ≤ Ti ≤ 5, 1 ≤ Pi ≤ 1,000)

**출력(Output)**

첫째 줄에 백준이가 얻을 수 있는 최대 이익을 출력한다.

---

## 1\. DP로 접근

 해당 문제는 첫째날-->마지막날로 접근하는 것보다, 마지막날-->첫째날로 접근하는 것이 더 쉽습니다.

7일에 잡혀있는 일은 2일이 걸리기 때문에 아무리 금액이 높더라도 진행할 수 없습니다.

6일에도 4일이 걸리기 때문에 N+1 즉, 7일이 넘어가기 때문에 진행할 수 없습니다.

5일에는 2일이 걸리는 일이 있습니다. 5~6일에 일을 하고 얻는 15의 수익이 얻을 수 있는 최대 수익입니다.

4일째에는 1일짜리 일을 추가로 하고, 5일까지의 일에 더해주는 것이 최대 수익이 됩니다. 즉, 20 + 15 = 35

3일째에도 1일짜리 일을 추가로 하고, 4일까지의 일에 더해주는 것이 최대 수익이 됩니다. 10 + 20 + 15 = 45

2일째에는 5일이 걸리는 일이 있습니다. 5일을 했을때 최대 이익과 위에서 했던 일중에서 비교를 해봐야 합니다.

2~6일까지 일을 해서 얻는 수익은 20입니다.

이것은 3~6까지 세가지 일을 해서 얻는 수익 45와 비교하면 낮습니다. 그러므로 2일째 일은 하지 않습니다.

1일째에도 마찬가지로 3일이 걸리는 일이 있습니다.

1~3일을 일을 하면 10의 수익을 얻을 수 있습니다.

이것은 3일째에 일을 하는 10의 수익과 같습니다. 그러므로 1일째에 일을 선택하던지 3일째의 일을 선택하던지 하면 최대 수익을 얻을 수 있습니다.

|   | 1일 | 2일 | 3일 | 4일 | 5일 | 6일 | 7일 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Ti | 3 | 5 | 1 | 1 | 2 | 4 | 2 |
| Pi | 10 | 20 | 10 | 20 | 15 | 40 | 200 |
| DP | 45 | 45 | 45 | 35 | 15 | \- | \- |

이러한 조건에서 위와 같이 최대 수익은 45가 나올 수 있습니다.

이것을 코드로 표현하겠습니다.

---

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

/**
 * 14501. 퇴사
 * @author gahusb
 * Input : N - 퇴사전까지 남은 기간, Ti - 상담을 완료하는데 걸리는 기간, Pi - 상담을 했을 때 받을 수 있는 금액
 * Output : 상담을 적절히 했을 때, 백준이가 얻을 수 있는 최대 수익
 *
 */
public class Resignation {
	static int[] t, p, dp;
	
	static void calcMaxIncome(int dDay) {
		int next;
		
		for (int i = dDay; i > 0; i--) {
	        next = i + t[i];
	        if (next > dDay + 1) {
	            dp[i] = dp[i + 1];
	        } else {
	            dp[i] = dp[i + 1] > dp[next] + p[i] ? dp[i + 1] : dp[next] + p[i];
	        }
	    }
		
		System.out.println(dp[1]);
	}
	
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		
		int N = Integer.parseInt(st.nextToken());
		t = new int[N+2];
		p = new int[N+2];
		dp = new int[N+2];
		for (int i = 1; i <= N; i++) {
			if (!st.hasMoreTokens()) st = new StringTokenizer(br.readLine());
			t[i] = Integer.parseInt(st.nextToken());
			p[i] = Integer.parseInt(st.nextToken());
		}
		
		calcMaxIncome(N);
	}
}
```

---

## 2\. 완전탐색

\-- 완전 탐색의 방법은 나중에 추후 업데이트 --

이것을 코드로 표현하겠습니다.

```java
public class Resignation {
	static int[] t, p;
	static int N, income;
	
	static void calcMaxIncome(int day, int cost) {
		if(day > N) {
			return;
		}
		
		income = income > cost ? income : cost;
		
		for (int i = day; i < N; i++) {
			calcMaxIncome(i + t[i], cost + p[i]);
		}
	}
	
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		
		N = Integer.parseInt(st.nextToken());
		t = new int[N];
		p = new int[N];
		for (int i = 0; i < N; i++) {
			if (!st.hasMoreTokens()) st = new StringTokenizer(br.readLine());
			t[i] = Integer.parseInt(st.nextToken());
			p[i] = Integer.parseInt(st.nextToken());
		}
		
		calcMaxIncome(0, 0);
		System.out.println(income);
	}
}
```