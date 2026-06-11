# 🟩 Git Merge 학습 노트  

## 🟢 전체 목차  
- 01_git_branch.md: 브랜치가 무엇인지, 왜 쓰는지, 어떻게 만들고 이동하는지  
- ✅ 02_git_merge.md: 브랜치를 main에 합치는 방법과 병합 종류  
- 03_git_conflict.md: 충돌이 왜 생기는지와 해결 방법  

<br><br>

## 🟢 1. Merge 뜻  

### 🟡 Merge는 나누어진 작업을 합치는 것이다  
- Merge는 한국어로 `병합하다`, `합치다`라는 뜻이다.  
- Git에서 merge는 한 branch의 커밋을 다른 branch에 합치는 작업이다.  
- 과제에서는 보통 `feature/작업명` branch에서 작업한 내용을 `main` branch에 합친다.  

### 🟡 초등학생 버전 설명  
- `main`은 반 친구들이 같이 보는 최종 공책이다.  
- `feature/son-git-merge-note`는 내가 혼자 연습한 공책이다.  
- 연습한 내용이 맞으면 최종 공책에 붙인다.  
- 이 붙이는 작업이 merge다.  

<br><br>

## 🟢 2. Merge를 하는 이유  

| 이유 | 설명 |
| --- | --- |
| 작업 반영 | 내 branch에서 만든 내용을 main에 넣기 위해 사용한다. |
| 협업 기록 | 누가 어떤 작업을 했는지 커밋 기록으로 남길 수 있다. |
| Pull Request 연결 | GitHub에서는 PR 리뷰 후 merge하는 흐름을 사용한다. |
| main 유지 | 검토가 끝난 작업만 main에 들어가게 할 수 있다. |

<br><br>

## 🟢 3. Merge 기본 흐름  

```txt
1. main에서 새 branch 생성
2. 새 branch에서 파일 수정
3. commit 생성
4. main으로 이동
5. git merge 새 branch
6. 결과 확인
```

<br><br>

## 🟢 4. 로컬에서 Merge 실습하기  

### 🟡 1단계: main으로 이동  
```bash
git switch main
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |
| --- | --- | --- |
| `git` | Git | 버전 관리 프로그램 이름이다. 공식 약자 풀네임은 없다. |
| `switch` | switch | 현재 작업 위치를 다른 branch로 바꾼다. |
| `main` | main branch | 최종본 기준 branch다. |

#### ⚫️ 왜 main으로 이동하는가  
- merge는 `내가 서 있는 branch`에 `다른 branch`를 합치는 명령이다.  
- `main`에 합치고 싶으면 먼저 `main`에 서 있어야 한다.  

<br>

### 🟡 2단계: 실습용 branch 만들고 이동  
```bash
git switch -c feature/son-merge-practice
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |
| --- | --- | --- |
| `-c` | create | branch를 만들면서 바로 이동한다. |
| `feature/son-merge-practice` | branch 이름 | merge 실습용 작업 branch다. |

<br>

### 🟡 3단계: 파일 만들기  
```bash
touch son_merge_practice.md
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |
| --- | --- | --- |
| `touch` | touch | 빈 파일을 만들거나 파일 수정 시간을 갱신한다. |
| `.md` | Markdown | 문서 작성용 파일 확장자다. |

<br>

### 🟡 4단계: 변경 파일 확인  
```bash
git status
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |
| --- | --- | --- |
| `status` | status | 현재 branch에서 어떤 파일이 바뀌었는지 보여준다. |

<br>

### 🟡 5단계: 커밋 만들기  
```bash
git add son_merge_practice.md
git commit -m "docs: add merge practice note"
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |
| --- | --- | --- |
| `add` | add | 커밋할 파일을 staging area에 올린다. |
| `commit` | commit | 변경 내용을 하나의 기록으로 저장한다. |
| `-m` | message | 커밋 메시지를 바로 입력한다. |
| `docs` | documentation | 문서 수정이라는 뜻의 커밋 타입이다. |

<br>

### 🟡 6단계: main으로 돌아가기  
```bash
git switch main
```

- 이제 `feature/son-merge-practice`의 내용을 `main`에 합칠 준비를 한다.  

<br>

### 🟡 7단계: merge 실행  
```bash
git merge feature/son-merge-practice
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |
| --- | --- | --- |
| `merge` | merge | 다른 branch의 커밋을 현재 branch에 합친다. |
| `feature/son-merge-practice` | branch 이름 | 현재 branch인 main에 합칠 대상이다. |

#### ⚫️ 핵심 문장  
- `git merge feature/son-merge-practice`의 뜻은 `현재 내가 서 있는 branch에 feature/son-merge-practice를 합쳐라`다.  

<br>

### 🟡 8단계: 결과 확인  
```bash
git log --oneline --graph --all
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |
| --- | --- | --- |
| `log` | log | 커밋 기록을 보여준다. |
| `--oneline` | one line | 커밋 하나를 한 줄로 짧게 보여준다. |
| `--graph` | graph | branch와 merge 흐름을 선으로 보여준다. |
| `--all` | all | 모든 branch의 기록을 보여준다. |

<br><br>

## 🟢 5. Merge의 대표 종류  

### 🟡 Fast-forward merge  
- Fast-forward는 `빨리 감기`라는 뜻이다.  
- main에 새 커밋이 없고, feature branch만 앞으로 나아갔을 때 발생한다.  
- Git은 main 포인터를 feature branch 위치로 그냥 앞으로 옮긴다.  

```txt
merge 전
main:    A
feature: A - B - C

merge 후
main:    A - B - C
```

<br>

### 🟡 3-way merge  
- 3-way merge는 세 지점을 비교해서 합치는 방식이다.  
- 세 지점은 `공통 조상 커밋`, `main의 최신 커밋`, `feature의 최신 커밋`이다.  
- main과 feature가 서로 다른 방향으로 커밋을 만들었을 때 사용된다.  

```txt
merge 전
          B - C  feature
        /
A - D - E        main

merge 후
          B - C
        /     \
A - D - E ----- M  main
```

### 🟡 Merge commit  
- 위 그림의 `M`이 merge commit이다.  
- merge commit은 두 branch를 합쳤다는 기록이다.  
- 협업 기록을 확인할 때 유용하다.  

<br><br>

## 🟢 6. GitHub에서 Merge하는 흐름  

### 🟡 과제에서 권장되는 실제 흐름  
```txt
feature/son-merge-practice
↓
git push -u origin feature/son-merge-practice
↓
GitHub Pull Request 생성
↓
팀원 리뷰
↓
Approve
↓
Merge pull request
↓
main 반영
```

### 🟡 branch를 GitHub에 올리는 명령어  
```bash
git push -u origin feature/son-merge-practice
```

#### ⚫️ 명령어 해설  
| 부분 | 풀네임 또는 뜻 | 설명 |
| --- | --- | --- |
| `git` | Git | 버전 관리 프로그램이다. 공식 약자 풀네임은 없다. |
| `push` | push | 내 컴퓨터의 커밋을 GitHub 같은 원격 저장소로 올린다. |
| `-u` | upstream | 로컬 branch와 원격 branch를 연결한다. |
| `origin` | origin | 원격 저장소의 기본 별명이다. 보통 GitHub 저장소를 뜻한다. |
| `feature/son-merge-practice` | branch 이름 | GitHub에 올릴 작업 branch 이름이다. |

### 🟡 GitHub 관련 단어  
| 단어 | 풀네임 또는 뜻 | 설명 |
| --- | --- | --- |
| `PR` | Pull Request | 내 branch를 main에 합쳐달라고 요청하는 기능이다. |
| `Approve` | approve | 리뷰어가 합쳐도 된다고 승인하는 것이다. |
| `Review` | review | 코드를 읽고 질문, 수정 요청, 승인 등을 남기는 것이다. |
| `origin` | origin | 원격 저장소의 기본 별명이다. 보통 GitHub 저장소를 뜻한다. |
| `push` | push | 내 컴퓨터 커밋을 GitHub로 올리는 것이다. |

<br><br>

## 🟢 7. Merge할 때 충돌이 나는 경우  

### 🟡 충돌이 자주 생기는 상황  
| 상황 | 설명 |
| --- | --- |
| 같은 파일 같은 줄 수정 | 두 사람이 같은 위치를 다르게 고치면 Git이 하나를 고르지 못한다. |
| 한쪽은 파일 이름 변경, 한쪽은 내용 수정 | 파일을 옮긴 것인지, 내용을 유지해야 하는지 Git이 판단하기 어렵다. |
| 한쪽은 삭제, 한쪽은 수정 | 삭제할지 수정본을 살릴지 사람이 결정해야 한다. |

### 🟡 충돌이 났을 때 기본 순서  
```txt
1. 당황하지 않는다.
2. git status로 충돌 파일을 확인한다.
3. 파일을 열고 충돌 마커를 찾는다.
4. 최종으로 남길 내용만 정리한다.
5. git add로 해결 완료 표시를 한다.
6. git commit으로 merge를 마무리한다.
```

### 🟡 충돌 확인 명령어  
```bash
git status
```

#### ⚫️ 출력 예시  
```txt
both modified: son_merge_practice.md
```

- `both modified`는 양쪽 branch가 같은 파일을 수정했다는 뜻이다.  
- 자세한 충돌 실습은 `03_git_conflict.md`에서 한다.  

<br><br>

## 🟢 8. Merge 전 확인 체크리스트  

| 확인 | 이유 |
| --- | --- |
| `git status`가 깨끗한가 | 커밋 안 된 작업이 있으면 merge 중 헷갈릴 수 있다. |
| 지금 branch가 main인가 | main에 합치려면 main에 서 있어야 한다. |
| 합칠 branch 이름이 맞는가 | 엉뚱한 branch를 merge하면 기록이 꼬인다. |
| 최신 main을 받았는가 | 오래된 main 기준이면 충돌 가능성이 커진다. |

<br><br>

## 🟢 9. 초보자 실수 정리  

| 실수 | 왜 문제인지 | 해결 |
| --- | --- | --- |
| feature branch에서 `git merge main`을 함 | main에 넣는 것이 아니라 feature에 main을 가져오는 것이다. | 내가 어디에 서 있는지 `git branch`로 확인한다. |
| main으로 이동하지 않고 merge함 | 원하는 곳이 아닌 branch에 합쳐질 수 있다. | `git switch main` 후 merge한다. |
| 충돌 파일을 대충 저장함 | 충돌 마커가 그대로 남을 수 있다. | `<<<<<<<`, `=======`, `>>>>>>>`가 없는지 확인한다. |
| 리뷰 없이 main에 직접 merge함 | 과제의 PR 기반 협업 요구사항을 못 맞춘다. | GitHub에서 Pull Request를 만들고 리뷰받는다. |

<br><br>

## 🟢 10. 한 줄 요약  

- Merge는 작업 branch의 커밋을 main에 합치는 작업이다.  
- merge 명령은 `현재 내가 서 있는 branch`에 `다른 branch`를 합친다.  
- 과제에서는 로컬 merge보다 GitHub Pull Request를 통한 merge 기록이 더 중요하다.  
