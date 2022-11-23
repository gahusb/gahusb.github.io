---
layout: post
title: Synology NAS 사용 정리
image:
subtitle: docker, server운영, spring, media, photo 등 활용법 정리
category: devlog
tags: synology
accent_image: 
  background: url('/assets/img/web_dev/webdev_sidebar.jpeg') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  Synology 사용하면서 익힌 활용법 정리
invert_sidebar: true
sitemap: false
comments: true
---

# 시놀로지 NAS

* toc
{:toc .large-only}

## 왜 NAS?
요즘에는 사람들이 NAS를 많이들 활용하고 있다.<br />
NAS는 Network Attached Storage의 약자로 네트워크에 연결된 저장장치로 사람들이 흔히 쓰는 클라우드의 기능을 하고 있는 일종의 서버이다.

단순히 클라우드 서비스와 같이 저장소로 활용을 많이 하고 있지만, 그 뿐만이 아닌 서버 운영, DB, 공유 등의 다양한 기능을 제공하기도 한다. <br />
또한 어디서든 외장하드 같은 것을 번거롭게 들고 다니지 않아도 된다. <br />
이 부분이 내가 NAS를 구매한 이유 중 하나이다. <br />

사실 NAS를 구매한건 22년 05월로 사용한지는 벌써 반년이 지났지만 이제서야 글로 작성하면서 사용했던 방식이나 활용법을 글로 적어보려고 한다...ㅎ <br />
구매한 이유는 가족들의 사진을 저장하고 어디서든 공유하면서 사용하기 위한 목적이 우선적이면서 무료 클라우드가 가득 찼기에 매달 요금을 내면서 사용하기 싫었기 때문에 큰 출혈이 있더라고 구매하였다. <br />

그러면서 더더욱 욕심을 내며 사이드 프로젝트를 위한 서버로도 활용하면 좋을 것 같다는 생각을 했기 때문에 관련 기능을 지원하는 NAS를 찾아보았다. <br />
(실제로 이 글을 쓰는 지금 서버로 활용하여 개발한 프로젝트가 2~3개는 된다.)

## Synology
여러가지 장비를 비교하며 찾아보다가 Synology 사의 장비를 구매하기로 결정했다. <br />
그 이유는 DSM센터를 통해서 사용하기 편리한 UI를 제공하고 있으며, Synology사의 다양한 내부 패키지 기능을 활용하면 사용 범위성이 무궁무진하기 때문이다. <br />

현재 구매한 NAS는 DS220+ 2Bay 제품으로, 가격적으로도 그렇고 내가 원하는 기능 정도로만 사용하기에 적당한 제품이다. <br />
![DS220+](https://cdn.011st.com/11dims/resize/1000x1000/quality/75/11src/asin/B087ZCBWFH/B.jpg?1669202174482)

내가 NAS를 사용하면서 현재 쓰고 있는 기능들은 아래와 같다. <br />
> - DS Photo를 통해 가족들이랑 연결해서 모바일의 사진을 저장하고 공유하는 기능
> - Video Station 기능을 통해 나만의 OTT 서비스
> - 개인 파일들의 저장
> - Note Station을 통한 나만의 노트 정리
> - 웹 서비스 구동
> - MariaDB 구동
> - Lotto 추첨 안드로이드 앱을 위한 Spring 서버 (사이드 프로젝트)
> - Docker 활용하기

이제와서 봐도 참 알차게 사용한것 같아서 뿌듯하네! <br />

이렇게 다양하게 활용하면서 익혔던 기능이나 까먹기 쉬운 기능들을 정리하려고 한다. <br />