---
layout: post
title: Git Action CI 구성
categories: project
tags: [tasty-spot-finder, git-action, ci]
---
![git action](https://miro.medium.com/v2/resize:fit:1075/1*VtWbCHhIw6MMMXCil7lZ0g.png)

프로젝트의 빌드 및 테스트를 위한  GitHub Actions 설정을 진행하였습니다.
참고로, 해당 yaml에서 사용한 변수들은 git에서 따로 각 값을 입력해둔 상태로 활용한 것 입니다.

## Workflow 설명

### `on` 설정
GitHub Actions Workflow는 특정 이벤트 발생 시에만 실행되도록 설정됩니다.

- `pull_request`: main 또는 dev 브랜치로 올라온 Pull Request일 때 Workflow가 실행됩니다. 이는 코드 리뷰와 통합을 위한 PR이 발생할 때만 빌드를 수행하도록 하는데 유용합니다.
- `push`: main 또는 dev 브랜치로 직접 푸시할 때 Workflow가 실행됩니다. 직접 푸시할 때도 빌드를 수행해 원하는 브랜치에 변경사항이 반영되도록 합니다.

### `permissions` 설정
Workflow에서 필요한 리소스에 대한 접근 권한을 설정합니다. 이 경우 `contents` 권한을 `read`로 설정하여 리소스의 내용을 읽을 수 있도록 합니다.

### `env` 설정
환경 변수를 설정합니다. 주로 AWS와 관련된 정보 및 빌드 설정과 관련된 변수들을 지정합니다.

### `build` 작업
빌드 작업을 정의합니다. 이 작업은 Ubuntu 환경에서 실행되도록 설정되어 있습니다.

### `steps` 정의

---


## step 별 설명

### 1. PR 및 Branch 확인

GitHub 이벤트의 유형과 브랜치를 확인하여 올바른 상황에서 Workflow가 실행되도록 합니다.

```yaml
      - name: PR 및 Branch 확인
        run: |
          if [[ "${{ github.event_name }}" == 'pull_request' ]]; then
            if [[ "${{ github.event.action }}" == 'opened' || "${{ github.event.action }}" == 'synchronize' ]]; then
              if [[ "${{ github.event.pull_request.base.ref }}" == 'dev' || "${{ github.event.pull_request.base.ref }}" == 'main' ]]; then
                echo "Branch is allowed.";
              else
                echo "Branch is not allowed.";
                exit 1;
              fi
            else
              echo "Invalid action for pull request.";
              exit 1;
            fi
          elif [[ "${{ github.event_name }}" == 'push' ]]; then
            if [[ "${{ github.event.ref }}" == 'refs/heads/dev' || "${{ github.event.ref }}" == 'refs/heads/main' ]]; then
              echo "Branch is allowed.";
            else
              echo "Branch is not allowed.";
              exit 1;
            fi
          else
            echo "Invalid event.";
            exit 1;
          fi
```

### 2. 체크아웃

소스 코드를 가져옵니다.

```yaml
  - name: 체크아웃
    uses: actions/checkout@v2
```

### 3. JDK 11 설정

Java 개발을 위해 JDK 11을 설정합니다.

```yaml
  - name: JDK 11 세팅
    uses: actions/setup-java@v2
    with:
      java-version: '11'
      distribution: 'adopt'
```

### 4. 실행 권한 부여

Gradle 빌드 스크립트에 대한 실행 권한을 설정합니다.

```yaml
  - name: 실행 권한 부여
    run: chmod +x gradlew
```

### 5. properties 생성

필요한 환경 변수가 담긴 `application.yaml` 파일을 생성합니다.

```yaml
  - name: properties 생성
    run: |
      mkdir -p ./src/main/resources/
      echo "${{ secrets.APPLICATION }}" > ./src/main/resources/application.yaml
      cat ./src/main/resources/application.yaml
```

### 6. 빌드 진행

Gradle을 사용하여 프로젝트를 빌드합니다.

```yaml
  - name: 빌드 진행
    uses: gradle/gradle-build-action@67421db6bd0bf253fb4bd25b31ebb98943c375e1
    with:
      arguments: clean build
      cache-disabled: true
```

## 최종 Workflow 파일 (ci.yaml)

## Workflow 실행 흐름

1. PR이 오픈되거나 synchronize 될 때 또는 main 또는 dev 브랜치로 직접 푸시할 때 Workflow가 실행됩니다.
2. PR인 경우, 올바른 브랜치인지 확인하고, push인 경우 브랜치 확인합니다.
3. 필요한 작업을 수행하며, 환경 변수 및 빌드 설정을 적용합니다.
4. 빌드가 성공하면 Workflow가 완료됩니다.

## 전체 CI Workflow

```yaml
  name: 프로젝트 빌드 테스트
  
  on:
    pull_request:
      branches: [ "main", "dev" ]
    push:
      branches: [ "dev" ]
  
  permissions:
    contents: read
  
  env:
    AWS_REGION: ap-northeast-2
    S3_BUCKET_NAME: private-hyeon-bucket
    CODE_DEPLOY_APPLICATION_NAME: CICD_TEST_CD
    CODE_DEPLOY_DEPLOYMENT_GROUP_NAME: CICD-TEST-GROUP
  
  jobs:
    build:
      runs-on: ubuntu-latest
      steps:
        - name: PR 및 Branch 확인
          run: |
            if [[ "${{ github.event_name }}" == 'pull_request' ]]; then
              if [[ "${{ github.event.action }}" == 'opened' || "${{ github.event.action }}" == 'synchronize' ]]; then
                if [[ "${{ github.event.pull_request.base.ref }}" == 'dev' || "${{ github.event.pull_request.base.ref }}" == 'main' ]]; then
                  echo "Branch is allowed.";
                else
                  echo "Branch is not allowed.";
                  exit 1;
                fi
              else
                echo "Invalid action for pull request.";
                exit 1;
              fi
            elif [[ "${{ github.event_name }}" == 'push' ]]; then
              if [[ "${{ github.event.ref }}" == 'refs/heads/dev' || "${{ github.event.ref }}" == 'refs/heads/main' ]]; then
                echo "Branch is allowed.";
              else
                echo "Branch is not allowed.";
                exit 1;
              fi
            else
              echo "Invalid event.";
              exit 1;
            fi
  
        - name: 체크아웃
          uses: actions/checkout@v2
  
        - name: JDK 11 세팅
          uses: actions/setup-java@v2
          with:
            java-version: '11'
            distribution: 'adopt'
  
        - name: 실행 권한 부여
          run: chmod +x gradlew
  
        - name: properties 생성
          run: |
            mkdir -p ./src/main/resources/
            echo "${{ secrets.APPLICATION }}" > ./src/main/resources/application.yaml
            cat ./src/main/resources/application.yaml
  
        - name: 빌드 진행
          uses: gradle/gradle-build-action@67421db6bd0bf253fb4bd25b31ebb98943c375e1
          with:
            arguments: clean build
            cache-disabled: true
```

이렇게 설정된 GitHub Actions Workflow는 프로젝트의 빌드를 효율적으로 관리하고, 특정 브랜치에 대한 제약을 설정하여 안정적인 CI/CD를 구축하는 결과를 가져오게 됩니다.


### 후기
초기에는 설정하는 내용에 대해서 복잡했지만, 하나씩 과정을 이해하다보니 어떤 부분이 왜 step으로써 필요한지 이해할 수 있었습니다.
