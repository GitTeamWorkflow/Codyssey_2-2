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

## 시나리오 2
1. 테스트 파일 staged, commit 후 mixed reset을 한다.
```bash
pbk@park/04_command_simulation# echo "test1" >> test.txt
pbk@park/04_command_simulation# git add test.txt
pbk@park/04_command_simulation# git commit -m "feat: reset mixed test"
[feat/37-park-command-simulation d6f5d5b] feat: reset mixed test
 1 file changed, 1 insertion(+)
```
2. log 확인
```bash
pbk@park/04_command_simulation# git log
commit d6f5d5b6be1dc874e8267757f9fd9682ed31c601 (HEAD -> feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 18:30:46 2026 +0900

    feat: reset mixed test

commit bb6c72d2e5b023300311f50cba50dc54193a36b6 (origin/feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 18:28:14 2026 +0900

    feat: add reset scenario1
```

3. git status 확인
```bash
pbk@park/04_command_simulation# git status
On branch feat/37-park-command-simulation
Your branch is ahead of 'origin/feat/37-park-command-simulation' by 1 commit.
  (use "git push" to publish your local commits)
```

4. 직전 commit을 mixed reset 한다.
```bash
pbk@park/04_command_simulation# git reset --mixed HEAD~1
Unstaged changes after reset:
M       practice-notes/park/04_command_simulation/reset.md
M       practice-notes/park/04_command_simulation/test.txt
```

5. log 확인
```bash
pbk@park/04_command_simulation# git log
commit bb6c72d2e5b023300311f50cba50dc54193a36b6 (HEAD -> feat/37-park-command-simulation, origin/feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 18:28:14 2026 +0900

    feat: add reset scenario1
```

6. git status 확인
```bash
pbk@park/04_command_simulation# git status
On branch feat/37-park-command-simulation
Your branch is up to date with 'origin/feat/37-park-command-simulation'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   test.txt
```

### 요약
- mixed reset을 사용하여 직전 commit을 되돌리면 HEAD와 staging area가 변경되고, working directory는 그대로 남는다.

## 시나리오 3
1. 테스트 파일 staged, commit 후 hard reset을 한다.
```bash
pbk@park/04_command_simulation# touch test.txt
pbk@park/04_command_simulation# echo "test2" >> test.txt
pbk@park/04_command_simulation# cat test.txt
test
test1
test2
pbk@park/04_command_simulation# git add test.txt
pbk@park/04_command_simulation# git commit -m "feat: reset hard test"
```
2. log 확인
```bash
pbk@park/04_command_simulation# git log
commit 3733b49e86f65aaebde8a6782246903dd77fc79a (HEAD -> feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 18:36:26 2026 +0900

    feat: reset hard test

commit a0cbf9c0ec56ff52a31b1767687605c49a8826d5 (origin/feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 18:35:04 2026 +0900

    feat: add reset scenario2
```

3. 직전 commit을 hard reset 한다.
```bash
pbk@park/04_command_simulation# git reset --hard HEAD~1
HEAD is now at a0cbf9c feat: add reset scenario2
```

4. log 확인
```bash
pbk@park/04_command_simulation# git log
commit a0cbf9c0ec56ff52a31b1767687605c49a8826d5 (HEAD -> feat/37-park-command-simulation, origin/feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 18:35:04 2026 +0900

    feat: add reset scenario2
```

5. git status 확인
```bash
pbk@park/04_command_simulation# git status
On branch feat/37-park-command-simulation
Your branch is up to date with 'origin/feat/37-park-command-simulation'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   reset.md

no changes added to commit (use "git add" and/or "git commit -a")
```

6. test.txt 파일 내용 확인
```bash
pbk@park/04_command_simulation# cat test.txt
test
test1
```

### 요약
- hard reset을 사용하여 직전 commit을 되돌리면 HEAD와 staging area와 working directory 모두 변경된다.


### 결론
- soft reset: HEAD만 변경, staging area와 working directory는 그대로
- mixed reset: HEAD와 staging area 변경, working directory는 그대로
- hard reset: HEAD와 staging area와 working directory 모두 변경
    

