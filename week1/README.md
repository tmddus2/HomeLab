1. 홈서버용 미니 PC ubuntu 설치
2. 홈서버에 tailscale 설치하고 tailscale login

```bash
sudo apt update
sudo apt install curl
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

1. 로컬 노트북에도 tailscale 설치 & 실행

```bash
tailscale up --ssh --accept-routes
```

1. ssh 로그인

```bash
ssh <user>@<tailscale-ip>
```

![image.png](https://github.com/user-attachments/assets/62340c91-0cda-413c-97d0-bad6ac29aa6a)

SSH 접속
