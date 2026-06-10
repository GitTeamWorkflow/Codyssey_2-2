# git stash Simulation

## git stash
- 현재 작업중인 내용을 임시로 저장
- 브랜치에서 작업 도중 commit 하지 않고 다른 브랜치로 이동해야 할 때 사용

### 시나리오 1
1. 파일 추가 수정
```bash
pbk@park/04_command_simulation# echo "stash test" >> test.txt
pbk@park/04_command_simulation# cat test.txt
test
test1
stash test
pbk@park/04_command_simulation# git add test.txt
pbk@park/04_command_simulation# git commit -m  "feat: stash test"
[feat/37-park-command-simulation 18310d0] feat: stash test
 1 file changed, 1 insertion(+)
```

2. stash를 사용하여 임시 저장
```bash
pbk@park/04_command_simulation# git stash
Saved working directory and index state WIP on feat/37-park-command-simulation: 18310d0 feat: stash test
```

3. log 확인
```bash
pbk@park/04_command_simulation# git log
commit 18310d073f6e7027a4911be1b83b94bc48287b82 (HEAD -> feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 19:32:00 2026 +0900

    feat: stash test
```

4.stash를 사용하여 임시 저장된 내용 복구
```bash
pbk@park/04_command_simulation# git stash pop
```

5. test.txt 확인
```bash
pbk@park/04_command_simulation# cat test.txt
test
test1
stash test
```

6. stash pop 후 log 확인
```bash
pbk@park/04_command_simulation# git log
commit 18310d0073688f581d4c03789c0ffb8672e60292 (HEAD -> feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 19:32:48 2026 +0900

    feat: stash test

commit 246d7e37074bbdb341e17075c23b88ca4664df8c (origin/feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 18:04:17 2026 +0900

    feat: learn ammend
```

### 요약
- stash pop를 사용하여 임시 저장된 내용을 복구하면 stash가 사라진다.

### 시나리오 2
1. test.txt에 변경사항 추가
```bash
pbk@park/04_command_simulation# echo "stash test apply" >> test.txt
pbk@park/04_command_simulation# cat test.txt
test
test1
stash test
stash test apply
```
2. git stash
```bash
pbk@park/04_command_simulation# git stash
Saved working directory and index state WIP on feat/37-park-command-simulation: 18310d0 feat: stash test
```
3. stash를 사용하여 임시 저장된 내용 복구
```bash
pbk@park/04_command_simulation# git stash apply
```

4. test.txt 확인
```bash
pbk@park/04_command_simulation# cat test.txt
test
test1
stash test
stash test apply
```

5. stash list 확인
```bash
pbk@park/04_command_simulation# git stash list
stash@{0}: WIP on feat/37-park-command-simulation: 18310d0 feat: stash test
```

### 요약
- stash apply 를 사용하면 stash list에 stash가 그대로 남아있는다.

## 시나리오 3
1. test.txt 에 메시지 추가
```bash
pbk@park/04_command_simulation# echo "stash test message" >> test.txt
pbk@park/04_command_simulation# cat test.txt
test
test1
stash test
stash test apply
stash test message
```
2. stash 에 메시지 추가
```bash
pbk@park/04_command_simulation# git stash -m "stash test"
Saved working directory and index state On feat/37-park-command-simulation: stash test
pbk@park/04_command_simulation# git stash list
stash@{0}: On feat/37-park-command-simulation: stash test
stash@{1}: WIP on feat/37-park-command-simulation: 18310d0 feat: stash test
```

2. stash apply로 메시지를 추가한 stash 복구 
```bash
pbk@park/04_command_simulation# git stash apply stash@{0}
```
