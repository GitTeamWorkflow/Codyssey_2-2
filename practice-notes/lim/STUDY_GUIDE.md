# 학습 가이드 — 실전 Git 협업 워크플로우 (with Claude)

> 이 문서는 `MISSION.md`를 수행하기 위해 **무엇을 공부하고, 어떤 명령어를 알아야 하는지**를 정리한 가이드입니다.
> 대상: "개발은 하지만 **Git 협업(팀 단위 PR/리뷰/충돌 해결)** 은 처음"인 사람.
>
> 📌 *이 가이드는 Claude(Claude Code)가 `MISSION.md`를 분석해 초안으로 정리해준 자료입니다. 직접 손으로 쳐보는 실습은 [`TUTORIAL_GUIDE.md`](./TUTORIAL_GUIDE.md)에서 진행하세요.*

---

## 0. 가정 / 확인 필요

> 추측으로 채우지 않고 모아둔 항목입니다. 시작 전에 팀과 확인하세요.

| # | 항목 | 왜 확인이 필요한가 | 현재 레포에서 확인된 사실 |
| --- | --- | --- | --- |
| 1 | **이 레포는 아직 git 저장소가 아님** | 현재 `B2-2/` 폴더에는 `MISSION.md` 하나뿐이고 `.git`, `README`, `package.json`, `Dockerfile`, `src/` 등이 **전혀 없음**. 즉 모든 것을 처음부터 만들어야 함 | `git rev-parse` 실패(레포 아님), 파일은 `MISSION.md`만 존재 |
| 2 | **팀 인원/역할** | 3~5인 필수(제약 사항). 누가 어떤 문서/유틸을 맡을지 미정 | MISSION.md에 인원 범위만 명시 |
| 3 | **저장소 위치(옵션 A vs B)** | 옵션 A=GitHub Organization(권장), 옵션 B=개인 저장소 + Collaborator. **Branch Protection의 "승인 1명 필요"는 보통 Organization 또는 public 레포에서 더 쉽게 동작** → 팀 계정 플랜에 따라 다름 | MISSION 4.1 |
| 4 | **"간단한 결과물" 택1 (A/B/C)** | 유틸 함수(A) / 팀 소개(B) / 학습 노트(C) 중 무엇을 만들지 팀 합의 필요. 선택에 따라 `src/` 또는 `team/` 폴더 구성이 달라짐 | MISSION 4.10 |
| 5 | **Python 사용 여부** | 개발 환경에 "Python 3.10 이상"이 적혀 있으나, 결과물(A)에서 "언어 자유"라 Python이 **필수는 아님**. 유틸 함수를 Python으로 할지 결정 필요 | MISSION 6, 4.10 |
| 6 | **GitHub CLI(`gh`) 설치 여부** | PR/이슈를 터미널에서 만들면 편하지만, 웹 UI로도 전부 가능. 설치 안 했다면 웹으로 진행 가능 | 미확인 (로컬에 설치 여부 확인 필요) |
| 7 | **로컬 git 사용자 설정** | 커밋에 이름/이메일이 박힘. `git config user.name/email`이 GitHub 계정과 맞는지 확인해야 기여도가 올바르게 집계됨 | 미확인 |

---

## 1. 미션 요약

**한 줄:** 3~5인 팀이 하나의 GitHub 저장소에서 **GitHub Flow(브랜치 → PR → 리뷰 → 머지)** 로 협업하며, 충돌 해결과 Git 트러블슈팅을 **재현 가능한 문서로 남기는** 미션.

핵심은 "복잡한 코드 구현"이 아니라 **협업 과정(브랜치·PR·리뷰·이슈·충돌·기록)** 자체다(제약 7).

### 완료 기준 (Definition of Done)

아래가 **모두** 충족되면 끝난 것이다. (체크리스트는 5장 참고)

- [ ] 팀 GitHub 저장소 1개 + `main`에 **Branch Protection**(직접 push 금지 / PR 머지만 / 승인 1명 이상) — 4.1
- [ ] 폴더 구조: `README.md`, `docs/`, `src/`, (선택)`team/` — 4.1
- [ ] 협업 문서 3종: `docs/CONTRIBUTING.md`, `docs/conflict-resolution.md`, `docs/troubleshooting-log.md` — 4.9 / 4.7 / 4.8
- [ ] `SUBMISSION.md`(팀원별 PR/문서/증빙 인덱스) — 2장
- [ ] GitHub Flow 적용 + 선택 이유 3줄 기록 — 4.2
- [ ] 모든 작업이 **Issue → PR** 로 연동, PR 본문에 `Closes #N` — 4.3
- [ ] 커밋 메시지 컨벤션 준수(의미없는 메시지 금지) — 4.4
- [ ] **팀원별**: PR 생성·머지 2개 / 리뷰 2개(본인 PR 제외) / 본인 PR에 리뷰 반영 1회 — 4.5
- [ ] 각 PR에 **실질 코멘트 1개 이상** + 작성자↔리뷰어 상호작용 1회 — 4.6
- [ ] **충돌 해결 2회 이상**(최소 1회는 "비자명 충돌") → `conflict-resolution.md` 기록 — 4.7
- [ ] **트러블슈팅 4종**(amend / reset --soft / revert / stash) 전부 수행 + 팀원별 1개 이상 참여 → `troubleshooting-log.md` — 4.8
- [ ] "간단한 결과물" 택1 완성, 팀원별 기여 커밋 1건 이상 — 4.10
- [ ] `git log --oneline --graph --all` 히스토리 증빙(텍스트/스크린샷) — 2장
- [ ] (보너스/선택) `git rebase -i` squash·reword 문서화, `.github/CODEOWNERS` — 5장

---

## 2. 선행 개념

> 각 항목: **(a) 한 줄 설명 / (b) 이 미션에서 왜 필요한가**. `[필수]`는 미션 요구사항에 직접 연결, `[선택]`은 보너스/품질 향상.

### 2.1 Git 기본 멘탈 모델

- **커밋 = 스냅샷 + 부모 포인터** `[필수]`
  (a) 커밋은 그 시점의 파일 상태 전체를 가리키며 이전 커밋을 부모로 연결한 노드.
  (b) "브랜치가 커밋을 가리키는 포인터"라는 4.4 과제 목표를 설명하려면 이 그림이 먼저 있어야 함.
- **브랜치 = 커밋을 가리키는 움직이는 라벨** `[필수]`
  (a) 브랜치는 무겁게 복사된 게 아니라 특정 커밋을 가리키는 41바이트 포인터일 뿐.
  (b) 과제 목표 "브랜치가 내부적으로 어떻게 동작하는지" 직접 대응(3장).
- **HEAD** `[필수]`
  (a) "지금 내가 서 있는 커밋/브랜치"를 가리키는 포인터. `HEAD~1`은 한 칸 전 커밋.
  (b) `reset --soft HEAD~1`(4.8) 이해의 핵심.
- **3단계 영역: Working Directory / Staging(Index) / Repository** `[필수]`
  (a) 파일 수정 → `add`로 스테이징 → `commit`으로 저장소 기록. 3개의 분리된 공간.
  (b) `reset`의 `--soft/--mixed/--hard` 차이가 "어느 영역까지 되돌리나"이기 때문(4.8, 과제목표 reset/revert/stash 차이).

### 2.2 협업 모델

- **원격(remote) / origin / clone / push / pull / fetch** `[필수]`
  (a) GitHub의 저장소가 `origin`, 내 PC로 복제가 `clone`, 올리기 `push`, 받기 `pull(=fetch+merge)`.
  (b) 팀 저장소를 받아 작업하고 올리는 모든 흐름의 기본(4.1~4.5).
- **GitHub Flow** `[필수]`
  (a) `main`은 항상 배포 가능, 작업은 `feature/*` 브랜치 → PR → 리뷰 → `main` 머지, 끝나면 브랜치 삭제. (Git Flow보다 단순한 전략)
  (b) 4.2가 GitHub Flow 적용을 직접 요구. 선택 이유 3줄도 써야 함.
- **Pull Request(PR)** `[필수]`
  (a) "내 브랜치를 main에 합쳐달라"는 변경 제안 + 리뷰·토론 공간.
  (b) 4.3/4.5/4.6 전부 PR 중심. 본문에 What/Why/How + `Closes #N` 필수.
- **Issue ↔ PR 연동** `[필수]`
  (a) 작업을 Issue로 등록하고, PR 본문 `Closes #N`으로 머지 시 자동 닫기.
  (b) 4.3 직접 요구. `SUBMISSION.md`에서 팀원별 추적 가능해야 함.
- **코드 리뷰 / Approve / Request changes / 라인 코멘트** `[필수]`
  (a) PR의 특정 라인에 코멘트·질문·대안 제시, 승인(approve)하면 머지 가능.
  (b) 4.5/4.6: 팀원별 리뷰 2개, "LGTM만 금지", 상호작용 1회 기록.
- **Branch Protection Rule** `[필수]`
  (a) `main`에 직접 push 막고, PR + 승인 N명을 강제하는 GitHub 설정.
  (b) 4.1 직접 요구(직접 push 금지 / PR만 / 승인 1명).

### 2.3 충돌(Conflict)

- **머지 충돌이 생기는 이유** `[필수]`
  (a) 두 브랜치가 같은 파일의 같은(또는 인접) 부분을 다르게 바꾸면 Git이 자동 선택 불가 → 사람이 결정.
  (b) 4.7이 의도적 충돌 2회 + 비자명 충돌 1회를 요구.
- **충돌 마커 `<<<<<<<` / `=======` / `>>>>>>>`** `[필수]`
  (a) 위=현재 브랜치(HEAD), 아래=합치려는 브랜치 내용. 마커를 지우고 원하는 결과로 정리.
  (b) 과제 목표에 "마커가 무엇을 의미하는지 설명" 명시. `conflict-resolution.md`에 그대로 붙임.
- **비자명(non-trivial) 충돌** `[필수]`
  (a) 같은 hunk 충돌, 또는 한쪽이 파일 이동/이름변경·삭제 + 다른쪽 내용수정으로 생기는 까다로운 충돌.
  (b) 4.7이 "최소 1회는 비자명 충돌" 요구.

### 2.4 되돌리기 3형제 (과제 목표에 직접 명시)

- **`reset`** `[필수]` (a) 브랜치 포인터를 과거로 이동(로컬 히스토리 변경). (b) 4.8 `reset --soft HEAD~1`.
- **`revert`** `[필수]` (a) 되돌리는 "새 커밋"을 추가(히스토리 보존, 안전). (b) 4.8 "원격에 push된 커밋 취소"는 revert로.
- **`stash`** `[필수]` (a) 커밋하기 애매한 작업을 임시 보관 후 깨끗한 상태로 전환. (b) 4.8 `stash`/`stash pop`.
- **셋의 차이/선택 기준** `[필수]` (b) 과제 목표가 "셋의 차이와 언제 쓰는지 설명"을 직접 요구 → 6장 트러블슈팅 표 참고.

### 2.5 커밋 메시지 컨벤션

- **Conventional Commits 스타일(`feat:`, `fix:`, `docs:`, `refactor:`)** `[필수]`
  (a) 타입 접두어 + 무엇을/왜 바꿨는지 드러나는 제목.
  (b) 4.4가 컨벤션 문서화 + "의미없는 메시지(update/wip/fix 등) 금지" 판정.

### 2.6 보너스/품질 (선택)

- **`git rebase -i` (squash / reword)** `[선택]`
  (a) 개인 브랜치의 커밋들을 합치거나 메시지를 다시 쓰며 히스토리 정리.
  (b) 5장 보너스. **단, 공유 브랜치에서는 금지**(제약: 합의 없는 강제 푸시/리베이스 금지).
- **`.github/CODEOWNERS`** `[선택]`
  (a) 경로별 책임 리뷰어를 자동 지정하는 파일.
  (b) 5장 보너스(리뷰어 자동화).
- **PR/Issue 템플릿(`.github/`)** `[선택]`
  (a) PR/이슈 본문 기본 양식 자동 삽입.
  (b) 4.5 What/Why/How 양식을 매번 강제하기 좋음(품질 향상).

---

## 3. 명령어 레퍼런스

> 예시는 이 미션 기준(브랜치명 `feature/<name>-<topic>`, 문서 경로 `docs/...`)으로 작성. 복붙 후 이름만 바꾸면 됨.

### 3.1 초기 설정 & 저장소 시작 (미션 4.1)

| 명령어 | 용도 | 실제 예시 | 주의점 |
| --- | --- | --- | --- |
| `git config` | 커밋 작성자 정보 | `git config --global user.name "Hong"`<br>`git config --global user.email "you@ex.com"` | GitHub 계정 이메일과 맞춰야 기여도 집계됨(가정 #7) |
| `git clone` | 팀 저장소 받기 | `git clone https://github.com/<org>/<repo>.git` | 저장소는 GitHub에서 먼저 생성 |
| `git init` | (대안) 로컬에서 시작 | `git init` | 보통은 GitHub에서 만들고 clone이 편함 |
| `git remote -v` | 원격 주소 확인 | `git remote -v` | 잘못된 origin이면 push 실패 |

### 3.2 매일 쓰는 핵심 흐름 (미션 4.2 / 4.5)

| 명령어 | 용도 | 실제 예시 | 주의점 |
| --- | --- | --- | --- |
| `git status` | 현재 상태 확인 | `git status` | **모든 작업의 시작점.** 자주 칠 것 |
| `git switch -c` | feature 브랜치 생성+이동 | `git switch -c feature/hong-math-utils` | 최신 main에서 분기(아래 pull 먼저) |
| `git switch` | 브랜치 이동 | `git switch main` | 미커밋 변경 있으면 막힐 수 있음 → stash |
| `git add` | 스테이징 | `git add src/math_utils.py`<br>`git add .` | `.`은 전체. 의도 안 한 파일 주의 |
| `git commit` | 커밋 | `git commit -m "feat: add add()/sub() math utils"` | 메시지 컨벤션 준수(4.4) |
| `git push` | 원격에 올리기 | `git push -u origin feature/hong-math-utils` | 첫 push는 `-u`. main 직접 push는 Protection에 막힘(정상) |
| `git pull` | 원격 변경 받기 | `git switch main && git pull` | 작업 전 main 최신화 → 충돌 줄임 |
| `git fetch` | 받되 병합 안 함 | `git fetch origin` | 원격 상태만 갱신, 안전 |
| `git log --oneline --graph --all` | 히스토리 그래프(증빙) | `git log --oneline --graph --all` | **2장 제출 증빙.** `> history.txt`로 저장 가능 |
| `git branch -d` | 머지된 브랜치 삭제 | `git branch -d feature/hong-math-utils` | GitHub Flow: 머지 후 정리 |

### 3.3 충돌 해결 (미션 4.7)

| 명령어 | 용도 | 실제 예시 | 주의점 |
| --- | --- | --- | --- |
| `git merge` | 브랜치 합치기(충돌 유발/해결 연습) | `git merge main` | 충돌 시 마커가 파일에 삽입됨 |
| (편집기) | 마커 수정 | `<<<<<<<`/`=======`/`>>>>>>>` 지우고 정리 | 마커 한 줄도 남기면 코드 깨짐 |
| `git add` + `git commit` | 충돌 해결 마무리 | `git add . && git commit` | 머지 충돌 후 commit이 머지 완료 |
| `git merge --abort` | 머지 취소(원상복구) | `git merge --abort` | 충돌이 꼬였을 때 탈출구 |
| `git mv` | 파일 이름변경(비자명 충돌 재현용) | `git mv src/a.py src/b.py` | 4.7 "이동/이름변경 vs 내용수정" 시나리오 |

### 3.4 트러블슈팅 4종 (미션 4.8 — 전부 필수 수행)

| 명령어 | 용도 | 실제 예시 | 주의점 |
| --- | --- | --- | --- |
| `git commit --amend` | 최근 커밋 메시지/내용 수정 | `git commit --amend -m "fix: correct typo in README"` | **이미 push했다면 히스토리 변경** → 공유 브랜치에선 주의 |
| `git reset --soft HEAD~1` | 마지막 커밋만 취소, 변경은 스테이징에 유지 | `git reset --soft HEAD~1` | `--soft`=변경 유지 / `--hard`=**변경 삭제(위험)** |
| `git revert` | push된 커밋을 안전하게 되돌리는 새 커밋 | `git revert <commit_hash>` | 히스토리 보존 → 공유 브랜치에 안전 |
| `git stash` / `git stash pop` | 작업 임시 보관 / 복원 | `git stash`<br>`git stash pop` | `stash list`로 목록 확인. pop은 충돌날 수 있음 |
| `git reflog` | 잃어버린 커밋 찾기(구조용) | `git reflog` | reset 실수 복구의 안전망 |

### 3.5 보너스 (미션 5장 — 선택)

| 명령어 | 용도 | 실제 예시 | 주의점 |
| --- | --- | --- | --- |
| `git rebase -i` | 커밋 squash/reword 정리 | `git rebase -i HEAD~3` | **개인 feature 브랜치에서만.** 공유 브랜치 금지(제약) |
| `git push --force-with-lease` | 리베이스 후 안전한 강제 push | `git push --force-with-lease` | 그냥 `--force`보다 안전. 팀 합의 필수 |

### 3.6 GitHub CLI `gh` (선택 — 없으면 웹 UI로 동일하게 가능)

| 명령어 | 용도 | 실제 예시 | 주의점 |
| --- | --- | --- | --- |
| `gh auth login` | gh 로그인 | `gh auth login` | 설치 필요(가정 #6) |
| `gh issue create` | 이슈 생성 | `gh issue create --title "math utils 추가" --body "..."` | 웹 UI로도 가능 |
| `gh pr create` | PR 생성 | `gh pr create --base main --head feature/hong-math-utils --title "feat: math utils" --body "Closes #1 ..."` | `Closes #N` 꼭 본문에 |
| `gh pr review` | 리뷰 작성 | `gh pr review 3 --comment -b "src/x.py:10 — 경계값 처리는?"` | 실질 코멘트(4.6) |

> 터미널에서 로그인이 필요하면 프롬프트에 `! gh auth login` 처럼 `!`를 붙여 직접 실행하세요.

---

## 4. 학습·수행 순서 (로드맵)

> 의존관계 순서대로. 괄호는 관련 MISSION 절.

### Phase 0 — 개념 워밍업 (혼자, 1~2h)
1. 2.1~2.4 개념 읽기 → 특히 "브랜치=포인터", "reset/revert/stash 차이"를 말로 설명해보기(과제 목표).
2. 빈 연습용 폴더에서 `init → add → commit → branch → merge(충돌)`를 혼자 1회 굴려보기.

### Phase 1 — 팀/저장소 셋업 (팀, 0.5~1h) — 4.1
3. 팀 구성·역할 분담, 결과물 택1 결정(가정 #2,#4).
4. GitHub에 저장소 생성(옵션 A 권장) → 팀원 초대.
5. 기본 구조 푸시: `README.md` / `docs/` / `src/` / (선택)`team/`.
6. **`main` Branch Protection 설정**(직접 push 금지 / PR 머지 / 승인 1명). ← 이후 모든 작업이 PR로 강제됨.

### Phase 2 — 협업 규칙 합의 & 문서화 (팀, 1h) — 4.2 / 4.4 / 4.9
7. 브랜치 네이밍·커밋 컨벤션·PR 규칙·리뷰 규칙·충돌 대응 흐름 합의.
8. `docs/CONTRIBUTING.md` 작성(분담) + GitHub Flow 선택 이유 3줄.
> 이 문서 자체도 Issue→브랜치→PR→리뷰→머지로 만들어 **연습 1회차**로 삼는 걸 추천.

### Phase 3 — 결과물 작업 (각자, 2~4h) — 4.3 / 4.5 / 4.6 / 4.10
9. 각자 Issue 생성 → `feature/*` 브랜치 → 작업 → 커밋(컨벤션) → push → PR(`Closes #N`, What/Why/How).
10. 서로의 PR에 **실질 코멘트** 리뷰 → 작성자 반영(커밋/답글) → 승인 → 머지.
11. 팀원별 정족수 채우기: PR 2 / 리뷰 2 / 반영 1.

### Phase 4 — 충돌 실습 (팀, 1~2h) — 4.7
12. 의도적 충돌 2회(최소 1회 비자명) 만들고 해결 → `docs/conflict-resolution.md` 템플릿대로 기록.

### Phase 5 — 트러블슈팅 4종 (팀 분담, 1~2h) — 4.8
13. amend / reset --soft / revert / stash 각각 수행, 팀원별 1개 이상 참여 → `docs/troubleshooting-log.md`.

### Phase 6 — 마무리 & 제출 (팀, 0.5h) — 2장
14. `git log --oneline --graph --all` 캡처/저장.
15. `SUBMISSION.md` 작성(팀원별 PR/문서/증빙 링크).
16. (선택) 보너스: `rebase -i` 정리 문서화, `.github/CODEOWNERS`.
17. 5장 체크리스트로 DoD 전수 점검.

---

## 5. 실습/검증 체크리스트

> 각 단계가 "됐는지"를 스스로 확인하는 명령/기준.

**셋업(4.1)**
- [ ] `git remote -v` → 팀 저장소 origin 맞음
- [ ] GitHub Settings→Branches에 main 보호 규칙 보임 / `main`에 직접 `git push` 시도 → **거부되면 정상**
- [ ] `ls` → `README.md docs/ src/` 존재

**작업 흐름(4.2~4.5)**
- [ ] `git branch` → 브랜치명이 컨벤션(`feature/<name>-<topic>`) 따름
- [ ] PR 화면에 `Closes #N`이 연결돼 Issue가 자동 닫힘 표시
- [ ] `git log --oneline` → 커밋 메시지에 `feat:`/`fix:`/`docs:` 등 타입, 금지어(update/wip) 없음
- [ ] 각 팀원: 머지된 PR 2 / 작성한 리뷰 2 / 본인 PR 반영 1 (GitHub PR 목록·필터로 확인)

**리뷰 품질(4.6)**
- [ ] 각 PR에 라인 근거 실질 코멘트 ≥1, 작성자↔리뷰어 답글/수정 ≥1

**충돌(4.7)**
- [ ] `docs/conflict-resolution.md`에 충돌 기록 ≥2(그중 비자명 ≥1), 마커 캡처 포함
- [ ] 충돌 해결 커밋/PR 링크가 문서에 있음

**트러블슈팅(4.8)**
- [ ] `docs/troubleshooting-log.md`에 amend/reset/revert/stash **4종 전부** + 각 참여자 이름
- [ ] revert 기록에 "왜 reset 대신 revert인가"가 적혀 있음(과제 목표)

**결과물/제출(4.10 / 2장)**
- [ ] 결과물 택1 완성 + 팀원별 기여 커밋 ≥1 (`git log --author="이름"`로 확인)
- [ ] `git log --oneline --graph --all` 증빙 저장
- [ ] `SUBMISSION.md`에 팀원별 PR·문서·증빙 링크 모두

---

## 6. 자주 막히는 지점 (트러블슈팅)

| 증상 / 에러 | 원인 | 대처 |
| --- | --- | --- |
| `main`에 push 했더니 `protected branch ... rejected` | Branch Protection 정상 동작 | feature 브랜치 만들어 PR로. (의도된 동작) |
| PR에 "Review required" 떠서 머지 불가 | 승인 1명 미충족(4.1) | 팀원에게 리뷰·approve 요청 |
| `Closes #N` 썼는데 이슈가 안 닫힘 | 본문이 아닌 코멘트에 썼거나, 다른 저장소 이슈 | PR **본문**에 `Closes #N`, 같은 레포 번호인지 확인 |
| `merge` 후 코드가 깨짐 | 충돌 마커(`<<<<<<<` 등)를 안 지움 | 파일에서 마커 검색해 제거 후 `add`/`commit` |
| `git switch` 거부 — "local changes would be overwritten" | 미커밋 변경 존재 | `git stash` 후 이동, 돌아와 `git stash pop` |
| `reset --hard` 했더니 작업이 사라짐 | `--hard`는 변경 삭제 | `git reflog`로 직전 커밋 해시 찾아 `git reset --hard <hash>` 복구 |
| `pull` 시 "divergent branches" / merge vs rebase 물음 | 로컬·원격 갈라짐 | 협업 중엔 보통 merge. `git pull --no-rebase`(팀 합의대로) |
| `push --force`로 팀원 커밋이 날아감 | 공유 브랜치 강제 푸시(제약 위반) | 금지. 굳이 필요하면 `--force-with-lease` + 팀 합의 |
| 커밋 작성자가 다른 사람으로 표시 | `user.email`이 GitHub 계정과 불일치 | `git config user.email` 수정(기여도 집계 영향) |
| `amend` 후 push 거부 | 이미 push된 커밋의 히스토리 변경 | 개인 브랜치면 `--force-with-lease`, 공유 브랜치면 amend 대신 새 커밋 |

---

## 7. 참고 자료 (공식 문서 위주)

- Git 공식 책(한국어) — https://git-scm.com/book/ko/v2 (브랜치·머지·reset/revert/stash 챕터)
- `git` 각 명령 레퍼런스 — https://git-scm.com/docs (예: `git-reset`, `git-revert`, `git-stash`)
- GitHub Flow 설명 — https://docs.github.com/ko/get-started/using-github/github-flow
- Pull Request 만들기 — https://docs.github.com/ko/pull-requests
- 키워드로 이슈 닫기(`Closes #`) — https://docs.github.com/ko/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue
- Branch Protection Rule — https://docs.github.com/ko/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches
- CODEOWNERS — https://docs.github.com/ko/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners
- Conventional Commits — https://www.conventionalcommits.org/ko/v1.0.0/
- GitHub CLI(`gh`) — https://cli.github.com/manual/
- (충돌 해결 가이드) https://docs.github.com/ko/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts

---

> **다음 행동 제안:** Phase 0 개념 워밍업 → Phase 1에서 저장소 생성·Branch Protection 먼저. 그 시점에 가정 #2/#3/#4(인원·저장소옵션·결과물)를 팀과 확정하세요.
