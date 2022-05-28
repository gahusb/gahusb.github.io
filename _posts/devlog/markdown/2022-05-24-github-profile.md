---
layout: post
title: 눈을끄는 Github Profile 꾸미기
image:
subtitle: github profile
category: devlog
tags: markdown
accent_image: 
  background: url('/assets/img/blog/caleb-george.jpg') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  Github에서 제공하는 profile 꾸미기를 이용하여 나만의 독특한 프로필s 만들기
invert_sidebar: true
sitemap: false
---

# Github profile 꾸미기

* toc
{:toc .large-only}

 - 깃허브에서 제공하는 나의 profile 꾸미기 기능을 이용해 나만의 독특한 profile을 만들 수 있다.
 - 유니크함을 더해주는 badge 등을 이용하여 눈에띄는 profile을 만들 수 있다.

 ![gitprofile](/assets/img/dev/git-profile.png)

 ---

# Git profile을 위한 repository 만들기

![gitrepo](/assets/img/dev/create-gitrepo.png)

우선 깃허브 프로필을 설정하려면 본인의 ID와 일치하는 이름으로 레퍼지토리를 생성해야 한다.

나의 경우 github ID가 gahusb이니 gahusb으로 레포지토리를 만들어줬다.

일반적인 레퍼지토리 생성때와는 다르게 생성화면에서 고양이가 나타나며 말을 건다.

README.md 파일 생성하도록 체크하라는 내용이다. 바로 체크하고 레퍼지토리를 생성하자!

바로 이 레퍼지토리의 README.md 파일이 내 깃허브 프로필에 출력되는 것임을 알 수 있다.

# Read.me 꾸미기

## 헤더

우선 맨 위에 **Hello World**를 보여주는 헤더 부분을 추가 할 수 있다.

이 부분은 'capsule-render'라는 오픈API를 사용하였다.

[https://github.com/kyechan99/capsule-render](https://github.com/kyechan99/capsule-render)

이 오픈 소스는 awesome 하게도 크기, 모양, 문구 등을 자신의 취향에 맞게 선택하여 사용할 수 있고, 나의 프로필을 더욱 눈에 띄게 만들어줄 수 있다.

## 방문자 수

프로필인란것은 결국에 나에 대한 정보를 다른 사람에게 보여주기 위한 것이라고 생각한다.

때문에 얼마나 노출되었는지 판단할 수 있다면 좋다고 생각한다.

이것을 나타내주기 위해서 Hit라는 오픈 소스를 사용했다.

[https://hits.seeyoufarm.com/](https://hits.seeyoufarm.com/)

![Hit](/assets/img/dev/hit-badge.png)

위의 사이트에서 나의 github 주소를 입력하고 OPTIONS를 통해 색상과 끝의 라운드 정도 등을 선택하고

아래부분에 MARKDOWN의 주소를 COPY하여 내 read.me의 원하는 위치에 넣으면 방문하는 수에 따라서 카운트가 증가한다.

## 나에 대한 상세 설명

위에 간단한 방문 인사말고 나에 대해서 좀더 재미있게 표현하기 위해서 코드로 나를 표현하는 부분을 넣어 볼 수 있다.

```
(``` Grave + 언어)
```javascript
const jaeoh = {
    pronouns: "He" | "Him",
    code: ["Java", "C", "C++", "Javascript", "Python"],
    liveIn: Yeongdeungpo-gu, Seoul, Republic of Korea
};
```

위 코드처럼 내가 넣을 코드 스타일을 선택하여 나에 대한 정보를 입력해서 나를 소개하였다.

## 소셜 정보 & contact

나에게 연락 할 수 있는 Email이나 소셜 정보를 넣어주었다.

```
[![GitHub](icons/github.png)](https://github.com/gahusb)
[![LinkedIn](icons/linkedin.png)](https://www.linkedin.com/in/jaeoh-park-gahusb/)
[![Instagram](icons/instagram.png)](https://www.instagram.com/gahusb/)
```

위와 같이 나의 정보와 누가봐도 해당 소셜에 해당하는 아이콘으로 표시해줄 수 있다.

아이콘은 [여기](https://github.com/gahusb/gahusb/tree/main/icons)서 다운로드 받아서 나의 레포지토리에 넣고 사용할 수 있다.

## 기술 스택

뱃지(Badge)를 사용하여 나의 기술 스택을 이쁘게 표현할 수 있다.

[https://shields.io/](https://shields.io/)

위 shields라는 곳에서 내가 원하는 형태로 뱃지를 만들어서 사용할 수도 있고,

[https://simpleicons.org/](https://simpleicons.org/)

위 simpleicons라는 사이트에서 내가 원하는 기술 스택에 해당하는 아이콘을 받아서 뱃지로 만들어 사용할 수 있다.

```
![Java](https://img.shields.io/badge/Java-orange?style=flat-square&logo=java)
```

예를 들어 JAVA의 경우 위 **Java-orange**라는 뱃지의 모양에 내가 원하는 뱃지의 이름을 입력하고, 스타일, 로고 등을 선택하여 사용할 수 있다.

이런 나만의 뱃지를 만들기 귀찮다면,

[https://cocoon1787.tistory.com/689](https://cocoon1787.tistory.com/689)

이런 뱃지들을 만들어서 공유하는 오픈 소스를 가져다가 사용할 수 있다.

나만의 기술 스택들을 좀 더 자유롭고 눈에 띄게 바꿔보자

## Github stat

내가 지금까지 commit한 횟수나 repositories에 있는 언어를 분석하여 어떤 언어를 가장 많이 사용했는지 표시할 수 있다.
일종의 My github analytics 이라고 할까?

자신이 깃에서 어떤 활동을 하고 있는지 자세히 보여주는 지표이다.

```markdown
![아이디's github stats](https://github-readme-stats.vercel.app/api?username=아이디&show_icons=true)
```
**아이디**로 되어 있는 부분에 나의 github ID를 넣어서 사용할 수 있다.

나의 경우

```html
<p>
  <a href="https://github.com/gahusb">
    <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=gahusb&layout=compact&show_icons=true&show_owner=false&hide_title=false&theme=gruvbox" />
  </a>
  <a href="https://github.com/gahusb">
    <img src="https://github-readme-stats.vercel.app/api?username=gahusb&hide_title=false&show_icons=true&include_all_commits=false&theme=gruvbox" />
  </a>
</p>
```

위에 처럼 html 태그에 url을 넣어서 좀더 옵션을 제어하여 사용하였다.

나의 ID인 gahusb 부분을 지우고 자신의 github ID를 넣어서 사용하시면 됩니다.

# 더하기

중간중간 markdown에 나타나는 emoji의 경우 아래 글을 참고해서 사용하시면 됩니다.

[https://gahusb.github.io/devlog/markdown-emoji.html](https://gahusb.github.io/devlog/markdown-emoji.html)

또한 더 다양한 사람들의 github profile을 참고해서 나만의 프로필을 꾸미고 싶다면

[https://github.com/abhisheknaiidu/awesome-github-profile-readme](https://github.com/abhisheknaiidu/awesome-github-profile-readme
)

위의 사이트에서 여러 사람들의 awesome한 read.me를 참고하여 나의 프로필에 적용해보자 😊