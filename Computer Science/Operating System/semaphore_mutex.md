## 임계 영역 (Critical Section)

: 여러 프로세스가 데이터를 공유하며 수행될 때, 각 프로세스에서 공유 데이터를 접근하는 프로그램 코드 블럭

- 여러 프로세스가 동일 자원을 동시에 참조하여 오류가 발생할 위험 가능성이 있는 영역
- 성능 향상을 위해 최소화해야 함

<br>

## 뮤텍스 (Mutex)

: `Critical Section`을 가진 스레드들의 실행시간이 서로 겹치지 않고 `상호배제`(Mutual Exclution) 되도록 하는 기술

- 한 프로세스에 의해 소유될 수 있는 `Key를 기반`으로 한 상호배제 기법
  - Key에 해당하는 어떤 객체가 있음
  - 이 객체를 소유한 프로세스 또는 스레드만이 공유자원 접근 가능
- 다중 프로세스들의 공유 자원 접근을 조율하기 위해 `동기화` 또는 `락`을 사용
- 즉, 뮤텍스 객체를 두 스레드가 `동시에 사용할 수 없음`

```c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

pthread_mutex_t mutex;
int cnt=0;

void *count(void *arg) {
    int i;
    char* name = (char*)arg;

    pthread_mutex_lock(&mutex);

    //======== critical section =============
    cnt=0;
    for (i = 0; i <10; i++) {
        printf("%s cnt: %d\n", name,cnt);
        cnt++;
        usleep(1);
    }
    //========= critical section ============
    pthread_mutex_unlock(&mutex);
}

int main() {
    pthread_t thread1,thread2;

    pthread_mutex_init(&mutex,NULL);

    pthread_create(&thread1, NULL, count, (void *)"thread1");
    pthread_create(&thread2, NULL, count, (void *)"thread2");

    pthread_join(thread1, NULL);
    pthread_join(thread2, NULL);

    pthread_mutex_destroy(&mutex);
}
```

- int `pthread_mutex_init`(pthread_mutex_t *restrict mutex, const pthread_mutexattr_t *restrict attr);
  - mutex를 사용하기 전에 `동적으로 초기화`하는 함수
- int `pthread_mutex_lock`(pthread_mutex_t \*mutex);
  - Critical Section 진입 시 `코드 구역을 잠그는 함수`
- int `pthread_mutex_unlock`(pthread_mutex_t \*mutex);
  - Critical Section 종료 시 `락을 푸는 함수`
- int `pthread_mutex_destroy`(pthread_mutex_t \*mutex);
  - Mutex 동적 생성 후 Mutex를 종료할 때 호출하는 함수

<br>

## 세마포어 (Semaphore)

: 멀티 프로그래밍 환경에서 공유된 자원에 대한 접근을 제한하는 방법

- 상호 배제를 OS와 프로그래밍 언어 수준에서 지원하는 메커니즘
- 블록(수면)과 깨움을 지원
- 세마포어 → 정수 값을 갖는 변수
  - `Initialize Operation`: 세마포어 값을 음이 아닌 값으로 초기화
  - `Wait Operation`: 세마포어 값 감소, `음수`가 되면 호출한 프로세스 `블록`
  - `Signal Operation`: 세마포어 값 증가, `양수`가 되면 블록된 프로세스를 `깨움`

<br>

> ### 일반 세마포어 (Counting Semaphore)

- P, V Operations
- `P`: 임계 구역에 들어가기 전에 수행 → `semWait`
- `V`: 임계 구역에서 나올 때 수행 → `semSignal`

```c
struct semaphore {
    int count;
    queueType queue;
};
void semWait(semaphore s) {
    s.count--;
    if(s.count < 0) {
        /*
         * 요청한 프로세스를 s.queue에 연결
         * → 요청한 프로세스를 블록 상태로 전이
         */
    }
}
void semSignal(semaphore s){
    s.count++;
    if(s.count >= 0) {
        /*
         * s.queue에 연결돼 있는 프로세스를 큐에서 제거
         * → 프로세스의 상태를 실행 가능으로 전이시키고, Ready List에 연결
         */
    }
}
```

<br>

> ### 이진 세마포어 (mutex)

```c
struct binary_semaphore {
    enum { zero, one } value;
    queueType queue;
};
void semWaitN(binary_semaphore s) {
    if(s.value == one) {}
        s.value = zero;
    } else {
        /*
         * 요청한 프로세스를 s.queue에 연결
         * → 요청한 프로세스를 블록 상태로 전이
         */
    }
}
void semSignalB(binary_semaphore s){
    if(s.queue is empty()){
        s.value = one;
    } else {
        /*
         * s.queue에 연결돼 있는 프로세스를 큐에서 제거
         * → 프로세스의 상태를 실행 가능으로 전이시키고, Ready List에 연결
         */
    }
}
```

<br>

> ### 세마포어를 이용한 상호배제 예제

```c
const int n = /* 프로세스 개수 */
semaphore s = 1;

void p(int i){
    while(true){
        semWait(s);

        /* Critical Section */;
        semSignal(s);
        /* Critical Section 이후의 코드*/;
    }
}
void main() {
    parbigin (P(1), P(2), ... , P(n));
}
```

<br>

## 뮤텍스와 세마포어의 차이

- 동기화 대상의 `갯수`
  - 뮤텍스 → 1개
  - 세마포어 → n개
- 세마포어는 뮤텍스가 될 수 있음
  - Binary Semaphore == mutex
- `자원 소유` 관련
  - 뮤텍스 → 자원 소유가 가능하고, 책임을 가짐
  - 세마포어 → 자원 소유 불가능
- `헤제` 관련
  - 뮤텍스 → 소유하고 있는 스레드만이 뮤텍스 해제 가능
  - 세마포어 → 소유하고 있지 않는 스레드도 세마포어 해제 가능

<br>

---

### Reference

- [@reakwon](https://reakwon.tistory.com/98)
- [@chelseashin](https://chelseashin.tistory.com/40)
