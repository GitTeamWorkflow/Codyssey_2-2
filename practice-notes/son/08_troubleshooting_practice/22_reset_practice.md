# 🟩 git reset --soft HEAD~1 실습  

## 🟢 로컬 커밋 취소하고 변경 상태 유지하기  
방금 올린 커밋이 너무 성급했거나 누락된 파일이 있을 때, 커밋 기록 자체는 취소하되 내가 열심히 작성한 코드는 지우지 않고 그대로 남겨서 다시 작업할 수 있게 해주는 기능이다.  


<br><br>

## 🟢 실습 시나리오 진행  

<br>

### 🟡 1단계: 되돌릴 대상 커밋 생성  
실습을 위해 파일을 하나 수정하고, 새로운 버전을 커밋해 본다.  
```bash
echo "바보"  > test_reset_soft.txt  
git add test_reset_soft.txt  
git commit -m "바보"  

# 실제 terminal 내용  
[feature/26-git-troubleshooting-practice cbfd296] 바보  
 1 file changed, 1 insertion(+)  
 create mode 100644 practice-notes/son/08_troubleshooting_practice/test_reset_soft.txt  
```

<br>

### 🟡 2단계: soft reset 실행하기  
- 방금 한 커밋(`feat: 계산기 모듈 추가`)을 취소하되, `test_reset_soft.txt` 파일의 내용은 지우지 않고 그대로 보존하기 위해 아래 명령어를 실행한다.  
```bash
git reset --soft HEAD~1  
```


<br>

### 🟡 3단계: 상태 확인하기  
- 명령이 정상 작동했는지 확인하기 위해 깃 상태와 커밋 로그를 확인한다.  
```bash
git status  

On branch feature/26-git-troubleshooting-practice  
Your branch is ahead of 'origin/feature/26-git-troubleshooting-practice' by 2 commits.  
  (use "git push" to publish your local commits)  

Changes to be committed:  
  (use "git restore --staged <file>..." to unstage)  
        new file:   test_reset_soft.txt   # 🔥 다시 stage로 돌아온 것을 확인  
```

- 커밋 로그 확인  
```bash
git log --oneline  
# 최신 커밋이 이전 commit amend 실습 내요인 것을 확인  
7b12608 (HEAD -> feature/26-git-troubleshooting-practice) feat: complete git commit amend practice  
```


