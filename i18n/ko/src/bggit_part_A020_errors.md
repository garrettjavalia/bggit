# 부록: 오류와 무서운 메시지 {#appendix-errors-and-scary-messages}

[i[Errors]<]

## 분리된 HEAD {#detached-head}

[i[`HEAD`-->Detached]<]
[i[Errors-->Detached `HEAD`]<]

단두대가 떠오를 만큼 섬뜩한 다음 메시지를 보셨나요?

``` {.default}
You are in 'detached HEAD' state. You can look around, make
experimental changes and commit them, and you can discard any
commits you make in this state without impacting any branches by
switching back to a branch.

If you want to create a new branch to retain commits you create,
you may do so (now or later) by using -c with the switch command.
Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead
to false

HEAD is now at 0da5af9 line 1
```

이는 브랜치를 체크아웃하는 대신 커밋을 직접 체크아웃했다는 뜻입니다. 즉, `HEAD`가 더는 브랜치에 붙어 있지 않고 "분리된" 상태입니다.

이 상태에서 벗어나는 방법은 다음과 같습니다.

1. HEAD를 분리한 체크아웃을 되돌립니다.

   ``` {.default}
   git switch -
   ```

2. 완전히 다른 브랜치로 전환합니다.

   ``` {.default}
   git switch main
   ```

3. 현재 위치에 새 브랜치를 만들고 체크아웃합니다.

   ``` {.default}
   git switch -c newbranch
   ```

이제 `HEAD`는 더 이상 분리되어 있지 않습니다.

[i[`HEAD`-->Detached]>]
[i[Errors-->Detached `HEAD`]>]

## 업스트림 브랜치 이름이 현재 브랜치와 일치하지 않음 {#upstream-branch-name-doesnt-match-current}

[i[Errors-->Branch name doesn't match]<]

다음 명령을 실행했다면 어떻게 될까요?

``` {.default}
git branch -c newbranch
```

사실 실행하려던 명령은 다음과 같았는데 말입니다.

``` {.default}
git switch -c newbranch
```

그랬다면 다음 상황에 놓일 수 있기 때문입니다.

``` {.default}
fatal: The upstream branch of your current branch does not match
the name of your current branch.  To push to the upstream branch
on the remote, use

    git push origin HEAD:main

To push to the branch of the same name on the remote, use

    git push origin HEAD

To choose either option permanently, see push.default in 'git help
config'.

To avoid automatically configuring an upstream branch when its name
won't match the local branch, see option 'simple' of
branch.autoSetupMerge in 'git help config'.
```

무슨 일인지 알아보도록 브랜치 이름을 확인해 봅시다.

``` {.default}
$ git branch -vv
  main      fc645f2 [origin/main] line 2
* newbranch 7c21054 [origin/main: behind 1] line 1
```

이는 로컬 브랜치 이름과 대괄호 안의 대응하는 원격 추적 브랜치를 알려 줍니다. 수상한 점이 보이나요?

`main`은 `origin/main`에 대응하는 것 같습니다.

그런데 `newbranch`도 **역시** `origin/main`에 대응합니다! 어떻게 된 걸까요?!

`git branch -c newbranch`를 실행하면 현재 브랜치(이 예에서는 `main`)를 다른 브랜치로 _복사_하는데, 이때 _원격 추적 브랜치까지_ 복사하기 때문입니다. 가능하다면 `newbranch`는 `origin/newbranch`와 연결되어야 하므로 좋지 않은 소식입니다.

몇 가지 선택지가 있습니다.

1. `newbranch`를 `origin`으로 푸시하고 `origin/newbranch`로 추적하고 싶습니다.

   다음 명령으로 푸시하면서 원격 추적 브랜치 이름을 바꾸면 됩니다.

   ``` {.default}
   $ git push -u origin newbranch
   ```

2. 로컬 브랜치로만 두고 원격 저장소에는 필요하지 않습니다.

   이 경우 업스트림 설정만 해제합니다.

   ``` {.default}
   $ git branch --unset-upstream newbranch
   ```

[i[Errors-->Branch name doesn't match]>]

## 현재 브랜치에 업스트림 브랜치가 없음 {#current-branch-has-no-upstream-branch}

[i[Errors-->No upstream branch]<]

푸시하려는데 다음 메시지가 나오나요?

``` {.default}
fatal: The current branch topic1 has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin topic1

To have this happen automatically for branches without a tracking
upstream, see 'push.autoSetupRemote' in 'git help config'
```

이는 `topic1`에 업스트림 추적 브랜치가 없고 로컬 브랜치일 뿐이라는 뜻입니다.

이 브랜치를 정말 푸시하려는 것이라면 제안된 지시를 그대로 따르면 됩니다.

실수로 잘못된 브랜치에서 푸시하고 있다면 먼저 올바른 브랜치로 전환하세요.

[i[Errors-->No upstream branch]>]

[i[Errors]>]
