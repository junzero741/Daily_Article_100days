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
> 포스트맨에서 API 작동 확인

```javascript
// index.js

app.post('/api/Login', (req, res) => {
  // Mock User
  const user = {
    username: 'junzero741',
    email: 'junzero741@gmail.com'
  }
  jwt.sign({user: user}), 'secretkey', {expiresIn: '30s'}, (err, token) => {
    res.json({token});
  });
})
```
![image](https://user-images.githubusercontent.com/71166372/134774568-e7489a50-c25e-4a7b-9d24-f10df713c6dc.png)
> Post 엔드포인트


이제 토큰을 인증해줄 함수를 만들어 헤더에 넘겨주자.
```javascript
// Access token
// Authorization : Bearer <access token>

// Verify Token
function verifyToken (req, res, next) {
  // Get Auth header value
  const bearerHeader = req.headers['authorization'];
  // check if bearer is undefined
  if (typeof bearerHeader !== 'undefined') {
     const bearer = bearerHeader.split(' ');
     const bearerToken = bearer[1];
     req.token = bearerToken;
     next();
  } else {
    res.sendStatus(403);
  }
}
```

```javascript
// index.js

// Post to Validate the API with jwt token
app.post('/api/validate', verifyToken, (req, res) => {
   jwt.verify(req.token, 'secretkey', (err, authData) => {
      if(err) {
        res.sendStatus(403);
      } else {
        res.json({
          message: 'Validated',
          authData
        });
      }
  });
});
```
![image](https://user-images.githubusercontent.com/71166372/134774784-3c7138dd-198d-4182-b642-9b2b56abfeec.png)
> 토큰 없이 API를 활성화하려고 시도하면 403 에러가 뜬다.


로그인 API 를 통해 토큰을 얻고 헤더에 넘겨주면 다음과 같은 결과를 얻을 수 있다.
![image](https://user-images.githubusercontent.com/71166372/134774839-4cd92b81-94af-4f14-8c0c-35263eefb6b5.png)


