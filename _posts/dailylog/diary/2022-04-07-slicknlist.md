---
layout: post
title: 블로그 기능 추가
image: 
category: dailylog
tags: diary
accent_image: 
  background: url('/assets/img/daily/diary_sumnail.jpeg') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  일상 기록
invert_sidebar: true
---

# 게시글 테스트

* toc
{:toc .large-only}

---

## slick image
[Slick](https://kenwheeler.github.io/slick/)
slick 사이트에 들어가서 `get it now`를 눌러 다운로드 받아주어 slick 폴더를 `/assets/css/slick`에 복사>붙여넣기 해준다.

그리고 scss를 수정하여 표출 가능하도록 변경!   

> assests/css/slick/slick-theme.css   

```css
.slick-prev:before
{
    content: url(left.png);
}
[dir='rtl'] .slick-prev:before
{
    content: url(right.png);
}

.slick-next:before
{
    content: url(right.png);
}
[dir='rtl'] .slick-next:before
{
    content: url(left.png);
}
```

게시글 원하는 위치에 아래와 같이 넣어주면 된다.

```html
<div class="main_center">
    <div><img src= "/assets/img/blog/hydejack-8.jpg" style="width: auto; height: 500px;"></div>
    <div><img src="/assets/img/blog/hydejack-8.png" style="width: auto; height: 500px;"></div>
    <div><img src= "/assets/img/blog/hydejack-9-dark.jpg" style="width: auto; height: 500px;"></div>
</div>
<script>
    $(document).ready(function() {
        $('.main_center').slick({
            autoplay : true, /*자동으로 슬라이딩됨*/
            dots : true, /* 하단 점 버튼 */
            speed : 300 /* 이미지가 슬라이딩시 걸리는 시간 */,
            infinite : true,
            autoplaySpeed : 30000 /* 이미지가 다른 이미지로 넘어 갈때의 텀 */,
            arrows : true,
            slidesToShow : 1,
            slidesToScroll : 1,
            touchMove : true, /* 마우스 클릭으로 끌어서 슬라이딩 가능여부 */
            nextArrows : true, /* 넥스트버튼 */
            prevArrows : true,
            arrow : true, /*false면 좌우 버튼 없음, true면 좌우 버튼 보임*/
            fade : false
        });
    });
</script>
```

.slick() 안의 옵션을 원하는대로 설정하여 사용 할 수 있다.

<div class="main_center">
    <div><img src= "/assets/img/blog/hydejack-8.jpg" style="width: auto; height: 500px;"></div>
    <div><img src="/assets/img/blog/hydejack-8.png" style="width: auto; height: 500px;"></div>
    <div><img src= "/assets/img/blog/hydejack-9-dark.jpg" style="width: auto; height: 500px;"></div>
</div>

<script>
    $(document).ready(function() {
        $('.main_center').slick({
            autoplay : true, /*자동으로 슬라이딩됨*/
            dots : true, /* 하단 점 버튼 */
            speed : 300 /* 이미지가 슬라이딩시 걸리는 시간 */,
            infinite : true,
            autoplaySpeed : 30000 /* 이미지가 다른 이미지로 넘어 갈때의 텀 */,
            arrows : true,
            slidesToShow : 1,
            slidesToScroll : 1,
            touchMove : true, /* 마우스 클릭으로 끌어서 슬라이딩 가능여부 */
            nextArrows : true, /* 넥스트버튼 */
            prevArrows : true,
            arrow : true, /*false면 좌우 버튼 없음, true면 좌우 버튼 보임*/
            fade : false
        });
    });
</script>

---
## 게시글 목차 만들기
h1 타이틀 바로 아래에

```
* toc
{:toc .large-only}
```

를 추가하여 헤더를 기준으로 목차 생성

---
## 페이지 버튼 만들기

> _include/components/page-button.html   

```html
<div class="page-control">
    <div>
        {% if page.previous.url %}
        <a id="prev" class="w-btn-outline w-btn-gray-outline" href="{{ page.previous.url }}">&laquo; {{ page.previous.title }}</a>
        {% endif %}
    </div>
    <div>
        {% if page.next.url %}
        <a id="next" class="w-btn-outline w-btn-gray-outline" href="{{ page.next.url }}">{{ page.next.title }} &raquo;</a>
        {% endif %}
    </div>
</div>
```

> _layouts/post.html   

```console
{% include components/page-button.html %}
```

About 위에 위치하여 작성자 소개 위에 위치하도록 한다.

원하는 스타일대로 변경을 위해서 다음과 같이 작업해준다.   

> _sass/my-style.scss   

```scss
.page-control {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.page-control > div {
  max-width: 50%;
}.page-control {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.page-control > div {
  max-width: 50%;
}

.w-btn-outline {
  position: relative;
  padding: 15px 30px;
  border-radius: 15px;
  font-family: "paybooc-Light", sans-serif;
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
  text-decoration: none;
  font-weight: 600;
  transition: 0.25s;
}

.w-btn-outline:hover {
  letter-spacing: 2px;
  transform: scale(1.2);
  cursor: pointer;
}

.w-btn-outline:active {
  transform: scale(1.5);
}

.w-btn-gray-outline {
  border: 3px solid #a3a1a1;
  color: #6e6e6e;
}

.w-btn-gray-outline:hover {
  background-color: #a3a1a1;
  color: #e3dede;
}
```

---

## 게시글에 유튜브플레이어 보기

> _includes/components/youtubePlayer.html   

```html
<style>
    .embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; }
    .embed-container iframe,
    .embed-container object,
    .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }
</style>

<div class="embed-container" >
    <iframe src="https://www.youtube.com/embed/{{ include.id }}" frameborder="0" allowfullscreen="" onclick="ga('send', 'event', 'post', 'click', 'youtubePlayer');">
    </iframe>
</div>
```

위 처럼 youtube 영상의 id를 가져와서 표출 할 수 있도록 만들고

```console
`{% include components/youtubePlayer.html id="{youtube ID}" %}`
```

게시글 원하는 위치에 youtube id를 넣어주면 된다.

![image](https://likelion.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F72612a68-022b-424b-8ee1-4600408d2b93%2FUntitled.png?table=block&id=b894acab-60c6-4de6-b0c2-7e0c15a7c844&spaceId=c69962b0-3951-485b-b10a-5bb29576bba8&width=1220&userId=&cache=v2)

위와 같이 `watch?v=` 이후에 오는 id를 가져다가 사용한다.

{% include components/youtubePlayer.html id='0TNFb5zgpbg' %}

---

## 게시물 조회수 보이기
Hits 이용   
> [Hits](https://hits.seeyoufarm.com/)

TARGET URL에 나의 블로그 주소를 입력하고, Options에서 여러 설정들을 조정하여 내가 원하는 스타일의 hits를 만들 수 있다. 미리보기를 통해 표시되는것을 볼 수 있으니 참고해서 만들면 된다.

그렇게 생성된 아래의 HTML Link를 복사해서

> _layouts/post.html   

에 넣어서 각 게시물 별로 조회수가 보이는 위치를 조정할 수 있다.

> _includes/body/sidebar-sticky.html   

에 넣으면 사이드바는 어떤 게시물을 봐도 같이 보이는것이기 때문에 전체 조회수를 체크할 수 있다.

만약 조회수 가장 오른쪽에 생성되는 링크모양 아이콘이 싫다면

> _sass/my-style.scss

```scss
a.external::after, a::after {
  display: none;
}
```

를 추가하여 제거 할 수 있다.

---

