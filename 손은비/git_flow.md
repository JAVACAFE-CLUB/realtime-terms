## Git

Git은 형상 관리 중 소스 코드의 버전 관리를 담당하는 도구

## Git Flow

Git을 이용한 협업 개발에서 branch를 어떻게 나누고 관리할지에 대한 표준화된 작업 흐름(== 브랜치 전략)

| branch name  | Purpose             |
| ------------ | ------------------- |
| master(main) | 실제 서비스 배포    |
| develop      | 개발                |
| feature      | 각 기능 개발        |
| release      | 배포 전 **QA**      |
| hotfix       | 배포 후 긴급한 버그 |

1.master(main) branch 로부터 develop branch 생성\
2.develop branch 로부터 각 feature branch 생성\
3.배포할 때는 develop -> master(main)이 아닌 release branch에서 QA 후 master(main)과 develop에 반영\
4.긴급한 버그일 경우 hotfix에서 수정 후 master(main)과 develop에 반영

\*\* 크게 위의 5가지 branch로 나뉘며 팀 컨벤션에 의해 추가 및 수정되기도 함\
ex) feature 대신 feat을 쓰자 -> feat/auth \
ex) branch에서 depth를 나눠서 더 구체적으로 기능을 나누자 -> feat/auth/Oauth\
ex) refactoring 하는 branch를 만들자 -> refactor/boardMng

## Git 기능 정리

**[git stash]**\
"지금 작업 중인 코드는 임시 저장할게!"

```
-- 변경사항 임시 저장 후 작업 디렉토리 초기화
git sash
-- 저장된 stash 목록
git stash list
-- 가장 최신 stash 불러오기
git stash apply
-- 가장 최신 stash 불러오며 목록에서 삭제
git stash pop
-- 특정 stash 불러오기, 삭제
git stash apply stash@{n}
git stash drop stash@{n}
```

<br>

**[git fetch]**\
"원격 저장소의 최신 변경사항을 로컬로 가져오되, 현재 작업중인 것은 변경하지 않을게"\
"언제 사용하는가?" -> "내 로컬 branch는 유지하되 remote의 최신 상태를 확인하고 싶을 때. pull하기 전 변화만 보고 싶을 때"\
\*\* `git pull`은 `fetch+merge` 라 바로 적용\
"git fetch는 충돌이 안날까?" -> "단순히 원격 저장소의 최신 커밋 기록을 가져오는 작업"

```
-- 원격 저장소의 모든 변경 내용 가져오기
git fetch
-- Origin 저장소의 모든 변경 내용 가져오기
git fetch origin
-- 특정 브랜치만 가져오기
git fetch origin 브랜치명

-- 병합 전 차이 확인
git diff HEAD origin/main

** fetch -> diff -> merge 순서로 진행하면 안정적으로 merge 가능
```

## 질문

1.release-3q, release-4q 브랜치를 동시에 진행하고 있을 때, 다음주 긴급 버그픽스가 필요하다면 어떻게 처리할지 논하시오.

```
1. hotfix branch를 main branch로 부터 생성한다.
2. 버그픽스 후에 main, develop에 반영한다.

-----

# 현재 작업중이었다면 main branch로 이동 후 branch 생성
git checkout main
git pull origin main
git checkout -b hotfix/[긴급한 버그 내용]

(뚝딱뚝딱)

# main에 반영
git checkout main
git merge hotfix/[긴급한 버그 내용]
git push origin main

# dev에 반영
git checkout dev
git merge hotfix/[긴급한 버그 내용]
git push origin dev
```

(궁금한 점) release branch에도 바로 반영해야 할까? 보통은 어떻게 할까?

<br>

2.이미 배포된 release를 8시간 후 롤백하고 싶을 때 어떻게 해야할지 논하시오.

```
# (중요) 작업 전 반드시 팀원들에게 공지하기 -> git stash, backup 요청

git checkout release

# git tag 작성한 경우
git reset --hard v1.5.0

# git tag 작성하지 않은 경우
git reset --hard 2asef6

# git push는 "앞으로 나아가는 변경"만 허용하기 때문에, rollback의 경우 --force 작성
git push origin release --force

** --force-with-lease
안전한 강제 푸시. 누군가 branch를 수정했다면 push를 막아줌
```

(궁금한 점 1) 8시간 배포동안 DB 변경이 있을텐데, DB 데이터 롤백 문제?
(궁금한 점 2) 무중단 배포 vs 중단 배포가 있을텐데, 롤백 난이도는 무중단 배포가 더 쉽다?

<br>

3.git flow 전략에서 cherry-pick이 필요한 경우와 cherry-pick 시 히스토리 관리를 어떻게 해야할지 논하시오.\
\*\* cherry-pick?
-> 다른 branch의 특정 커밋 하나를 선택해 현재 branch에 적용하는 명령어

```
[cherry-pick이 필요한 경우]
- 긴급하게 일부 파일/기능 적용이 필요한 경우(conflict과 같은 issue를 피하자)
- 해당 commit 내용을 내 feature에서도 실험해보고 싶을 때

[cherry-pick시 히스토리 관리]
- cherry-pick할 경우 동일 작업이라도 commit hash가 달라질 수 있으므로, 원본 commit 번호를 명시
- 무분별한 cherry-pick은 히스토리가 복잡해짐
```
