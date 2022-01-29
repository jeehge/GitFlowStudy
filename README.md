# GitFlowStudy

목표 : Git Flow 공부해보자!

**직접 해보자!** 
- Sourcetree
- Terminal

<br>

빈센트 드리센(Vincent Driessen)의 branch model


Git flow가 사용하는 branch는 크게 두 가지가 있다.

프로젝트가 끝날 때 까지 항상 유지되는 `main branch`와 필요할 때만 사용하고 소멸되는 `sub branch`

`main branch`에는 `master`와 `develop`가 있고

`sub branch`에는 `feature`, `release`, `hotfix`가 있다

- master : 빌드가 되어야 함. 제품 출시 할 수 있은 branch.
- develop : 개발 branch. feature, release가 분리되고 병합 됨.
- feature : develop로 부터 개별 기능 개발하는 branch.
- release : 출시 버전 준비하는 branch. 
- hotfix : 출시 버전에서 발생한 버그를 수정하는 branch. master에서 분기

**hotfix와 release 는 master과 develop에 머지해준다**


git flow 공부하다보면 꼭 보는 유명한 그림이라 우선 첨부!


<img width="388" alt="스크린샷 2022-01-27 오후 3 39 25" src="https://user-images.githubusercontent.com/98507062/151305259-c96dc9c2-c0e8-40b1-aa9a-961f2638d90c.png">



**참고링크**

[우린 Git-flow를 사용하고 있어요 | 우아한형제들 기술블로그](https://techblog.woowahan.com/2553/)

[git flow model](https://www.youtube.com/watch?v=EzcF6RX8RrQ)
