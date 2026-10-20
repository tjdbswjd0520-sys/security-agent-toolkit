# Day 7 Docker ② · 어제 명령 복습 · 이미지 · 이미지 버전 · 오후 (이름 · 2026-10-20)

> **쓰는 법**
> - 문제를 하나 풀 때마다 그 문제 제목 아래를 채웁니다.
> - 질문마다 `쓸 내용:` 에 **무엇을 쓰면 되는지** 적혀 있습니다. 그 내용대로 `답:` 뒤에 씁니다.
> - 「왜 그럴까요?」 같은 질문은 한 단어로 끝내지 말고, 내 화면에서 본 값이나 배운 내용을 근거로 이유까지 씁니다.
> - 🔍 는 그 교시에 꼭 알아야 할 핵심 질문입니다. 답을 못 찾았으면 「모름 — 막힌 곳: …」 이라고 씁니다.

---

## 5교시 · 어제 명령 복습 · 서버 두 대 켜고 접속 기록 구분해 읽기

### 문제 5-1 · 어제와 같은 명령으로 서버 두 대를 켜고 접속 기록을 구분해 읽기

- (가) 내가 `web-three` 와 `web-four` 를 켠 `docker run` 명령 두 개를 붙여 넣어 주세요.
  - 쓸 내용: 윈도는 `MSYS_NO_PATHCONV=1` 부터, 맥북은 `docker run` 부터 친 그대로 두 줄
  - 답:

```
MSYS_NO_PATHCONV=1 docker run -d --name web-three -p 8082:80 -v "$(pwd)/site-three:/usr/share/nginx/html:ro" nginx:1.28
MSYS_NO_PATHCONV=1 docker run -d --name web-four -p 8083:80 -v "$(pwd)/site-four:/usr/share/ng
inx/html:ro" nginx:1.28
```

- (나) 🔍 `docker ps` 에서 `web-three` 와 `web-four` 줄의 `PORTS` 값을 각각 적어 주세요.
  - 쓸 내용: 「web-three: … / web-four: …」
  - 답:web-three: 8082 / web-four: 8083
- (다) 🔍 8082 로 세 번, 8083 으로 한 번 접속했을 때 `docker logs web-three` 와 `docker logs web-four` 의 `"GET / HTTP/1.1"` 줄은 각각 몇 개였고, 왜 그렇게 나뉘나요?
  - 쓸 내용: ① web-three 의 줄 수 ② web-four 의 줄 수 ③ 그렇게 나뉘는 이유(어느 포트로 온 요청이 어느 서버에 기록되는지)
  - 답:3개, 1개. 8082 포트로 들어온 요청은 web-three의 로그에 기록되고, 8083 포트로 들어온 요청은 web-four의 로그에 기록되기 때문이다. 따라서 8082로 세 번 접속하면 web-three에 3개, 8083으로 한 번 접속하면 web-four에 1개가 기록된다. 
- (라) 🔍 `stop` 뒤에 `docker ps` 와 `docker ps -a` 에서 두 서버가 각각 어떻게 보였고, `rm` 뒤에는 어떻게 되었나요?
  - 쓸 내용: 「stop 뒤 ps: … / stop 뒤 ps -a: … / rm 뒤 ps -a: …」
  - 답:
  stop 뒤 ps: 아무것도 출력되지 않음. / stop 뒤 ps -a: 4901b677ee8e   nginx:1.28    "/docker-entrypoint.…"   3 minutes ago   Exited (0) 17 seconds ago             web-four
7413ed79d875   nginx:1.28    "/docker-entrypoint.…"   6 minutes ago   Exited (0) 17 seconds ago             web-three
76f29492bc6e   hello-world   "/hello"                  24 hours ago    Exited (0) 24 hours ago               quirky_murdock
12f9dbe692af   hello-world   "/hello"                  24 hours ago    Exited (0) 24 hours ago               frosty_fermi / rm 뒤 ps -a:76f29492bc6e   hello-world   "/hello"   24 hours ago   Exited (0) 24 hours ago             quirky_murdock
12f9dbe692af   hello-world   "/hello"   24 hours ago   Exited (0) 24 hours ago             frosty_fermi
- (한 줄) 보안 담당에게 보낼 답을 한 문장으로 적어 주세요.
  - 쓸 내용: 보안 담당이 바로 읽을 수 있는 한 문장(서버를 몇 대 어떻게 켰고 기록이 어떻게 남았는지)
  - 답:Nginx 웹 서버 컨테이너 4대 중 web-three와 web-four를 실행하고, 8082 포트로 들어온 요청은 web-three 로그에, 8083 포트로 들어온 요청은 web-four 로그에 각각 기록되는 것을 확인했습니다.
- 이 교시에서 새로 알게 된 것 하나를 적어 주세요.
  - 쓸 내용: 자유롭게
  - 답:

## 6교시 · 내 이미지 만들기 (Dockerfile)

### 문제 6-1 · Dockerfile 두 줄을 써서 내 이미지 만들기

- (가) 내가 쓴 Dockerfile 두 줄을 붙여 넣어 주세요.
  - 쓸 내용: Dockerfile 두 줄(`FROM` 줄 · `COPY` 줄)
  - 답:

```
FROM nginx:1.28
COPY site-three/index.html /usr/share/nginx/html/index.html

```

- (나) 🔍 `FROM` 과 `COPY` 가 각각 하는 일을 적어 주세요.
  - 쓸 내용: ① FROM 이 하는 일 ② COPY 가 하는 일
  - 답:① 'FROM'은 새 이미지에 포함할 기존 이미지를 지정하는 기능을 갖고 있다. ② 'COPY'는 내 pc의 파일을 이미지 안의 지정한 경로에 복사하는 기능을 갖고 있다.
- (다) 🔍 5교시와 달리 `docker run` 에 `-v` 가 없는데도 내 화면이 나오는 이유는 무엇인가요?
  - 쓸 내용: ① 어제 파일이 컨테이너에 닿던 방법 ② 오늘 파일이 서버에 있는 위치와 시점
  - 답:
  ①  -v 옵션으로 내 PC의 폴더와 컨테이너 내부 폴더를 연결해 파일을 제공했다.

②  Dockerfile의 COPY 명령으로 HTML 파일을 이미지를 빌드하는 시점에 이미지 내부로 복사했기 때문에, docker run에 -v가 없어도 컨테이너에서 해당 파일을 보여 줄 수 있다.
- (라) 🔍 `docker build` 출력에서 `[2/2] COPY` 줄과 `naming to` 줄을 붙여 넣고, 각각 무엇을 알려 주는지 적어 주세요.
  - 쓸 내용: ① 두 줄 ② 각 줄이 알려 주는 것
  - 답:

```
[2/2] COPY site-three/index.html /usr/share  1.0s
naming to docker.io/library/my-nginx:1    0.0s
```
site-three/index.html/에 있는 파일을 usr/share에 복사한다.
ocker.io/library/ 위치에 my-nginx라는 이름의 이미지 빌드. 태그는 1.

## 7교시 · 이미지 버전 다루기 · 되돌리기 · 정리 · 올리기

### 문제 7-1 · 파일을 고쳐 보고, 이미지를 다시 만들어 반영하기

- (가) 파일을 고친 직후 새로 고침했을 때 화면에 무엇이 나왔나요?
  - 쓸 내용: 새로 고침했을 때 보인 화면의 큰 제목
  - 답: site-three
- (나) 🔍 파일을 고쳤는데도 화면이 바뀌지 않은 이유와, 바꾸려면 무엇을 해야 하는지 적어 주세요.
  - 쓸 내용: ① 바뀌지 않은 이유(서버가 보는 파일이 어디 있는지) ② 바꾸려면 할 일
  - 답:① site-three/index.html의 내용을 고쳤지만 고친 내용을 my-nginx에 적용하지 않았기 때문에 바뀌지 않았다. ② 고친 버전을 build 커맨드로 적용한 다음 새롭게 docker를 연결해 실행한다.
- (다) 🔍 5교시의 `-v` 방식과 6교시의 이미지에 넣는 방식은 파일을 고쳤을 때 반영되는 방법이 어떻게 다른가요?
  - 쓸 내용: ① -v 방식의 반영 방법 ② 이미지에 넣는 방식의 반영 방법
  - 답:
  ① -v 방식: 내 PC의 폴더와 컨테이너 내부 폴더가 연결되어 있으므로, PC에서 파일을 수정하면 컨테이너에서도 변경된 파일을 바로 확인할 수 있다. 브라우저를 새로 고침하면 수정 내용이 반영된다.

② 이미지에 넣는 방식: docker build로 수정된 파일을 포함한 새 이미지를 만들고, 그 이미지로 컨테이너를 새로 실행해야 한다. 기존 이미지와 컨테이너에는 수정 내용이 자동으로 반영되지 않는다.

### 문제 7-2 · 이전 버전으로 되돌리고 쓰지 않는 이미지 지우기

- (가) 이전 버전으로 되돌린 뒤 `docker ps` 의 `IMAGE` 칸 값과 브라우저에 나온 제목을 적어 주세요.
  - 쓸 내용: 「IMAGE: … / 브라우저 제목: …」
  - 답:IMAGE: my-nginx:1 / 브라우저 제목: site-three
- (나) 🔍 쓰고 있는 `my-nginx:1` 을 `docker rmi` 로 지우려 했을 때 에러 문구의 핵심과, 어떻게 해결했는지 적어 주세요.
  - 쓸 내용: ① 에러 문구의 핵심 ② 지워지지 않은 이유 ③ 해결한 순서
  - 답:①  container 6a628c5def3e is using its referenced image 90b5eed284ae ② 해당 이미지를 사용하고 있기 때문에 지울 수 없었다. ③ my-mgninx:2 이미지 삭제 > my-site 컨테이너 삭제 > docker rmi 커맨드로 my-mgninx:1 이미지 삭제.
- (다) 🔍 이미지에 `:1` · `:2` 같은 태그를 붙여 두면 문제가 생겼을 때 어떤 점이 좋은가요?
  - 쓸 내용: ① 되돌리는 방법 ② 어느 버전이 켜져 있는지 아는 방법
  - 답:①  해당 버전으로 컨테이너를 다시 실행한다. ② 어docker ps에서 컨테이너가 사용하는 이미지와 태그를 확인, docker images에서 보유한 이미지와 태그를 확인.
- (한 줄) 보안 담당에게 보낼 답을 한 문장으로 적어 주세요.
  - 쓸 내용: 보안 담당이 바로 읽을 수 있는 한 문장(되돌린 일과 정리한 일)
  - 답:
  이미지 :1 삭제 시 해당 이미지를 참조하는 컨테이너 때문에 오류가 발생했지만, 컨테이너를 먼저 삭제한 뒤 이미지를 정리했으며, 버전별 태그를 활용하면 문제 발생 시 정상 버전으로 되돌릴 수 있습니다.
- 이 교시에서 새로 알게 된 것 하나를 적어 주세요.
  - 쓸 내용: 자유롭게
  - 답:

### 문제 7-3 · 오늘 만든 것을 깃허브에 올리기

- (가) `git status` 를 쳤을 때 빨간 목록에 줄이 몇 개였는지, 그 가운데 `.env` · `.venv` 가 있었는지 적어 주세요.
  - 쓸 내용: ① 빨간 목록의 줄 수 ② .env · .venv 가 있었는지
  - 답: 8개, 없음.
