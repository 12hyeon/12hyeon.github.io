---
layout: post
title: 원격 및 로컬 저장소 사용
categories: git
tags: [git, origin, study]
---
![git](https://www.20i.com/blog/wp-content/uploads/2022/08/git-blog-header.png)

## Git 용어

- 푸시 : 로컬의 커밋을 원격에 올리는 것
- 풀 : 원격의 커밋을 로컬에 내려받는 것
- 클론 : 원격저장소의 코드와 버전 전체를 컴퓨터로 내려받는 것

---


## 원격저장소에 커밋 올리기

### 방법 1. github내에서 create repository로 원격 저장소를 생성 & 올리기

```
echo "# test" >> README.md
git init

git add README.md
# 추가할 파일

git commit -m "first commit"
# 파일의 커밋 메시지

git branch -M main
git remote add origin https://github.com/12hyeon/test.git
# 로컬저장소에 원격저장소 주소를 알려줌

git push -u origin main
# 원격 저장소에 올리기
```

### 방법 2. 로컬저장소의 내용을 기본저장소에 올리기

```
git remote add origin https://github.com/12hyeon/test.git
git branch -M main
git push -u origin main
```

---


## 원격 저장소의 커밋을 로컬에 내려받기

파일을 내려받을 폴더내에서 '### Git Bash Here### '(마우스 오른쪽) 버튼을 누르면, 해당 폴더 경로의 git 환경에 들어가게 됩니다.

그 후에 'https://github.com/~.git' 부분에 해당하는 자신의 원격저장소의 url을 넣어 입력하면 됩니다.

```
$ git clone https://github.com/~.git .
```

마지막에 한 칸 띄고 찍은 점은 현재 폴더에 원격저장소 내용을 다운 받겠다는 의미입니다.

---


## 원격 저장소의 새로운 커밋을 로컬에 가져오기

```
$ git pull orign master
```
