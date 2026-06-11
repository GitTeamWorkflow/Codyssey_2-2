# git revert Simulation

## git revert
- 특정 commit을 되돌리는 새로운 commit을 생성
    
## 시나리오
1. 테스트 파일 생성 및 staged, commit
```bash
bk@park/04_command_simulation# echo "revert test" >> test.txt
pbk@park/04_command_simulation# cat test.txt
test
test1
revert test
pbk@park/04_command_simulation# git add test.txt
pbk@park/04_command_simulation# git commit -m "feat: revert test"
[feat/37-park-command-simulation 83944ce] feat: revert test
 1 file changed, 1 insertion(+)
```
2. log 확인
```bash
pbk@park/04_command_simulation# git log
ccommit 83944ce9ef6848c9b8522a8d6e0d532d9221214d (HEAD -> feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 19:08:38 2026 +0900

    feat: revert test

commit d841f2e62814add91b8c6d304b7eabc045026aa0 (origin/feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 18:42:22 2026 +0900

    feat: add reset scenario3
```
3. 직전 commit을 revert 한다.
```bash
pbk@park/04_command_simulation# git revert 83944ce9ef6848c9b8522a8d6e0d532d9221214d
[feat/37-park-command-simulation e46cbac] Revert "feat: revert test"
 1 file changed, 1 deletion(-)
```
4. test.txt 확인
```bash
pbk@park/04_command_simulation# cat test.txt
test
test1
```
5. git status 확인
```bash
pbk@park/04_command_simulation# git status
On branch feat/37-park-command-simulation
Your branch is ahead of 'origin/feat/37-park-command-simulation' by 4 commits.
# revert는 새로운 commit을 만들기 때문에 commit 수가 늘어난다.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   revert.md

no changes added to commit (use "git add" and/or "git commit -a")
```
6. git log 확인
```bash
pbk@park/04_command_simulation# git log
commit e46cbacddb3fd28f41de5b020b9dbe2aba4d1037 (HEAD -> feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 19:22:31 2026 +0900

    Revert "feat: revert test"
    
    This reverts commit 83944ce9ef6848c9b8522a8d6e0d532d9221214d.

commit 83944ce9ef6848c9b8522a8d6e0d532d9221214d
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 19:08:38 2026 +0900

    feat: revert test

commit d841f2e62814add91b8c6d304b7eabc045026aa0 (origin/feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 18:42:22 2026 +0900

    feat: add reset scenario3
```

## 결론
- revert는 ***변경사항을 취소하려는 commit id(hash)***를 사용하여 되돌린다.
- reset과 다르게 revert는 ***새로운 commit을 만들기 때문에*** commit 수가 늘어난다.