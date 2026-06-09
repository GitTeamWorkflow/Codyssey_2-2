# Git `revert` 정리

## 1. `git revert`란?

`git revert`는 특정 commit의 변경사항을 취소하는 **새로운 commit을 생성하는 명령어**이다.

즉, 기존 commit을 삭제하거나 기록을 되돌리는 것이 아니라, 이전 commit의 변경 내용이 미적용된 commit을 새로 만든다.

예를 들어 다음과 같은 commit 이력이 있다고 가정한다.

```text
A --- B --- C
```

여기서 `C` commit을 취소하고 싶을 때 `revert`를 사용하면 `C`가 사라지는 것이 아니라, `C`의 변경사항을 취소하는 새로운 commit `D`가 생성된다.

```text
A --- B --- C --- D
```

여기서 `D`는 `C`의 변경사항을 되돌리는 commit이다.

---

## 2. `revert`의 핵심 특징

`git revert`의 핵심은 다음과 같다.

- 기존 commit 이력을 삭제하지 않는다.
- 취소 작업도 새로운 commit으로 기록한다.
- 이미 원격 저장소에 push한 commit을 되돌릴 때 안전하다.
- 팀 협업 중 공유된 commit을 되돌릴 때 적합하다.

즉, `revert`는 이력을 보존하면서 안전하게 변경사항을 취소하는 방법이다.

---

## 3. 기본 사용법

특정 commit을 되돌릴 때는 다음 명령어를 사용한다.

```bash
git revert <commit-id>
```

예시:

```bash
git revert a1b2c3d
```

이 명령어를 실행하면 `a1b2c3d` commit에서 변경한 내용을 취소하는 새로운 commit이 생성된다.

---

## 4. commit ID 확인 방법

되돌릴 commit을 찾기 위해 commit log를 확인한다.

```bash
git log --oneline
```

예시 출력:

```text
c9f1a23 [Fix] 로그인 오류 수정
a1b2c3d [Feat] 소셜 로그인 기능 추가
e7d8f91 [Docs] README 수정
```

여기서 `[Feat] 소셜 로그인 기능 추가` commit을 취소하고 싶다면 다음과 같이 실행한다.

```bash
git revert a1b2c3d
```

---

## 5. `revert` 실행 후 동작

`git revert`를 실행하면 Git은 기본적으로 commit 메시지 편집기를 연다.

기본 메시지는 보통 다음과 같이 생성된다.

```text
Revert "[Feat] 소셜 로그인 기능 추가"

This reverts commit a1b2c3d...
```

메시지를 그대로 저장하고 종료하면 revert commit이 생성된다.

Vim이 열렸다면 다음 순서로 저장하고 종료한다.

```text
Esc 입력
:wq 입력
Enter
```

---

## 6. `revert`를 사용하는 상황

### 6.1 이미 push한 commit을 취소해야 할 때

이미 GitHub에 push된 commit은 다른 팀원이 pull했을 가능성이 있다.

이때 `reset`으로 이력을 바꾸면 팀원들의 commit 이력과 충돌할 수 있다.

따라서 공유된 commit은 `reset`보다 `revert`로 되돌리는 것이 안전하다.

```bash
git revert <commit-id>
git push origin <branch-name>
```

---

### 6.2 main 브랜치에 잘못된 commit이 merge되었을 때

PR이 merge된 후 문제가 발견될 수 있다.

예를 들어 `main`에 잘못된 기능이 들어갔다면 해당 commit 또는 merge commit을 revert하여 문제를 해결할 수 있다.

과제에서 `main` 직접 push가 금지되어 있다면, 직접 `main`에 push하지 않고 별도 브랜치에서 revert 작업 후 PR을 생성하는 것이 좋다.

```bash
git switch main
git pull origin main
git switch -c fix/revert-wrong-commit

git revert <commit-id>

git push -u origin fix/revert-wrong-commit
```

그다음 GitHub에서 Pull Request를 생성한다.

---

### 6.3 특정 기능만 제거하고 싶을 때

여러 commit 중 특정 기능을 추가한 commit만 취소하고 싶을 때 `revert`를 사용할 수 있다.

예시:

```text
A --- B --- C --- D
```

여기서 `B` commit만 취소하고 싶다면 다음과 같이 한다.

```bash
git revert B의_commit_id
```

결과:

```text
A --- B --- C --- D --- E
```

`E` commit은 `B`의 변경사항만 취소하는 commit이다.

---

## 7. `reset`과 `revert`의 차이

`reset`과 `revert`는 모두 변경사항을 되돌릴 때 사용할 수 있지만, 방식이 다르다.

| 구분 | reset | revert |
|---|---|---|
| 동작 방식 | HEAD 위치를 이전 commit으로 이동 | 특정 commit을 취소하는 새 commit 생성 |
| 기존 이력 | 삭제되거나 변경될 수 있음 | 그대로 유지됨 |
| 원격 push 후 사용 | 위험할 수 있음 | 안전함 |
| 협업 브랜치에서 사용 | 주의 필요 | 권장 |
| 사용 목적 | 로컬 commit 정리 | 공유된 commit 취소 |

---

## 8. `reset`을 쓰는 경우

`reset`은 보통 아직 push하지 않은 로컬 commit을 수정할 때 사용한다.

예시:

```bash
git reset --soft HEAD~1
```

이 명령어는 마지막 commit을 취소하되 변경사항은 staged 상태로 유지한다.

사용 상황:

- 방금 만든 commit 메시지를 고치고 싶을 때
- 아직 push하지 않은 commit을 다시 정리하고 싶을 때
- 로컬에서 commit 단위를 다시 나누고 싶을 때

---

## 9. `revert`를 쓰는 경우

`revert`는 이미 push했거나 팀원과 공유된 commit을 취소할 때 사용한다.

예시:

```bash
git revert <commit-id>
```

사용 상황:

- 이미 GitHub에 push한 commit을 취소하고 싶을 때
- `main`에 merge된 변경사항을 되돌리고 싶을 때
- 팀원이 이미 pull한 commit을 안전하게 취소하고 싶을 때
- commit 이력을 남기면서 문제를 해결하고 싶을 때

---

## 10. `revert`와 PR 작업 흐름

과제에서 `main` 직접 push가 금지되어 있다면, revert도 PR을 통해 처리하는 것이 좋다.

```bash
git switch main
git pull origin main

git switch -c fix/revert-readme-change

git revert <commit-id>

git push -u origin fix/revert-readme-change
```

이후 GitHub에서 Pull Request를 생성한다.

```text
base: main
compare: fix/revert-readme-change
```

PR 제목 예시:

```text
[Fix] 잘못된 README 변경사항 revert
```

PR 본문 예시:

```md
## 작업 내용

- 잘못 반영된 README 변경사항을 revert했다.
- 기존 commit 이력을 유지하면서 문제 commit의 변경사항만 취소했다.

## 관련 Issue

Fixes #이슈번호
```

---

## 11. 여러 commit을 revert하는 방법

여러 commit을 되돌리고 싶다면 하나씩 revert할 수 있다.

```bash
git revert <commit-id-1>
git revert <commit-id-2>
```

여러 commit을 하나의 revert commit으로 만들고 싶다면 `--no-commit` 옵션을 사용할 수 있다.

```bash
git revert --no-commit <commit-id-1>
git revert --no-commit <commit-id-2>
git commit -m "[Fix] Revert wrong changes"
```

`--no-commit`은 변경사항만 working directory와 staging area에 적용하고, commit은 바로 만들지 않는 옵션이다.

---

## 12. merge commit을 revert하는 경우

일반 commit은 다음처럼 revert할 수 있다.

```bash
git revert <commit-id>
```

하지만 merge commit은 부모 commit이 2개 이상이므로 어느 부모를 기준으로 되돌릴지 지정해야 한다.

이때 `-m` 옵션을 사용한다.

```bash
git revert -m 1 <merge-commit-id>
```

여기서 `-m 1`은 첫 번째 부모를 기준으로 merge commit을 되돌리겠다는 의미이다.

보통 `main` 브랜치에 merge된 PR을 되돌릴 때는 다음과 같은 형태가 된다.

```bash
git revert -m 1 <merge-commit-id>
```

주의할 점은 merge commit revert는 일반 commit revert보다 복잡할 수 있으므로, 가능하면 GitHub PR 화면의 `Revert` 버튼을 사용하는 것이 더 안전하다.

---

## 13. GitHub에서 PR을 Revert하는 방법

GitHub에서는 merge된 Pull Request에 대해 `Revert` 버튼을 제공하는 경우가 있다.

사용 흐름:

```text
GitHub 저장소
→ Pull requests
→ Closed
→ 되돌릴 merged PR 선택
→ Revert 버튼 클릭
→ 새 Pull Request 생성
```

이 방식은 GitHub가 revert용 브랜치와 PR을 만들어주기 때문에 실수 가능성이 줄어든다.

과제에서 PR 기반 협업을 보여줘야 한다면 GitHub의 `Revert` 버튼을 사용한 뒤 새 PR을 생성하는 방식도 좋다.

---

## 14. revert 중 conflict가 발생하는 경우

`revert`도 conflict가 발생할 수 있다.

예를 들어 되돌리려는 commit 이후에 같은 파일의 같은 부분이 다시 수정되었다면 Git이 자동으로 취소 작업을 적용하지 못할 수 있다.

해결 순서:

```bash
git status
```

충돌 파일을 직접 수정한다.

```bash
git add <파일명>
git revert --continue
```

만약 revert를 취소하고 싶다면 다음 명령어를 사용한다.

```bash
git revert --abort
```

---

## 15. revert 관련 주요 명령어

| 명령어 | 설명 |
|---|---|
| `git revert <commit-id>` | 특정 commit의 변경사항을 취소하는 새 commit 생성 |
| `git revert --no-commit <commit-id>` | revert 변경사항만 적용하고 commit은 만들지 않음 |
| `git revert --continue` | conflict 해결 후 revert 계속 진행 |
| `git revert --abort` | 진행 중인 revert 취소 |
| `git revert -m 1 <merge-commit-id>` | merge commit을 첫 번째 부모 기준으로 revert |

---

## 16. 실습 예시

### 상황

`README.md`에 잘못된 내용이 들어간 commit을 이미 push했다.

commit log:

```text
c9f1a23 [Docs] README 잘못된 설명 추가
a1b2c3d [Docs] README 기본 구조 작성
e7d8f91 Initial commit
```

잘못된 commit은 다음이다.

```text
c9f1a23
```

### 해결 과정

새 브랜치를 만든다.

```bash
git switch main
git pull origin main
git switch -c fix/revert-readme
```

잘못된 commit을 revert한다.

```bash
git revert c9f1a23
```

commit이 생성되었는지 확인한다.

```bash
git log --oneline
```

원격 저장소에 push한다.

```bash
git push -u origin fix/revert-readme
```

GitHub에서 PR을 생성한다.

```text
base: main
compare: fix/revert-readme
```

팀원의 approve를 받은 뒤 merge한다.

---

## 17. 최종 정리

`git revert`는 특정 commit을 삭제하는 명령어가 아니라, 특정 commit의 변경사항을 취소하는 새로운 commit을 만드는 명령어이다.

핵심은 다음과 같다.

```text
reset = commit 이력을 되돌림
revert = 되돌리는 commit을 새로 만듦
stash = 작업 중인 변경사항을 임시 저장
```

협업 중 이미 push된 commit을 취소해야 한다면 보통 `reset`보다 `revert`가 안전하다.

특히 `main` 브랜치에 잘못된 변경사항이 들어간 경우에는 직접 수정하거나 이력을 삭제하기보다, revert 브랜치를 만들고 PR을 통해 되돌리는 방식이 좋다.
