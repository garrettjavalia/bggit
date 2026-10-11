# 참조 로그, "reflog" {#the-reference-log-reflog}

[i[Reflog]<]

그동안 여러분은 커밋하고 브랜치를 만들며 온갖 일을 해 왔습니다. Git은 빅
브라더처럼 여러분을 지켜보고 엿들으며 하는 일을 모두 기록했습니다.

그리고 이 기록을 유용하게 활용할 수 있습니다.

현재 브랜치를 버리고 싶어서 하드 리셋 같은 작업을 했다고 합시다.

그런데 잠깐! 방금 리셋으로 지나쳐 버린 커밋 중 하나에서 무언가가 실제로
필요했습니다! 다시 돌아갈 방법이 있을까요? 그곳에는 브랜치도 없고 커밋 ID도
기억나지 않습니다. 어느 것의 조상 커밋도 아니므로 `git log`도 도움이 되지
않습니다.

어떻게 되찾을 수 있을까요?

구원투수 `git reflog`가 나설 차례입니다!

reflog에는 여러분이 한 온갖 작업과 커밋 ID가 기록되며, 이 기록은 90일 동안
보관됩니다[^9721]. 그 시간이 지나면 고아 커밋(즉, 그 위에 브랜치가 없는
커밋)은 가비지 컬렉션의 대상이 됩니다.

[^9721]: 기본값은 90일입니다. `gc.reflogExpire` 구성 옵션으로 변경할 수
    있습니다.

## 어디에 쓸 수 있을까요? {#what-can-we-use-it-for}

[i[Reflog-->Uses]]

온갖 일에 사용할 수 있습니다.

* 고아 커밋 살펴보기
* 삭제한 브랜치 다시 만들기
* 잘못된 리셋에서 복구하기
* 다른 브랜치의 작업까지 포함해 저장소에서 작업한 순서 살펴보기
* 그 밖에도 많습니다!

기본적으로 저장소의 선형 작업 기록을 돌아볼 방법을 제공하고, 그 과정에 있던
커밋 해시를 알려 줍니다.

따라서 저장소를 이전 상태로 하드 리셋하고 싶다면 reflog에서 그 이전 커밋을
찾을 수 있습니다[^ab30].

[^ab30]: 물론 push한 것의 기록은 절대로 다시 쓰지 말아야 한다는 점을
    기억하세요.

## 고아 커밋 돌아보기 {#looking-back-at-an-orphan-commit}

[i[Reflog-->Finding an orphan commit]<]

다음 작업을 수행하는 예시를 실행해 봅시다.

1. `main` 브랜치에서 `foo.txt` 파일을 커밋합니다.
2. 새 브랜치 `topic1`을 만듭니다.
3. 새 브랜치에서 다른 파일 `bar.txt`를 추가하고 커밋합니다.
4. `bar.txt`를 수정하고 그 변경을 커밋합니다.
5. 이제 `topic1`을 포기하기로 합니다. `main` 브랜치로 돌아가 `topic1`을
   강제로 삭제합니다.
6. 그런데 어떤 이유로 `topic1`의 그 커밋을 다시 봐야겠다고 마음을 바꿉니다.
   하지만 브랜치를 삭제했습니다. 이런.
7. reflog에서 원하는 `topic1`의 커밋을 찾습니다.
8. 그 커밋으로 전환합니다(`HEAD`를 분리합니다).

Git에서 실행하면 다음과 같습니다. 적어도 처음 다섯 단계까지는요.

``` {.default}
$ echo 'Line 1' > foo.txt                  # Create foo.txt
$ git add foo.txt
$ git commit -m 'added foo.txt'
  [main (root-commit) 90bd7cc] added foo.txt
   1 file changed, 1 insertion($)
   create mode 100644 foo.txt
$ git switch -c topic1                     # Switch to topic1
  Switched to a new branch 'topic1'
$ echo 'Line 1' > bar.txt                  # Create bar.txt
$ git add bar.txt
$ git commit -m 'added bar.txt'
  [topic1 4219f83] added bar.txt
   1 file changed, 1 insertion($)
   create mode 100644 bar.txt
$ echo 'Line 2' >> bar.txt                 # Modify bar.txt
$ git add bar.txt
$ git commit -m 'appended to bar.txt'
  [topic1 bf8b8cf] appended to bar.txt
   1 file changed, 1 insertion($)
$ git switch -                             # Switch back to main
  Switched to branch 'main'
$ git branch -D topic1                     # Delete topic1
  Deleted branch topic1 (was bf8b8cf).
```

이 시점에서 `bar.txt`에 만든 커밋을 다시 보고 싶다고 합시다. `git log`로
한번 잘 찾아보세요!

``` {.default}
$ git log
commit 90bd7cc6c3c530798872827ba02cb7db4fd422c2 (HEAD -> main)
Author: User <user@example.com>
Date:   Fri Oct 4 16:24:56 2024 -0700

    added foo.txt
```

이게 전부인가요? `bar.txt` 관련 내용은 다 어디 갔죠? 그 내용은 이 커밋
`90bd7`의 자손인 `topic1` 커밋에 있었습니다. `git log`는 조상만 보여
주므로 `bar.txt` 변경 사항은 하나도 보이지 않습니다.

드디어 이 장의 주제인 reflog에 도착했습니다. 한번 들여다봅시다.

``` {.default}
$ git reflog
  90bd7cc (HEAD -> main) HEAD@{0}: checkout: moving from topic1 to
                                             main
  bf8b8cf HEAD@{1}: commit: appended to bar.txt
  4219f83 HEAD@{2}: commit: added bar.txt
  90bd7cc (HEAD -> main) HEAD@{3}: checkout: moving from main to
                                             topic1
  90bd7cc (HEAD -> main) HEAD@{4}: commit (initial): added foo.txt
```

그래, 바로 이거죠! `bar.txt`에 만든 변경 사항이 보입니다! 왼쪽에는 커밋
해시도 있습니다! 그러면 그 커밋으로 전환할 수 있습니다!

``` {.default}
$ git switch --detach bf8b8cf
  HEAD is now at bf8b8cf appended to bar.txt
$ git log
  commit bf8b8cf826bbf667cdd088cfcecbc1086c24de3b (HEAD)
  Author: Brian "Beej Jorgensen" Hall <beej@beej.us>
  Date:   Fri Oct 4 16:24:56 2024 -0700

      appended to bar.txt

  commit 4219f83f22f8a90cb8d57128501facb58b292003
  Author: Brian "Beej Jorgensen" Hall <beej@beej.us>
  Date:   Fri Oct 4 16:24:56 2024 -0700

      added bar.txt

  commit 90bd7cc6c3c530798872827ba02cb7db4fd422c2 (main)
  Author: Brian "Beej Jorgensen" Hall <beej@beej.us>
  Date:   Fri Oct 4 16:24:56 2024 -0700

      added foo.txt
```

로그가 있군요... 파일 내용도 있을까요?

``` {.default}
$ cat bar.txt
  Line 1
  Line 2
```
  
물론이죠!

`main`으로 돌아가 어떤 일이 생기는지 봅시다.

``` {.default}
$ git switch -
  Warning: you are leaving 2 commits behind, not connected to
  any of your branches:

    bf8b8cf appended to bar.txt
    4219f83 added bar.txt

  If you want to keep them by creating a new branch, this may be a
  good time to do so with:

    git branch <new-branch-name> bf8b8cf

  Switched to branch 'main'
```

Git이 이렇게 말하는 것입니다. "이봐요, 90일이 지나면 이 커밋 두 개를 가비지
컬렉션으로 정리할 겁니다. 보관하고 싶다면 브랜치를 연결하세요."

친절하게 그 방법까지 알려 줍니다.

앞서 `topic1`을 강제로 삭제했지만, 그럴 생각이 아니었다면 이제 간단히 다시
만들 수 있습니다. 그렇게 해 봅시다.

``` {.default}
$ git branch topic1 bf8b8cf
$ git switch topic1
  Switched to branch 'topic1'
$ cat bar.txt
  Line 1
  Line 2
```

보시다시피 커밋을 영영 잃었다고 생각한 온갖 곤경에서 reflog가 구해 줄 수
있습니다.

[i[Reflog-->Finding an orphan commit]>]

## Reflog 선택자 {#reflog-selectors}

[i[Reflog-->Selectors]<]

앞의 reflog 출력 예시를 다시 살펴봅시다.

``` {.default}
$ git reflog
  598c84e (HEAD -> main) HEAD@{0}: checkout: moving from topic1 to
                                             main
  dc3d6a3 HEAD@{1}: commit: appended to bar.txt
  0789880 HEAD@{2}: commit: added bar.txt
  598c84e (HEAD -> main) HEAD@{3}: checkout: moving from main to
                                             topic1
  598c84e (HEAD -> main) HEAD@{4}: commit (initial): added foo.txt
```

`HEAD@{3}` 같은 것이 보이나요? 이를 사용해 특정 커밋을 체크아웃할 수
있습니다(예를 들면 커밋 해시 대신 사용할 수 있습니다).

`HEAD@{3}`은 "`HEAD`보다 3개 앞선 커밋"이라는 뜻이 **아닙니다**. 하지만 특정
커밋으로 전환할 때 사용할 수 있는 식별자입니다.

``` {.default}
$ git switch --detach HEAD@{1}
  HEAD is now at dc3d6a3 appended to bar.txt
```

이렇게 간단합니다.

[i[Reflog-->Selectors]>]

[i[Reflog]>]
