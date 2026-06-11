# 🟩 git revert 실습  

## 🟢 원격 저장소에 업로드된 커밋 되돌리기  
이미 GitHub(원격 저장소)에 업로드(push)를 완료한 커밋에 잘못된 부분이 있을 때, 기존 버전을 강제로 삭제하지 않고 "기존 커밋을 취소했다"는 기록을 새로 남기며 안전하게 되돌리는 방법이다.  


<br><br>

## 🟢 실습 시나리오 진행  

### 🟡 1단계: 취소 대상이 되는 잘못된 커밋 생성  
실습을 위해 잘못된 코드를 담은 커밋을 생성한다.  
```bash
echo "error code" > test_revert.txt  
git add test_revert.txt  
git commit -m "feat: test revert practice"  

# commit 완료 terminal log  
[feature/26-git-troubleshooting-practice ce5b6c8] feat: test revert practice  
 1 file changed, 1 insertion(+)  
 create mode 100644 practice-notes/son/08_troubleshooting_practice/test_revert.txt  
```

<br>

### 🟡 2단계: 취소할 커밋 해시(Hash) 확인  
커밋 로그를 조회하여 내가 취소하고 싶은 커밋의 고유 번호(해시값) 앞자리 7글자를 찾아낸다.  
```bash
git log --oneline  
```
-  hash 확인:  
    - `ce5b6c8` feat: test revert practice   
    - ce5b6c8c36e947afe163aaf4fb0e59d3f5edbc1d  


<br>

### 🟡 3단계: revert 명령어로 취소용 커밋 만들기  
해당 해시 코드를 인자로 전달하여 취소 작업을 수행한다.  
```bash
git revert ce5b6c8c36e947afe163aaf4fb0e59d3f5edbc1d  
```
- test_revert.txt 사라짐  


<br>

### 🟡 4단계: 결과 로그 확인  
```bash
git log --oneline  

# terminal에서 잘 확인됨  
d629247 (HEAD -> feature/26-git-troubleshooting-practice) Revert "feat: test revert practice"  
```  
- 이처럼 기존 버그 커밋을 보존하면서, 그것을 되돌린(Revert) 흔적이 히스토리에 고스란히 남아 협업할 때 안전하다.  



<br>

### 🟡 reset과의 차이점  
- **reset**은 시간을 통째로 과거로 돌려 이후의 흔적을 아예 없애버리는 방식이고,  
- **revert**는 과거의 일을 취소하는 새로운 사건(커밋)을 미래에 기록하는 방식이다.  
- 이미 원격 저장소에 공유한 코드를 수정할 때는 반드시 **revert**를 써야 팀원들의 깃 역사와 부딪치지 않는다.  


