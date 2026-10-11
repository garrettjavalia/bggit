# 구성 {#configuration}

[i[Configuration]<]

이 책의 아주아주 먼 앞부분에서 Git 구성을 조금 설정했습니다. 다음과 같이
했었죠.

``` {.default}
$ git config set --global user.name "Your Name"
$ git config set --global user.email "your-email@example.com"
```

이 명령을 실행하면 구성 정보가 파일에 추가되고, 그 파일의 정보가 시스템의
모든 Git 저장소에 적용됩니다.

물론 로컬 구성으로 덮어쓰지 않는 한 그렇습니다. 이 내용은 잠시 후 더
살펴보겠습니다.

> **어떤 분은 "잠깐, 전에는 `git config`로 변수를 설정할 때 `set`을 쓴 적이
> 없는데요!"라고 말할지도 모릅니다.** 맞습니다. 하지만 그것은 더 이상
> 권장되지 않는 이전 `git config` 사용법입니다. 오래된 Git 버전에서는 그
> 방식만 쓸 수 있을지도 모릅니다. 새 사용법에서 오류가 발생한다면 아래의
> [이전 Git 버전](#config-old)을 보세요.

그 명령 중 한 줄을 다시 살펴봅시다.

``` {.default}
$ git config set --global user.name "Your Name"
                              ↑          ↑
                          variable     value
```

이 줄에는 중요한 요소가 두 가지 있습니다.

1. *변수*, 즉 값을 설정하려는 대상입니다.
2. *값*, 즉 그 변수에 넣는 값입니다.

이 경우 변수는 `user.name`이고 값은 `"Your Name"`입니다.

> **`user.name`과 `user.email`, 이 두 변수가 하는 일은** 커밋 메시지에 들어갈
> 값을 설정하는 것입니다! 커밋할 때 쓰이는 여러분의 신원이죠! 여기에 다른
> 사람의 이름과 이메일을 적기만 하면 세상 누구든 놀랄 만큼 쉽게 사칭할 수
> 있다는 점도 덧붙여 둡니다. 이를 완화하는 방법 중 하나는 [fl[커밋에 디지털
> 서명하기|https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work]]이며,
> [신원 변경하기](#changing-identity) 장에서 조금 더 알아볼 수 있습니다.

이 장의 명령에서 오류가 발생한다면 아래의 [이전 Git 버전](#config-old) 절을
보세요.

## 로컬 구성 {#local-configuration}

[i[Configuration-->Local]<]

위의 `git config` 명령에서 `--global` 스위치를 눈여겨봤을 수도 있습니다.

``` {.default}
$ git config set --global user.name "Your Name"
```

명시적으로 `--global`이라고 하지 않으면 Git은 로컬 구성을 뜻한다고
가정합니다.

로컬 구성이란 무엇일까요? 현재 들어 있는 저장소에만 적용되고 다른
저장소에는 적용되지 않는 구성입니다.

*로컬 구성의 구성 옵션은 전역 구성을 덮어씁니다!*

왜 이렇게 설정하는지 실용적인 예를 들어 봅시다. 취미 프로젝트에 쓰는 개인
이메일과 유료 업무에 쓰는 계약자 이메일이 있다고 합시다. 독립 계약자라서 이
모든 프로젝트가 한 컴퓨터에 있습니다.

하지만 계약 업무에는 업무용 신원(이름과 이메일)을 쓰고, 취미 작업에는 해커
신원을 쓰고 싶습니다.

한 가지 방법은 다음 값을 전역으로 설정하는 것입니다.

``` {.default}
$ git config set --global user.name "HAx0rBYnit3"
$ git config set --global user.email "l333T@example.com"
```

이 값이 모든 저장소의 기본값이 됩니다. 그리고 업무용 새 저장소가 생겼다고
합시다.

``` {.default}
$ git init corporate_job_12
  Initialized empty Git repository in /user/corporate_job_12/.git/
```

그곳으로 들어가 해당 저장소에만 로컬 구성을 설정합니다(`--global`을 지정하지
않았으므로 로컬입니다).

``` {.default}
$ cd corporate_job_12
$ git config set user.name "Professional Name"
$ git config set user.email "professional@example.com"
```

이제 `corporate_job_12` 디렉터리에서만 커밋에 업무용 이름과 이메일을
사용합니다. 다른 모든 곳에서는 최정예 해커 이름을 사용합니다.

로컬 구성을 지정하면 모든 전역 구성을 저장소별로 덮어쓸 수 있습니다.

마지막으로 저장소의 로컬 구성은 저장소 루트 디렉터리의 `.git/config` 파일에
있습니다.

[i[Configuration-->Local]>]

## 현재 구성 나열하기 {#listing-the-current-config}

[i[Configuration-->Listing]<]

`git config list`로 현재 구성을 볼 수 있습니다. 전역 구성을 보고 싶다면
`--global` 플래그를 추가하세요.

``` {.default}
$ git config list
  user.name=HAx0rBYnit3
  user.email=l333T@example.com
  init.defaultbranch=main
  core.repositoryformatversion=0
  core.filemode=true
  core.bare=false
  core.logallrefupdates=true
  user.name=Professional Name
  user.email=professional@example.com
```

`user.name`과 `user.email`이 두 번 나오는 것을 볼 수 있습니다. 첫 번째 값은
전역 구성에서 왔으며, 뒤에 나오는 로컬 구성의 값이 이를 덮어씁니다.

[i[Configuration-->Listing]>]

## 변수 조회, 설정, 삭제하기 {#getting-setting-and-deleting-variables}

[i[Configuration-->Get]]
다음은 "조회" 예시입니다.

``` {.default}
$ git config get user.name
  Professional Name
```

`git config list`에서는 전역 값과 로컬 값이 모두 보였지만, 여기서는 활성
값(이 경우 로컬 값)만 반환한다는 점에 유의하세요.

[i[Configuration-->Set]]
"설정"은 이미 살펴봤습니다.

``` {.default}
$ git config set user.name "Harvey Manfrengensenton"
```

큰따옴표는 셸이 이름을 인수 하나로 전달하게 합니다. 보통 셸은 공백을 기준으로
인수를 나눕니다. 작은따옴표도 사용할 수 있으며, 값에 셸 특수 문자가 있을 때
유용합니다. 셸 애호가 여러분께 사과하며 터무니없이 단순화한 규칙을 말하자면,
값에 공백이 있으면 따옴표로 감싸세요.

설정 명령은 기존 `user.name` 변수 값을 덮어씁니다.

[i[Configuration-->Delete]]
[i[Configuration-->Unset]]
마지막으로 `unset`을 사용해 변수를 삭제할 수 있습니다.

``` {.default}
$ git config unset user.name
```

## 자주 쓰는 변수 {#some-popular-variables}

[i[Configuration-->Commonly-set variables]<]

설정할 수 있는 변수를 알아보려면 해당 명령의 매뉴얼 페이지를 보세요. 즐겨
쓰는 검색 엔진에서 `man git whatever`를 검색해 첫 번째 결과를 보면 대개 찾을
수 있습니다. 예를 들어 `man git pull`을 검색해 첫 결과를 열면 `git pull`의
구성 변수를 찾을 수 있습니다.

또한 [fl[`git config` 매뉴얼 페이지에는 둘러볼 수 있는 아주 긴 변수
목록이 있습니다|https://git-scm.com/docs/git-config#_variables]].

그중 재미있고 자주 쓰는 항목은 다음과 같습니다.

변수            | 설명
----------------|----------------------------------------------------------
`user.name`        | 사용자 이름
`user.email`       | 사용자 이메일
`pull.rebase`      | pull할 때 리베이스를 시도하려면 `true`, 병합을 시도하려면 `false`로 설정합니다.
`core.editor`      | 커밋 메시지 등에 사용할 기본 편집기입니다. `vim`, `nano`, `code`, `emacs` 등으로 설정합니다.
`merge.tool`       | 기본 병합 도구입니다. 예: `meld` 등.
`diff.tool`        | 기본 diff 도구입니다. 예: `vimdiff`.
`difftool.prompt`  | Git이 diff 도구를 실행할지 매번 묻지 않게 하려면 `false`로 설정합니다.
`color.ui`         | Git 출력을 더 다채롭게 표시하려면 `true`로 설정합니다.
`core.autocrlf`    | Windows를 사용하고 **WSL은 사용하지 않으며**, 원격 저장소에는 Unix 방식 줄바꿈이 있고, 작업 트리에서는 Windows 방식 줄바꿈을 쓰고 싶다면 `true`로 설정합니다. 다른 시스템에서는 `input`으로 설정하세요. 모두 Windows의 유서 깊은 줄바꿈 문제를 우회하기 위한 것입니다.
`commit.gpgsign`   | [GPG 커밋 서명](#gpg-signing)을 구성했고 항상 서명하려면 `true`로 설정합니다.
`help.autocorrect` | 명령을 잘못 입력했을 때 Git이 의도했다고 생각하는 명령을 표시하려면 `0`으로 설정합니다. 수정된 명령을 즉시 실행하려면 `immediate`, 실행할지 물어보게 하려면 `prompt`로 설정합니다.

다시 말하지만 이런 변수는 *훨씬* 많습니다. 문서에서 더 둘러보세요.

[i[Configuration-->Commonly-set variables]>]

## 구성 직접 편집하기 {#editing-the-config-directly}

[i[Configuration-->Editing directly]<]

편집기(`core.editor` 변수에 지정된 편집기)를 실행해 구성 파일을 직접 편집할
수 있습니다. 어떤 사람에게는 이 방법이 더 쉬울 수 있습니다.

다음과 같이 편집기를 실행할 수 있습니다.

``` {.default}
$ git config edit
```

전역 구성 파일을 편집하려면 `--global` 플래그를 추가하세요.

편집기에 들어가면 다음과 비슷한 구성 파일이 보입니다.

``` {.default .numberLines}
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true

[user]
    name = Your Name
    email = user@example.com
```

살펴보면 `user.name`과 `user.email`이 어디에 들어갔는지 알 수 있습니다.
구성 파일은 이런 식으로 구성됩니다.

여기서 편집하고 변경 사항을 저장할 수 있습니다. 어떤 사람에게는 명령줄에서
변수를 추가하거나 수정하는 것보다 이 방법이 더 쉬울 수 있습니다.

> **부주의한 편집으로 구성을 망가뜨리면 흥미로운 시간을 보내게 됩니다.**
> `git config edit`을 다시 실행할 수 없게 됩니다. 즐겨 쓰는 텍스트 편집기에서
> 구성 파일을 직접 고쳐야 합니다.
>
> 로컬 구성 파일은 해당 저장소의 루트 디렉터리를 기준으로 `.git/config`에
> 있습니다.
>
> 전역 구성 파일은 Unix 계열 시스템에서는 `~/.gitconfig`, Windows에서는
> `C:\Users\YourName\.gitconfig`에 있습니다.
>
> 편집기에서 해당 파일을 열어 실수를 고치고 저장하면 `git config edit`이
> 다시 작동할 것입니다.

[i[Configuration-->Editing directly]>]

## 조건부 구성 {#conditional-configuration}

[i[Configuration-->Conditional]<]

자세히 이야기하고 싶은 범위를 넘지만, 짚고 넘어갈 만큼 멋진 기능입니다.

Git 구성 파일에는 다른 구성 파일을 *포함*할 수 있습니다. 구성 파일이 난장판이
됐을 때 논리적으로 나눌 방법을 제공합니다.

*조건부 포함*도 가능합니다. 즉, 어떤 조건이 참인지에 따라 파일을 포함하도록
선택할 수 있습니다.

검사할 수 있는 조건은 다음과 같습니다.

* 이 저장소가 있는 디렉터리
* 특정 브랜치에 있는지 여부
* 특정 원격 저장소가 구성돼 있는지 여부

이 기능은 온갖 강력한 능력을 제공합니다. 개인적으로는 제게 필요한 수준을
넘어서서 한 번도 써 본 적이 없지만, 그건 어디까지나 제 이야기입니다.

[fl[공식 책에서 더 많은 정보와 예시를
확인하세요|https://git-scm.com/docs/git-config#_conditional_includes]].

[i[Configuration-->Conditional]>]

## 이전 Git 버전 {#config-old}

[i[Configuration-->Deprecated usage]<]

최신 버전의 Git이 설치돼 있다고 가정했습니다. 그렇지 않다면 명령이 다를 수
있습니다.

[fl[`git config` Git 매뉴얼 페이지에 변경 사항이 완전히
정리돼 있습니다|https://git-scm.com/docs/git-config#_deprecated_modes]].

이 장에서 사용한 최신 명령은 다음과 같습니다.

```{.default}
git config get user.email                     # Get
git config set user.email "user@example.com"  # Set
git config unset user.email                   # Delete
git config list                               # List
git config edit                               # Edit
```

이에 해당하는 이전 명령은 다음과 같습니다.

``` {.default}
git config user.email                     # Get
git config user.email "user@example.com"  # Set
git config --unset user.email             # Delete
git config --list                         # List
git config --edit                         # Edit
```

가능하다면 새 명령을 사용하세요!

[i[Configuration-->Deprecated usage]>]

[i[Configuration]>]
