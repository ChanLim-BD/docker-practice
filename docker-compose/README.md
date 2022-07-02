![](https://velog.velcdn.com/images/chan9708/post/1538b836-af37-493b-9f18-e7bc6c62c5f3/image.png)

## Docker compose

**다중 컨테이너 도커 애플리케이션을 정의하고 실행하기 위한 도구입니다.**

### 멀티 컨테이너 사이에 통신을 할 수 있게 합니다.

![](https://velog.velcdn.com/images/chan9708/post/f57803d4-a82b-401c-a884-4317aa0dfd56/image.png)

* 서로 다른 컨테이너가 존재하고 컨테이너 사이에는 어떠한 설정이 없으면 접근할 수 없습니다.
* 따라서, Node.js 앱에서 Redis 서버에 접근을 할 수 없습니다.

**: Redis?**
- REmote Dictionary Server
- 메모리 기반의 키-값 구조 데이터 관리 시스템이며, 모든 데이터를 메모리에 저장하고 조회할 수 있는 비관계형 데이터베이스 (NoSQL)입니다.
- 메모리에 저장을 하기 때문에 MySQL과 같은 데이터베이스에 저장하는 것과 데이터를 불러올 때 훨씬 빠르게 처리할 수 있습니다.
- 비록 메모리에 저장하지만 영속적으로도 보관할 수 있습니다.
- 그래서 서버를 재부팅해도 데이터를 유지할 수 있는 장점이 있습니다.


---
## Docker compose 작성
### docker-compose.yml
: **YAML - ain't markup language**
: 일반적으로 구성 파일 및 데이터가 저장되거나 전송되는 응용 프로그램에서 사용됩니다.
: 원래는 XML이나 json 포맷으로 많이 쓰였지만, 조금 더 사람이 읽기 쉬운 형태로 나타낸게 yaml입니다.

![](https://velog.velcdn.com/images/chan9708/post/33f4856b-e282-4212-aa3a-dd5ad59dbbe9/image.png)

```yaml
version: "3"
services:
  redis-server:
    image: "redis"
  node-app:
    build: .
    ports:
     - "5000:8080"
```
* **version** : 도커 컴포즈의 버전
* **services** : 이곳에 실행하려는 컨테이너들을 정의
* **redis-server** : 컨테이너 이름
* **image** : 컨테이너에서 사용하는 이미지
* **node-app** : 컨테이너 이름
* **build** : 현재 디렉토리에 있는 Dockerfile 사용
* **ports** : 포트 맵핑 - 로컬포트 : 컨테이너 포트

```shell
>>> docker-compose up
```

---

## Docker compose stop & down

**도커 컴포즈를 통해 작동시킨 컨테이너들을 한꺼번에 중단 시키려면**
```shell
>>> docker-compose down
```
** 으로 할 수 있습니다.**

### docker-compose up VS docker-compose up --build

* **docker-compose up** : 이미지가 없을 때 이미지를 빌드하고 컨테이너 시작합니다.
* **docker-compose up --build** : 이미지가 있든 없든 이미지를 빌드하고 컨테이너를 시작합니다.

### -d

```shell
>>> docker-compose up -d
```

: detached 모드로서 앱을 백그라운드에서 실행합니다.
: 앱에서 나오는 output을 표출하지 않습니다.