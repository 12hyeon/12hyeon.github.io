---
layout: post
title: Spring 예외 처리
categories: spring
tags: [exception, spring, study]
---
![spring](https://spring.io/img/og-spring.png)


예외 처리는 Spring 애플리케이션에서 중요한 부분 중 하나입니다. 

이 섹션에서는 Spring의 기본 예외 처리 방식과 다양한 예외 처리 방법에 대해 알아보겠습니다.

## Spring 기본 예외 처리

Spring에서는 BasicErrorController가 기본적으로 제공되어 

예외가 발생하면 /error로 에러를 전달하도록 웹 애플리케이션 서버(WAS)에서 설정되어 있습니다.

## 스프링의 다양한 예외 처리 방법

### 1) ResponseStatusException

```java
    public class Exception {
      public ResponseEntity<Item> getItem(@PathVariable String id) {
          try {
              return ResponseEntity.ok(productService.getItem(id));
          } catch (NoSuchElementException e) {
              throw new ResponseStatusException(HttpStatus.NOT_FOUND, "Item Not Found");
          }
      }
    }
```
장점: 기본 예외를 빠르게 처리하고, 예외 클래스와의 결합도를 낮출 수 있습니다.

한계: 일관된 예외 처리가 어렵고, 예외 요청이 WAS까지 전달됩니다.

### 2) @ExceptionHandler

```java
  public class ItemController {
  
    @ExceptionHandler(NoSuchElementException.class)
    public ResponseEntity<String> handleNoSuchElementException(NoSuchElementException exception) {
      return ResponseEntity.status(HttpStatus.NOT_FOUND).body(exception.getMessage());
    }
  }
```

장점: 에러 응답을 자유롭게 다룰 수 있으며(상태 코드, 메세지, 에러 등), 예외 처리 코드가 중복될 가능성이 있습니다.

사용 방식
- 내부적으로 정의한 값 대신 HTTP 표준 상태 값을 사용하는 것이 클라이언트와 유지보수 측면에서 좋습니다.
- @ExceptionHandler에 등록된 예외 클래스와 메서드의 파라미터 클래스가 일치해야 합니다.
- 구체적인 예외 핸들러를 찾고, 없으면 부모 예외 핸들러를 찾습니다.

### 3) @RestControllerAdvice (Spring 4.3 이상)

```java
  public abstract class ResponseEntityExceptionHandler {
  
    protected ResponseEntity<Object> handleExceptionInternal(
      Exception ex, @Nullable Object body, HttpHeaders headers, HttpStatus status, WebRequest request){
  
    }
  }
  
  @RestControllerAdvice // 전역 예외 처리
  public class GlobalExceptionHandler extends ResponseEntityExceptionHandler {
  
    @ExceptionHandler(NotFoundAccountException.class)
    public ResponseEntity<?> handleNotFoundEntity(NotFoundException e) {
      final ErrorResponse errorResponse = ErrorResponse.builder()
        .code("Item Not Found")
        .message(e.getMessage()).build();
  
      return ResponseEntity.status(HttpStatus.NOT_FOUND).body(errorResponse);
    }
  
    // throw new RestApiException(ErrorCode.BAD_REQUEST)에 의해 호출
    @ExceptionHandler(RestApiException.class)
    protected ResponseEntity<ErrorResponse> handleCustomException(RestApiException ex) {
      ErrorCode errorCode = ex.getErrorCode();
      return handleExceptionInternal(errorCode);
    }
  }
```

RestControllerAdvice를 사용하면 전역 예외 처리로 에러 응답을 일관성 있게 처리할 수 있으며, 응답을 JSON 형식으로 반환합니다.

여러 ControllerAdvice가 있는 경우 @Order 어노테이션으로 순서를 지정할 수 있습니다.

## 결론

최종적으로 RestControllerAdvice를 사용하는 것이 가장 좋은 방식임을 확인할 수 있습니다.

---
참고 자료

- https://mangkyu.tistory.com/204
- https://velog.io/@u-nij/Spring-%EC%A0%84%EC%97%AD-%EC%98%88%EC%99%B8-%EC%B2%98%EB%A6%AC-RestControllerAdivce-%EC%A0%81%EC%9A%A9
