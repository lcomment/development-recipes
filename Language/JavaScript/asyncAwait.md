## async / await

&nbsp; ES2015에 추가된 기능에 대해 공부했다면 `프로미스`에 대해 알 것이다. 프로미스는 훌륭한 비동기 처리 방안이지만, 프로미스를 보다 편하고 깔끔하게 활용하는 방안이 고안되었다. ES2017에 추가된 `async/await` 기능이다. ES2017에서 추가된 `async/await`는 콜백 지옥을 해결했지만 여전히 긴 `promise` 코드를 짧게 줄여주었고, 간단한 사용법으로 가독성까지 높였다. 아래 코드는 promise를 활용한 예제다.

```javascript
function findAndSaveUser(Users) {
  Users.findOne()
    .then((user) => {
      user.name = "gildong";
      return user.save();
    })
    .then((user) => {
      return Users.findOne({ gender: "m" });
    })
    .then((user) => {
      // 생략
    })
    .catch((err) => {
      console.error(err);
    });
}
```

&nbsp; 프로미스를 활용한 코드는 매우 유용하지만, 여전히 코드가 길어 작성하거나 읽기 힘들다. 아래는 async/await를 활용한 코드다.

```javascript
async function findAndSaveUser(Users) {
  try {
    let user = await Users.findOne({});
    user.name = "zero";
    user = await user.save();
    user = await Users.findOne({ gender: "m" });
    // 생략
  } catch (error) {
    console.error(error);
  }
}
```

&nbsp; function 키워드 앞에 async를 붙이고, 내부의 promise를 반환하는 비동기 처리 함수 앞에 await를 붙여주기만 하면 된다. `async/await`는 Node 7버전부터 사용할 수 있다.

---

### **참고자료**

- 서적: Node.js 교과서
