---
layout: post
title: Commit 메시지 형식
categories: git
tags: [git, commit, study]
---
![git](https://www.20i.com/blog/wp-content/uploads/2022/08/git-blog-header.png)

git에서 commit를 하는 경우에는 일반적으로 정해진 형식이 있다.

## 기본 구조

**제목/본문/꼬리말** 세가지 파트로 나누고, 각 파트는 한줄씩 건너서 구분합니다.

> type: Subject
>
> body
>
> footer

Subject : 50자 이하면서 대문자로 시작하고, 마침표로 끝나지 말아야합니다.
> feat: Summarize changes in around 50 characters or less

Body : 72자 이내로 커밋의 내용과 이유를 설명해 필요한 경우만 사용됩니다.

Footer : 선택사항으로 문제 ID 참조에 사용됩니다.
>Resolves: #123

## Commit Type

-   feat: 새로운 기능 추가
-   fix: 버그 수정
-   docs: 문서 수정
-   style: 코드 포맷팅, 세미콜론 누락, 코드 변경이 없는 경우
-   refactor: 코드 리펙토링
-   test: 테스트 코드, 리펙토링 테스트 코드 추가
-   chore: 빌드 업무 수정, 패키지 매니저 수정

---

참고 자료

- [git 스타일 가이드](https://udacity.github.io/git-styleguide/)
