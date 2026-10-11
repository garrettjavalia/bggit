# 파일 상태 {#file-states}

[i[File States]<]

지나가면서 이미 이 이야기를 꽤 많이 했습니다.

새 파일을 만들었다면 커밋하기 전에 `git add`로 스테이징 영역에 추가해야 합니다.

파일을 수정했다면 커밋하기 전에 `git add`로 스테이징 영역에 추가해야 합니다.

`foo.txt`를 스테이징 영역에 추가했다면 커밋하기 전에 `git restore --staged foo.txt`로 스테이징 영역에서 뺄 수 있습니다.

그러므로 파일은 여러 “상태”에 있을 수 있고, 그 상태 사이를 옮길 수 있음이 분명합니다.

파일이 어떤 상태인지 알아내고 그 상태에서 한 일을 “되돌리는” 방법에 관한 힌트를 얻을 때는 `git status`가 최고의 친구입니다(이름 변경만은 예외입니다. 그 난장판 이야기는 곧 하겠습니다).

## Git 파일이 가질 수 있는 상태 {#what-states-can-files-in-git-be-in}

**추적되지 않음(Untracked)**, **수정되지 않음(Unmodified)**, **수정됨(Modified)**, **스테이징됨(Staged)**의 네 가지입니다.

* [i[File States-->Untracked]] **추적되지 않음**: Git이 이 파일에 관해 아무것도 모릅니다(예를 들어 작업 트리에 방금 만들었고 아직 추가하지 않은 파일). Git은 이를 무시하지만 상태 출력에는 나타냅니다.

  `git add`로 스테이징됨 상태로 옮기면 Git이 이 파일을 알게 할 수 있습니다.

  파일이 필요 없다면 그냥 삭제해도 되고, 그 자리에 두되 Git이 계속 무시하게 하려면 `.gitignore`에 추가해도 됩니다.

* [i[File States-->Unmodified]] **수정되지 않음**: Git이 이 파일을 알고 있고 저장소에도 들어 있습니다. 하지만 마지막 커밋 이후에는 바꾸지 않았습니다.

  파일을 바꾸고 저장하면 수정됨 상태로 옮길 수 있습니다.

  `git rm`으로 파일을 제거하면 제거된 파일은 스테이징됨 상태가 됩니다. (잠깐, 파일을 제거했는데 스테이징 영역에 올라간다고요? 맞습니다! 나중에 더 설명합니다.)

* [i[File States-->Modified]] **수정됨**: Git이 이 파일을 알고 있으며 파일이 바뀌었다는 것도 압니다. 이제 변경 사항을 스테이징하거나 되돌릴 수 있습니다.

  `git add`로 파일을 스테이징됨 상태로 바꿀 수 있습니다.

  `git restore`로 파일을 수정되지 않음 상태로 바꿀(변경 사항을 버릴) 수 있습니다.

* [i[File States-->Staged]] **스테이징됨**: 다음 커밋에 포함할 준비가 된 파일입니다.

  `git commit`으로 커밋하면 수정되지 않음 상태로 바꿀 수 있습니다.

  `git restore --staged`로 파일을 스테이징 영역에서 빼고 수정됨 상태로 되돌릴 수 있습니다.

[i[Workflow-->File states]] 파일을 저장소에 추가할 때는 보통 다음 과정을 거칩니다.

1. 사용자가 새 파일을 만들고 저장합니다. 이 파일은 **추적되지 않음** 상태입니다.

2. 사용자가 `git add`로 파일을 추가합니다. 이제 파일은 **스테이징됨** 상태입니다.

3. 사용자가 `git commit`으로 파일을 커밋합니다. 이제 파일은 **수정되지 않음** 상태이고 저장소의 일부가 되어 사용할 준비가 끝났습니다.

저장소에 들어간 뒤의 일반적인 파일 생명 주기는 첫 단계만 다릅니다.

1. 사용자가 파일을 바꾸고 저장합니다. 이제 파일은 **수정됨** 상태입니다.

2. 사용자가 `git add`로 파일을 추가합니다. 이제 파일은 **스테이징됨** 상태입니다.

3. 사용자가 `git commit`으로 파일을 커밋합니다. 이제 파일은 **수정되지 않음** 상태이고 저장소의 일부가 되어 사용할 준비가 끝났습니다.

한 커밋에는 서로 다른 파일의 여러 변경 사항이 묶이는 경우가 많다는 점을 기억하세요. 한 번 커밋하기 전에 그 파일들을 모두 스테이징 영역에 추가합니다.

상태를 바꾸는 방법의 일부는 다음과 같습니다.

* **추적되지 않음** → `git add foo.txt` → **스테이징됨**(“새 파일”로)
* **수정됨** → `git add foo.txt` → **스테이징됨**
* **수정됨** → `git restore foo.txt` → **수정되지 않음**
* **수정되지 않음** → `foo.txt` 편집 → **수정됨**(좋아하는 편집기로)
* **스테이징됨** → `git commit` → **수정되지 않음**
* **스테이징됨** → `git restore --staged` → **수정됨**

다시 말하지만, `git status`는 상태 변경을 되돌리는 방법을 자주 알려 줍니다.

## 수정되지 않음에서 추적되지 않음으로 {#unmodified-to-untracked}

[i[Untracking files]]

`git rm`의 한 변형을 쓰면 파일을 저장소에서는 제거하면서 작업 트리에는 그대로 둘 수 있습니다. 파일은 보관하고 싶지만 Git이 더는 추적하지 않게 하고 싶은 경우입니다.

이렇게 하려면 `--cached` 옵션을 사용합니다.

다음은 저장소에서 `foo.txt`를 제거하되 작업 트리에는 남겨 두는 예입니다.

``` {.default}
$ ls
  foo.txt

$ git rm --cached foo.txt
  rm 'foo.txt'

$ git status
  On branch main
  Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
	  deleted:    foo.txt

  Untracked files:
    (use "git add <file>..." to include in what will be committed)
	  foo.txt

$ ls
  foo.txt
```

`status` 출력에서 Git이 파일 삭제를 스테이징했지만, 그 파일이 존재하며 추적되지 않는다고도 표시하는 것을 볼 수 있습니다. 뒤이어 실행한 `ls`도 파일이 여전히 존재함을 보여 줍니다.

이 시점에 커밋하면 파일은 추적되지 않음 상태가 됩니다.

## 여러 상태에 동시에 있는 파일 {#files-in-multiple-states}

[i[File States-->Multiple]]
실제로 파일은 어느 정도 여러 상태에 동시에 있을 수 있습니다. 기술적으로 더 정확히 말하면 서로 다른 상태인 파일 사본들이 있을 수 있습니다. 예를 들어 스테이징 영역에는 한 버전이 있고, 작업 트리에는 다르게 수정한 또 다른 버전이 *동시에* 있을 수 있습니다. 담긴 데이터가 같지 않으므로 기술적으로는 실제로 서로 다른 파일입니다.

파일을 스테이징하면 그 순간 파일의 **사본**을 스테이징하는 셈이라는 점만 기억하세요. 작업 트리의 파일을 다시 수정하는 것을 막는 것은 없습니다. 그러면 다음처럼 커밋할 준비가 된 한 버전은 스테이징 영역에 있고, 아직 스테이징하지 않은 변경이 더 들어간 다른 버전은 작업 트리에 있게 됩니다.

``` {.default}
$ git status
  On branch main
  Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
	  modified:   foo.txt

  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working
    directory)
	  modified:   foo.txt
```

파일을 다시 추가하면 스테이징 영역의 버전을 덮어쓸 수 있습니다. 그리고 여러 형태로 `restore`를 주문처럼 외우고 실행하면 파일을 각기 다르게 바꿀 수 있습니다. `git restore`의 `--staged`와 `--worktree` 옵션을 찾아보세요.

이렇게 여러 상태가 동시에 있을 때 파일을 옮기는 방법은 독자 여러분의 연습 문제로 남기겠습니다. 다만 이런 일이 가능하다는 사실만큼은 알려 드리고 싶었습니다.

[i[File States]>]
