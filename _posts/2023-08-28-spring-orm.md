---
layout: post
title: Java ORM 기술
categories: spring
tags: [orm, spring, study]
---
![spring](https://spring.io/img/og-spring.png)

ORM(객체 관계 매핑)은 자바의 객체와 관계형 데이터베이스를 맵핑하는 프레임워크로, SQL문을 일일이 작성하지 않고 객체로 구현할 수 있도록 합니다.

## JDBC

JDBC(Java Database Connectivity)는 자바에서 데이터베이스에 접근할 수 있도록 제공하는 API로, 모든 영속성 프레임워크는 내부적으로 plain JDBC API를 사용합니다. 

그러나 JDBC를 사용하면 많은 코드를 작성해야하고, 트랜잭션 처리와 예외 처리 등이 필요하여 비효율적입니다. 

Spring에서는 이를 보완하기 위해 JDBC Template을 제공하여 개발자가 SQL 작성과 결과 처리에 집중할 수 있도록 도움을 줍니다.

---


## 영속성

영속성은 데이터를 생성한 프로그램이 종료되어도 데이터가 존재하는 특성을 말합니다.

영속성을 가진 프레임워크는 데이터베이스와 연동되는 시스템을 빠르게 개발하고 안정적으로 구동할 수 있도록 합니다.

---


## ORM

ORM은 데이터베이스 객체를 자바 객체로 매핑하여 객체 간의 관계를 반영하고 SQL을 자동으로 생성하는 프레임워크입니다. JPA, Hibernate 등이 ORM의 영속성 API 중 일부입니다.

### ORM의 영속성 API

영속성 API에서는 find, read, query, count, get에 by에 붙은 형태의 메소드를 선언하며, 이를 통해 Repository 인터페이스에서 자동으로 구현할 수 있습니다.
