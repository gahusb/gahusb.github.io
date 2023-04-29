---
layout: post
title: TCP/IP
image:
subtitle: TCP/IP의 이해
category: devlog
tags: cs
accent_image: 
  background: url('/assets/img/web_dev/webdev_sidebar.jpeg') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  TCP를 이해하고 정리하자.
invert_sidebar: true
sitemap: false
comments: true
---

# TCP(Transmission Control Protocol) / IP(Internet Protocol)
TCP 프로토콜을 자주 사용하면서도 CS 지식으로 연결되는 부분이 있기 때문에 정리하고자 한다. <br />

TCP/IP 를 사용하면서 해당 지식을 정확하게 알지 못하는 것은 어불성설이다. <br />
따라서 상세히 알아보고 정리하고자 한다. <br />

* toc
{:toc .large-only}

## TCP/IP 란?
전송 제어 프로토콜의 약자로, 인터넷 프로토콜 스위프트(IP)의 핵심 프로토콜 중 하나로, IP와 함께 TCP/IP라는 명칭으로 불린다. <br />

TCP/IP 를 사용하겠다는 것은 **IP** 주소 체계를 따르고 IP Routing을 이용해 목적지에 도달하며 **TCP** 의 특성을 활용해 송신자와 수신자의 논리적 연결을 생성하고 신뢰성을 유지할 수 있도록 하겠다는 의미이다. 즉, TCP/IP를 말한다는 것은 송신자가 수신자에게 IP 주소를 사용하여 데이터를 전달하고 그 데이터가 제대로 도달했는지, 너무 빠르진 않았는지, 받았다는 응답이 오는지에 대한 이야기를 하는 것이다. <br />

> Transport Layer(4 Layer)
>> 송신자와 수신자의 논리적 연결을 담당하는 부분으로, 신뢰성 있는 연결을 유지할 수 있도록 도와줍니다.  
>> 즉 Endpoint(사용자) 간의 연결을 생성하고 데이터를 얼마나 보냈는지, 얼마나 받았는지, 제대로 받았는지 등을 확인합니다.  
>> TCP와 UDP가 대표적입니다.  

> Network Layer(3 Layer)  
>> IP(Internet Protocol)이 활용되는 부분으로, 한 Endpoint가 다른 Endpoint로 가고자 하는 경우 경로와 목적지를 찾아줍니다.  
>> 이를 Routing이라고 하며 대역이 다른 IP들이 목적지를 향해 제대로 찾아갈 수 있도록 돕는 역할을 합니다,  

> 출처 : OSI 7 Layer 쉽게 이해하기  
> [네트워크 엔지니어 환영의 AWS 기술블로그]

인터넷에서 무언가를 다운로드할 때 중간에 끊기거나 빠지는 부분 없이 완벽하게 받을 수 있는 이유도 TCP의 이러한 특성 덕분이다.  
그렇게 때문에 위에서 언급한 것처럼 HTTP, HTTPS, FTP, SMTP 등과 같이 데이터를 안정적으로 모두 보내는 것을 중요시하는 프로토콜들의 기반이 된다. <br />
TCP를 기반으로 하는 프로토콜들은 TCP의 `3-way handshake`를 거친 후, 각자 프로토콜(Layer 7)에 기반한 교환 과정을 거친다는 의미이다. <br />

![SSL Handshake](https://cf-assets.www.cloudflare.com/slt3lc6tev37/5aYOr5erfyNBq20X5djTco/3c859532c91f25d961b2884bf521c1eb/tls-ssl-handshake.png)

위 이미지는 TCP 기반의 프로토콜인 HTTPS의 `SSL handshake`를 도식화한 것이다. <br />
TCP는 4 Layer이고 HTTPS는 7 Layer 이다. <br />

파란색 부분은 TCP의 `3-way handshake`이고, 노란색 부분은 HTTPS의 `SSL handshake`이다. <br />
HTTPS는 TCP 기반이기 때문에 SSL handshake에 앞서 3-way handshake를 하는 것을 볼 수 있다. <br />

## TCP 개요
TCP는 OSI 7 Layer 중에 4 계층에 해당한다. IP가 그저 목적지를 제대로 찾아가는 것에 중점을 둔다면, TCP는 통신하고자 하는 양쪽 단말(Endpoint)이 통신할 준비가 되었는지, 데이터가 제대로 전송되었는지, 데이터가 가는 도중 변질되지는 않았는지, 수신자가 얼마나 받았고 빠진 부분은 없었는 등을 점검한다. <br />
이런 정보는 TCP Header에 담겨 있으며 SYN, ACK, FIN, RST, Source Port, Destination Port, Sequence Number, Window Size, Checksum 등과 같은 신뢰성 보장과 흐름 제어, 혼잡 제어에 관여할 수 있는 요소들을 포함하고 있다. <br />
또한 IP Header와 TCP Header를 제외한 TCP가 실을 수 있는 데이터의 크기를 `세그먼트(Segment)`라고 부른다. <br />

![TCP Header](https://images.velog.io/images/inourbubble2/post/d48725bc-6060-4688-8f0e-70530a9c0250/image.png)

<center>`TCP Header의 구조(출처: Wikipedia)`</center>

TCP는 IP의 정보들뿐만 아니라 Port를 이용하여 연결한다. 한 쪽 단말(Endpoint)에 도착한 데이터가 어느 입구(Port)로 들어가야 하는지 알아야 연결을 시도할 수 있기 때문이다. <br />