---
layout: post
title: Spring Data JPA
categories: spring
tags: [jpa, spring, study]
---
![spring](https://spring.io/img/og-spring.png)

## 정렬 / Page<?> / Slice<?>

반환 값을 Page로 설정하면, total 쿼리를 자동으로 실행합니다.

Slice는 전체 데이터의 개수를 가져오지 않고, 추가로 더 1개가 있는지 확인합니다.

Spring Data를 통해 반환 값을 변경하는 것만으로 Slice를 Page 형태로 변환할 수 있습니다.


### 페이징 쿼리

Total count가 많을수록 성능이 저하됩니다.

카운트 쿼리도 복잡하지 않다면, countQuery를 분리해서 성능을 개선할 수 있습니다.


### 코드

```java
  class MemberServiceImpl {
  
      public void test() {
  
        PageRequest pageRequest = PageRequest.of(0, 3, Sort.by(Sort.Direction.DESC, "username"));
        Page<Member> page = memberRepository.findByAge(10, pageRequest);
  
        List<Member> content = page.getContent(); // 조회된 데이터
        assertThat(content.size()).isEqualTo(3); // 조회된 데이터 수 
        assertThat(page.getTotalElements()).isEqualTo(5); // 전체 데이터 수 
        assertThat(page.getNumber()).isEqualTo(0); // 페이지 번호 
        assertThat(page.getTotalPages()).isEqualTo(2); // 전체 페이지 번호 
        assertThat(page.isFirst()).isTrue(); // 첫번째 항목인가? 
        assertThat(page.hasNext()).isTrue(); // 다음 페이지가 있는가?
        
        // API로 전송시킬 수 있는 형태로 변환
        Page<MemberDto> dtoPage = page.map(m -> new MemberDto());
  
      }
  }
```

## 벌크성 수정 쿼리

영속성 컨텍스트를 무시하고 DB에 즉시 적용합니다. 아직 1차 캐시에 반영되지 않은 상태입니다.

영속성 컨텍스트를 제거하려면, em.flush() → em.clear() 또는 clearAutomatically = true로 설정합니다.

```java
  @Repository
  public interface MemberRepository extends JpaRepository<User, Long> {
    
      @Modifying(clearAutomatically = true) 
      @Query("update Member m set m.age = m.age + 1 where m.age >= :age") 
      int bulkAgePlus(@Param("age") int age);
  
  }
```

## 지연로딩 → N+1 문제

실제 연관 관계의 속성에 접근하지 않으면, join 쿼리를 실행하지 않습니다.

Fetch join을 사용하면 연관된 모든 것을 가져옵니다.

```java
  @Repository
  public interface MemberRepository extends JpaRepository<User, Long> {

  @EntityGraph(attributePaths = {"team"})
  List<Member> findAll();
  
  }
```

## 결론 : 성능 최적화

1. 정렬에 Page 사용
2. countQuery를 분리
3. 이름 수정 → 벌크성 쿼리
4. 지연 로딩 모든 연관관계에 적용
