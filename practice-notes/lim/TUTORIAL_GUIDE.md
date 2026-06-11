# Git 실습 튜토리얼 — 손으로 직접 쳐보기

> 짝꿍 문서: [`STUDY_GUIDE.md`](./STUDY_GUIDE.md) (개념·명령어 레퍼런스).
> 이 튜토리얼은 **혼자, 안전한 연습용 폴더에서** `MISSION.md`에 나오는 모든 Git 기술을 손으로 굴려보는 게 목적입니다.
> 팀 레포를 건드리지 않으므로 마음껏 망쳐도 됩니다. (Lab 11만 GitHub 필요 — 선택)

## ⚠️ zsh 사용자 주의 (macOS 기본 셸)
- 명령에 **느낌표 `!`** 가 있으면 zsh가 히스토리 확장으로 가로채 `zsh: event not found` 에러가 납니다.
- 해결: 그 문자열은 **작은따옴표**로 감싸세요. 예) `printf 'Hello team!\n'` (큰따옴표 ❌)
- 작은따옴표 안에서도 `printf` 는 `\n` 을 줄바꿈으로 처리하니 동작은 동일합니다.

## 사용법
- 코드 블록을 **한 줄씩** 복붙하면서, "지금 무슨 일이 일어났나"를 매번 `git status` / `git log --oneline --graph` 로 확인하세요.
- 각 Lab 끝의 **✅ 확인** 기준이 나오면 통과.
- 각 Lab의 **🔗 미션 매핑**으로 "이게 미션 어디에 필요한지" 연결해 두세요.
- 각 Lab의 **📸 캡쳐 포인트**에서 화면을 캡쳐해 [스크린샷 양식](#-스크린샷-양식)대로 끼워 넣으세요. (미션 증빙으로 재활용됨)
- 다 끝나면 [마지막 정리](#실습-끝-정리)로 연습 폴더를 지웁니다.

---

## 📸 스크린샷 양식

실습하면서 찍은 캡쳐를 일관된 규칙으로 정리해두면, 그대로 **미션 증빙**(`SUBMISSION.md`의 evidence, 충돌/트러블슈팅 기록)으로 재활용됩니다.

### 1) 이미지 저장 위치
연습 폴더가 아니라 **남길 문서 폴더** 안에 모읍니다.

```text
docs/
├── TUTORIAL_GUIDE.md        ← 이 문서
└── images/                ← 캡쳐 이미지 폴더 (없으면 새로 만들기)
    ├── lab01-log.png
    └── lab03-conflict-markers.png
```

```bash
# 이미지 폴더 한 번만 생성
mkdir -p ~/Codyssey/B2-2/docs/images
```

### 2) 파일명 규칙
`lab<번호>-<무엇>.png` 형식 — 나중에 정렬·검색이 쉽습니다.

| 예시 파일명 | 무엇을 찍은 것 |
| --- | --- |
| `lab01-log-graph.png` | `git log --oneline --graph --all` 결과 |
| `lab03-conflict-markers.png` | 충돌 마커가 보이는 파일 |
| `lab06-revert-history.png` | revert 커밋이 추가된 히스토리 |

### 3) 복붙용 임베드 템플릿
캡쳐를 넣을 자리에 아래를 붙이고 경로·캡션만 바꾸면 됩니다.

```markdown
**📸 캡쳐: <무엇을 보여주는지>**

![<대체텍스트>](./images/lab01-log-graph.png)

> 위 화면: `git log --oneline --graph --all` 결과. main 이 최신 커밋을 가리킨다.
```

> 💡 GitHub PR/이슈 본문에서는 이미지 파일을 **드래그&드롭**하면 자동으로 `![...](url)` 이 생성됩니다. 레포에 커밋해 넣을 때만 위의 상대경로(`./images/...`)를 씁니다.

### 4) 캡쳐 팁 (macOS)
- 영역 캡쳐: `⌘ + Shift + 4` → 드래그 (바탕화면에 `.png` 저장됨)
- 특정 창만: `⌘ + Shift + 4` 누른 뒤 `Space` → 창 클릭
- 저장된 파일을 `docs/images/` 로 옮기고 위 규칙대로 이름 변경

### 5) 꼭 남겨야 하는 캡쳐 (미션 증빙과 직결)
| 캡쳐 | 어느 Lab에서 | 미션 증빙 |
| --- | --- | --- |
| `git log --oneline --graph --all` 그래프 | Lab 2 / 전체 끝 | 2장 "Git 히스토리 증빙" |
| 충돌 마커가 찍힌 파일 | Lab 3, Lab 4 | 4.7 `conflict-resolution.md` |
| `reset --soft` / `revert` / `stash` / `amend` 전·후 화면 | Lab 5~8 | 4.8 `troubleshooting-log.md` |
| PR 화면 (Closes # 연결) | Lab 11 | 4.3 / `SUBMISSION.md` |

---

## Lab 0 — 연습용 샌드박스 만들기

```bash
# 홈 아래에 버려도 되는 연습 폴더 생성 (팀 레포와 완전 분리)
mkdir -p ~/git-practice && cd ~/git-practice

# 커밋 작성자 확인 (비어 있으면 채우기 — STUDY_GUIDE 가정 #7)
git config user.name
git config user.email
# 비었다면:
# git config --global user.name "Your Name"
# git config --global user.email "you@example.com"
```

✅ **확인:** `pwd` 가 `~/git-practice`, `git config user.email` 이 GitHub 계정과 같다.

---

## Lab 1 — init / add / commit / log (기본 + "브랜치=포인터" 감 잡기)

```bash
cd ~/git-practice
git init demo && cd demo

# 첫 파일 만들고 커밋
printf "# Demo\n" > README.md
git status                       # Untracked files 에 README.md
git add README.md
git status                       # Changes to be committed
git commit -m "docs: add README"

# 두 번째 커밋
printf "line 1\n" > notes.txt
git add notes.txt
git commit -m "docs: add notes with line 1"

# 히스토리 + 브랜치 포인터 보기
git log --oneline --graph --all
git branch -v                    # main 이 마지막 커밋을 가리킴(=포인터)
cat .git/HEAD                    # HEAD 가 refs/heads/main 을 가리킴
```

✅ **확인:** `git log --oneline` 에 커밋 2개. `git branch -v` 에서 `main` 이 최신 커밋 해시를 가리킨다.
🔗 **미션 매핑:** 과제 목표 "브랜치=커밋을 가리키는 포인터" 설명 / 커밋 컨벤션(4.4).

---

## Lab 2 — 브랜치 만들고 병합하기 (GitHub Flow 축소판)

```bash
cd ~/git-practice/demo

# feature 브랜치 분기 (미션 네이밍 흉내)
git switch -c feature/add-greeting
printf 'Hello team!\n' >> notes.txt   # 느낌표(!) 때문에 작은따옴표 사용 (zsh 주의 참고)
git add notes.txt
git commit -m "feat: add greeting line"

# main 으로 돌아가 병합
git switch main
git merge feature/add-greeting
git log --oneline --graph --all

# 머지 끝난 브랜치 정리 (GitHub Flow)
git branch -d feature/add-greeting
```

✅ **확인:** `main` 의 `notes.txt` 에 "Hello team!" 이 들어왔다. 브랜치 삭제됨.
🔗 **미션 매핑:** GitHub Flow(4.2) — feature 분기 → 작업 → main 병합 → 브랜치 삭제.

---

## Lab 3 — 충돌 만들고 해결하기 (자명한 충돌)

> ⚠️ **순서가 핵심:** 충돌은 *공통 조상에서 양쪽이 갈라져 같은 줄을 다르게* 바꿔야 납니다.
> "브랜치 먼저 분기 → **main과 feature 양쪽 모두** 커밋" 순서를 지켜야 해요.
> (main을 안 움직이고 feature만 고치면 충돌 없이 fast-forward 됩니다.)

```bash
cd ~/git-practice/demo

# 1) 먼저 브랜치를 분기한다 (공통 조상에서 갈라짐)
git switch -c feature/title-change
printf '# Cool Project (feature side)\n' > README.md
git commit -am "docs: change README title on feature"

# 2) main 으로 돌아가 '같은 줄'을 다르게 수정한다 ← 이 커밋이 있어야 갈라짐!
git switch main
printf '# Demo (main side)\n' > README.md
git commit -am "docs: update README title on main"
#    ↑ 여기서 반드시 '1 file changed' 가 떠야 함!
#      'nothing to commit, working tree clean' 이 뜨면 = 내용이 이미 같다는 뜻 →
#      main 이 안 움직여서 충돌 대신 fast-forward 됨. README 에 다른 텍스트를 써서 다시.

# 3) main 으로 합치면 → 충돌!
git merge feature/title-change   # CONFLICT 발생
git status                        # both modified: README.md
cat README.md                     # <<<<<<< ======= >>>>>>> 마커 확인
```

이제 **마커를 직접 지우고** 원하는 내용으로 정리합니다(에디터로 README.md 열기). 예를 들어 한 줄로:

```text
# Demo Project
```

`<<<<<<<`, `=======`, `>>>>>>>` 줄을 **한 줄도 남기지 말고** 지운 뒤:

```bash
git add README.md
git commit                        # 머지 커밋 메시지 그대로 저장하면 완료
git switch -c /dev/null 2>/dev/null; git branch -d feature/title-change 2>/dev/null
git log --oneline --graph --all
```

✅ **확인:** `grep -n "<<<<<<<\|=======\|>>>>>>>" README.md` 결과가 **비어 있다**(마커 잔존 없음).
🔗 **미션 매핑:** 4.7 충돌 해결 / 마커 의미 설명(과제 목표). 실제 미션에선 이 과정을 `docs/conflict-resolution.md` 에 기록.

> 💡 충돌이 꼬이면 언제든 `git merge --abort` 로 병합 직전으로 되돌릴 수 있다.

---

## Lab 4 — 비자명 충돌 (이름변경 vs 내용수정)

미션 4.7은 "최소 1회 비자명 충돌"을 요구합니다.

> ⚠️ **흔한 함정:** 한쪽이 "이름만" 바꾸고(내용 동일) 다른 쪽이 "내용만" 바꾸면 → Git의 rename 감지가
> 알아서 합쳐버려서(`Merge made by the 'ort' strategy`) **충돌이 안 납니다.**
> 진짜 충돌을 보려면 **양쪽이 같은 줄을 다르게** 건드려야 하고, 거기에 한쪽 rename을 얹습니다.

```bash
cd ~/git-practice/demo

# 공통 출발점에 파일 하나 준비
printf 'line A\nline B\n' > data.txt
git add data.txt && git commit -m "feat: add data.txt"

# feature 브랜치: 이름 변경 + line A 수정
git switch -c feature/rename-data
git mv data.txt info.txt
printf 'line A (feature)\nline B\n' > info.txt
git commit -am "refactor: rename data.txt to info.txt and edit line A"

# main: '같은 line A'를 다르게 수정 (이름은 그대로) ← '1 file changed' 확인!
git switch main
printf 'line A (main)\nline B\n' > data.txt
git commit -am "feat: edit line A in data.txt"

# 병합 → rename + content 충돌!
git merge feature/rename-data
git status                        # 'both modified: info.txt' (rename된 이름으로)
cat info.txt                      # <<<<<<< ======= >>>>>>> 마커 확인
```

> ⚠️ **두 가지 결과가 나올 수 있습니다** (Git의 rename 감지가 닮은 정도로 판단하기 때문):
>
> **(A) content 충돌** — rename으로 인식됨 → `info.txt` 안에 `<<<<<<<` 마커 생김. Lab 3처럼 정리:
> ```bash
> # info.txt 의 마커를 원하는 한 줄로 정리한 뒤
> grep -n '<<<<<<<\|=======\|>>>>>>>' info.txt   # 비어 있으면 OK
> git add info.txt && git commit
> ```
>
> **(B) modify/delete 충돌** — rename 인식 안 됨 → `CONFLICT (modify/delete): data.txt deleted in ... and modified in HEAD`.
> 이땐 **마커가 없습니다.** 파일을 살릴지/지울지 *결정*해서 알려줍니다:
> ```bash
> git rm data.txt                       # 옛 이름은 삭제 채택 (info.txt로 옮겨갔으니)
> printf 'line A (merged)\nline B\n' > info.txt   # 최종 내용 결정(선택)
> git add info.txt
> git commit
> ```
> 결정 규칙: 유지=`git add <파일>`, 삭제 채택=`git rm <파일>`.

```bash
git log --oneline --graph --all   # (A)(B) 어느 쪽이든 머지 커밋 확인
```

✅ **확인:** 최종에 `info.txt` 만 남고(`data.txt` 없음), 마커/Unmerged 잔존 없음.
🔗 **미션 매핑:** 4.7 "비자명 충돌"(이름변경 + 내용수정 동시). 이 기록을 `conflict-resolution.md` 에 남기면 비자명 1회 충족.

> 💡 더 단순하게 충돌 내는 법: 한쪽 `git rm data.txt` (삭제) + 다른쪽 내용수정 → 항상 `CONFLICT (modify/delete)`. 미션은 "이름변경 **또는 삭제**"를 모두 인정(4.7).

---

## Lab 5 — `reset --soft HEAD~1` (마지막 커밋만 취소, 변경은 유지)

```bash
cd ~/git-practice/demo

printf "oops half-done\n" >> notes.txt
git commit -am "wip"              # 일부러 '의미없는 메시지'로 커밋 (4.4 금지 예)

git log --oneline | head -3
git reset --soft HEAD~1           # 커밋만 취소, 변경은 staged 로 살아있음
git status                        # notes.txt 가 'Changes to be committed'
git commit -m "docs: add work-in-progress notes (재작성)"   # 제대로 다시 커밋
```

✅ **확인:** "wip" 커밋이 사라지고, 같은 변경이 **의미있는 메시지**로 다시 커밋됨.
🔗 **미션 매핑:** 4.8 `reset --soft HEAD~1`. ⚠️ `--hard` 는 변경까지 삭제하니 연습에서도 주의.

---

## Lab 6 — `revert` (push된 커밋을 안전하게 되돌리기)

```bash
cd ~/git-practice/demo

# 되돌릴 '나쁜' 커밋을 하나 만든다
printf "BUGGY CHANGE\n" >> notes.txt
git commit -am "feat: add buggy change"
BAD=$(git rev-parse HEAD)         # 방금 커밋 해시 저장

# revert: 되돌리는 '새 커밋'을 추가 (히스토리는 보존)
git revert --no-edit $BAD
git log --oneline | head -3       # 'Revert "feat: add buggy change"' 커밋이 위에 추가됨
tail -n3 notes.txt                # BUGGY CHANGE 가 사라짐
```

✅ **확인:** 히스토리에 원래 커밋 + Revert 커밋이 **둘 다** 남고, 파일 내용은 되돌려졌다.
🔗 **미션 매핑:** 4.8 `revert`. **왜 reset이 아니라 revert?** → 이미 공유(push)된 히스토리는 지우면 안 되니까(제약: 공유 브랜치 히스토리 재작성 금지). 이 "왜"를 troubleshooting-log에 적기.

---

## Lab 7 — `stash` / `stash pop` (작업 임시 보관)

```bash
cd ~/git-practice/demo

# 커밋하기 애매한 작업 중인데 브랜치를 옮겨야 하는 상황
printf "temporary experiment\n" >> notes.txt
git status                        # 변경 있음
git switch main 2>&1 || true      # (변경 때문에 막히는 경우가 있음)

git stash                         # 작업을 선반에 올림 → 작업트리 깨끗
git status                        # clean
git stash list                    # stash@{0} 보관 확인

# 다른 일 하고 돌아와서 복원
git stash pop                     # 보관한 변경 복원
git status                        # temporary experiment 변경 복귀
git checkout -- notes.txt         # 실험 변경 버리기(연습 정리)
```

✅ **확인:** `git stash` 직후 트리가 clean, `git stash pop` 후 변경이 되돌아온다.
🔗 **미션 매핑:** 4.8 `stash`/`stash pop`.

---

## Lab 8 — `commit --amend` (최근 커밋 메시지 고치기)

```bash
cd ~/git-practice/demo

printf "v1\n" > version.txt
git add version.txt
git commit -m "add verison"       # 오타 메시지

git commit --amend -m "feat: add version.txt"   # 메시지 교정
git log --oneline | head -1       # 'feat: add version.txt' 로 바뀜
```

✅ **확인:** 직전 커밋 메시지가 새 메시지로 교체됨(해시도 바뀜).
🔗 **미션 매핑:** 4.8 `commit --amend`. ⚠️ **이미 push한 커밋이면** 해시가 바뀌어 강제 푸시가 필요 → 공유 브랜치에선 주의.

---

## Lab 9 — `rebase -i` 로 커밋 정리 (보너스, squash/reword)

```bash
cd ~/git-practice/demo
git switch -c feature/messy-history

# 지저분한 커밋 3개 만들기
printf "a\n" >> log.txt && git add log.txt && git commit -m "wip a"
printf "b\n" >> log.txt && git commit -am "wip b"
printf "c\n" >> log.txt && git commit -am "wip c"
git log --oneline | head -3

# 최근 3개를 대화형으로 정리 (에디터가 열림)
git rebase -i HEAD~3
```

에디터에서 첫 줄은 `pick`, 나머지 두 줄은 `squash`(또는 `s`)로 바꿔 저장하면 3개가 1개로 합쳐집니다. 그 다음 합쳐진 커밋 메시지를 `feat: add log entries a/b/c` 처럼 다시 씁니다.

```bash
git log --oneline | head -2       # 커밋 1개로 정리됨
```

✅ **확인:** 3개의 wip 커밋이 1개의 의미있는 커밋으로 합쳐졌다.
🔗 **미션 매핑:** 5장 보너스(히스토리 정리). ⚠️ **개인 feature 브랜치에서만.** 공유 브랜치 rebase 금지(제약).

> 💡 메시지만 고치고 싶으면 `squash` 대신 `reword`(`r`)를 쓴다.

---

## Lab 10 — `reflog` 안전망 (실수 복구)

```bash
cd ~/git-practice/demo

# 일부러 위험한 reset
git log --oneline | head -1
git reset --hard HEAD~1           # 마지막 커밋 날림 (변경까지 삭제)
git log --oneline | head -1       # 커밋 사라짐

# reflog 로 사라진 커밋 해시 찾아 복구
git reflog | head -5
git reset --hard HEAD@{1}         # 바로 직전 상태로 복구
git log --oneline | head -1       # 복구됨
```

✅ **확인:** `--hard` 로 날린 커밋을 `reflog` 로 되살렸다.
🔗 **미션 매핑:** 트러블슈팅 안전망(STUDY_GUIDE 6장). 실수했을 때 당황 안 하기.

---

## Lab 11 — (선택) GitHub Flow + PR 실습

> 이건 GitHub 원격이 필요합니다. **팀 레포가 아니라**, 연습용 개인 레포를 하나 만들어 PR 흐름만 익히세요.

```bash
# (사전) GitHub에 빈 연습 레포 생성 후 URL 준비, 또는 gh 사용
cd ~/git-practice/demo
git remote add origin https://github.com/<you>/git-practice-demo.git
git push -u origin main

# 이슈 → 브랜치 → 작업 → PR
gh issue create --title "연습: greeting 함수 추가" --body "PR 흐름 연습"   # 이슈번호 확인
git switch -c feature/practice-pr
printf "def hi():\n    return 'hi'\n" > hi.py
git add hi.py && git commit -m "feat: add hi() function"
git push -u origin feature/practice-pr

gh pr create --base main --title "feat: add hi()" \
  --body "Closes #1

## What
- hi() 추가
## Why
- PR 흐름 연습
## How
- 로컬 실행 확인"
```

✅ **확인:** GitHub에서 PR이 보이고, 본문 `Closes #1` 이 이슈와 연결된다.
🔗 **미션 매핑:** 4.3(Issue↔PR), 4.5(PR 생성), 본문 What/Why/How. **실제 미션은 이 흐름을 팀 레포에서 반복**하는 것.

---

## 미션 기술 ↔ Lab 대응표

| 미션 요구 | 관련 Lab |
| --- | --- |
| 브랜치=포인터 이해 (과제목표) | Lab 1 |
| GitHub Flow (4.2) | Lab 2, Lab 11 |
| 충돌 해결 (4.7) | Lab 3 |
| 비자명 충돌 (4.7) | Lab 4 |
| `reset --soft` (4.8) | Lab 5 |
| `revert` (4.8) | Lab 6 |
| `stash`/`stash pop` (4.8) | Lab 7 |
| `commit --amend` (4.8) | Lab 8 |
| `rebase -i` (보너스) | Lab 9 |
| 실수 복구 안전망 | Lab 10 |
| Issue↔PR, PR 본문 (4.3/4.5) | Lab 11 |

> 이 표의 Lab을 한 번씩 손으로 통과하면, 미션에서 실제로 칠 명령은 전부 경험한 상태가 됩니다.

---

## 실습 끝, 정리

```bash
# 연습 폴더 통째로 삭제 (팀 레포와 무관)
rm -rf ~/git-practice
```

⚠️ `rm -rf` 는 되돌릴 수 없으니 경로(`~/git-practice`)가 맞는지 한 번 더 확인하고 실행하세요.
