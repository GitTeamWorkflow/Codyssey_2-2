# Branch Protection Rule 항목 정리

## 1. Branch Protection Rule이란?

Branch Protection Rule은 GitHub 저장소의 특정 브랜치를 보호하기 위한 설정이다.

예를 들어 `main` 브랜치에 Branch Protection Rule을 적용하면 다음과 같은 규칙을 설정할 수 있다.

* `main` 브랜치에 직접 push 금지
* Pull Request(PR)를 통한 병합만 허용
* PR 병합 전 최소 N명 이상의 승인 필요
* 강제 push 방지
* 브랜치 삭제 방지

팀 프로젝트에서는 `main` 브랜치를 안정적으로 유지하고, 모든 변경사항을 리뷰 후 병합하기 위해 사용한다.

---

## 2. Branch name pattern

### 설명

Branch Protection Rule을 적용할 브랜치 이름을 지정하는 항목이다.

과제에서는 `main` 브랜치에 보호 규칙을 적용해야 하므로 다음과 같이 입력한다.

```text
main
```

### 설정 예시

```text
Branch name pattern: main
```

### 의미

`main` 브랜치에만 이 보호 규칙이 적용된다.

---

## 3. Require a pull request before merging

### 설명

해당 브랜치에 직접 push하지 않고, 반드시 Pull Request(PR)를 통해서만 병합하도록 강제하는 옵션이다.

이 옵션을 켜면 `main` 브랜치에 직접 commit을 push할 수 없고, 다른 브랜치에서 작업한 뒤 PR을 생성해야 한다.

### 설정

```text
ON
```

### 의미

```text
feature/park → Pull Request → Review → main merge
```

위와 같은 흐름으로만 `main` 브랜치에 변경사항을 반영할 수 있다.

### 과제 요구사항과의 관계

과제의 다음 조건을 만족한다.

```text
main 직접 push 금지
PR을 통한 병합만 허용
```

---

## 4. Require approvals

### 설명

Pull Request를 병합하기 전에 최소 승인 수를 요구하는 옵션이다.

이 옵션을 켜면 PR을 만든 뒤 다른 팀원의 승인이 있어야 `main` 브랜치에 merge할 수 있다.

### 설정

```text
ON
```

### Required number of approvals before merging

```text
1
```

### 의미

PR을 merge하려면 최소 1명의 approve가 필요하다.

### 과제 요구사항과의 관계

과제의 다음 조건을 만족한다.

```text
최소 1명 승인(approve) 필요
```

---

## 5. Dismiss stale pull request approvals when new commits are pushed

### 설명

PR이 승인된 후 새로운 commit이 추가되면 기존 승인을 취소하는 옵션이다.

예를 들어 팀원이 PR을 approve한 뒤 PR 작성자가 새로운 commit을 push하면, 기존 approve가 무효화된다.

### 예시

```text
1. PR 생성
2. 팀원이 approve
3. PR 작성자가 새로운 commit push
4. 기존 approve 취소
5. 다시 approve 필요
```

### 추천 설정

```text
OFF
```

### 이유

실무에서는 안전한 옵션이지만, 과제에서는 절차가 복잡해질 수 있다.

---

## 6. Require review from Code Owners

### 설명

`CODEOWNERS` 파일에 지정된 담당자의 승인을 요구하는 옵션이다.

예를 들어 `.github/CODEOWNERS` 파일에 다음과 같이 작성할 수 있다.

```text
/docs/ @docs-manager
/src/ @backend-developer
```

이 경우 `docs/` 또는 `src/` 경로의 파일이 수정되면 지정된 담당자의 review가 필요하다.

### 추천 설정

```text
OFF
```

### 이유

과제에서 `CODEOWNERS` 설정을 요구하지 않았다면 필요하지 않다.

---

## 7. Restrict who can dismiss pull request reviews

### 설명

PR review를 취소할 수 있는 사람을 제한하는 옵션이다.

예를 들어 특정 관리자나 팀만 review dismiss 권한을 갖도록 설정할 수 있다.

### 추천 설정

```text
OFF
```

### 이유

일반적인 과제 협업에서는 review dismiss 권한을 따로 제한할 필요가 없다.

---

## 8. Allow specified actors to bypass required pull requests

### 설명

특정 사용자, 팀, 앱이 Pull Request 필수 규칙을 우회할 수 있도록 허용하는 옵션이다.

이 옵션을 설정하면 지정된 사용자는 PR 없이도 브랜치에 직접 변경사항을 반영할 수 있다.

### 추천 설정

```text
OFF
```

### 이유

과제 요구사항이 다음과 같기 때문이다.

```text
main 직접 push 금지
PR을 통한 병합만 허용
```

따라서 특정 사용자가 PR 규칙을 우회하도록 허용하지 않는 것이 좋다.

---

## 9. Require approval of the most recent reviewable push

### 설명

가장 최근에 push한 사람이 아닌 다른 사람이 approve해야 merge할 수 있도록 하는 옵션이다.

즉, PR 작성자가 마지막으로 push한 뒤 자기 자신이 approve하는 것을 방지한다.

### 예시

```text
1. 박범규가 feature/park 브랜치에 push
2. PR 생성
3. 박범규가 직접 approve 불가
4. 다른 팀원이 approve 필요
```

### 추천 설정

```text
선택 사항
```

### 과제에서의 의미

팀원 간 리뷰 과정을 명확하게 보여주고 싶다면 켜도 좋다.

다만 팀원이 실제로 approve할 수 있어야 한다.

---

## 10. Require status checks to pass before merging

### 설명

PR을 merge하기 전에 테스트, 빌드, lint 같은 자동 검사가 성공해야 하는 옵션이다.

예를 들어 GitHub Actions를 설정한 경우 다음과 같은 검사를 요구할 수 있다.

* build
* test
* lint

### 추천 설정

```text
OFF
```

### 이유

현재 과제에서 GitHub Actions나 자동 테스트를 따로 설정하지 않았다면 이 옵션을 켜지 않는 것이 좋다.

켜면 status check가 없어서 PR merge가 막힐 수 있다.

---

## 11. Require conversation resolution before merging

### 설명

PR의 리뷰 댓글이나 대화가 모두 해결되어야 merge할 수 있도록 하는 옵션이다.

예를 들어 팀원이 리뷰에서 수정 요청을 남겼다면, 해당 conversation을 resolve해야 merge할 수 있다.

### 추천 설정

```text
선택 사항
```

### 의미

협업 과정을 더 엄격하게 관리할 수 있지만, 과제 필수 조건은 아니다.

---

## 12. Require signed commits

### 설명

서명된 commit만 허용하는 옵션이다.

GPG 또는 SSH 서명이 포함된 commit만 해당 브랜치에 반영할 수 있다.

### 추천 설정

```text
OFF
```

### 이유

보안이 중요한 프로젝트에서는 유용하지만, 과제에서는 팀원들의 Git 설정이 복잡해질 수 있다.

---

## 13. Require linear history

### 설명

브랜치의 commit 기록을 일직선으로 유지하도록 강제하는 옵션이다.

이 옵션을 켜면 merge commit이 제한될 수 있고, 일반적으로 다음 방식의 병합을 사용하게 된다.

* Squash merge
* Rebase merge

### 장점

* commit 기록이 깔끔해진다.
* 변경 이력을 따라가기 쉽다.

### 단점

* Git 사용이 익숙하지 않으면 conflict 해결이 어려울 수 있다.

### 추천 설정

```text
OFF
```

---

## 14. Require deployments to succeed before merging

### 설명

특정 배포 환경에 성공적으로 배포되어야 PR을 merge할 수 있도록 하는 옵션이다.

실제 서비스 프로젝트에서는 staging 또는 production 배포 성공 여부를 기준으로 사용할 수 있다.

### 추천 설정

```text
OFF
```

### 이유

과제 저장소에서는 보통 배포 환경을 사용하지 않으므로 필요하지 않다.

---

## 15. Lock branch

### 설명

브랜치를 읽기 전용 상태로 만드는 옵션이다.

이 옵션을 켜면 사용자가 해당 브랜치에 push할 수 없다.

### 추천 설정

```text
OFF
```

### 이유

`main` 브랜치에 직접 push를 막는 것은 필요하지만, PR merge까지 막히면 협업이 어려워질 수 있다.

과제에서는 `Require a pull request before merging` 옵션으로 직접 push를 막는 것이 적절하다.

---

## 16. Do not allow bypassing the above settings

### 설명

관리자나 특별 권한을 가진 사용자도 위에서 설정한 보호 규칙을 우회하지 못하게 하는 옵션이다.

### 설정

```text
ON
```

### 의미

Admin 권한이 있는 사용자도 `main` 브랜치에 직접 push하거나 PR 승인 규칙을 건너뛸 수 없다.

### 과제에서의 의미

과제 요구사항인 `main 직접 push 금지`를 더 확실하게 지킬 수 있다.

---

## 17. Restrict who can push to matching branches

### 설명

해당 브랜치에 push할 수 있는 사람, 팀, 앱을 제한하는 옵션이다.

특정 사용자만 `main` 브랜치에 push할 수 있도록 설정할 때 사용한다.

### 추천 설정

```text
OFF
```

### 이유

과제에서는 특정 사람에게 직접 push 권한을 주는 것이 아니라, 모든 변경사항을 PR로 병합하는 것이 목적이다.

따라서 이 옵션을 사용하지 않아도 된다.

---

## 18. Allow force pushes

### 설명

강제 push를 허용하는 옵션이다.

강제 push는 기존 commit 기록을 덮어쓸 수 있다.

예시:

```bash
git push --force
```

### 추천 설정

```text
OFF
```

### 이유

`main` 브랜치에서 force push를 허용하면 commit 기록이 손상될 수 있으므로 꺼두는 것이 좋다.

---

## 19. Allow deletions

### 설명

해당 브랜치를 삭제할 수 있도록 허용하는 옵션이다.

### 추천 설정

```text
OFF
```

### 이유

`main` 브랜치가 삭제되면 저장소의 기준 브랜치가 사라질 수 있다.

따라서 `main` 브랜치에서는 삭제를 허용하지 않는 것이 좋다.

---

## 20. 과제용 최종 설정

과제 요구사항은 다음과 같다.

```text
main 직접 push 금지
PR을 통한 병합만 허용
최소 1명 승인 필요
```

이를 만족하기 위한 설정은 다음과 같다.

| 항목                                          |  설정  |
| ------------------------------------------- | ------ |
| Branch name pattern                         | `main` |
| Require a pull request before merging       | ON     |
| Require approvals                           | ON     |
| Required number of approvals before merging | `1`    |

이 설정을 적용하면 `main` 브랜치에 직접 push할 수 없고, 반드시 Pull Request를 생성한 뒤 최소 1명의 승인을 받아야 `main`에 병합할 수 있다.
