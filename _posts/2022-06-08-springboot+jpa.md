---
title: 깃허브 블로그 글 조회수 기능 추가 일단 실패 (Chirpy 테마)
author: mamulee
date: 2022-06-08 15:04:00 +0900
categories: [Studying, Notes]
tags: []
img_path: /assets/img/page-views/
---

# 실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발

## 프로젝트 환경 설정

### H2 데이터베이스 설치
`H2 데이터베이스` : 개발이나 테스트 용도로 가볍고 편리한 DB, 웹 콘솔 환경 제공
https://h2database.com/html/main.html
맞는 버전 (.zip) 다운로드 후
> h2/bin 위치에서
> 
> ![콘솔창 1](1.png)

> JAVA로 실행되기 때문에 JAVA 설치 필수
{: .prompt-tip }

> ![콘솔창 2](2.png)
> 나는 위와 같이 오류가 발생해 파일 실행 권한을 추가해 주었다.
{: .prompt-error }

![실행된 화면](3.png)

`JDBC URL` : jdbc:h2:~/jpashop -> 연결
![연결 후 화면](4.png)

`JDBC URL` : jdbc:h2:tcp://localhost/~/jpashop -> 연결

> DB 파일 생성할 때만 파일 모드, 이후는 네트워크 모드로 접근
> 
> h2.sh를 항상 실행해놔야 DB 동작 가능
