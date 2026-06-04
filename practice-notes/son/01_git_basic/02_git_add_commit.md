# 🟩 Git 버전 만들기 (git status, add, commit)  

소스코드의 변경 내용을 영구적으로 저장하는 '버전 만들기'의 흐름을 배운다. `git status`로 상태를 확인하고, `git add`로 대기실에 올린 뒤, `git commit`으로 버전을 완성하는 과정을 실습한다.  

<br><br>

## 🟢 버전 만들기 3단계 흐름  
- Git에서 버전을 만들 때는 작업 공간에서 저장소로 바로 저장되지 않는다. 반드시 아래의 3단계를 거친다.  
    - **1단계 (git status)**: 현재 작업 폴더의 변경 상태를 확인한다.  
    - **2단계 (git add)**: 변경된 파일 중 버전으로 만들 파일들을 선택해서 스테이지(Staging Area, 대기실)에 올린다.  
    - **3단계 (git commit)**: 스테이지에 있는 파일들을 묶어서 진짜 버전으로 완성하고 저장소(Repository)에 기록한다.  

<br><br>

## 🟢 1단계: 저장소 상태 확인하기 (git status)  
- 현재 내 컴퓨터의 작업 공간에 어떤 변화가 있는지 깃에게 물어보는 명령어다.  
    ```bash
    git status
    ```
- 만약 실습용 파일 `test.txt`를 새로 만들고 `git status`를 입력하면 아래와 같은 메시지가 나타난다.  
    ```text
    On branch main
    No commits yet

    Untracked files:
      (use "git add <file>..." to include in what will be committed)
            test.txt

    nothing added to commit but untracked files present (use "git add" to track)
    ```
    - **Untracked files**: Git이 아직 추적하지 않는 새로 만들어진 파일이라는 뜻이다.  

<br><br>

## 🟢 2단계: 스테이지에 파일 올리기 (git add)  
- 내가 작업한 파일 중, 버전으로 기록하고 싶은 파일만 골라내는 과정이다.  

### 🟡 특정 파일만 올리기  
- 파일 하나만 쏙 골라서 스테이지에 올리고 싶을 때 사용한다.  
    ```bash
    git add [파일명]
    ```
- 예시: `test.txt` 파일만 올리기  
    ```bash
    git add test.txt
    ```

### 🟡 변경된 파일 전부 한 번에 올리기  
- 수정한 파일이 너무 많을 때, 현재 폴더의 모든 변경 사항을 한 번에 올린다.  
    ```bash
    git add .
    ```
    - 여기서 마침표 `.`는 '현재 디렉토리와 그 하위 폴더 전체'를 의미한다.  

### 🟡 실수로 올린 파일 다시 내리기 (add 취소)  
- 실수로 엉뚱한 파일을 스테이지에 올렸을 때, 삭제하지 않고 안전하게 다시 내릴 수 있다.  
    ```bash
    git restore --staged [파일명]
    ```
    - 이 명령어를 실행해도 파일 내용은 지워지지 않고, 스테이지(대기실)에서만 안전하게 내려온다.  

<br><br>

## 🟢 3단계: 버전 완성하기 (git commit)  
- 스테이지에 대기 중인 파일들을 하나의 완성된 버전(커밋)으로 묶어 저장소에 기록한다.  

### 🟡 커밋 메시지와 함께 커밋하기  
- 이 버전이 어떤 변경 사항을 담고 있는지 설명하는 메시지를 반드시 함께 작성해야 한다.  
    ```bash
    git commit -m "커밋 메시지"
    ```
- 예시: 테스트 파일 생성 완료 후 커밋하기  
    ```bash
    git commit -m "feat: create test.txt"
    ```

### 🟡 커밋 완료 상태 확인하기  
- 커밋을 완료한 후, 정상적으로 버전이 만들어졌는지 확인하려면 다시 상태 조회를 입력한다.  
    ```bash
    git status
    ```
- 성공했다면 아래와 같은 안내가 나오며 작업 공간이 깨끗해진다.  
    ```text
    nothing to commit, working tree clean
    ```
