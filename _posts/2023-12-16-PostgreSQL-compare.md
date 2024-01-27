---
layout: post
title: PostgreSQL와 MySQL 비교
categories: DB
tags: [database, progressql, study]
---
![database](https://assets.toptal.io/images?url=https%3A%2F%2Fbs-uploads.toptal.io%2Fblackfish-uploads%2Fcomponents%2Fblog_post_page%2Fcontent%2Fcover_image_file%2Fcover_image%2F1282568%2Fregular_1708x683_0712-Bad_Practices_in_Database_Design_-_Are_You_Making_These_Mistakes_Dan_Newsletter-f90d29e5d2384eab9f4f76a0a18fa9a8.png)

## PostgreSQL과 MySQL

PostgreSQL과 MySQL은 각각의 특징으로 서로 다른 사용 분야에서 적합하게 활용됩니다. 주로 MySQL은 RDBMS로서 쉽고 빠르게 데이터베이스를 구축하여 사용할 수 있으며, 주로 웹사이트 및 온라인 트랜잭션에 중점을 둔 속도와 안정성에 강조가 되어 있습니다.

반면 PostgreSQL은 객체 관계형 데이터베이스(ORDMNS)로, 상속, 함수, 오버로딩과 같은 기능을 가지고 있어 복잡한 쿼리와 대규모 데이터베이스를 다루는 데에 적합합니다. 이에 따라 PostgreSQL은 복잡한 대량의 데이터 작업을 수행하는 데 특히 유리합니다.

이를 그래픽으로 나타낸 데이터베이스 랭킹에 따르면 PostgreSQL이 MySQL에 비해 높은 위치에 있습니다.

![DB 사용 순위](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FoyqQz%2FbtsqBqLIo4N%2FD1acKIA5E7MYSrc4lo9XSK%2Fimg.png)

MySQL은 주로 웹의 표준 구성 요소로 사용되며, PostgreSQL은 다양한 데이터 유형을 지원하고 객체 유형 데이터, 사용자 정의 함수 및 저장 프로시저 등을 지원하여 복잡한 대량의 데이터 작업을 수행하는 데에 적합합니다.

### 질문

1. 왜 PostgreSQL을 선택했는지(비교와 함께)
   - 복잡한 쿼리를 높은 성능으로 수행하기 위해, MySQL에 비해 대용량의 데이터 작업에 뛰어난 PostgreSQL을 선택하게 되었습니다.

2. 어떤 면에서 PostgreSQL이 효과적이었는지
   - PostgreSQL은 특히 특수한 데이터베이스 상황 처리에 유리하며, 테이블과 열 정보뿐만 아니라 데이터 유형, 함수, 액세스 방법 등의 많은 정보를 카탈로그 기반 작업으로 확장할 수 있어 복잡한 대량의 데이터 작업을 효과적으로 수행할 수 있었습니다.

---

참고 자료
- [PostgreSQL vs MySQL: The Critical Differences](https://www.integrate.io/ko/blog/postgresql-vs-mysql-the-critical-differences-ko/)
- [How Extensibility Works in PostgreSQL](https://www.postgresql.org/docs/9.0/extend-how.html)
