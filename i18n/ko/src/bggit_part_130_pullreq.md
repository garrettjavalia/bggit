# GitHub: 포크와 풀 리퀘스트 {#github-forking-and-pull-requests}

[i[Fork]<]
[i[Pull requests]<]

여러분이 참여하는 많은 프로젝트에서는 모두가 사용하는 주 저장소에 쓰기 권한이
있을 수 있습니다. 즉, 공동 작업자로 추가되어 바로 push할 수 있습니다.

그렇다면 GitHub 저장소를 포크하거나 풀 리퀘스트를 만들 필요가 없습니다. 늘
하던 대로 계속 커밋하고 push하면 됩니다.

하지만 GitHub 저장소를 변경하고 싶은데 쓰기 권한이 없다면 어떨까요? 변경하고
싶은 저장소를 포크했을 때 이런 상황이 생길 수 있습니다. 내 포크에는 쓸 수
있지만 포크해 온 저장소에는 쓸 수 없습니다. 내 포크에서 만든 변경 사항을 원래
저장소에 어떻게 넣을까요?

살펴봅시다.

***포크***는 다른 사람의 GitHub 저장소에서 "Fork" 명령으로 GitHub에 만든
clone입니다. GitHub가 어느 저장소에서 포크했는지 추적하는 기록을 관리한다는
점만 빼면 평범한 clone입니다.

***업스트림***은 관례상 포크해 온 원격 저장소의 이름입니다. "업스트림"의 다른
정의와 충돌한다는 것을 압니다. 하지만 이 장에서는 포크의 맥락에서 이런
뜻이라고 합시다.

***풀 리퀘스트***(줄여서 "PR")는 내 포크에서 만든 변경 사항을 원래 저장소
소유자에게 제안하는 방법입니다.

> **포크와 풀 리퀘스트는 Git이 아니라 GitHub의 기능입니다.** GitHub가 웹
> 사이트에 구현해 제공하는 추가 기능입니다.

예를 들어 마음에 드는 오픈 소스 프로젝트를 찾았는데 버그가 있다고 합시다.
프로젝트의 GitHub 저장소에 쓸 권한이 없다면 어떻게 고칠 수 있을까요?

[i[Fork-->Process]]

풀 리퀘스트를 만드는 과정은 다음과 같습니다.

1. GitHub에서 저장소를 포크합니다. 이제 나만의 clone이 생겼습니다.
2. 내 저장소를 로컬 컴퓨터에 clone합니다. 이제 GitHub의 내 포크와 로컬
   컴퓨터의 clone, 두 개가 있습니다. (둘 다 내 소유입니다.)
3. 로컬 컴퓨터에서 수정하고 테스트합니다.
4. 수정 사항을 GitHub 포크에 push합니다.
5. GitHub에서 풀 리퀘스트를 만듭니다. 병합해 주길 바라는 변경 사항이 있다고
   업스트림 소유자에게 알립니다.
6. 업스트림 소유자가 GitHub에서 PR을 검토하고 병합할지 결정합니다. 원하면
   병합하고, 그렇지 않으면 댓글로 변경을 요청하거나 삭제합니다.
7. 작업이 끝났다면 선택적으로 포크를 삭제할 수 있습니다.

한번 해 봅시다! 아래에서 사용하는 제 예시 저장소에는 부담 없이 PR을 보내세요.
저는 그냥 삭제할 것입니다(병합하지 않습니다). 개인적인 일로 받아들이지 마세요.
모두 검토할 시간이 없을 뿐입니다.

## 포크 만들기 {#making-a-fork}

[i[Fork-->Creating]<]

[fl[제 테스트 저장소|https://github.com/beejjorgensen/git-example-repo]]로
가서 다음을 해 봅시다.

* 오른쪽 위의 "Fork" 버튼을 클릭합니다.
* "Owner"에서 사용자 이름을 선택합니다.
* "Repository name"에서 기본값을 선택하거나 새 이름을 만듭니다.
* 저장소에 브랜치가 여러 개 있고 포크에도 포함하고 싶다면 선택적으로 "Copy
  the main branch only" 체크박스를 해제합니다. 지금 하지 않아도 나중에 언제든
  직접 브랜치를 가져올 수 있습니다.
* "Create fork" 버튼을 클릭합니다.

> **아무 걱정 없이 해도 됩니다.** 내 포크를 만들었다는 사실을 원래 소유자는
> 알지 못합니다. 원래 저장소에 아무 피해 없이 삭제할 수 있습니다. 포크는 원래
> 저장소와 독립적이며 내가 소유한 GitHub상의 clone이라는 점을 기억하세요.

이제 내 포크의 프로젝트 페이지로 이동하고, 페이지의 작은 글씨에는 "forked
from beejjorgensen/git-example-repo"라고 표시됩니다.

이제 GitHub에 마음대로 다룰 수 있는 나만의 저장소 버전이 생겼습니다.

평소처럼 clone, pull, push하거나 저장소를 삭제하는 등 무엇이든 할 수 있습니다.
원래 저장소 소유자는 알지 못하며, 변경 사항은 내 저장소에만 영향을 줍니다.
나중에는 풀 리퀘스트를 보내 변경 사항을 원래 업스트림 저장소에 병합해 달라고
요청하는 방법을 살펴봅니다.

[i[Fork-->Creating]>]

## 변경 사항 만들기 {#making-your-changes}

변경 사항을 만들어 봅시다. 먼저 *내* 저장소, 즉 만든 포크를 로컬 컴퓨터에
clone해야 합니다.

평소처럼 "Code" 버튼을 열고 clone할 SSH 링크를 선택합니다(GitHub CLI를
사용한다면 그 방법을 써도 됩니다).

``` {.default}
$ git clone git@github.com:user/git-example-repo.git
  Cloning into 'git-example-repo'...
  remote: Enumerating objects: 4, done.
  remote: Counting objects: 100% (4/4), done.
  remote: Compressing objects: 100% (3/3), done.
  remote: Total 4 (delta 0), reused 4 (delta 0), pack-reused 0
  Receiving objects: 100% (4/4), done.
```

그런 다음 `cd`로 디렉터리에 들어가 파일을 볼 수 있습니다.

``` {.default}
$ cd git-example-repo
$ ls
  hello.py    README.md
```

`hello.py`를 다음과 같이 수정합시다.

``` {.py .numberLines}
#!/usr/bin/env python

print("Hello, world!")
print("This is my program!")
print("This is my modification")
```

파일을 추가하고 커밋한 뒤 push합시다.

``` {.default}
$ git add hello.py
$ git commit -m "modified"
  [main 5d3fe49] modified
   1 file changed, 1 insertion(+)
$ git push
  Enumerating objects: 5, done.
  Counting objects: 100% (5/5), done.
  Delta compression using up to 8 threads
  Compressing objects: 100% (3/3), done.
  Writing objects: 100% (3/3), 1.02 KiB | 1.02 MiB/s, done.
  Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
  To github.com:user/git-example-repo.git
     4332527..5d3fe49  main -> main
```

다시 말하지만 업스트림이 아니라 내 포크에만 push했습니다. GitHub의 내 포크
페이지에서 변경 사항을 볼 수 있습니다.

## 업스트림과 포크 동기화하기 {#syncing-the-upstream-with-your-fork}

[i[Fork-->Syncing with Upstream]<]

PR을 만들 준비가 됐나요? 잠깐만 기다리세요!

그사이 업스트림 소유자가 저장소를 변경했다면 어떨까요? 내 코드가 그쪽 코드의
최신 버전과 잘 작동하는지 확인하고 싶지 않나요?

물론 확인하고 싶을 것입니다.

내 포크 페이지에 펼쳐 볼 수 있는 "Sync fork" 버튼이 있다는 것을 눈치챘을 수
있습니다.

버튼을 열었을 때 "This branch is not behind the upstream"이라고 나오면
축하합니다! 이미 최신 상태입니다! 다음 절의 설명대로 PR을 만드세요.

"This branch is out-of-date"라고 나오며 "Update branch" 버튼이 보인다면
축하합니다! 최신 상태는 아니지만 충돌 없이 최신으로 만들 수 있습니다.
"Update branch"를 클릭한 뒤 다음 절처럼 PR을 만드세요. ("Discard" 버튼도
보일 수 있지만 변경 사항을 버리고 싶은 것이 아니라면 누르지 마세요!)

대신 "This branch has conflicts that must be resolved"라고 나온다면 나쁜
소식입니다. 내 저장소의 변경 사항이 업스트림에 있는 다른 사람의 변경 사항과
충돌합니다. 선택지는 다음과 같습니다.

* UI에서 풀 리퀘스트를 열 수 있다고 알려 줍니다. 다음 절의 설명대로
  브라우저에서 충돌을 해결할 기회가 생깁니다.
* 변경 사항을 그냥 버리고 업스트림 내용으로 바꿀 수도 있다고 알려 줍니다.
  안타깝군요.
* 또는 먼저 PR을 열지 않고 명령줄에서 업스트림의 브랜치를 내 브랜치에 실제로
  병합해 그곳에서 충돌을 해결할 수 있습니다. 아래의 [명령줄에서
  동기화하기](#sync-cl)를 보세요.

일반적으로 업스트림 저장소와 계속 동기화하는 것이 좋습니다. 작업하는 동안 내
변경 사항이 업스트림 변경 사항과 충돌하지 않는지 확인할 수 있습니다. 풀
리퀘스트를 보낼 준비가 됐을 때까지 기다렸다가 끝에서 충돌을 한꺼번에 해결하는
것보다 훨씬 낫습니다.

[i[Fork-->Syncing with Upstream]>]

## 풀 리퀘스트 만들기 {#making-a-pull-request}

[i[Pull request-->Creating]<]

포크를 만족스럽게 수정했으니 업스트림 관리자에게 공식 저장소로 받아들일
의향이 있는지 물을 수 있습니다.

> **아직 준비되지 않았을 수도 있습니다!** 답이 없거나 패치를 개선해 달라는
> 답이 와도 개인적으로 받아들이지 마세요. 양쪽 모두 만족스럽게 작업을
> 마치도록 소유자와 협력하세요.

시작합시다!

* "Contribute" 버튼을 누른 뒤 "Open pull request"를 클릭합니다.
* "This branch has conflicts that must be resolved"라는 문구가 있는지 찾습니다.
  이 문구가 있다면 업스트림에서 PR을 자동 적용할 수 없어 수동 작업이
  필요하다는 뜻입니다. 그냥 거절할 가능성이 훨씬 큽니다. 이를 피할 방법은
  다음과 같습니다.
  * PR을 열지 말고 돌아가 업스트림과 동기화하고 충돌을 고친 뒤 다시
    시도합니다.
  * 또는 UI의 "Resolve conflicts" 버튼을 클릭하고 브라우저 내 편집기로 직접
    해결합니다.
* 좋은 제목을 추가합니다.
* 훌륭한 설명을 추가합니다. 다른 사람에게 내 코드를 프로젝트에 넣어 달라고
  요청하는 것이므로, 검토하기 쉽도록 코드가 하는 일을 설명해야 합니다. (이
  예시는 제 저장소로 오므로 좋은 내용을 썼다고 치면 됩니다. 어차피 저는 PR을
  닫을 것입니다.)
* "Create pull request"를 클릭합니다!

그러면 PR 페이지로 이동합니다. 댓글을 더 추가하거나 마음이 바뀌었다면 요청을
닫을 수 있습니다.

업스트림 소유자는 PR이 들어온 것을 보게 되며, 이제 답을 기다려야 합니다.

개선이나 다른 질문을 담은 댓글로 답할 수도 있습니다. PR을 거절하고 병합하지
않은 채 닫을 수도 있습니다. 아니면 받아들일 수도 있죠! 신나는 날입니다!

[i[Pull request-->Creating]>]

## 반대편: 풀 리퀘스트 병합하기 {#flipside-merging-a-pull-request}

[i[Pull request-->Merging]<]

업스트림 소유자라면 누군가 PR을 보냈을 때 이메일과 GitHub 오른쪽 위 알림으로
통지를 받습니다(알림을 꺼 두지 않았다면 말이죠).

알림을 받으면 GitHub의 프로젝트 페이지로 가서 PR을 어떻게 할지 결정할 수
있습니다.

프로젝트 페이지 위쪽에는 현재 대기 중인 PR 수가 숫자로 표시된 "Pull
requests" 버튼이 있습니다.

* 버튼을 클릭해 목록을 봅니다.
* PR 제목을 클릭해 해당 PR로 이동합니다.

이제 PR을 보고 있습니다. 설명을 읽어 무슨 일을 하는지 확인하고, **매우
중요하게도** 코드를 검토하세요!

> **아마 모르는 사람이 작성한 코드를 받아들이려는 참입니다.** 이 세상의
> 대부분 사람은 친절하지만, 악성 코드를 끼워 넣어 여러분을 이용하려는 악의적
> 행위자(업계 용어로는 *개자식*)가 없다는 뜻은 아닙니다. 기여자를 1년 동안
> 알고 지냈더라도 긴 계획을 꾸미고 있을 수 있습니다. 그럴 리 없다고 생각한다면
> [flw[2024년에 일어난 XZ utils 해킹을 읽어 보세요|XZ_Utils_backdoor]].

코드를 검토하려면 설명 바로 아래의 기여자 아바타와 커밋 메시지를 보세요. 커밋
메시지를 클릭하면 [_Diff로 파일 비교하기_ 장](#diff)에서 설명한 diff가
보입니다. `+`로 표시된 줄은 추가됐고 `-`로 표시된 줄은 제거됐습니다.

기여자가 편집한 파일 자체를 그대로 보고 싶다면 오른쪽의 "..."를 누른 다음
"View file"을 클릭하세요.

거의 맞지만 수정이 필요하다면 "Edit file"을 눌러 PR에 직접 커밋을 추가할
수도 있습니다.

모두 괜찮아 보인다면 아래로 스크롤하세요. 바라건대 "This branch has no
conflicts with the base branch"와 "Merging can be performed automatically"라는
문구가 보일 것입니다. 좋은 소식입니다.

그렇게 표시된다면 "Merge pull request"를 클릭하기만 하면 변경 사항이 저장소에
추가되고 PR이 닫힙니다. 기여자에게 감사하는 댓글도 달면 좋습니다. 결국 무료로
작업을 제공해 준 셈이니까요!

> **PR을 닫아도 삭제되지는 않습니다.** 다시 열 수 있습니다.

하지만 PR이 충돌해 자동으로 병합할 수 없다고 합시다. GitHub가 "This branch
has conflicts that must be resolved"라고 불평하며 몇 가지 선택지를 줍니다.

업스트림 소유자는 "Resolve conflicts" 버튼을 클릭해 가능하다면 문제를 고칠 수
있습니다.

아니면 PR을 거절하고 연 사람에게 충돌을 해결해 달라고 요청해 자동 병합으로
내 삶을 조금 더 편하게 만들 수 있습니다.

[i[Pull request-->Merging]>]

## 브랜치로 여러 풀 리퀘스트 만들기 {#making-many-pull-requests-with-branches}

[i[Pull request-->With branches]<]

풀 리퀘스트에는 이런 특징이 있습니다. 하나를 만들면 브랜치의 모든 변경 사항을
하나로 묶습니다. 변경 사항이 서로 완전히 다른 일을 하더라도 모두 같은 PR에
들어갑니다.

관리 측면에서는 가끔 그리 좋지 않습니다. 이슈 #1용 PR과 이슈 #2용 PR을 따로
만들고 싶을 수도 있습니다!

그러려면 포크의 clone에서 PR마다 로컬 브랜치를 만들고 그 브랜치들을 포크에
push합니다. 그런 다음 PR을 만들 때 사용할 브랜치를 선택합니다. 브랜치 이름이
`feature1` 같은 것이어도 업스트림의 `main` 브랜치에 병합할 수 있습니다.

기능을 위한 새 브랜치를 만듭니다.

``` {.default}
$ git switch -c feature1
  Switched to a new branch 'feature1'
```

그런 다음 변경하고 추가한 뒤 커밋합니다.

``` {.default}
$ vim readme.txt
$ git add readme.txt
$ git commit -m "feature 1"
  [feature1 1ad9e92] feature 1
   1 file changed, 1 insertion(+)
```

그런 다음 원격 추적 브랜치를 설정하면서 내 저장소에 push합니다.

``` {.default}
$ git push -u origin feature1
  Enumerating objects: 5, done.
  Counting objects: 100% (5/5), done.
  Delta compression using up to 8 threads
  Compressing objects: 100% (2/2), done.
  Writing objects: 100% (3/3), 979 bytes | 979.00 KiB/s, done.
  Total 3 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
  remote: Resolving deltas: 100% (1/1), completed with 1 local
  remote: object.
  remote:
  remote: Create a pull request for 'feature1' on GitHub by
  remote: visiting:
  remote:      https://github.com/user/fork/pull/new/feature1
  remote:
  To github.com:user/fork.git
   * [new branch]      feature1 -> feature1
  branch 'feature1' set up to track 'origin/feature1'.
```

이제 GitHub로 돌아가 PR을 보낼 수 있습니다. (원격 저장소의 응답이 PR을 보낼
GitHub URL을 친절하게 알려 주는 것도 보세요!)

GitHub UI에는 "feature1 had recent pushes 4 minutes ago"라는 편리한 작은 팝업과
PR을 만들 수 있는 "Compare and pull request" 버튼이 있을 수 있습니다.

시간이 오래 지나 팝업이 사라졌어도 걱정하지 마세요. 왼쪽 위에 지금은 아마
"main"이라고 표시된 브랜치 선택 버튼이 보이나요? 버튼을 열고 PR을 만들
"feature1" 브랜치를 선택합니다. 그런 다음 "Contribute"를 클릭해 PR을 여세요.

PR 위쪽에는 병합 대상 저장소와 브랜치가 표시되고, 오른쪽에는 병합해 올 내
저장소와 브랜치 이름이 표시됩니다.

나머지 PR 과정은 평소와 같습니다.

**병합이 끝날 때까지 브랜치를 삭제하지 마세요!** 안전하게 병합되면 GitHub가
PR 페이지에 "Delete branch" 버튼을 표시합니다. 이 버튼은 GitHub의 브랜치를
삭제하지만 명령줄의 `feature1`과 `origin/feature1`은 직접 삭제해야 합니다.

[i[Pull request-->With branches]>]

## 풀 리퀘스트 삭제하기 {#deleting-a-pull-request}

[i[Pull request-->Deleting]<]

짧은 답: 할 수 없습니다.

긴 답: 할 수 있습니다.

아이러니하게도 짧은 답이 더 깁니다. 규칙은 제가 만든 게 아닙니다.

제대로 된 긴 답은 이렇습니다. 업스트림 소유자이고 PR에 민감한 정보가 있다면
할 수 있습니다.

포크한 사람이든 포크된 쪽이든 UI에서 PR을 삭제할 방법은 없습니다. 실수로
[flw[사회 보장 번호 078-05-1120|Social_Security_number#SSNs_used_in_advertising]]
같은 민감한 정보를 넣었다면 특히 낭패입니다.

하지만 희망이 완전히 사라진 것은 아닙니다! 업스트림 소유자는 GitHub의 가상
도우미를 찾아가 [fl[풀 리퀘스트 삭제를
요청할 수 있습니다|https://support.github.com/request?q=pull+request+removals]].
듣기로는 된다고 합니다. 직접 해 보지는 않았습니다.

포크한 사람이 자신이 만든 PR을 삭제할 방법이 있다면 저는 본 적이 없습니다.
업스트림 소유자에게 사정을 호소하고 삭제해 달라고 해야 합니다.

어쨌든 유출된 인증 정보는 지금 당장 반드시 바꾸고, 이를 교훈으로 삼아야
합니다.

[i[Pull request-->Deleting]>]

## 명령줄에서 동기화하기 {#sync-cl}

[i[Fork-->Syncing with Upstream]<]

GitHub에는 업스트림 변경 사항을 포크에 가져오는 멋진 Sync 버튼이 있으며,
반가운 추가 기능이었습니다. 예전에는 어려운 방법으로 해야 했습니다.

하지만 어려운 방법에는 장점이 하나 더 있습니다. 업스트림이 내 변경 사항과
충돌하면 PR을 만들기 전에 로컬에서 병합할 수 있습니다. GitHub UI에서는 충돌을
해결하려면 PR을 만들어야 합니다.

또한 명령줄을 좋아하고 업스트림을 내 브랜치에 빠르게 동기화하고 싶다면 이
방법을 쓸 수 있습니다.

계획은 다음과 같습니다.

1. 업스트림 저장소를 가리키는 `upstream` 원격 저장소를 추가합니다.
2. `upstream`에서 데이터를 fetch합니다.
3. 업스트림 브랜치를 내 브랜치에 병합합니다.
4. 충돌을 해결합니다.
5. 내 브랜치를 push합니다.
6. 이제 바라건대 충돌하지 않을 PR을 보냅니다.

해 봅시다. 내 `main` 브랜치에서 업스트림의 `main` 브랜치와 동기화를
시도하겠습니다. 충돌이 있을 때의 모습을 보여 드립니다. (충돌이 없다면 병합은
자동으로 성공합니다.)

가장 먼저, 아직 하지 않았다면 `upstream` 원격 저장소가 원래 소유자의 저장소를
가리키도록 설정합니다. 바로 포크해 온 저장소입니다. 여기에 push하지는 않을
것이므로 SSH나 HTTP 방식으로 접근할 수 있습니다. (이 원격 저장소에는 어떤
이름이든 붙일 수 있지만 `upstream`이 흔한 관례입니다.)

``` {.default}
$ git remote add upstream https://github.com/other/orig-repo.git
```

그런 다음 업스트림 저장소에서 새 커밋을 가져와 내 작업에 병합해야 합니다.

[i[Fetch]]

``` {.default}
$ git fetch upstream
  remote: Enumerating objects: 5, done.
  remote: Counting objects: 100% (5/5), done.
  remote: Compressing objects: 100% (1/1), done.
  remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
  Unpacking objects: 100% (3/3), 950 bytes | 950.00 KiB/s, done.
  From https://github.com/other/upstream
   * [new branch]      main       -> upstream/main

$ git switch main   # Make sure we're on the main branch

$ git merge upstream/main
  Auto-merging readme.txt
  CONFLICT (content): Merge conflict in readme.txt
  Automatic merge failed; fix conflicts and then commit the result.
```

(원한다면 리베이스해도 됩니다.)

이제 파일을 편집해 충돌을 해결하고 평소처럼 해결 과정을 마칩니다.

그런 다음 GitHub의 내 포크로 다시 push합니다!

``` {.default}
$ git push
  Enumerating objects: 7, done.
  Counting objects: 100% (7/7), done.
  Delta compression using up to 8 threads
  Compressing objects: 100% (2/2), done.
  Writing objects: 100% (3/3), 999 bytes | 999.00 KiB/s, done.
  Total 3 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
  remote: Resolving deltas: 100% (1/1), completed with 1 local
  remote: object.
  To github.com:user/fork.git
   8b2476c..c8a7e0a  main -> main
```

이제 GitHub UI로 돌아가 PR을 열면 "These branches can be automatically
merged"라고 나올 것입니다. 모두의 귀에 음악처럼 들리는 말입니다.

`upstream` 원격 저장소를 설정하고 나면 이후 동기화할 때는 [i[Fetch]]
`git fetch upstream`을 실행한 뒤 내 작업을 병합하거나 리베이스하기만 하면 됩니다.

[i[Fork-->Syncing with Upstream]>]

[i[Pull requests]>]
[i[Fork]>]
