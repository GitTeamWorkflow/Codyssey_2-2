# 🟩 Git Restore 학습 노트

## 🟢 전체 목차
- ✅ 01_git_restore.md: 아직 커밋하지 않은 수정 내용을 되돌리는 방법
- 02_git_reset.md: 최근 커밋을 취소하고 작업물은 남기는 방법
- 03_git_revert.md: 이미 공유된 커밋을 안전하게 취소하는 방법
- 04_git_stash.md: 작업 중인 내용을 잠시 보관하는 방법

<br><br>

## 🟢 1. Git Restore 뜻

### 🟡 Restore는 되돌리기다
- Restore는 한국어로 `복원하다`, `되돌리다`라는 뜻이다.
- `git restore`는 아직 커밋하지 않은 파일 변경 내용을 되돌릴 때 사용한다.
- 쉽게 말하면, 파일을 마지막 커밋 상태로 다시 돌리는 명령어다.

### 🟡 초등학생 버전 설명
- 공책에 연필로 잘못 썼다.
- 아직 선생님에게 제출하지 않았다.
- 지우개로 방금 쓴 내용을 지운다.
- Git에서는 이 지우개 역할을 `git restore`가 한다.

<br><br>

## 🟢 2. Git Restore가 필요한 상황

| 상황 | 사용 명령어 | 결과 |
| --- | --- | --- |
| 파일을 수정했는데 취소하고 싶다 | `git restore 파일명` | 파일 내용이 마지막 커밋 상태로 돌아간다. |
| `git add`한 파일을 stage에서 내리고 싶다 | `git restore --staged 파일명` | 파일은 유지되고 stage에서만 내려간다. |
| 여러 파일 수정을 한 번에 취소하고 싶다 | `git restore .` | 현재 폴더 아래 수정 내용이 되돌아간다. |

<br><br>

## 🟢 3. 꼭 알아야 하는 Git 상태 3단계

| 단계 | 영어 | 뜻 | 설명 |
| --- | --- | --- | --- |
| 작업 폴더 | Working Tree | 실제 파일을 수정하는 공간 | VS Code, Vim 등으로 파일을 고치는 곳이다. |
| 스테이지 | Staging Area | 커밋할 파일을 올려두는 대기 공간 | `git add`를 하면 이곳으로 올라간다. |
| 저장소 | Repository | 커밋이 저장되는 공간 | `git commit`을 하면 기록으로 저장된다. |

### 🟡 흐름 그림
```txt
Working Tree
↓ git add
Staging Area
↓ git commit
Repository
```

### 🟡 restore가 되돌리는 위치
```txt
git restore 파일명
=> Working Tree의 수정 내용을 되돌림

git restore --staged 파일명
=> Staging Area에 올린 것을 다시 내림
```

<br><br>

## 🟢 4. 명령어 1: 수정한 파일 되돌리기

### 🟡 상황
- `bye.txt` 파일을 수정했다.
- 그런데 수정한 내용이 마음에 들지 않는다.
- 마지막 커밋 상태로 되돌리고 싶다.

### 🟡 실습 순서
```bash
vim bye.txt
git status
git restore bye.txt
git status
```

### 🟡 명령어 해설
| 명령어 | 풀네임 또는 뜻 | 설명 |
| --- | --- | --- |
| `vim` | Vi IMproved | 터미널에서 파일을 수정하는 편집기다. |
| `git` | Git | 버전 관리 프로그램 이름이다. 공식 약자 풀네임은 없다. |
| `status` | status | 현재 파일 변경 상태를 보여준다. |
| `restore` | restore | 수정된 파일을 마지막 커밋 상태로 되돌린다. |
| `bye.txt` | text file | 실습에 사용할 텍스트 파일이다. |
| `.txt` | text | 일반 텍스트 파일 확장자다. |

### 🟡 결과
- `bye.txt`의 수정 내용이 사라진다.
- 마지막 커밋에 저장되어 있던 내용으로 돌아간다.
- 이 작업은 되돌리기 어렵기 때문에 실행 전에 진짜 지워도 되는지 확인해야 한다.

<br><br>

## 🟢 5. 명령어 2: Stage에 올린 파일 내리기

### 🟡 상황
- 파일을 수정했다.
- `git add`로 stage에 올렸다.
- 그런데 아직 커밋하고 싶지 않다.
- 파일 내용은 그대로 두고, stage에서만 내리고 싶다.

### 🟡 실습 순서
```bash
vim bye.txt
git status
git add bye.txt
git status
git restore --staged bye.txt
git status
```

### 🟡 명령어 해설
| 명령어 | 풀네임 또는 뜻 | 설명 |
| --- | --- | --- |
| `add` | add | 변경 파일을 Staging Area에 올린다. |
| `restore` | restore | 파일 상태를 되돌린다. |
| `--staged` | staged | Staging Area에 올라간 파일을 다시 내린다. |
| `bye.txt` | text file | stage에서 내릴 파일이다. |

### 🟡 결과
- 파일 내용은 사라지지 않는다.
- `git add`만 취소된다.
- 다시 커밋하려면 `git add bye.txt`를 다시 해야 한다.

<br><br>

## 🟢 6. `git restore`와 `git restore --staged` 차이

| 명령어 | 파일 내용 삭제 여부 | stage 취소 여부 | 언제 사용 |
| --- | --- | --- | --- |
| `git restore 파일명` | 삭제됨 | 해당 없음 | 수정한 내용 자체를 버릴 때 |
| `git restore --staged 파일명` | 삭제 안 됨 | 취소됨 | `git add`만 취소하고 싶을 때 |

### 🟡 핵심 차이
- `git restore 파일명`은 파일 내용을 되돌린다.
- `git restore --staged 파일명`은 stage에 올린 것만 취소한다.
- 둘은 전혀 다르다.

<br><br>

## 🟢 7. 여러 파일 한 번에 되돌리기

### 🟡 현재 폴더 전체 수정 취소
```bash
git restore .
```

### 🟡 명령어 해설
| 명령어 | 풀네임 또는 뜻 | 설명 |
| --- | --- | --- |
| `.` | current directory | 현재 폴더를 뜻한다. |

### 🟡 주의
- `git restore .`는 현재 폴더 아래의 수정 내용을 한 번에 되돌린다.
- 여러 파일의 수정 내용이 사라질 수 있다.
- 초보자는 먼저 `git status`로 어떤 파일이 바뀌었는지 확인한 뒤 사용해야 한다.

<br><br>

## 🟢 8. Restore, Reset, Revert 차이

| 명령어 | 주로 되돌리는 대상 | 기록 삭제 여부 | 안전한 사용 상황 |
| --- | --- | --- | --- |
| `git restore` | 아직 커밋하지 않은 파일 수정 | 커밋 기록과 관계 없음 | 내 작업 폴더 수정 취소 |
| `git reset` | 커밋 기록 | 기록을 바꿀 수 있음 | push 전, 혼자 작업한 커밋 취소 |
| `git revert` | 특정 커밋의 효과 | 기록 삭제 안 함 | 이미 push한 커밋 취소 |

### 🟡 초보자 판단 기준
- 파일만 잘못 고쳤다: `git restore`
- 방금 커밋했는데 커밋을 취소하고 싶다: `git reset`
- GitHub에 이미 올린 커밋을 취소하고 싶다: `git revert`

<br><br>

## 🟢 9. 실수 방지 체크리스트

| 확인 | 이유 |
| --- | --- |
| `git status`를 먼저 봤는가 | 어떤 파일이 바뀌었는지 알아야 한다. |
| 파일 내용을 버려도 되는가 | `git restore 파일명`은 수정 내용을 없앤다. |
| stage만 취소하려는 것인가 | 그 경우 `--staged`를 붙여야 한다. |
| 여러 파일을 한 번에 되돌리는가 | `git restore .`는 영향 범위가 크다. |

<br><br>

## 🟢 10. 자주 나오는 출력 해석

### 🟡 수정된 파일이 있을 때
```txt
modified: bye.txt
```

- `modified`는 파일이 수정되었다는 뜻이다.
- 이 상태에서 `git restore bye.txt`를 하면 수정 내용이 사라진다.

### 🟡 stage에 올라간 파일이 있을 때
```txt
Changes to be committed:
    modified: bye.txt
```

- `Changes to be committed`는 커밋 예정이라는 뜻이다.
- 이 상태에서 `git restore --staged bye.txt`를 하면 stage에서 내려간다.

<br><br>

## 🟢 11. 한 줄 요약

- `git restore 파일명`은 수정 내용을 버린다.
- `git restore --staged 파일명`은 `git add`만 취소한다.
- 실행 전에는 반드시 `git status`로 상태를 확인한다.
