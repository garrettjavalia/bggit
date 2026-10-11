# 되돌리기: 커밋 취소하기 {#revert}

[i[Revert]<]

변경 사항을 만들어 커밋했는데 그 때문에 모든 것이 엉망이 됐다고 합시다.
파일을 이전 버전으로 되돌리고 싶습니다.

이미 떠올렸을 법한 조금 촌스러운 방법이 있습니다. 파일이 원하는 모습이던
이전 커밋으로 HEAD를 분리하고, 파일을 안전한 곳에 복사한 다음, HEAD를 다시
`main`에 연결하고 이전 파일을 작업 트리의 기존 파일 위에 복사합니다. 그리고
추가하고 커밋하면 됩니다! 실제로 작동하는 방법이긴 합니다...

하지만 더 정석적인 방법인 `git revert`를 사용해 봅시다.

되돌리기를 사용하면 현재 상태를 만든 바로 직전 커밋이 아니더라도 커밋 하나의
변경 사항을 실제로 취소할 수 있습니다. 이를테면 커밋을 30개 만들었는데 네
번째 커밋은 더 이상 원하지 않는다는 사실을 알게 됐다고 합시다. 그 커밋만
되돌릴 수 있습니다!

일반적인 되돌리기는 새 커밋을 만들며 이전 커밋을 지우지 않습니다. 따라서
기록을 다시 쓰지 않으므로 이미 push한 커밋도 이 방법으로 안전하게 되돌릴 수
있습니다.

## 되돌리기 실행하기 {#performing-the-revert}

상당히 간단합니다. 로그를 거슬러 올라가 되돌리려는 커밋 ID를 찾고 되돌리면
됩니다.

예를 들어 로그에 다음 내용이 있다고 합시다.

``` {.default}
commit 9fef4fe6d42b91c12b5217829e8d98d738f84d61
Author: Brian "Beej Jorgensen" Hall <beej@beej.us>
Date:   Fri Jul 26 16:59:44 2024 -0700

    Added Line 50
```

이 커밋을 더 이상 원하지 않는다면 커밋 ID로 되돌릴 수 있습니다. 해시의 앞
몇 글자만으로 충분하므로 여기서는 그 부분만 입력하겠습니다.

``` {.default}
$ git revert 9fef4
  Auto-merging foo.txt
  [main de415f4] Revert "Added Line 50"
   1 file changed, 1 deletion(-)
```

이 예시에서는 충돌이 없으므로(아래에서 더 설명합니다) 편집기가 열리고 커밋
메시지를 수정할 수 있습니다. 되돌리기는 새 커밋을 만든다는 사실을 기억하세요!

``` {.default .numberLines}
Revert "Added Line 50"

This reverts commit 9fef4fe6d42b91c12b5217829e8d98d738f84d61.
```

파일을 저장하면 `git status`는 깨끗한 상태라고 알려 줍니다.

다시 `git log`를 실행하면 되돌리기 커밋이 보입니다.

``` {.default}
$ git log
  commit de415f4f0cd645b1e551a6ac56e13f73850c88db (HEAD -> main)
  Author: Brian "Beej Jorgensen" Hall <beej@beej.us>
  Date:   Fri Jul 26 17:01:54 2024 -0700

    Revert "Added Line 50"

    This reverts commit 9fef4fe6d42b91c12b5217829e8d98d738f84d61.
```

어떤 커밋이든 되돌릴 수 있으며, 심지어 그 자체가 되돌리기인 커밋도 가능합니다!
되돌리기를 되돌리는 것이죠!

지금은 되돌리기가 순조롭게 진행된 예시였습니다. 하지만 되돌릴 커밋 이후에 그
커밋의 변경 지점과 가까운 곳을 수정했다면 어떨까요? 충돌할 수 있을까요?
물론입니다!

## 되돌리기 충돌 {#revert-conflicts}

[i[Revert-->Conflicts]<]

병합이나 리베이스와 마찬가지로 되돌리기에서도 충돌이 생길 수 있습니다. 충돌
해결에 익숙하지 않다면 되돌리기 충돌과 가장 비슷한 [리베이스
충돌](#rebasing-conflicts) 절을 다시 살펴보세요.

예를 들어 코드의 37번째 줄을 변경한 다음, 역시 37번째 줄을 변경했던 커밋을
되돌리면 Git은 어떻게 해야 할지 판단할 수 없습니다. 내 커밋 이전 상태로
되돌려야 할까요, 아니면 더 앞선 그 커밋 이전 상태로 되돌려야 할까요?

따라서 해결해야 할 되돌리기 충돌이 생깁니다. 해결 방식은 이미 살펴본 다른
충돌과 매우 비슷합니다.

되돌리기를 시도하다 충돌이 생기면 다음과 같은 메시지가 나옵니다.

``` {.default}
$ git revert 5af89a8985c001ec02409d77e093fb7be45495ff
  Auto-merging foo.txt
  CONFLICT (content): Merge conflict in foo.txt
  error: could not revert 5af89a8... Added Line 69
  hint: After resolving the conflicts, mark them with
  hint: "git add/rm <pathspec>", then run
  hint: "git revert --continue".
  hint: You can instead skip this commit with "git revert --skip".
  hint: To abort and get back to the state before "git revert",
  hint: run "git revert --abort".
  hint: Disable this message with
  hint: "git config advice.mergeConflict false"
```

여기서 선택할 수 있는 몇 가지 방법도 알려 줍니다. 우리의 친구 `git
status`로 더 많은 정보를 얻을 수 있습니다.

``` {.default}
$ git status
  On branch main
  You are currently reverting commit 5af89a8.
    (fix conflicts and run "git revert --continue")
    (use "git revert --skip" to skip this patch)
    (use "git revert --abort" to cancel the revert operation)

  Unmerged paths:
    (use "git restore --staged <file>..." to unstage)
    (use "git add <file>..." to mark resolution)
	  both modified:   foo.txt

  no changes added to commit (use "git add" and/or "git commit -a")
```

여기서는 다음 중 하나를 할 수 있습니다.

* 파일을 편집해 충돌을 해결하고 `git add`한 뒤 `git revert --continue`로
  되돌릴 다음 커밋이 있다면 그 커밋으로 넘어갑니다.
* `git revert --abort`로 작업을 완전히 중단합니다.
* `git revert --skip`으로 이 특정 커밋의 되돌리기를 건너뜁니다. 되돌리려던
  모든 커밋을 건너뛰면 중단한 것과 같습니다.

충돌을 해결하면 이전과 마찬가지로 새 커밋의 커밋 메시지를 입력하게 됩니다.

[i[Revert-->Conflicts]>]

## 여러 커밋 되돌리기 {#reverting-multiple-commits}

[i[Revert-->Multiple commits]<]

명령줄에서 되돌릴 커밋을 여러 개 한꺼번에 지정할 수 있습니다.

다음은 커밋 두 개를 되돌리는 예시입니다.

``` {.default}
$ git revert 4c0b3 81d2a
  Auto-merging foo.txt
  [main ab3169d] Revert "Added Line 50"
   1 file changed, 1 deletion(-)
  Auto-merging foo.txt
  [main b63f003] Revert "Added Line 10"
   1 file changed, 1 deletion(-)
```

그러면 새 되돌리기 커밋 두 개가 생깁니다. 되돌리기 과정에서 커밋 메시지도 두
번 편집합니다.

커밋 범위를 지정할 수도 있습니다. 반드시 오래된 것부터 최신 것 순서로
지정하세요. 그렇지 않으면 `empty commit set passed` 오류가 발생합니다.

``` {.default}
$ git revert 4c0b3^..81d2a
  Auto-merging foo.txt
  [main ab3169d] Revert "Added Line 50"
   1 file changed, 1 deletion(-)
  Auto-merging foo.txt
  [main b63f003] Revert "Added Line 10"
   1 file changed, 1 deletion(-)
```

이 경우에도 되돌리기마다 하나씩 많은 커밋이 생깁니다. 원한다면 [이 커밋들을
스쿼시](#squashing-commits)할 수 있고, `-n`("커밋 없음")을 사용해 준비될
때까지 Git이 커밋하지 않게 할 수도 있습니다.

``` {.default}
$ git revert -n ee71e 123e8
  Auto-merging foo.txt
  Auto-merging foo.txt
```

이 시점에는 두 커밋이 되돌려진 상태로 파일이 스테이징돼 있습니다. 이제 이
두 변경을 담은 커밋 하나를 만들 수 있습니다. 범위를 지정했을 때도 같은
방법을 쓸 수 있습니다.

물론 충돌이 생길 수도 있고, 이미 살펴본 그 무척 재미있는 방법으로 해결해야
합니다.

[i[Revert-->Multiple commits]>]
[i[Revert]>]
