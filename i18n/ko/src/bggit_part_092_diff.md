# Diff로 파일 비교하기 {#diff}

[i[Diff]<]

강력한 `git diff` 명령은 파일이나 커밋 두 개의 차이를 보여 줍니다. 앞부분에서 잠깐 언급했지만, 여기서는 이 명령으로 할 수 있는 여러 가지 일을 더 깊이 살펴보겠습니다.

처음에는 읽기가 쉽지 않지만 시간이 지나면 익숙해집니다. 제가 가장 자주 쓰는 용도는 작업 트리에서 무엇을 바꿨는지 빠르게 훑어보고, 무엇을 스테이징 영역에 추가할지와 어떤 커밋 메시지를 쓸지 떠올리는 것입니다.

## 기본 사용법 {#basic-usage}

[i[Diff-->Understanding the output]<]

가장 기본적인 사용 사례는 작업 트리의 파일 몇 개를 수정한 뒤, 원래 내용과 추가한 내용 사이의 차이를 보고 싶은 경우입니다.

예를 들어 `hello.py` 파일을 수정했지만 아직 스테이징하지 않았다고 합시다. 다음과 같이 변경 내용을 확인할 수 있습니다.

``` {.default}
$ git diff
  diff --git a/hello.py b/hello.py
  index 4a8f53f..8ee1fe4 100644
  --- a/hello.py
  +++ b/hello.py
  @@ -1,4 +1,8 @@
   def hello():
  -    print("Hello, world!")
  +    print("HELLO, WORLD!")
  +
  +def goodbye():
  +    print("See ya!")

   hello()
  +goodbye()
```

저게 대체 무엇일까요? 당연히 한 치 앞도 볼 수 없는 난장판입니다!

_끝_

좋습니다. 심호흡하고 이해해 봅시다.

출력 전체에 `hello.py`가 도배되어 있으므로 이 파일을 이야기하고 있다고 무리 없이 가정할 수 있습니다. diff가 여러 파일을 보고한다면(예: 커밋 두 개를 비교할 때) 출력에 파일마다 별도의 절이 생깁니다.

> **`index` 줄에는 블롭 해시와 파일 권한이 있습니다.** 블롭 해시는 비교하는 각 상태에서 특정 파일의 해시입니다. 보통은 신경 쓸 필요가 없습니다. 어쩌면 평생 신경 쓰지 않아도 될지 모릅니다.

그 뒤의 두 줄은 이전 버전 파일 `a/hello.py`에 빼기 기호가 붙고, 아직 스테이징하지 않은 새 버전 `b/hello.py`에 더하기 기호가 붙는다고 나타냅니다.

다음에는 `@@ -1,4 +1,8 @@`이 있습니다. 이전 버전의 1~4행과 새 버전의 1~8행을 보여 준다는 뜻입니다. (그러니 적어도 줄 몇 개를 추가한 것은 분명합니다.)

마침내 이 모든 것의 알맹이, 즉 실제로 무엇이 바뀌었는지에 도달했습니다. 이전 버전은 빼기, 새 버전은 더하기라는 점을 기억하면서 diff의 그 부분만 다시 봅시다.

``` {.default}
   def hello():
  -    print("Hello, world!")
  +    print("HELLO, WORLD!")
  +
  +def goodbye():
  +    print("See ya!")

   hello()
  +goodbye()
```

규칙은 다음과 같습니다.

* 줄 앞에 `-`가 있으면 이전 버전에서 그 줄이 이랬다는 뜻입니다.

* 줄 앞에 `+`가 있으면 수정된 새 버전에서 그 줄이 이렇다는 뜻입니다.

* 줄 앞에 아무것도 없으면 두 버전 사이에서 바뀌지 않았다는 뜻입니다.

> **diff는 파일의 모든 줄을 보여 주지 않습니다!** 바뀐 부분과 그 주변의 몇 줄만 보여 줍니다. 파일의 서로 다른 부분에 변경 사항이 있다면 바뀌지 않은 부분은 diff에서 건너뜁니다.

diff를 읽는 또 다른 방법은 `-`가 붙은 줄은 제거되었고 `+`가 붙은 줄은 추가되었다고 생각하는 것입니다.

[i[Diff-->Understanding the output]>]

## 스테이징 영역 Diff 보기 {#diffing-the-stage}

[i[Diff-->The stage]<]

내용을 스테이징 영역에 추가한 뒤 이전 커밋과 diff를 보고 싶다면 어떻게 할까요?

`git diff`만 입력하면 아무것도 나오지 않습니다!

왜일까요? 기본적으로 diff는 *작업 트리와 스테이징 영역의 차이*를 보여 주기 때문입니다. 방금 그 파일을 스테이징하여 작업 트리에서 스테이징 영역으로 복사했으므로 둘은 똑같습니다. 따라서 diff에 차이가 나타나지 않습니다.

스테이징 영역과 이전 커밋의 diff는 어떻게 볼까요?

답은 아주 쉽습니다. `git diff --staged`[^91c6]입니다. 끝.

[^91c6]: `--staged` 플래그가 더 현대적인 방식입니다. 이전 버전의 Git에서는 `git diff --cached`를 사용했습니다.

하지만 이 하위 절에서는 무슨 일이 일어나는지 조금 더 깊이 파고들어 작동 원리를 더 잘 이해하도록 하겠습니다.

사고 모형을 세울 시간입니다!

다음 두 가지가 참이라고 합시다. 실제로 참인지 아닌지는 별로 중요하지 않습니다.

> _"모형일 뿐이야."_\
> \ \ \ \ \ \ \ \ \ \ \ \ —팻시, _몬티 파이튼과 성배_

1. 스테이징 영역에는 현재 커밋에서 수정되지 않은 **모든** 파일의 *복사본*이 있습니다.

2. `git status`나 `git diff`는 작업 트리와 스테이징 영역 사이에 차이가 있는 파일만 보여 줍니다.

따라서 수정 사항이 없다면 `git diff`는 차이를 보여 주지 않습니다. 스테이징 영역과 작업 트리가 같기 때문입니다.

이제 작업 트리의 파일을 수정하고 `git diff`를 실행하면 작업 트리가 스테이징 영역과 다르므로 변경 사항이 *나타납니다*.

하지만 수정한 파일을 스테이징 영역에 추가하면 스테이징 영역과 작업 트리가 다시 같아집니다. `git diff`에는 차이가 나타나지 않습니다.

*`git diff`는 **항상** 작업 트리와 스테이징 영역을 비교합니다.* (특정 커밋의 diff를 보는 경우는 예외이며 아래에서 설명합니다.) 이 경우 수정한 파일을 스테이징 영역에 추가했으므로 작업 트리와 같습니다. 따라서 diff가 없습니다.

작업 트리를 수정했지만 파일을 스테이징 영역에 추가하지 않은 경우와 비교해 보세요. 이 경우 스테이징 영역의 파일은 마지막 커밋과 같고 작업 트리와는 다릅니다. 따라서 `git diff`가 차이를 보여 줍니다.

좋습니다. 그러면 스테이징 영역의 내용과 마지막 커밋의 diff를 *보고 싶다면* 어떻게 할까요? 즉 작업 트리와 스테이징 영역 대신 스테이징 영역과 `HEAD`의 diff를 보고 싶습니다.

다시 결론으로 돌아갑니다.

``` {.default}
$ git diff --staged
```

이것으로 됩니다. 스테이징 영역의 내용과 마지막 커밋 사이의 diff를 실행하여 스테이징한 변경 사항을 보여 줍니다.

[i[Diff-->The stage]>]

## 더 재미있는 Diff {#more-diff-fun}

diff로 할 수 있는 작업의 예를 빠르게 훑어봅시다.

### 임의의 커밋이나 브랜치 Diff 보기 {#diff-any-commits-or-branches}

[i[Diff-->Other commits]]
[i[Diff-->Other branches]]
작업 트리나 스테이징 영역의 diff만 볼 수 있는 것은 아닙니다. 어떤 커밋 두 개든 실제로 비교할 수 있습니다. 두 커밋 사이의 모든 차이를 보여 줍니다.

예를 들어 커밋 해시를 안다면 직접 비교할 수 있습니다.

``` {.default}
$ git diff d977 27a3
```

브랜치 이름 두 개가 있다면 다음과 같이 합니다.

``` {.default}
$ git diff main topic
```

서로 섞어도 됩니다.

``` {.default}
$ git diff main 27a3
```

`HEAD`를 사용해도 됩니다.

``` {.default}
$ git diff HEAD 27a3
```

상대적인 `HEAD`도 가능합니다. 다음 명령은 `HEAD` 바로 전 커밋과 `HEAD`의 diff를 봅니다.

``` {.default}
$ git diff HEAD^ HEAD
```

다음 명령은 `HEAD`보다 네 커밋 앞선 커밋과 `HEAD`보다 세 커밋 앞선 커밋을 비교합니다.

``` {.default}
$ git diff HEAD~4 HEAD~3
```

### Diff 순서 {#diffing-order}

다음 두 방식 모두 올바르지만 서로 다른(반대 방향의) 결과를 냅니다.

``` {.default}
$ git diff main topic
$ git diff topic main
```

다음과 같다고 생각할 수 있습니다.

``` {.default}
$ git diff FROM TO
```

즉 "Git아, 커밋 `FROM`에서 커밋 `TO`로 가려면 어떤 변경을 해야 하는지 알려 줘"라는 뜻입니다.

`foo.md` 파일을 만들어 `First`라는 한 줄을 넣고 커밋했다고 합시다. 그런 다음 내용을 `Second`로 덮어쓰고 다시 커밋했습니다.

이 예에서는 "`HEAD` 바로 전 커밋에서 `HEAD` 커밋으로 가려면 무엇을 바꿔야 할까?"라고 물을 수 있습니다.

``` {.default}
$ git diff HEAD^ HEAD
  diff --git a/foo.md b/foo.md
  index d00491f..495a7e9 100644
  --- a/foo.md
  +++ b/foo.md
  @@ -1 +1 @@
  -First
  +Second
```

`HEAD` 바로 전 커밋에서 `HEAD`로 가려면 `First`를 삭제하고 `Second`를 추가해야 한다고 알려 줍니다.

하지만 순서를 뒤집어 "`HEAD`에서 `HEAD` 바로 전 커밋으로 돌아가려면 무엇을 바꿔야 할까?"라고 물으면 반대 결과가 나옵니다.

``` {.default}
$ git diff HEAD HEAD^
  diff --git a/foo.md b/foo.md
  index 495a7e9..d00491f 100644
  --- a/foo.md
  +++ b/foo.md
  @@ -1 +1 @@
  -Second
  +First
```

여기서는 `HEAD` 바로 전 커밋으로 돌아가려면 `Second`를 삭제하고 `First`를 추가해야 한다고 알려 줍니다.

그러므로 `git diff FROM TO`는 `FROM` 커밋에서 `TO` 커밋으로 가기 위해 필요한 변경 사항을 알려 준다는 점을 기억하세요.

### 부모 커밋과 Diff 보기 {#diffing-with-parent-commit}

[i[Diff-->Parent commit]]
방금 다음 예를 보았습니다.

``` {.default}
$ git diff HEAD~4 HEAD~3
```

하지만 `HEAD~4`는 `HEAD~3`의 부모이므로 쓸 수 있는 축약 표기가 있을까요? 있습니다!

``` {.default}
$ git diff HEAD~4 HEAD~3
$ git diff HEAD~3^!          # Same thing!
```

커밋을 그 부모와 비교하고 싶은 어디에서든 사용할 수 있습니다. 결국 특정 커밋 하나에 어떤 변경 사항이 들어 있는지만 보여 줍니다.

``` {.default}
$ git diff HEAD^!
$ git diff HEAD~3^!
$ git diff main^!
$ git diff 27a3^!
```

### 문맥 더 보기 {#more-context}

[i[Diff-->Additional context]]
기본적으로 `git diff`는 변경 사항 주변의 문맥을 3줄 보여 줍니다. 5줄처럼 더 많이 보고 싶다면 `-U` 옵션을 사용하세요.

``` {.default}
$ git diff -U5
```

### 파일 이름만 보기 {#just-the-file-names}

[i[Diff-->File names only]]
변경된 파일의 목록만 보고 싶다면 `--name-only` 옵션을 사용할 수 있습니다.

``` {.default}
$ git diff --name-only
```

### 공백 무시하기 {#ignoring-whitespace}

[i[Diff-->Ignore whitespace]]
소스 코드에서 탭과 스페이스가 뒤섞이는 혼란을 겪을 때가 있는데, 언제나 고통스럽습니다. 전문가의 비법을 알려 드리죠. 하나만 정해서 쓰고 팀의 다른 사람도 똑같이 쓰게 하세요. 어기면 점심을 사는 벌을 줍시다.

하지만 비교할 때 공백을 무시하도록 `git diff`에 명령할 수 있습니다.

``` {.default}
$ git diff -w
$ git diff --ignore-all-space    # Same thing
```
### 특정 파일만 보기 {#just-certain-files}

[i[Diff-->Specific files]]
특정 파일만 diff로 볼 수 있습니다.

한 가지 방법은 `--` 뒤에 파일 이름을 놓는 것입니다.

``` {.default}
$ git diff -- hello.py
$ git diff -- hello.py another_file.py
```

`--` 앞에 커밋이나 브랜치를 지정할 수도 있습니다.

``` {.default}
$ git diff somebranch -- hello.py
```

작업 트리의 `hello.py`를 `somebranch`의 버전과 비교합니다.

또는 커밋이나 브랜치 두 개를 지정하여 그곳의 파일을 비교할 수 있습니다.

``` {.default}
$ git diff main somebranch -- hello.py
```

마지막으로 글롭과 작은따옴표를 사용해 특정 파일 확장자로 제한할 수 있습니다.

``` {.default}
$ git diff '*.py'
```

Python 파일의 diff만 보여 줍니다.

### 브랜치 사이의 Diff {#inter-branch-diffs}

[i[Diff-->Between branches]]
두 브랜치를 비교하는 흥미로운 방식입니다.

브랜치 두 개에 있는 커밋을 비교하는 다음 예는 이미 보았습니다.

``` {.default}
$ git diff branch1 branch2
```

하지만 때로는 *브랜치가 갈라진 뒤* 어느 브랜치에서 무엇이 바뀌었는지 알고 싶습니다.

즉 위 명령이 알려 주는 `branch1`과 `branch2` 사이의 *현재* 차이를 알고 싶은 것이 아닙니다.

`branch2`가 추가하거나 삭제했지만 `branch1`에는 없는 내용을 알고 싶은 것입니다.

이를 보려면 다음 표기법을 사용합니다.

``` {.default}
$ git diff branch1...branch2
```

"`branch1`과 `branch2`의 공통 조상과 `branch2`의 diff를 본다"는 뜻입니다.

다시 말해 `branch1`이 모르는 사이 `branch2`에서 이루어진 모든 변경 사항을 알려 달라는 것입니다. 두 브랜치가 갈라진 뒤 `branch1`에서 바뀐 것은 보여 주지 않습니다.

## Difftool {#difftool}

diff 출력이 읽기 어렵다는 것을 압니다. 연습하기를 권하며, 충분히 연습하면 출력을 꿰뚫어 볼 수 있게 된다는 살아 있는 증거로 저 자신을 제시합니다. 결국에는 읽기 쉬워지기까지 합니다. 상상하기 어려울 수도 있지만 정말 그렇습니다!

그렇다고 해도 diff를 더 다루기 쉽게 만들어 주는 서드 파티 도구가 있고 Git은 이런 도구를 지원합니다. [diff 도구](#difftool) 장에서 더 자세히 읽어 볼 수 있습니다.

[i[Diff]>]
