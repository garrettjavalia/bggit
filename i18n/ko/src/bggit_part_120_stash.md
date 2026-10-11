# 스태시: 변경 사항을 잠시 치워 두기 {#stash}

[i[Stash]<]

무언가를 작업하던 중 다른 변경 사항을 pull하고 싶어졌지만, 아직 작업물이
완전히 망가진 상태라 커밋할 준비가 되지 않았다면 `git stash`가 도움이
됩니다. 작업 중인 내용을 한쪽에 치워 두고 작업 트리를 마지막 커밋 상태로
되돌려 줍니다.

변경 사항이 사라진 것처럼 보이지만 걱정하지 마세요. 안전하게 스태시해
두었으므로 나중에 다시 가져올 수 있습니다.

그런 다음 새 내용을 pull해 최신 상태로 만들고, 그 위에 스태시한 내용을 다시
꺼낼 수 있습니다.

개념적으로는 작은 리베이스와 비슷합니다.

## 예시 {#example}

현재 최신 상태라고 합시다.

``` {.default}
$ git pull
```

좋습니다. 이제 코딩을 시작합니다. 기존 `foo.rs` 파일을 열고 평소처럼 코드를
조금 추가합니다.

그때 옆자리의 Chris가 말합니다. "잠깐만요. 방금 `main`에 중요한 업데이트를
했으니 그걸 사용하세요!"

여러분은 생각합니다. "이런, 한창 작업 중이었는데." 아직 커밋할 준비는 안
됐지만 Chris의 변경 사항은 가져오고 싶습니다.

파일을 저장하고 다음 명령을 실행합니다.

``` {.default}
$ git stash
  Saved working directory and index state WIP on main: c72c245
                                some very descriptive commit message
```

유심히 보고 있었다면 편집기의 파일이 이전 모습으로 돌아가는 것을 봤을 수도
있습니다! 변경 사항이 취소되어 스태시에 보관됐습니다!

이 시점에 `git status`를 실행하면 다음과 같이 보입니다.

``` {.default}
$ git status
  On branch main
  Your branch is up to date with 'origin/main'.

  nothing to commit, working tree clean
```

모두 깨끗합니다. 이제 pull해서 최신 `main`을 가져올 수 있다는 뜻입니다.
그렇게 해 봅시다.

``` {.default}
$ git pull
  remote: Enumerating objects: 5, done.
  remote: Counting objects: 100% (5/5), done.
  remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
  remote: (from 0)
  Unpacking objects: 100% (3/3), 943 bytes | 943.00 KiB/s, done.
  From /home/beej/tmp/origin
     10a8ad6..e286011  main       -> origin/main
  Updating 10a8ad6..e286011
  Fast-forward
   foo.rs | 1 +
   1 file changed, 1 insertion(+)
```

이제 최신 상태가 됐습니다.

잠깐, 무엇을 작업하고 있었죠? 아, 맞다! 스태시해 뒀죠! `pop`으로 그 변경
사항을 다시 꺼냅시다.

``` {.default}
$ git stash pop
  Auto-merging foo.rs
  On branch main
  Your branch is up to date with 'origin/main'.

  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working
    directory)
	  modified:   foo.rs

  no changes added to commit (use "git add" and/or "git commit -a")
  Dropped refs/stash@{0} (046ac112f8c02c3dc02984ad71d353a3e5be9a7a)
```

자동 병합이라니 좋은 소리입니다. 잘 된 것 같군요. 이제 파일을 보면 변경
사항이 스태시에서 나와 다시 적용된 것을 볼 수 있습니다. `foo.rs`는 "수정됨"
상태이며, 계속 작업하거나 추가하고 커밋할 준비가 됐습니다.

## 스태시 스택 {#the-stash-stack}

[i[Stash-->The stack]<]

[flw[스택 추상 자료형|Stack_(abstract_data_type)]]에 익숙하다면 `git stash
pop`을 읽는 순간 귀가 솔깃했을 것입니다.

그렇습니다. Git은 스택으로 스태시를 추적합니다. 스택이 익숙하지 않다면 먼저
관련 내용을 읽어 보세요.

* `git stash`는 작업 트리를 스태시 스택에 push합니다.
* `git stash pop`은 스태시 스택 맨 위 항목을 pop해 작업 트리에 적용합니다.
* `git stash list`는 현재 스태시 스택을 보여 줍니다.
* `git stash drop`은 특정 스태시 스택 항목을 삭제합니다.

따라서 `stash`한 뒤 다른 작업을 하고 다시 `stash`하면 스택에 스태시가 두
개 생깁니다.

``` {.default}
$ git stash list
  stash@{0}: WIP on main: 659b132 added repo1 another line
  stash@{1}: WIP on main: 659b132 added repo1 another line
```

스택 맨 위는 `stash@{0}`입니다.

그냥 `git stash pop`을 실행하면 인덱스 `0`인 맨 위 스태시를 가져와
스택에서 제거하고 작업 트리에 적용합니다.

스택 중간의 항목을 pop하고 싶다면 스태시 이름으로 지정할 수도 있습니다.

``` {.default}
$ git stash pop 'stash@{1}'
$ git stash pop --index 1       # same thing
```

마찬가지로 `stash drop`은 스택 맨 위 항목을 꺼내되 변경 사항을 작업 트리에
적용하지 **않고** 버립니다.

스택 중간의 항목을 버리고 싶다면 `stash drop`에도 특정 스태시 이름을 지정할
수 있습니다.

[i[Stash-->The stack]>]

## 충돌 {#conflicts}

[i[Stash-->Conflicts]<]

병합과 리베이스 중 생기는 충돌을 한참 읽었으니, 여기서 조금 걱정되기 시작할
수 있습니다.

스태시하고 pull한 다음 스태시를 pop했는데, 그 내용이 pull한 변경 사항과
충돌하면 어떻게 될까요? 그런 일이 생길 수 있을까요?

물론 생길 수 있습니다. 만세.

그런 일이 생기면 다음과 같이 보입니다.

``` {.default}
$ git stash pop
  Auto-merging foo.rs
  CONFLICT (content): Merge conflict in foo.rs
  On branch main
  Your branch is up to date with 'origin/main'.

  Unmerged paths:
    (use "git restore --staged <file>..." to unstage)
    (use "git add <file>..." to mark resolution)
	  both modified:   foo.rs

  no changes added to commit (use "git add" and/or "git commit -a")
  The stash entry is kept in case you need it again.
```

딱 봐도 병합 충돌이고, 편집기에서는 더욱 그렇게 보입니다.

``` {.rs .numberLines}
fn main() {
<<<<<<< Updated upstream
    println!("This is critically fixed");
=======
    println!("This is sorta working");
>>>>>>> Stashed changes
}
```

아래쪽에는 우리가 고치려고 했던 스태시된 변경 사항이 보이고, 이것이
업스트림에서 온 Chris의 수정과 충돌한다는 것도 알 수 있습니다.

그러니 병합할 때처럼 파일을 원하는 모습으로 편집해 *올바르게* 만들고
저장합니다. 하지만 아직 상태는 깨끗하지 않습니다.

``` {.default}
$ git status
  On branch main
  Your branch is up to date with 'origin/main'.

  Unmerged paths:
    (use "git restore --staged <file>..." to unstage)
    (use "git add <file>..." to mark resolution)
	  both modified:   foo.rs

  no changes added to commit (use "git add" and/or "git commit -a")
```

`git add`로 추가해 해결됐다고 표시합시다.

이 시점에는 두 가지 일이 일어날 수 있습니다.

1. pull한 버전만 받아들였다면(즉, 충돌한 내 변경 사항을 버렸다면) 새로
   일어나는 일은 없습니다. 결국 그 버전이 담긴 커밋은 이미 저장소에 있으므로
   Git은 이쯤에서 끝내도 된다는 것을 압니다. `git status`는 깨끗한 상태를
   보고합니다.

2. pull한 것과 다른 버전을 받아들였다면(즉, 내 변경 사항 일부 또는 전부를
   유지했다면) `git status`는 그 파일이 수정됐고 커밋을 위해 스테이징됐다고
   보고합니다.

   아직 커밋할 준비가 되지 않았다면 `git restore --staged`로 파일을
   스테이징 해제하세요. 그러면 파일은 단순히 수정된 상태가 되고, 커밋하기
   전에 더 작업할 수 있습니다.

**두 충돌 상황 모두 스태시한 변경 사항은 여전히 스태시에 남아 있습니다!**
분명 `stash pop`을 실행했지만 충돌이 생기면 스태시는 그대로 남고 실제로
pop되지 않습니다.

그 스태시가 더 필요 없다면(아마 그럴 것입니다) `git stash drop`으로 해당
스태시를 스택에서 버리고 깔끔하게 정리할 수 있습니다.

[i[Stash-->Conflicts]>]

## 새 파일 스태시하기 {#stashing-new-files}

[i[Stash-->New files]]

작업 트리에 새 파일을 추가했지만 현재 추적되지 않는 상태라면 어떨까요?
스태시가 이 파일을 볼 수 있을까요?

아닙니다. 먼저 파일을 추가해야 합니다. `git add`를 실행한 뒤(커밋은 하지
마세요!) 스태시하세요. 새 파일이 작업 트리에서 사라질 것입니다.

[i[Stash]>]
