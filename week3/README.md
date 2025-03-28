## 홈서버를 활용한 CI/CD

- 기존에 프로젝트를 진행할 때 구축했던 CI/CD
    - GitHub Actions 사용
    - AWS EC2에 접속해서 self-hosted runner 설정
    - action이 트리거 되면 self-hosted runner로 등록해둔 서버 환경에서 CI/CD 작동

홈서버도 EC2와 동일한 서버이기 때문에 CI/CD를 구축하는데 다른 점은 없다.
홈서버를 구축한만큼 홈서버를 self-hosted runner로 등록해서 CI/CD를 적용해본다.

</br>

## How to

GitHub repo > Settings > Actions/Runners 에서 아키텍처에 맞는 가이드 따라 설치

</br>

### 서버에 runner 패키지 설치하기

```bash
# Create a folder
$ mkdir actions-runner && cd actions-runner

# Download the latest runner package
$ curl -o actions-runner-osx-x64-2.323.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.323.0/actions-runner-osx-x64-2.323.0.tar.gz

# Optional: Validate the hash
# 파일이 손상되지 않았는지 SHA-256 해시 값을 검증하는 과정
# 해시 값이 일치하지 않으면 파일이 손상되었을 가능성
$ echo "5dd3f423e8f387a47ac53a5e355e0fe105f0a9314d7823dea098dca70e1bd2c9  actions-runner-osx-x64-2.323.0.tar.gz" | shasum -a 256 -c

# Extract the installer
# tar는 파일을 압축하거나 압축을 푸는 명령어
$ tar xzf ./actions-runner-osx-x64-2.323.0.tar.gz
```

### config.sh 실행

### run.sh 실행

### svc.sh

```bash
$ sudo ./svc.sh install
$ sudo ./svc.sh start
```

<img width="567" alt="image (5)" src="https://github.com/user-attachments/assets/2f910ac3-cffa-431d-8395-62a580dbe522" />

<img width="567" alt="image (6)" src="https://github.com/user-attachments/assets/7d9d3731-4383-4c42-8bf6-feb74a4d6ea8" />

</br>

## GitHub Actions Workflow

```yaml
name: CI - Debug

run-name: Debug - ${{ github.ref_name || github.ref }}

on:
  workflow_dispatch: {}

jobs:
  ci_debugger:
    runs-on:
      - X64

    steps:
      - name: Template
        run: |
          echo "Hello world!"

      - name: Set ssh pub key
        run: |
          curl https://github.com/${{github.actor}}.keys > ~/.ssh/temp_keys
          cp -rf ~/.ssh/temp_keys ~/.ssh/authorized_keys
          ls -al ~/.ssh

      - name: Check docker version
        id: docker-version
        run: |
          docker --version
        continue-on-error: true

      - name: Install Docker (if failed)
        if: failure()
        run: |
          curl -fsSL https://get.docker.com | sh
```

- main 브랜치에 push 되었을 때
    - Hello world! 출력하고
    - ssh key 세팅하고
    - docker (없으면) 설치하도록 작성
 

<img width="567" alt="image" src="https://github.com/user-attachments/assets/4f50790e-9f9f-4bb8-bfe9-4585c7e9410e" />

트리거되면 CI/CD가 잘 동작하는 것을 확인할 수 있다.
