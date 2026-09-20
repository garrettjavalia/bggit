# 커밋 수정하기 {#amend}

[i[Commit-->Amending]<]

Git을 사용하면 마지막 커밋을 비교적 쉽게 수정할 수 있습니다.

> **주의!** 이 절에서는 이력을 바꾸는 방법을 다룹니다. 이력을 바꿀 때의
> 단 하나뿐인 규칙을 잊지 맙시다. 푸시한 것의 이력을 바꾸지 말지어다.
> 다른 누군가가 이미 이전 변경 사항을 풀했을 수도 있고, 그러면 서로의
> 커밋 이력이 처참하게 어긋나서 큰 소리가 오갈 수 있기 때문입니다.
>
> 요컨대 변경 사항을 푸시했다면 다른 누군가가 이미 풀했다고 가정해야
> 합니다. 이때 커밋을 수정하면(즉, 이력을 바꾸면) 큰 골칫거리가 생깁니다.
>
> 더 짧게 말하면, 푸시했다면 이미 늦었습니다. 더는 커밋을 수정하지 마세요.

그렇다면 어떤 때에 이 기능을 쓸까요?

* 커밋 메시지를 망쳐서 다시 쓰고 싶을 수 있습니다.
* 파일 몇 개를 추가하는 것을 깜빡했을 수 있습니다.

물론 우리 중 누구도 평생 한 번도 해 본 적 없는 실수겠지요?

## 커밋 메시지 수정하기 {#amending-the-commit-message}

[i[Commit-->Amending commit messages]<]

이건 꽤 쉽습니다. 제가 망친 커밋을 예로 들어 보겠습니다. 이 시점에는
이미 완전히 커밋된 상태라는 데 주목하세요. `git commit`을 이미 실행했습니다.
하지만 결정적으로, 아직 푸시하지는 않았습니다.

상태와 로그를 확인해 봅시다.

``` {.default}
$ git status
  On branch main
  nothing to commit, working tree clean
$ git log
  commit d7fba6838a689c3de15a27e272e8e4123d7c2460 (HEAD -> main)
  Author: User <user@example.com>
  Date:   Thu Nov 21 20:39:04 2024 -0800

    addded
```

커밋 메시지에 "d"가 하나 너무 많습니다. 고치는 방법은 다음처럼 간단합니다.

``` {.default}
$ git commit --amend
```

그러면 곧바로 편집기가 열리고 메시지를 바꿀 수 있습니다.

편집기를 사용하고 싶지 않다면 명령줄에서 바꿀 수도 있습니다.

``` {.default}
$ git commit --amend -m "the new commit message"
```

이렇게 하면 커밋 작성자는 그대로 유지됩니다. (이미 여러분이 작성자였을
가능성이 크므로 99.9999%의 경우에는 이것이 원하는 동작일 겁니다.) 신원을
바꾸고 싶다면 `git config`로 신원을 다시 설정한 다음 다음 명령을 실행해야
합니다.

``` {.default}
$ git commit --amend --reset-author
```

[i[Commit-->Amending commit messages]>]

## 커밋에 파일 추가하기 {#adding-some-files-to-the-commit}

[i[Commit-->Amending files]<]

으악! 방금 커밋했는데 파일 하나를 추가하는 걸 깜빡했습니다! `foo.c`와
`bar.c`는 넣었지만 `baz.h`가 빠졌습니다!

확인해 봅시다.

``` {.default}
$ ls
  bar.c  baz.h  foo.c
$ git log --name-only
  commit b307686933dca3db718e6a3e3f8226be11e7e278 (HEAD -> main)
  Author: User <user@example.com>
  Date:   Thu Nov 21 20:47:08 2024 -0800

      added

  bar.c
  foo.c
```

좋습니다. 그러면 `baz.h`를 어떻게 넣을까요? 이렇게 하면 됩니다.

1. `git add baz.h`로 스테이징 영역에 추가합니다.
2. `git commit --amend`로 커밋에 넣습니다.

그러면 커밋 메시지를 편집할 수 있도록 편집기가 열립니다. 그대로 저장해도
됩니다. 또는 명령줄에서 `-m` 옵션을 지정해 새 메시지를 줄 수도 있습니다.

파일만 추가하는 상황이라면 커밋 메시지를 전혀 바꿀 필요가 없을 수도
있습니다. 그럴 때는 다음 명령만 실행하면 됩니다.

``` {.default}
$ git commit --amend --no-edit
```

그러면 수정 작업은 수행하되 커밋 메시지는 전혀 편집하지 않습니다.

이것으로 마지막 커밋을 쉽게 수정할 수 있습니다. 다만 수정하기 전에 아직
푸시하지 않았는지 꼭 확인하세요.

[i[Commit-->Amending files]>]

[i[Commit-->Amending]>]
