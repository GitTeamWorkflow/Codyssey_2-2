# git amend Simulation

## git amend
- 이전 커밋에 변경사항을 적용하고 싶을 때 사용

## 시나리오 1

1. 브랜치에서 첫번째 커밋을 만들었다.
```bash
pbk@park/04_command_simulation# git commit -m "feat: create md"
```
2. 커밋 메시지를 변경하려 하는데 그 전에 현재 저장된 내역을 확인하고 싶다.
```bash
pbk@park/04_command_simulation# git log
commit c28d87bbca31ec0088461bf2f114354a55dafc9d (HEAD -> feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 17:48:56 2026 +0900

    feat: create md
```
3. 커밋 메시지를 변경하고, staging 된 파일도 추가한다.
```bash
pbk@park/04_command_simulation# git commit --amend
```

4. vi 를 사용해서 커밋 메시지를 변경한다.
- vi 편집기 화면
```bash
feat: create md

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
...
```

- vi 편집기에서 커밋 메시지 수정
```bash
feat: create command md

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
...
```
5. 수정 후 로그 확인
```bash
pbk@park/04_command_simulation# git log           
commit 8d0ed8943cf86bd148d888bee9dd70745b2fb27f (HEAD -> feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 17:48:56 2026 +0900

    feat: create command md
```

## 시나리오 2 

1. commit 후 해당 파일을 추가 수정했다.
- 현재 commit 명 : feat: learn amend
2. 동일한 commit에 변경사항을 적용하려한다.
```bash
pbk@park/04_command_simulation# git add amend.md 
```
```bash
pbk@park/04_command_simulation# git commit --amend --no-edit
[feat/37-park-command-simulation e3723da] feat: learn amend
 Date: Wed Jun 10 18:04:17 2026 +0900
 1 file changed, 54 insertions(+)
```
3. 로그 확인
```bash
pbk@park/04_command_simulation# git log                      
commit e3723dafc04192dfa2b02ab9540e6b97cb02d312 (HEAD -> feat/37-park-command-simulation)
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 18:04:17 2026 +0900

    feat: learn amend

commit 8d0ed8943cf86bd148d888bee9dd70745b2fb27f
Author: pbk98 <bumkyu8425@naver.com>
Date:   Wed Jun 10 17:48:56 2026 +0900

    feat: create command md
```

### 정리
1. git commit --amend는 마지막 commit을 다시 만드는 명령어라서, 이미 commit했던 파일 내용을 수정한 뒤 그 commit에 다시 포함할 수 있으며 이 과정에서 커밋 메시지도 변경할 수 있다.

2. git commit --amend --no-edit는 위 1번 기능에서 커밋 메시지를 변경하고 싶지 않을 때 사용한다.

3. amend 옵션은 직전의 커밋을 변경하는 옵션이기 때문에 그 이전의 커밋을 변경하려면 rebase 옵션을 사용해야 한다.