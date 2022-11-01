---
layout: post
title: Spring 개념
image:
subtitle: jpa + lombok 사용하기
category: devlog
tags: spring
accent_image: 
  background: url('/assets/img/web_dev/webdev_sidebar.jpeg') center/cover
  overlay: false
accent_color: '#ccc'
theme_color: '#ccc'
description: >
  스프링의 jpa + lombok 방식 활용하기
invert_sidebar: true
sitemap: false
comments: true
---

# 스프링 프로젝트

* toc
{:toc .large-only}

## JPA, 자바 지속성 API(JAVA Persistence API)
자바 플랫폼 SE와 EE를 사용하는 응용프로그래메서 관계형 데이터베이스의 관리를 표현하는 Java API이다.
이전에 SSAFY를 다니며 배운 경험은 있지만 다시 한 번 사용하면서 정리해보기로 한다.

## lombok
getter, setter 등의 반복 메서드를 자동으로 연결하고 도구를 빌드하여 Java를 향상시키는 Java 라이브러리이다. <br />
[lombok](https://projectslombok.org/)

## 1. Dependency 추가 (jpa, lombok, mariaDB)
> Maven의 경우 pom.xml에 추가

```xml
<!-- JPA -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<!-- lombok -->
<dependency>
  <groupId>org.projectlombok</groupId>
  <artifactId>lombok</artifactId>
  <optional>true</optional>
</dependency>

<!-- mariaDB -->
<dependency>
  <groupId>org.mariadb.jdbc</groupId>
  <artifactId>mariadb-java-client</artifactId>
</dependency>
```

> gradle의 경우 build.gradle에 추가

```gradle
dependencies {
  // JPA
  implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
  // lombok
  implementation 'org.projectlombok:lombok'
  // mariaDB
  implementation 'org.mariadb.jdbc:mariadb-java-client'
}
```

여기서 의문점이 좀 들었다. <br />
Maven과 Gradle의 차이점은 무엇일까? 바로 알아보자 <br />
-> [Maven과 Gradle](https://gahusb.github.io/devlog/maven-gradle.html)

## 2. 데이터베이스 설정
> application.properties

```java
spring.datasource.driverClassName=org.mariadb.jdbc.Driver
spring.datasource.url=jdbc:mariadb://IP:3306/databaseName
spring.datasource.username=user
spring.datasource.password=pwd
```

> aplication.yml

```java
spring:
  datasource:
    driverClassName: org.mariadb.jdbc.Driver
    url: jdbc:mariadb://IP:3306/databaseName
    username: user
    password: pwd
```

## 3. 재사용될 컬럼을 추상 클래스로 작성
> CommonVo.java

```java
import java.time.LocalDateTime;
import javax.persistence.Column;
import javax.persistence.MappedSuperclass;
import org.hibernate.annotations.CreationTimestamp;
import lombok.Data;

// Getter, Setter Auto Create
@Data
// 공통 매핑 정보가 필요할 때, 부모 클래스에 선언하고 속성만 상속 받아서 사용하고 싶을 때 사용한다.
@MappedSuperclass
public abstract class CommonVo {

  // register id
  private String regId;

  // modify id
  private String modId;

  // register Date time
  @CreationTimestamp
  /*
  * 보통 JPA는 SAVE시에 모든 칼럼을 INSERT한다.
  * 그럴 경우, NOT NULL로 설정된 칼럼은 기본값으로 삽입되는 것이 아닌 NULL로 삽입을 시도한다.
  * 이로 인해 에러가 발생하는데, 이럴 경우에 아예 쿼리에서 빼버려서 실행이 안되게 만들 수 있다.
  * 쿼리에서 제외된 칼럼은 DB에 지정된 default값으로 삽입이 된다.
  * 특정 칼럼을 제외하고 save하는 방법은 다음과 같다.
  * insertable = false, updatable = false
  */
  @Column(updatable = false)
  private LocalDateTime regDtm;

  // modify Date time
  @CreationTimestamp
  private LocalDateTime modDtm;
}
```

## 4. CommonVo를 상속받은 MemberVo 작성
> MemberVo.java

```java
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import com.melon.boot.common.vo.CommonVo;
import lombok.Data;

// Getter, Setter Auto Create
@Data
@Entity(name = "member")
public class MemberVo extends CommonVo {
  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long mbrSeq;
  private String id;
  private String pwd;
  private String name;
  private String addrLoad;
}
```

## 5. JpaRepository를 상속받은 interface 작성
> MemberRepository.java

```java
package com.melon.boot.member.service.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import com.melon.boot.member.vo.MemberVo;

public interface MemberRepository extends JpaRepository<MemberVo, Long> {
}
```

## 6. Service Class 작성
> MemberService.java

```java
import java.util.List;
import java.util.Optional;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import com.melon.boot.member.service.repository.MemberRepository;
import com.melon.boot.member.vo.MemberVo;

@Service
public class MemberService {

  @Autowired
  private MemberRepository memberRepository;

  // INSERT
  public MemberVo insert(MemberVo memberVo) throws Exception {
    return memberRepository.save(memberVo);
  }

  // SELECT LIST
  public List<MemberVo> findAll() throws Exception {
    return memberRepository.findAll();
  }

  // SELECT BY ID
  public Optional<MemberVo> findById(Long mbrSeq) throws Exception {
    return memberRepository.findById(mbrSeq);
  }

  // UPDATE
  public MemberVo updateById(MemberVo memberVo) throws Exception {
    Optional<MemberVo> e = memberRepository.findById(memberVo.getMbrSeq());

    MemberVo resultMember = null;
    if(e.isPresent()) {
      resultMember = memberRepository.save(memberVo);
    }
    return resultMember;
  }

  // DELETE
  public void deleteById(Long mbrSeq) throws Exception {
    memberRepository.deleteById(mbrSeq);
  }
}
```

## 7. Controller Class 작성
> MemberController.java

```java
import java.time.LocalDateTime;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import com.melon.boot.member.service.MemberService;
import com.melon.boot.member.vo.MemberVo;

@Controller
@RequestMapping("/member")
public class MemberController {

  @Autowired
  MemberService memberService;

  @GetMapping("/insert")
  public void insertMember() throws Exception {
    try {
      MemberVo memberVo = new MemberVo();

      memberService.insert(memberVo);
    } catch(Exception e) {
      e.printStackTrace();
    }
  }

  @GetMapping("/selectList")
  public void selectMemberList() throws Exception {
    try {
      memberService.findAll();
    } catch(Exception e) {
      e.printStackTrace();
    }
  }

  @GetMapping("/select")
  public void selectMember() throws Exception {
    try {
      memberService.findById((long) 1);
    } catch(Exception e) {
      e.printStackTrace();
    }
  }

  @GetMapping("/update")
  public void updateMember() throws Exception {
    try {
      MemberVo memberVo = new MemberVo();

      memberService.updateById(memberVo);
    } catch(Exception e) {
      e.printStackTrace();
    }
  }

  @GetMapping("/delete")
  public void deleteMember(MemberVo memberVo) throws Exception {
    try {
      memberService.deleteById(memberVo.getMbrSeq);
    } catch(Exception e) {
      e.printStackTrace();
    }
  }
}
```

## 8. 테스트 URL
 - /member/insert
 - /member/selectList
 - /member/select
 - /member/update
 - /member/delete


위에서처럼 jpa, lombok, mariadb를 사용해서 CRUD 통신하는 서버를 만들어보았다. <br />
이제 이것을 바탕으로 GLotto에 해당하는 데이터 테이블을 만들고 데이터를 넣은다음 돌아가는 서버를 만들 수 있겠다. <br />