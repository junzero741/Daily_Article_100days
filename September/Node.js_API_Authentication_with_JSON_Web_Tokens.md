22번째 글

링크 : https://javascript.plainenglish.io/node-js-api-authentication-with-json-web-tokens-bb511f603723


# JWT 와 Node.js API 인증

## 서문
* JWT에 어떻게 접근하는지, JWT로 우리의 라우트를 어떻게 보호할지에 대해서 알아보자.
* JWT 는 데이터를 암호화하고 각각의 유저의 인증을 도와주는 웹 토큰이다.


## 작업환경
* express : 서버 프레임워크
* jsonwebtoken : JWT 를 확인하는 데 쓰일 라이브러리.
* nodemon : 파일에 변경사항이 있으면 서버를 알아서 재시작해주는 라이브러리.
* 터미널에 다음과 같이 입력하여 설치
```bash
npm init -y
npm i express jsonwebtoken
npm i -g nodemon
```
## 프로젝트 구조

![image](https://user-images.githubusercontent.com/71166372/134762089-683b4570-8147-4886-85aa-8afff6707ec0.png)


## 서버파일 작성
```javascript
// index.js
const express = require('express');
const jwt = require('jsonwebtoken');
const app = express();

app.listen(5000, () => console.log('listening on port 5000'));

app.get('/api', (req, res) => {
  res.json({
    message: 'Welcome to the API!',
    });
});
```
![image](https://user-images.githubusercontent.com/71166372/134762738-e581d42d-ac20-4a89-b9ac-56afae90d9be.png)





