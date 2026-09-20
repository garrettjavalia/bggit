# 리베이스: 커밋 옮기기 {#rebase}

[i[Rebase]<]

리베이스의 제1원칙부터 시작하겠습니다. ***푸시한 것은 절대 리베이스하지
마세요.*** 즉, 아무도 보지 않은 로컬 변경 사항만 리베이스하세요. 리베이스한
뒤에는 푸시해도 됩니다.

*그 결과를 이해한다면* 푸시한 것도 리베이스할 수 있으므로 법칙보다는 지침에
가깝습니다. 하지만 대개 좋은 상황은 아니므로 일반적으로 피해야 합니다.

리베이스는 *이력을 다시 쓰기* 때문입니다. 그러면 이전 이력이 있는 저장소를
클론한 다른 개발자의 이력과 어긋나고, 다시 동기화하기가 상당히 어려워집니다.

Git에는 이력을 다시 쓰는 다른 명령도 있습니다. 일반 원칙은 *이미 푸시한
것의 이력은 절대 다시 쓰지 않는다*입니다. 정말로 무엇을 하는지 아는 경우는
예외입니다.

## 병합과 비교하기 {#contrasted-to-merging}

[i[Rebase-->Compared to merging]<]
[i[Merge-->Compared to rebasing]<]

신나게 리베이스 이야기를 시작하기 전에 병합을 짧게 복습해 봅시다. 그림_#.1은
앞서 본 예를 조금 바꾼 것으로, 갈라진 브랜치 두 개가 있습니다. 여러분이
`topic` 브랜치에서 작업한다고 합시다.

![갈라진 브랜치 두 개.](img_110_010.pdf "Two divergent branches.")

누군가 `main`을 변경했다는 말을 들었고, 여러분의 변경 사항을 아직 `main`에
넣지는 않으면서 그 변경 사항을 `topic` 브랜치에 반영하고 싶습니다.

이때 `main`의 변경 사항을 `topic`에 넣는 병합 방식은 *병합 커밋*이라는 새
커밋을 만드는 것이었습니다. 병합 커밋은 부모 커밋 두 개(여기서는 `(2)`와
`(4)`)의 변경 사항을 담아 그림_#.2의 `(5)`로 표시된 새 커밋을 만듭니다.

![갈라진 브랜치 두 개를 병합한 모습.](img_110_020.pdf "Two divergent branches, merged.")

이 시점의 로그를 보면 `topic` 브랜치에서 그래프의 다른 모든 커밋에 담긴
변경 사항을 볼 수 있습니다.

이것으로 충분합니다. 제대로 작동했고 원하는 일을 했습니다. 병합은 이 문제에
완전히 타당한 해결책입니다.

하지만 병합에는 몇 가지 단점이 있습니다. 사실 `main`의 최신 내용을 우리
브랜치에서 사용하고 싶었을 뿐, 무언가를 커밋하고 싶지는 않았습니다. 그런데
모두에게 보이는 새 커밋을 만들었습니다.

게다가 커밋 그래프가 고리를 이루어 이력이 원하는 것보다 조금 더 복잡해졌습니다.

정말 좋았을 방법은 `topic`의 커밋 `(3)`과 `(4)`를 가져와 그 변경 사항을
`main`의 `(2)`에 적용하는 것입니다. 즉 `topic`이 `(1)`에서 갈라진 대신
`(2)`에서 갈라진 척할 수 있을까요?

`(2)`에서 갈라졌다면 원했던 `main`의 변경 사항을 갖게 됩니다.

필요한 것은 커밋을 분기점 `(1)`까지 되감은 뒤 커밋 `(2)` 위에 다시 적용하는
방법입니다. 즉 커밋 `(1)`이던 `topic` 브랜치의 기반을 커밋 `(2)`라는 다른
기반으로 바꿔야 합니다. 커밋 `(2)`로 ***리베이스***하려는 것입니다!

[i[Rebase-->Compared to merging]>]
[i[Merge-->Compared to rebasing]>]

## 작동 원리 {#how-it-works}

바로 그렇게 해 봅시다. 커밋 `(3)`에서 만든 변경 사항을 가져와 `main`의
커밋 `(2)`에 적용합니다. 그러면 커밋 `(2)`와 `(3)`의 변경 사항을 모두 담은
완전히 새로운 커밋이 생깁니다. (중요하게도 이 커밋은 전에는 존재하지
않았습니다. `(2)`와 `(3)`의 변경 사항을 함께 담은 커밋은 없었습니다.)
`(3)`에서 만든 변경 사항을 담았으므로 새 커밋을 `(3')`("3 프라임")이라고
부르겠습니다.

그런 다음 커밋 `(4)`에도 같은 작업을 합니다. 이전 커밋 `(4)`의 변경 사항을
`(3')`에 적용하여 새 커밋 `(4')`을 만듭니다.

그러면 그림_#.3과 같은 결과를 얻습니다.

![`main` 위로 리베이스한 `topic` 브랜치.](img_110_030.pdf "topic branch rebased on main")

이제 `(3')`과 `(4')`이 `main` 위로 리베이스된 모습이 보입니다! 그리고
`topic` 브랜치에는 이제 `main` 브랜치의 커밋 `(2)`가 들어 있습니다!

다시 말하면 두 커밋에는 원래 커밋 `(3)`과 `(4)`에 있던 것과 같은 변경 사항이
있지만, 이제 `main`의 커밋 `(2)`에 적용되었습니다. 이제 `main`의 변경 사항도
포함하므로 코드는 필연적으로 달라집니다. 즉 이전 커밋 `(3)`과 `(4)`는 사실상
사라졌고, 리베이스가 같은 변경 사항을 다른 기반점에 담은 새 커밋 두 개로
대체했습니다.

> **방금 이력을 바꿨습니다.** 이 장 첫머리에서 이력을 다시 쓴다고 한 것이
> 바로 이것입니다. 다른 개발자가 이전 커밋 `(3)`과 `(4)`를 가지고 그 위에서
> 자신의 새 커밋을 만들고 있다고 상상해 보세요. 그런데 여러분이 리베이스하여
> 커밋 `(3)`과 `(4)`를 사실상 없애 버렸습니다. 이제 두 사람의 커밋 이력이
> 서로 다르므로 이를 정리하며 온갖 *재미*™를 맛보게 됩니다.
>
> 푸시하지 않은 커밋만 리베이스하면 문제가 생기지 않습니다. 하지만 다른
> 개발자가 여러분의 커밋을 가지고 있다면(이미 푸시했고 그들이 풀했기
> 때문이라면) 그 커밋을 리베이스하지 마세요!

## 언제 해야 할까요? {#when-should-i-do-this}

[i[Rebase-->When to use]]
정해진 규칙은 없습니다. 어떤 조직에는 모든 사람이 항상 리베이스하여 커밋
이력을 더 깔끔하게(병합 커밋과 고리 없이) 유지한다는 규칙이 있습니다.

다른 조직에서는 완전한 이력을 보존하도록 늘 병합하라고 합니다.

## 풀과 리베이스 {#pulling-and-rebasing}

[i[Rebase-->And pulling]<]

예전에 배운 내용을 기억한다면, 풀은 사실 [i[Fetch]] *페치*와 *병합*이라는
두 작업으로 이루어집니다.

페치는 원격 저장소의 새 데이터를 모두 내려받지만 브랜치나 작업 트리에
아무것도 실제로 병합하지 않습니다. 따라서 페치 뒤에는 로컬 변경 사항이
보이지 않습니다.

하지만 풀은 뒤이어 일반 병합을 수행하므로 원격 추적 브랜치의 변경 사항이
로컬 브랜치에 나타납니다.

모든 설정을 마치고 `main` 브랜치에 있다고 가정할 때 다음 명령을 실행하면,

``` {.default}
$ git pull
```

Git은 실제로 다음과 비슷한 작업을 합니다.

[i[Fetch]]

``` {.default}
git fetch                # Get all the information from origin
git merge origin/main    # Merge origin/main into main
```

(`origin/main`은 원격 추적 브랜치라는 점을 기억하세요. 로컬 컴퓨터의
`main`이 아니라 `origin`에 있는 `main` 버전입니다.)

하지만 여기서 병합만 할 수 있는 것은 아닙니다. 리베이스를 다루는 장인 만큼,
대신 리베이스를 하게 만들 수 있다고 짐작했을 텐데 맞습니다.

방법은 다음과 같습니다.

``` {.default}
$ git pull --rebase
```

그러면 다음 두 작업이 일어납니다.

[i[Fetch]]

``` {.default}
git fetch                # Get all the information from origin
git rebase origin/main   # Rebase main into origin/main
```

현재 저장소에서 이를 기본 동작으로 삼으려면 다음 명령을 한 번 실행합니다.

``` {.default}
$ git config pull.rebase true
```

모든 저장소의 기본 동작으로 삼으려면 다음과 같이 합니다.

``` {.default}
$ git config --global pull.rebase true
```

풀할 때 항상 리베이스하도록 저장소를 설정했더라도 다음 명령으로 설정을
재정의하여 병합을 강제할 수 있습니다(원한다면 말입니다).

``` {.default}
$ git pull --no-rebase  # Do a merge instead of a rebase
```

[i[Rebase-->And pulling]>]

## 충돌 {#rebasing-conflicts}

[i[Rebase-->Conflicts]<]

병합할 때는 다른 브랜치의 변경 사항과 충돌할 가능성이 있으며, 앞에서 본
것처럼 이를 해결해야 합니다.

리베이스에서도 같은 일이 생길 수 있을까요?

물론입니다! 리베이스하려는 대상 커밋이 여러분의 커밋과 충돌하면 병합할 때와
같은 문제를 겪습니다.

다행히 Git에서는 병합과 비슷한 방법으로 충돌을 해결할 수 있습니다.

간단한 예로 시작합시다. 다음 내용을 담은 텍스트 파일이 있습니다.

``` {.default}
The magic number is 1.
```

이를 `main` 브랜치의 커밋에 넣습니다.

그 위치에서 새 `topic` 브랜치를 만듭니다.

그런 다음 `main` 브랜치에서 숫자를 `2`로 바꾸고 커밋합니다.

`topic` 브랜치에서는 숫자를 `3`으로 바꾸고 커밋합니다.

그러면 그림_#.4와 같은 상황이 됩니다.

![충돌할 준비가 된 브랜치.](img_110_040.pdf "Branches ready for conflict")

마지막으로 `topic`을 `main` 위로 리베이스해 봅니다.

이때 Git은 혼란에 빠집니다. `main`의 마지막 커밋에는 `2`가 있고, `topic`은
그 변경 전에 갈라졌으므로 이를 모른다는 사실을 압니다. 또 `topic`의 마지막
커밋에는 `3`이 있다는 것도 압니다. 어느 쪽이 맞을까요?

`topic` 브랜치에서 리베이스를 시도하여 무슨 일이 일어나는지 봅시다.

``` {.default}
$ git rebase main
  Auto-merging magic.txt
  CONFLICT (content): Merge conflict in magic.txt
  error: could not apply 9f19221... Update to 3
  hint: Resolve all conflicts manually, mark them as resolved with
  hint: "git add/rm <conflicted_files>", then run "git rebase
  hint: --continue".
  hint: You can instead skip this commit: run "git rebase --skip".
  hint: To abort and get back to the state before "git rebase", run
  hint: "git rebase --abort".
  hint: Disable this message with "git config advice.mergeConflict
  hint: false"
  Could not apply 9f19221... Update to 3
```

워워. 좋습니다. 자동으로는 할 수 없군요. "모든 충돌을 수동으로 해결"한 뒤
추가하고, `--continue` 플래그를 붙여 리베이스를 다시 실행해 계속하라고 합니다.

> **힌트를 계속 읽으면** 내용이 더 있습니다. `--skip`은 뒤에서 다루겠지만,
> 지금 당장 감당하기 어려운 충돌이라면 다음 명령을 실행할 수 있다는 점을
> 기억하세요.
>
> ``` {.default}
> $ git rebase --abort
> ```
>
> <!-- ` -->
> 그러면 애초에 시작하지 않았던 것처럼 돌아갑니다.

조금 익숙하게 들릴 수 있습니다. 기본적으로 병합 충돌 때 거친 것과 같은 과정입니다.

1. 충돌 파일을 편집하여 *올바르게* 만듭니다.
2. 추가합니다.
3. 리베이스를 계속합니다.

해 봅시다. 편집기에서 `magic.txt` 파일을 열면 다음 내용이 보입니다.

``` {.default .numberLines}
<<<<<<< HEAD
The magic number is 2
=======
The magic number is 3
>>>>>>> 9f19221 (Update to 3)
```

병합 충돌 때와 똑같이 Git이 이 줄에 대한 두 선택지를 보여 줍니다. 팀과
상의하여 파일에 무엇을 넣을지 합의하고, 있어서는 안 될 내용을 모두 지워
*올바르게* 만듭니다.

``` {.default}
The magic number is 3
```

그리고 저장합니다.

이제 이 시점에 무엇을 하기로 했더라요? 잊었어도 괜찮습니다. `git status`를
실행하여 현재 상태를 확인하세요.

``` {.default}
$ git status
  interactive rebase in progress; onto 6ceeefb
  Last command done (1 command done):
     pick 9f19221 Update to 3
  No commands remaining.
  You are currently rebasing branch 'topic' on '6ceeefb'.
    (fix conflicts and then run "git rebase --continue")
    (use "git rebase --skip" to skip this patch)
    (use "git rebase --abort" to check out the original branch)
  
  Unmerged paths:
    (use "git restore --staged <file>..." to unstage)
    (use "git add <file>..." to mark resolution)
	  both modified:   magic.txt
  
  no changes added to commit (use "git add" and/or "git commit -a")
```

아, 맞다! `--continue`였지요?

``` {.default}
$ git rebase --continue
  magic.txt: needs merge
  You must edit all merge conflicts and then
  mark them as resolved using git add
```

뭐라고요? 아, 상태 메시지를 더 읽었어야 했습니다. `git add`를 사용하여
`magic.txt` 파일이 해결되었다고 표시하라고 합니다. 그렇게 해 봅시다.

``` {.default}
$ git add magic.txt
$ git status
  interactive rebase in progress; onto 6ceeefb
  Last command done (1 command done):
     pick 9f19221 Update to 3
  No commands remaining.
  You are currently rebasing branch 'topic' on '6ceeefb'.
    (all conflicts fixed: run "git rebase --continue")

  Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
	  modified:   magic.txt
```

상태가 더 보기 좋아졌습니다. (다만 병합할 때 특별한 "merge" 상태에 들어가는
것과 마찬가지로 Git이 특별한 "rebase" 상태에 있다는 점에 주목하세요. 다시
정상적으로 Git을 사용하려면 중단하거나 계속해야 합니다.)

이제 `--continue`입니다.

``` {.default}
$ git rebase --continue
```

그러면 커밋 메시지를 편집하도록 편집기가 열립니다. 커밋 메시지가 더는 커밋을
제대로 설명하지 않는다면 바꿀 기회입니다. (즉 충돌을 해결하면서 커밋을 완전히
다른 내용으로 바꿨다면 메시지도 고쳐야 할 수 있습니다.) 필요하면 편집하고 저장하세요.

그러면 Git이 다음과 같이 말합니다.

``` {.default}
[detached HEAD 443fa53] Update to 3
 1 file changed, 1 insertion(+), 1 deletion(-)
Successfully rebased and updated refs/heads/topic.
```

`git status`를 보면 모든 것이 해결되었습니다.

그 모든 작업이 끝나면 그림_#.5의 새 커밋 그래프를 볼 수 있습니다.

![리베이스 충돌을 해결한 뒤.](img_110_050.pdf "After rebase conflict resolution.")

마지막으로 한 가지 더 말하겠습니다. 풀할 때마다 리베이스에서 같은 충돌을
계속 해결하고 있다면 그 과정을 자동화하는 데 도움이 되는 [i[`git
rerere`]] [fl[`git
rerere`|https://git-scm.com/book/en/v2/Git-Tools-Rerere]]를 살펴보세요.

[i[Rebase-->Conflicts]>]

## 커밋 스쿼시하기 {#squashing-commits}

[i[Rebase-->Squashing commits]<]

이 개념은 깔끔한 커밋 이력이라는 생각과 잘 맞습니다.

저장 용량 한도를 초과했다는 경고 상자를 추가하는 기능을 구현하라는 일을
맡았다고 합시다.

문제없습니다. 기능을 추가하고 "Added feature #121"이라는 메시지로
커밋합니다. (아직 푸시하지는 않습니다.)

``` {.default}
alert("Strrage limit exceeeded");
```

커밋한 뒤 오타를 발견합니다. 이런.

오타를 고치고 "Fixed typo"라는 메시지로 커밋합니다.

``` {.default}
alert("Storage limit exceeeded");
```

끝났습니다.

잠깐만요! 오타가 또 있습니다! 장난하나요?

그래서 고칩니다.

``` {.default}
alert("Storage limit exceeded");
```

그리고 "Fixed another typo"라는 커밋을 하나 더 추가합니다.

이제 로컬 커밋 이력은 다음과 같습니다.

``` {.default}
Fixed another typo
Fixed a typo
Added feature #121
```

그리 깔끔하지 않지요? 원래는 기능 #121을 구현한 커밋 하나여야 했습니다.

다행히 아직 푸시하지 않았으므로 이력을 자유롭게 다시 쓸 수 있습니다!

리베이스의 ***스쿼시(squashing)*** 기능으로 이 일을 할 수 있습니다.

두 오타 수정 커밋을 기능을 처음 구현하려 했던 이전 커밋에 스쿼시하려는 것입니다.

먼저 로그를 봅시다.

``` {.default}
$ git log
commit c1820e6d0da19013208b389d264310162477b099 (HEAD -> main)
Author: User <user@example.com>
Date:   Wed Jul 17 11:53:10 2024 -0700

    Fixed another typo

commit c62c0db7b82e6b415d36bd0f00d568fd503164b7
Author: User <user@example.com>
Date:   Wed Jul 17 11:53:10 2024 -0700

    Fixed typo

commit ab84a428b8baae0078ee0647a67b34a89a6abed8
Author: User <user@example.com>
Date:   Wed Jul 17 11:53:10 2024 -0700

    Added feature #121

commit a95854659e31d203e2325eee61d892c9cdad767c
Author: User <user@example.com>
Date:   Wed Jul 17 11:53:10 2024 -0700

    Added
```

리베이스이므로 어떤 커밋 위로 리베이스해야 합니다. 기능 추가 커밋의 _바로
이전_ 커밋, 즉 ID가 `a9585`로 시작하는 커밋입니다.

스쿼시를 할 수 있는 특별한 리베이스 모드인 _대화형_ 모드로 실행하려 합니다.
`-i` 플래그를 사용합니다.

``` {.default}
$ git rebase -i a9585
```

그러면 다음 정보와 그 아래에 지시 사항이 가득한 거대한 주석 블록이 든
편집기가 열립니다.

``` {.default .numberLines}
pick ab84a42 Added feature #121
pick c62c0db Fixed typo
pick c1820e6 Fixed another typo
```

익숙한 로그의 역순이 아니라 시간 순서대로 나열된 점에 주목하세요.

주석 블록에 표시된(이 안내서에는 싣지 않은) 수많은 옵션을 보세요! pick,
reword, edit, squash, fixup… 고를 것이 아주 많습니다. 짐작하겠지만 상당히
강력한 이력 다시 쓰기 모드에 들어와 있습니다.

지금은 거의 같은 기능인 "squash"와 "fixup"만 살펴봅시다.

"squash"부터 보겠습니다. 오타 수정 커밋들을 "Added feature" 커밋에 합치려
합니다. squash 모드로 할 수 있습니다.

파일을 다음과 같이 편집하겠습니다.

``` {.default .numberLines}
pick ab84a42 Added feature #121
squash c62c0db Fixed typo
squash c1820e6 Fixed another typo
```

그러면 "Fixed another typo"를 "Fixed typo"에 스쿼시한 다음, 그 결과를
"Added feature #121"에 스쿼시합니다.

`pick`은 그저 "이 커밋을 그대로 사용한다"는 뜻입니다.

> **이 명령에는 모두 축약형이 있습니다.** `squash` 대신 `s`를 사용할 수도
> 있었습니다.

파일을 저장하면 다음 내용이 든 또 다른 편집기가 곧바로 열립니다.

``` {.default .numberLines}
# This is a combination of 3 commits.
# This is the 1st commit message:

Added feature #121

# This is the commit message #2:

Fixed typo

# This is the commit message #3:

Fixed another typo
```

여기서는 커밋 세 개를 하나로 스쿼시한 새 리베이스 커밋을 만들므로 새 커밋
메시지를 쓸 수 있습니다. 친절하게도 Git이 세 커밋 메시지를 모두 넣어
두었습니다. 원하는 커밋 메시지만 남기도록 줄여 봅시다.

``` {.default .numberLines}
Added feature #121
```

저장하고 나오면 메시지가 나타납니다.

``` {.default}
[detached HEAD 4bc6bca] Added feature #121
 Date: Wed Jul 17 11:53:10 2024 -0700
 1 file changed, 1 insertion(+)
 create mode 100644 foo.js
Successfully rebased and updated refs/heads/main.
```

성공은 좋은 것입니다. 저는 성공이 좋습니다.

> **분리된 HEAD라는 말은 무엇일까요?** Git은 리베이스 중에 `HEAD`를 잠시
> 분리합니다. 걱정하지 마세요. 알아서 다시 붙여 줍니다.

이제 커밋 이력이 말끔해졌습니다.

``` {.default}
commit 4bc6bca6870d124b3eebc9afd32486a5a23189fc (HEAD -> main)
Author: User <user@example.com>
Date:   Wed Jul 17 11:53:10 2024 -0700

    Added feature #121

commit a95854659e31d203e2325eee61d892c9cdad767c
Author: User <user@example.com>
Date:   Wed Jul 17 11:53:10 2024 -0700

    Added
```

앞의 로그와 비교하면 "Added feature" 커밋 ID가 바뀐 것을 볼 수 있습니다.
어쨌든 리베이스를 했으므로 이전 커밋은 사라지고 새 커밋으로 대체되었습니다.

이 모든 작업을 마친 *이제야* 푸시할 수 있습니다. 이력을 다시 쓰는 일이므로
푸시한 뒤에는 해서는 안 된다는 점을 늘 기억하세요.

[i[Rebase-->Squashing commits]>]

### Squash와 Fixup 비교 {#squash-versus-fixup}

[i[Rebase-->Fixup]]

이제 `squash` 대신 `fixup`을 쓰는 방법을 짧게 살펴봅시다. 기본적으로 합쳐질
대상 커밋의 메시지만 남긴다는 점을 빼면 같습니다. 따라서 다음과 같이 실행하면,

``` {.default .numberLines}
pick fbc1075 Added feature #121
fixup fd4ca42 Fixed typo
fixup 6a10e97 Fixed another typo
```

Git은 즉시 다음 메시지를 내놓습니다.

``` {.default}
Successfully rebased and updated refs/heads/main.
```

Git 로그에는 "Added feature #121" 커밋만 보입니다. `fixup`을 사용하면 Git이
스쿼시되는 커밋의 메시지를 자동으로 버립니다.

## 리베이스에서 여러 충돌이 발생할 때 {#multiple-conflicts-in-the-rebase}

[i[Rebase-->Conflicts]<]

커밋을 병합할 때 충돌이 여러 개 생기면 큰 병합 커밋 하나에서 모두 해결하고
끝냅니다. `git commit`으로 모든 작업을 마무리합니다.

리베이스는 조금 다릅니다. 커밋을 새 기반 위에 하나씩 "재생"하므로 재생할
때마다 병합 충돌이 생길 수 있습니다. 즉 *리베이스하면서 여러 충돌을 잇달아
해결해야 할 수도 있습니다*.

예를 들어 토픽 브랜치에서 `foo.txt`를 수정한 커밋을 만들었다고 합시다. 그런
다음 `bar.txt`를 수정한 *또 다른 커밋*을 만들었습니다.

그런데 여러분도 모르는 사이 `main` 브랜치의 누군가도 두 파일을 수정했습니다.
따라서 리베이스할 때 반드시 충돌합니다.

그래서 `git rebase main`을 시작하자마자 문제가 생깁니다. `foo.txt`가
충돌한다고 합니다.

이를 고치고 `git rebase --continue`를 실행한 다음 커밋 메시지를 편집하여
계속 진행합니다.

하지만 그러면 `bar.txt`를 수정한 *다음* 커밋으로 넘어가 리베이스를 시도할
뿐입니다. 이 커밋도 충돌합니다!

따라서 이를 고치고 `git rebase --continue`를 실행한 다음 커밋 메시지를
편집하여 계속 진행합니다. 또다시 말입니다.

마침내 성공 메시지가 나타납니다.

``` {.default}
[detached HEAD 31c3947] topic change bar
 1 file changed, 1 insertion(+)
Successfully rebased and updated refs/heads/topic.
```

이 때문에 병합은 간단한 커밋으로 끝낼 수 있지만, 리베이스는 모든 커밋이
깔끔하게 리베이스될 때까지 `git rebase --continue`를 반복 실행해야 합니다.

좋은 일일까요, 나쁜 일일까요? 각 커밋을 따로 병합할 기회가 생기므로 상황을
이해하고 오류를 피하기 더 쉬울 수 있다는 점에서는 낫습니다. 하지만 동시에
끝까지 처리하는 데 손이 더 많이 갑니다.

언제나 그렇듯 작업에 맞는 도구를 사용하세요!

[i[Rebase-->Conflicts]>]

[i[Rebase]>]
