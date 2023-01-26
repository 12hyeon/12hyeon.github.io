---
layout: post
title: transaction 및 index
categories: DB
tags: [database, transaction, index, study]
---
![database](https://assets.toptal.io/images?url=https%3A%2F%2Fbs-uploads.toptal.io%2Fblackfish-uploads%2Fcomponents%2Fblog_post_page%2Fcontent%2Fcover_image_file%2Fcover_image%2F1282568%2Fregular_1708x683_0712-Bad_Practices_in_Database_Design_-_Are_You_Making_These_Mistakes_Dan_Newsletter-f90d29e5d2384eab9f4f76a0a18fa9a8.png)

## 트랜잭션

### 정의

데이터베이스의 상태를 변화시키기 위해서 수행하는 작업의 단위로, 여러 작업의 묶음을 의미합니다. 예를 들어, 게시물 등록은 저장과 조회를 합친 하나의 트랜잭션입니다.

### 특징 4가지

![트랜잭션 특징](https://www.databricks.com/wp-content/uploads/2021/02/delta-lake-1-min.png)

- 원자성(모두 반영 또는 아예 반영하지 않음)
- 일관성(중간에 업데이트 되더라도 처음 참조한 기준 이용)
- 독립성
- 지속성(성공 시, 영구적 반영)

### 관련 단어

질의어(SQL): DDL(Data Definition Language) / DML(Data Manipulation Language)로 나뉩니다.

데이터 정의 언어: 스키마 조작 / 데이터 조작 언어: 레코드 조작 

테이블: 행과 열의 데이터 집합, `@Table`을 통해 엔티티와 테이블을 매핑합니다.

스키마: 제약 조건(`@Column`), 무결성 규칙, 데이터 유형, 테이블 등을 정의한 것으로, PostgreSQL, 오라클, MySQL 등에서 다르게 해석될 수 있습니다.

무결성: 데이터의 일관성을 유지하기 위한 제한을 둔 것으로, 개체 무결성, 참조 무결성 등이 있습니다.

### 트랜잭션 종료 명령어

`commit`: 트랜잭션이 성공적으로 종료됨

`rollback`: 비정상 종료로 인한 연산 결과를 취소


## 인덱스

### 정의

데이터베이스 테이블의 검색 속도를 향상시키기 위한 자료 구조로, 추가적인 쓰기와 저장 공간을 활용합니다. 전체적인 연산 속도를 향상시킵니다.

### 장점

조회 속도 향상으로 인한 성능 향상이 이루어집니다.

### 단점

10%의 저장 공간을 인덱스 관리에 사용하며, 잘못 사용하면 성능이 저하될 수 있습니다.

### 구현 방식*

해시 테이블과 B+ 트리가 주로 사용되며, 각 인덱스는 DML에 따라 반영되어야 합니다.

### 인덱스 생성 시 고려 사항
- 카디널리티(값 중복도), 선택도(한 칼럼 값으로 얼마나 여러 row가 조회되는지), 조회 활용도, 수정 빈도를 고려해야 합니다.
- 테이블당 하나의 Clustered Index만 생성되며, Non-Clustered Index는 여러 개 생성될 수 있습니다.
- **데이터베이스에서의 null**: 값이 존재하지 않음을 나타냅니다. (입력되지 않은, 알 수 없는 값)

---

참고 자료

- [무결성](https://untitledtblog.tistory.com/123)
- [인덱스](https://mangkyu.tistory.com/96)
