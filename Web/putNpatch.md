## PUT과 PATCH

### PUT

> The HTTP PUT request method `creates` a new resource or `replaces` a representation of the target resource with the request payload.

: HTTP PUT 메서드는 요청 페이로드를 사용해 `새로운 리소스를 생성`하거나 대상 리소스를 나타내는 `데이터를 대체`합니다.

<br>

### PATCH

> The HTTP PATCH request method applies `partial modifications` to a resource.

: HTTP PATCH 메소드는 리소스의 `부분적인 수정`을 할 때에 사용됩니다.

## 차이점

&nbsp; PUT과 PATCH는 정의에서부터 차이점을 보인다. 아래의 설명을 보면 더 확실히 이해할 수 있을 것이다.

> ### _리소스 예시_

| id  | name | value |
| :-: | :--: | :---: |
|  1  |  a   |  100  |
|  2  |  b   |  200  |
|  3  |  c   |  300  |

<br>

> ### 1. Update 방식

```json
1. PUT /users?id=1
{
    name: "d",
    value: 400
}

2. PUT /users?id=1
{
    name: "d"
}
```

&nbsp; PUT의 정의에서는 새로운 리소스를 생성하거나 데이터를 대체한다고 했다. `1번`의 경우 자원의 모든 상태에 새로운 값을 대입하여 보낸 요청이다. 반대로 `2번`은 `name` 데이터에만 새로운 값을 보낸 요청이다.

| id  | name | value |
| :-: | :--: | :---: |
|  1  |  d   |  400  |
|  2  |  b   |  200  |
|  3  |  c   |  300  |

&nbsp; `1번`의 경우, 새로운 자원이 기존 리소스를 대체했다.

| id  | name | value |
| :-: | :--: | :---: |
|  1  |  d   |  null  |
|  2  |  b   |  200  |
|  3  |  c   |  300  |

&nbsp; 하지만 `2번`의 경우, 보내지 않은 값이 `null`로 대체된다. 즉, 대상 리소스를 나타내는 데이터를 대체하는 `Update` 방식이다.

```json
1. PATCH /users?id=1
{
    name: "d",
    value: 400
}

2. PATCH /users?id=1
{
    name: "d"
}
```

&nbsp; 같은 데이터로 PATCH 요청을 보내보면,

| id  | name | value |
| :-: | :--: | :---: |
|  1  | `d`  | `400` |
|  2  |  b   |  200  |
|  3  |  c   |  300  |

&nbsp; `1번`의 경우, 보낸 값 모두 수정 되었다.

| id  | name | value |
| :-: | :--: | :---: |
|  1  |  d   |  100  |
|  2  |  b   |  200  |
|  3  |  c   |  300  |

&nbsp; `2번` 또힌 보내지 않은 값은 그대로 유지되고, `name`만 새로운 데이터로 수정됐다. 즉, 부분적인 수정을 하는 `Update` 방식이다.

<br>

> ### 2. 요청한 URI에 자원이 존재하지 않을 때

```json
1. PUT /users?id=4
{
    name: "d",
    value: 400
}

2. PATCH /users?id=4
{
    name: "d",
    value: 400
}
```

&nbsp; `PUT`의 경우, 새로운 자원을 `생성`해준다. 하지만 `PATCH`는 새로운 자원을 생성하지 않고 `오류`를 뱉어낸다.

<br>

---

### **참고자료**

- [@vagabondms](https://velog.io/@vagabondms/기술-스터디-PUT과-PATCH-차이)
