# 🟩 이미 공유된 커밋 안전하게 취소하기 (git revert)  

이미 GitHub(원격 저장소)에 업로드(push)된 커밋을 삭제하지 않고, 안전하게 취소하는 방법인 `git revert`에 대해 배운다.  

<br><br>

## 🟢 git revert 란?  
- 이전 커밋을 없애는 대신, **특정 커밋의 변경 사항을 완전히 반대로 적용하는 새로운 커밋을 만드는 명령어**다.  
- 기록을 지우는 `reset`과 달리 과거 기록을 다 남겨둔 채 '되돌리기 버전을 하나 더 얹는 방식'이기 때문에, 이미 다른 팀원들과 공유한 원격 저장소 커밋을 취소할 때 유일하게 안전한 방법이다.  

<br><br>

## 🟢 reset vs revert의 결정적 차이  
- **reset (시간 여행)**  
    - 과거로 강제 이동하고 그 이후의 커밋 기록을 지워버림.  
    - 혼자 작업할 때만 써야 함.  
- **revert (반대 작업 추가)**  
    - 기존 커밋 기록을 그대로 유지하면서 반대 효과를 내는 커밋을 생성함.  
    - 이미 push하여 협업 중인 브랜치에서 필수적으로 사용해야 함.  

<br><br>

## 🟢 실습: 원격 저장소 커밋 취소하기  

### 🟡 1. 실습용 커밋 만들기  
- 실수한 셈 치고 `revert_test.txt` 파일을 만들고 커밋한다.  
    ```bash
    touch revert_test.txt
    git add revert_test.txt
    git commit -m "feat: add wrong file"
    ```

### 🟡 2. 취소할 커밋 해시 확인하기  
- `git log --oneline` 명령어로 방금 만든 커밋의 해시(앞 7자리)를 확인한다.  
    ```bash
    git log --oneline
    ```
    - 예시: `a1b2c3d feat: add wrong file` 이라면 해시는 `a1b2c3d`이다.  

### 🟡 3. Revert 실행하기  
- 아래 명령어를 입력하여 해당 커밋의 효과를 뒤집는다.  
    ```bash
    git revert a1b2c3d
    ```
- 명령어를 치면 자동으로 Vim 편집기가 열리면서 커밋 메시지를 적으라고 나온다.  
    - 보통 `Revert "feat: add wrong file"` 이라는 기본 메시지가 적혀 있으므로, 그대로 저장하고 나온다. (`:wq` 입력)  

### 🟡 4. 결과 확인하기  
- 폴더를 보면 `revert_test.txt` 파일이 감쪽같이 사라진 것을 볼 수 있다.  
- `git log --oneline`을 입력해보면 기존의 커밋도 그대로 남아있고, 그 위에 `Revert "feat: add wrong file"`이라는 새로운 커밋이 추가된 것을 볼 수 있다.  

<br><br>

## ⚠️ Revert 중 충돌이 났을 때  
- 취소하려는 과거 커밋 이후에 다른 커밋들이 같은 파일의 코드를 이미 고쳐버렸다면, revert 시 충돌(Conflict)이 발생할 수 있다.  
- 당황하지 말고 충돌 파일을 열어 충돌 마커를 정리해 올바른 코드를 남겨둔 뒤 아래 명령어를 차례로 입력한다.  
    ```bash
    git add [충돌해결한파일명]
    git revert --continue
    ```
- 만약 revert 작업을 도중에 아예 포기하고 원상복구하고 싶다면 아래 명령어를 입력한다.  
    ```bash
    git revert --abort
    ```
