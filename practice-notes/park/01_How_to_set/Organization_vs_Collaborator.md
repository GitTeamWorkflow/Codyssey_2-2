# GitHub Organization과 Collaborator 비교

## 1. 개요

GitHub에서 팀원과 함께 저장소를 관리하는 방식은 크게 두 가지가 있다.

* GitHub Organization 저장소 생성
* 개인 저장소 생성 후 Collaborator 초대

두 방식 모두 팀 협업이 가능하지만, 저장소의 소유 주체와 권한 관리 방식에서 차이가 있다.

---

## 2. Organization이란?

Organization은 개인 계정과 별도로 생성하는 팀 또는 조직 단위의 작업 공간이다.

예시:

```text
GitTeamWorkflow
└── Codyssey_2-2
```

위 구조에서 `GitTeamWorkflow`는 Organization이고, `Codyssey_2-2`는 Organization 소유의 저장소이다.

Organization에서는 여러 명의 팀원을 초대하고, 저장소별 또는 팀별로 권한을 부여할 수 있다.

---

## 3. Collaborator란?

Collaborator는 개인 계정 소유의 저장소에 초대된 협업자를 의미한다.

예시:

```text
PBK98
└── Codyssey_2-2
```

위 구조에서 `Codyssey_2-2`는 `PBK98` 개인 계정 소유의 저장소이고, 다른 팀원들은 Collaborator로 초대되어 작업할 수 있다.

---

## 4. Organization과 Collaborator의 차이

| 구분         | Organization                   | Collaborator             |
| ---------- | ------------------------------ | ------------------------ |
| 소유 주체      | 팀 또는 조직                        | 개인 계정                    |
| 저장소 주소 예시  | `GitTeamWorkflow/Codyssey_2-2` | `PBK98/Codyssey_2-2`     |
| 초대 방식      | Organization 멤버 또는 팀으로 초대      | 개인 저장소에 Collaborator로 초대 |
| 권한 관리      | 조직, 팀, 저장소 단위로 관리 가능           | 저장소 단위로 개별 관리            |
| 여러 저장소 관리  | 여러 저장소를 체계적으로 관리하기 좋음          | 저장소가 많아지면 관리가 불편함        |
| 팀 프로젝트 적합성 | 높음                             | 가능하지만 개인 저장소 중심          |
| 설정 난이도     | 비교적 복잡함                        | 간단함                      |
| 과제 권장 여부   | 권장                             | 대체 가능                    |

---

## 5. Organization의 장점

| 장점            | 설명                                                    |
| ------------- | ----------------------------------------------------- |
| 팀 단위 관리 가능    | 저장소가 개인 계정이 아니라 팀 공간에 소속된다.                           |
| 권한 관리가 체계적    | Owner, Member, Team, Repository Role 등으로 권한을 나눌 수 있다. |
| 여러 저장소 관리에 유리 | 프로젝트가 여러 개일 때 저장소를 한 공간에서 관리할 수 있다.                   |
| 팀별 권한 부여 가능   | 문서팀, 개발팀, 리뷰팀처럼 팀 단위로 권한을 설정할 수 있다.                   |
| 팀 프로젝트에 적합    | 개인 소유가 아닌 팀 소유 저장소로 관리할 수 있다.                         |

---

## 6. Organization의 단점

| 단점              | 설명                                                    |
| --------------- | ----------------------------------------------------- |
| 설정이 복잡할 수 있음    | Organization 생성, 멤버 초대, 저장소 권한 설정이 필요하다.              |
| 초대 수락 필요        | 팀원이 Organization 초대를 수락해야 정상적으로 협업할 수 있다.             |
| 권한 설정 실수 가능     | Organization 멤버여도 repository 권한이 없으면 push가 불가능할 수 있다. |
| 단순 과제에는 과할 수 있음 | 저장소 하나만 사용하는 작은 과제에서는 설정이 다소 복잡할 수 있다.                |

---

## 7. Collaborator의 장점

| 장점        | 설명                            |
| --------- | ----------------------------- |
| 설정이 간단함   | 개인 저장소에 팀원만 초대하면 바로 협업할 수 있다. |
| 빠르게 시작 가능 | Organization을 만들 필요가 없다.      |
| 작은 팀에 적합  | 2~3명 정도의 짧은 프로젝트나 실습에 적합하다.   |
| 관리 항목이 적음 | 팀, 조직, 역할 설정을 따로 관리하지 않아도 된다. |

---

## 8. Collaborator의 단점

| 단점                 | 설명                             |
| ------------------ | ------------------------------ |
| 저장소가 개인 계정에 종속됨    | 프로젝트가 특정 개인 계정 소유로 남는다.        |
| 팀 단위 관리가 약함        | 팀별 권한 설정이나 여러 저장소 관리가 어렵다.     |
| 권한 관리가 단순함         | Organization보다 세밀한 권한 관리가 어렵다. |
| 프로젝트 소유권이 애매할 수 있음 | 팀 프로젝트이지만 개인 저장소처럼 보일 수 있다.    |

---

## 9. 권한 관리 차이

### Organization 방식

Organization에서는 다음과 같은 방식으로 권한을 관리할 수 있다.

* Organization Owner
* Organization Member
* Repository Admin
* Repository Maintain
* Repository Write
* Repository Read
* Team 단위 권한

예시:

```text
GitTeamWorkflow Organization
├── Owner: 팀장
├── Member: 팀원
└── Repository: Codyssey_2-2
    ├── Admin: 저장소 관리자
    └── Write: 일반 팀원
```

### Collaborator 방식

개인 저장소에서는 저장소 소유자가 팀원을 Collaborator로 직접 초대한다.

예시:

```text
PBK98/Codyssey_2-2
├── Owner: PBK98
└── Collaborator: 팀원 A, 팀원 B
```

---

## 10. 과제 기준 추천 방식

과제 조건은 다음과 같다.

```text
옵션 A(권장): GitHub Organization 저장소 생성
옵션 B(대체): 개인 저장소 생성 후 Collaborator 초대
```

과제에서 Organization 방식을 권장하는 이유는 팀 프로젝트 구조를 더 명확하게 보여줄 수 있기 때문이다.

따라서 본 프로젝트에서는 다음 방식을 사용하는 것이 적절하다.

```text
Organization: GitTeamWorkflow
Repository: Codyssey_2-2
기본 브랜치: main
팀장 권한: Admin
팀원 권한: Write
협업 방식: Pull Request 기반 협업
```

---

## 11. 팀 과제에서의 권한 추천

| 역할            | 추천 권한 | 설명                                       |
| ------------- | ----- | ---------------------------------------- |
| 팀장 또는 저장소 관리자 | Admin | 저장소 설정, Branch Protection Rule, 권한 관리 가능 |
| 일반 팀원         | Write | 브랜치 push, Pull Request 생성, 리뷰 참여 가능      |
| 문서 확인만 필요한 사람 | Read  | 저장소 읽기만 가능                               |

일반 팀원은 `Write` 권한이면 충분하다.

`Admin` 권한은 저장소 설정을 변경할 수 있으므로 필요한 사람에게만 부여하는 것이 좋다.

---

## 12. 최종 정리

| 항목     | Organization               | Collaborator         |
| ------ | -------------------------- | -------------------- |
| 핵심 개념  | 팀 단위 저장소 관리 방식             | 개인 저장소에 협업자 초대       |
| 적합한 상황 | 팀 프로젝트, 장기 프로젝트, 여러 저장소 관리 | 개인 과제, 단기 협업, 간단한 실습 |
| 장점     | 체계적인 권한 관리, 팀 소유 구조        | 설정이 간단하고 빠름          |
| 단점     | 설정이 비교적 복잡함                | 개인 계정에 종속됨           |
| 과제 추천  | 권장                         | 대체 가능                |

본 프로젝트에서는 팀 단위 협업과 권한 관리를 명확히 하기 위해 GitHub Organization 방식을 사용한다.
