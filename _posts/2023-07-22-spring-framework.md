---
layout: post
title: Spring Framework 핵심
categories: spring
tags: [concept, spring, study]
---
![spring](https://spring.io/img/og-spring.png)

Spring 프레임워크는 자바 어플리케이션 개발을 위한 강력한 도구로, 애플리케이션의 구조를 제공해줍니다. 이 글에서는 Spring의 핵심 컨셉과 프로그래밍 모델에 대해 알아보겠습니다.

## 프레임워크 vs 라이브러리

프레임워크와 라이브러리는 제어 흐름의 권한이 어디에 있는지에 따라 다릅니다.

- **프레임워크**: 전체적인 흐름을 가지고 있으며 사용자가 정해진 방식으로 기능을 구현합니다. Spring, Django, Flask, Angular, Vue.js 등이 이에 해당합니다.
- **라이브러리**: 사용자가 전체 흐름을 만들며, 라이브러리를 가져와서 필요한 곳에서 사용합니다.



## 스프링 컨테이너

Spring의 핵심 컴포넌트 중 하나는 스프링 컨테이너입니다. 이 컨테이너는 설정 정보를 참고하여 어플리케이션을 구성하는 객체를 생성하고 관리합니다.



## 스프링의 프로그래밍 모델

### 1. IoC / DI (Inversion of Control / Dependency Injection)

#### 1-1. Inversion of Control (제어의 역전)

- 사용자는 스프링 컨테이너가 생성한 객체를 제공받는 입장입니다.
- 스프링 컨테이너는 제어의 역전을 관리하며, Bean factory가 Bean을 관리하고, 이를 상속받아 확장한 Application Context를 사용합니다.

#### 1-2. Dependency Injection (의존성 주입)

객체의 has-a 관계의 강한 결합을 풀어주는 방법으로, 주로 생성자 주입이 사용됩니다.

---

### 2. PSA (Portable Service Abstraction)

PSA는 특정한 환경과 기술에 종속되지 않고 유연한 어플리케이션을 생성할 수 있도록 도와줍니다.

PSA의 대표적인 예시는 트랜잭션 추상화입니다.

---

### 3. AOP (Aspect-Oriented Programming)

AOP는 어플리케이션의 핵심 비즈니스 로직과 부가 기능 로직을 분리하여 독립적인 실행과 코드 재사용을 위한 기술입니다.

```java
  public class CalculatorAspect implements MethodInterceptor {
      @Override
      public Object invoke(MethodInvocation method) {
          // 보조 업무 설정
          Log log = LogFactory.getLog(this.getClass());
          log.info("시~작! ------------------------- ");
          
          // 주 업무(core concern) 호출 부분~!!
          Object core = method.proceed();
  
          log.info("끄~읕! ------------------------- ");
  
          return core;    
      }
  }
```
