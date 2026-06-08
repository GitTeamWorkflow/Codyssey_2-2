# 🟩 .gitignore 파일 관리 제외하기  

버전을 만들 때 절대 Git에 올리면 안 되는 파일들이 있다. (예: 비밀번호가 적힌 파일, 용량이 너무 큰 파일) 이런 파일들을 자동으로 모른 척하게 만드는 방법을 배운다.  



<br><br>

## 🟢 .gitignore 이란?  
- Git이 추적하지 않고 무시할 파일이나 폴더의 목록을 적어두는 특별한 파일이다.  
- 이 파일에 이름을 적어두면 `git status`를 입력해도 해당 파일이 나타나지 않는다.  



<br><br>

## 🟢 파일 무시 및 커밋 실습  

### 🟡 1. 무시할 파일 만들기  
- 터미널에서 아래 명령어로 실습용 파일들을 만든다.  
    ```bash
    touch coding.html style.css secret.txt
    ```

### 🟡 2. 상태 확인하기  
- `git status`로 새로 만든 파일들이 추적 대상(Untracked)으로 나오는지 확인한다.  
    ```bash
    git status
    ```
- `secret.txt`를 포함한 모든 파일이 빨간색 글씨로 보일 것이다.  

### 🟡 3. .gitignore 파일 만들고 작성하기  
- `.gitignore`라는 이름의 파일을 만든다. (앞에 마침표 `.`가 반드시 들어가야 한다.)  
    ```bash
    touch .gitignore
    ```
- Vim이나 메모장으로 `.gitignore` 파일을 열고 무시할 파일명을 적는다.  
    ```text
    vim .gitignore
    secret.txt
    ```

### 🟡 4. 상태 다시 확인하기  
- 다시 `git status`를 입력해본다.  
    ```bash
    git status
    ```
- 놀랍게도 `secret.txt`는 사라지고, `coding.html`, `style.css`, 그리고 새로 만든 `.gitignore` 파일만 목록에 보일 것이다.  

### 🟡 5. 남은 파일들 버전에 올리기 (git add)  
- 이제 무시되지 않은 안전한 파일들(`coding.html`, `style.css`, `.gitignore`)만 스테이지(Stage)에 올린다.  
    ```bash
    git add coding.html style.css .gitignore
    ```
- 또는 아래 명령어를 사용하여 현재 폴더에서 무시되지 않은 파일 전부를 한 번에 올릴 수도 있다.  
    ```bash
    git add .
    ```

### 🟡 6. 버전 완성하기 (git commit)  
- 스테이지에 올라간 파일들을 커밋하여 하나의 버전으로 기록한다.  
    ```bash
    git commit -m "feat: add coding.html and style.css with gitignore"
    ```
- 이렇게 하면 `secret.txt`는 저장소에 들어가지 않고, 제외된 상태로 버전이 생성된다.  



<br><br>

## 🟢 자주 쓰는 .gitignore 작성 패턴  
- 파일 하나하나 적지 않고, 특정 규칙으로 여러 파일을 한 번에 무시할 수도 있다.  

### 🟡 주요 패턴 예시  
- 특정 확장자 모두 제외하기  
    - `*.log` (모든 .log 파일 제외)  
- 특정 폴더 전체 제외하기  
    - `node_modules/` (node_modules 폴더와 그 안의 모든 파일 제외)  
- 특정 폴더 안의 특정 파일 제외하기  
    - `config/secret.json`  



<br><br>

## 🟢 이미 커밋한 파일을 뒤늦게 무시하고 싶을 때 (중요)  
- 이미 `git add`를 하고 `git commit`까지 완료해서 Git이 관리하고 있는 파일(Tracked File)은 `.gitignore`에 써도 무시되지 않는다.  
- 이때는 Git 저장소에서 추적만 멈추도록 강제로 명령해야 한다.  

### 🟡 추적 제외 명령어  
- `git rm --cached [파일명]`  
    - 예시: `git rm --cached secret.txt`  
    - 파일 자체는 내 컴퓨터에서 삭제되지 않고 그대로 유지되지만, Git 저장소의 추적 대상에서만 쏙 빠진다.  

### 🟡 추적 제외 완료를 위한 add와 commit  
- 추적을 제외한 사실도 변경 사항이므로, 이를 반영하기 위해 다시 `add`하고 `commit`해야 한다.  
    ```bash
    git add .gitignore secret.txt
    git commit -m "chore: stop tracking secret.txt"
    ```
- 이제 `secret.txt`는 Git 저장소에서 추적이 완전히 중단되고 제외된다.  
