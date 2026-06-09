# Git과 GitHub의 차이점

## 1. Git이란?

Git은 **버전 관리 도구**이다.

파일의 변경 이력을 기록하고, 이전 상태로 되돌리거나 여러 브랜치로 나누어 작업할 수 있게 해준다.

Git은 내 컴퓨터에 설치해서 사용하는 프로그램이며, 인터넷이 없어도 로컬에서 사용할 수 있다.

예를 들어 다음 작업들은 Git이 담당한다.

* 파일 변경사항 추적
* commit 생성
* branch 생성
* merge
* reset
* revert
* stash
* 과거 commit 확인

즉, Git은 프로젝트의 변경 이력을 관리하는 도구이다.

---

## 2. GitHub란?

GitHub는 **Git 저장소를 원격으로 저장하고 협업할 수 있게 해주는 웹 서비스**이다.

로컬 컴퓨터에서 Git으로 commit한 내용을 GitHub에 push하면, GitHub 서버에 저장소가 업로드된다.

GitHub에서는 다음과 같은 협업 기능을 사용할 수 있다.

* 원격 저장소 생성
* push / pull
* Pull Request
* Issue
* Code Review
* Branch Protection Rule
* Organization
* Collaborator 관리
* GitHub Actions

즉, GitHub는 Git 저장소를 온라인에서 관리하고 팀원들과 협업할 수 있게 해주는 서비스이다.

---

## 3. Git과 GitHub의 핵심 차이

| 구분        | Git                                     | GitHub                            |
| --------- | --------------------------------------- | --------------------------------- |
| 종류        | 버전 관리 도구                                | Git 저장소 호스팅 서비스                   |
| 사용 위치     | 로컬 컴퓨터                                  | 웹 / 원격 서버                         |
| 인터넷 필요 여부 | 기본 기능은 인터넷 없이 가능                        | 대부분 인터넷 필요                        |
| 주요 역할     | 변경 이력 관리                                | 저장소 공유 및 협업                       |
| 관리 대상     | commit, branch, merge 등                 | repository, PR, issue, review 등   |
| 예시 명령어/기능 | `git commit`, `git branch`, `git merge` | Pull Request, Issue, Organization |

---

## 4. Git과 GitHub의 관계

Git과 GitHub는 서로 다른 개념이지만 함께 자주 사용된다.

흐름은 다음과 같다.

```text
내 컴퓨터에서 Git으로 작업
        ↓
git add
        ↓
git commit
        ↓
git push
        ↓
GitHub 원격 저장소에 업로드
```

즉, Git은 로컬에서 변경 이력을 관리하고, GitHub는 그 Git 저장소를 온라인에 보관하고 공유한다.

---

## 5. 로컬 저장소와 원격 저장소

Git을 사용하면 내 컴퓨터에 로컬 저장소가 생긴다.

```text
내 컴퓨터
└── project/
    ├── README.md
    ├── src/
    └── .git/
```

여기서 `.git/` 디렉토리는 Git이 commit, branch, log 등을 저장하는 공간이다.

GitHub에 저장소를 만들고 연결하면 원격 저장소가 생긴다.

```text
GitHub
└── PBK98/project
```

로컬 저장소와 원격 저장소는 `push`, `pull`, `clone` 같은 명령어로 동기화한다.

---

## 6. commit과 push의 차이

### commit

`commit`은 내 컴퓨터의 로컬 Git 저장소에 변경사항을 저장하는 작업이다.

```bash
git add README.md
git commit -m "[Docs] README 수정"
```

commit을 하면 변경 이력이 내 컴퓨터의 `.git` 디렉토리에 저장된다.

---

### push

`push`는 로컬에 있는 commit을 GitHub 원격 저장소로 업로드하는 작업이다.

```bash
git push origin main
```

또는 작업 브랜치를 push할 수 있다.

```bash
git push -u origin feature/park
```

즉, commit은 로컬 저장이고, push는 GitHub로 업로드하는 작업이다.

---

## 7. clone, pull, push의 의미

| 명령어         | 의미                              |
| ----------- | ------------------------------- |
| `git clone` | GitHub 원격 저장소를 내 컴퓨터로 복사        |
| `git pull`  | GitHub의 최신 내용을 내 로컬 저장소로 가져오기   |
| `git push`  | 내 로컬 commit을 GitHub 원격 저장소로 업로드 |

예시:

```bash
git clone https://github.com/조직명/저장소명.git
```

```bash
git pull origin main
```

```bash
git push origin feature/park
```

---

## 8. Git만 사용하는 경우

GitHub 없이도 Git은 사용할 수 있다.

예를 들어 내 컴퓨터에서만 프로젝트를 관리할 경우 다음 작업이 가능하다.

```bash
git init
git add .
git commit -m "Initial commit"
git log
```

하지만 GitHub를 사용하지 않으면 다른 사람과 원격으로 쉽게 공유하거나 Pull Request 기반 협업을 하기는 어렵다.

---

## 9. GitHub를 사용하는 이유

GitHub를 사용하면 Git 저장소를 온라인에 올려 팀원과 협업할 수 있다.

GitHub의 장점은 다음과 같다.

* 저장소를 온라인에 백업할 수 있다.
* 팀원과 코드를 공유할 수 있다.
* Pull Request로 변경사항을 리뷰할 수 있다.
* Issue로 작업을 관리할 수 있다.
* Branch Protection Rule로 `main` 브랜치를 보호할 수 있다.
* Organization으로 팀 단위 저장소를 관리할 수 있다.

---

## 10. 예시로 이해하기

README 파일을 수정한다고 가정한다.

### 1. Git으로 로컬에서 변경사항 저장

```bash
git add README.md
git commit -m "[Docs] README 수정"
```

이 단계에서는 변경사항이 아직 내 컴퓨터에만 있다.

### 2. GitHub로 업로드

```bash
git push origin docs/update-readme
```

이제 GitHub 원격 저장소에도 commit이 올라간다.

### 3. GitHub에서 PR 생성

```text
base: main
compare: docs/update-readme
```

팀원이 Pull Request를 확인하고 approve하면 `main`에 병합할 수 있다.

---

## 11. 한 문장으로 정리

```text
Git은 버전 관리 도구이고, GitHub는 Git 저장소를 온라인에서 저장하고 협업할 수 있게 해주는 서비스이다.
```

더 쉽게 말하면 다음과 같다.

```text
Git = 내 컴퓨터에서 변경 이력을 관리하는 도구
GitHub = 그 Git 저장소를 인터넷에 올려 팀원과 함께 쓰는 공간
```

---

## 12. 최종 요약

| 질문                       | 답                                 |
| ------------------------ | --------------------------------- |
| Git은 무엇인가?               | 파일 변경 이력을 관리하는 버전 관리 도구           |
| GitHub는 무엇인가?            | Git 저장소를 원격으로 저장하고 협업하는 웹 서비스     |
| Git만 있어도 사용할 수 있는가?      | 가능하다                              |
| GitHub만 있으면 Git 없이 가능한가? | 제한적으로 가능하지만, 일반적으로 Git과 함께 사용한다   |
| commit은 어디에 저장되는가?       | 로컬 Git 저장소                        |
| push는 무엇인가?              | 로컬 commit을 GitHub 원격 저장소에 업로드하는 것 |
| PR은 어디서 사용하는가?           | GitHub에서 사용하는 협업 기능               |

---
