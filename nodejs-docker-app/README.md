![](https://velog.velcdn.com/images/chan9708/post/7ca874f2-6f8e-4dca-b935-9bc89d68f620/image.png)


![](https://velog.velcdn.com/images/chan9708/post/7db95ac0-2b37-4884-8a9c-47591fd6f6a8/image.png)

---

## Code.

> **package.json**
```javascript
{
  "name": "nodejs-docker-app",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "dependencies": {
    "express":"4.18.1"
  },
  "author": "",
  "license": "ISC"
}
```
>
> **server.js**
```js
const express = require('express');
>
const PORT = 8080;
>
//APP
const app = express();
app.get('/', (req,res) => {
    res.send("Hello World")
});
>
app.listen(PORT);
console.log('Server is running');
```
>
> **dockerfile**
>
```js
FROM node:10
>
WORKDIR /usr/src/app
>
COPY package.json ./
>
RUN npm install
>
COPY ./ ./
>
CMD [ "node", "server.js" ]
```

---
### 기존 컨테이너를 실행하기 위해 사용한 명령어
```shell
>>> docker run <이미지 이름>
```

### 이제 컨테이너를 실행하기 위해 사용할 명령어
```shell
>>> docker run -p [할당할 포트] : [서버의 기입된 포트] <이미지 이름>
```

![](https://velog.velcdn.com/images/chan9708/post/a4bff0a9-1957-48cb-ab69-8a5cd7a8fdb5/image.png)

![](https://velog.velcdn.com/images/chan9708/post/f787ed72-cd31-47a1-aee6-4a6d85fb3ce5/image.png)

---

### WORKING DIRECTORY
![](https://velog.velcdn.com/images/chan9708/post/0ac13ff7-e492-41a6-a104-23b57e497190/image.png)

![](https://velog.velcdn.com/images/chan9708/post/5d094d3c-27f5-4f88-a679-b03faa1bd25a/image.png)


: **이미지 안에서 애플리케이션 소스 코드를 가지고 있을 디렉토리를 생성하는 것입니다.**
: 이 디렉토리가 애플리케이션에 working directory가 됩니다.

![](https://velog.velcdn.com/images/chan9708/post/89d07f06-d16d-4f53-bd6a-d1d8f5bfd2da/image.png)

**workdir를 지정하지 않고 그냥 COPY할 때 생기는 문제**
* 원래 이미지에 있던 파일과 이름이 같을 경우 덮어쓰기 때문입니다.
* 모든 파일이 한 디렉토리에 들어가 버리면서 정리 정돈이 안되어있습니다.

---

### 애플리케이션 소스 변경으로 다시 빌드할 때 '효율적으로' 하는 법
![](https://velog.velcdn.com/images/chan9708/post/e1c9efb6-b712-4026-8044-27c146e242b1/image.png)

: **위에 도표를 보면 수정본에는 RUN 위에 COPY package.json ./이 하나가 추가되었고 원래 COPY가 RUN 아래로 내려갔습니다.**

-> **npm install 할 때 불필요한 다운로드를 피하기 위해서입니다.**
: 원래 모듈을 다시 받는 것은 모듈에 변화가 생겨야만 다시 받아야 하는데 소스코드에 조금의 변화만 생겨도 모듈 전체를 다시 받는 문제점을 해결합니다.

---
## Docker Volume

: 이제 npm install 전에 package.json만 따로 변경을 해줘서 쓸 데 없이 모듈을 다시 받지 않아도 됩니다.
: 하지만 **아직도 소스를 변경할 때 마다 변경된 소스 부분은 COPY한 후 이미지를 다시 빌드를 해주고 컨테이너를 실행해줘야지 변경된 소스가 화면에 반영이 됩니다.**

-> **Docker Volume이 효율적으로 바꿔준다.**

### 기존
![](https://velog.velcdn.com/images/chan9708/post/d881d6e7-31eb-48ab-b771-03f8630c6a79/image.png)

**호스트 디렉토리에 있는 파일을 도커 컨테이너에 '복사'하는 것입니다.**

### docker volume
![](https://velog.velcdn.com/images/chan9708/post/08629a73-3527-4b9e-b5f8-90f24f6e82c9/image.png)

**볼륨은 도커 컨테이너에서 호스트 디렉토리에 있는 파일을 참조(Mapping)하는 것입니다.**

```shell
>>> docker run -p 5000:8080 -v /usr/src/app/node_modules -v$(pwd):/usr/src/app <이미지 아이디>
```

* 호스트 디렉토리에 node_module은 없으므로 컨테이너에서 참조(Mapping)을 하지 말라고 지정.

* pwd 경로에 있는 디렉토리 혹은 파일을 /usr/src/app 경로에서 참조.

### PWD?
- 현재 작업 중인 디렉토리의 이름을 출력하는데 사용합니다.

---

# Reference

> [따라하며 배우는 도커와 CI환경](https://www.inflearn.com/course/%EB%94%B0%EB%9D%BC%ED%95%98%EB%A9%B0-%EB%B0%B0%EC%9A%B0%EB%8A%94-%EB%8F%84%EC%BB%A4-ci/dashboard)
[Docker Documentation](docs.docker.com/)
