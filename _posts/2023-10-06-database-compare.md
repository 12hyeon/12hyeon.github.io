---
layout: post
title: 데이터베이스 비교
categories: DB
tags: [database, study]
---
![database](https://assets.toptal.io/images?url=https%3A%2F%2Fbs-uploads.toptal.io%2Fblackfish-uploads%2Fcomponents%2Fblog_post_page%2Fcontent%2Fcover_image_file%2Fcover_image%2F1282568%2Fregular_1708x683_0712-Bad_Practices_in_Database_Design_-_Are_You_Making_These_Mistakes_Dan_Newsletter-f90d29e5d2384eab9f4f76a0a18fa9a8.png)

## DB의 핵심 3가지

데이터베이스의 핵심 3가지는 무결성, 안전성, 그리고 확장성입니다. 무결성은 정확하고 일관성 있는 데이터를 의미하며, 안전성은 고장이 발생하지 않는 상태를 나타냅니다. 확장성은 scale-up(코어, 메모리 등을 확장)과 scale-out(기기를 여러개 늘리는 방식)으로 나뉘며, 선택은 크기를 키울 것인지, 기기를 늘릴 것인지에 따라 달라집니다.

>무결성 : 정확, 일관성 (누가 보더라도 동일)
>
>안전성 : 고장x 
>
>확장성 (조회를 한 사람 vs 여러 사람)
>- scale-up 코어, 메모리 등을 확장 = 크기를 키울건지
>
>- scale-out 기기를 여러개 = 기기를 늘릴건지

## 관계형 DB

테이블 사이에 관계를 맺어서 중복데이터 줄입니다.

하지만 laterncy가 많이 소모 & 예전에는 메모리가 비싸서 주로 사용했음- 중복된 데이터를 없애고 메모리 비용을 아낍니다.

데이터 수정 시 관련된 여러 테이블을 변경해야 하기 때문에 시간이 오래 걸릴 수 있음

- row-oriented : 읽는 양이 적은 경우, **많은 데이터 삽입**에 적합하고, row뒤에 바로 row가 붙어서 삽입이 쉽습니다.
- column-oriented : **읽는 것이 빨라서** 데이터 분석에 사용하고, 각 칼럼끼리 모아두는 구조로 삽입이 많으면 부적합 합니다.


## NOSQL

NOSQL은 서로 다른 종류의 데이터를 저장할 수 있고, 유연한 스키마를 가지고 있어 데이터 구조를 동적으로 변경이 가능합니다. 

Key-value는 Redis와 같은 데이터베이스에 적합합니다. 

Graph는 관계를 표현하기에 유리하나 한국에서는 덜 사용됩니다. 

Document는 JSON 데이터를 넣기에 적합합니다.

## CAP 이론

분산 시스템에서 **일관성**, **분산 저장 가능성**, **가용성(항상 데이터에 접근 가능성)** 중 두 가지만 선택할 수 있다는 이론입니다.

### 선택지
1. 높은 가용성을 제공하면서 일부 일관성을 희생하는 NoSQL 데이터베이스 
2. 높은 일관성을 제공하면서 가용성을 희생하는 관계형 데이터베이스
