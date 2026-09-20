# 원격 저장소: 다른 곳에 있는 저장소 {#remotes-repos-in-other-places}

[i[Remote]<]

_원격 저장소_는 클론하고 푸시하고 풀할 수 있는 원격 서버에 붙인 이름일 뿐입니다.

이 서버는 URL로 식별합니다. GitHub에서는 처음 저장소를 클론할 때 복사했던 URL입니다.

일상적인 Git 작업에서 이 URL로 서버를 식별할 수도 있지만 입력하기 번거롭습니다. 그래서 원격 서버 URL에 별명을 붙이고, 이를 흔히 "원격 저장소"라고 부릅니다.

이미 여러 번 본 원격 저장소로 [i[Remote-->`origin`]] `origin`이 있습니다. 클론해 온 원격 저장소의 별명이며, 클론할 때 Git이 자동으로 설정합니다.

## 원격 저장소와 브랜치 표기법 {#remote-and-branch-notation}

[i[Remote-->Remote branches]<]

시작하기 전에, Git은 특정 원격 저장소의 특정 브랜치를 나타낼 때 `remotename/branchname`이라는 슬래시 표기법을 사용한다는 점을 알아 두세요.

예를 들어 다음은 `origin`이라는 원격 저장소의 `main` 브랜치를 가리킵니다.

``` {.default}
origin/main
```

다음은 `nitfol`이라는 원격 저장소의 `feature3490` 브랜치를 가리킵니다.

``` {.default}
nitfol/feature3490
```

원격 추적 브랜치 장에서 더 자세히 설명하겠습니다.

[i[Remote-->Remote branches]>]

## 원격 저장소 목록 보기 {#getting-a-list-of-remotes}

[i[Remote-->Listing]<]

어느 저장소 디렉터리에서든 `-v` 옵션을 붙여 `git remote`를 실행하면 그 저장소에 설정된 원격 저장소를 볼 수 있습니다.

``` {.default}
$ git remote -v
  origin    https://github.com/example-repo.git (fetch)
  origin    https://github.com/example-repo.git (push)
```

`origin`이라는 원격 저장소에서 풀(그 일부가 `fetch`입니다)할 때와 푸시할 때 같은 URL을 사용하고 있습니다. 두 작업에 같은 URL을 쓰는 경우는 아주 흔합니다.

그리고 이 URL은 처음 저장소를 클론할 때 GitHub에서 복사했던 바로 그 URL입니다.

[i[Remote-->Listing]>]

## 원격 저장소 URL 바꾸기 {#changing-a-remotes-url}

[i[Remote-->Setting the URL]<]

원격 저장소 이름은 저장소를 클론해 온 어떤 URL의 별칭일 뿐이라는 점을 기억하세요.

GitHub에서 푸시와 풀에 모두 사용할 SSH 키를 완벽하게 설정했는데, 실수로 HTTPS URL을 사용하여 저장소를 클론했다고 합시다. 그러면 다음과 같은 원격 저장소가 보입니다.

``` {.default}
$ git remote -v
origin    https://github.com/example-repo.git (fetch)
origin    https://github.com/example-repo.git (push)
```

이어서 푸시하려 하자 GitHub가 HTTPS 원격 저장소에는 푸시할 수 없다고 알려 줍니다… 이런!

클론할 때 SSH URL을 복사하려던 것이었으며, 제 경우에는 다음과 같습니다.

``` {.default}
git@github.com:beejjorgensen/git-example-repo.git
```

다행히 세상이 끝난 것은 아닙니다. 별칭이 가리키는 곳만 바꾸면 됩니다.

(아래 예는 책에서 너무 넓어지지 않도록 두 줄로 나눴지만 한 줄로 써도 됩니다. 백슬래시는 Bash에 명령이 다음 줄로 이어진다고 알려 줍니다.)

``` {.default}
$ git remote set-url origin \
             git@github.com:beejjorgensen/git-example-repo.git
```

이제 원격 저장소를 살펴보면 다음과 같습니다.

``` {.default}
$ git remote -v
origin    git@github.com:beejjorgensen/git-example-repo.git (fetch)
origin    git@github.com:beejjorgensen/git-example-repo.git (push)
```

이제 푸시할 수 있습니다! (SSH 키를 설정했다는 가정 아래에서 말입니다.)

[i[Remote-->Setting the URL]>]

## 원격 저장소 추가하기 {#adding-a-remote}

[i[Remote-->Adding]<]

다른 원격 저장소를 추가하지 못할 이유는 없습니다.

흔한 예는 GitHub 프로젝트를 _포크_한 경우입니다(뒤에서 더 설명합니다). 포크는 다른 사람의 공개 저장소를 자신의 계정으로 쉽게 클론할 수 있게 하는 GitHub 기능이며, 여러분이 만든 변경 사항을 원래 저장소와 편리하게 공유할 방법을 제공합니다.

제가 Linux 소스 저장소를 포크했다고 합시다. 제 포크를 클론하면 다음 원격 저장소가 보입니다.

``` {.default}
origin    git@github.com:beejjorgensen/linux.git (fetch)
origin    git@github.com:beejjorgensen/linux.git (push)
```

저는 실제 Linux 소스 코드 저장소에 접근 권한이 없지만, 이를 포크하여 제 저장소 복사본을 얻을 수 있습니다.

이제 Linus Torvalds가 자신의 저장소를 변경하더라도 제게 자동으로 보이지는 않습니다. 따라서 그의 변경 사항을 가져와 제 저장소에 병합할 방법이 필요합니다.

그의 저장소를 가리킬 방법이 필요하므로 그곳을 가리키는 `reallinux`라는 원격 저장소를 추가하겠습니다.

``` {.default}
$ git remote add reallinux https://github.com/torvalds/linux.git
```

이제 원격 저장소는 다음과 같습니다.

``` {.default}
origin    git@github.com:beejjorgensen/linux.git (fetch)
origin    git@github.com:beejjorgensen/linux.git (push)
reallinux    https://github.com/torvalds/linux.git (fetch)
reallinux    https://github.com/torvalds/linux.git (push)
```

> [i[Remote-->`upstream` convention]]
> 보통 GitHub에서 포크한 저장소의 원본을 가리키는 원격 저장소를 설정할 때는 `upstream`이라고 부르는 경우가 많지만, 저는 분명히 `reallinux`라고 불렀습니다.
>
> 이렇게 한 이유는 뒤에서 [원격 추적 브랜치](#remote-tracking-branch)를 설명할 때 "upstream"을 다른 의미로 사용할 것이며, 두 가지가 혼동되기를 원하지 않기 때문입니다.
>
> 실제 작업에서 포크해 온 원본 저장소를 가리키는 원격 저장소를 설정할 때는 `upstream`이라고 부르는 것이 비교적 일반적인 관례라는 점만 기억하세요.

[i[Remote-->Sync with `upstream`]]

이제 다음 명령으로 Linus의 저장소에서 모든 변경 사항을 받을 수 있습니다.

``` {.default}
$ git fetch reallinux
```

그리고 제 브랜치에 병합할 수 있습니다(Linux 저장소는 기본 브랜치에 `main` 대신 `master`를 사용합니다).

``` {.default}
$ git switch master            # My local master
$ git merge reallinux/master   # Note the slash notation!
```

충돌을 모두 처리하고 나면 `reallinux`의 `master` 브랜치를 제 로컬 `master`에 병합합니다.

제가 커밋을 하나 더 만들면 로컬 `HEAD`와 `master`가 새 커밋으로 이동하고, `origin/master`(GitHub에 있는 제 포크)와 `reallinux/master`(Linus의 저장소)는 더 뒤에 남습니다.

재미 삼아 GitHub의 `origin` 원격 저장소에는 없는 커밋을 두 개 만들었다고 합시다. 그러면 일부를 잘라 내고 시연 목적으로 꾸며 낸 로그는 다음과 비슷합니다.

``` {.default}
commit 2d7d5d (HEAD -> master)
commit cde831
commit 311eb3 (origin/master)
commit d5d2cc (reallinux/master)
```

이 시점에는 `git push`를 실행하여 로컬 `master`의 변경 사항을 GitHub로 보내고 그곳의 `origin/master`를 따라잡게 합니다. 그러면 맨 위 커밋은 다음처럼 보입니다.

``` {.default}
commit 2d7d5d (HEAD -> master, origin/master)
commit cde831
commit 311eb3
commit d5d2cc (reallinux/master)
```

`reallinux/master`는 여전히 어딘가 뒤에 남습니다. (그리고 Linus가 제 변경 사항을 친히 병합해 줄 때까지 그곳에 머물 것입니다.)

로컬 `master`가 `origin`의 `master`와 동기화되지 않을 수 있다는 점이 흥미롭지 않나요?

[원격 추적 브랜치 장](#remote-tracking-branch)에서 살펴보겠습니다.

[i[Remote-->Adding]>]

[i[Remote]>]
