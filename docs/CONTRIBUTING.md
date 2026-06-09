# 🟩 Team Contributing Guide  

- 이 문서는 우리 팀이 GitHub Flow로 협업할 때 지키는 공통 규칙을 정리한다.  
- 모든 작업은 Issue 생성 > Branch 생성 및 작업 > Pull Request 리뷰 > main 병합 순서로 진행한다.  

<br>

#### ⚫️ GitHub Flow를 선택한 이유  

- 브랜치 구조가 단순해서 팀원이 작업 흐름을 빠르게 이해할 수 있다.  
- 모든 변경이 PR과 리뷰를 거치므로 변경 이유와 검증 기록이 남는다.  
- `main`을 안정적인 상태로 유지하면서 작은 단위로 자주 병합할 수 있다.  

<br>

#### ⚫️ 전체적인 작업 흐름  

1. Issue를 생성한다.  
예시 : [Docs] CONTRIBUTING.md 브랜치 전략, 작업 흐름 예시 업데이트 #7  

2. Issue 번호를 확인한다.  
Issue 생성시 Git에서 Issue 번호를 부여한다.  

3. `feature/<이름>-<작업요약>` 브랜치를 만든다.  
```bash
git switch main  
git pull origin main  
git switch -c docs/7-docs-contributing-브랜치-전략-작업-흐름-예시-업데이트  
```

4. 작업 후 커밋한다.  
```bash
git add CONTRIBUTING.md  
git commit -m "[Docs] CONTRIBUTING.md 브랜치 전략 수정, 작업 흐름 예시 업데이트"  
```

5. PR 본문에 `Closes #이슈번호` 또는 `Fixes #이슈번호`를 작성한다.  
Add a description  
Closes #이슈번호  







<br><br><br><br>

---

## 🟢 1. Issue 규칙  
- 모든 작업은 Issue에서 시작한다.  

### 🟡 Issue Title  
- **새로운 기능 추가 (Feat)**  
    - [Feat] 소셜 로그인(구글, 네이버) 연동  
    - [Feat] 상품 상세 페이지 내 리뷰 정렬 필터(최신순·별점순) 구현  
    - [Feat] 마이페이지 회원 탈퇴 및 데이터 삭제 기능 추가  
- **버그 및 오류 수정 (Fix)**  
    - [Fix] 모바일 환경에서 네비게이션 바가 화면을 가리는 현상 수정  
- **구조 변경 (Refactor)**  
    - [Refactor] 레거시 인증 미들웨어 구조 개선 및 가독성 확보  
- **빌드 설정, 의존성, 인프라 (Chore / CI/CD)**  
    - [Chore] 개발 환경 구축을 위한 .env.example 파일 최신화  
- **문서 작업 (Docs)**  
    - [Docs] README.md에 로컬 서버 구동을 위한 환경 변수 설정 방법 보완  

### 🟡 Issue Description  
- **작업 목적**  





<br><br>

## 🟢 2. 브랜치 규칙  
- 우리 팀은 GitHub Flow 기반의 브랜치 전략을 사용한다.  

### 🟡 브랜치 종류  

| 브랜치          | 용도                            |  
| ------------ | ----------------------------- |
| `main`       | 최종 결과물이 반영되는 기본 브랜치           |  
| `feature/*`  | 새로운 기능 추가 또는 실습 단위 작업 브랜치     |  
| `docs/*`     | README, 보고서, 팀 문서 등 문서 작업 브랜치 |  
| `fix/*`      | 오류 수정 또는 기존 기능 수정 브랜치         |  
| `refactor/*` | 기능 변화 없이 코드 구조 개선 작업 브랜치      |  
| `chore/*`    | 설정 파일, 폴더 구조, 의존성 등 기타 작업 브랜치 |  


### 🟡 브랜치 네이밍 규칙  

#### ⚫️ 형식  
feature/<이슈번호>-<팀원명>-<작업요약>  
fix/<이슈번호>-<팀원명>-<수정요약>  
docs/<이슈번호>-<팀원명>-<문서요약>  

- 예시:  
    - `feature/1-lee-string-utils`  
    - `feature/2-park-team-note`  
    - `docs/3-lim-contributing-guide`  
    - `fix/4-son-readme-links`  

#### ⚫️ 상세 규칙  
- 이름은 영문 소문자로 작성한다.  
- 작업 요약은 2~4개 단어로 짧게 작성하고, 단어 사이는 `-`로 연결한다.  
- 한 브랜치에는 하나의 Issue 또는 하나의 작업 단위만 포함한다.  
- 공동 작업 브랜치가 필요하면 `feature/team-conflict-practice`처럼 `team`을 사용한다.  
 




<br><br>

## 🟢 3. 커밋 메시지 컨벤션  

- 커밋 메시지는 아래 형식을 사용한다.  
    - <type>: <subject>  

#### ⚫️ 사용 가능한 type  
- `feat`: 새로운 기능, 파일, 예시 추가  
- `fix`: 버그 수정, 잘못된 링크/오타/동작 수정  
- `docs`: 문서 작성 또는 수정  
- `refactor`: 동작 변화 없는 구조 개선  
- `test`: 테스트 또는 검증 코드 추가  
- `chore`: 설정, 폴더 구조, 기타 관리 작업  

- 예시:  
    - `feat: learned git branch and merge`  
    - `docs: add conflict resolution record`  
    - `fix: update readme practice note links`  
    - `refactor: organize team note folders`  

#### ⚫️ 상세 규칙  
- subject는 영어 또는 한국어 모두 가능하지만 변경 대상을 알 수 있어야 한다.  
- 한 커밋에는 하나의 목적만 담는다.  
- 이미 push한 커밋은 팀 합의 없이 rebase나 강제 push로 수정하지 않는다.  







<br><br>

## 🟢 5. Pull Request 작성 규칙  

- 모든 feature 브랜치는 PR로 `main`에 병합한다.  
- PR 작성자는 본인 PR을 직접 승인하지 않는다.  

### 🟡 PR 제목 형식  
<type>: <작업 요약>


### 🟡 PR 본문  
```markdown
## 변경 사항(What)  
- 

Closes #이슈번호  
```

### 🟡 PR 병합 조건:  
- 실질 리뷰 코멘트가 최소 1개 이상 있어야 한다.  
- 최소 1명 이상의 approve가 있어야 한다.  
- 충돌이 있으면 작성자가 해결한 뒤 다시 리뷰를 요청한다.  







<br><br>

## 🟢 6. Code Review 규칙  
- 신규 내용 및 변경 이유를 함께 검증하는 절차다.  

### 🟡 리뷰어 규칙  
- 본인 PR은 리뷰 승인하지 않는다.  
- 코드나 내용에 대한 의견을 간략하게 정리한다.  

#### ⚫️ 실질 리뷰 코멘트 예시  
- `README의 practice-notes 링크가 실제 폴더명과 맞는지 확인이 필요합니다.`  
- `이 함수는 빈 문자열 입력에서 어떤 값을 반환해야 하는지 docstring에 적으면 좋겠습니다.`  
- `충돌 해결 기록에 어떤 브랜치에서 충돌이 났는지 추가하면 재현하기 쉬울 것 같습니다.`  







<br><br>

## 🟢 7. 충돌 발생 시 대응 흐름  

충돌이 발생하면 당황해서 파일을 덮어쓰지 않고 아래 순서로 처리한다.  

1. 충돌이 난 브랜치와 대상 브랜치를 확인한다.  
2. `git status`로 충돌 파일 목록을 확인한다.  
3. 충돌 파일에서 `<<<<<<<`, `=======`, `>>>>>>>` 마커를 찾는다.  
4. 양쪽 변경 의도를 확인하고 필요한 내용만 남긴다.  
5. 충돌 마커를 모두 제거한다.  
6. 실행 또는 문서 링크 확인 등 가능한 검증을 수행한다.  
7. `git add <파일>` 후 충돌 해결 커밋을 만든다.  
8. 🔥 PR에 어떤 충돌이었고 어떻게 해결했는지 댓글로 남긴다.  
9. `docs/conflict-resolution.md`에 상황, 원인, 해결 절차, 결과, 주의점을 기록한다.  








<br><br>
 
## 🟢 8. 트러블슈팅 기록 규칙  

Git 명령 실습이나 실수 복구는 `docs/troubleshooting-log.md`에 기록한다.  

기록에는 아래 항목을 포함한다.  

- 참여자  
- 상황  
- 사용한 명령어  
- 해결 절차  
- 결과  
- 배운 점 또는 주의점  

특히 아래 네 가지는 팀 전체가 반드시 기록한다.  

- `git commit --amend`  
- `git reset --soft HEAD~1`  
- `git revert`  
- `git stash` / `git stash pop`  








<br><br>

## 🟢 9. 금지 사항  

- `main` 브랜치 직접 push 금지  
- 팀 합의 없는 강제 push 금지  
- 공유 브랜치에서 무리한 히스토리 재작성 금지  
