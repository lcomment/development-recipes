## **포트(Port)란?**

: ip 내에서 애플리케이션 상호 구분(프로세스 구분)을 위해 사용하는 번호

- 0번부터 65535번까지 있음
- 0번 ~ 1023번: `well-known port`
- 1024번 ~ 49151번: registered port
- 49152번 ~ 65535번: dynamic port

### well-known port 예시

| Port |    20     |    21     | 22  |  23  | 53  |    80    | 119  |         443         |
| :--: | :-------: | :-------: | :-: | :--: | :-: | :------: | :--: | :-----------------: |
| Role | FTP(data) | FTP(제어) | SSH | 텔넷 | DNS | www HTTP | NNTP | TSL/SSL 방식의 HTTP |

</br>

## **포트포워딩(Port-Forwarding)이란?**

: 컴퓨터 네트워크에서 패킷이 라우터나 방화벽 같은 네트워크 게이트웨이를 가로지르는 동안 네트워크 주소를 변환해주는 것

### 포트포워딩의 목적

- `public` HTTP 서버를 `private` LAN 안에 실행
- 인터넷으로부터 private LAN 상의 호스트에 대한 `시큐어 셸(Secure SHell; SSH) 접근을 허가`
- 인터넷으로부터 호스트에 대한 `FTP 접근을 허가`
- 프라이빗 LAN에 위치한 `공개 게임 서버`를 실행

<p align=center><img src='../../resources/port1.png' width=400></p>

&nbsp; 공유기를 설치하면 공유기와 연결된 PC들은 192.168~로 시작하는 ip를 공유기로부터 할당 받는다. 그리고 공유기 또한 ISP 업체로부터 할당받은 IP를 갖는다. 공유기를 중심으로 공유기 뒤에 있는 PC들은 `내부 ip`라고 부르고, 공유기를 `외부 ip`라고 부르는데, 만약 다른 영역에 있는 PC에서 내부 ip에 있는 192.168.0.20 PC에 접속하고자 하는 요청이 들어왔을 때 공유기는 어느 PC로 연결을 해주어야 할지 모르는 상태가 된다.

&nbsp; 이러한 상황에서 공유기에게 해당 포트로 요청이 오면 192.168.0.20 PC로 연결하라는 이정표를 달아주는 것을 `포트 포워딩`이라고 한다.

---

### **참고자료**

- 블로그
  - [ooeunz tistory](https://ooeunz.tistory.com/104)
  - [hanamon.kr](https://hanamon.kr/네트워크-기본-ip-주소와-포트-port/)
- [위키백과](https://ko.wikipedia.org/wiki/포트_포워딩)
