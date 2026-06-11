# Codyssey_2-2 Git Team Workflow

## 목차

1. [프로젝트 소개](#1-프로젝트-소개)
2. [팀 구성](#2-팀-구성)
3. [저장소 구조](#3-저장소-구조)
4. [Git과 GitHub의 차이](#4-git과-github의-차이)
5. [Git Workflow](#5-git-workflow)
6. [Branch 전략](#6-branch-전략)
7. [Issue 작성 규칙](#7-issue-작성-규칙)
8. [Commit Message Convention](#8-commit-message-convention)
9. [Pull Request 규칙](#9-pull-request-규칙)
10. [Code Review 규칙](#10-code-review-규칙)
11. [Branch Protection Rule](#11-branch-protection-rule)
12. [충돌 해결 실습](#12-충돌-해결-실습)
13. [비자명 충돌 해결 실습](#13-비자명-충돌-해결-실습)
14. [Reset / Revert / Stash 정리](#14-reset--revert--stash-정리)
15. [협업 중 문제 상황과 해결 방법](#15-협업-중-문제-상황과-해결-방법)
16. [Organization과 Collaborator 비교](#16-organization과-collaborator-비교)
17. [실습 기록 문서](#17-실습-기록-문서)
18. [최종 정리](#18-최종-정리)

---

## 1. 프로젝트 소개

이 프로젝트는 Git과 GitHub를 활용한 팀 협업 방식을 실습하기 위한 과제이다.

본 프로젝트에서는 GitHub Flow를 기반으로 Issue, Branch, Commit, Pull Request, Code Review, Merge 과정을 실습한다.

---

## 2. 팀 구성

| 이름   | 역할 | GitHub ID |
| ---- | -- | --------- |
| 박범규  | 팀장 | PBK98 |
| 손보람 | 팀원 | sourcreamsource |
| 이초롱 | 팀원 | 0802222 |
| 임종한 | 팀원 |LimJongHan|

---

## 3. 저장소 구조

```text
Codyssey_2-2/
├── README.md
├── SUBMISSON.md
├── docs/
├── practice-notes/
└── .gitignore
```

---

## 4. Git과 GitHub의 차이

Git은 로컬 환경에서 변경 이력을 관리하는 버전 관리 도구이다.

GitHub는 Git 저장소를 원격으로 저장하고, 팀원들과 협업할 수 있게 해주는 웹 서비스이다.

---

## 5. Git Workflow

본 프로젝트는 GitHub Flow 방식을 사용한다.

```text
Issue 생성
↓
main 브랜치에서 작업 브랜치 생성
↓
파일 수정
↓
commit
↓
push
↓
Pull Request 생성
↓
review / approve
↓
main 브랜치에 merge
```

---

## 6. Branch 전략

| 브랜치          | 용도                  |
| ------------ | ------------------- |
| `main`       | 최종 결과물이 반영되는 안정 브랜치 |
| `feature/*`  | 기능 추가 및 실습 작업 브랜치   |
| `docs/*`     | 문서 작성 및 수정 브랜치      |
| `fix/*`      | 오류 수정 브랜치           |
| `refactor/*` | 코드 구조 개선 브랜치        |
| `chore/*`    | 설정 및 기타 작업 브랜치      |

---

## 7. Issue 작성 규칙

작업을 시작하기 전 Issue를 생성한다.

Issue에는 작업 목적, 작업 내용, 담당자, 완료 조건을 작성한다.

---

## 8. Commit Message Convention

커밋 메시지는 작업 유형을 알 수 있도록 작성한다.

| 태그           | 의미         |
| ------------ | ---------- |
| `[Feat]`     | 기능 추가      |
| `[Fix]`      | 오류 수정      |
| `[Docs]`     | 문서 수정      |
| `[Refactor]` | 코드 구조 개선   |
| `[Chore]`    | 설정 및 기타 작업 |

예시:

```bash
git commit -m "[Docs] Add conflict resolution record"
git commit -m "[Fix] Resolve merge conflict"
git commit -m "[Feat] Add calculator function"
```

---

## 9. Pull Request 규칙

모든 작업은 Pull Request를 통해 `main` 브랜치에 반영한다.

PR에는 작업 내용, 변경 파일, 관련 Issue 번호를 작성한다.

예시:

```md
## 작업 내용

- Git Workflow 문서 작성
- Branch 전략 정리

## 관련 Issue

Closes #1
```

---

## 10. Code Review 규칙

PR은 최소 1명 이상의 팀원에게 review를 받아야 한다.

리뷰어는 변경사항을 확인하고 문제가 없으면 approve한다.

---

## 11. Branch Protection Rule

`main` 브랜치는 보호 브랜치로 설정한다.

적용 규칙은 다음과 같다.

* `main` 직접 push 금지
* Pull Request를 통한 merge 필수
* 최소 1명 이상의 approve 필요
* force push 금지
* branch 삭제 금지

---

## 12. 충돌 해결 실습

팀 전체 기준 최소 2회 이상의 충돌 해결 실습을 진행한다.

일반 충돌은 같은 파일의 같은 hunk를 서로 다르게 수정하여 발생시킨다.

---

## 13. 비자명 충돌 해결 실습

비자명 충돌은 파일 이름 변경/이동/삭제와 내용 수정이 동시에 발생하는 상황을 통해 실습한다.

예시:

```text
팀원 A: 파일 내용 수정
팀원 B: 파일 내용 수정 + 파일 이름 변경
```

충돌 해결 과정은 `docs/conflict-resolution.md`에 기록한다.

---

## 14. Reset / Revert / Stash 정리

Git에서 작업을 되돌리거나 임시 저장할 때 사용하는 명령어를 정리한다.

| 명령어          | 용도                          |
| ------------ | --------------------------- |
| `git reset`  | commit 이력을 이전 상태로 되돌림       |
| `git revert` | 기존 commit을 취소하는 새 commit 생성 |
| `git stash`  | 현재 작업 내용을 임시 저장             |

---

## 15. 협업 중 문제 상황과 해결 방법

협업 중 발생할 수 있는 문제 상황과 해결 방법을 정리한다.

예시:

* 의미 없는 commit message가 push된 경우
* conflict가 발생한 경우
* stash apply 중 충돌이 발생한 경우
* PR 방향을 잘못 설정한 경우
* main이 최신 상태가 아닌 경우

---

## 16. Organization과 Collaborator 비교

GitHub Organization과 Collaborator 방식의 차이를 정리한다.

| 구분         | Organization  | Collaborator     |
| ---------- | ------------- | ---------------- |
| 저장소 소유     | 조직            | 개인               |
| 권한 관리      | Team 단위 관리 가능 | 개인별 초대           |
| 팀 프로젝트 적합성 | 높음            | 비교적 단순한 프로젝트에 적합 |

---

## 17. 실습 기록 문서

자세한 실습 기록은 다음 문서에 정리한다.

* `docs/conflict-resolution.md`
* `docs/git-workflow.md`
* `docs/git-vs-github.md`
* `docs/reset-revert-stash.md`
* `practice-notes/`

---

## 18. 최종 정리

본 프로젝트를 통해 Git과 GitHub의 차이, GitHub Flow 기반 협업 방식, Branch 전략, Pull Request, Code Review, 충돌 해결 과정을 실습하였다.

또한 Branch Protection Rule을 적용하여 `main` 브랜치를 보호하고, 팀 단위 협업에서 안전하게 변경사항을 관리하는 방법을 익혔다.
