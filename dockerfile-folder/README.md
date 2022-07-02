![](https://velog.velcdn.com/images/chan9708/post/65378d77-b620-4ca8-b840-1c9591edd5eb/image.png)

## Docker Image?

1. 도커 이미지는 컨테이너를 만들기 위해 필요한 설정이나 종속성들을 갖고 있는 소프트웨어 패키지입니다.

2. 지금까지 해왔듯이 도커 이미지는 DockerHub에 이미 다른 사람들이 만들어 놓은 것을 이용할 수도 있으며, 직접 도커 이미지를 만들어서 사용할 수도 있고 직접 만든 것을 DockerHub에 업로드할 수도 있습니다.

> EX)
``` shell
>>> docker create <이미지 이름>
```
>
![](https://velog.velcdn.com/images/chan9708/post/0b02f880-0c76-44fa-a7f2-58005a74f7af/image.png)

**컨테이너는 도커 이미지로 생성하는데, 그렇다면 도커 이미지는 어떻게 생성할까요?**

1. dockerfile 작성
	- 도커파일이란 도커 이미지를 만들기 위한 설정 파일입니다.
	- 컨테이너가 어떻게 행동해야 하는지에 대한 설정을 정의합니다.
2. docker 클라이언트
	- 도커 파일에 입력된 명령들이 도커 클라이언트에 전달되어야 합니다.
3. docker server
	- 도커 클라이언트에 전달된 모든 중요한 작업들을 하는 공간입니다.
4. image 생성

---

## 1. Dockerfile 작성

### Dockerfile?
- 도커 이미지를 만들기 위한 설정 파일이며, 컨테이너가 어떻게 행동해야 하는지에 대한 설정들을 정의해 주는 곳입니다.


### Dockerfile 만드는 순서
1. 베이스 이미지를 명시해줍니다. (파일 스냅샷에 해당합니다.)
2. 추가적으로 필요한 파일들을 다운 받기 위한 몇가지 명령어를 명시해줍니다. (파일 스냅샷에 해당합니다.)

### 베이스 이미지?
- 도커 이미지는 여러 개의 레이어들로 되어 있습니다.
- 그중에서 베이스 이미지는 이 이미지의 기반이 되는 부분입니다.

![](https://velog.velcdn.com/images/chan9708/post/8b90af4a-ed01-4fbd-abe0-5b25e2f03815/image.png)

: 만약 이 이미지에 무엇인가 추가한다면, 위에 보이는 레이어가 이미지에 추가됩니다.
-> layer caching (레이어 캐싱)

**hello 문구 출력하기**

![](https://velog.velcdn.com/images/chan9708/post/6a73a788-4d39-4940-9cfe-0ee01b2aefd0/image.png)

* **FROM** 
	- 이미지 생성 시 기반이 되는 레이어입니다.
    - <이미지 이름>:<태그> 형식으로 작성합니다.
    - 태그를 안 붙이면 자동적으로 가장 최신 버전으로 다운받습니다.
* **RUN** 
	- 도커 이미지가 생성되기 전에 수행할 쉘 명령어입니다.
* **CMD** 
	- 컨테이너가 시작되었을 때 실행할 실행 파일 또는 쉘 스크립트입니다.
    - 해당 명령어는 Dockerfile 내 1회만 쓸 수 있습니다.

---

## 2. 도커 클라이언트

**도커 파일에 입력된 것들이 도커 클라이언트에 전달되어서 도커 서버가 인식하게 해야합니다.**
```shell
>>> docker build ./ 또는 docker build .
```
: 해당 디렉토리 내에서 dockerfile이라는 파일을 찾아서 도커 클라이언트에 전달해줍니다.
: docker build 뒤에 ./ 와 .은 둘 다 현재 디렉토리를 가리킵니다.

![](https://velog.velcdn.com/images/chan9708/post/b5fb8742-c7b9-40e8-b360-15debf9de8bc/image.png)

**베이스 이미지에서 다른 종속성이나 새로운 커맨드를 추가할 때는 임시 컨테이너를 만든 후 그 컨테이너를 토대로 새로운 이미지를 만듭니다. 그리고 그 임시 컨테이너를 지웁니다.**

![](https://velog.velcdn.com/images/chan9708/post/cb2f1104-f351-44e2-aa26-1f2fe1ed9c71/image.png)

---
## 3. 도커 서버 및 실행 (이름주기)
![](https://velog.velcdn.com/images/chan9708/post/e3522973-7dd0-4ae0-ae95-a0112c5588f0/image.png)

: **도커 파일로 만든 이미지 아이디가 0234b3a80a0c70... 너무 복잡합니다.**

```shell
>>> docker build -t <도커 아이디>/<저장소/프로젝트 이름>:<version> ./
```
![](https://velog.velcdn.com/images/chan9708/post/93bda120-e04e-4174-8e47-3333a7aa8b3d/image.png)

---
# Reference

> [따라하며 배우는 도커와 CI환경](https://www.inflearn.com/course/%EB%94%B0%EB%9D%BC%ED%95%98%EB%A9%B0-%EB%B0%B0%EC%9A%B0%EB%8A%94-%EB%8F%84%EC%BB%A4-ci/dashboard)
[Docker Documentation](docs.docker.com/)






