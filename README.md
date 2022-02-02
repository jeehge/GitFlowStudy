# GitFlowStudy

목표 : Git Flow 공부해보자!

<br>

빈센트 드리센(Vincent Driessen)의 branch model


Git flow가 사용하는 branch는 크게 두 가지가 있다.

프로젝트가 끝날 때 까지 항상 유지되는 `main branch`와 필요할 때만 사용하고 소멸되는 `sub branch`

`main branch`에는 `master`와 `develop`가 있고

`sub branch`에는 `feature`, `release`, `hotfix`가 있다

- master : 빌드가 되어야 함. 제품 출시 할 수 있은 branch.
- develop : 개발 branch. feature, release가 분리되고 병합 됨.
- feature : develop로 부터 개별 기능 개발하는 branch. develop 브랜치로 머지되어야 함.
- release : 출시 버전 준비하는 branch. develop와 master 브랜치에 머지되어야 함.
- hotfix : 출시 버전에서 발생한 버그를 수정하는 branch. master에서 분기

**hotfix와 release 는 master과 develop에 머지해준다**


git flow 공부하다보면 꼭 보는 유명한 그림이라 우선 첨부!


<img width="388" alt="스크린샷 2022-01-27 오후 3 39 25" src="https://user-images.githubusercontent.com/98507062/151305259-c96dc9c2-c0e8-40b1-aa9a-961f2638d90c.png">



**직접 해보자!** 

<br>

# Sourcetree

# Terminal

### Feature branch

- 새로운 feature 브랜치 생성

```
$ git checkout -b myfeature develop
# "myfeature" 브랜치 생성 후 전환
```

- 완료된 feature 브랜치를 develop 브랜치에 합치기

```
$ git checkout develop
# "develop" 브랜치로 전환

$ git merge --no-ff myfeature # --no-ff 플래그는 머지 시 새로운 커밋을 생성하도록 함
# Updating ea1b82a..02e9557
# (Summary of changes)

$ git branch -d myfeature
# 머지 후 해당 브랜치를 제거

$ git push origin develop
```

--no-ff 플래그를 사용하면 fast-forward(빠르게?) 머지할 수 있으며, 이전 feature에 대한 히스토리를 그룹화하여 남길 수 있도록 해준다. 아래의 그림은 --no-ff 플래그의 사용 여부에 따른 차이를 보여준다.

![스크린샷 2022-02-02 오후 11 29 28](https://user-images.githubusercontent.com/8108570/152173380-c04027f4-1c12-459a-a71c-0c5c483cccd4.png)


오른쪽의 경우 어떤 커밋들이 병합되었는지에 대한 git history를 확인하는 것이 불가능하다. 따라서 이 경우에는 모든 로그 메시지를 분석하여 어떤 feature에 대한 작업이 이루어졌는지를 수동으로 확인해야 하는 불편함이 있다. 


### Release branch

- Release 브랜치 생성

```
$ git checkout -b release-1.2 develop
# 새로운 브랜치 "release-1.2" 전환
# 위 명령어 이후 우리는 Xcode 또는 Android Studio에서 새로운 버전에 대한 버전넘버 및 빌드 번호를 1.2로 수정한다.
# Files modified successfully, version bumped to 1.2.

$ git commit -a -m "Bumped version number to 1.2"
# [release-1.2 74d9424] Bumped version number to 1.2
# 1 files changed, 1 insertions(+), 1 deletions(-)
```

위의 명령어를 통해 새로운 release 브랜치를 생성 및 커밋까지 진행하였다면 해당 브랜치에 대한 작업을 수행하면 된다. QA 등을 통해 발견된 bug-fix 등이 이 브랜치에서 수행될 수 있다. 큰 규모의 수정(기능 추가 등)은 절대 이 브랜치에서 수행하지 않도록 주의해야 한다.
이러한 작업이 모두 완료되었을 경우 다시 develop 브랜치로 머지한다.


- Release 브랜치 끝내기

브랜치가 실제 배포 가능한 상태까지 작업이 완료되었다면, 해당 브랜치를 master 브랜치에 머지하도록 한다 (물론 위에서 서술한 것 처럼 develop 브랜치에도 머지해야 한다)

master 브랜치에 release 브랜치를 머지하였다는 것은 다음 업데이트가 반영됐음을 의미한다. (git hook을 통해 자동 배포도 가능하다고 이야기 했음)

머지 후 해당 업데이트 버전으로 tag를 작성하면 해당 버전에 대한 업데이트가 마무리되고 다음 업데이트를 준비할 수 있다.






**참고링크**

[우린 Git-flow를 사용하고 있어요 | 우아한형제들 기술블로그](https://techblog.woowahan.com/2553/)

[git flow model](https://www.youtube.com/watch?v=EzcF6RX8RrQ)
