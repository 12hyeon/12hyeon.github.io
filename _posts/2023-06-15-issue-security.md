---
layout: post
title: Security 페이지 접근 오류
subtitle: 회원가입 기능 개발 중 발생한 오류 정리
categories: spring
tags: [flavor-spot-finder, security, spring, study]
---
![issue](https://media.licdn.com/dms/image/D4D12AQFTLbZ6lG12cQ/article-cover_image-shrink_720_1280/0/1676142448152?e=2147483647&v=beta&t=MDixC7Dzu4W6CqIfeAZJpCGSwZ3pH2uUATLjxFaRXrk)

## 📌 Security 주의사항 

이전에 자주 사용되던 'WebSecurityConfigurerAdapter'는 Spring Security 5.7.0-M2부터 더이상 사용되지 않습니다. 

대신 Spring Security는 'SecurityFilterChain'을 사용하기를 권장하고 있습니다.

## SecurityFilterChain

WebSecurityConfigurerAdapter의 'webSecurityCustomizer()'와의 차이점

  : `SecurityFilterChain`을 반환하고 빈으로 등록함으로써 컴포넌트 기반의 보안 설정이 가능합니다.

### 1. 문제 발생 원인

`build.gradle`에서 dependencies로 "org.springframework.boot:spring-boot-starter-security"를 사용했을 때

Security에 의해 자동으로 **localhost:8080/login**으로 페이지가 넘어가서 먼저 username과 password를 통해 권한 확인을 받아야 하는 경우

위 구조에서 id = user & password = (서버에 출력되는 password 값)을 사용해도 "**권한이 거부되었다**"는 메시지가 뜨는 경우

![권한 거부 메시지](https://velog.velcdn.com/images/12hyeon/post/325e494f-afcd-4388-a4fa-6e65ad38c924/image.png)

### 2. 문제 해결 방법

![github 이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FEEaNP%2FbtsDSsK2ukl%2FVkJo6S64y1hxs8LRaX35CK%2Fimg.png)


```java
    public class SecurityConfig {
      @Bean
      public SecurityFilterChain securityFilterChain(HttpSecurity httpSecurity) throws Exception {
          return httpSecurity
                  .httpBasic().disable()
                  .csrf().disable()
                  .cors().and()
                  .authorizeRequests()
                  .antMatchers("/api/**").permitAll()
                  .antMatchers("/api/users/new-user", "/api/users").permitAll()
                  .and()
                  .sessionManagement()
                  .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                  .and()
                  .build();
      }
  }
```
