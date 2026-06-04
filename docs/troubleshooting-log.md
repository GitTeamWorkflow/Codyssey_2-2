# Troubleshooting Log

이 문서는 Git 협업 중 발생한 실수 복구와 명령어 실습 기록을 남기는 공간이다. 모든 기록은 다른 팀원이 같은 상황을 재현하고 이해할 수 있도록 상황, 명령어, 결과, 주의점을 포함한다.

## 기록 요약

| 번호 | 시나리오 | 참여자 | 상태 |
| --- | --- | --- | --- |
| 1 | `git commit --amend`로 최근 커밋 메시지 수정 | Park | 완료 |
| 2 | `git reset --soft HEAD~1`로 로컬 커밋 취소 후 변경 유지 | Lim | 완료 |
| 3 | `git revert`로 push된 커밋 되돌리기 | Lee | 완료 |
| 4 | `git stash` / `git stash pop`으로 작업 임시 보관 | Son | 완료 |

## 1. 최근 커밋 메시지 수정: `git commit --amend`

### 참여자

- Park

### 상황

최근 커밋 메시지를 `update`처럼 변경 내용을 알기 어려운 문구로 작성했다. 아직 원격 저장소에 push하기 전이었기 때문에 `git commit --amend`로 커밋 메시지만 수정했다.

### 사용한 명령어

```bash
git log --oneline -1
git commit --amend -m "docs: add park practice note"
git log --oneline -1
```

### 해결 절차

1. `git log --oneline -1`로 가장 최근 커밋 메시지를 확인했다.
2. 커밋이 아직 push되지 않은 로컬 커밋인지 확인했다.
3. `git commit --amend -m "docs: add park practice note"`로 메시지를 수정했다.
4. 다시 `git log --oneline -1`로 수정 결과를 확인했다.

### 결과

의미 없는 커밋 메시지를 팀 커밋 컨벤션에 맞는 메시지로 변경했다.

### 주의점

- 이미 원격에 push한 커밋을 amend하면 커밋 해시가 바뀐다.
- 공유 브랜치에서 amend 후 강제 push가 필요해지는 상황은 팀 합의 없이 진행하지 않는다.

## 2. 로컬 커밋 취소 후 변경 유지: `git reset --soft HEAD~1`

### 참여자

- Lim

### 상황

문서 수정 중 아직 분리해야 할 변경사항을 한 커밋에 함께 담았다. 원격에 push하기 전이었으므로 커밋만 취소하고 작업 내용은 staging 상태로 유지했다.

### 사용한 명령어

```bash
git log --oneline -3
git reset --soft HEAD~1
git status
git commit -m "docs: add lim practice note"
```

### 해결 절차

1. `git log --oneline -3`으로 최근 커밋을 확인했다.
2. 되돌릴 커밋이 로컬 커밋인지 확인했다.
3. `git reset --soft HEAD~1`로 최근 커밋을 취소했다.
4. `git status`로 변경사항이 staging 상태에 남아 있는지 확인했다.
5. 변경 파일을 작업 단위에 맞게 확인한 뒤 다시 커밋했다.

### 결과

작업 내용은 잃지 않고 커밋만 취소하여 더 적절한 커밋 메시지와 작업 단위로 정리했다.

### 주의점

- `--soft`는 변경사항을 staging 영역에 남긴다.
- `--mixed`는 staging을 해제하고 working tree에 남긴다.
- `--hard`는 변경사항을 삭제할 수 있으므로 과제 협업 중에는 매우 신중하게 사용한다.

## 3. 원격에 push된 커밋 취소: `git revert`

### 참여자

- Lee

### 상황

이미 원격 브랜치에 push된 커밋에서 잘못된 파일 수정이 발견되었다. 공유 히스토리를 바꾸지 않기 위해 `reset` 대신 `revert`를 사용했다.

### 사용한 명령어

```bash
git log --oneline
git revert <되돌릴_커밋해시>
git status
git log --oneline -3
git push origin <현재브랜치>
```

### 해결 절차

1. `git log --oneline`으로 되돌릴 커밋 해시를 확인했다.
2. `git revert <되돌릴_커밋해시>`를 실행했다.
3. 충돌이 없으면 자동으로 revert 커밋을 생성했다.
4. `git log --oneline -3`으로 되돌림 커밋이 추가되었는지 확인했다.
5. 원격 브랜치에 push하고 PR에 되돌림 사유를 적었다.

### 결과

원격 히스토리를 재작성하지 않고 잘못된 변경만 취소하는 새 커밋을 만들었다.

### 주의점

- `revert`는 기존 커밋을 삭제하지 않고 반대 변경을 담은 새 커밋을 만든다.
- 이미 push된 커밋을 취소할 때는 `reset`보다 `revert`가 협업에 안전하다.
- merge commit을 revert할 때는 `-m` 옵션이 필요할 수 있으므로 팀원과 확인한다.

## 4. 작업 임시 보관: `git stash` / `git stash pop`

### 참여자

- Son

### 상황

feature 브랜치에서 문서를 수정하던 중 main의 최신 변경을 먼저 받아야 했다. 아직 커밋하기 애매한 작업이 있었기 때문에 stash로 임시 보관한 뒤 브랜치를 전환했다.

### 사용한 명령어

```bash
git status
git stash push -m "docs: temporary practice note draft"
git switch main
git pull origin main
git switch <작업브랜치>
git stash list
git stash pop
git status
```

### 해결 절차

1. `git status`로 커밋하지 않은 변경사항을 확인했다.
2. `git stash push -m "docs: temporary practice note draft"`로 작업을 임시 저장했다.
3. `main`으로 이동해 최신 변경사항을 가져왔다.
4. 다시 작업 브랜치로 돌아왔다.
5. `git stash list`로 저장된 stash를 확인했다.
6. `git stash pop`으로 임시 저장한 작업을 복원했다.
7. 충돌 여부를 확인한 뒤 작업을 이어갔다.

### 결과

커밋하기 전 작업을 잃지 않고 브랜치를 전환했으며, 최신 main 기준으로 작업을 계속 진행했다.

### 주의점

- `stash pop`은 적용에 성공하면 stash 목록에서 항목을 제거한다.
- 복원 후 충돌이 날 수 있으므로 반드시 `git status`로 확인한다.
- 보관 기록을 남기고 싶으면 `git stash apply`를 사용한 뒤 필요할 때 직접 drop할 수 있다.

## 명령어 비교 정리

| 명령어 | 주요 목적 | 변경사항 보존 | 협업 시 주의점 |
| --- | --- | --- | --- |
| `git commit --amend` | 최근 커밋 수정 | 보존 | push 전 로컬 커밋에 사용하는 것이 안전 |
| `git reset --soft HEAD~1` | 최근 커밋 취소 | staging 상태로 보존 | push된 커밋에는 사용하지 않는 것이 안전 |
| `git revert <hash>` | 특정 커밋 되돌리기 | 새 커밋으로 반대 변경 생성 | 공유 브랜치에서 안전한 취소 방법 |
| `git stash` | 작업 임시 보관 | stash 영역에 보존 | `pop` 후 충돌 여부 확인 필요 |
