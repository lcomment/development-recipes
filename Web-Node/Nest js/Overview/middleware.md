## **Middleware란?**

&nbsp; 미들웨어는 route 핸들러 전에 호출되는 함수다. 미들웨어의 기능은 `request`, `response` 객체에 액세스 할 수 있고, 애플리케이션의 request-response 사이클에서 `next()` 미들웨어 함수에 액세스 할 수 있다. next 미들웨어는 일반적으로 `next`라고 표시한다.

<p align=center><img src='../../../resources/nest/middleware.png' width=512></p>

</br>

---

### **참고자료**

- [Nest Docs](https://docs.nestjs.com/exception-filters)
