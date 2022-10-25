## 깃 플로우 (Git Flow)

: [Viencent Driessen](https://nvie.com/posts/a-successful-git-branching-model/)이라는 사람의 블로그 글에 의해 퍼진 Git을 이용한 개발 방법론

- 기능이 아닌 `방법론`
- 각자 개발 환경에 따라 수정 및 변형하여 사용

> ### Git-Flow의 5가지 브랜치

- `master`
  - 기준이 되는 브랜치
  - 제품을 `배포`
- `develop`
  - 개발 브랜치
  - 이 브랜치를 기준으로 각자 작업한 기능을 Merge
- `feature`
  - 단위 기능을 개발하는 브랜치
  - 기능 개발 후 develop 브랜치에 Merge
- `release`
  - 배포를 위해 master 브랜치에 보내기 전에 `QA`를 하기 위한 브랜치
- `hotfix`
  - master 브랜치로 배포했을 때 `긴급 수정`하는 브랜치

<br>

> ### Vincent Driessen이 소개한 Git-Flow

<p align=center>
    <img src='../../resources/git/gitFlow.png' width=600>
</p>

1. `Master` 브랜치와 `develop` 브랜치를 나누는 것이 핵심
2. `develop` 브랜치를 현재 개발 완료 상태와 일치시키면서 동료와의 `conflict를 방지`하기 위해 `feature` 브랜치 이용
   - `develop`에서 `feature` 브랜치를 생성해서 새로운 작업 수행
   - 작업 종료 후 `feature` 브랜치를 최신 `develop`에 merge
3. 모든 기능 완료 후 `release` 브랜치를 만들어 QA를 하며 `보완` 및 `버그 픽스` 수행
4. 완료 후 release 브랜치를 `develop` 브랜치와 `master` 브랜치로 보냄
5. `master` 브랜치에서 버전 추가를 위해 태그를 하나 생성하고 배포
6. 배포 후 갑작스런 버그 발견 및 발생 시 `hotfix` 브랜치를 만들어 `긴급 수정` 후 태그를 생성하고 바로 `수정 배포`

---

### 참고 자료

- [@nvie](https://nvie.com/posts/a-successful-git-branching-model/)
- [@nJo2](https://ux.stories.pe.kr/183)
- [@gangnamunni](https://blog.gangnamunni.com/post/understanding_git_flow/)
