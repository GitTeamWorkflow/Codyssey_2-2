## reset

`reset`은 현재 브랜치의 HEAD를 특정 commit으로 되돌리는 명령어이다.

예시:

```bash
git reset --soft HEAD~1
```

직전 commit을 취소하지만 변경 파일은 staging area에 남긴다.

```bash
git reset --mixed HEAD~1
```

직전 commit을 취소하고 변경 파일은 working directory에 남긴다.

```bash
git reset --hard HEAD~1
```

직전 commit과 변경 내용을 모두 삭제한다.

주의할 점은 `reset --hard`는 작업 내용을 잃을 수 있으므로 신중하게 사용해야 한다는 것이다.

### 사용 상황

- 아직 push하지 않은 commit을 정리할 때
- 잘못 만든 commit을 로컬에서 수정할 때
- commit 메시지나 commit 단위를 다시 정리할 때

공유 브랜치나 이미 push한 commit에는 함부로 사용하지 않는 것이 좋다.

---

# `HEAD`와 `git reset --soft HEAD~1` 정리

## 1. HEAD란?

Git에서 `HEAD`는 **현재 내가 위치한 커밋**을 의미한다.

보통 브랜치에 checkout 되어 있다면, `HEAD`는 현재 브랜치의 최신 커밋을 가리킨다.

예시:

```text
A --- B --- C
          ↑
         main
          ↑
         HEAD
```

위 상태에서 현재 브랜치가 `main`이고 최신 커밋이 `C`라면 다음과 같다.

```text
HEAD = C
```

즉, `HEAD`는 현재 브랜치가 가리키는 최신 커밋이다.

---

## 2. `HEAD~1`의 의미

`HEAD~1`은 **현재 HEAD에서 한 단계 이전 커밋**을 의미한다.

예시:

```text
A --- B --- C
     ↑     ↑
  HEAD~1  HEAD
```

각 표현의 의미는 다음과 같다.

| 표현       | 의미                |
| -------- | ----------------- |
| `HEAD`   | 현재 커밋             |
| `HEAD~1` | 현재 커밋의 바로 이전 커밋   |
| `HEAD~2` | 현재 커밋의 두 단계 이전 커밋 |
| `HEAD~3` | 현재 커밋의 세 단계 이전 커밋 |

예를 들어 커밋 기록이 다음과 같다고 가정한다.

```text
commit3  [Docs] README 수정
commit2  [Feat] HTML 파일 추가
commit1  Initial commit
```

현재 최신 커밋이 `commit3`이라면 다음과 같다.

```text
HEAD = commit3
HEAD~1 = commit2
HEAD~2 = commit1
```

---

## 3. 동작 예시

현재 커밋 기록이 다음과 같다고 가정한다.

```text
A --- B --- C
          ↑
         HEAD
```

여기서 다음 명령어를 실행한다.

```bash
git reset --soft HEAD~1
```

실행 후 커밋 위치는 다음과 같이 바뀐다.

```text
A --- B
     ↑
    HEAD
```

하지만 `C` 커밋에서 변경했던 파일 내용은 삭제되지 않는다.

변경사항은 staging area에 남아 있으므로 바로 다시 commit할 수 있다.

```bash
git commit -m "새로운 커밋 메시지"
```

---

## 4. HEAD를 확인하는 방법

현재 커밋 기록을 확인하려면 다음 명령어를 사용한다.

```bash
git log --oneline
```

예시 출력:

```text
a3c91f2 [Docs] README 수정
b7e12aa [Feat] HTML 파일 추가
c812e1d Initial commit
```

이 상태에서 각 위치는 다음과 같다.

```text
HEAD = a3c91f2
HEAD~1 = b7e12aa
HEAD~2 = c812e1d
```

브랜치 흐름까지 함께 보고 싶다면 다음 명령어를 사용한다.

```bash
git log --oneline --graph --decorate --all
```

---

## 5. `HEAD^`와 `HEAD~1`의 차이

일반적인 직선 커밋 이력에서는 `HEAD^`와 `HEAD~1`은 거의 같은 의미이다.

| 표현       | 의미                |
| -------- | ----------------- |
| `HEAD^`  | HEAD의 부모 커밋       |
| `HEAD~1` | HEAD에서 한 단계 이전 커밋 |
| `HEAD^^` | 부모의 부모 커밋         |
| `HEAD~2` | HEAD에서 두 단계 이전 커밋 |

일반적인 상황에서는 다음 두 표현이 같은 커밋을 가리킨다.

```text
HEAD^ = HEAD~1
```

다만 merge commit처럼 부모 커밋이 여러 개인 경우에는 `HEAD^1`, `HEAD^2`처럼 구분할 수 있다.

---

## 6. reset 옵션 비교

`git reset`은 옵션에 따라 변경사항을 남기는 방식이 다르다.

| 명령어                        | 커밋 취소 | 변경 파일 유지 | staging 유지 |
| -------------------------- | ----: | -------: | ---------: |
| `git reset --soft HEAD~1`  |     O |        O |          O |
| `git reset --mixed HEAD~1` |     O |        O |          X |
| `git reset --hard HEAD~1`  |     O |        X |          X |

---

## 7. `--soft`

```bash
git reset --soft HEAD~1
```

### 의미

* 마지막 commit을 취소한다.
* 파일 변경사항은 유지한다.
* staging 상태도 유지한다.

### 사용 상황

* commit 메시지를 다시 작성하고 싶을 때
* 방금 만든 commit을 취소하고 다시 commit하고 싶을 때
* commit은 취소하되 변경 내용은 그대로 두고 싶을 때

---

## 8. `--mixed`

```bash
git reset --mixed HEAD~1
```

### 의미

* 마지막 commit을 취소한다.
* 파일 변경사항은 유지한다.
* staging 상태는 해제된다.

즉, `git add` 하기 전 상태로 돌아간다.

### 사용 상황

* commit을 취소하고 변경 파일을 다시 골라서 add하고 싶을 때
* 실수로 너무 많은 파일을 한 commit에 넣었을 때

---

## 9. `--hard`

```bash
git reset --hard HEAD~1
```

### 의미

* 마지막 commit을 취소한다.
* 파일 변경사항도 삭제한다.
* staging 상태도 삭제한다.

### 주의

`--hard`는 변경 내용을 완전히 없앨 수 있으므로 신중하게 사용해야 한다.

### 사용 상황

* 마지막 commit과 변경사항을 완전히 버리고 싶을 때
* 로컬 작업 내용을 모두 되돌려도 괜찮을 때

---

## 10. 정리

```bash
git reset --soft HEAD~1
```

은 다음과 같은 명령어이다.

```text
현재 HEAD에서 한 커밋 이전으로 되돌린다.
단, 마지막 commit의 변경 내용은 staged 상태로 유지한다.
```

즉, 마지막 commit만 취소하고 파일 변경사항은 그대로 남기고 싶을 때 사용한다.

가장 핵심적인 의미는 다음과 같다.

```text
HEAD = 현재 커밋
HEAD~1 = 바로 이전 커밋
git reset --soft HEAD~1 = 마지막 commit만 취소하고 변경사항은 유지
```
