# Ruleset 정리

## 1. Ruleset이란?

Ruleset은 GitHub 저장소의 특정 브랜치에 대해 보호 규칙을 설정하는 기능이다.

예를 들어 `main` 브랜치에 ruleset을 설정하면 다음과 같은 규칙을 강제할 수 있다.

* `main` 브랜치에 직접 push 금지
* Pull Request(PR)를 통한 병합만 허용
* PR 병합 전 최소 1명 이상의 승인 필요
* 강제 push 방지
* 브랜치 삭제 방지

즉, `main` 브랜치를 안정적으로 유지하고 팀 협업 과정에서 실수를 줄이기 위한 기능이다.

---

## 2. Ruleset 기본 항목

### 2.1 Ruleset name

Ruleset의 이름을 설정하는 항목이다.

예시:

```text
Protect main branch
```

또는

```text
main branch protection
```

과제에서는 `main` 브랜치를 보호하는 목적이므로 다음과 같이 설정할 수 있다.

```text
Ruleset name: Protect main branch
```

---

### 2.2 Enforcement status

Ruleset을 실제로 적용할지 정하는 항목이다.

| 항목       | 의미                         |
| -------- | -------------------------- |
| Active   | 규칙을 실제로 적용한다               |
| Evaluate | 규칙 위반 여부만 확인하고 실제로 막지는 않는다 |
| Disabled | 규칙을 비활성화한다                 |

과제에서는 실제로 규칙이 적용되어야 하므로 다음과 같이 설정한다.

```text
Enforcement status: Active
```

---

### 2.3 Bypass list

Ruleset을 우회할 수 있는 사용자나 팀을 지정하는 항목이다.

Bypass list에 포함된 사용자는 ruleset을 무시하고 `main` 브랜치에 직접 push할 수 있다.

하지만 과제 요구사항은 다음과 같다.

```text
main 직접 push 금지
PR을 통한 병합만 허용
```

따라서 Bypass list는 비워두는 것이 좋다.

```text
Bypass list: 비워두기
```

---

### 2.4 Target branches

Ruleset을 어떤 브랜치에 적용할지 정하는 항목이다.

과제에서는 `main` 브랜치에 Branch Protection Rule을 설정해야 하므로 다음과 같이 지정한다.

```text
Target branches: main
```

브랜치 패턴 예시는 다음과 같다.

| 패턴          | 의미                        |
| ----------- | ------------------------- |
| `main`      | main 브랜치에만 적용             |
| `feature/*` | feature/로 시작하는 모든 브랜치에 적용 |
| `release/*` | release/로 시작하는 모든 브랜치에 적용 |
| `*`         | 모든 브랜치에 적용                |

과제에서는 `main`만 입력하면 된다.

---

## 3. Rules 항목

### 3.1 Restrict creations

브랜치 생성을 제한하는 옵션이다.

예를 들어 `main` 브랜치를 아무나 새로 만들 수 없도록 제한할 수 있다.

이미 `main` 브랜치가 존재하는 경우 과제에서는 필수 항목이 아니다.

```text
추천 설정: OFF
```

---

### 3.2 Restrict updates

브랜치 업데이트를 제한하는 옵션이다.

브랜치 업데이트란 해당 브랜치에 push하여 내용을 변경하는 것을 의미한다.

다만 과제에서 원하는 것은 모든 변경을 막는 것이 아니라, 직접 push는 막고 PR merge는 허용하는 것이다.

따라서 이 옵션보다는 `Require a pull request before merging` 옵션이 더 중요하다.

```text
추천 설정: 필수 아님
```

---

### 3.3 Restrict deletions

브랜치 삭제를 막는 옵션이다.

`main` 브랜치가 삭제되면 저장소의 기준 브랜치가 사라질 수 있으므로 켜는 것이 좋다.

```text
추천 설정: ON
```

---

### 3.4 Require linear history

커밋 기록을 일직선으로 유지하게 하는 옵션이다.

이 옵션을 켜면 일반적인 merge commit을 제한하고, 보통 다음 방식의 병합을 사용하게 된다.

* Squash merge
* Rebase merge

장점은 커밋 기록이 깔끔해진다는 것이다.

단점은 Git 사용이 익숙하지 않은 경우 충돌 해결이 어려울 수 있다는 것이다.

과제 필수 조건은 아니므로 꺼도 된다.

```text
추천 설정: OFF
```

---

### 3.5 Require deployments to succeed

배포가 성공해야 PR을 merge할 수 있게 하는 옵션이다.

실제 서비스 프로젝트에서는 staging 또는 production 배포 성공 여부를 기준으로 사용할 수 있다.

하지만 일반 과제 저장소에서는 배포 환경이 없기 때문에 필요하지 않다.

```text
추천 설정: OFF
```

---

### 3.6 Require signed commits

서명된 커밋만 허용하는 옵션이다.

GPG 또는 SSH 서명이 포함된 커밋만 merge할 수 있게 한다.

보안이 중요한 프로젝트에서는 유용하지만, 과제에서는 팀원들의 Git 설정이 복잡해질 수 있다.

```text
추천 설정: OFF
```

---

### 3.7 Require a pull request before merging

`main` 브랜치에 직접 push하지 않고 반드시 Pull Request를 통해서만 병합하도록 강제하는 옵션이다.

과제 요구사항 중 다음 항목을 만족하기 위해 반드시 필요하다.

```text
main 직접 push 금지
PR을 통한 병합만 허용
```

```text
추천 설정: ON
```

---

### 3.8 Required approvals

PR을 merge하기 전에 필요한 승인 수를 설정하는 항목이다.

과제 요구사항은 다음과 같다.

```text
최소 1명 승인 필요
```

따라서 승인 수를 1로 설정한다.

```text
Required approvals: 1
```

```text
추천 설정: ON
```

---

### 3.9 Dismiss stale pull request approvals when new commits are pushed

PR이 승인된 후 새로운 commit이 추가되면 기존 승인을 무효화하는 옵션이다.

예를 들어 다음과 같은 상황에서 동작한다.

```text
1. 팀원이 PR을 approve 한다.
2. PR 작성자가 새로운 commit을 push한다.
3. 기존 approve가 취소된다.
4. 다시 approve를 받아야 한다.
```

실무에서는 안전한 옵션이지만, 과제에서는 절차가 번거로워질 수 있다.

```text
추천 설정: 선택 사항
```

---

### 3.10 Require review from Code Owners

`CODEOWNERS` 파일에 지정된 담당자의 승인을 요구하는 옵션이다.

예를 들어 `.github/CODEOWNERS` 파일에 다음과 같이 작성할 수 있다.

```text
/docs/ @docs-manager
/src/ @backend-developer
```

이 경우 `docs/` 또는 `src/` 경로의 파일이 수정되면 지정된 담당자의 승인이 필요하다.

과제에서 `CODEOWNERS` 설정을 요구하지 않았다면 필요하지 않다.

```text
추천 설정: OFF
```

---

### 3.11 Require approval of the most recent reviewable push

마지막으로 push한 사람이 아닌 다른 사람이 approve해야 하는 옵션이다.

즉, PR 작성자가 스스로 approve하여 merge하는 것을 방지한다.

예시:

```text
내가 마지막으로 push함
→ 내가 직접 approve 불가
→ 다른 팀원이 approve해야 함
```

과제에서 팀원 간 승인 과정을 명확히 보여주려면 켜도 좋다.

```text
추천 설정: ON 가능
```

---

### 3.12 Require status checks to pass

테스트나 빌드가 성공해야 PR을 merge할 수 있게 하는 옵션이다.

예를 들어 GitHub Actions를 통해 다음 검사를 수행할 수 있다.

* build
* test
* lint

하지만 GitHub Actions나 테스트 자동화를 설정하지 않았다면 이 옵션을 켜면 merge가 막힐 수 있다.

```text
추천 설정: OFF
```

---

### 3.13 Require branches to be up to date before merging

PR 브랜치가 최신 `main` 내용을 포함해야 merge할 수 있게 하는 옵션이다.

다른 사람이 먼저 `main`에 merge한 내용이 있다면, 내 브랜치에도 최신 `main` 내용을 반영해야 한다.

예시 명령어:

```bash
git switch feature/park
git pull origin main
git push
```

협업에서는 좋은 옵션이지만, 충돌 해결이 익숙하지 않다면 과제에서는 꺼도 된다.

```text
추천 설정: 선택 사항
```

---

### 3.14 Require conversation resolution before merging

PR에 남아 있는 리뷰 댓글이나 대화가 모두 해결되어야 merge할 수 있게 하는 옵션이다.

예를 들어 팀원이 리뷰에서 수정 요청을 남겼다면, 해당 대화를 해결 처리해야 merge할 수 있다.

협업 연습에는 좋은 옵션이지만 과제 필수 조건은 아니다.

```text
추천 설정: 선택 사항
```

---

### 3.15 Require merge queue

PR을 바로 merge하지 않고 queue에 넣어 순서대로 검사 후 merge하는 기능이다.

대규모 프로젝트에서는 유용하지만, 일반 과제 저장소에서는 필요하지 않다.

```text
추천 설정: OFF
```

---

### 3.16 Block force pushes

강제 push를 막는 옵션이다.

강제 push는 기존 commit 기록을 덮어쓸 수 있기 때문에 위험하다.

예시:

```bash
git push --force
```

`main` 브랜치에서는 강제 push를 막는 것이 안전하다.

```text
추천 설정: ON
```

---

### 3.17 Require code scanning results

CodeQL 같은 보안 분석 결과가 통과해야 merge할 수 있게 하는 옵션이다.

보안 스캔 설정을 따로 하지 않았다면 과제에서는 필요하지 않다.

```text
추천 설정: OFF
```

---

## 4. 과제용 설정

과제 요구사항은 다음과 같다.

```text
main 직접 push 금지
PR을 통한 병합만 허용
최소 1명 승인 필요
```

이를 만족하기 위한 추천 설정은 다음과 같다.

| 항목                                    | 설정                  |
| ------------------------------------- | ------------------- |
| Ruleset name                          | Protect main branch |
| Enforcement status                    | Active              |
| Bypass list                           | 비워두기                |
| Target branches                       | main                |
| Require a pull request before merging | ON                  |
| Required approvals                    | 1                   |
| Block force pushes                    | ON                  |
| Restrict deletions                    | ON                  |

---

이 설정을 적용하면 `main` 브랜치에 직접 push할 수 없고, 반드시 Pull Request를 생성한 뒤 최소 1명의 승인을 받아 merge해야 한다.
