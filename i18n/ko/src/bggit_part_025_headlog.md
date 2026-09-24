# Git 로그와 `HEAD` {#the-git-log-and-head}
[i[Log]<]

Git 저장소에 커밋을 만들면 Git은 각 커밋을 나중에 찾아볼 수 있는 로그에
추적합니다. 지금 살펴봅시다.

## 로그 예제 {#an-example-log}

`git log`를 입력하면 커밋 로그를 볼 수 있습니다.

제가 방금 파일 하나를 추가하고 "Added"라는 커밋 메시지로 커밋한 번만
있는 저장소에 있다고 해 봅시다.

``` {.default}
$ git log
```

다음과 같이 나옵니다.

``` {.default}
commit 5a02fede3007edf55d18e2f9ee3e57979535e8f2 (HEAD -> main)
Author: User Name <user@example.com>
Date:   Thu Feb 1 09:24:52 2024 -0800

    Added
```

커밋을 하나 더 만들면 로그가 더 길어집니다.

``` {.default}
commit 5e8cb52cb813a371a11f75050ac2d7b9e15e4751 (HEAD -> main)
Author: User Name <user@example.com>
Date:   Thu Feb 1 12:36:13 2024 -0800

    More output

commit 5a02fede3007edf55d18e2f9ee3e57979535e8f2
Author: User Name <user@example.com>
Date:   Thu Feb 1 09:24:52 2024 -0800

    Added
```

가장 최근의 커밋 항목이 출력 맨 위에 있다는 점을 눈여겨보세요.

## 로그에는 무엇이 있나요? {#whats-in-the-log}

로그에서 눈여겨볼 내용이 몇 가지 있습니다.

* 커밋 설명
* 날짜
* 커밋을 만든 사용자

그리고 `commit`이라는 단어 뒤에는 아주 긴
[fl[16진수|https://en.wikipedia.org/wiki/Hexadecimal]]가 있습니다.

이것이 _커밋 ID_ 또는 *커밋 해시*입니다. 특정 커밋을 식별하는 데 사용할
수 있는 전역적으로 고유한 숫자입니다.

보통은 이 값을 알 필요가 없지만, 시간을 거슬러 올라가거나 여러 개발자가
참여하는 프로젝트에서 커밋을 추적할 때 유용할 수 있습니다.

맨 위에는 `(HEAD -> main)`이라는 부분도 보입니다. 이건 무엇일까요?

[i[Log]>]

## `HEAD` 참조 {#the-head-reference}

[i[`HEAD`]<]

각 커밋에는 다음과 같이 고유하지만 다루기 불편한 식별자가 있다는 것을
보았습니다.

`5a02fede3007edf55d18e2f9ee3e57979535e8f2`

다행히도 사람이 쓰기 더 편한 기호 이름으로 커밋을 가리키는 방법이 몇
가지 있습니다.

`HEAD`는 이런 참조 중 하나입니다. 프로젝트 하위 디렉터리에서 지금 보고
있는 브랜치나 커밋을 나타냅니다[^7b00]. 이전 커밋을 보러 갈 수 있다고
했던 말을 기억하나요? `HEAD`를 해당 커밋으로 옮기면 됩니다.

[^7b00]: 여기서는 말을 조금 늘여 잡고 있습니다. `HEAD`는 전환한 커밋을
    바라봅니다. `HEAD`를 그 커밋으로 옮긴 뒤 파일 일부를 수정했다면 이는
    프로젝트 하위 디렉터리의 내용과 정확히 같지 않을 수도 있습니다.
    커밋은 스냅샷이지만, 수정한 파일을 담은 새 커밋을 만들기 전까지 그
    변경 사항은 스냅샷에 들어가지 않습니다.

> **아직 브랜치를 이야기하지 않았지만, `HEAD`는 보통 브랜치를
> 가리킵니다.** 기본값은 `main` 브랜치입니다. 하지만 지금 너무 앞서가고
> 있으므로, 보통은 브랜치를 거쳐 간접적으로 커밋을 가리키더라도 계속
> `HEAD`가 커밋을 가리킨다고 말하겠습니다.
>
> 그러니 이는 약간 거짓말이지만, 용서해 주길 바랍니다.

용어를 하나 짚고 갑시다. 지금 보고 있는 Git 하위 디렉터리와 그 안의
모든 파일을 *작업 트리*라고 합니다. 작업 트리는 `HEAD`가 가리키는
커밋에 나타난 모습의 파일에, 여러분이 만든 커밋되지 않은 변경 사항을
더한 것입니다.

따라서 `HEAD`를 다른 커밋으로 전환하면 작업 트리의 파일도 그 커밋을
반영하도록 업데이트됩니다.

> **중요하게도 작업 트리 파일의 데이터는 `HEAD`가 가리키는 현재 커밋의
> 파일 데이터와 다를 수 있습니다.** 작업 트리에서 파일을 수정했지만
> 아직 커밋하지 않았을 때 이런 일이 생깁니다.

그렇다면 `HEAD`가 어느 커밋을 가리키는지 어떻게 알 수 있을까요? 바로
로그 맨 위에 나옵니다.

``` {.default}
commit 5e8cb52cb813a371a11f75050ac2d7b9e15e4751 (HEAD -> main)
Author: User Name <user@example.com>
Date:   Thu Feb 1 12:36:13 2024 -0800

    More output
```

첫 줄에 `HEAD`가 보입니다. `HEAD`가 다음 ID의 커밋을 가리킨다는
뜻입니다.

`5e8cb52cb813a371a11f75050ac2d7b9e15e4751`

다시 말하지만 이 역시 약간 거짓말입니다. `HEAD -> main`은 실제로
`HEAD`가 `main` 브랜치를 가리키고, `main`이 커밋을 가리킨다는
뜻입니다. 따라서 `HEAD`는 커밋을 간접적으로 가리킵니다. 이에 관해서는
나중에 더 설명하겠습니다.

## 시간을 거슬러 올라가기와 분리된 `HEAD` {#going-back-in-time-and-detached-head}

[i[`HEAD`-->Detached]]

제 전체 Git 로그는 다음과 같습니다.

``` {.default}
commit 5e8cb52cb813a371a11f75050ac2d7b9e15e4751 (HEAD -> main)
Author: User Name <user@example.com>
Date:   Thu Feb 1 12:36:13 2024 -0800

    More output

commit 5a02fede3007edf55d18e2f9ee3e57979535e8f2
Author: User Name <user@example.com>
Date:   Thu Feb 1 09:24:52 2024 -0800

    Added
```

파일을 살펴보면 "More output" 커밋에 표시된 변경 사항이 보일 것입니다.
하지만 시간을 거슬러 이전 커밋으로 가서 당시 파일이 어떤 모습이었는지
보고 싶다고 해 봅시다. 어떻게 하면 될까요?

> **예를 들어 이전 커밋에는 있었지만 그 뒤로 삭제된 변경 사항을**
> 살펴보고 싶을 수도 있습니다.

[i[Switch]] `git switch` 명령을 사용하면 됩니다.

> **브랜치를 전환하기 전에 모든 내용을 커밋하여 `git status`가 모든
> 것이 깨끗하다고 알려 주어야 합니다.** 그렇지 않다면 전환하기 전에
> 커밋을 만들거나 변경 사항을 [스태시](#stash)하세요.

첫 번째 커밋, 즉 ID가
`5a02fede3007edf55d18e2f9ee3e57979535e8f2`인 커밋을 체크아웃해 봅시다.

다음과 같이 입력할 수 있습니다.

``` {.default}
$ git switch --detach 5a02fede3007edf55d18e2f9ee3e57979535e8f2
```

그러면 작동합니다. 하지만 ID에서 고유한 숫자를 적어도 4개 지정해야
한다는 규칙이 있으므로 다음과 같이 해도 됩니다.

``` {.default}
$ git switch --detach 5a02
```

결과는 같습니다.

그 결과는 다음과 같습니다.

``` {.default}
HEAD is now at 5a02fed
```

`git log`로 둘러봅시다.

``` {.default}
commit 5a02fede3007edf55d18e2f9ee3e57979535e8f2 (HEAD)
Author: User <user@example.com>
Date:   Thu Feb 1 09:24:52 2024 -0800

    Added
```

이게 전부입니다! 커밋이 하나뿐이라고요?! 제가 만든 두 번째 커밋은 어디
있죠? 영원히 사라진 건가요?!

아닙니다. 모두 괜찮습니다. _[햇볕 아래 잠든 새끼 고양이의 마음을
진정시키는 사진.]_

`HEAD`가 특정 커밋에 있으면 그 시점의 스냅샷에 보이던 세상을 바라보는
것입니다. 이 관점에서는 미래의 커밋이 아직 "일어나지" 않았습니다. 그
커밋들은 여전히 어딘가에 있지만, 이름을 지정해 다시 전환해야 합니다.
(무슨 빌어먹을 _타임머신_ 같군요!)

그리고 `(HEAD)`라고 나온 첫 줄에서 달라진 점이 보이나요? 맞습니다.
`main`이 보이지 않습니다.

`main` 브랜치는 여전히 최신 커밋, 즉 "More output"이라는 설명이 붙은
커밋을 바라보고 있기 때문입니다. 그래서 이 관점에서는 보이지 않습니다.

게다가 이는 `HEAD`가 더 이상 `main`에 *붙어* 있지 않다는 뜻입니다. 이
상태를 [i[`HEAD`-->Detached]] *분리된 HEAD*라고 합니다. `git switch`는
정말로 의도한 경우가 아니면 이렇게 하지 못하게 하므로 `--detach`를
지정한 것입니다. (다시 붙이는 것은 쉽습니다. 붙이고 싶은 브랜치로
전환하기만 하면 됩니다.)

> **앞에서 `HEAD`가 커밋을 가리킨다고 말하는 것은 약간 거짓말이라고 한
> 것을 기억하나요?** 분리된 HEAD 상태에서는 실제로 **그렇습니다**.
> 분리된 HEAD 상태는 `HEAD`가 브랜치 대신 커밋을 가리킬 때 생기는
> 상태일 뿐입니다. 다시 붙이려면 다시 브랜치를 가리키도록 바꿔야 합니다.

`HEAD`를 `main` 브랜치에 다시 붙여 봅시다. 두 가지 방법이 있습니다.

1. `git switch -`: 이전에 있던 곳으로 전환합니다. 이 경우에는
   `main`입니다.
2. `git switch main`: 명시적으로 `main`으로 전환합니다.

해 봅시다.

``` {.default}
$ git switch main
  Previous HEAD position was 5a02fed Added
  Switched to branch 'main'
```

이 `git switch`에는 `--detach`가 없다는 점을 눈여겨보세요! HEAD를
분리하는 것이 아니라 다시 붙이고 있으므로, 우리가 무슨 일을 하는지
안다고 Git에 알려 줄 필요가 없습니다.

> **`--detach`를 잊어도 걱정하지 마세요.** 필요하다면 Git이 알려 줍니다.
>
> ``` {.default}
> $ git switch 5a02
>   fatal: a branch is expected, got commit '5a02'
>   hint: If you want to detach HEAD at the commit, try again
>         with the --detach option.
> ```

<!-- ` -->

이제 `git log`를 실행하면 모든 변경 사항이 다시 보입니다.

``` {.default}
commit 5e8cb52cb813a371a11f75050ac2d7b9e15e4751 (HEAD -> main)
Author: User Name <user@example.com>
Date:   Thu Feb 1 12:36:13 2024 -0800

    More output

commit 5a02fede3007edf55d18e2f9ee3e57979535e8f2
Author: User Name <user@example.com>
Date:   Thu Feb 1 09:24:52 2024 -0800

    Added
```

그리고 작업 트리는 `main` 커밋에 있는 모습대로 파일을 보여 주도록
업데이트됩니다.

`HEAD -> main`이 보이나요? 화살표는 `HEAD`가 `main`에 다시 붙었다는
뜻입니다. (`HEAD`가 `main`과 같은 커밋에서 분리되어 있다면
`HEAD, main`이 보일 것입니다.)

[i[`HEAD`]>]

## 옛 명령: `git checkout` {#the-old-command-git-checkout}

[i[Checkout]<]

예로부터, 아니 `git switch`가 생기기 전 옛날에는 이 모든 일을 하는
`git checkout`이라는 명령이 있었습니다. `git checkout`은 많은 일을
했고 지금도 그렇습니다. 워낙 많은 일을 하기 때문에 Git 유지 관리자는
그 기능 중 일부를 `git switch`와 다른 명령으로 분리하려고 해 왔습니다.

> **여전히 `checkout`을 사용해야 할 때가 있지만**, 사용하는 Git 버전이
> `switch`를 지원한다면 지금은 그런 때가 아닙니다. 가능하면 `switch`를
> 사용하고 이 절은 건너뛰세요.

하지만 앞 절의 작업을 `git switch` 대신 `git checkout`만 사용해 다시
해 봅시다.

``` {.default}
$ git checkout 5a02
```

그러면 다음과 같이 나옵니다.

``` {.default}
Note: switching to '5a02fede3007edf55d18e2f9ee3e57979535e8f2'.

You are in 'detached HEAD' state. You can look around, make
experimental changes and commit them, and you can discard any
commits you make in this state without impacting any branches by
switching back to a branch.

If you want to create a new branch to retain commits you create, you
may do so (now or later) by using -c with the switch command.
Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead
to false

HEAD is now at 5a02fed Added
```

이런, 무서운 말이 잔뜩 나오지만 이제 분리된 HEAD 상태라고 Git이 알려
주는 것뿐입니다. 특정 커밋 해시로 전환해 HEAD를 `main` 브랜치에서
분리했으니 당연히 그렇습니다.

다음 명령으로 `main` 브랜치로 돌아갈 수 있습니다.

``` {.default}
$ git checkout main
```

앞서 말한 여러 `git switch` 명령으로 돌아갈 수도 있지만, 이 절에서는
그 명령들이 존재하지 않는 척하겠습니다.

[i[Checkout]>]

## `HEAD` 기준의 상대 커밋 {#commits-relative-to-head}

[i[`HEAD`-->Commits relative to]]

`HEAD`보다 앞선 커밋으로 가는 몇 가지 단축 표기법이 있습니다. 가령
"이 커밋보다 세 번째 앞선 커밋으로 전환하고 싶다"는 경우입니다.

우선 별로 쓸모없는 예제부터 보겠습니다.

[i[Switch]]

``` {.default}
$ git switch --detach HEAD
```

이 명령은 `HEAD`를 원래 `HEAD`가 있던 곳으로 옮깁니다. 즉 아무 데도
옮기지 않습니다. (그래도 브랜치에서 분리하는 효과는 있습니다.)

하지만 현재 `HEAD`가 있는 곳의 바로 *이전* 커밋으로 옮기고 싶다면
어떻게 할까요? 다음과 같이 ***캐럿 표기법***을 사용하면 됩니다.

``` {.default}
$ git switch --detach HEAD^
```

그러면 이전 커밋으로 이동합니다.

> **이런 명령에서는 보통 `HEAD` 대신 `@`를 입력할 수 있다는 사실이
> 잘 알려지지 않은 재미있는 사실™인가 봅니다.** 예를 들어 앞의 명령은
> 다음과 같이 쓸 수 있습니다.
>
> ``` {.default}
> $ git switch --detach HEAD^
> $ git switch --detach @^     # Same thing
> ```
>
> <!-- ` -->
> 일부 셸에서는 제대로 작동하게 하려면 `@`를 따옴표로 감싸야 할 수도
> 있다는 점에 유의하세요. 그래도 잠재적으로 키 입력 세 번을 아낄 수
> 있습니다.

_세 번째 이전_ 커밋으로 가고 싶다면 어떨까요? 캐럿을 더 붙이면 됩니다!

``` {.default}
$ git switch --detach HEAD^^^
```

아니면 열 번째 이전 커밋도 가능합니다!

``` {.default}
$ git switch --detach HEAD^^^^^^^^^^
```

백 번째 이전 커밋도요!

``` {.default}
$ git switch --detach HEAD^^^^^^^^^^^^^^^^^^^^forget this
```

이 캐럿을 전부 입력하려니 지칩니다. 다행히도 우리에게는 ***물결표
표기법***이라는 *또 다른* 단축 표기법이 있습니다. 다음 두 줄은 같은
뜻입니다.

``` {.default}
$ git switch --detach HEAD^^^
$ git switch --detach HEAD~3
```

물결표 뒤에 몇 커밋만큼 뒤로 가고 싶은지 숫자만 지정하면 됩니다.
그러면 앞의 예제는 다음과 같습니다.

``` {.default}
$ git switch --detach HEAD~100   # Much easier
```

이렇게 말해 놓고도 개인적으로 저는 보통 뒤로 몇 개인지 세는 대신
로그를 보고 특정 커밋으로 바로 이동합니다.

> *"하지만 그건 그냥, 뭐랄까, 내 의견일 뿐이야."*\
> \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ —The Dude
