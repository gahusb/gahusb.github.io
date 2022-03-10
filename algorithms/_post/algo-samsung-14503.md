---
layout: post
title: Study-Algorithms
image: /assets/img/blog/jj-ying.jpg
subtitle: 14503.로봇 청소기 - 삼성기출문제
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

# 14503. 로봇 청소기 - 삼성기출문제 (from.백준알고리즘)

해당 문제는 시뮬레이션 문제로 단계별로 수행하는 함수를 이용하여 풀 수 있는 문제입니다.

---

### 14503\. 로봇 청소기

로봇 청소기가 주어졌을 때, 청소하는 영역의 개수를 구하는 프로그램을 작성하시오.

로봇 청소기가 있는 장소는 N×M 크기의 직사각형으로 나타낼 수 있으며, 1×1크기의 정사각형 칸으로 나누어져 있다. 각각의 칸은 벽 또는 빈 칸이다. 청소기는 바라보는 방향이 있으며, 이 방향은 동, 서, 남, 북중 하나이다. 지도의 각 칸은 (r, c)로 나타낼 수 있고, r은 북쪽으로부터 떨어진 칸의 개수, c는 서쪽으로 부터 떨어진 칸의 개수이다.

로봇 청소기는 다음과 같이 작동한다.

1.  현재 위치를 청소한다.
2.  현재 위치에서 현재 방향을 기준으로 왼쪽방향부터 차례대로 탐색을 진행한다.
    1.  왼쪽 방향에 아직 청소하지 않은 공간이 존재한다면, 그 방향으로 회전한 다음 한 칸을 전진하고 1번부터 진행한다.
    2.  왼쪽 방향에 청소할 공간이 없다면, 그 방향으로 회전하고 2번으로 돌아간다.
    3.  네 방향 모두 청소가 이미 되어있거나 벽인 경우에는, 바라보는 방향을 유지한 채로 한 칸 후진을 하고 2번으로 돌아간다.
    4.  네 방향 모두 청소가 이미 되어있거나 벽이면서, 뒤쪽 방향이 벽이라 후진도 할 수 없는 경우에는 작동을 멈춘다.

로봇 청소기는 이미 청소되어있는 칸을 또 청소하지 않으며, 벽을 통과할 수 없다.

#### 입력(Input)

첫째 줄에 세로 크기 N과 가로 크기 M이 주어진다. (3 ≤ N, M ≤ 50)

둘째 줄에 로봇 청소기가 있는 칸의 좌표 (r, c)와 바라보는 방향 d가 주어진다. d가 0인 경우에는 북쪽을, 1인 경우에는 동쪽을, 2인 경우에는 남쪽을, 3인 경우에는 서쪽을 바라보고 있는 것이다.

셋째 줄부터 N개의 줄에 장소의 상태가 북쪽부터 남쪽 순서대로, 각 줄은 서쪽부터 동쪽 순서대로 주어진다. 빈 칸은 0, 벽은 1로 주어진다. 지도의 첫 행, 마지막 행, 첫 열, 마지막 열에 있는 모든 칸은 벽이다.

로봇 청소기가 있는 칸의 상태는 항상 빈 칸이다.

#### 출력(Output)

첫째 줄에 T초가 지난 후 구사과 방에 남아있는 미세먼지의 양을 출력한다.

---

### 1\. 문제 풀이

이 문제를 풀때에 생각해야 할 것은 두 가지 입니다.

1.  최초의 자리도 청소
2.  바라보고 있는 방향에 따라서 왼쪽의 좌표를 설정하는 것

최초에 자리를 청소한다고 하고 map에서 현 자리를 청소 한걸로 표시하고 스타트 합니다.

그리고 바라보고 있는 방향 왼쪽 좌표를 바라보기 때문에 배열에 현재 바라보고 있는 방향을 인덱스로 왼쪽을 보는 값을 고정 시켜줍니다. 저는

> // 북 - (0,-1), 동 - (-1,0), 남 - (0,1), 서 - (1,0)  
> static int\[\]\[\] left = { {0, -1, 0, 1}, {-1, 0, 1, 0}, };

으로 고정시켜서 사용하였습니다.

-   현재 북쪽을 바라보고 있고, 왼쪽 방향을 탐색하려 합니다.
-   left\[0\]\[\] 에 있는 값을 현재 위치에서 더해주고, 청소가 가능하면 청소를하고 해당 방향으로 변경
-   청소가 불가능하면 해당 방향으로 회전하여 다시 진행
-   네 방향 모두 불가한 것을 체크하기 위해 count 변수를 두어 확인해 주고 네 방향 전부 불가능 하면 현재 바라보고 있는 방향에서 뒤로 한 칸 후진 합니다.
-   만약 후진도 못하면 종료합니다.

이것을 코드로 표현하겠습니다.

---

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;


public class Robot_Vaccumcleaner {
	// 3 <= N, M <= 50 || 0 - empty, 1 - wall
	// d : 0 - North, 1 - East, 2 - South, 3 - West
	// 현재 위치에서 현재 방향을 기준으로 왼쪽방향부터 차례대로 탐색
	// 로봇 청소기가 있는 칸의 상태는 항상 빈 칸이다.
	static int N, M, r, c, d;
	static int clean;
	static int[][] map;
	// 바라보고 있는 방향에 따라서 탐색하는 왼쪽의 좌표가 다르다.
	// 북 - (0,-1), 동 - (-1,0), 남 - (0,1), 서 - (1,0)
	static int[][] left = {
			{0, -1, 0, 1},
			{-1, 0, 1, 0},
	};
	
	static void cleanning() {
		int cnt = 0;
		while(true) {
			int nxR = r + left[0][d];
			int nxC = c + left[1][d];
			if(nxR < 0 || N <= nxR || nxC < 0 || M <= nxC || map[nxR][nxC] == -1 || map[nxR][nxC] == 1) {	// 왼쪽에 청소 할 공간이 없다면, 그 위치로 회전.
				if(cnt >= 4) {	// 네 방향 모두 청소가 되어 있거나 벽인경우 방향 그대로 뒤로 후진
					switch(d) {
						case 0: nxR = r + 1; nxC = c; 	  break;
						case 1: nxR = r; 	 nxC = c - 1; break;
						case 2: nxR = r - 1; nxC = c; 	  break;
						case 3: nxR = r; 	 nxC = c + 1; break;
					}
					if(map[nxR][nxC] == 1) {	// 네 방향 모두 청소가 됭 있고, 뒤가 벽인 경우 종료.
						break;
					}
					r = nxR; c = nxC;
					cnt = 0;
				} else {
					d -= 1;
					if(d < 0) d = 3;
					cnt++;
				}
			} else if(map[nxR][nxC] == 0) {	// 왼쪽 방향에 청소 공간이 존재한다면,
				d -= 1;
				if(d < 0) d = 3;
				map[nxR][nxC] = -1;
				clean++;
				cnt = 0;
				r = nxR; c = nxC;
			}
		}
	}
	
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		
		N = Integer.parseInt(st.nextToken());
		M = Integer.parseInt(st.nextToken());
		map = new int[N][M];
		st = new StringTokenizer(br.readLine());
		r = Integer.parseInt(st.nextToken());
		c = Integer.parseInt(st.nextToken());
		d = Integer.parseInt(st.nextToken());
		for (int i = 0; i < N; i++) {
			if(!st.hasMoreTokens()) st = new StringTokenizer(br.readLine());
			for (int j = 0; j < M; j++) {
				map[i][j] = Integer.parseInt(st.nextToken());
			}
		}
		
		clean = 1; map[r][c] = -1;
		cleanning();
		
		System.out.println(clean);
	}
}
```