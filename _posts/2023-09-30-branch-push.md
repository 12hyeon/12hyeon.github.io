---
layout: post
title: 특정 원격 브랜치로 push
categories: git
tags: [git, commit, push, study]
---
![git](https://www.20i.com/blog/wp-content/uploads/2022/08/git-blog-header.png)

## commit의 브랜치 이동

실수로 main에 commit을 진행하고 있던 작업을 friends 브랜치로 이동시키는 작업을 예시로 설명드리겠습니다.

### git checkout 이동할 브랜치

1. 현재 main 브랜치에서 우선 friends 브랜치로 이동합니다.

![브랜치 이동](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbdVgEA%2Fbtsht1AGAgO%2FYmHBkVSU7vnTQT35idxhVK%2Fimg.png)

Problem : checkout을 위해서는 application.properties 파일에 대해서 overwritten이 발생하는 문제가 있다고 합니다. 

해당 문제를 해결하기 위해서 저는 test용으로 수정해두었기에 이전 버전과 동일한 상태로 파일을 만들어서 해당 문제를 해결하였습니다.

### git reflog

2. git reflog 명령을 통해서 가져올 commit 목록을 확인합니다.

![git reflog](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FujdqT%2Fbtshp7ibxGu%2FkfKKC8OBTlKHjHVgre0dAK%2Fimg.png)

저는 이중에서 가장 아래 commit을 이동시켜보도록 하겠습니다.

### git cherry-pick \[id\]

3. 다음은 현재 브랜치로 commit을 가져올 타이밍입니다.

![cherry-pick](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fpv6f5%2Fbtshp3z48sr%2FpljOwQoJIk57mUzkfdato0%2Fimg.png)

Problem : 이번에도 아직 로컬에 변경사항이 남아있어서 commit 또는 stach 처리를 진행하라고 합니다. 저는 현재까지의 변경사항에 대해 commit을 진행 후 해당 작업을 반복하였습니다.

![cherry-pick error](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FGUd8q%2FbtshlU4OUic%2FiiMB2E9RYWThKXTP8D4Ar0%2Fimg.png)

무사히 cherry-pick에 의해서 main -> friends 브랜치로 commit이 이동한 것을 확인할 수 있습니다

---


## 특정 원격 브랜치로 push

이용하는 방법은 git push origin 이후에 main 브랜치(local)에서 실제 git의 friends 브랜치에 올리겠다는 명령을 통해서 다른 브랜치에서 작업한 것처럼 local 작업을 올릴 수도 있습니다.

![push](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsSaNE%2Fbtshp5kkW7v%2FwtkSYd0lLTbkk2nC0iwBQk%2Fimg.png)

위 작업을 진행하면, github repository에서는 다음의 내용을 확인할 수 있습니다.

![repository](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fd5Ac8O%2Fbtshp7CvKiC%2Fo3gpukxfoCCjJ0eI8SiKu0%2Fimg.png)

해당 작업에서 pull request 버튼을 통해서 해당 작업을 실제 프로젝트 code에 적용할지 여부를 확인 받기 위해 PR을 보낼 수 있게 됩니다.

---

### 후기

해당 작업 중에서 처음보는 오류가 많았지만, 잘 읽고 찾아보니 기존에 존재하는 파일이 add 혹은 commit이 진행되지 않은 상태로 남아있다보니 발생한 문제가 많았던 것 같습니다.

이러한 부분을 유의하여 작업을 하다보면, 더 속도가 빨라질 것 같는 생각이 듭니다.

또는 아래의 방법으로 local에서는 main에서 작업했어도 git에는 특정 원격 브랜치에 올리는 방법을 이용하시는 것도 좋습니다.

local에서 작업을 하다보니, 자주 발생하는 문제였기에 이렇게 정리해봄으로써 git을 더 유용하게 사용할 수 있게 되는 좋은 시간이였다는 생각이 듭니다.
