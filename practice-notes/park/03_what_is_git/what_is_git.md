# GitHub와 Git 저장소의 동작 방식

## 1. GitHub는 무엇인가?

GitHub는 Git 저장소를 원격으로 보관하고 협업할 수 있게 해주는 서비스이다.

Google Drive가 일반 파일을 클라우드에 저장하는 서비스라면, GitHub는 Git 저장소를 클라우드에 저장하는 서비스라고 볼 수 있다.

다만 GitHub는 단순히 파일만 저장하는 것이 아니라, Git의 commit 기록, branch, tag, 변경 이력, Pull Request, Issue, Review 기록 등을 함께 관리한다.

---

## 2. Google Drive와 GitHub의 공통점

Google Drive와 GitHub는 모두 인터넷에 데이터를 저장하고 다른 사람과 공유할 수 있다는 공통점이 있다.

| 구분           | 설명                          |
| ------------ | --------------------------- |
| Google Drive | 일반 파일을 클라우드에 저장한다.          |
| GitHub       | Git 저장소를 원격 서버에 저장한다.       |
| 공통점          | 인터넷을 통해 데이터를 저장하고 공유할 수 있다. |

단순하게 보면 GitHub는 Git 저장소를 위한 클라우드 저장 공간이라고 이해할 수 있다.

---

## 3. Google Drive와 GitHub의 차이점

Google Drive는 주로 현재 파일을 저장하고 공유하는 데 초점이 있다.

반면 GitHub는 파일뿐만 아니라 변경 이력 전체를 저장하고 관리한다.

| 구분    | Google Drive   | GitHub                        |
| ----- | -------------- | ----------------------------- |
| 저장 대상 | 일반 파일          | Git 저장소                       |
| 관리 중심 | 현재 파일          | commit 이력과 변경 과정              |
| 버전 관리 | 제한적            | Git 기반 버전 관리                  |
| 협업 방식 | 파일 공유 중심       | branch, commit, PR, review 중심 |
| 사용 목적 | 문서, 이미지, 자료 저장 | 코드, 문서, 프로젝트 이력 관리            |

---

## 4. GitHub에 저장되는 정보

GitHub에는 단순히 파일만 저장되는 것이 아니다.

GitHub 저장소에는 다음과 같은 정보가 함께 저장된다.

* 파일 내용
* commit 기록
* branch 포인터
* tag
* commit 메시지
* 작성자 정보
* 파일 변경 이력
* Pull Request 기록
* Issue 기록
* Review 기록

즉, GitHub는 프로젝트의 현재 상태뿐만 아니라, 프로젝트가 어떻게 변경되어 왔는지도 함께 관리한다.

---

## 5. Git 객체란?

Git은 파일과 commit 정보를 내부적으로 객체 형태로 저장한다.

대표적인 Git 객체는 다음과 같다.

| Git 객체 | 의미                       |
| ------ | ------------------------ |
| commit | 특정 시점의 저장 기록             |
| tree   | 해당 commit 시점의 폴더 및 파일 구조 |
| blob   | 실제 파일 내용                 |

구조를 단순화하면 다음과 같다.

```text
commit 객체
└── tree 객체
    └── blob 객체
```

예를 들어 `README.md`를 commit하면 Git은 단순히 `README.md` 파일만 저장하는 것이 아니라, 해당 파일 내용과 폴더 구조, commit 정보를 Git 객체로 저장한다.

---

## 6. Git 객체와 해시값

Git은 commit, tree, blob 같은 객체를 해시값으로 구분한다.

commit을 만들면 다음과 같은 고유한 ID가 생성된다.

```text
a3c91f2b7e12aa9f...
```

이 값을 보통 다음과 같이 부른다.

* commit hash
* commit ID
* SHA 값

예를 들어 다음 명령어를 실행하면 commit hash를 확인할 수 있다.

```bash
git log --oneline
```

예시 출력:

```text
a3c91f2 [Docs] README 수정
b7e12aa [Feat] 계산기 코드 추가
c812e1d Initial commit
```

여기서 `a3c91f2`는 해당 commit을 가리키는 짧은 해시값이다.

---

## 7. 로컬 Git 저장소와 GitHub 원격 저장소

로컬에서 Git 저장소를 만들거나 clone하면 프로젝트 폴더 안에 `.git` 디렉토리가 생긴다.

```text
project/
├── README.md
├── calculator.py
└── .git/
```

여기서 `.git/` 디렉토리는 로컬 Git 저장소이다.

| 구분            | 의미                                 |
| ------------- | ---------------------------------- |
| 로컬 작업 파일      | 실제로 사용자가 보고 수정하는 파일                |
| `.git/` 디렉토리  | commit, branch, log 등 Git 내부 정보 저장 |
| GitHub 원격 저장소 | 로컬 Git 저장소를 인터넷에 저장한 원격 저장소        |

---

## 8. commit과 push의 차이

### commit

`commit`은 로컬 Git 저장소에 변경사항을 저장하는 작업이다.

```bash
git add README.md
git commit -m "[Docs] README 수정"
```

이 명령을 실행하면 내 컴퓨터의 `.git` 디렉토리 안에 새로운 commit 객체가 생성된다.

즉, commit은 먼저 로컬에서 발생한다.

```text
내 컴퓨터의 .git 저장소에 commit 생성
```

---

### push

`push`는 로컬 Git 저장소에 있는 commit을 GitHub 원격 저장소로 업로드하는 작업이다.

```bash
git push origin feature/park
```

push를 하면 로컬에 있던 commit 객체들이 GitHub 원격 저장소에도 저장된다.

```text
내 컴퓨터 .git 저장소
        ↓ git push
GitHub 원격 저장소
```

즉, push는 로컬 commit을 GitHub와 동기화하는 작업이다.

---

## 9. 브랜치 포인터의 의미

Git에서 브랜치는 특정 commit을 가리키는 포인터이다.

예를 들어 다음과 같은 commit 이력이 있다고 가정한다.

```text
A --- B --- C
          ↑
         main
```

여기서 `main` 브랜치는 `C` commit을 가리킨다.

```text
main → C commit
```

즉, 브랜치 포인터는 파일 자체를 가리키는 것이 아니라 commit 객체를 가리킨다.

---

## 10. GitHub의 브랜치도 포인터이다

로컬 브랜치와 마찬가지로 GitHub의 브랜치도 특정 commit을 가리키는 포인터이다.

예를 들어 다음과 같은 브랜치가 있다고 하자.

```text
main → c812e1d commit
feature/park → a3c91f2 commit
```

이 상태에서 로컬에서 다음 명령어를 실행하면:

```bash
git push origin feature/park
```

GitHub 원격 저장소에도 `feature/park` 브랜치가 생성되거나 갱신된다.

즉, GitHub는 브랜치 이름과 해당 브랜치가 가리키는 commit hash를 함께 관리한다.

---

## 11. GitHub는 파일을 어떻게 저장하는가?

GitHub는 단순히 파일을 폴더 형태로만 저장하는 것이 아니라, Git 객체와 commit 이력을 저장한다.

정리하면 다음과 같다.

```text
branch
  ↓
commit
  ↓
tree
  ↓
blob
```

각 요소의 의미는 다음과 같다.

| 요소     | 의미                  |
| ------ | ------------------- |
| branch | 특정 commit을 가리키는 포인터 |
| commit | 특정 시점의 저장 기록        |
| tree   | 해당 시점의 폴더/파일 구조     |
| blob   | 실제 파일 내용            |

따라서 GitHub는 commit한 파일의 현재 내용뿐만 아니라, 과거 변경 이력까지 저장하고 관리한다.

---

## 12. GitHub와 Google Drive의 차이를 한 문장으로 정리

Google Drive는 일반 파일을 저장하고 공유하는 클라우드 서비스이다.

GitHub는 Git 객체, commit 이력, branch, PR, Issue를 저장하고 관리하는 Git 저장소 호스팅 서비스이다.

즉, GitHub는 단순 파일 저장소가 아니라 버전 관리와 협업을 위한 원격 Git 저장소이다.

---

## 13. 최종 정리

GitHub에 대한 이해를 정리하면 다음과 같다.

* GitHub는 계정 또는 Organization 아래에 저장소를 제공한다.
* 로컬에서 commit한 내용은 먼저 내 컴퓨터의 `.git` 디렉토리에 저장된다.
* `git push`를 하면 로컬 commit 객체들이 GitHub 원격 저장소로 업로드된다.
* GitHub는 파일만 저장하는 것이 아니라 commit, branch, tag, 변경 이력 등을 함께 저장한다.
* Git 객체는 해시값으로 구분된다.
* 브랜치는 파일이 아니라 commit을 가리키는 포인터이다.
* GitHub의 브랜치도 특정 commit hash를 가리킨다.

핵심 문장:

```text
GitHub는 Google Drive처럼 저장 공간을 제공하지만, 단순 파일 저장소가 아니라 Git commit, branch, tag, 변경 이력을 저장하는 원격 Git 저장소 서비스이다.
```
