## **ORM** (Object-Relational Mapping)

: 애플리케이션의 객체를 RDB 테이블에 자동으로 영속화 해주는 것

### 📎 장점

- `객체 지향`적인 코드로 인해 더 직관적이고 비즈니스 로직에 집중할 수 있게 해줌
  - SQL의 절차적이고 순차적 접근이 아닌 객체 지향적 접근
  - 내부적으로는 쿼리를 생성
  - `개발자는 신경쓰지 않아도 됨`
- `재사용` 및 `유지보수`의 편리성 증가
  - 독립적으로 작성돼 재활용 가능
  - `모델(model)`에서 가공된 데이터를 `컨트롤러(controller)`에 의해 `뷰(view)`와 합쳐지는 형태
    - 디자인 패턴을 견고히 다지는 데에 유리
  - 매핑정보가 명확
- DBMS에 대한 `종속성 감소`
  - 구현 방법뿐 아니라 자료형 타입까지 종속성 감소
  - DBMS를 교체하는 작업 또한 비교적 적은 리스크와 시간 소요

### 📎 단점

- 완벽한 ORM으로만 서비스 구현은 어려움
  - 설계가 신중해야 함
  - 프로젝트가 복잡하고 클수록
    - 난이도 상승
    - `속도 저하` 및 일관성 파괴
  - 일부 대형 쿼리는 속도를 위해 `별도의 튜닝`이 필요할 수 있음

</br>

---

### **참고자료**

- Web
  - [@incodom](http://www.incodom.kr/ORM)
  - [@dbjh](https://dbjh.tistory.com/77)
