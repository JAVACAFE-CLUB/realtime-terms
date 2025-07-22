## GitFlow**란?**

운영 코드 안정성 확보 + 팀 협업 최적화를 목적으로 설계된 
브랜치를 나누는 방법에 대한 분류 중 하나이다.

Git Flow에서는 브랜치를 아래와 같은 5종류로 나눈다.

- **main(master)**
    - 운영 배포된 안정적인 코드 (운영 서버 = main 상태)
- **feature**
    - 각 기능 별 개발 브랜치
- **develop**
    - feature에서 개발된 내용을 가지고 있는 브랜치
    - 다음 release 후보 코드 (개발자들이 feature 합치는 곳)
- **release**
    - 배포를 하기 전 내용을 QA하기 위한 브랜치
    - 운영 배포 준비 브랜치 (develop에서 QA용으로 따옴)
- **hotfix**
    - main 브랜치로 배포를 하고 나서 버그가 생겼을 때 빨리 고치기 위한 브랜치
    - 운영 긴급 수정 브랜치 (main에서 바로 따옴)
    

## **정상적인 GitFlow 프로세스**

### 1.  Feature 개발 단계

- *feature 브랜치는 develop에서 따서 기능 개발 후 develop에 merge*

```bash
# 기능개발용 feature/login 브랜치 생성
git checkout develop
git checkout -b feature/login

# 기능 개발 후 커밋
echo "login feature" >> app.txt
git add app.txt
git commit -m "feat: login 기능 추가"

# 개발한 기능 develop에 merge
git checkout develop
git merge --no-ff feature/login -m "merge: login feature"
git branch -d feature/login
```

```bash
# 1. feature 브랜치 생성 & 작업
git flow feature start login

echo "login feature code" >> app.txt
git add . && git commit -m "feat: login 기능 추가"

git flow feature finish login
# (자동으로 develop에 merge되고 feature 브랜치 삭제)
```

### 2. Release 준비 단계

- 모든 기능 개발이 완료되면 QA를 위해 develop에서 release 브랜치를 만든다.
- QA 중 버그 발생 시 여기서 fix 한다.
- *release 브랜치는 develop을 freeze하고 QA/버그 수정만 허용*

```bash
# release 브랜치 생성
git checkout develop
git checkout -b release/1.0

# QA 중 발견된 버그 fix
echo "QA 수정 사항" >> app.txt
git commit -m "fix: QA 수정"
```

```bash
# 2. release 브랜치 생성 & QA
git flow release start 1.0.0
echo "fix QA 버그" >> app.txt
git add . && git commit -m "fix: QA 수정"
```

### 3.  운영 배포 단계

- *release를 main에 병합 → 운영 배포 후 develop과 동기화*
- 운영에 배포한 버전을 명확히 기록하기 위해 태그 추가

```bash

# *release를 main에 병합*
git checkout main
git merge --no-ff release/1.0 -m "release: v1.0"

# main 브랜치에 v1.0 태그 추가(운영에 배포한 버전을 명확히 기록)
git tag v1.0

# *운영 배포 후 develop과 동기화*
git checkout develop
git merge --no-ff release/1.0 -m "merge release/1.0 into develop"

git branch -d release/1.0
```

```bash
# 운영 배포 준비 완료 후 finish
git flow release finish 1.0.0
# (release/1.0.0 → main merge + 태그 v1.0.0 생성 + develop에도 merge)
```

### 4. 긴급 수정(HOTFIX) 단계

- *hotfix는 main에서 따서 운영에 빠르게 반영 → 이후 develop에 merge로 코드 일관성 유지*
- 운영서버에 올라간 긴급 수정 버전을 식별하기 위해 태그 추가

```bash
# 버그픽스를 위해 main에서 hotfix용 브랜치 생성
git checkout main
git checkout -b hotfix/urgent-fix

# 버그픽스 후 커밋
echo "urgent fix" >> app.txt
git commit -m "fix: urgent production issue"

# main에 반영
git checkout main
git merge --no-ff hotfix/urgent-fix -m "hotfix: urgent fix"

# main 브랜치에 v1.0.1 태그 추가 ( 운영서버에 올라간 긴급 수정 버전을 기록 )
git tag v1.0.1

# *운영 배포 후 develop에 반영*
git checkout develop
git merge --no-ff hotfix/urgent-fix -m "merge hotfix into develop"

git branch -d hotfix/urgent-fix
```

```bash
# 🔥 4. hotfix 브랜치 생성 & 긴급 수정
git flow hotfix start 1.0.1
echo "urgent fix" >> app.txt
git add . && git commit -m "fix: urgent hotfix"

git flow hotfix finish 1.0.1
# (hotfix/1.0.1 → main merge + 태그 v1.0.1 생성 + develop에도 merge)
```

## 언제 GitFlow를 써야할까?

### ❗ [발생가능한 문제] **develop에 다른 feature가 들어와서 release에 섞이는 문제**

📝 상황 요약

1. `develop`에서 `feature/회원가입`을 따서 개발 → QA 후 OK
2. `release/회원가입` 브랜치 만들어서 운영 준비
3. 그런데 그 사이에 누군가 `feature/쿠폰`을 `develop`에 merge
4. 이제 `release/회원가입`에 **아직 검증 안 된 쿠폰 코드가 포함됨**

### **→ 왜 이런 일이 생기나?**

- git-flow는 **모든 feature가 develop을 거쳐야** 해서
    
    👉 develop이 항상 “next release 후보군” 역할을 함
    
- 하지만 여러 feature가 **동시에 올라오면 release 후보군이 섞임**

### **전 회사의 GitHub Flow 변형 전략**

GitHub Flow에 `dev`, `qa`, `stage` 브랜치를 추가해 환경별 배포를 구분한 구조입니다.

- 브랜치 역할
    - main : 운영코드
    - stage: 운영 직전 검증 환경 ( git-flow 전략에서의 release )
    - qa: QA 서버 배포용 브랜치
    - dev: 개발자 통합 브랜치
- 개발 프로세스
    - main에서 feature 생성
    - 개발 완료 후 해당 feature를 dev → qa → stage → main 순서로 각각 반영
- **장점**
    - 각 feature가 **main에서 독립적으로 파생 → QA/Stage에서 개별 테스트 가능**
    - git-flow보다 브랜치 구조가 단순
    - 운영 버전이 하나인 경우 훨씬 효율적
- **단점**
    - 운영 버전이 여러 개인 상황에는 적합하지 않음
    - 긴급 수정 시 dev/qa/stage에 모두 머지해야 함

### **여기서 GitHub Flow 변형 전략이 좋은 이유**

- `main`에서 feature 브랜치를 파니까 **feature가 독립적이다.**
    
    → 그래서 다른 feature가 섞일 일이 없음.
    

### **GitFlow의 대응 방식**

- **release freeze 정책 사용**
    - release 브랜치가 생성되면, develop에 새로운 feature merge 금지, 긴급 수정만 허용

❌ **release freeze 문제점!**

- 다음 release용 feature를 develop에 머지 못함
→ 개발자는 develop에 못 머지하니 QA 서버나 개발 서버 배포가 막힘
(release-a QA 중인데 release-b 대상 feature는 dev에 못 올리는 문제)

- 해결방법
    - **Feature Toggle**
        - release-b feature를 develop에 merge는 함
        - 하지만 기능을 **toggle로 비활성화**
        - release-a 배포 중에도 QA 서버에 올라가긴 하지만 사용자에게는 보이지 않음
            
            ```java
            
            if (FeatureToggle.isEnabled("newCheckoutFlow")) {
                runNewCheckoutFlow();
            } else {
                runOldCheckoutFlow();
            }
            ```
            
        
        ✅ 장점
        
        - QA 서버에서 release-b feature도 테스트 가능
        - develop에 미리 올려도 운영 영향 없음
        
        ⚠️ 단점
        
        - toggle 관리 복잡 (실무에서는 toggle 청소 안 해서 코드 썩음)
        - 초기 설계 때부터 고려해야 함

- **Release Branch Fork**
    - release-a QA 중 develop freeze 상태 → release-b 대상 feature는 별도 브랜치에서 개발
        - ex: `next-develop`, `feature/checkout-v2`
    - release-a 배포 끝나면 next-develop을 develop로 승격
        
        ```bash
        git checkout -b next-develop
        # release-b feature 개발 진행
        ```
        
    
    ✅ 장점
    
    - release-b feature는 독립적으로 QA 가능
    - release freeze 영향 없음
    
    ⚠️ 단점
    
    - 브랜치가 늘어나고 병합 순서 관리가 필요
    - **“next-develop과 develop 충돌지옥”** 발생 가능

- **QA 환경 분리**
    - QA 서버를 각각 운영
        - QA1: release-a 전용
        - QA2: release-b feature용
    - release-a QA 중인 develop에 다른 feature를 머지하지 않음
    - release-b feature는 QA2에서 개별 테스트 후 release 브랜치에만 머지
    
    ✅ 장점
    
    - QA 환경 분리로 충돌 방지
    - release freeze가 개발 속도를 늦추지 않음
    
    ⚠️ 단점
    
    - QA 환경 2개 유지비용
    - 배포 스크립트 관리 복잡도 ↑

→ 엄청 복잡하네…

### ✅ GitFlow가 필요한 상황

- 운영 버전이 **여러 개 병행**되는 경우 (ex: 1.0 유지보수 + 2.0 개발)
    
    ```markdown
    < 실제 예시>
    📱 카카오톡
    - v1.0: 현재 앱 스토어에 배포된 버전
    	- 긴급 보안 패치 필요
    
    - v2.0: 프로필 동영상 기능 추가 중 → QA 중
    
    요구사항
    - v1.0에 보안 패치 배포 → 고객들이 계속 사용 가능해야 함
    - 동시에 v2.0 개발도 계속되어야 함
    ```
    
- 긴급 수정이 잦은 서비스 (hotfix 필수) → 사실 이건 GitFlow만의 장점은 아님
- 대규모 조직에서 release QA 파이프라인이 많을 때

### ❌ GitFlow가 과한 경우

- 배포 사이클 짧은 스타트업
- 운영 버전이 단일한 경우

### 💡 정리

GitFlow 는 release 안정성을 최우선으로 설계되었다.
그래서 **“QA 중에 다른 feature 들어오는 거 절대 금지”** 전략을 고수한다. **( 개발 속도 < 배포 안정성)**

하지만 요즘처럼 빠른 배포 사이클(DevOps/CD)에서는 이 구조가 안맞는 경우도 있으니
상황봐서 맞는 전략을 선택하자.

## 케이스 기반 정리

### ❓ release-3q, release-4q 브렌치를 동시에 진행하고 있을때 
다음주 긴급 버그픽스가 필요하다면 어떻게 처리할지 논하시오.

- 상황
    - 운영 버그 발생 (main 기준으로 핫픽스 필요)
    - `release/3q`, `release/4q` QA 진행 중
- 해결방법
    - main 기준으로 hotfix 브랜치를 파고 버그를 수정한 후 각각의 브랜치에 반영
        
        ```bash
        # 운영 기준(main)에서 핫픽스 브랜치 생성
        git checkout main
        git flow hotfix start login-bugfix
        
        # 버그 수정
        echo "fix login issue" >> app.txt
        git add app.txt
        git commit -m "fix: urgent login bug"
        
        # 핫픽스 완료 → 운영 배포 및 develop 동기화
        git flow hotfix finish login-bugfix
        # (자동으로 main & develop에 merge, main에 v1.0.1 태그 생성)
        
        # release 브랜치에 핫픽스 적용
        # 방법 1: merge
        git checkout release/3q
        git merge --no-ff hotfix/login-bugfix -m "merge: login bugfix into release/3q"
        
        # 방법 2: cherry-pick
        git checkout release/4q
        git cherry-pick <hotfix_commit_hash>
        # (conflict 발생 시 수동 해결)
        ```
        
    - 고민사항 : merge vs cherry-pick
        - 버그 수정 범위가 크다면? → merge
        - 버그 수정 범위가 작다면? →  cherry-pick으로 최소 수정만 반영
    

### ❓ 이미 배포된 release를 8시간후 롤백하고 싶을때 어떻게 해야 할지 논하시오.

- 상황
    - 운영 배포 후 문제가 발생해 **8시간 내 롤백**
- 해결방법
    - 코드 롤백 : 이전 버전을 태그로 복원
        
        ```bash
        git checkout main
        git reset --hard v1.0.0
        ```
        
- 고민사항 : 롤백하려는 코드의 영향도 파악 후 롤백 진행
    - ex) DB 롤백 고려
        - 배포에 DB 마이그레이션이 포함된 경우 코드만 롤백해도 DB 구조는 이전과 맞지 않음 
        → 서비스 장애 가능성

### ❓ gitflow 전략에서 cherry-pick이 필요한 경우와 cherry-pick 시 히스토리 관리를 어떻게 해야 할지 논하시오.

- **답변**
    - *특정 커밋만 선택적으로 다른 브랜치에 적용*할 때 사용
        
        ex)
        
        **(1) 운영 hotfix를 여러 release에 반영하는 경우**
        
        - **상황**
            - `main`에서 긴급 수정(hotfix)을 적용했는데
            - `release/1.0`, `release/2.0`에도 동일 수정 필요
            - release/1.0, release/2.0 코드베이스가 diverge되어 단순 merge 불가
        
        → 필요한 수정 커밋만 선택해 가져와서 적용
        
        **(2) QA 중인 release에만 특정 커밋을 적용하는 경우**
        
        - **상황**
            - develop에 merge된 feature 중 일부만 release에 반영 필요
            - 다른 feature는 아직 QA 안 된 상태라서 제외해야 함
        
        → 필요한 커밋만 골라서 release에 적용, 다른 feature 영향 없이 QA 가능
        
        **(3) 긴급 수정 이후 develop에 반영**
        
        - **상황**
            - 긴급 수정은 main에서 작업했지만
            - develop에는 코드 구조가 많이 달라 직접 merge 시 충돌 위험
        
        → 충돌 범위를 최소화하고 수정사항만 develop에 반영
        
    
    - 히스토리 관리 전략
        - 커밋 메시지 관리 : cherry-pick 시 **원본 커밋 해시 명시**
            
            ```bash
            git cherry-pick abc123
            # 커밋 메시지:
            # fix: 로그인 버그 수정 (cherry-picked from abc123)
            ```
            
        

### ❗feature 브랜치 오래 묵힌 상태에서 머지

- **상황**: 개발자가 feature 브랜치를 몇 주간 오래 끌다 머지하려고 보니 develop에 변화가 너무 많아 충돌 지옥
- **문제**:
    
    ```bash
    # 나쁜 사례 (묵힌 feature 브랜치) - 3주 전 develop에서 따온 feature /login
    develop:     A---B---C---D---E---F---G (3주간 커밋 폭주)
    feature/login:   A---X---Y---Z
    ```
    
    → 나중에 merge하려면 A~G까지 한 번에 충돌 해결해야 함 
    
    - 머지 충돌이 너무 많아 revert와 cherry-pick 난무
    - feature 브랜치를 갈아엎고 다시 파야 하는 상황
- **처리**
    
    ```bash
    # 좋은 사례 (주기적 rebase) - feature/login 개발 중간마다 최신 develop 가져오기
    git checkout feature/login
    git fetch origin
    git rebase origin/develop
    ```
    
    - feature 브랜치도 주기적으로 develop에서 rebase 해주는 습관이 필요
        
        → 최신 develop 기준으로 feature 브랜치가 갱신됨
        
        → 나중에 merge할 때 충돌 최소화
        
    - 큰 기능은 **feature toggle**로 나눠서 배포
