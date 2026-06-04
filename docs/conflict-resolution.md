# Conflict Resolution

이 문서는 팀 협업 중 의도적으로 만들고 해결한 Git 충돌 기록을 정리한다. 충돌이 발생한 이유, 해결 절차, 최종 판단 근거를 남겨 이후 같은 문제가 생겼을 때 재현 가능한 참고 자료로 사용한다.

## 충돌 해결 원칙

- 충돌이 발생한 PR 작성자가 1차 해결 담당자가 된다.
- 같은 파일을 수정한 팀원은 각자 변경 의도를 설명한다.
- 충돌 마커(`<<<<<<<`, `=======`, `>>>>>>>`)는 해결 후 반드시 제거한다.
- 한쪽 변경을 무조건 버리지 않고, 두 변경의 목적을 비교해 필요한 내용만 남긴다.
- 해결 후 `git status`, 파일 내용 확인, 문서 링크 확인 등 가능한 검증을 수행한다.
- 해결 과정은 PR 댓글과 이 문서에 함께 남긴다.

## 충돌 마커 의미

```txt
<<<<<<< HEAD
현재 브랜치의 변경 내용
=======
병합하려는 브랜치의 변경 내용
>>>>>>> feature/example
```

- `<<<<<<< HEAD`: 현재 체크아웃된 브랜치의 변경 시작점
- `=======`: 두 브랜치 변경 내용의 구분선
- `>>>>>>> 브랜치명`: 병합 대상 브랜치의 변경 끝점

## 실습 기록 요약

| 번호 | 충돌 유형 | 관련 파일 | 담당자 | 결과 |
| --- | --- | --- | --- | --- |
| 1 | 같은 파일의 인접 라인 수정 | `README.md` | Park, Lim | 양쪽 변경 의도를 합쳐 해결 |
| 2 | 파일 이동/이름 변경과 내용 수정 | `practice-notes/team/test.html` | Lee, Son | 새 위치 기준으로 내용 보존 |

## 1. 같은 파일 같은 영역 수정 충돌

### 참여자

- 작성자: Park
- 리뷰어/공동 수정자: Lim

### 관련 브랜치

- 기준 브랜치: `main`
- 작업 브랜치 A: `feature/park-readme-index`
- 작업 브랜치 B: `feature/lim-readme-index`

### 관련 파일

- `README.md`

### 상황

두 팀원이 `README.md`의 같은 위치에 학습 노트 목차를 추가했다. 한쪽은 팀원별 practice note 링크를 추가했고, 다른 한쪽은 제출 문서 링크를 추가했다. 두 변경 모두 필요했지만 같은 hunk에 들어가면서 merge 시 충돌이 발생했다.

### 충돌 원인

- 두 브랜치가 같은 파일의 같은 영역을 서로 다르게 수정했다.
- Git은 두 변경 중 어떤 순서와 내용을 최종본으로 선택해야 하는지 자동 판단할 수 없었다.

### 해결 전 예시

```md
<<<<<<< HEAD
## Practice Notes
- [Park](practice-notes/park/park.md)
- [Lim](practice-notes/lim/lim.md)
=======
## Submission Links
- [Contributing Guide](docs/CONTRIBUTING.md)
- [Conflict Resolution](docs/conflict-resolution.md)
>>>>>>> feature/lim-readme-index
```

### 해결 절차

1. `git status`로 충돌 파일이 `README.md`임을 확인했다.
2. 두 브랜치의 변경 목적을 비교했다.
3. 학습 노트 목차와 제출 문서 링크가 모두 필요하다고 판단했다.
4. 충돌 마커를 제거했다.
5. 두 섹션을 각각 분리해 최종 README 구조에 반영했다.
6. 링크 경로가 실제 파일 경로와 맞는지 확인했다.
7. `git add README.md` 후 충돌 해결 커밋을 생성했다.

### 해결 후 최종 형태

```md
## Practice Notes
- [Park](practice-notes/park/park.md)
- [Lim](practice-notes/lim/lim.md)

## Submission Links
- [Contributing Guide](docs/CONTRIBUTING.md)
- [Conflict Resolution](docs/conflict-resolution.md)
```

### 사용한 명령어

```bash
git switch feature/park-readme-index
git merge main
git status
git add README.md
git commit -m "fix: resolve readme index conflict"
```

### 결과

두 팀원의 변경사항을 모두 보존했고, README에서 학습 노트와 제출 문서 링크를 모두 확인할 수 있게 되었다.

### 배운 점

- 같은 파일을 수정할 때는 작업 전 Issue나 PR에서 수정 위치를 공유하면 충돌 가능성을 줄일 수 있다.
- 충돌 해결은 한쪽을 선택하는 작업이 아니라 변경 의도를 합치는 작업이다.

## 2. 파일 이동/이름 변경과 내용 수정 충돌

### 참여자

- 작성자: Lee
- 리뷰어/공동 수정자: Son

### 관련 브랜치

- 기준 브랜치: `main`
- 작업 브랜치 A: `feature/lee-team-page`
- 작업 브랜치 B: `feature/son-team-page-update`

### 관련 파일

- 기존 파일: `practice-notes/team/test.html`
- 이동 후 파일: `practice-notes/team/index.html`

### 상황

Lee는 팀 소개 HTML 파일 이름을 `test.html`에서 `index.html`로 변경했다. 동시에 Son은 기존 `test.html` 파일의 본문 내용을 수정했다. 한쪽은 파일 이름을 바꾸고 다른 한쪽은 기존 파일 내용을 바꿨기 때문에 병합 과정에서 비자명 충돌이 발생했다.

### 충돌 원인

- 한 브랜치는 파일을 rename했다.
- 다른 브랜치는 rename 전 파일의 내용을 수정했다.
- Git이 수정된 내용을 새 파일명에 자동으로 반영해야 하는지 판단하기 어려운 상황이었다.

### 해결 방향

최종 파일명은 `index.html`로 정하고, Son이 수정한 본문 내용은 새 파일에 반영하기로 했다. `test.html`은 임시 이름이므로 최종 결과물에는 남기지 않는다.

### 해결 절차

1. `git status`로 rename/delete 또는 both modified 상태를 확인했다.
2. `practice-notes/team/test.html`과 `practice-notes/team/index.html`의 내용을 비교했다.
3. 최종 파일명은 `index.html`로 유지하기로 팀원끼리 합의했다.
4. Son의 본문 수정 내용을 `index.html`에 반영했다.
5. 더 이상 필요 없는 `test.html`은 추적 대상에서 제거했다.
6. 브라우저 또는 파일 열람으로 HTML 내용이 깨지지 않는지 확인했다.
7. `git add practice-notes/team/index.html` 후 충돌 해결 커밋을 생성했다.

### 사용한 명령어

```bash
git switch feature/lee-team-page
git merge main
git status
git diff
git add practice-notes/team/index.html
git commit -m "fix: resolve team page rename conflict"
```

### 결과

팀 소개 페이지의 최종 파일명을 `index.html`로 정리했고, 기존 파일에 작성된 본문 변경사항도 잃지 않고 보존했다.

### 배운 점

- 파일 이름 변경과 내용 수정은 일반적인 같은 줄 충돌보다 원인 파악이 어렵다.
- rename 작업을 할 때는 PR 설명에 파일 이동 이유를 명확히 적어야 리뷰어가 변경 의도를 이해하기 쉽다.
- 파일 이동 PR은 가능하면 내용 수정과 분리하는 편이 충돌 가능성을 줄인다.

## 충돌 해결 체크리스트

충돌이 발생하면 아래 항목을 확인한다.

- [ ] 어떤 브랜치끼리 충돌이 났는가?
- [ ] 어떤 파일에서 충돌이 났는가?
- [ ] 충돌 마커를 모두 제거했는가?
- [ ] 양쪽 변경 의도를 확인했는가?
- [ ] 필요한 변경사항을 모두 보존했는가?
- [ ] 파일 경로와 링크가 실제 구조와 맞는가?
- [ ] `git status`에서 unmerged 상태가 사라졌는가?
- [ ] PR 댓글 또는 문서에 해결 과정을 남겼는가?

## 충돌 예방 규칙

- 같은 파일을 여러 명이 수정할 때는 Issue 댓글이나 PR 설명에 수정 범위를 먼저 공유한다.
- 큰 문서 하나를 여러 명이 동시에 수정해야 하면 섹션별 담당자를 정한다.
- 파일 이동, 파일명 변경, 대량 포맷팅은 별도 PR로 분리한다.
- 작업 시작 전 `main`의 최신 변경사항을 반영한다.
- 충돌 해결 커밋 메시지는 `fix: resolve <대상> conflict` 형식으로 작성한다.
