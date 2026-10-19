# Day 6 Docker ① · nginx 서버 · 오후 (이름 · 2026-10-19)

> **쓰는 법**
> - 문제를 하나 풀 때마다 그 문제 제목 아래를 채웁니다.
> - 질문마다 `쓸 내용:` 에 **무엇을 쓰면 되는지** 적혀 있습니다. 그 내용대로 `답:` 뒤에 씁니다.
> - 「왜 그럴까요?」 같은 질문은 한 단어로 끝내지 말고, 내 화면에서 본 값이나 배운 내용을 근거로 이유까지 씁니다.
> - 🔍 는 그 교시에 꼭 알아야 할 핵심 질문입니다. 답을 못 찾았으면 「모름 — 막힌 곳: …」 이라고 씁니다.

---

## 5교시 · 왜 도커인가 · 이미지와 컨테이너 · 같이 설치하기

### 문제 5-1 · 설치를 확인하고 첫 컨테이너 실행하기

- (가) `docker --version` 으로 나온 값을 적어 주세요.
  - 쓸 내용: 「Docker version … build …」 한 줄
  - 답:Docker version 29.8.2, build 7fc2dff
- (나) 🔍 `docker run hello-world` 출력이 말하는 일의 순서를 5.5 그림의 번호(① ~ ④)에 맞춰 설명해 주세요.
  - 쓸 내용: ① ~ ④ 번호마다 한 일 (클라이언트 · 엔진 · Docker Hub · 컨테이너)
  - 답:
  ① 클라이언트(Client): 사용자가 터미널에서 docker run hello-world 명령어를 입력하면 Docker 클라이언트가 명령을 Docker 엔진에 전달한다.

② 엔진(Engine): Docker 엔진은 로컬에 hello-world 이미지가 있는지 확인하고, 없으면 Docker Hub에서 이미지를 가져온다.

③ Docker Hub: hello-world 이미지를 Docker 엔진에 제공한다. 엔진은 이미지를 다운로드한다.

④ 컨테이너(Container): Docker 엔진이 이미지로 컨테이너를 생성하고 실행한다. 컨테이너가 Hello from Docker! 메시지를 출력하고 종료한다.

- (다) 🔍 `docker images` 에 보인 것과 `docker ps -a` 에 보인 것은 각각 이미지와 컨테이너 중 무엇이고, 어떻게 다른가요?
  - 쓸 내용: ① images 에 보인 것과 그 종류 ② ps -a 에 보인 것과 그 종류 ③ 둘의 차이
  - 답:
  ① docker images에 보이는 것 — 이미지(Image)

Docker에서 사용할 수 있는 이미지 목록이다. 이미지란 컨테이너를 생성하기 위한 템플릿이다. 출력에는 hello-world:latest 이미지가 보인다.

② docker ps -a에 보이는 것 — 컨테이너(Container)

실행 중이거나 실행이 끝난 모든 컨테이너의 목록이다. 출력에는 hello-world 이미지로 만든 컨테이너 2개가 보인다.

③ 둘의 차이

이미지는 컨테이너를 만들기 위한 원본이고, 컨테이너는 이미지로 생성해 실행하는 인스턴스이다. 하나의 이미지로 여러 컨테이너를 만들 수 있으므로, 현재처럼 이미지 하나에 컨테이너 두 개가 존재할 수 있다.

- (라) 🔍 `docker images` 출력을 붙여 넣고, `hello-world` 줄의 `IMAGE` · `ID` · `EXTRA` 값과 각각의 뜻을 적어 주세요.
  - 쓸 내용: ① 출력 ② `IMAGE` · `ID` · `EXTRA` 의 값과 뜻
  - 답:
  IMAGE: hello-world:latest — 이미지 이름은 hello-world, 태그는 latest이다. latest는 해당 이미지에 붙은 태그 이름이다.

ID: 11a3eb1597e8 — 이미지의 고유 식별자(ID) 앞부분이다. 이미지의 종류와 버전을 구별할 때 사용한다.

EXTRA: 제공된 출력에는 EXTRA 열이 표시되어 있지 않으므로 값이 없다. 화면 위쪽의 i Info → U In Use는 안내 표시이며 이미지의 EXTRA 값은 아니다.

```
docker images
            i Info →   U  In Use
IMAGE                ID          
hello-world:latest   11a3eb1597e8

User@DESKTOP-OF0CMDB MINGW64 ~/security-agent-toolkit (main)
$ docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED              STATUS                          PORTS     NAMES
76f29492bc6e   hello-world   "/hello"   29 seconds ago       Exited (0) 27 seconds ago                 quirky_murdock
12f9dbe692af   hello-world   "/hello"   About a minute ago   Exited (0) About a minute ago             frosty_fermi
```

- (마) 🔍 `docker ps -a` 출력을 붙여 넣고, `hello-world` 줄의 `STATUS` · `PORTS` · `NAMES` · `IMAGE` 값과 각각의 뜻을 적어 주세요.
  - 쓸 내용: ① 출력 ② `STATUS` · `PORTS` · `NAMES` · `IMAGE` 의 값과 뜻
  - 답:
  STATUS: Exited (0) — 컨테이너 실행이 종료되었으며, 종료 코드 0은 정상적으로 작업이 끝났음을 의미한다.

PORTS: 값이 비어 있다 — 공개하거나 연결하도록 설정된 포트가 없다는 뜻이다. hello-world는 메시지를 출력하는 프로그램이므로 포트 연결이 필요하지 않다.

NAMES: quirky_murdock, frosty_fermi — Docker가 각 컨테이너에 자동으로 부여한 이름이다.

IMAGE: hello-world — 각 컨테이너를 생성하는 데 사용한 이미지 이름이다.

```
docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED              STATUS                          PORTS     NAMES
76f29492bc6e   hello-world   "/hello"   29 seconds ago       Exited (0) 27 seconds ago                 quirky_murdock
12f9dbe692af   hello-world   "/hello"   About a minute ago   Exited (0) About a minute ago             frosty_fermi
```


### 문제 5-2 · Docker Hub 에서 이미지 이름 읽고 없는 이미지 받아 보기

- (가) `nginx:1.28` 에서 `nginx` 와 `1.28` 이 각각 무엇을 정하는지 적어 주세요.
  - 쓸 내용: ① nginx 가 정하는 것 ② 1.28 이 정하는 것
  - 답:
  nginx: 사용할 Docker 이미지의 이름을 정한다. Nginx는 웹 서버 및 리버스 프록시 소프트웨어이다.
1.28: 사용할 이미지의 버전(태그)을 정한다. 따라서 nginx:1.28은 Nginx의 1.28 버전에 해당하는 이미지를 지정한다.
- (나) 🔍 `docker pull nginxx` 를 쳤을 때 나온 오류 문구와 그 뜻을 적어 주세요.
  - 쓸 내용: ① 오류 문구 ② 뜻 ③ 고치는 방법
  - 답:
  Using default tag: latest
Error response from daemon: pull access denied for nginxx, repository does not exist or may require 'docker login'
해당 저장소를 찾을 수 없어 다운로드에 실패했다는 내용이다. 올바른 이름으로 수정하면 오류를 해결할 수 있다.
- (한 줄) 보안 담당에게 보낼 답을 한 문장으로 적어 주세요.
  - 쓸 내용: 보안 담당이 바로 읽을 수 있는 한 문장(도커를 쓰면 무엇이 같아지는지)
  - 답:
  Docker 이미지를 사용하면 개발·테스트·운영 환경에서 동일한 이미지 버전을 사용해 실행 환경의 차이를 줄이고, 일관된 배포와 보안 관리를 할 수 있습니다.
- 이 교시에서 새로 알게 된 것 하나를 적어 주세요.
  - 쓸 내용: 자유롭게
  - 답:

## 6교시 · nginx 서버 켜고 접속 기록 읽기

### 문제 6-1 · 포트 없이 켜 보고, 포트를 연결해 접속하고, 기록 읽기

- (가) 포트 없이 켠 `web` 에 접속했을 때 어떻게 되었는지 적어 주세요.
  - 쓸 내용: 접속했을 때 나온 화면이나 오류와 `docker ps` 의 PORTS 열
  - 답: 웹페이지에 접속할 수 없었다.
- (나) 🔍 `-p 8080:80` 이 하는 일을 내 PC의 포트와 컨테이너의 포트로 나눠 설명해 주세요.
  - 쓸 내용: ① 내 PC 쪽 포트 ② 컨테이너 쪽 포트 ③ 연결이 하는 일
  - 답:① 8080번 포트로 브라우저의 접속 요청을 받는다. ② 80번 포트로 nginx 웹 서버가 요청을 받는다. ③ 내 pc의 8080번 포트로 들어온 요청을 컨테이너의 80번 포트로 전달한다.
- (다) 🔍 `docker logs web` 출력에서 `GET /` 줄 하나를 붙여 넣고, 접속자 주소 · 시각 · 요청 · 응답 번호를 찾아 적어 주세요.
  - 쓸 내용: ① `GET /` 줄 ② 접속자 주소 · 시각 · 요청 · 응답 번호 네 가지
  - 답:172.17.0.1 - - [19/Oct/2026:06:08:58 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/155.0.0.0 Safari/537.36" "-"
② 172.17.0.1 · 19/Oct/2026:06:08:58 +0000 · GET / HTTP/1.1 · 200
### 문제 6-2 · 컨테이너 안을 들여다보고, 끄고, 지우기

- (가) 컨테이너 안에서 `ls /usr/share/nginx/html` 로 본 파일 이름을 모두 적고, 그중 브라우저의 첫 화면이 어느 파일인지 적어 주세요.
  - 쓸 내용: ① 파일 이름 모두 ② 첫 화면 파일
  - 답:파일 이름은 50x.html, index.html이며, 브라우저의 첫 화면으로 표시되는 파일은 index.html이다.
- (나) 🔍 `docker stop web` 뒤에 `docker ps` 와 `docker ps -a` 에 각각 `web` 이 보이나요? 차이를 설명해 주세요.
  - 쓸 내용: ① stop 뒤 ps 에 보이는지 ② ps -a 에 보이는지 ③ 차이의 이유
  - 답:docker stop web 실행 후 docker ps에는 web이 보이지 않지만, docker ps -a에는 web이 보인다. docker ps는 실행 중인 컨테이너만 표시하고, docker ps -a는 중지된 컨테이너도 표시하기 때문이다. stop은 컨테이너를 중지할 뿐 삭제하지 않는다.
- (다) 🔍 같은 이름으로 다시 `run` 했을 때 나온 오류 문구와, `docker rm` 이 필요한 이유를 적어 주세요.
  - 쓸 내용: ① 오류 문구 ② rm 이 필요한 이유
  - 답:docker run 실행 시 The container name "/web" is already in use라는 오류가 발생했다. 중지된 컨테이너도 이름을 계속 차지하므로, 같은 이름으로 새 컨테이너를 만들려면 docker rm web으로 기존 컨테이너를 삭제해야 한다.
- (한 줄) 보안 담당에게 보낼 답을 한 문장으로 적어 주세요.
  - 쓸 내용: 보안 담당이 바로 읽을 수 있는 한 문장(켜는 명령 · 기록 보는 명령 · 정리 방법)
  - 답:
- 이 교시에서 새로 알게 된 것 하나를 적어 주세요.
  - 쓸 내용: 자유롭게
  - 답:docker run -d --name web -p 8080:80 nginx:1.28 명령으로 웹 서버를 실행하고, docker logs web으로 접속 기록을 확인하며, 사용이 끝나면 docker stop web으로 중지한 뒤 docker rm web으로 컨테이너를 삭제합니다.

## 7교시 · 내 페이지 연결 · 두 서버 · 올리기

### 문제 7-1 · 컨테이너 안의 변경이 사라지는 것을 보고, 내 폴더를 연결하기

- (가) 컨테이너 안에서 바꾼 뒤 `docker rm -f web` 으로 지우고 다시 켰을 때 화면이 어떻게 되었는지 적어 주세요.
  - 쓸 내용: ① 바꾼 뒤 화면 ② 지우고 다시 켠 뒤 화면
  - 답:
  수정한 뒤에는 브라우저에 inside라는 글자가 표시되었다. 이후 docker rm -f web으로 컨테이너를 삭제하고  다시 실행하자, 원래의 Welcome to nginx! 화면이 다시 표시되었다.
- (나) 🔍 컨테이너 안에서 바꾼 내용이 사라진 이유와, `-v` 로 폴더를 연결하면 무엇이 달라지는지 적어 주세요.
  - 쓸 내용: ① 사라진 이유 ② -v 로 연결하면 달라지는 점
  - 답:컨테이너 안에서 수정한 파일은 해당 컨테이너에 저장되므로, 컨테이너를 삭제하면 수정한 내용도 함께 사라진다. 반면 -v로 내 PC의 폴더를 컨테이너 내부 폴더에 연결하면 파일을 내 PC에 보관하면서 컨테이너에서 사용할 수 있다. 따라서 컨테이너를 삭제하고 새로 만들어도 내 PC에 있는 파일은 유지되어 수정한 내용을 계속 사용할 수 있다.
- (다) 🔍 `MSYS_NO_PATHCONV=1` 없이 `-v` 를 실행했을 때 화면에 무엇이 나왔고 왜 그랬는지 적어 주세요.
  - 쓸 내용: ① 화면에 나온 것 ② 그렇게 된 이유
  - 답:브라우저에 Welcome to nginx! 화면이 표시되었다. Windows의 Git Bash가 -v 뒤에 지정한 컨테이너 내부 경로를 Windows 경로로 자동 변환하여 폴더 연결이 잘못되었기 때문이다. MSYS_NO_PATHCONV=1을 명령 앞에 붙이면 이러한 경로 자동 변환을 방지하여 올바르게 연결할 수 있다.

### 문제 7-2 · 서버 두 대를 띄워 포트로 구분하고, 파일을 고쳐 바로 반영되는지 보기

- (가) 두 주소(`8080` · `8081`)에서 보이는 화면을 각각 적어 주세요.
  - 쓸 내용: 두 주소의 화면 문구
  - 답:http://127.0.0.1:8080/ : site_one, 첫 번째 서버입니다 - 문구를 고쳤습니다
  http://127.0.0.1:8080/ : site_two, 두 번째 서버입니다
- (나) 🔍 `docker ps` 의 PORTS 열에서 두 서버를 구분해 주는 것은 무엇인가요?
  - 쓸 내용: PORTS 열에서 서버를 구분하는 값
  - 답: PORTS 열에서 호스트 pc의 포트 번호 8080과 8081로 두 서버를 구분할 수 있다.
- (다) 🔍 `:ro` 는 무슨 뜻이고, 왜 읽기 전용으로 연결하나요?
  - 쓸 내용: ① :ro 의 뜻 ② 읽기 전용으로 연결하는 이유
  - 답:① :ro는 read-only의 약자로, 내 PC의 폴더를 컨테이너에 읽기 전용으로 연결한다는 뜻이다. ② 컨테이너 안의 Nginx가 웹 페이지 파일을 수정하거나 삭제하지 못하게 하여 원본 파일을 보호하고, 불필요한 변경을 방지해 보안을 강화하기 위해 읽기 전용으로 연결한다.
- (한 줄) 보안 담당에게 보낼 답을 한 문장으로 적어 주세요.
  - 쓸 내용: 보안 담당이 바로 읽을 수 있는 한 문장(두 서버를 어떻게 띄웠는지)
  - 답:docker run -d --name web-one -p 8080:80 -v "$(pwd)/site-one:/usr/share/nginx/html:ro" nginx:1.28 명령으로 첫 번째 서버를, docker run -d --name web-two -p 8081:80 -v "$(pwd)/site-two:/usr/share/nginx/html:ro" nginx:1.28 명령으로 두 번째 서버를 실행했으며, 각 서버는 내 PC의 서로 다른 포트(8080, 8081)를 통해 연결되고 내 PC의 웹 파일을 읽기 전용으로 제공한다.
- 이 교시에서 새로 알게 된 것 하나를 적어 주세요.
  - 쓸 내용: 자유롭게
  - 답:

### 문제 7-3 · 오늘 만든 것을 깃허브에 올리기

- (가) `git status` 를 쳤을 때 빨간 목록에 줄이 몇 개였는지, 그 가운데 `.env` · `.venv` 가 있었는지 적어 주세요.
  - 쓸 내용: ① 줄 수 ② .env · .venv 가 있었는지
  - 답:8개. 없다.
