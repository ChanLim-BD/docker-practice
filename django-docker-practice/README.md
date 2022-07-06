# Docker Compose로 Django 프로젝트 Setup


## 1. requirements.txt
>
```shell
$ mkdir django-docker-practice && cd django-docker-practice
```
>
requirements.txt
```
Django
psycopg2
```

---

## 2. Dockerfile 생성

>
```go
FROM python:3
ENV PYTHONUNBUFFERED 1
WORKDIR /web
COPY . .
RUN pip install -r requirements.txt
```

위 `Dockerfile` 파일은 `python:3` 이미지를 기반으로 다음 일련의 작업들을 순차적으로 명시하고 있습니다.

* 이미지 내 `PYTHONUNBUFFERED` 환경 변수를 1로 설정
* 이미지 내 작업 디렉터리를 `/web`으로 변경
* 현재 디렉터리의 모든 파일을 이미지 내의 작업 디렉터리로 복사
* `pip`를 이용해서 이미지 안에 패키지 설치

그리고 `build`
```shell
D:\Projects\docker-practice\django-docker-practice>docker build .
[+] Building 93.7s (10/10) FINISHED
 => [internal] load build definition from Dockerfile                                         
 => => transferring dockerfile: 135B                                                         
 => [internal] load .dockerignore                                                            
 => => transferring context: 2B                                                              
 => [internal] load metadata for docker.io/library/python:3                                  
 => [auth] library/python:pull token for registry-1.docker.io                                
 => [1/4] FROM docker.io/library/python:3@sha256:eeed7cac682f9274d183f8a7533ee1360a26acb3616a
 => => resolve docker.io/library/python:3@sha256:eeed7cac682f9274d183f8a7533ee1360a26acb3616a
 => => sha256:eeed7cac682f9274d183f8a7533ee1360a26acb3616aa712b2be7896f80d8c5f 2.35kB / 2.35k
 => => sha256:850b7f7626e5ca9822cc9ac36ce1f712930d8c87eb31b5937dba4037fe204034 2.22kB / 2.22k
 => => sha256:0f95b1e38607bbf15b19ad0d111f2316e92eb047a35370eac71973c636acb9d2 8.53kB / 8.53k
 => => sha256:1339eaac5b67d16d6d9f41fb7a7b96f7cebf3ba4beab36cbb60935aa772af583 55.01MB / 55.0
 => => sha256:4c78fa1b97999d08408734a61040475ade5bd7e33e91c0d5170dba2c7c7a92fd 5.16MB / 5.16M
 => => sha256:14f0d2bd524377dc42d072443c0e5e7cafa14f5df609d39bb1f717f43817a2cd 10.88MB / 10.8
 => => sha256:76e5964a957d206950c8c0de99f3c491ecec78887ebe4df0ac5ab9ceb536a4d5 54.58MB / 54.5
 => => sha256:cc4bb1a04a94a9015f79b0d36ee942b63bd486da0ef79689d4326398b561fa3a 196.76MB / 196
 => => extracting sha256:1339eaac5b67d16d6d9f41fb7a7b96f7cebf3ba4beab36cbb60935aa772af583    
 => => sha256:3dee34a94cb0bff32985a098419d9017f8eb56b546004d176ae6fa8d247615ea 6.29MB / 6.29M
 => => sha256:72ee0a082e689aaac88b303be7891a84b626ccd4c6e64aa56c0cafaf1e3941f4 19.69MB / 19.6
 => => sha256:fd1da769482d24fc5b941b6f8f4df2c62fe6816534487ab37d037715bfa5f3be 233B / 233B   
 => => sha256:a27ec1e00a3ed5d453b3bbaa6f8dcdbdd0b0c97e45207ba5d20d8c40e8f5d897 2.87MB / 2.87M
 => => extracting sha256:4c78fa1b97999d08408734a61040475ade5bd7e33e91c0d5170dba2c7c7a92fd    
 => => extracting sha256:14f0d2bd524377dc42d072443c0e5e7cafa14f5df609d39bb1f717f43817a2cd    
 => => extracting sha256:76e5964a957d206950c8c0de99f3c491ecec78887ebe4df0ac5ab9ceb536a4d5    
 => => extracting sha256:cc4bb1a04a94a9015f79b0d36ee942b63bd486da0ef79689d4326398b561fa3a    
 => => extracting sha256:3dee34a94cb0bff32985a098419d9017f8eb56b546004d176ae6fa8d247615ea    
 => => extracting sha256:72ee0a082e689aaac88b303be7891a84b626ccd4c6e64aa56c0cafaf1e3941f4    
 => => extracting sha256:fd1da769482d24fc5b941b6f8f4df2c62fe6816534487ab37d037715bfa5f3be    
 => => extracting sha256:a27ec1e00a3ed5d453b3bbaa6f8dcdbdd0b0c97e45207ba5d20d8c40e8f5d897    
 => [internal] load build context                                                            
 => => transferring context: 196B                                                            
26e96bc05a9ef7d690f1b36434cd
       0.1s
```

그리고 실행해보기

```shell
D:\Projects\docker-practice\django-docker-practice>docker run -it 90da28160
Python 3.10.5 (main, Jun 23 2022, 10:42:52) [GCC 10.2.1 20210110] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import django
>>> print(django.get_version())
4.0.6
>>> quit()
```

---

### 3. docker-compose.yml

* Docker Compose는 여러 개의 서비스를 이뤄진 애플리케이션을 하나의 YAML 파일에 정의할 수 있도록 해줍니다.
* 우리가 셋업하려는 Django 프로젝트는 Django 애플리케이션과 PostgreSQL 데이터베이스로 이루어집니다.

다음과 같이 프로젝트 최상위 디렉터리에 `docker-compose.yml`을 생성하고, web 서비스로 Django 애플리케이션을 `db` 서비스로 PostgreSQL 데이터베이스를 정의해줍니다.


```yml
version: "3"

services:
  web:
    build: .
    command: python manage.py runserver 0:8000
    ports:
      - "8000:8000"
    volumes:
      - .:/web
    depends_on:
      - db
  db:
    image: postgres
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
```

* `web` 서비스를 보시면 현재 디렉터리에 있는 `Dockerfile`을 이용해서 이미지를 빌드하고, Django 애플리케이션을 구동하기 위해서 `python manage.py runserver 0:8000` 커맨드를 실행하게 되어 있습니다. 
* 또한, 호스트의 8000 포트와 컨테이너의 8000 포트를 바인드(bind)시키고, 현재 디렉터리를 컨테이너의 `/web` 디렉터리로 마운트(mount)하고 있습니다.
*  마지막으로 `web` 서비스가 돌아가기 전에 `db` 서비스가 반드시 먼저 돌아갈 수 있도록 `depends-on` 설정이 되어 있습니다.
* `db` 서비스는 `postgres` 외부 이미지를 사용하도록 되어 있으며, `POSTGRES_USER`와 `POSTGRES_PASSWORD` 환경변수를 설정해주고 있습니다.

---

## 4. 프로젝트 생성
```shell
$ docker-compose run web django-admin startproject test .
```

```shell
$ tree
.
├── Dockerfile
├── docker-compose.yml
├── manage.py
├── test
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── requirements.txt
```

---

## 프로젝트 실행
```shell
$ docker-compose up
Starting django-app_db_1 ... done
Recreating django-app_web_1 ... done
Attaching to django-app_db_1, django-app_web_1
db_1   |
db_1   | PostgreSQL Database directory appears to contain a database; Skipping initialization
db_1   |
db_1   | 2020-05-14 02:15:02.745 UTC [1] LOG:  starting PostgreSQL 12.2 (Debian 12.2-2.pgdg100+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 8.3.0-6) 8.3.0, 64-bit
db_1   | 2020-05-14 02:15:02.745 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
db_1   | 2020-05-14 02:15:02.745 UTC [1] LOG:  listening on IPv6 address "::", port 5432
db_1   | 2020-05-14 02:15:02.748 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
db_1   | 2020-05-14 02:15:02.764 UTC [25] LOG:  database system was shut down at 2020-05-14 02:12:56 UTC
db_1   | 2020-05-14 02:15:02.769 UTC [1] LOG:  database system is ready to accept connections
web_1  | Watching for file changes with StatReloader
web_1  | Performing system checks...
web_1  |
web_1  | System check identified no issues (0 silenced).
web_1  |
web_1  | You have 17 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
web_1  | Run 'python manage.py migrate' to apply them.
web_1  | May 14, 2020 - 02:15:03
web_1  | Django version 3.0.6, using settings 'test.settings'
web_1  | Starting development server at http://0:8000/
web_1  | Quit the server with CONTROL-C.
```