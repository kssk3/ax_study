# Git 이력·브랜치·병합 TIL

## 1. 커밋과 브랜치

- 커밋은 작업의 의미 있는 스냅샷이며, 메시지에는 무엇을 바꿨는지 적는다.
- 브랜치는 특정 커밋을 가리키는 이름표다. 기능 작업은 별도 브랜치에서 진행한 뒤 병합한다.

```bash
git status
git switch -c feature/payment
git add PAYMENT.md
git commit -m "feat: 결제 양식 추가"
```

> ⭐️ `git add .` 전에 `git status`로 커밋에 들어갈 파일을 확인한다.

## 2. 병합과 충돌

현재 브랜치에 다른 브랜치를 합친다.

```bash
git switch main
git merge feature/payment
```

같은 부분을 다르게 수정하면 충돌이 난다. 충돌 표시를 정리한 뒤 `git add`와 `git commit`으로 해결한다.

```bash
git status
git add ORDER.md
git commit
```

> ⭐️ `<<<<<<<`, `=======`, `>>>>>>>` 표시가 남지 않았는지 확인한다. 병합을 취소하려면 `git merge --abort`를 사용한다.

## 3. 원격 저장소 확인

`origin/main`은 원격 `main`의 마지막 확인 위치를 가리킨다. `fetch`는 원격 이력만 가져오며 작업 파일은 바꾸지 않는다.

```bash
git fetch origin
git log --oneline main..origin/main
git push -u origin main
```

## 4. 자주 쓰는 점검 명령

```bash
git status
git branch -vv
git log --oneline --graph --decorate --all
```
