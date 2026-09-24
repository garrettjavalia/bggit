# 패치 모드: 변경 사항 일부 적용하기 {#patch-mode-applying-partial-changes}

[i[Patch mode]<]

많은 Git 명령은 ***패치 모드***로 전환하는 `-p` 스위치를 지원합니다. 특정
명령에 변경 사항 *일부*만 선택하고 *전부*는 선택하지 않을 수 있는 강력한
모드입니다.

`-p`를 사용할 수 있는 명령에는 `add`, `reset`, `stash`, `restore`, `commit`
등이 있습니다.

파일에 변경 사항이 있고 "이 변경 중 *일부*에만 무언가를 하고 싶다"고 생각할
때마다 패치 모드가 도움이 됩니다.

용어를 하나 알아봅시다. Git은 서로 가까이 있는 변경 사항의 모음을 *헝크*라고
부릅니다. 예를 들어 `foo()` 함수에 몇 줄을 추가하고 `bar()` 함수에도 몇 줄을
추가했다면, 변경 묶음마다 하나씩 헝크가 두 개 생길 가능성이 큽니다.

패치 모드에서는 작업할 헝크를 선택할 수 있습니다.

## 패치 모드에서 파일 추가하기 {#adding-files-in-patch-mode}

[i[Patch mode-->Add]<]

파일에 `Line 1`부터 `Line 8`까지 추가한 커밋이 있다고 합시다.

``` {.default}
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
```

그리고 위와 아래에 한 줄씩 추가하는 변경을 만듭니다.

``` {.default}
Line BEGIN
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
Line END
```

추가하고 커밋하려는 순간, 지금은 `Line END`가 아니라 `Line BEGIN`만 추가하고
싶다는 사실을 깨닫습니다.

평범한 `git add`를 실행하면 두 변경이 모두 스테이징 영역에 추가됩니다. 하지만
`git add -p`를 실행하면 둘 중 하나를 고를 수 있습니다. 해 봅시다.

먼저 diff를 살펴봅시다.

``` {.default}
$ git diff
  diff --git a/foo.txt b/foo.txt
  index a982fdc..125f6ac 100644
  --- a/foo.txt
  +++ b/foo.txt
  @@ -1,3 +1,4 @@
  +Line BEGIN
   Line 1
   Line 2
   Line 3
  @@ -6,3 +7,4 @@ Line 5
   Line 6
   Line 7
   Line 8
  +Line END
```

자세히 보면 위에 `Line BEGIN`, 아래에 `Line END`를 추가했다는 것을 알 수
있습니다. (diff에서 앞에 `+`가 붙은 줄은 추가된 줄임을 기억하세요.)

이제 패치 추가를 해 봅시다.

``` {.default}
$ git add -p
  diff --git a/foo.txt b/foo.txt
  index a982fdc..125f6ac 100644
  --- a/foo.txt
  +++ b/foo.txt
  @@ -1,3 +1,4 @@
  +Line BEGIN
   Line 1
   Line 2
   Line 3
  (1/2) Stage this hunk [y,n,q,a,d,j,J,g,/,e,p,?]? 
```

선택지가 정말 많군요! 쉬운 것은 "예"를 뜻하는 `y`와 "아니요"를 뜻하는
`n`입니다. `?`를 입력하면 더 자세한 도움말도 볼 수 있습니다.

또한 이것이 두 헝크 중 첫 번째라는 것도 보입니다. 파일 위쪽과 아래쪽에 변경이
하나씩 있으니 말이 됩니다.

여기서는 첫 번째 헝크를 유지하고 싶으므로 `y`라고 답합니다.

그러면 두 번째 헝크로 넘어갑니다.

``` {.default}
(1/2) Stage this hunk [y,n,q,a,d,j,J,g,/,e,p,?]? y
@@ -6,3 +7,4 @@ Line 5
 Line 6
 Line 7
 Line 8
+Line END
(2/2) Stage this hunk [y,n,q,a,d,K,g,/,e,p,?]?
```

이 헝크는 스테이징하지 않도록 `n`이라고 답하겠습니다. 그러면 셸 프롬프트로
돌아옵니다.

이제 현재 상태를 보려고 `git status`를 입력할 텐데, 먼저 어떤 내용이 나올지
생각해 보세요.

한 변경은 스테이징됐고 다른 변경은 스테이징되지 않았습니다. 스테이징되지 않은
변경이 있을 때 파일은 어떤 상태일까요? 스테이징된 변경이 있을 때는요? 지금은
둘 다 있죠?

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

역시 그렇습니다! 파일의 변경 사항을 일부만 추가했으므로 추가한 변경은
스테이징 영역에 있고, 추가하지 않은 변경은 여전히 작업 트리에 있습니다.
변경 사항을 *전부* 스테이징하지 않았으니 *반드시* 이렇게 돼야 합니다!

이제 스테이징 영역에 일부만 추가된 변경 사항을 커밋할 수 있습니다.

[i[Patch mode-->Add]>]

## 패치 모드에서 파일 리셋하기 {#resetting-files-in-patch-mode}

[i[Patch mode-->Reset]<]

`git add -p`의 반대에 가까운 명령이 `git reset -p`입니다. `reset -p`로
*스테이징 영역에 있는* 헝크를 골라 변경할 수 있습니다.

마지막 부분 때문에 조금 이상하게 느껴질 수 있습니다. `add -p`는 작업 트리에서
헝크를 골라 스테이징 영역에 추가하고, `reset -p`는 특정 커밋을 기준으로
스테이징 영역에서 헝크를 골라 제거한다고 생각하면 됩니다.

즉, 이전 커밋으로 리셋하되 리셋할 헝크를 선택할 수 있습니다.

> **이것은 하드, 소프트, 혼합 리셋이 아닙니다.** 별개의 기능입니다. `-p`와
> 함께 특정 리셋 유형을 지정하려 하면 Git이 불평합니다. 완전히 다른 명령이어야
> 한다고 주장할 수도 있겠지만, Git이 다 그렇죠!

커밋이 두 개 있다고 합시다. 첫 번째 커밋에는 `Line 1`부터 `Line 8`까지
추가했고, 두 번째 커밋에는 앞 예시처럼 `Line BEGIN`과 `Line END`를
추가했습니다.

그런데 이제 `Line END`를 리셋하고 싶지만 다른 커밋의 일부입니다.
`git reset -p`로 이 부분만 떼어낼 수 있습니다. 해 봅시다.

로그는 다음과 같습니다.

``` {.default}
commit d2d5899a253d5ce277d4d5981d03a43e68da6677 (HEAD -> main)
Author: User Name <user@example.com>
Date:   Fri Oct 11 16:12:26 2024 -0700

    updated

commit aae754f46130b6d86680e74caa98642becc88d6e
Author: User Name <user@example.com>
Date:   Fri Oct 11 16:12:04 2024 -0700

    added
```

이전 커밋 `aae75`로 일부만 리셋하고 싶습니다. 첫 번째 헝크는 리셋하지 않겠다고
"아니요", 두 번째 헝크는 리셋하겠다고 "예"라고 답하겠습니다. 다음과 같습니다.

``` {.default}
$ git reset -p aae75
  diff --git b/foo.txt a/foo.txt
  index 125f6ac..a982fdc 100644
  --- b/foo.txt
  +++ a/foo.txt
  @@ -1,4 +1,3 @@
  -Line BEGIN
   Line 1
   Line 2
   Line 3
  (1/2) Apply this hunk to index [y,n,q,a,d,j,J,g,/,e,p,?]? n
  @@ -7,4 +6,3 @@ Line 5
   Line 6
   Line 7
   Line 8
  -Line END
  (2/2) Apply this hunk to index [y,n,q,a,d,K,g,/,e,p,?]? y
```

첫 질문은 "'Line BEGIN'을 제거할까요?"라고 묻고 저는 "아니요"라고 답했습니다.
두 번째 질문은 "'Line END'를 제거할까요?"라고 묻고 저는 "예"라고 답했습니다.

현재 상태는 어떨까요?

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

흠. 스테이징 영역과 `HEAD`의 차이를 확인해 봅시다.

``` {.default}
$ git diff --staged
  diff --git a/foo.txt b/foo.txt
  index 125f6ac..e0e1d89 100644
  --- a/foo.txt
  +++ b/foo.txt
  @@ -7,4 +7,3 @@ Line 5
   Line 6
   Line 7
   Line 8
  -Line END
```

`HEAD`와 비교했을 때 스테이징 영역에서는 `Line END`가 제거됐다는 뜻입니다.
바로 `reset -p`에 요청한 내용이니 좋습니다. 계획대로 가고 있습니다.

그런데 `foo.txt`는 왜 수정된 상태일까요? 살펴봅시다.

``` {.default}
$ git diff
  diff --git a/foo.txt b/foo.txt
  index e0e1d89..125f6ac 100644
  --- a/foo.txt
  +++ b/foo.txt
  @@ -7,3 +7,4 @@ Line 5
   Line 6
   Line 7
   Line 8
  +Line END
```

스테이징 영역과 비교했을 때 작업 트리의 끝에는 `Line END`가 추가돼 있다는
뜻입니다.

실제로 작업 트리의 `foo.txt` 파일을 보면 *여전히 `Line END`가 들어 있습니다*.

``` {.default}
$ cat foo.txt
  Line BEGIN
  Line 1
  Line 2
  Line 3
  Line 4
  Line 5
  Line 6
  Line 7
  Line 8
  Line END
```

이게 무슨 뜻일까요? `reset -p`는 스테이징 영역을 건드렸지만 작업 트리는
건드리지 않았다는 뜻입니다. 작업 트리는 여전히 마지막 커밋 때와 같습니다.
(`git diff HEAD`는 변경 사항을 표시하지 않습니다.)

솔직히 이는 원하는 결과가 아닐 가능성이 큽니다. 헝크를 리셋하는 **동시에**
작업 트리도 그 헝크에 맞춰 리셋하고 싶었을 수 있습니다.

하지만 여전히 그렇게 만들 수 있습니다! 리셋된 헝크가 커밋할 준비를 마치고
스테이징 영역에 있다는 것을 기억하세요! 커밋해 봅시다!

``` {.default}
$ git commit -m "remove END"
  [main 46badfe] remove END
   1 file changed, 1 deletion(-)
```

됐습니다. 이제 스테이징 영역과 `HEAD`가 같고, 둘 다 `Line END`가 제거된
상태입니다. 하지만 `status`가 알려 주듯 작업 트리에는 여전히 `Line END`가
있습니다.

``` {.default}
$ git status
  On branch main
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working
    directory)
	  modified:   foo.txt
```

그러면 리셋한 변경을 작업 트리에도 어떻게 반영할까요? 힌트에 답이 바로
나와 있습니다.

``` {.default}
$ git restore foo.txt
```

됐습니다. 이제 모든 곳에서 `Line END`가 완전히 제거돼 상태가 같습니다.

> **패치 리셋 중 스테이징 영역과 작업 트리를 동기화하는 다른 방법도
> 있습니다.** `reset -p`를 실행한 뒤 다음 명령으로 `foo.txt` 파일을 스테이징
> 영역에서 작업 트리로 복사할 수 있습니다.
>
> ``` {.default}
> git checkout -- foo.txt
> ```
>
> <!-- ` -->
> 그러면 스테이징 영역과 작업 트리가 같아져 커밋이 끝났을 때 모든 상태가
> 일치합니다.

[i[Patch mode-->Reset]>]

## 다른 패치 모드 명령 {#other-patch-mode-commands}

`stash`, `restore`, `commit` 등에도 `-p`를 사용할 수 있습니다. UI는 기본적으로
위에서 설명한 것과 같은 방식으로 동작합니다. 특정 명령을 더 알아보려면 해당
매뉴얼 페이지를 보세요.

[i[Patch mode]>]
