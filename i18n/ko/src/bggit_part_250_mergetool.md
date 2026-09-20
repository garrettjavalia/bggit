# Mergetool {#mergetool}

[i[Mergetool]<]

병합 충돌이 일어날 때 Git이 파일에 넣는 `>>>>>`, `=====`, `<<<<<` 같은 표시가 몹시 싫은가요?

그렇다면 여러분이 찾는 것은 _병합 도구_일 수 있습니다. 병합 도구는 여러분의 변경 사항, 충돌하는 변경 사항, 원하는 병합 결과를 그래픽 화면으로 보여 줍니다. 그리고 이해하기 쉬운 형태로 제시합니다.

> **개인적으로 저는 병합 도구를 좋아하지 않습니다.** 터무니없는 말처럼 들리겠지만 잠깐 설명해 보겠습니다. 병합 충돌이 생겼을 때 해야 할 일은 `=====` 구분자가 들어 있는 파일을 편집해 _올바른_ 상태로 만드는 것뿐이라는 점을 기억하시나요? 구분자를 걷어 내면서 파일이 올바르게 될 때까지 수정해야 합니다.
>
> 여러분과 파일만 있을 뿐입니다. 내용을 건드리는 중간자는 없습니다. 작업을 끝내면 남는 것이 바로 최종 답입니다.
>
> 하지만 병합 도구는 본질적으로 중간자입니다. 일을 제대로 끝내려면 우리가 그 도구를 올바르게 사용하고 있다고 믿어야 합니다. 저로서는 아마 올바르게 사용했더라도, 결과가 _올바른지_ 확인하려고 다시 직접 살펴봐야 할 것 같은 느낌이 듭니다.
>
> 제가 인정하는 장점은, 충돌 파일을 편집할 때 사실상 위아래로 변경 사항을 보는 것과 달리 병합 도구에서는 대개 변경 사항을 나란히 볼 수 있다는 것입니다. 파일 안에 크고 충돌하는 덩어리가 여러 개 있을 때는 이 덕분에 병합 도구가 더 사용하기 쉬울 수 있습니다.
>
> 하지만 실제로 저는 병합 도구를 전혀 쓰지 않습니다. 또한 실제로 *아주 많은* 사람이 병합 도구를 사용합니다.

## 병합 도구의 동작 {#merge-tool-operations}

병합 도구는 파일별로 작동합니다. 따라서 병합 도구를 사용할 때는 충돌이 난 특정 파일 하나에 사용하게 됩니다.

어떤 도구든 대개 적어도 세 개의 패널을 보여 줍니다.

* 충돌하는 여러분의 변경 사항
* 충돌하는 상대방의 변경 사항
* _올바른_ 결과 파일

그리고 핵심 동작도 대체로 같습니다.

* **다음 충돌로 이동**—모든 패널이 다음 충돌로 이동합니다.
* **이전 충돌로 이동**
* **내 것 선택**—충돌하는 _여러분의_ 변경 사항을 최종 결과에 복사합니다. 즉, 여러분의 변경 사항이 _올바른_ 것입니다.
* **상대 것 선택**—충돌하는 _상대방의_ 변경 사항을 최종 결과에 복사합니다. 즉, 상대방의 변경 사항이 _올바른_ 것입니다.

병합 도구가 실행될 때 첫 번째 충돌에서 시작한다고 가정하면 다음과 같이 사용합니다.

1. "내 것"이나 "상대 것" 중 하나를 선택해 _올바른_ 변경 사항을 남깁니다.
2. 다음 충돌로 이동합니다.
3. 모든 충돌을 해결할 때까지 1단계부터 반복합니다.

모든 충돌을 훑으며 둘 중 하나를 골랐다면 최종 결과가 _올바른지_ 확인하고 결과를 저장하거나 완료합니다.

병합 도구는 결과를 대신 스테이징해 두므로, 커밋하여 병합을 끝낼 준비가 됩니다.

## 병합 도구 예시 {#some-example-merge-tools}

도구는 아주 많습니다. 여기서는 몇 가지 링크를 알파벳순으로 싣겠습니다. 별도 표시가 없다면 여러 플랫폼에서 실행됩니다.

* [fl[Araxis Merge|https://www.araxis.com/merge/index.en]]—Windows, Mac
* [fl[Beyond Compare|https://www.scootersoftware.com/]]
* [fl[Code Compare|https://www.devart.com/codecompare/]]—Windows
* [fl[KDiff3|https://invent.kde.org/sdk/kdiff3]]
* [fl[Meld|https://meldmerge.org/]]
* [fl[P4Merge|https://www.perforce.com/products/helix-core-apps/merge-diff-tool-p4merge]]
* [fl[Vimdiff|https://www.vim.org/]]
* [fl[WinMerge|https://winmerge.org/]]—Windows

또한 VS Code와 IntelliJ 같은 IDE에는 Git과 독립적으로 작동하는 자체 내장 병합 도구가 흔히 있습니다(Git에서 아무것도 설정할 필요가 없습니다).

## Vimdiff를 병합 도구로 사용하기 {#using-vimdiff-as-a-merge-tool}

[i[Mergetool-->With Vimdiff]<]

Vimdiff는 필요한 기능을 모두 갖췄고 설정이 조금 까다로우므로, 이를 병합 도구로 사용하는 과정을 빠르게 살펴보겠습니다. 다른 서드 파티 도구도(VS Code 및 이 기능이 내장된 다른 IDE는 제외) 설정 방식이 비슷합니다. 다른 도구의 올바른 설정법은 인터넷에서 검색하세요.

> **이것은 Vim 튜토리얼이 아닙니다.** 따라서 파일 저장이나 종료 같은 작업은 할 줄 안다고 가정하겠습니다. Vim에서 창을 바꾸려면 `CTRL-W`를 누른 다음 커서 방향을 누릅니다. 예를 들어 왼쪽 창으로 이동하려면 `CTRL-W` 다음에 `h`를 누릅니다.

무엇보다 먼저 설정부터 해 봅시다.

``` {.default}
$ git config --global set merge.tool vimdiff
$ git config --global set mergetool.vimdiff.cmd \
                             'vimdiff "$LOCAL" "$REMOTE" "$MERGED"'
$ git config --global set difftool.vimdiff.cmd \
                             'vimdiff "$LOCAL" "$REMOTE"'
```

(책 여백에 맞추려고 긴 명령을 나눴습니다. 한 줄에 써도 됩니다.)

마지막 줄은 `vimdiff`와 `difftool`에 두 패널 보기를 명시적으로 설정합니다. 이 설정이 없으면 `mergetool.vimdiff.cmd` 지시문 때문에 `difftool`이 세 패널로 표시됩니다. 아마 원하는 모습은 아닐 것입니다.

설정을 마쳤으니 병합 충돌이 생겼다고 해 봅시다.

``` {.default}
$ git merge branch
  Auto-merging foo.txt
  CONFLICT (content): Merge conflict in foo.txt
  Automatic merge failed; fix conflicts and then commit the result.
```

이 시점에는 흔히 볼 수 있는 전형적인 병합 충돌 상태입니다.

``` {.default}
$ git status
  On branch main
  You have unmerged paths.
    (fix conflicts and run "git commit")
    (use "git merge --abort" to abort the merge)

  Unmerged paths:
    (use "git add <file>..." to mark resolution)
	  both modified:   foo.txt

  no changes added to commit (use "git add" and/or "git commit -a")
```

병합 도구를 설정했으니 사용해 봅시다.

``` {.default}
$ git mergetool
```

> **Git이 병합 도구를 정말 실행할지 묻는다면**(`git mergetool`을 방금 실행했으니 아마 당연히 원하겠지요), 다음 설정 명령으로 그 "기능"을 끌 수 있습니다.
>
> ``` {.default}
> $ git config --global set mergetool.prompt false
> ```

<!-- ` -->

그러면 패널 세 개가 있는 Vim 창이 나타납니다. 왼쪽은 로컬 변경 사항, 가운데는 저장소에 존재하는 파일, 오른쪽은 병합 결과입니다.

목표는 오른쪽 내용을 _올바르게_ 만드는 것입니다. 물론 그곳에서 직접 파일을 수정해도 되지만, 그러면 애초에 병합 도구를 왜 쓰겠습니까?

따라서 앞에서 정리한 단계를 따르겠습니다.

`git mergetool`을 처음 실행하면 커서가 왼쪽 창에 놓인 채 첫 번째 충돌로 이동합니다. 왼쪽 창에는 우리가 만든 변경 사항이 있습니다.

가운데 창에서는 저장소에 있는 그에 대응하는 변경 사항을 볼 수 있습니다.

오른쪽 창에는 작업을 마쳤을 때 스테이징될 내용이 보입니다. 지금은 오른쪽 창에 `=====`와 `<<<<<` 같은 것들이 모두 보입니다. 하지만 잠시 뒤 바꿀 것입니다.

*커서를 오른쪽 창으로 옮기세요.* 여기서 실제 작업이 이루어집니다.

커서가 강조된 구역(아마 여러 색으로 표시될 것입니다)에 있는지 확인하세요. 이 강조된 구역이 교체할 대상입니다.

어느 변경 사항을 사용할지 골라 봅시다.

여러분의 변경 사항을 남기고 저장소의 변경 사항을 버리려면 다음 Vim 명령을 사용하세요.

``` {.default}
:diffget LOCAL
```

여러분의 변경 사항을 버리고 저장소의 변경 사항을 남기려면 다음 명령을 사용하세요.

``` {.default}
:diffget REMOTE
```

둘 중 하나를 실행하면 오른쪽 창의 내용이 원하는 것으로 바뀝니다.

그런 다음 `]c`로 다음 충돌로 이동할 수 있습니다. (이전 충돌로 이동하려면 `[c`를 사용합니다.)

오른쪽 창이 _올바르게_ 될 때까지 반복하세요. 오른쪽 창을 원하는 만큼 직접 편집해도 됩니다.

끝났으면 오른쪽 창을 저장하고 모든 창을 종료합니다.

**주의할 점은** 병합 도구를 종료하는 순간 Git이 맨 오른쪽 창에 저장된 것을 무엇이든 스테이징한다는 것입니다. 너무 일찍 종료해 작업을 마치기 전에 내용이 스테이징되었다면 해당 파일에 `git checkout --merge`를 사용하여 스테이징 영역에서 빼고 "both modified" 상태로 되돌리세요.

충돌 파일이 여러 개라면 Git은 다음 파일을 처리하도록 병합 도구를 다시 엽니다.

모든 작업이 끝나면 변경 사항이 적용되며, 평소처럼 커밋하여 병합을 완료할 수 있습니다.

그런데 잠깐만요. 전에는 없던 저 `.orig` 파일은 무엇일까요? 계속 읽어 보세요!

[i[Mergetool-->With Vimdiff]>]

## 원본 백업하기 {#backing-up-the-originals}

[i[Mergetool-->File backups]]

기본적으로 병합 도구를 사용하면 Git은 병합 도구가 건드리기 전의 파일을 백업으로 보관합니다. 다음처럼 `.orig` 확장자가 붙은 파일을 볼 수 있습니다.

``` {.default}
foo.txt.orig
bar.txt.orig
```

원한다면 이를 `.gitignore`에 추가할 수 있고, 다음 설정 변수로 애초에 생성되지 않게 할 수도 있습니다.

``` {.default}
$ git config --global set mergetool.keepBackup false
```

[i[Mergetool]>]
