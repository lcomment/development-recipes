## **parser란 무엇인가?**

&nbsp; 가지고 있는 데이터를 원하는 상태로 `가공`하는 과정을 `parsing`이라고 하고, 이를 수행하는 함수나 모듈이 바로 `parser`다. parser의 종류로는 bodyParser, cookieParser, JSON.parse, JSON.stringify 등이 있다.

</br>

## **body-parser란?**

> Contains key-value pairs of data submitted in the request body.
> By default, it is `undefined`, and is populated when you use `body-parsing middleware` such as body-parser and multer.  
> **`< Express documents >`**

&nbsp; express 문서에 따르면, POST나 PUT 요청 시 미들웨어 없이 `req.body`에 접근하는 경우, 기본으로 `undefined`가 설정돼 있으므로 bodyParser, multer와 같은 미들웨어를 사용하여 요청 데이터 값에 접근해야 한다는 안내를 찾을 수 있다. body가 포함된 요청 시에는 서버 내에서 해석 가능한 형태로 변형해야 사용할 수 있는 것이다.

```javascript
const express = require('express')
const bodyParser = require('body-parser')
const app = express();

app.use(bodyParser().json());
app.post('/sign-up', function(req, res) => {
  console.log(req.body);
});
```

&nbsp; 따라서 위 코드와 같이 `npm i body-parser`을 통해 body-parser 모듈을 설치하여 express에 붙여 사용하면 body의 값을 불러올 수 있다.

</br>

## **Express v4.16.0**

> This is a built-in middleware function in Express. It parses incoming requests with JSON payloads and is based on body-parser.  
> **`< Express documents >`**

&nbsp; express.js 4.16.0버전부터는 `built-in body-parser`를 넣었다. 따라서 아래 코드와 같이 body-parser을 import하지 않아도 된다.

```javascript
const express = require('express')
const app = express();

app.use(express.json())l
app.post('/sign-up', function(req, res) => {
  console.log(req.body)
});
```

</br>

---

### **참고자료**

- Blog
  - [Express Docs](https://expressjs.com/en/guide/migrating-4.html#example-migration)
  - [@yejinh](https://velog.io/@yejinh/express-미들웨어-bodyParser-모듈)
  - [@chullino](https://medium.com/@chullino/1분-패키지-소개-body-parser를-소개합니다-하지만-body-parser를-쓰지-마세요-bc3cbe0b2fd)
