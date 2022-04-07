---
layout: post
title: 소고기 먹부림
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

## slick image
Slick
<div class="main_center">
    <div><img src= "/assets/img/blog/hydejack-8.jpg" style="width: 700px; height: auto;"></div>
    <div><img src="/assets/img/blog/hydejack-8.png" style="width: 700px; height: auto;"></div>
    <div><img src= "/assets/img/blog/hydejack-9-dark.jpg" style="width: 700px; height: auto;"></div>
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

## 게시글 목차 만들기
