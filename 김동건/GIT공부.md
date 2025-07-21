## Git 실무 시나리오 기반 질문과 답변

### 1. `release-3q`, `release-4q` 브랜치를 동시에 진행하고 있을 때 다음 주 긴급 버그픽스가 필요하다면 어떻게 처리할지 논하시오.

**답변:**

* `hotfix` 브랜치를 `main` 또는 `production`에서 분기하여 처리.
* 수정 완료 후 `main`, `release-3q`, `release-4q` 브랜치에 병합 또는 cherry-pick.

**명령어:**

```bash
git checkout main
git pull origin main
git checkout -b hotfix/urgent-bugfix
# 수정 후
git commit -am "fix: urgent bug fix"

git checkout main
git merge --no-ff hotfix/urgent-bugfix

git checkout release-3q
git cherry-pick <commit_hash>

git checkout release-4q
git cherry-pick <commit_hash>
```

---

### 2. 이미 배포된 release를 8시간 후 롤백하고 싶을 때 어떻게 해야 할지 논하시오.

**답변:**

* 이전 태그로 재배포 또는 revert 커밋 생성.
* DB 롤백 필요 여부 확인.

**명령어:**

```bash
git tag --sort=-creatordate
git checkout tags/release-3q-v1.2.3
# 배포 도구로 재배포
# 또는

# 문제가 된 커밋 되돌리기
git revert <commit_hash>
```

---

### 3. GitFlow 전략에서 cherry-pick이 필요한 경우와 히스토리 관리를 어떻게 해야 할지 논하시오.

**답변:**

* 특정 브랜치에만 커밋 반영 필요 시 사용.
* cherry-pick 시 충돌 및 중복 커밋 주의.

**명령어:**

```bash
git checkout release-3q
git cherry-pick <commit_hash>
# 충돌 해결 후
git add .
git cherry-pick --continue
```

---

### 4. 특정 릴리즈 브랜치에서 테스트가 완료되기 전에 다른 브랜치에서 hotfix가 병합되었습니다. 이때 release 브랜치와 main 브랜치 간 동기화를 어떻게 해야 할지 설명해주세요.

**답변:**

* main에 해당 hotfix를 cherry-pick하거나, 테스트 완료 후 release 브랜치를 merge.

**명령어:**

```bash
git log release-3q
git checkout main
git cherry-pick <hotfix_commit_hash>
```

---

### 5. Git에서 과거 커밋을 수정하거나 제거해야 할 필요가 있을 때 어떤 전략(git rebase, reset 등)을 선택할 것이며, 팀 협업 중일 때 주의해야 할 점은 무엇인가요?

**답변:**

* `rebase -i`: 로컬 커밋 정리용. 협업 중인 브랜치엔 비추천.
* `reset`: 브랜치 초기화. 공유 브랜치엔 위험.
* `revert`: 안전하게 커밋 되돌리기.

**명령어:**

```bash
git rebase -i HEAD~3
git reset --hard <commit_hash>
git revert <commit_hash>
```

---

### 6. 긴급하게 두 기능 브랜치를 병합하려고 할 때 충돌이 발생했습니다. 이 경우 충돌을 어떤 기준으로 해결할지, 그리고 merge 후 히스토리를 어떻게 정리할지 설명해주세요.

**답변:**

* 기능/비즈니스 기준으로 충돌 해결.
* merge: 히스토리 보존, squash: 깔끔한 기록, rebase: 직선형 히스토리.

**명령어:**

```bash
git merge feature-b  # 충돌 발생
# 충돌 해결 후
git add .
git commit

git merge --squash feature-b
git rebase main
```

---

### 7. 협업 중 git rebase를 잘못해서 동료의 커밋이 날아간 적이 있다고 가정해봅시다. 이런 상황에서 어떻게 복구할 수 있을까요?

**답변:**

* `reflog`로 잃어버린 커밋 찾기, `reset` 또는 `cherry-pick`으로 복구.

**명령어:**

```bash
git reflog
git checkout <old_commit_hash>
git reset --hard <old_commit_hash>
```

---

### 8. 팀 전체 코드베이스에 영향을 주는 대규모 리팩토링 작업을 진행해야 할 때, 어떤 Git 브랜치 전략으로 작업하고, 어떻게 리뷰 및 병합을 관리할 것인지 설명해주세요.

**답변:**

* `feature/refactor-*` 브랜치에서 작업.
* 변경 단위로 PR 분리 및 리뷰.
* feature toggle 고려.

**명령어:**

```bash
git checkout -b feature/refactor-auth
git commit -am "refactor: auth 모듈 분리"
# git add -p 로 변경 단위 나눠 커밋
```

---

### 9. Git에서 force push가 필요한 상황은 어떤 경우일 수 있으며, 이를 실무에서 사용할 때 어떤 위험이 있고 어떻게 안전하게 사용할 수 있을까요?

**답변:**

* rebase 후 강제 푸시 필요.
* 위험: 히스토리 유실, 협업 브랜치 충돌.
* 안전: `--force-with-lease`, 보호 브랜치 설정.

**명령어:**

```bash
git push --force-with-lease
```

---

### 10. 여러 서비스나 공통 모듈을 Git에서 관리할 때, submodule, subtree, monorepo 중 어떤 전략을 선택할지 기준과 이유를 설명해주세요.

**답변:**

| 전략        | 장점     | 단점      | 적용 예시    |
| --------- | ------ | ------- | -------- |
| Submodule | 독립성 유지 | 동기화 어려움 | 외부 라이브러리 |
| Subtree   | 관리 쉬움  | 히스토리 증가 | 공통 모듈 공유 |
| Monorepo  | 통합 관리  | 빌드 복잡   | 대규모 조직   |

**명령어 예시:**

```bash
git submodule add https://github.com/example/lib.git libs/lib
git subtree add --prefix=libs/lib https://github.com/example/lib.git main --squash
```
