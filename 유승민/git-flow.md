# 기본적인 git 명령어

## 🚀 초기 설정

### 사용자 정보 설정

```bash
git config --global user.name "이름"
git config --global user.email "이메일@example.com"
```

### 설정 확인

```bash
git config --list
git config user.name
git config user.email
```

---

## 📁 저장소 생성 및 복제

### 로컬 저장소 초기화

```bash
git init
```

### 원격 저장소 복제

```bash
git clone <저장소_URL>
git clone <저장소_URL> <폴더명>
```

---

## 👀 파일 상태 확인

### 상태 확인

```bash
git status                  # 현재 상태 확인
git status -s              # 짧은 형식으로 상태 확인
```

### 변경 사항 확인

```bash
git diff                    # 수정된 파일의 변경 내용 확인
git diff --staged           # 스테이징된 파일의 변경 내용 확인
git diff HEAD               # 마지막 커밋과 현재 작업 디렉토리 비교
```

---

## ✅ 파일 추가 및 커밋

### 파일 스테이징

```bash
git add <파일명>             # 특정 파일 추가
git add .                   # 모든 파일 추가
git add *.js               # 특정 확장자 파일만 추가
git add -A                 # 모든 변경사항 추가 (삭제된 파일 포함)
```

### 커밋

```bash
git commit -m "커밋 메시지"   # 커밋 생성
git commit -am "메시지"      # 스테이징과 커밋을 동시에
git commit --amend          # 마지막 커밋 수정
```

---

## 🌿 브랜치 관리

### 브랜치 확인

```bash
git branch                  # 로컬 브랜치 목록
git branch -r              # 원격 브랜치 목록
git branch -a              # 모든 브랜치 목록
```

### 브랜치 생성 및 전환

```bash
git branch <브랜치명>        # 브랜치 생성
git checkout <브랜치명>      # 브랜치 전환
git checkout -b <브랜치명>   # 브랜치 생성 후 전환
git switch <브랜치명>        # 브랜치 전환 (Git 2.23+)
git switch -c <브랜치명>     # 브랜치 생성 후 전환 (Git 2.23+)
```

### 브랜치 병합 및 삭제

```bash
git merge <브랜치명>         # 현재 브랜치에 다른 브랜치 병합
git branch -d <브랜치명>     # 브랜치 삭제 (병합된 브랜치만)
git branch -D <브랜치명>     # 브랜치 강제 삭제
```

---

## 🌐 원격 저장소

### 원격 저장소 관리

```bash
git remote                  # 원격 저장소 목록
git remote -v              # 원격 저장소 URL 포함 목록
git remote add <이름> <URL> # 원격 저장소 추가
git remote remove <이름>    # 원격 저장소 제거
```

### 푸시와 풀

```bash
git push                    # 현재 브랜치를 원격으로 푸시
git push origin <브랜치명>   # 특정 브랜치 푸시
git push -u origin <브랜치명> # 업스트림 설정하며 푸시
git pull                    # 원격 저장소에서 변경사항 가져와서 병합
git fetch                   # 원격 저장소에서 변경사항만 가져오기
```

---

## 📜 히스토리 조회

### 로그 확인

```bash
git log                     # 커밋 히스토리 확인
git log --oneline          # 한 줄로 커밋 히스토리
git log --graph            # 그래프로 브랜치 히스토리
git log --all --graph --oneline # 모든 브랜치 그래프
git log -p                 # 변경 내용과 함께 로그
git log --author="이름"     # 특정 작성자의 커밋만
```

### 특정 커밋 확인

```bash
git show <커밋_해시>        # 특정 커밋의 상세 정보
git show HEAD              # 최신 커밋 정보
git show HEAD~1            # 이전 커밋 정보
```

---

## ⏪ 되돌리기

### 작업 디렉토리 되돌리기

```bash
git checkout -- <파일명>    # 파일 변경사항 취소
git restore <파일명>        # 파일 변경사항 취소 (Git 2.23+)
git checkout .             # 모든 변경사항 취소
```

### 스테이징 취소

```bash
git reset HEAD <파일명>     # 특정 파일 스테이징 취소
git restore --staged <파일명> # 스테이징 취소 (Git 2.23+)
git reset HEAD             # 모든 스테이징 취소
```

### 커밋 되돌리기

```bash
git reset --soft HEAD~1    # 커밋만 취소 (변경사항 유지)
git reset --mixed HEAD~1   # 커밋과 스테이징 취소
git reset --hard HEAD~1    # 모든 변경사항 완전 취소
git revert <커밋_해시>      # 특정 커밋을 되돌리는 새 커밋 생성
```

---

## 🛠️ 기타 유용한 명령어

### Stash (임시 저장)

```bash
git stash                  # 현재 작업 임시 저장
git stash save "메시지"      # 메시지와 함께 임시 저장
git stash list             # 저장된 stash 목록
git stash pop              # 가장 최근 stash 적용 후 삭제
git stash apply            # stash 적용 (삭제하지 않음)
git stash drop             # stash 삭제
```

### 태그

```bash
git tag                     # 태그 목록
git tag <태그명>            # 태그 생성
git tag -a <태그명> -m "메시지" # 주석 태그 생성
git push origin <태그명>    # 태그 푸시
git push origin --tags     # 모든 태그 푸시
```

### 파일 무시

```bash
# .gitignore 파일에 추가
*.log                      # 모든 .log 파일 무시
node_modules/             # 폴더 무시
!important.log            # 특정 파일은 추적
```

---

## 💡 자주 사용하는 워크플로우

### 기본 작업 흐름

```bash
git status                 # 1. 상태 확인
git add .                  # 2. 변경사항 스테이징
git commit -m "메시지"     # 3. 커밋
git push                   # 4. 원격으로 푸시
```

### 새 기능 개발

```bash
git checkout -b feature/새기능  # 1. 새 브랜치 생성
# 개발 작업...
git add .                      # 2. 변경사항 추가
git commit -m "새 기능 추가"   # 3. 커밋
git checkout main              # 4. main 브랜치로 전환
git merge feature/새기능       # 5. 병합
git branch -d feature/새기능   # 6. 브랜치 삭제

```

# git flow 명령어

## 🌟 Git Flow 소개

- Git Flow는 Git 브랜치 모델을 구현한 도구

### 주요 브랜치 타입

- **main/master**: 프로덕션 배포용 브랜치
- **develop**: 개발 통합 브랜치
- **feature**: 새 기능 개발 브랜치
- **release**: 릴리즈 준비 브랜치
- **hotfix**: 긴급 수정 브랜치
- **support**: 장기 지원 브랜치

## 🚀 설치 및 초기화

### Git Flow 설치

```bash
# macOS (Homebrew)
brew install git-flow-avh

# Ubuntu/Debian
sudo apt-get install git-flow

# Windows (Git for Windows 포함)
# 별도 설치 필요 없음
```

### 저장소 초기화

```bash
git flow init                # 대화형 초기화
git flow init -d            # 기본값으로 초기화
```

### 초기화 설정 확인

```bash
git branch                  # 브랜치 확인 (main, develop 생성됨)
git flow version           # Git Flow 버전 확인
```

---

## 🔧 Feature 브랜치

새로운 기능을 개발할 때 사용하는 브랜치

### Feature 시작

```bash
git flow feature start <feature-name>      # 새 feature 브랜치 생성 및 전환
git flow feature start login-system        # 예시: 로그인 시스템 기능
```

### Feature 작업 중

```bash
git add .                                  # 변경사항 스테이징
git commit -m "커밋 메시지"                # 커밋
git flow feature publish <feature-name>    # 원격에 feature 브랜치 푸시
```

### Feature 완료

```bash
git flow feature finish <feature-name>     # feature를 develop에 병합 후 브랜치 삭제
git flow feature finish login-system       # 예시
```

### Feature 목록 및 상태

```bash
git flow feature list                      # 모든 feature 브랜치 목록
git flow feature                          # 현재 feature 브랜치 상태
```

### Feature 가져오기

```bash
git flow feature pull origin <feature-name>  # 원격 feature 브랜치 가져오기
git flow feature track <feature-name>        # 원격 feature 브랜치 추적
```

---

## 📦 Release 브랜치

배포 준비를 위한 브랜치

### Release 시작

```bash
git flow release start <version>           # 새 release 브랜치 생성
git flow release start 1.0.0              # 예시: 버전 1.0.0 릴리즈
git flow release start v2.1.0             # 예시: v 접두사 포함
```

### Release 작업 중

```bash
# 버그 수정, 문서 업데이트 등
git add .
git commit -m "릴리즈 준비: 버그 수정"
git flow release publish <version>         # 원격에 release 브랜치 푸시
```

### Release 완료

```bash
git flow release finish <version>          # release를 main과 develop에 병합
git flow release finish 1.0.0              # 태그 생성 후 브랜치 삭제
```

### Release 목록

```bash
git flow release list                      # 모든 release 브랜치 목록
git flow release                          # 현재 release 브랜치 상태
```

---

## 🚨 Hotfix 브랜치

프로덕션 환경의 긴급한 버그를 수정할 때 사용

### Hotfix 시작

```bash
git flow hotfix start <version>            # main 브랜치에서 hotfix 생성
git flow hotfix start 1.0.1               # 예시: 긴급 패치 버전
git flow hotfix start hotfix-critical-bug  # 예시: 설명적 이름 사용
```

### Hotfix 작업 중

```bash
# 긴급 버그 수정
git add .
git commit -m "긴급 수정: 로그인 오류 해결"
git flow hotfix publish <version>          # 원격에 hotfix 브랜치 푸시
```

### Hotfix 완료

```bash
git flow hotfix finish <version>           # main과 develop에 병합 후 태그 생성
git flow hotfix finish 1.0.1              # 브랜치 삭제
```

### Hotfix 목록

```bash
git flow hotfix list                       # 모든 hotfix 브랜치 목록
git flow hotfix                           # 현재 hotfix 브랜치 상태
```

---

## 🛡️ Support 브랜치

장기간 지원이 필요한 이전 버전을 위한 브랜치

### Support 시작

```bash
git flow support start <version> <base>    # 특정 태그에서 support 브랜치 생성
git flow support start 1.x 1.0.0          # 예시: 1.0.0 태그에서 1.x 지원 브랜치
```

### Support 목록

```bash
git flow support list                      # 모든 support 브랜치 목록
git flow support                          # 현재 support 브랜치 상태
```

---

## 🔧 기타 명령어

### 설정 확인

```bash
git flow config                           # Git Flow 설정 확인
git config --get gitflow.branch.master   # main 브랜치 이름 확인
git config --get gitflow.branch.develop  # develop 브랜치 이름 확인
```

### 브랜치 접두사 설정

```bash
git config gitflow.prefix.feature "feat/"     # feature 브랜치 접두사
git config gitflow.prefix.release "rel/"      # release 브랜치 접두사
git config gitflow.prefix.hotfix "fix/"       # hotfix 브랜치 접두사
```

### 원격 작업

```bash
git push origin develop                   # develop 브랜치 푸시
git push origin main                      # main 브랜치 푸시
git push origin --tags                   # 모든 태그 푸시
```

---

## 🔄 Git Flow 워크플로우

### 1. 새 기능 개발 플로우

```bash
# 1. 새 기능 시작
git flow feature start user-authentication

# 2. 기능 개발
# 코드 작성...
git add .
git commit -m "사용자 인증 기능 구현"

# 3. 원격에 공유 (선택적)
git flow feature publish user-authentication

# 4. 기능 완료 (develop에 병합)
git flow feature finish user-authentication

```

### 2. 릴리즈 플로우

```bash
# 1. 릴리즈 시작
git flow release start 2.0.0

# 2. 릴리즈 준비 (버그 수정, 문서 업데이트)
git add .
git commit -m "릴리즈 2.0.0 준비완료"

# 3. 릴리즈 완료 (main, develop 병합 및 태그 생성)
git flow release finish 2.0.0

# 4. 태그와 브랜치 푸시
git push origin main
git push origin develop
git push origin --tags
```

### 3. 핫픽스 플로우

```bash
# 1. 긴급 수정 시작
git flow hotfix start 2.0.1

# 2. 버그 수정
git add .
git commit -m "긴급 수정: 결제 오류 해결"

# 3. 핫픽스 완료 (main, develop 병합)
git flow hotfix finish 2.0.1

# 4. 변경사항 푸시
git push origin main
git push origin develop
git push origin --tags
```

---

## 💡 Git Flow vs 일반 Git 비교

### 일반 Git 명령어와 Git Flow 비교

| 작업 | 일반 Git | Git Flow |
| --- | --- | --- |
| 기능 브랜치 생성 | git checkout -b feature/login | git flow feature start login |
| 기능 완료 | git checkout develop && git merge feature/login | git flow feature finish login |
| 릴리즈 생성 | git checkout -b release/1.0.0 develop | git flow release start 1.0.0 |
| 핫픽스 생성 | git checkout -b hotfix/1.0.1 main | git flow hotfix start 1.0.1 |

---

## ⚠️ 주의사항 및 팁

### 주의사항

- Git Flow를 사용하기 전에 팀원 모두가 동일한 브랜치 전략에 동의해야 함
- `git flow finish` 명령어는 되돌릴 수 없으므로 신중하게 사용
- 원격 저장소와 동기화할 때는 별도로 push 작업이 필요

### 유용한 팁

```bash
# 현재 어떤 Git Flow 브랜치에 있는지 확인
git branch | grep -E "(feature|release|hotfix)"

# Git Flow 브랜치 정리 (완료된 브랜치들 삭제)
git branch -d $(git branch | grep -E "feature|release|hotfix")

# 모든 원격 브랜치 확인
git branch -r | grep -E "(feature|release|hotfix)"

```

---

## 🎯 Git Flow 모범 사례

### 커밋 메시지 컨벤션

```bash
git commit -m "feat: 사용자 로그인 기능 추가"
git commit -m "fix: 결제 시스템 버그 수정"
git commit -m "docs: API 문서 업데이트"
git commit -m "refactor: 코드 구조 개선"

```

### 브랜치 네이밍 컨벤션

```bash
git flow feature start user-auth          # kebab-case 사용
git flow feature start payment-integration
git flow release start v1.2.0             # 버전 네이밍
git flow hotfix start critical-security-fix

```

# git flow 정리

| **main** | **가장 마지막 릴리즈 버전을 바라보고 있는, 이미 출시(배포) 된 브랜치** |
| --- | --- |
| **develop** | **다음 출시 버전을 개발하는 브랜치** |
| **feature** | **새 기능(feature) 개발하는 브랜치** : 버그를 수정 개발하는 브랜치 |
| **release** | **릴리즈 배포를 위한 브랜치** : 버저닝 작업과 Prod 환경 QA에서 나온 버그 수정 |
| **hotfix** | **배포된 릴리즈 버전에 문제가 있을 때 즉각 대응을 위한 브랜치** : 릴리즈 버전에 대응하는 문제 버그만 수정 |

### case1. Rebase 충돌 해결

- 상황 설명 : feature 브랜치에서 작업 후 main 브랜치를 rebase하려고 했는데 충돌이 발생
- 문제 발생 과정

    ```bash
    # 상황 재현
    git checkout feature/user-profile
    git rebase main
    
    # 출력: 충돌 발생!
    # CONFLICT (content): Merge conflict in src/components/UserProfile.js
    # error: could not apply abc1234... 사용자 프로필 UI 개선
    ```

- 해결 단계
    - 1단계: 충돌 파일 확인

        ```bash
        git status
        # 충돌된 파일 확인
        
        git diff
        # 충돌 내용 상세 확인
        ```

    - 2단계: 충돌 해결

        ```bash
        # 충돌된 파일 수동 편집
        # <<<<<<< HEAD
        # 현재 브랜치의 내용
        # =======
        # 적용하려는 커밋의 내용
        # >>>>>>> abc1234 (사용자 프로필 UI 개선)
        
        # 에디터에서 충돌 마커 제거 후 원하는 내용으로 수정
        ```

    - 3단계: 충돌 해결 완료

        ```bash
        # 수정된 파일을 스테이징
        git add src/components/UserProfile.js
        
        # rebase 계속 진행
        git rebase --continue
        
        # 만약 더 많은 충돌이 있다면 1-3단계 반복
        ```

    - 4단계: Rebase 완료 후 확인

        ```bash
        # 로그 확인
        git log --oneline --graph
        
        # 원격 저장소에 강제 푸시 (주의: 팀원과 상의 후)
        git push origin feature/user-profile --force-with-lease
        ```


### case2. 잘못된 커밋 되돌리기

- 상황 설명 : 실수로 잘못된 코드를 커밋했고, 이미 원격 저장소에 푸시까지 완료 → 다른 팀원들도 이 변경사항을 받았을 수 있음
- 문제 발생 과정

    ```bash
    # 잘못된 커밋 생성
    git add .
    git commit -m "프로덕션 데이터베이스 설정 변경"
    git push origin main
    
    # 나중에 발견: 실제 프로덕션 DB 정보가 포함되어 보안 문제!
    ```

- 해결 방법 : Revert 사용 (안전한 방법)
    - 1단계: 문제 커밋 식별

        ```bash
        # 최근 커밋 로그 확인
        git log --oneline -5
        
        # 출력 예시:
        # a1b2c3d (HEAD -> main) 프로덕션 데이터베이스 설정 변경  ← 문제 커밋
        # e4f5g6h 사용자 인증 기능 추가
        # i7j8k9l README 문서 업데이트
        ```

    - 2단계: Revert 실행

        ```bash
        # 문제 커밋을 되돌리는 새로운 커밋 생성
        git revert a1b2c3d
        
        # 에디터가 열리면 revert 커밋 메시지 작성
        # "Revert '프로덕션 데이터베이스 설정 변경' - 보안 문제로 인한 되돌림"
        ```

    - 3단계: 변경사항 푸시

        ```bash
        git push origin main
        # 팀원들에게 알림: "보안 문제로 인해 커밋을 되돌렸습니다"
        ```


### **case3. 처음 프로젝트 셋팅부터 구현, 배포까지 하는 앱팀**

1. 팀 리더가 new project 생성

    ```bash
    mkdir project
    ```

2. 팀 리더가 git 공용 repository에 올림

    ```bash
    git init
    git remote add origin {remote_url}
    git push origin main
    ```

3. 나머지 팀원들은 공용 repository에 올라 온 project 로컬에 가져옴

    ```bash
    git clone {remote_url}
    ```

4. 전 팀원 git flow 도구 사용해서 init (초기화) 해줌

    ```bash
    git flow init
    ```

5. 폭풍 엔터로 브랜치 명명규칙은 기본값을 따라줌
6. 모든 팀원 로컬 환경에서 처음에 main 브랜치 뿐만 있었는데 develop 브랜치가 로컬에 생겨남
7. 각자 맡은 작업을 시작 하기 위해 git flow 도구 사용해서 feature 브랜치 생성

    ```bash
    git flow feature start {branch_name}
    ```

8. (( feature 폭풍 작업 ))
9. 작업 완료 후 feature 브랜치를 push 해서 PR 올림

    ```bash
    git push origin feature/{branch_name}
    ```

10. PR 타이틀, 설명 등 작성 후 리뷰어 팀원들 등록
11. PR 승인 되면 Squash and Merge 버튼을 통해 압축 된 하나의 커밋으로 develop에 머지

    ```bash
    git checkout develop
    git merge --squash {branch_name}
    ```

12. 그리고 기존 feature 브랜치는 삭제

    ```bash
    git branch -d {branch_name}
    ```

13. 또 다른 feature 작업을 위해 가장 마지막으로 업데이트 된 develop 브랜치를 pull

    ```bash
    git pull origin develop
    ```

14. (( 7~13번 무한 반복 ))
15. 어느정도 팀이 원하는 작업을 다 완료 해서 릴리즈 배포할 때가 됐다면 릴리즈를 담당 할 한명을 정함
16. 현 버전 릴리즈 담당 1명 / 나머지는 그 다음 버전 릴리즈를 위한 feature 무한 작업
17. 릴리즈 담당자는 git flow 도구 사용해서 release 브랜치 생성

    ```bash
    git flow release start v1.0.0
    ```

18. (( 버저닝 작업 ))
19. release 브랜치 기반으로 배포 파일을 만듬
20. 배포 = 심사 요청
21. 심사 요청 후 이상이 있다면? 릴리즈 브랜치 내 에서 수정
22. 심사를 모두 무사히 통과해 정상적으로 배포 완료까지 수행
23. git flow 도구 사용해서 release 브랜치 finish 수행

    ```bash
    git flow release finish 'v1.0.0'
    ```

24. 릴리즈 브랜치 이름으로 태그도 추가
25. git flow 도구로 인해 자동으로 릴리즈 브랜치는 삭제되며, main에 체크아웃
26. main 브랜치 + 버저닝 태그와 함께 푸시

    ```bash
    git push --tags
    ```

27. develop 브랜치에 체크아웃하고 main 브랜치 내용을 백머지

    ```bash
    git checkout develop
    git merge main
    ```

28. develop 브랜치 푸시

    ```bash
    git push origin develop
    ```

29. (( 7~28번 무한 반복 ))

### **case4. 프러덕션에 올라가있는 앱에서 버그를 발견한 앱 개발자**

(( 내 로컬 프로젝트에 git flow init(초기화)가 되어있는 가정 ))

1. 호들짝 놀래며 자책하기
2. 참담한 현 상황을 슬랙 채널을 통해 슬픈 소식을 침착하게 알림
3. 기존 작업하고 있던 걸 중단
4. main 브랜치로 체크아웃

    ```bash
    git checkout main
    ```

5. pull 받아서 최신 main 브랜치를 바라봄

    ```bash
    git pull origin main
    ```

6. git flow 도구 사용해서 hotfix 브랜치 생성

    ```bash
    git flow hotfix start v1.0.1_bugname
    ```

7. (( 버그 수정 침착하게 뚝딱뚝딱 ))
8. (( 버저닝 작업 ))
9. hotfix 브랜치 기반으로 배포 파일을 만듬
10. 배포 = 심사 요청
11. 심사 요청 후 이상이 있다면? 릴리즈 브랜치 내 에서 수정
12. 심사를 모두 무사히 통과해 정상적으로 배포 완료까지 수행
13. git flow 도구 사용해서 hotfix 브랜치 finish 수행

    ```bash
    git flow hotfix finish 'v1.0.1_bugname'
    ```

14. hotfix 브랜치 이름으로 태그도 추가
15. git flow 도구로 인해 자동으로 hotfix 브랜치에 했던 작업을 main 브랜치와 develop 브랜치에 머지를 해주고 develop 브랜치로 체크아웃
16. develop 브랜치 최종사항을 버저닝 태그와 함께 푸시

    ```bash
    git push origin develop --tags
    ```

17. main 브랜치도 체크아웃해서 푸시

    ```bash
    git checkout main
    git push origin main --tags
    ```

18. develop 브랜치에 체크아웃하고 main 브랜치 내용을 백머지

    ```bash
    git checkout develop
    git merge main
    ```

19. develop 브랜치 푸시

    ```bash
    git push origin develop
    ```

20. 다시 develop 브랜치에 체크아웃하고 멈췄던 작업을 다시수행

    ```bash
    git checkout develop
    ```


### case5: 누군가 develop 을 건드렸다!!!

- 상황 설명
    - 팀장이 develop 브랜치의 히스토리 정리를 위해 강제 푸시
    - 이때 팀원 A는 `feature/user-dashboard` 브랜치에서, 팀원 B는 `feature/payment-system` 브랜치에서 각각 작업 중
    - 두 feature 브랜치 모두 강제 푸시 이전의 develop 브랜치를 기반으로 하고 있어서 동기화 문제가 발생
- 문제 발생 과정

    ```bash
    *# 팀장이 실행한 작업*
    git checkout develop
    git rebase -i HEAD~5  *# 커밋 히스토리 정리 (커밋 합치기, 메시지 수정 등)*
    git push origin develop --force-with-lease
    
    *# 팀원들의 상황# 팀원 A - feature/user-dashboard 브랜치에서 작업 중*
    git checkout feature/user-dashboard
    git rebase develop
    *# error: could not apply abc1234... 대시보드 컴포넌트 추가# CONFLICT (content): Merge conflict in src/components/Dashboard.js
    
    # 팀원 B - feature/payment-system 브랜치에서 작업 중*  
    git checkout feature/payment-system
    git merge develop
    *# error: Your local changes would be overwritten by merge.*
    ```

- 팀원 A 상황: feature/user-dashboard (Rebase 진행 중 충돌)
    - 1단계: 현재 상황 파악

        ```bash
        *# 현재 브랜치와 상태 확인*
        git status
        *# On branch feature/user-dashboard
        # You are currently rebasing branch 'feature/user-dashboard' on 'abc1234'.
        # Unmerged paths: src/components/Dashboard.js
        # 충돌 내용 확인*
        git diff
        ```

    - 2단계: Rebase 중단 후 새로운 접근

        ```bash
        *# 현재 rebase 중단*
        git rebase --abort
        
        *# 원격에서 최신 main 가져오기*
        git fetch origin
        git checkout main
        git reset --hard origin/main
        
        *# feature 브랜치의 현재 작업 백업*
        git checkout feature/user-dashboard
        git branch backup-user-dashboard  *# 백업 브랜치 생성*
        ```

    - 3단계: Feature 브랜치를 새로운 main 기반으로 재생성

        ```bash
        *# 새로운 main을 기반으로 feature 브랜치 재생성*
        git checkout main
        git checkout -b feature/user-dashboard-new
        
        *# 기존 feature 브랜치의 커밋들을 하나씩 적용*
        git log backup-user-dashboard --oneline
        *# 예시 출력:
        # def4567 대시보드 UI 개선
        # abc1234 대시보드 컴포넌트 추가
        # 987feda (origin/main) 이전 main 커밋
        
        # 필요한 커밋들을 cherry-pick으로* feature/user-dashboard-new 으로
        # 뽑아 넣음
        git cherry-pick abc1234  *# 대시보드 컴포넌트 추가*
        git cherry-pick def4567  *# 대시보드 UI 개선
        
        # 충돌 발생 시 해결 후 계속 진행
        # git add .
        # git cherry-pick --continue*
        ```

    - 4단계: 기존 브랜치 교체

        ```bash
        *# 기존 feature 브랜치 삭제 후 새 브랜치로 교체*
        git branch -D feature/user-dashboard
        git branch -m feature/user-dashboard-new feature/user-dashboard
        
        *# 원격에 강제 푸시 (feature 브랜치이므로 안전)*
        git push origin feature/user-dashboard --force-with-lease
        ```

- 팀원 B 상황: feature/payment-system (Merge 시도 중 오류)
    - 1단계: 현재 작업 상태 보존

        ```bash
        *# 현재 브랜치에서 작업 중인 내용 커밋*
        git checkout feature/payment-system
        git add .
        git commit -m "WIP: 결제 API 연동 작업 중"
        
        *# 백업 브랜치 생성*
        git branch backup-payment-system
        ```

    - 2단계: 최신 main 브랜치로 업데이트

        ```bash
        *# 최신 main 브랜치 가져오기*
        git fetch origin
        git checkout main
        git reset --hard origin/main
        ```

    - 3단계: Feature 브랜치 리베이스 (Interactive 사용)

        ```bash
        git checkout feature/payment-system
        
        *# Interactive rebase로 정리하면서 새 main에 적용
        # 자동으로 변경사항을 유지하면서 새로운 base 위에 커밋들을 다시 적용*
        git rebase -i main
        
        *# 에디터에서 커밋 정리
        # pick 1234567 결제 시스템 기본 구조
        # pick 2345678 결제 API 엔드포인트 추가  
        # pick 3456789 결제 검증 로직 구현
        # squash 4567890 WIP: 결제 API 연동 작업 중
        
        # 충돌 발생 시 해결*
        git status  *# 충돌 파일 확인
        
        # 파일 수정 후*
        git add .
        git rebase --continue
        ```

    - 4단계: 원격 브랜치 업데이트

        ```bash
        *# 원격 feature 브랜치 업데이트*
        git push origin feature/payment-system --force-with-lease
        ```


# Challenge Case

### release-3q, release-4q 브렌치를 동시에 진행하고 있을때 다음주 긴급 버그픽스가 필요하다면 어떻게 처리할지 논하시오.

- 상황 분석
    - **현재 상태**: `release-3q`, `release-4q` 브랜치가 동시 진행 중
    - **문제**: 다음주 긴급 버그픽스 필요
    - **고려사항**: 어느 브랜치에서 시작할지, 다른 브랜치들에 어떻게 반영할지
- **문제 해결**
    - **버그가 main/develop에서 발생한 경우**

        ```bash
        # 1. main에서 hotfix 브랜치 생성
        git checkout main
        git flow hotfix start 3.2.1-critical
        
        # 2. 버그 수정
        # 코드 수정...
        git add .
        git commit -m "fix: 긴급 보안 취약점 수정"
        
        # 3. hotfix 완료 (main과 develop에 자동 병합)
        git flow hotfix finish 3.2.1-critical
        
        ```

    - **버그픽스를 각 release 브랜치에 적용:**

        ```bash
        # release-3q에 적용
        git checkout release-3q
        git cherry-pick <hotfix-commit-hash>
        git push origin release-3q
        
        # release-4q에 적용
        git checkout release-4q
        git cherry-pick <hotfix-commit-hash>
        git push origin release-4q
        
        # 충돌 발생 시:
        # 1. 파일 수정으로 충돌 해결
        # 2. git add .
        # 3. git cherry-pick --continue
        ```


### 이미 배포된 release를 8시간후 롤백하고 싶을때 어떻게 해야 할지 논하시오.

- 상황 분석
    - **현재 상태**: 프로덕션에 새 버전이 배포됨 (8시간 경과)
    - **문제**: 심각한 이슈 발견으로 이전 버전으로 롤백 필요
    - **고려사항**: 데이터 무결성, 사용자 영향 최소화, 빠른 복구
- 즉시 롤백하기

    ```bash
    # 이전 커밋으로 되돌리기
    git revert <문제있는-커밋-해시>
    git push origin main
    
    # 또는 강제로 이전 상태로 되돌리기 (주의 필요)
    git reset --hard <이전-안정-버전-해시>
    git push --force-with-lease origin main
    ```


### gitflow 전략에서 cherry-pick이 필요한 경우와 cherry-pick 시 히스토리 관리를 어떻게 해야 할지 논하시오.

- GitFlow에서 Cherry-pick이 필요한 경우들
1. **Hotfix를 여러 Release 브랜치에 적용**
    - 상황 : main에서 생성된 hotfix를 여러 release 브랜치에 적용

    ```bash
    # hotfix 생성 및 완료
    git flow hotfix start 2.1.3
    # 수정 작업...
    git flow hotfix finish 2.1.3
    
    # 여러 release 브랜치에 적용
    for release in release-2.2 release-3.0; do
        git checkout $release
        git cherry-pick <hotfix-commit-hash>
        git push origin $release
    done
    ```

2. **Feature에서 특정 커밋만 Release에 포함**
    - 상황 : feature 브랜치의 일부 커밋만 release에 포함하고 싶은 경우

    ```bash
    # feature 브랜치의 커밋 확인
    git checkout feature/user-profile
    git log --oneline
    # abc1234 사용자 프로필 UI 개선        ← 이것만 release에 포함
    # def5678 프로필 편집 기능 (미완성)     ← 제외
    # ghi9012 프로필 이미지 업로드 (완성)   ← 이것만 release에 포함
    
    # release 브랜치에 선택적 적용
    git checkout release-2.1
    git cherry-pick abc1234  # UI 개선만
    git cherry-pick ghi9012  # 이미지 업로드만
    # def5678는 제외 (미완성이므로)
    ```

3. **긴급 수정을 develop보다 release에 먼저 적용**
    - 상황: release 브랜치에서 발견된 버그를 즉시 수정하고 나중에 develop에 반영

    ```bash
    # release 브랜치에서 직접 수정
    git checkout release-2.1
    git commit -m "fix: 결제 모듈 긴급 수정"
    
    # 나중에 develop에 반영
    git checkout develop
    git cherry-pick <fix-commit-hash>
    ```

- Cherry-pick 시 히스토리 관리 전략
    - 추적 가능한 커밋 메시지 작성

    ```bash
    # Cherry-pick 시 원본 정보 포함
    git cherry-pick <commit-hash> -x
    
    # 결과: 커밋 메시지에 원본 정보 자동 추가
    # fix: 사용자 인증 버그 수정
    #
    # (cherry picked from commit abc1234567890abcdef1234567890abcdef12)
    
    # 수동으로 더 상세한 정보 추가
    git cherry-pick <commit-hash> --no-commit
    git commit -m "fix: 사용자 인증 버그 수정
    
    Original commit: abc1234 from feature/auth-fix
    Applied to: release-2.1 for emergency deployment
    Related: #JIRA-123
    
    (cherry picked from commit abc1234567890abcdef1234567890abcdef12)"
    
    ```

    - slack 체리밭 채널 운영