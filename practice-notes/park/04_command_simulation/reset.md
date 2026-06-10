# git reset Simulation

## git reset
- HEAD를 특정 커밋으로 되돌릴 때 사용

### 시나리오
1. 테스트 파일 생성 및 staged, commit
```bash
pbk@park/04_command_simulation# touch test.txt
pbk@park/04_command_simulation# echo "test" >> test.txt
pbk@park/04_command_simulation# git add test.txt
pbk@park/04_command_simulation# git commit -m "feat: create test.txt"
```
2. log 확인
```bash
pbk@park/04_command_simulation# git log
commit 8f35a6645f53dbee9d9637a4cf310a9f399a1433 (HEAD -> feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 18:19:56 2026 +0900

    feat: create test.txt

commit 246d7e37074bbdb341e17075c23b88ca4664df8c (origin/feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 18:04:17 2026 +0900

    feat: learn ammend
```

3. 직전 commit을 soft reset 한다.
```bash
pbk@park/04_command_simulation# git reset --soft HEAD~1
```

4. log 확인
```bash
pbk@park/04_command_simulation# git log
commit 246d7e37074bbdb341e17075c23b88ca4664df8c (HEAD -> feat/37-park-command-simulation, origin/feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 18:04:17 2026 +0900

    feat: learn ammend
```

5. git status 확인
```bash
pbk@park/04_command_simulation# git status
On branch feat/37-park-command-simulation
Your branch is up to date with 'origin/feat/37-park-command-simulation'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   test.txt
```

### 요약
- soft reset을 사용하여 직전 commit을 되돌리면 HEAD만 변경되고, staging area와 working directory는 그대로 남는다. 따라서 staging area에 파일이 staging된 상태로 남게 된다.

### 결론
- soft reset: HEAD만 변경, staging area와 working directory는 그대로
- mixed reset: HEAD와 staging area 변경, working directory는 그대로
- hard reset: HEAD와 staging area와 working directory 모두 변경
    
