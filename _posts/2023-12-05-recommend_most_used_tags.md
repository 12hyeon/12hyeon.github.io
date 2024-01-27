---
layout: post
title: 많이 사용된 태그 추천
categories: project
tags: [sns-feed, redis, tag]
---
![레디스](https://www.bleepstatic.com/content/hl-images/2022/12/01/Redis.jpg)

## Redis

Key-Value 구조로, Value는 String, Set, Sorted Set, Hash, List 등 다양하게 가능합니다.


### Sorted Set

> List : 순서, 중복o ↔ Sorted Set : 정렬

set에 score라는 가중치 필드가 추가된 데이터 형태를 사용합니다.

일반 set과 달리 저장된 member의 순서도 관리됩니다.

score에 따라 오름차순 정렬되며, 사전 순으로 정렬됩니다.

이 구조의 장점은 특정 범위 내 데이터를 쉽게 조회할 수 있고, score를 기준으로 정렬된 데이터를 얻을 수 있습니다.

단점으로는 데이터를 추가/삭제하는 경우, member와 score 모두를 관리해야 합니다.

이러한 구조는 좋아요 처리나 일일 방문자 수를 기록하는 경우에 유용하며, 중복을 허용하지 않습니다.

### HyperLogLogs

HyperLogLogs는 중복된 요소의 개수를 카운트하기 위한 집합 기반 데이터 구조입니다.

이 구조의 장점은 정확성을 유지하면서 메모리를 적게 사용한다는 것입니다 (모든 값이 12KB로 고정).

단점으로는 중복된 데이터의 근사치를 파악하며, 이때 오차 범위가 0.81%입니다.

이러한 구조는 데이터의 대략적인 근사치를 파악하고 메모리를 효율적으로 사용해야 하는 경우에 유용합니다.

### 결론

정렬과 중복 없는 정확한 데이터 개수가 필요한 경우에는 sorted set을 사용합니다.

데이터 근사치를 파악하고 메모리를 효율적으로 사용하는 경우에는 HyperLogLogs를 사용합니다.


## 필터(Filter) vs 인터셉터(Interceptor)

상황은 posting의 저장과 검색에서 tag를 사용하는 경우입니다.

### Filter

Filter는 서블릿에 요청이 전달되기 전(서블릿 컨테이너)/후에 url 패턴에 맞는 작업을 처리합니다.

Request와 Response를 조작할 수 있으며, 주로 공통된 보안, 인증 작업, 모든 요청에 대한 로깅, 압축 및 인코딩에 사용됩니다.

### Interceptor
Interceptor는 서블릿에 요청이 컨트롤러 호출 전(스프링 컨테이너)/후에 요청/응답을 참조합니다.

스프링에서 예외처리가 가능하며, 주로 세부적인 보안 작업, API 호출에 대한 로깅, Request 정보 가공에 사용됩니다.


## AOP

AOP는 공통으로 사용되는 관심사의 모듈화를 목적으로 합니다.

주로 RestController를 이용하면서 JSON으로 반환해 뷰랜더링의 후 처리 작업이 필요 없을 때 사용됩니다.

> AOP, 커스텀 에러 핸들러, 필터를 이용해서 요청/응답을 가로채서 수행하는 방식을 이용할 수 있습니다.
