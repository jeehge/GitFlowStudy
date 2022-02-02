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



# **직접 해보자!**  -> ever 제공 pdf 자료

## Sourcetree

- 공부 진행해야 함

<br>

## Terminal

### Feature branch

* develop 브랜치로부터 분기
* develop 브랜치로 머지되어야 함
* Feature 브랜치는 *곧 또는 미래에 있을 업데이트에 대한 새로운 기능 개발*을 위해 사용된다. 이 브랜치가 생성되었다고 해서 해당 기능이 어떤 release에 반영될 지 바로 정해지는 것은 아니다. 이 브랜치는 정말 해당 기능이 개발 단게에 있지만, 추후 develop에 머지될 것이라는 것을 의미한다. (마찬가지로 반영될 수도 있고, 제거될 수도 있다.)

<br>

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

<br>

### Release branch

* develop 브랜치로부터 분기
* develop 및 master 브랜치로 머지되어야 함
* Release 브랜치는 *새로운 production release에 대한 준비*를 위하여 사용하는 브랜치이다. 마이너 버그 픽스, 버전 변경(프로젝트) 등의 메타데이터 변경에 대한 것도 이 브랜치에서 수행한다.
* develop 브랜치에서 release 브랜치로 분기하는 최적의 상황은 새로운 업데이트(release)에 대한 요구가 거의 다 반영되었을 경우이다. 최소한 새 릴리즈를 위해 개발된 feature 브랜치들이 develop에 머지된 상태여야 한다.
* develop 브랜치는 다음 업데이트에 대한 내용을 반영하고 있지만, 어떤 버전으로 출시될지에 대한 정보는 불분명하다. 따라서 develop 브랜치에 반영된 기능의 범위에 따라 release 브랜치 생성 시에 버전을 정해주도록 하는 것이 좋다.

- Release 브랜치 생성

Release 브랜치는 develop 브랜치로부터 생성된다. 에를들어 현재 버전이 1.1.5이고 다음 큰 규모의 업데이틀 준비하고 있다 가정하며, develop 브랜치가 release 하기 위한 준비가 되어 있을 경우 우리는 다음 버전을 1.2(1.1.6 또는 2.0이 아닌)로 정할 수 있을 것이다. 따라서 새로운 버전 넘버를 반영하여 아래와 같이 브랜치를 생성할 수 있다.

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

아래의 코드는 1.2 버전에 대한 release 브랜치 머지 및 태깅하는 명령어이다. (태깅은 일반적으로 릴리즈 시에 작성한다고 나와있으나, git 또는 프로젝트에서 어떤 의미로 사용되는지 알아보아야 함)

```
- Master 브랜치에 머지 및 태깅
$ git checkout master
# 'master' 브랜치로 전환

$ git merge --no-ff release-1.2
# release 브랜치 머지

$ git tag -a 1.2


- Develop 브랜치에 머지
$ git checkout develop
# 'develop' 브랜치로 전환

$ git merge --no-ff release-1.2
# 머지


- 위의 두 과정을 마친 후 release 브랜치를 제거해주도록 하자
$ git branch -d release-1.2
# Deleted branch release-1.2 (was ff452fe).
```

<br>

### Hotfix branch

* master 브랜치로부터 분기
* develop 및 master 브랜치로 머지되어야 함
* 핫픽스 블내치는 의도치 않게 새로운 production release를 만든다는 점에서 release 블내치와 비슷한 면이 있다. 이 브랜치는 현재 배포된 버전에서 *긴급하게 발견된 오류 또는 의도치 않은 상황에 대한 수정*이 필요한 경우 사용한다.


![스크린샷 2022-02-03 오전 12 38 47](https://user-images.githubusercontent.com/8108570/152186180-3a67e341-3841-4f5d-b12a-13893d68b10c.png)

위의 그림에서 확인할 수 있듯이, 릴리즈 브랜치와 비슷하지만 정말 긴급한 수정사항이 있을 때 master branch에서 분기하여 작업 후 develop 및 master 브랜치로 머지하게 된다. 


- Hotfix branch 생성하기

예를들어 1.2 버전에 대한 hotfix를 만들어야 한다면, 마스터 브랜치에서 문제가 발생한 버전에 블내치를 생성하여 체크아웃한다.

```
$ git checkout -b hotfix-1.2.1 master
# 새 브랜치 "hotfix-1.2.1" 생성 및 전환
# 1.2.1 버전으로 수정 (Xcode 또는 AndroidStudio)

$ git commit -a -m "Bumped version number to 1.2.1"
# [hotfix-1.2.1 41e61bb] Bumped version number to 1.2.1 
# 1 files changed, 1 insertions(+), 1 deletions(-)
```

브랜치 생성 후 버전을 올리는 것을 절대 잊지 말고 작업ㅇ르 진행하도록 하자

위의 작업을 통해 새 hotfix 브랜치 생성 및 버그 수정에 대한 커밋이 모두 완료되었다면, 다시 master 및 develop 브랜치로 머지해준다.

```
- 마스터 브랜치에 머지 및 태깅
$ git checkout master
# 'master'로 전환

$ git merge --no-ff hotfix-1.2.1
# hotfix 머지

$ git tag -a 1.2.1
# 해당 버전에 대한 태그 지정


- 디벨롭 브랜치에 머지
$ git checkout develop
# 'develop' 로 전환

$ git merge --no-ff hotfix-1.2.1
# hotfix 머지


- 브랜치 제거
$ git branch -d hotfix-1.2.1
# Deleted branch hotfix-1.2.1 (was abbe5d6)
```


*여기서 만약 버그 픽스 이전에 이미 해당 업데이트 버전에 대한 Release 브랜치가 있을 경우* hotfix에 대한 변경 사항을 develop 브랜치가 아닌 release 브랜치에 머지하도록 한다. 이렇게 하면 hotfix에 대한 수정사항도 포함하여 release 머지 시 develop 및 master 브랜치에 포함될 것이다.  



**참고링크**

[A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)

[우린 Git-flow를 사용하고 있어요 | 우아한형제들 기술블로그](https://techblog.woowahan.com/2553/)

[git flow model](https://www.youtube.com/watch?v=EzcF6RX8RrQ)
