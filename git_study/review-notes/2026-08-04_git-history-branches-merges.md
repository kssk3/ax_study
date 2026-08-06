# Git 이력·브랜치·병합 복습

## 1. 전체 흐름

이번 프로젝트들은 작은 기능을 커밋으로 남기고, 기능 브랜치에서 작업한 뒤 `main`에 병합하거나 원격 저장소와 동기화하는 흐름을 연습한다.

1. 기능 단위로 파일을 만들거나 수정한 뒤 커밋한다.
2. 독립적인 작업은 브랜치에서 진행한다.
3. 브랜치의 변경을 병합하고, 같은 줄을 서로 수정했다면 충돌을 해결해 병합 커밋을 만든다.
4. 필요하면 원격 브랜치(`origin/main`)와 연결해 이력을 공유한다.

## 2. 커밋: 작업의 의미 있는 기록

`study`에서는 회원가입 → 로그인 → 탈퇴 → 상품 등록 → 결제 양식처럼 기능이 커밋 단위로 쌓였다. 커밋 메시지는 **무엇을 왜 바꿨는지** 드러나야 이후 `git log`에서 이력을 읽기 쉽다.

```bash
git status
git add MEMBER.md PRODUCT.md
git commit -m "feat: 상품 등록 기능 추가"
git log --oneline --graph --all
```

| 상태 | 의미 |
| --- | --- |
| 작업 디렉터리 | 아직 Git 기록에 넣지 않은 파일 변경 |
| 스테이징 영역 | 다음 커밋에 포함할 변경을 고른 상태 |
| 커밋 | 특정 시점의 확정된 스냅샷 |

> ⭐️ `git add .`는 의도하지 않은 파일까지 포함할 수 있다. 특히 실습 중에는 `git status`로 먼저 포함 대상을 확인한다.

## 3. 브랜치: 기능 작업을 분리하는 포인터

`study`에는 `main`, `order`, `payment` 브랜치가 있고, `payment`는 `order`의 복원 커밋을 기반으로 결제 양식을 추가했다. `study3`도 `feature/order`에서 장바구니 기능을 이어서 작업한다. 브랜치는 파일을 복사하는 것이 아니라 **특정 커밋을 가리키는 이름표**다.

```bash
git switch -c feature/payment
# 결제 기능 작업 후
git add PAYMENT.md
git commit -m "feat: 결제 양식 추가"

git switch main
git merge feature/payment
```

```text
main     A---B
             \
feature       C---D

병합 후  A---B-------M  main
              \     /
               C---D    feature
```

> ⭐️ 병합하기 전에 반드시 대상 브랜치로 이동한다. `git merge feature/payment`는 **현재 브랜치에** `feature/payment`를 합친다.

## 4. 병합과 충돌 해결

`study2`는 주문서 양식을 기능 브랜치에서 병합한 뒤, `main`과 주문 브랜치가 모두 `ORDER.md`를 수정하면서 최종 병합 커밋 `fix: 충돌 해결`을 만들었다. 서로 다른 파일을 수정하면 대개 자동 병합되지만, 같은 부분을 다르게 수정하면 Git이 선택할 수 없어 충돌 상태가 된다.

```bash
git switch main
git merge feature/order-form

# 충돌이 나면 파일의 <<<<<<<, =======, >>>>>>> 표시를 수정한다.
git status
git add ORDER.md
git commit
```

| 상황 | 할 일 |
| --- | --- |
| 자동 병합 성공 | 결과를 확인하고 테스트한다 |
| 충돌 발생 | 표시 구간을 읽고 원하는 최종 내용으로 직접 수정한다 |
| 병합을 취소하고 싶음 | `git merge --abort` |

> ⭐️ 충돌 표시는 해결 과정의 임시 텍스트다. `<<<<<<<`, `=======`, `>>>>>>>`가 남은 채 커밋되지 않았는지 반드시 확인한다.

## 5. 원격 저장소와 추적 브랜치

`study4`, `study5`, `study6`에는 `origin/main`이 보인다. 로컬 `main`은 내 컴퓨터의 브랜치이고, `origin/main`은 마지막으로 확인한 원격 `main`의 위치를 가리키는 추적 브랜치다.

```bash
git remote -v
git fetch origin
git log --oneline main..origin/main   # 원격에만 있는 커밋
git push -u origin main               # 첫 push에서 추적 관계 설정
```

> ⭐️ `git fetch`는 원격 이력만 내려받고 작업 파일은 바꾸지 않는다. 작업 파일까지 병합하려면 `merge`, `rebase`, 또는 `pull`이 추가로 필요하다.

## 6. 반복된 이력에서 얻을 핵심

여러 저장소에 반복된 `문서 기록` 커밋과 같은 시작 이력은 한 번의 개념으로 정리하면 충분하다. 복습할 때는 커밋 제목 자체보다 다음 질문에 답할 수 있는지 확인한다.

1. 지금 내가 있는 브랜치와 각 브랜치가 가리키는 커밋은 무엇인가?
2. 이 변경을 별도 브랜치에서 해야 하는가, 현재 브랜치에서 해도 되는가?
3. 병합 충돌이 났을 때 어느 변경을 남길지 이해하고 검증했는가?
4. 로컬 브랜치와 `origin/<branch>`의 차이를 확인했는가?

```bash
# 실습 전·후에 자주 쓰는 점검 명령
git status
git branch -vv
git log --oneline --graph --decorate --all
```
