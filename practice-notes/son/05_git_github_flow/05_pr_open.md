# 🟩 Pull Request 생성하기  
## 🟢 Pull Request (PR)란 무엇인가?  
Pull Request는 직역하면 "당겨달라고 요청하기"이다.  
내가 만든 브랜치의 코드 변경 사항을 기본이 되는 main 브랜치에 가져가서(당겨서) 합쳐달라고 관리자나 동료들에게 요청을 보내는 기능이다.  

<br><br>

### 🟡 영어 단어 및 용어 풀네임  
| 용어 | 풀네임 (Full Name) | 해설 |  
| :--- | :--- | :--- |  
| PR | Pull Request | 변경된 코드를 원본 저장소에 당겨서 합쳐달라는 요청서이다. |  
| Repository | Repository (저장소) | 프로젝트의 파일과 버전 관리 기록들이 저장되어 있는 인터넷 공간이다. |  
| Compare | Compare (비교하다) | 원본 코드(base)와 내가 수정한 코드(compare) 사이의 차이점을 분석하여 보여주는 기능이다. |  
| Create | Create (생성하다) | 새로운 Pull Request 요청서를 공식적으로 등록하여 생성하는 행위이다. |  

<br><br>

## 🟢 실습하기: GitHub 웹사이트에서 PR 보내기  

<br><br>

### 🟡 1. GitHub 저장소 페이지 접속  
내 컴퓨터의 브랜치를 `git push`로 올리고 나면, GitHub 웹 사이트 내 저장소(Repository) 첫 화면에 노란색 안내창이 나타난다.  

<br><br>

### 🟡 2. Compare & pull request 버튼 클릭  
노란색 안내창 오른쪽에 있는 `Compare & pull request` 버튼을 클릭한다.  
- 버튼이 안 보인다면 `Pull requests` 탭으로 직접 이동해서 `New pull request` 버튼을 누르면 된다.  

<br><br>

### 🟡 3. 제목과 내용 작성하기  
동료들이 내가 작성한 코드를 쉽게 파악할 수 있도록 명확하게 작성해야 한다.  
- 제목: 무엇을 개발했는지 한눈에 알 수 있게 요약해서 적는다. (예: `feat: 로그인 화면 개발`)  
- 본문: 어떤 문제를 해결하기 위해 이 작업을 했는지, 구체적으로 무엇을 바꿨는지 적는다.  

<br><br>

### 🟡 4. Create pull request 버튼 누르기  
내용 작성이 완료되었다면 하단에 있는 초록색 `Create pull request` 버튼을 누른다.  
버튼을 누르면 공식적으로 내 코드가 검토 대기 상태가 되며, 다른 사람들과 이 코드에 대해 이야기를 나눌 수 있는 토론방이 열린다.  
