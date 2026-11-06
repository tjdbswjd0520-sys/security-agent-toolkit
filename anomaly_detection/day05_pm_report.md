# 4과목 Day 5 도커 컴포즈로 서버 두 대 켜기 · 웹 서버 로그 만들기 · 오후 (이름 · 2026-11-06(금))

> **쓰는 법**
> - 문제를 풀 때마다 그 문제 제목 아래 질문에 답합니다.
> - 질문마다 `쓸 내용:` 에 무엇을 쓰면 되는지 적혀 있습니다. 그대로 `답:` 뒤에 씁니다.
> - 출력이나 코드를 붙여 넣는 질문은 `답:` 아래 빈 코드 상자에 붙여 넣고, 설명은 `답:` 뒤에 씁니다.
> - 「왜」를 묻는 질문은 한 단어로 끝내지 말고, 내 화면에서 본 값이나 배운 내용을 근거로 이유까지 씁니다.
> - 🔍 는 이 교시에서 꼭 알아야 할 핵심 질문입니다. 답을 못 찾았으면 「모름 — 막힌 곳: …」 이라고 씁니다. 🔍 가 없는 질문도 못 적으면 같은 말을 씁니다.

---

## 5교시 · 도커 복습 · 컴포즈로 서버 두 대 한 번에 켜기

### 문제 5-1 · 서버 한 대를 복습하고 컴포즈로 두 대 켜기

- (가) 5교시 순서의 서버 켜기 · 확인 명령(`docker images` 부터 `curl` 까지)과 그 결과를 친 순서대로 붙여 넣고, 서비스 이름 두 개와 각 서비스의 포트를 적어 주세요.
  - 쓸 내용: ① 명령과 결과를 친 순서대로(코드 상자에 붙여 넣기): `docker images` · `docker run` · `docker ps` · `docker stop` · `docker compose up -d` · `docker compose ps` · `curl` 두 개 (`docker pull` 은 받았을 때만, `pwd` · `touch` · `ls` 는 붙이지 않아도 됩니다) ② 서비스 이름 두 개 ③ 각 서비스의 포트
  - 답:

```
User@DESKTOP-OF0CMDB MINGW64 ~/security-agent-toolkit/anomaly_detection (main)
$ docker compose up -d
[+] up 3/3
 ✔ Network anomaly_detection_default  Created     0.0s
 ✔ Container anomaly_detection-web2-1 Started     0.5s
 ✔ Container anomaly_detection-web1-1 Started     0.3s
(.venv) 
User@DESKTOP-OF0CMDB MINGW64 ~/security-agent-toolkit/anomaly_detection (main)
$ docker compose ps
NAME                       IMAGE        COMMAND            SERVICE   CREATED          STATUS PORTS
anomaly_detection-web1-1   nginx:1.28   "/docker-entrypoint.…"   web1      21 seconds ago   Up 20 seconds0.0.0.0:8080->80/tcp, [::]:8080->80/tcp
anomaly_detection-web2-1   nginx:1.28   "/docker-entrypoint.…"   web2      21 seconds ago   Up 20 seconds0.0.0.0:8081->80/tcp, [::]:8081->80/tcp
(.venv) 
User@DESKTOP-OF0CMDB MINGW64 ~/security-agent-toolkit/anomaly_detection (main)
$ curl -s -o /dev/null -w "%{http_code}\n" http://localhost:8080
200
(.venv) 
User@DESKTOP-OF0CMDB MINGW64 ~/security-agent-toolkit/anomaly_detection (main)
$ curl -s -o /dev/null -w "%{http_code}\n" http://localhost:8081
200

```
② web1, web2 ③ 8080, 8081

- (나) 🔍 서버 두 대를 `docker run` 으로 켜는 것과 `compose.yaml` 과 `docker compose up -d` 로 켜는 것을 비교해, 컴포즈가 더 나은 점을 두 가지 적어 주세요.
  - 쓸 내용: ① 컴포즈가 더 나은 점 두 가지
  - 답: 일일이 서버 이름과 커맨드를 입력하지 않고 한 문장으로 두 서버를 켤 수 있다. 다른 사람도 같은 구성으로 서버를 켤 수 있다.

- (다) 🔍 보안 사고가 난 뒤 감사팀이 「사고 당시와 같은 구성으로 서버를 다시 켜 보라」고 요청했습니다. 팀원 한 명은 3주 전에 긴 `docker run` 명령을 직접 쳐서 서버를 켰는데 그 명령을 기억하지 못합니다. 다른 팀은 `compose.yaml` 을 깃허브에 올려 두었습니다. 두 팀이 같은 구성을 다시 켤 수 있는지 비교하고, 파일에 `nginx:1.28` 처럼 버전을 적어 두는 것이 이런 재현에 왜 중요한지 적어 주세요.
  - 쓸 내용: ① 두 팀의 차이 ② 버전을 적어 두는 것이 재현에 중요한 이유
  - 답:① 커맨드로 킨 팀은 같은 구성을 다시 켤 수 없지만 깃허브에 올려 둔 팀은 켤 수 있다. ② 버전을 적어 두어야 문제가 생겼을 때 되돌아가거나 재현할 수 있기 때문이다.

- (라) 🔍 팀원 세 명이 같은 `compose.yaml` 로 서버를 켰는데 한 명의 PC 에서만 `8081` 번 포트를 다른 프로그램이 쓰고 있어서 `web2` 가 켜지지 않았습니다. 이 사람이 파일의 어느 줄을 어떻게 바꾸면 되는지 적고, 바꿔도 서버 안에서는 여전히 `80` 번 포트로 동작하는 이유를 적어 주세요. 이 사람이 `curl` 로 확인할 때 달라지는 점도 적어 주세요.
  - 쓸 내용: ① 바꿀 줄과 바꾼 모양 ② 서버 안은 그대로 80 인 이유 ③ curl 주소의 달라지는 점
  - 답:① web2: ports: "8081:80"  를 web2: ports: "8082:80" 로 바꾼다.           ② 보내는 요청은 바뀌지 않았기 때문에 서버 안에서는 여전히 80번 포트로 동작한다. ③ 마지막 숫자가 8081에서 8082로 바뀐다.

## 6교시 · 서버마다 접속 범위 정하기 · 설정 바꿔 적용하기

### 문제 6-1 · 서버마다 접속 범위를 정해 컴포즈 파일로 적용하기

- (가) 1번과 5번의 `docker compose ps` 출력을 붙여 넣고, 접속 범위가 바뀐 기존 서비스와 새로 추가된 서비스를 따로 적어 주세요.
  - 쓸 내용: ① 1번 출력(코드 상자에 붙여 넣기) ② 5번 출력(코드 상자에 붙여 넣기) ③ 접속 범위가 바뀐 기존 서비스 ④ 새로 추가된 서비스
  - 답:

```
① 
User@DESKTOP-OF0CMDB MINGW64 ~/security-agent-toolkit/anomaly_detection (main)
$ docker compose ps
NAME                       IMAGE        COMMAND            SERVICE   CREATED          STATUS PORTS
anomaly_detection-web1-1   nginx:1.28   "/docker-entrypoint.…"   web1      16 minutes ago   Up 16 minutes0.0.0.0:8080->80/tcp, [::]:8080->80/tcp
anomaly_detection-web2-1   nginx:1.28   "/docker-entrypoint.…"   web2      16 minutes ago   Up 16 minutes0.0.0.0:8081->80/tcp, [::]:8081->80/tcp
② 
User@DESKTOP-OF0CMDB MINGW64 ~/security-agent-toolkit/anomaly_detection (main)
$ docker compose -f compose.secure.yaml ps
NAME                       IMAGE        COMMAND            SERVICE   CREATED          STATUS PORTS
anomaly_detection-web1-1   nginx:1.28   "/docker-entrypoint.…"   web1      22 minutes ago   Up 22 minutes0.0.0.0:8080->80/tcp, [::]:8080->80/tcp
anomaly_detection-web2-1   nginx:1.28   "/docker-entrypoint.…"   web2      4 minutes ago    Up 10 seconds127.0.0.1:8081->80/tcp
anomaly_detection-web3-1   nginx:1.28   "/docker-entrypoint.…"   web3      4 minutes ago    Up 4 minutes127.0.0.1:8082->80/tcp
```

- (나) 🔍 `"8081:80"` 과 `"127.0.0.1:8081:80"` 은 누가 접속할 수 있는지가 어떻게 다른가요? 관리 화면이나 데이터베이스 서버를 이렇게 설정하는 이유도 적어 주세요.
  - 쓸 내용: ① 두 포트 줄에서 누가 접속할 수 있는지의 차이 ② 관리 화면 · 데이터베이스를 이렇게 설정하는 이유
  - 답: ① "8081:80" : 모든 네트워크,  "127.0.0.1:8081:80" : 자기 자신 ② 공격이 들어올 수 있는 범위를 줄이기 위해서 이다.

- (다) 🔍 `up -d` 의 출력에서 `web1` 은 `Running`, `web2` 는 `Recreated`, `web3` 은 `Created` 였습니다. 서버마다 출력이 다른 이유와, 운영 중인 서비스의 설정을 바꿀 때 이것이 왜 도움이 되는지 적어 주세요.
  - 쓸 내용: ① 서버마다 출력이 다른 이유 ② 서버를 끊지 않고 바꿀 수 있는 것의 쓸모
  - 답: ① up -d 는 파일에 적힌 원하는 상태와 지금 실행 중인 상태를 비교해서 필요한 만큼만 작업합니다.

web1: 설정이 바뀌지 않아서 그대로 둔 것이므로 Running
web2: 포트 설정이 바뀌어서 기존 컨테이너를 지우고 새 설정으로 다시 만든 것이므로 Recreated
web3: 파일에 새로 추가된 서비스라서 처음 만든 것이므로 Created
 ② 한 서버의 설정을 고치면서 다른 서버의 연결을 끊지 않아도 된다.

### 마무리 · 오늘 만든 것을 깃허브에 올리기

- (가) `git status` 를 쳤을 때 `.env` 나 `.venv` 가 보였는지, 보였다면 어떻게 했는지 적어 주세요.
  - 쓸 내용: ① `.env` · `.venv` 가 보였는지 ② 보였다면 한 조치
  - 답: 없다, gitignore를 편집한다.
