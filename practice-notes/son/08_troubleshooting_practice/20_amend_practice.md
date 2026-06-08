# 🟩 git commit --amend 실습  

## 🟢 최근 커밋 메시지 수정하기  
가장 최근에 저장한 커밋 메시지에 오타가 있거나, 설명이 부족할 때 이전 커밋을 덮어쓰는 방식으로 메시지를 편리하게 수정할 수 있는 방법이다.  


<br><br>

## 🟢 실습 시나리오 진행  

<br>

### 🟡 1단계: 오타가 포함된 커밋 작성  
연습용 파일을 하나 생성하고, 메시지에 일부러 오타(`로그은`)를 넣어서 커밋을 저장해 본다.  
```bash
echo "test" > test_amend.txt  
git add test_amend.txt  
git commit -m "feat: create test file for commit amend test"  
```


<br>

### 🟡 2단계: amend 명령어로 메시지 바로잡기  
오타를 발견했으니, 다시 파일을 수정할 필요 없이 최근의 커밋 메시지만 덮어써서 올바르게 바꾼다.  
```bash
git commit --amend -m "feat: 짠! 고침! create test file for commit amend test"  
```


<br>

### 🟡 3단계: 수정 결과 확인하기  
커밋 로그를 한 줄로 축약해서 보고, 오타가 잘 수정되었는지 확인한다.  
```bash
git log --oneline --graph --all  

# 실제 terminal 내용 확인  
* 88736da (HEAD -> feature/26-git-troubleshooting-practice) feat: 짠! 고침! create test file for commit amend test  
```


<br>

### 🟡 주의점  
- 이미 GitHub 원격 저장소에 업로드(push)를 완료한 커밋에 대해서는 `--amend` 사용을 권장하지 않는다.  
- 원격에 올라간 히스토리를 강제로 수정하면 다른 팀원들의 컴퓨터 기록과 꼬이게 되므로, 로컬 컴퓨터 안에서만 안전하게 실행해야 한다.  
