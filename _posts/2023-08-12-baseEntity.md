---
layout: post
title: Spring BaseEntity
categories: spring
tags: [jpa, BaseEntity, spring, study]
---
![spring](https://spring.io/img/og-spring.png)

## DATABASE

entity는 업무에 필요한 데이터를 저장하고 관리하는 테이블을 말합니다.

속성은 entity의 한 부분으로 업무에 필요한 최소 값의 단위로 테이블의 칼럼을 말합니다.

도메인은 속성의 값, 타입, 제약사항 등에 대한 범위를 의미합니다.

Java에서 ORM 기술인 JPA를 사용하면, 도메인을 관계형 데이터베이스 테이블에 매핑할 때 공통으로 가지는 컬럼들이 있습니다. 해당 칼럼으로는 생성일자, 수정일자, 식별자 등이 있습니다.



## JPA

Java에서 ORM 기술인 JPA를 사용하면, 도메인을 관계형 데이터베이스 테이블에 매핑할 때 공통으로 가지는 컬럼들이 있습니다. 해당 칼럼으로는 생성일자, 수정일자, 식별자 등이 있습니다.



## @MappedSuperclass

코딩에서는 '반복'을 최대한 피하기에 객체의 입장에서 공통 매핑 정보가 필요할 때 사용합니다.

**DB 테이블과 상관 없이 객체 입장**에서 name이나 id 등 공통 정보는 부모 클래스에 선언하고, 속성만 상속 받아서 사용하고 싶은 경우에 사용을 하며, 매핑정보만 상속받는 Superclass라는 의미입니다.

직접 생성해서 사용할 일이 없으므로 추상 클래스로 만들어서 사용합니다.

> @Entity가 있는 클래스는 @Entity나 @MappedSuperclass로 지정한 클래스만 상속할 수 있습니다.



## JPA Auditing

Spring Data JPA에 있는 **Audit** 라는 시간에 대해서 자동으로 값을 넣어주는 기능을 사용합니다. 해당 기능은 영속성 컨텍스트가 생성 또는 변경되는 경우에 따라 자동으로 시간을 매핑해서 데이터베이스에 넣어주게 됩니다.

### 설명
1.  해당 기능은 우선 build.gradle 파일에 spring-boot-starter-data-jpa를 추가하는 것으로 사용할 수 있게 됩니다.
2.  Application 파일에 @EnableJpaAuditing의 추가를 통해 Auditing 기능을 활성화시킬 수 있습니다.
3.  공통적인 컬럼을 처리할 클래스(BaseEntity)에 @EntityListeners(AuditingEntityListener.class)를 추가합니다.
4.  해당 클래스 변수에서는  @CreatedDate, @LastModifiedDate를 통해서 entity가 저장될 때, 시간이 자동 저장합니다.

### 코드
```java
  import java.time.LocalDateTime;
  
  @EntityListeners(AuditingEntityListener.class)
  @MappedSuperclass
  @Getter
  @Setter
  @DynamicInsert
  @DiscriminatorColumn
  @NoArgsConstructor
  public abstract class BaseEntity {
      @CreatedDate
      @Column(updatable = false, name = "create_at", nullable = false)
      @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss")
      private LocalDateTime createAt;
  
      @LastModifiedDate
      @Column(name = "update_at")
      @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd HH:mm:ss")
      private LocalDateTime updateAt;
  
      @Column(name = "status", columnDefinition = "int default 1")
      private int status = 1; // 상속관계 자동 매핑으로 자동으로 insert됨
  
      protected void delete() {
          this.status = 0;
      }
  
      // 삭제된 데이터 복구
      protected void restore() {
          this.status = 1;
      }
  }
```

### 상세 분석

@NoArgsConstructor : 자동으로 기본 생성자를 생성합니다.

@JsonFormat : JSON 응답값의 형식을 지정하며 날짜, 형식, 키 설정, 값 포함 여부, 응답 값 순서 등 여러 형태가 있습니다. 
기본적인 날짜 형식 지정으로는 @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "Asia/Seoul")가 있습니다.

@DynamicInsert / @DynamicUpdate : 삽입 또는 수정 쿼리문을 동적을 만드는 방식으로 null인 값은 제외하고(**불필요한 DB 부하 감소**) 쿼리문이 생성됩니다.

@DiscriminatorColumn(name="DTYPE") : 부모클래스에 선언하며 하위 클래스를 구분하는 용도입니다.

@DiscriminatorValue("XXX") : 하위 클래스에 선언하여, entity를 저장할 때 슈퍼타입의 구분 컬럼에 저장할 값을 지정합니다.(default = 클래스이름)

> 논리 모델을 물리 모델로 구현할 방법 3가지(JOINED, SINGLE_TABLE, TABLE_PER_CLASS)의 적용에 사용하는 @Inheritance(strategy=InheritanceType.XXX)와 함께 사용됩니다.


### 추가 공부

-   칼럼 : 관계형 DB에서 사용 vs 필드 : 파일 시스템에서 사용
