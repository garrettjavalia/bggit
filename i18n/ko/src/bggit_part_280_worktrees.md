# 작업 트리 {#worktrees}

[i[Worktree]<]

한 창에 코드를 띄워 놓고 즐겁게 `main`을 보고 있다고 해 봅시다. 그러다 "잠깐 `foobranch`의 파일 몇 개를 보면 좋겠는데"라는 생각이 듭니다.

하지만 `main`에서 하던 작업이 한창입니다. 그래서 저장하고, 스태시하고, `foobranch`로 전환하고, 필요한 것을 본 다음, 다시 돌아와 스태시를 팝하고 파일을 편집하는 수고를 합니다.

그리고 정확히 2.3초 뒤 `foobranch`를 다시 봐야 한다는 사실을 깨닫습니다. 으악.

같은 작업 트리에서 두 브랜치를 동시에 체크아웃할 수는 없습니다. 둘을 한꺼번에 보고 싶다면 어떻게 해야 할까요?

물론 클론을 하나 더 만들 수도 있지만, 클론의 데이터가 많다면 부담이 클 수 있습니다. 얕은 클론을 만들 수도 있지만… 이제는 좀 편법처럼 느껴집니다.

알고 보니 *worktree*가 더 나은 방법을 제공합니다. `git worktree`를 사용하면 다른 브랜치에 별도의 작업 트리를 하나 더 만들 수 있습니다. 그러면 둘을 보는 일은 `cd`를 하거나 터미널 창 두 개를 동시에 여는 것만큼 간단합니다.

## 작업 트리의 규칙과 규정 {#worktree-rules-and-regulations}

몇 가지 기본 규칙을 짧게 짚어 두겠습니다.

1. 같은 저장소의 두 작업 트리가 동시에 같은 브랜치를 가리킬 수 없습니다. 즉, 모든 작업 트리는 서로 다른 브랜치를 가리켜야 합니다. 예를 들어 두 작업 트리에서 동시에 `main`으로 전환하려 하면 Git이 막습니다.

2. [i[Worktree-->Choosing location]]새 작업 트리는 원하는 어디에나 둘 수 있습니다. 하지만 기존 작업 트리 *바깥쪽*의 형제 디렉터리에 두기를 강력히 권합니다.

3. 커밋, 푸시, 풀을 포함한 모든 일반 Git 명령을 어느 작업 트리에서든 실행할 수 있습니다.

4. 작업 트리는 **클론이 아닙니다**. 같은 저장소를 바라보는 또 하나의 화면입니다. 두 작업 트리가 같은 로컬 저장소를 바라보므로 한 작업 트리에서 만든 커밋은 다른 작업 트리에서도 즉시 사용할 수 있습니다(다만 자동으로 그 커밋으로 전환되지는 않습니다)! 두 클론이 같은 *원격* 저장소와 연결되는 클론의 경우와는 다릅니다.

5. [i[Worktree-->`main` worktree]]작업 트리 가운데 하나만 *진짜* 작업 트리입니다(`main` 브랜치와는 관계없이 "주 작업 트리"라고 부릅니다). 나머지에는 제대로 된 `.git` 디렉터리가 없습니다. 모든 Git 메타데이터를 잃고 싶지 않다면 진짜 작업 트리를 삭제하지 마세요! 작업 트리를 제거할 때는 늘 `git worktree remove`를 사용하세요. 어리석게 주 작업 트리를 지우려 해도 이 명령은 거부합니다.

지루한 법률 문구는 끝났으니 이제 무언가 해 봅시다!

## 새 작업 트리 만들기 {#making-a-new-worktree}

[i[Worktree-->Creating]<]

`main` 브랜치에 있는 `wumpus`라는 저장소의 루트에 있다고 해 봅시다. 그리고 `arrow`라는 다른 브랜치를 새 작업 트리에서 보고 싶습니다.

`git worktree add`를 사용하며 두 가지를 인수로 줍니다.

1. 작업 트리를 둘 디렉터리 이름입니다. 여기서는 `wumpus` 저장소의 `arrow` 브랜치를 체크아웃하므로 `../wumpus-arrow`라고 부르겠습니다. 물론 이름은 무엇이든 괜찮습니다.

   `../`에도 주목하세요. 부모 디렉터리로 이동하여 `wumpus-arrow`를 `wumpus`의 형제 디렉터리로 만들기 위한 것입니다.

2. 그 작업 트리에서 전환할 브랜치도 지정해야 합니다. 다시 말하지만 다른 작업 트리에 체크아웃된 브랜치는 사용할 수 없습니다.

해 봅시다!

``` {.default}
$ git worktree add ../wumpus-arrow arrow 
  Preparing worktree (checking out 'arrow')
  HEAD is now at 7da9b7f fix arrow flight
```

다 됐습니다. 이제 그 디렉터리로 `cd`해서 둘러볼 수 있습니다.

``` {.default}
$ cd ../wumpus-arrow
$ git status
  On branch arrow
  nothing to commit, working tree clean
```

또는 창을 두 개 열어도 됩니다. 하나는 `wumpus/` 디렉터리에서 `main` 브랜치를 보고, 다른 하나는 `wumpus-arrow/` 디렉터리에서 `arrow` 브랜치를 봅니다.

[i[Worktree-->Creating]>]

## 작업 트리 제거하기 {#removing-a-worktree}

[i[Worktree-->Removing]<]

먼저 제거할 운명에 놓인 작업 트리에 커밋하지 않은 변경 사항이 없는지 확인하세요. 그런 다음 그 경로를 알아내 제거합니다.

앞의 예에 나온 `wumpus/` 디렉터리에 있다고 합시다. 여기서 `wumpus-arrow/` 작업 트리를 다음과 같이 삭제할 수 있습니다.

``` {.default}
$ git worktree remove ../wumpus-arrow
```

이것으로 끝입니다.

다시 말하지만 그 작업 트리에서 만든 커밋은 삭제되지 않습니다. 작업 트리는 주 작업 트리와 같은 저장소를 바라보는 화면일 뿐이므로, 어느 작업 트리에서 만들든 커밋하는 즉시 저장소에 저장됩니다.

`worktree remove`에 지정하는 경로는 `worktree add`에 지정했던 것과 글자 하나하나까지 같을 필요가 없습니다. 같은 디렉터리만 가리키면 됩니다.

예를 들어 `wumpus/` 디렉터리에 있다면 다음 명령도 첫 번째 예와 같은 일을 합니다.

``` {.default}
$ cd ..
$ git worktree remove wumpus-arrow
```

심지어 지금 들어 있는 작업 트리 자체도 날려 버릴 수 있습니다.

``` {.default}
$ git worktree remove .
```

마지막으로 주 작업 트리를 제거하려 하면 Git 경찰이 여러분을 막습니다.

``` {.default}
$ git worktree remove wumpus
  fatal: 'wumpus' is a main working tree
```

[i[Worktree-->Removing]>]

## 작업 트리 목록 보기 {#listing-worktrees}

[i[Worktree-->Listing]<]

`worktree list` 명령으로 모든 작업 트리를 보고 어느 것이 주 작업 트리인지 알아낼 수 있습니다.

``` {.default}
$ git worktree list
  /home/user/wumpus        30d669a [main]
  /home/user/wumpus-arrow  7da9b7f [arrow]
```

왼쪽에는 디렉터리, 가운데에는 커밋 해시, 오른쪽에는 브랜치 이름이 표시됩니다.

목록의 첫 번째 작업 트리가 주 작업 트리, 즉 제거할 수 없는 작업 트리입니다.

[i[Worktree-->Listing]>]

## 작업 트리와 분리된 `HEAD` {#worktrees-and-detached-head}

[i[Worktree-->Detached `HEAD`]<]

같은 저장소의 두 작업 트리에서 같은 브랜치를 체크아웃할 수는 없습니다. 그러면 작업 트리 두 개가 브랜치 참조 하나의 위치를 놓고 다투게 되기 때문입니다.

[i[`HEAD`-->Detached]<]

하지만 두 작업 트리에는 각각 자체 `HEAD`가 있습니다! 이 점에서는 충돌이 없습니다. 따라서 한 작업 트리는 `main`에 두고, 다른 작업 트리는 `main`과 같은 커밋에서 `HEAD`를 분리할 수 있습니다. 물론 분리된 `HEAD`에서 커밋해도 `main`은 전혀 움직이지 않으므로 충돌이 없습니다.

``` {.default}
$ cd ../wumpus-arrow

$ git switch --detach main
  HEAD is now at 30d669a add drafts

$ git worktree list
  /home/user/wumpus        30d669a [main]
  /home/user/wumpus-arrow  30d669a (detached HEAD)
```

이 예에서 두 작업 트리 모두 같은 커밋 `30d669a`를 가리키지만, 같은 브랜치를 체크아웃한 것이 아니므로 괜찮습니다.

새 작업 트리의 `HEAD`를 `main` 커밋에서 분리하는 다음 예처럼, 분리된 `HEAD`로 새 작업 트리를 추가할 수도 있습니다.

``` {.default}
$ git worktree add --detach ../wumpus-worktree main
```

마지막으로 새 작업 트리를 만들 때 브랜치 대신 커밋 해시를 지정하면 분리된 `HEAD` 상태로 자동 생성됩니다.

[i[Worktree-->Detached `HEAD`]>]
[i[`HEAD`-->Detached]>]
[i[Worktree]>]
