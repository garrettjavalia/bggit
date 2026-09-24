# Git 별칭 {#git-aliases}

[i[Alias]<]

Git 명령 가운데는 입력하기 고된 것도 있습니다. 지금까지는 _그렇게까지_
복잡한 일을 할 필요가 없었지만, 언젠가는 그럴 수도 있습니다.

예를 들어 `git log`로 수정된 파일의 이름을 보고 싶다고 합시다. 어렵지
않습니다. 그렇게 하라고 명령하면 됩니다.

``` {.default}
$ git log --name-only
```

그러면 잘 작동합니다.

하지만 예를 들어 이 작업을 *아주 자주* 하게 됐다고 합시다. 슬슬
성가셔질 것입니다.

같은 일을 하는 `git logn` 같은 새 명령을 직접 만들 수 있다면 더 쉽지
않을까요?

바로 이럴 때 별칭을 씁니다.

이 장에서는 독자가 [구성 장](#configuration)을 읽었다고 가정합니다. 특히
여기 나온 명령이 작동하지 않는다면 [이전 Git 버전의
구성](#config-old)을 살펴보세요.

## 별칭 만들기 {#creating-an-alias}

[i[Alias-->Creating]<]

구성 인터페이스를 통해 별칭을 만듭니다. 기본적으로 설정하려는 것은
`alias.myname` 변수이며, 여기서 `myname`은 새 명령의 이름입니다.

`git logn`을 `git log --name-only`의 별칭으로 만들고 싶다고 합시다. 다음과
같이 하면 됩니다.

``` {.default}
$ git config set --global alias.logn 'log --name-only'
```

이제 다음 명령을 실행할 수 있습니다.

``` {.default}
$ git logn
```

그러면 이 명령이 `git log --name-only`의 별칭이 되어 사실상 그 명령을
실행합니다.

제 추측으로는 Git에 `log`나 `push` 같은 내장 명령이 여럿 있고, 내장
명령이 아닌 것을 실행하려 하면 `alias` 변수에서 그 이름을 찾는 듯합니다.
찾으면 그 값으로 바꾸는 것이죠. 내부에서 실제로 이렇게 돌아간다고 99%
확신합니다.

별칭도 평범한 구성 변수일 뿐이므로, 값을 조회하고 설정하고 삭제하는 방법은
[구성 장](#configuration)에 설명한 것과 같습니다.

[i[Alias-->Creating]>]

## 별칭 표시하기 {#displaying-aliases}

[i[Alias-->Displaying]<]

별칭은 그저 구성 변수이므로, 어떤 값인지 보고 싶으면 조회하면 됩니다.

``` {.default}
$ git config get alias.logx
```

별칭을 모두 보고 싶다면 다음 명령을 실행하세요.

``` {.default}
$ git config get --all --show-names --regexp '^alias\.'
```

엄청나게 성가신 명령이죠. 이 명령에도 별칭을 붙이길 권합니다. 야호!

> **이전 버전의 Git에서는 대신 이 명령을 사용합니다.**
> ``` {.display}
> $ git config --get-regexp ^alias\.
> ```
> <!-- ` -->

[i[Alias-->Displaying]>]

## 쓸 만한 별칭 예시 {#some-neat-sample-aliases}

[i[Alias-->Examples]<]

다음 예시 가운데 일부는 책에 들어가도록 여러 줄로 나눴습니다. 한 줄에
입력해도 되고, 셸에 명령이 다음 줄에서 계속된다고 알려 주는 `\` 이스케이프를
포함해 보이는 그대로 입력해도 됩니다.

`git adda`로 **변경된 파일을 모두 추가합니다**. 원한 것보다 더 많이 추가할
수 있으니 조심해서 사용하세요!

``` {.default}
$ git config set alias.adda "add --all"
```

`git logc`로 **커밋 그래프를 보여 주는 더 간결한 로그**를 표시합니다.

``` {.default}
$ git config set alias.logc "log --oneline --graph --decorate"
```

`git diffs`로 **스테이징 영역과 저장소의 차이를 비교합니다**.

``` {.default}
$ git config set alias.diffs "diff --staged"
```

`git aliases`로 **모든 별칭을 표시하게 합니다**.

``` {.default}
$ git config set alias.aliases \
    "config get --all --show-names --regexp '^alias\.'"
```

`git lol`로 **매우 화려하고 맞춤화된 로그를 만듭니다**.

(복사해 붙여 넣을 때 공백까지 정확히 그대로 옮기세요. 그렇지 않으면 셸이나
Git, 혹은 둘 다 불평할 것입니다.)

``` {.default}
$ git config set alias.lol "log --graph"\
" --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s"\
" %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

마지막 예시에서는 출력을 아주 세밀하게 제어할 수 있는 `--pretty` 서식을
적극 활용합니다. 자세한 내용은 [fl[`git log` 매뉴얼 페이지의 "Pretty
Formats" 절을 참고하세요|https://git-scm.com/docs/git-log#_pretty_formats]].

[i[Alias-->Examples]>]

## Git의 별칭 확장 살펴보기 {#seeing-gits-alias-expansion}

[i[Alias-->Debugging]<]

별칭을 추가했는데 작동하지 않는다고 합시다. 실행하면 오류만 나오고 무슨
일이 벌어지는지 그다지 분명하지 않습니다.

``` {.default}
$ git logx
fatal: unrecognized argument: --foobar
```

명령줄 맨 앞에 `GIT_TRACE=1`을 붙이면 Git에 더 많은 정보를 요청할 수
있습니다.

> **이렇게 하면 환경 변수 `GIT_TRACE`를 `1`로 설정하지만,** 이 명령 하나에만
> 적용됩니다. 지속되는 설정은 아닙니다. Git은 `GIT_TRACE`를 찾아보고, 이
> 변수가 있으면 동작을 바꿔야 한다는 것을 알고 있습니다.

다음은 출력 예시입니다.

``` {.default}
$ GIT_TRACE=1 git logx
  14:09:28.502707 git.c:758               trace: exec: git-logx
  14:09:28.502750 run-command.c:666       trace: run_command: git-l
  14:09:28.502905 git.c:416               trace: alias expansion: l
  14:09:28.502913 git.c:816               trace: exec: git log --fo
  14:09:28.502916 run-command.c:666       trace: run_command: git l
  14:09:28.502926 run-command.c:758       trace: start_command: /us
  14:09:28.504192 git.c:472               trace: built-in: git log 
  fatal: unrecognized argument: --foobar
```

아쉽게도 인쇄판 책에 맞추려고 오른쪽을 잘라 냈는데, 사실 우리가 정말 보고
싶은 부분이 바로 그쪽입니다. 잠시 후 살펴보겠습니다.

우선 왼쪽을 봅시다. 타임스탬프와 Git 코드의 어느 부분이 추적 정보를
출력하는지에 관한 정보가 보입니다. 그리고 마지막에는 앞서 본 오류가 나옵니다.

오른쪽으로 스크롤해서 `trace:` 뒤에 나오는 내용만 살펴봅시다.

``` {.default}
trace: exec: git-logx
trace: run_command: git-logx
trace: alias expansion: logx => log --foobar
trace: exec: git log --foobar
trace: run_command: git log --foobar
trace: start_command: /usr/lib/git-core/git log --foobar
trace: built-in: git log --foobar
```

조금 골라내야 할 수도 있지만, `run_command`와 `alias expansion`이 들어간
줄만 봅시다.

``` {.default}
trace: run_command: git-logx
trace: alias expansion: logx => log --foobar
trace: run_command: git log --foobar
```

여기서 무엇이 무엇으로 확장되는지 정확히 볼 수 있습니다. 별칭을 디버깅할 때
유용할 수 있습니다.

이 간단한 예시에는 조금 과한 방법일지 모르지만, 놀라울 정도로 복잡한 별칭에는
이 기법이 도움이 될 수 있습니다.

[i[Alias-->Debugging]>]

[i[Alias]>]
