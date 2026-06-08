# 🟩 Git 충돌 해결 실습하기 (비자명 충돌 포함)  

협업할 때 가장 자주 겪고 무서워하는 '충돌(Conflict)' 상황을 내 컴퓨터에서 의도적으로 만들어보고, 이를 똑똑하게 해결하는 방법을 배운다. 특히 과제 요구사항에 있는 '비자명 충돌' 상황 2가지를 직접 재현하고 해결해 본다.  


<br>

## 🟢 충돌 마커 이해하기  
- 충돌이 발생하면 Git은 파일 내부에 아래와 같은 특수한 표시(마커)를 남겨서 우리에게 수정을 요구한다.  
    ```text
    <<<<<<< HEAD  
    현재 내가 선택한 브랜치(예: main)의 내용  
    =======  
    가져와서 합치려고 하는 브랜치(예: feature/A)의 내용  
    >>>>>>> feature/A  
    ```
- 해결 방법: 이 마커들을 포함하여 불필요한 줄을 지우고, 최종적으로 남길 올바른 코드만 남긴 뒤 저장하면 된다.  


<br><br>

## 🟢 실습 1: 비자명 충돌 - 같은 위치 다른 내용 수정  
- 같은 파일의 아주 가까운 줄(인접 라인)을 서로 다른 브랜치에서 동시에 수정했을 때 발생하는 충돌이다.  

### 🟡 1. 기본 파일 생성 및 첫 커밋  
- `main` 브랜치에서 `conflict.txt` 파일을 만들고 커밋한다.  
    ```bash
    git switch main  
    echo "1번 줄: 공통 시작" > conflict.txt  
    git add conflict.txt  
    git commit -m "docs: add conflict.txt"  
    ```

### 🟡 2. 브랜치 A 만들고 수정하기  
- `feature/A` 브랜치를 파서 파일 내용을 고치고 커밋한다.  
    ```bash
    git branch feature/A  
    git switch feature/A  
    echo -e "1번 줄: 공통 시작\n2번 줄: A가 쓴 내용" > conflict.txt  
    git add conflict.txt  
    git commit -m "docs: edit by A"  
    ```

### 🟡 3. main 브랜치로 돌아와 다르게 수정하기  
- 다시 `main` 브랜치로 와서 똑같이 2번 줄 영역에 다른 내용을 쓰고 커밋한다.  
    ```bash
    git switch main  
    echo -e "1번 줄: 공통 시작\n2번 줄: main이 다르게 쓴 내용" > conflict.txt  
    git add conflict.txt  
    git commit -m "docs: edit by main"  
    ```

### 🟡 4. 병합 시도하여 충돌 유발하기  
- `main` 브랜치에 `feature/A`를 합치려고 하면 충돌이 터진다.  
    ```bash
    git merge feature/A  
    ```
- 출력 메시지 예시: `CONFLICT (content): Merge conflict in conflict.txt`  

### 🟡 5. 충돌 해결하기  
- `conflict.txt` 파일을 열면 충돌 마커가 보인다.  
- 마커들을 지우고 `A가 쓴 내용`과 `main이 다르게 쓴 내용`을 조화롭게 합친 최종본으로 파일을 수정해 저장한다.  
- 수정 완료 후 저장소에 새로 반영한다.  
    ```bash
    git add conflict.txt  
    git commit -m "fix: resolve merge conflict between main and feature/A"  
    ```


<br><br>

## 🟢 실습 2: 비자명 충돌 - 파일 이름 변경 vs 내용 수정  
- 한쪽 브랜치에서는 파일 이름을 바꾸거나 이동했고, 다른 쪽 브랜치에서는 그 파일의 내용을 고쳤을 때 일어나는 충돌(Rename/Modify Conflict)이다.  

### 🟡 1. 기본 파일 생성 및 커밋  
- `main` 브랜치에서 `target.txt` 파일을 생성하고 커밋한다.  
    ```bash
    git switch main  
    echo "수정 대상 파일" > target.txt  
    git add target.txt  
    git commit -m "docs: add target.txt"  
    ```

### 🟡 2. 브랜치 B 만들고 이름 변경하기  
- `feature/rename` 브랜치를 파서 파일 이름을 변경하고 커밋한다.  
    ```bash
    git branch feature/rename  
    git switch feature/rename  
    mv target.txt renamed_target.txt  
    git commit -m "refactor: rename target.txt"  
    ```

### 🟡 3. main 브랜치로 돌아와 파일 내용 수정하기  
- 다시 `main` 브랜치로 돌아와 기존 파일의 내용을 수정하고 커밋한다.  
    ```bash
    git switch main  
    echo "수정 대상 파일 (main이 수정한 줄)" > target.txt  
    git add target.txt  
    git commit -m "docs: modify target.txt"  
    ```

### 🟡 4. 병합 시도하여 충돌 유발하기  
- `main` 브랜치에서 `feature/rename` 브랜치를 합친다.  
    ```bash
    git merge feature/rename  
    ```
- 출력 메시지 예시: `CONFLICT (rename/modify): target.txt renamed to renamed_target.txt in feature/rename. target.txt kept in HEAD as target.txt`  

### 🟡 5. 충돌 해결하기  
- Git은 파일명이 바뀐 것과 내용이 수정된 것 중 무엇을 택해야 할지 몰라 두 파일을 모두 남겨둔 상태다.  
- 우리는 수정된 내용을 바뀐 파일명으로 합치고 기존 옛날 파일은 지우기로 결정한다.  
    ```bash
    # 1. 바뀐 파일명으로 수정한 내용을 복사해 붙여넣거나 정리한다.  
    # 2. 옛날 파일 target.txt는 삭제한다.  
    rm target.txt  
    
    # 3. 바뀐 파일 renamed_target.txt에 최신 수정한 내용을 적용하여 스테이지에 올린다.  
    git add renamed_target.txt target.txt  
    
    # 4. 커밋하여 해결을 완료한다.  
    git commit -m "fix: resolve rename/modify conflict"  
    ```
