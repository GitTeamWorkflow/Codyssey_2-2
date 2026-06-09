# Git `stash` 정리

## 1. `git stash`란?

`git stash`는 아직 commit하지 않은 변경사항을 임시로 저장하는 명령어이다.

작업 중인 파일을 commit하지 않고 잠시 보관해두고, working directory를 깨끗한 상태로 되돌릴 때 사용한다.

예를 들어 어떤 브랜치에서 작업 중인데 갑자기 다른 브랜치로 이동해야 할 때, 현재 수정 중인 내용을 commit하기 애매하다면 `stash`를 사용할 수 있다.

```bash
git stash
```

이 명령어를 실행하면 현재 수정 중이던 변경사항이 임시 저장되고, 작업 디렉토리는 마지막 commit 상태로 돌아간다.

---

## 2. `stash`가 필요한 이유

Git은 수정 중인 파일이 있을 때 브랜치 이동이나 merge를 막는 경우가 있다.

예를 들어 다음과 같은 상황이 있을 수 있다.

```text
현재 feature/park 브랜치에서 README.md 수정 중
아직 commit하기에는 작업이 덜 끝남
그런데 main 브랜치로 이동해서 급한 작업을 해야 함
```

이때 바로 브랜치를 이동하려고 하면 Git이 다음과 비슷한 메시지를 출력할 수 있다.

```text
Your local changes to the following files would be overwritten by checkout
```

이런 경우 `stash`를 사용하면 작업 중인 내용을 임시 저장하고 안전하게 다른 브랜치로 이동할 수 있다.

---

## 3. 기본 사용법

## 3.1 변경사항 임시 저장

```bash
git stash
```

또는 메시지를 붙여서 저장할 수도 있다.

```bash
git stash push -m "README 수정 작업 임시 저장"
```

메시지를 붙이면 나중에 stash 목록을 확인할 때 어떤 작업이었는지 알아보기 쉽다.

---

## 3.2 stash 목록 확인

```bash
git stash list
```

예시 출력:

```text
stash@{0}: On feature/park: README 수정 작업 임시 저장
stash@{1}: On docs/update-readme: branch strategy 작성 중
```

가장 최근 stash는 `stash@{0}`이다.

---

## 3.3 stash 적용 후 삭제

```bash
git stash pop
```

`pop`은 가장 최근 stash를 다시 적용하고, stash 목록에서 삭제한다.

즉, 임시 저장한 내용을 다시 꺼내면서 보관 목록에서는 제거한다.

---

## 3.4 stash 적용만 하고 삭제하지 않기

```bash
git stash apply
```

`apply`는 stash 내용을 다시 적용하지만, stash 목록에서는 삭제하지 않는다.

같은 stash를 여러 번 적용해야 하거나, 안전하게 남겨두고 싶을 때 사용한다.

---

## 3.5 특정 stash 적용

stash가 여러 개 있을 때는 번호를 지정할 수 있다.

```bash
git stash apply stash@{1}
```

또는 적용 후 삭제하려면 다음과 같이 한다.

```bash
git stash pop stash@{1}
```

---

## 4. `pop`과 `apply`의 차이

| 명령어 | 동작 | stash 목록에서 삭제 여부 |
|---|---|---|
| `git stash pop` | stash를 적용한다 | 삭제됨 |
| `git stash apply` | stash를 적용한다 | 유지됨 |

정리하면 다음과 같다.

```text
pop = 꺼내고 목록에서 제거
apply = 적용만 하고 목록에 유지
```

처음에는 실수 방지를 위해 `apply`를 사용하는 것도 좋다.

---

## 5. stash 삭제

## 5.1 특정 stash 삭제

```bash
git stash drop stash@{0}
```

## 5.2 모든 stash 삭제

```bash
git stash clear
```

`clear`는 모든 stash를 삭제하므로 주의해야 한다.

---

## 6. stash에 저장되는 것

기본적으로 `git stash`는 다음 변경사항을 저장한다.

- tracked file의 수정사항
- staging area에 올라간 변경사항

하지만 기본 `git stash`는 untracked file을 저장하지 않는다.

untracked file이란 Git이 아직 추적하지 않는 새 파일이다.

예시:

```text
새로 만든 파일: memo.md
아직 git add 하지 않음
```

이런 파일까지 stash하려면 `-u` 옵션을 사용한다.

```bash
git stash -u
```

또는:

```bash
git stash push -u -m "새 파일 포함 임시 저장"
```

---

## 7. tracked file과 untracked file

| 구분 | 의미 |
|---|---|
| tracked file | Git이 이미 추적 중인 파일 |
| untracked file | 새로 만들었지만 아직 Git이 추적하지 않는 파일 |

예시:

```bash
git status
```

출력 예시:

```text
Changes not staged for commit:
  modified: README.md

Untracked files:
  memo.md
```

여기서 `README.md`는 tracked file이고, `memo.md`는 untracked file이다.

기본 `git stash`는 `README.md` 수정사항만 저장하고, `memo.md`는 저장하지 않을 수 있다.

`memo.md`까지 함께 저장하려면 다음을 사용한다.

```bash
git stash -u
```

---

## 8. stash 사용 상황

`stash`는 다음 상황에서 유용하다.

| 상황 | 설명 |
|---|---|
| 브랜치를 급하게 이동해야 할 때 | commit하지 않은 변경사항 때문에 이동이 안 될 때 사용 |
| 작업이 덜 끝났을 때 | 아직 commit하기 애매한 작업을 잠시 보관 |
| pull 또는 merge 전에 작업물을 치워야 할 때 | 현재 변경사항과 충돌을 피하기 위해 사용 |
| 다른 작업을 먼저 해야 할 때 | 하던 일을 보관하고 다른 브랜치에서 작업 가능 |
| 실험 코드를 잠시 숨길 때 | 나중에 다시 꺼내서 확인 가능 |

---

## 9. 브랜치 이동 전 stash 예시

### 상황

`feature/park` 브랜치에서 작업 중인데, 급하게 `main` 브랜치로 이동해야 한다.

현재 상태 확인:

```bash
git status
```

출력 예시:

```text
modified: README.md
modified: docs/branch-strategy.md
```

아직 commit하기에는 작업이 덜 끝난 상태이다.

### 해결

작업 내용을 stash에 저장한다.

```bash
git stash push -m "브랜치 전략 문서 작성 중"
```

이제 working directory가 깨끗해졌는지 확인한다.

```bash
git status
```

브랜치를 이동한다.

```bash
git switch main
```

필요한 작업을 한 뒤 다시 원래 브랜치로 돌아온다.

```bash
git switch feature/park
```

stash를 다시 적용한다.

```bash
git stash pop
```

---

## 10. pull 전 stash 예시

### 상황

현재 브랜치에서 수정 중인 파일이 있는데, 원격 저장소의 최신 내용을 가져오고 싶다.

```bash
git pull origin main
```

그런데 현재 수정사항과 원격 변경사항이 충돌할 수 있다.

### 해결

현재 작업을 임시 저장한다.

```bash
git stash push -m "pull 전 작업 임시 저장"
```

최신 내용을 가져온다.

```bash
git pull origin main
```

stash를 다시 적용한다.

```bash
git stash pop
```

만약 충돌이 발생하면 충돌 파일을 직접 수정하고 다음 명령어를 실행한다.

```bash
git add .
git commit
```

---

## 11. stash와 conflict

`git stash pop` 또는 `git stash apply`를 했을 때 conflict가 발생할 수 있다.

이유는 stash에 저장된 변경사항과 현재 브랜치의 변경사항이 같은 파일의 같은 부분을 수정했기 때문이다.

이 경우 Git은 충돌 마커를 파일에 표시한다.

```text
<<<<<<< Updated upstream
현재 브랜치의 내용
=======
stash에서 가져온 내용
>>>>>>> Stashed changes
```

해결 순서는 다음과 같다.

```bash
git status
```

충돌 파일을 직접 수정한다.

```bash
git add <파일명>
```

필요하면 commit한다.

```bash
git commit -m "[Fix] Resolve stash conflict"
```

주의할 점은 `git stash pop` 중 conflict가 발생하면 stash가 자동으로 삭제되지 않을 수 있다는 것이다.

stash 목록을 확인한다.

```bash
git stash list
```

필요 없으면 직접 삭제한다.

```bash
git stash drop stash@{0}
```

---

## 12. stash 내용을 미리 확인하는 방법

stash에 저장된 변경사항을 확인할 수 있다.

```bash
git stash show
```

더 자세히 보고 싶으면 `-p` 옵션을 사용한다.

```bash
git stash show -p stash@{0}
```

이 명령어는 stash에 저장된 실제 diff를 보여준다.

---

## 13. stash에서 새 브랜치 만들기

stash 내용을 기반으로 새 브랜치를 만들 수도 있다.

```bash
git stash branch <새브랜치이름>
```

예시:

```bash
git stash branch feature/recover-work
```

이 명령어는 stash가 만들어졌던 시점의 commit을 기준으로 새 브랜치를 만들고, stash 내용을 적용한다.

작업 내용을 복구하기 좋다.

---

## 14. stash와 reset, revert 비교

| 명령어 | 목적 | 이력 변경 여부 | 주 사용 상황 |
|---|---|---|---|
| `stash` | 작업 중인 변경사항 임시 저장 | commit 이력 변경 없음 | commit 전 작업 보관 |
| `reset` | commit 이력을 이전 상태로 이동 | 이력 변경 가능 | push 전 commit 정리 |
| `revert` | 특정 commit을 취소하는 새 commit 생성 | 이력 유지 | push 후 안전하게 되돌리기 |

정리하면 다음과 같다.

```text
stash = 아직 commit하지 않은 작업을 잠시 숨김
reset = commit 자체를 되돌림
revert = 되돌리는 commit을 새로 만듦
```

---

## 15. stash 사용 시 주의할 점

`stash`는 편리하지만 무조건 안전한 보관함처럼 생각하면 안 된다.

주의할 점은 다음과 같다.

- stash를 너무 많이 쌓아두면 어떤 작업인지 헷갈린다.
- 메시지 없이 stash하면 나중에 구분하기 어렵다.
- `git stash clear`는 모든 stash를 삭제하므로 조심해야 한다.
- stash는 commit이 아니므로 장기 보관용으로 적합하지 않다.
- 중요한 작업은 stash보다 브랜치와 commit으로 관리하는 것이 좋다.

---

## 16. 권장 사용 방식

stash를 사용할 때는 메시지를 붙이는 것을 권장한다.

```bash
git stash push -m "README 문서 작성 중"
```

untracked file까지 포함해야 하면 `-u`를 붙인다.

```bash
git stash push -u -m "새 문서 파일 포함 임시 저장"
```

stash 목록을 자주 확인한다.

```bash
git stash list
```

필요 없는 stash는 삭제한다.

```bash
git stash drop stash@{0}
```

---

## 17. 실습 예시

### 상황

`docs/update-readme` 브랜치에서 README를 수정하고 있다.

아직 작업이 끝나지 않았지만, 급하게 `main` 브랜치의 최신 상태를 확인해야 한다.

### 해결 과정

현재 상태 확인:

```bash
git status
```

작업 내용 임시 저장:

```bash
git stash push -m "README 수정 작업 임시 저장"
```

`main` 브랜치로 이동:

```bash
git switch main
git pull origin main
```

다시 작업 브랜치로 복귀:

```bash
git switch docs/update-readme
```

stash 적용:

```bash
git stash pop
```

작업 계속 진행 후 commit:

```bash
git add README.md
git commit -m "[Docs] README 수정"
```

원격 브랜치 push:

```bash
git push -u origin docs/update-readme
```

---

## 18. 자주 사용하는 stash 명령어 요약

| 명령어 | 설명 |
|---|---|
| `git stash` | 현재 변경사항 임시 저장 |
| `git stash push -m "메시지"` | 메시지를 붙여 stash 저장 |
| `git stash -u` | untracked file까지 포함하여 stash 저장 |
| `git stash list` | stash 목록 확인 |
| `git stash pop` | 가장 최근 stash 적용 후 목록에서 삭제 |
| `git stash apply` | 가장 최근 stash 적용, 목록에는 유지 |
| `git stash apply stash@{1}` | 특정 stash 적용 |
| `git stash show` | stash 변경 요약 확인 |
| `git stash show -p` | stash diff 상세 확인 |
| `git stash drop stash@{0}` | 특정 stash 삭제 |
| `git stash clear` | 모든 stash 삭제 |
| `git stash branch <브랜치명>` | stash를 기반으로 새 브랜치 생성 |

---

## 19. 최종 정리

`git stash`는 commit하지 않은 변경사항을 임시 저장하는 기능이다.

핵심은 다음과 같다.

```text
작업 중인 변경사항을 잠시 보관한다.
working directory를 깨끗하게 만든다.
나중에 다시 적용할 수 있다.
```

특히 다음 상황에서 유용하다.

```text
브랜치를 이동해야 하는데 수정 중인 파일이 있을 때
pull 또는 merge 전에 현재 작업을 잠시 치워야 할 때
아직 commit하기 애매한 작업을 임시 보관할 때
```

다만 stash는 장기 보관용이 아니므로 중요한 작업은 브랜치와 commit으로 관리하는 것이 좋다.
