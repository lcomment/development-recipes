## CORS (Cross-Origin Resource Sharing Policy)

: SOP(; Same-Origin Policy)를 위반하고, 적절한 보안 인증을 받기 위한 메커니즘

- CORS 없이 모든 곳에서 데이터를 요청할 수 있게 하면 다른 사이트에서 원래 사이트를 흉내낼 수 있음
- 브라우저에서 `보호`하고, 필요한 경우에만 `서버와 협의`하여 요청하기 위해 CORS 필요

## Front-End에서 CORS 해결

##### (React 기준으로 작성했습니다)

> ### package.json에서 Proxy 설정

```json
    },
    "proxy": "접속하려는 서버의 루트 URL"
}
```

<br>

> ### http-proxy-middleware 사용

```typescript
const { createProxyMiddleware } = require("http-proxy-middleware");

module.exports = function (app) {
  app.use(
    createProxyMiddleware("/api", {
      target: "접속하려는 서버의 루트 URL",
      changeOrigin: true,
    })
  );
};
```

<br>

## Back-End에서 CORS 해결

> ### NestJS

- enableCors() 함수 사용

```typescript
// main.ts
const app = await NestFactory.create(AppModule);
app.enableCors({
  origin: process.env.CLIENT_URL,
  methods: ["GET", "POST", "PUT", "PATCH", "DELETE"],
});
```

<br>

> ### Spring Boot

- `CorsFilter` 생성하기
- `@CrossOrigin` 사용
- `WebMvcConfigurer` 사용
