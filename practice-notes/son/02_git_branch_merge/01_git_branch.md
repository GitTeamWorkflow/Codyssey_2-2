# 🟩 Git Branch 학습 노트  

## 🟢 전체 목차  
- ✅ 01_git_branch.md: 브랜치가 무엇인지, 왜 쓰는지, 어떻게 만들고 이동하는지  
- 02_git_merge.md: 브랜치를 main에 합치는 방법과 병합 종류  
- 03_git_conflict.md: 충돌이 왜 생기는지와 해결 방법  



<br><br>

## 🟢 1. Branch 뜻  

### 🟡 Branch는 작업 공간을 나누는 이름표다  
- Branch는 한국어로 `가지`라는 뜻이다.  
- Git에서 branch는 커밋을 가리키는 포인터다.  
- 쉽게 말하면, `main`이라는 기본 길에서 바로 작업하지 않고 새 길을 하나 만들어서 안전하게 작업하는 방법이다.  

### 🟡 초등학생 버전 설명  
- 공책 원본이 `main`이다.  
- 연습장 복사본이 `feature/son-branch-note` 같은 branch다.  
- 복사본에서 마음껏 고친 뒤, 괜찮으면 원본 공책에 붙인다.  
- 이렇게 하면 원본 공책을 망가뜨리지 않고 실험할 수 있다.  



<br><br>

## 🟢 2. Branch를 쓰는 이유  

| 이유 | 설명 |  
| --- | --- |
| main 보호 | `main`은 항상 제출 가능한 상태로 유지해야 한다. |  
| 작업 분리 | 팀원마다 자기 작업을 따로 진행할 수 있다. |  
| 실수 방지 | 잘못 수정해도 내 branch 안에서 먼저 고칠 수 있다. |  
| Pull Request 가능 | GitHub에서 내 branch를 main에 합치기 전에 리뷰받을 수 있다. |  



<br><br>

## 🟢 3. 과제에서 쓰는 Branch 규칙  

### 🟡 GitHub Flow 흐름  
```txt
main  
↓  
feature/작업이름  
↓  
commit  
↓  
push  
↓  
Pull Request  
↓  
review  
↓  
merge  
↓  
main  
```

### 🟡 추천 branch 이름  
```bash
feature/son-git-branch-note  
feature/son-git-merge-note  
docs/son-pr-template  
fix/son-conflict-record  
```

### 🟡 이름 규칙 해설  
| 이름 | 의미 |  
| --- | --- |
| `feature` | 새로운 기능이나 새 문서 작업 |  
| `docs` | 문서 수정 작업 |  
| `fix` | 잘못된 내용 수정 |  
| `son` | 작업한 사람 이름 |  
| `git-branch-note` | 어떤 작업인지 설명 |  



<br><br>

## 🟢 4. 자주 쓰는 Branch 명령어  

### 🟡 현재 branch 확인  
```bash
git branch  
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |  
| --- | --- | --- |
| `git` | Git | 버전 관리 프로그램 이름이다. 공식 약자 풀네임은 없다. |  
| `branch` | branch | branch 목록을 보거나 branch를 만들 때 쓰는 명령어다. |  

#### ⚫️ 출력 예시  
```txt
* main  
  feature/son-git-branch-note  
```

#### ⚫️ 보는 방법  
- `*` 표시가 붙은 곳이 현재 내가 서 있는 branch다.  
- 위 예시는 현재 `main` branch에 있다는 뜻이다.  

<br>

### 🟡 새 branch 만들기  
```bash
git branch feature/son-git-branch-note  
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |  
| --- | --- | --- |
| `git` | Git | 버전 관리 프로그램이다. |  
| `branch` | branch | branch를 만들거나 확인한다. |  
| `feature/son-git-branch-note` | branch 이름 | 새로 만들 branch 이름이다. |  

#### ⚫️ 중요  
- 이 명령어는 branch를 만들기만 한다.  
- 자동으로 이동하지 않는다.  
- 만든 branch로 이동하려면 `git switch`를 따로 해야 한다.  

<br>

### 🟡 branch 이동하기  
```bash
git switch feature/son-git-branch-note  
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |  
| --- | --- | --- |
| `git` | Git | 버전 관리 프로그램이다. |  
| `switch` | switch | 현재 작업 위치를 다른 branch로 바꾼다. |  
| `feature/son-git-branch-note` | branch 이름 | 이동할 branch 이름이다. |  

#### ⚫️ 출력 예시  
```txt
Switched to branch 'feature/son-git-branch-note'  
```

<br>

### 🟡 만들면서 바로 이동하기  
```bash
git switch -c feature/son-git-branch-note  
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |  
| --- | --- | --- |
| `git` | Git | 버전 관리 프로그램이다. |  
| `switch` | switch | branch를 이동한다. |  
| `-c` | create | 새 branch를 만들면서 바로 이동한다. |  
| `feature/son-git-branch-note` | branch 이름 | 만들고 이동할 branch 이름이다. |  

#### ⚫️ 초보자에게 추천하는 명령어  
- 새 branch를 만들 때는 `git switch -c 브랜치명`을 쓰는 것이 편하다.  
- branch 생성과 이동을 한 번에 처리하기 때문이다.  



<br><br>

## 🟢 5. 실습 순서  

### 🟡 1단계: main에 있는지 확인  
```bash
git branch  
```

- `* main`이 보이면 main에 있는 것이다.  
- 만약 다른 branch에 있다면 아래 명령어로 이동한다.  

```bash
git switch main  
```

<br>

### 🟡 2단계: 내 작업 branch 만들기  
```bash
git switch -c feature/son-git-branch-note  
```

- `feature/son-git-branch-note`라는 새 작업 공간을 만든다.  
- 동시에 그 branch로 이동한다.  

<br>

### 🟡 3단계: 파일 수정하기  
```bash
touch son_branch_practice.md  
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |  
| --- | --- | --- |
| `touch` | touch | 빈 파일을 만들거나 파일의 수정 시간을 갱신한다. |  
| `son_branch_practice.md` | Markdown 파일 이름 | 실습용 문서 파일이다. |  
| `.md` | Markdown | 문서 작성용 파일 확장자다. |  

<br>

### 🟡 4단계: 변경 상태 확인  
```bash
git status  
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |  
| --- | --- | --- |
| `git` | Git | 버전 관리 프로그램이다. |  
| `status` | status | 현재 파일 변경 상태를 보여준다. |  

#### ⚫️ 확인할 것  
- 새 파일이 `Untracked files`에 보이면 Git이 아직 추적하지 않는 파일이라는 뜻이다.  
- 커밋하려면 먼저 `git add`가 필요하다.  

<br>

### 🟡 5단계: 커밋 준비  
```bash
git add son_branch_practice.md  
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |  
| --- | --- | --- |
| `git` | Git | 버전 관리 프로그램이다. |  
| `add` | add | 변경 파일을 staging area에 올린다. |  
| `staging area` | staging area | 커밋할 파일을 잠시 모아두는 대기 공간이다. |  

<br>

### 🟡 6단계: 커밋 만들기  
```bash
git commit -m "docs: add branch practice note"  
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |  
| --- | --- | --- |
| `git` | Git | 버전 관리 프로그램이다. |  
| `commit` | commit | 변경 내용을 하나의 기록으로 저장한다. |  
| `-m` | message | 커밋 메시지를 바로 작성한다. |  
| `docs` | documentation | 문서 수정이라는 뜻의 커밋 타입이다. |  

<br>

### 🟡 7단계: branch 히스토리 확인  
```bash
git log --oneline --branches --graph  
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |  
| --- | --- | --- |
| `git` | Git | 버전 관리 프로그램이다. |  
| `log` | log | 커밋 기록을 보여준다. |  
| `--oneline` | one line | 커밋 하나를 한 줄로 짧게 보여준다. |  
| `--branches` | branches | 여러 branch의 커밋 기록을 함께 보여준다. |  
| `--graph` | graph | branch 흐름을 선 모양으로 보여준다. |  



<br><br>

## 🟢 6. GitHub에 Branch 올리기  

### 🟡 왜 branch를 GitHub에 올리는가  
- 내 컴퓨터에만 있는 branch는 팀원이 볼 수 없다.  
- Pull Request를 만들려면 내 branch를 GitHub 원격 저장소에 올려야 한다.  
- 이때 사용하는 명령어가 `git push`다.  

### 🟡 처음 올릴 때 사용하는 명령어  
```bash
git push -u origin feature/son-git-branch-note  
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |  
| --- | --- | --- |
| `git` | Git | 버전 관리 프로그램이다. |  
| `push` | push | 내 컴퓨터의 커밋을 원격 저장소로 올린다. |  
| `-u` | upstream | 현재 로컬 branch와 원격 branch를 연결한다. |  
| `origin` | origin | 원격 저장소의 기본 별명이다. 보통 GitHub 저장소를 뜻한다. |  
| `feature/son-git-branch-note` | branch 이름 | GitHub에 올릴 branch 이름이다. |  
| `upstream` | upstream | 내 branch가 어느 원격 branch와 연결되는지 나타내는 기준이다. |  

#### ⚫️ `-u`를 붙이면 좋은 이유  
- 다음부터는 branch 이름을 길게 쓰지 않아도 된다.  
- 한 번 연결한 뒤에는 아래처럼 짧게 올릴 수 있다.  

```bash
git push  
```



<br><br>

## 🟢 7. Branch 삭제  

### 🟡 병합이 끝난 branch 삭제  
```bash
git branch -d feature/son-git-branch-note  
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |  
| --- | --- | --- |
| `-d` | delete | 병합이 끝난 branch를 삭제한다. |  

#### ⚫️ 주의  
- `-d`는 안전한 삭제다.  
- 아직 merge되지 않은 branch는 Git이 삭제를 막을 수 있다.  
- 억지로 삭제하는 `-D`는 초보자 실습에서 쓰지 않는 것이 좋다.  



<br><br>

## 🟢 8. 초보자 실수 정리  

| 실수 | 왜 문제인지 | 해결 |  
| --- | --- | --- |
| main에서 바로 작업 | 팀 기준 최종본이 망가질 수 있다. | 작업 전 `git switch -c feature/작업명`을 한다. |  
| branch 만들고 이동 안 함 | 여전히 main에서 작업할 수 있다. | `git branch`로 `*` 위치를 확인한다. |  
| 이름을 `A`, `B`로 만듦 | 나중에 무슨 작업인지 모른다. | `feature/son-git-branch-note`처럼 의미 있게 쓴다. |  
| 커밋 메시지를 `update`로 씀 | 무엇을 바꿨는지 알 수 없다. | `docs: add branch practice note`처럼 쓴다. |  



<br><br>

## 🟢 9. 한 줄 요약  

- Branch는 `main`을 안전하게 지키기 위한 작업용 길이다.  
- 새 작업은 `main`에서 바로 하지 않고 `feature/작업명` branch에서 한다.  
- 작업 후 Pull Request를 만들고, 리뷰를 받은 뒤 main에 merge한다.  
