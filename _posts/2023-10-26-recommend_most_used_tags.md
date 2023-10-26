---
title: "[spring] 많이 사용된 태그 추천"
date: 2023-10-26
categories:
  - Blog
tags:
  - data
  - redis
---


### **Redis**

---

- Key-Value 구조로, Value는 String, Set, Sorted Set, Hash, List 등다양하게 가능



### ****Sorted Set : 정렬 (↔ List : 순서, 중복o)****

---

- set에 score라는 가중치 필드가 추가된 데이터 형태
- 일반 set과 달리 저장된 member의 순서도 관리
- score에 따라 오름차순 정렬 → 사전 순
- 장점 : 특정 범위 내 데이터 쉽게 조회, score 기준으로 정렬된 데이터
- 단점 : 데이터 추가/삭제 경우, **member와 score** 모두 관리

→ 좋아요 처리나 일일 방문자수 기록하는 경우 **(중복x)**

### **HyperLogLogs**

---

- 중복된 요소의 개수를 카운트하기 위한 집합 기반 데이터 구조
- 장점 : 정확성을 위해 메모리를 적게 사용 (모든 값이 12kb)
- 단점 : 중복된 데이터의 **근사치를 파악** (오차 범위 0.81%)

→ 데이터의 대략적인 근사치를 파악하고 메모리를 효율적으로 사용하는 경우

### 💡 **결론** 
<aside>
정렬과 중복 없는 정확한 데이터 개수가 필요한 경우 : sorted set

데이터 근사치 파악 + 메모리 효율 : hyperLogLogs

</aside>

<br>


### ****필터(Filter) vs 인터셉터(Interceptor)****

---

- 상황 : posting의 저장과 검색에서 tag를 사용한 경우

### ****Filter****

---

- 서블릿에 요청이 전달되기 전(서블릿 컨테이너)/후에 url 패턴에 맞는 작업 처리
- request, response 조작 가능
- 용도 : **공통된 보안**, 인증 작업 / **모든 요청에 대한 로깅** / 압축 및 인코딩

### ****Interceptor****

---

- 서블릿에 요청이 컨트롤러 호출 전(스프링 컨테이너)/후에 요청/응답을 참조
- 스프링에서 예외처리 가능
- 용도 : **세부적인 보안** 작업 / api 호출에 대한 로깅 / **request 정보 가공**

### **AOP**

---

- **공통으로 사용되는 관심사**의 모듈화를 목적으로 함
- 주로 **RestController**를 이용하면서 JSON으로 반환해 뷰랜더링의 후 처리 작업이 필요 없게 됨

⇒ **AOP, 커스텀 에러 핸들러, 필터**를 이용해서 요청/응답을 가로채서 수행하는 방식 이용