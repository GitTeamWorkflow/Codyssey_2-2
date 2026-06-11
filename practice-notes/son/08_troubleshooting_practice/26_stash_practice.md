# 🟩 git stash / git stash pop 실습  

## 🟢 미완성 코드 임시 보관하고 복구하기  
새로운 기능을 개발하고 있는 도중에 갑자기 다른 브랜치로 가서 버그를 고쳐야 하는 긴급 상황이 생겼을 때, 개발 중이던 미완성 코드를 커밋하지 않고 안전하게 임시 보관함에 숨겨두었다가 나중에 다시 꺼내오는 기능이다.  


<br><br>

## 🟢 실습 시나리오 진행  

<br>

### 🟡 1단계: 미완성 코드 작성 중인 상태 재현  
파일을 수정하고 있지만 아직 커밋으로 저장하기에는 코드가 지저분하고 완성되지 않은 상태를 재현한다.  
```bash
echo "debugging print..." >> test_stash_01.txt  
echo "debugging print..." >> test_stash_02.txt  

git status  
# terminal log  
On branch feature/26-git-troubleshooting-practice  
Your branch is ahead of 'origin/feature/26-git-troubleshooting-practice' by 6 commits.  
  (use "git push" to publish your local commits)  

Untracked files:  
  (use "git add <file>..." to include in what will be committed)  
        test_stash_01.txt  
        test_stash_02.txt  

nothing added to commit but untracked files present (use "git add" to track)  
```
- `git status`를 확인하면 `test_stash.txt` 파일이 빨간색(Modified, 수정됨)으로 뜬다. 이 상태에서 다른 브랜치로 이동하려 하면 깃이 경고를 띄우며 이동을 막는다.  



<br>

### 🟡 2단계: stash 명령어를 사용하기 위해서는 tracked file이 되어야함  

```bash
git add test_stash_01.txt  
git add test_stash_02.txt  
git commit -m "test: files for stash practice"  

# 변경사항 만들기  
echo "debugging print2..." >> test_stash_01.txt  
echo "debugging print2..." >> test_stash_02.txt  
```




<br>

### 🟡 3단계: stash 명령어로 작업물 숨기기  
미완성 작업물을 잠시 깃 보관소에 안전하게 숨겨둔다.  
```bash
git stash  

# terminal log  
Saved working directory and index state WIP on feature/26-git-troubleshooting-practice: 738160f test: files for stash practice  
```

- 깃이 현재 내가 위치한 브랜치(`feature/26-git-troubleshooting-practice`)와 가장 최근 커밋(`a1b2c3d test: files for stash practice`)의 상태를 기준으로, 미완성 상태의 작업(WIP)을 안전하게 저장(Saved)했음을 알려주기 위해 출력되는 것이다.  

<br>

### 🟡 4단계: 임시 보관 목록 확인  
작업물이 보관함에 안전하게 들어가 있는지 목록을 조회해 본다.  
```bash
git stash list  

# terminal log  
stash@{0}: WIP on feature/26-git-troubleshooting-practice: 738160f test: files for stash practice  
```

- 깃 임시 보관소는 상자들을 차곡차곡 위로 쌓아두는 구조(Stack)이다. 가장 최신에 숨긴 내용이 0번째 방(`stash@{0}`)에 위치하게 되며, 보관소 안에 있는 작업 내용물들을 확인하기 위해 리스트를 보여주는 것이다.  

<br>

### 🟡 5단계: 다른 작업 완료 후 원래 코드로 복구  
다른 긴급한 볼일(브랜치 전환 등)을 모두 끝마쳤다면, 원래 하던 일을 계속하기 위해 보관했던 작업물을 도로 꺼내온다.  
```bash
git stash pop  

# terminal log  
On branch feature/26-git-troubleshooting-practice  
Your branch is ahead of 'origin/feature/26-git-troubleshooting-practice' by 7 commits.  
  (use "git push" to publish your local commits)  

Changes not staged for commit:  
  (use "git add <file>..." to update what will be committed)  
  (use "git restore <file>..." to discard changes in working directory)  
        modified:   test_stash_01.txt  
        modified:   test_stash_02.txt  

no changes added to commit (use "git add" and/or "git commit -a")  
Dropped refs/stash@{0} (cb104eade149faf4ec4e31d0091dc833c14fefb1)  
```

- 성공적으로 꺼내왔으므로 임시 보관함에 들어있던 기록(`stash@{0}`)을 완전히 삭제(`Dropped`)했음을 의미한다.  

