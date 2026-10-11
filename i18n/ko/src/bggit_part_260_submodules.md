# 서브모듈 {#submodules}

[i[Submodules]<]

Git 저장소 _안에_ Git 저장소를 둘 수는 없습니다. 정확히 말하면 만들 수는 있지만, 바깥 저장소에 추가하려 하면 Git이 할 말이 아주 많습니다.

``` {.default}
warning: adding embedded git repository: inner
hint: You've added another git repository inside your current
hint: repository. Clones of the outer repository will not contain
hint: the contents of the embedded repository and will not know how
hint: to obtain it. If you meant to add a submodule, use:
hint:
hint: 	git submodule add <url> inner
hint:
hint: If you added this path by mistake, you can remove it from the
hint: index with:
hint:
hint: 	git rm --cached inner
hint:
hint: See "git help submodule" for more information.
hint: Disable this message with "git config advice.addEmbeddedRepo
hint: false"
```

아마 원하는 동작은 아닐 것입니다. 하지만 Git이 힌트를 하나 줍니다. 어쩌면 서브모듈을 사용하려던 게 아닐까요!

서브모듈은 완전히 별개의 저장소가 현재 저장소의 작업 트리 안에 나타나게 하는 방법입니다. 그뿐 아니라 현재 작업 트리에서 서브모듈의 특정 커밋이 그 서브모듈 트리에 나타나게 할 수 있습니다.

대표적인 사용 사례는 프로젝트가 소스까지 보유한 라이브러리에 의존하는 경우입니다. 라이브러리 저장소를 여러분 저장소의 서브모듈로 넣고, 사실상 특정 버전(정확히는 특정 커밋)에 고정할 수 있습니다.

예를 들어 코드가 FooLib 3.4.90 버전과 함께 작동한다고 합시다. FooLib를 서브모듈로 포함하고 그 버전에 고정합니다. 그러면 다른 팀이 FooLib를 계속 업데이트하더라도 빌드에 사용할 3.4.90 버전은 늘 확보됩니다.

나중에 준비가 되면 서브모듈을 최신 버전, 이를테면 4.0.1로 업데이트하고 그 버전에 고정할 수 있습니다.

서브모듈도 그저 평범한 Git 저장소라는 점이 중요합니다. 특별한 것은 없습니다. 다른 저장소 안에 사실상 클론하고 그 저장소에 논리적으로 연결하기로 했다는 점만 눈에 띕니다.

## 서브모듈이 있는 저장소 사용하기 {#using-a-repo-with-submodules}

이미 서브모듈을 사용하는 저장소를 클론하면 어떤 일이 일어나는지부터 이야기해 봅시다. 이 경우 다른 사람이 저장소와 서브모듈을 조립해 두었지만, 클론한 뒤 모든 서브모듈까지 받으려면 약간 더 작업해야 합니다.

프로젝트에 서브모듈을 추가하는 방법은 뒤에서 설명합니다.

다행히 시연용으로 이미 서브모듈이 정의된 클론 가능한 저장소를 준비해 두었습니다. (다시 푸시하고 싶다면 클론하기 전에 저장소를 포크하세요.)

클론하려는 저장소에 서브모듈이 있다는 사실을 이미 안다고 합시다. 그러면 모든 서브모듈 저장소도 함께 받고 싶다는 플래그를 붙여 클론할 수 있습니다. `--recurse-submodules` 옵션을 사용합니다.

``` {.default}
$ git clone --recurse-submodules \
        git@github.com:beejjorgensen/git-example-submodule-repo.git
```

(책 여백에 맞추려고 위 명령을 두 줄로 나눴습니다.)

이 명령은 해당 저장소와 그 저장소에 서브모듈로 나열된 모든 저장소를 클론합니다. 실제로 클론할 수 있는 저장소이니 실행해 보세요.

실행한 뒤 저장소 디렉터리에 들어가면 `git-example-repo` 디렉터리가 보입니다. 이 저장소 안에 서브모듈로 들어 있는 완전히 별개의 저장소입니다. 그 안으로 `cd`해 파일을 볼 수 있습니다!

> **서브모듈 디렉터리 트리에서 실행하는 모든 Git 명령은 서브모듈에만 적용됩니다!** 서브모듈 디렉터리 트리에서 커밋할 때는 특히 조심하세요. 서브모듈에서는 흔히 `HEAD`가 분리됩니다. 뒤에서 더 설명합니다.

`--recurse-submodules`를 깜빡했거나 서브모듈이 있다는 사실을 전혀 몰랐다고 합시다. 걱정하지 마세요! 나중에 받을 수 있습니다. 위 명령은 다음 두 개(또는 세 개)의 명령과 같습니다.

``` {.default}
$ git clone \
        git@github.com:beejjorgensen/git-example-submodule-repo.git
$ cd git-example-submodule-repo
$ git submodule update --recursive --init
```

이 명령도 서브모듈을 클론합니다. (`--recursive`는 서브모듈 안에 또 서브모듈이 있을 때(!!)를 위한 것이고, `--init`은 로컬 저장소에 필요한 몇 가지 장부 작업을 합니다. 이 정도면 꽤 두루뭉술한 설명이지요?)

당장은 이것만 알아도 작업을 시작하기에 충분할 수 있습니다! 기존 프로젝트를 빌드하는 데 저장소와 서브모듈만 필요했고, 여러분은 서브모듈 담당자가 아니라 빌드할 때 존재하기만 하면 되는 사람일 수 있습니다. 이제 작업을 시작하면 됩니다.

더 해야 할 일이 있다면 계속 읽으세요!

## 서브모듈 만들기 {#creating-a-submodule}

[i[Submodules-->Creating]<]

이미 저장소가 하나 있고, 다른 저장소를 서브모듈로 포함하기로 했다고 합시다.

주 저장소의 프로젝트가 빌드할 때 라이브러리 같은 다른 프로젝트에 의존하는 경우가 한 가지 사용 사례입니다. 라이브러리의 바이너리 형태를 쓰고 싶지 않거나 아예 존재하지 않아 직접 빌드해야 합니다.

서브모듈을 쓰지 않는다면 여러분의 저장소를 빌드하려는 사람마다 라이브러리 저장소도 클론하고 이 모든 것을 직접 다뤄야 합니다. `clone` 명령에 `--recurse-submodules` 플래그 하나만 더해 모든 준비를 마치고 빌드할 수 있다면 더 좋지 않을까요?

기존 저장소에 서브모듈을 추가하는 단계를 밟으며 어떻게 작동하는지 살펴봅시다.

제 예제 저장소나 여러분의 저장소, 또는 다른 누구의 저장소를 서브모듈로 사용해도 좋습니다. 다른 사람의 저장소를 서브모듈로 만들어도 그 사람은 알 수 없습니다.

먼저 테스트용 새 저장소를 만들고 재미 삼아 커밋 하나를 넣겠습니다.

``` {.default}
$ git init test_repo
  Initialized empty Git repository in /frotz/test_repo/.git/
$ cd test_repo
$ echo Hello, world > foo.txt
$ git add foo.txt
$ git commit -m added
```

서브모듈을 추가해 봅시다!

``` {.default}
$ git submodule add \
                  git@github.com:beejjorgensen/git-example-repo.git
  Cloning into '/home/beej/tmp/test_repo/git-example-repo'...
  remote: Enumerating objects: 4, done.
  remote: Counting objects: 100% (4/4), done.
  remote: Compressing objects: 100% (3/3), done.
  remote: Total 4 (delta 0), reused 4 (delta 0), pack-reused 0
  remote: (from 0)
  Receiving objects: 100% (4/4), done.
```

됐습니다! 아니, 거의 됐습니다. 상태를 확인해 봅시다.

``` {.default}
$ git status
  On branch main
  Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
	  new file:   .gitmodules
	  new file:   git-example-repo
```

스테이징 영역에 있는 저것들은 무엇일까요? `git-example-repo`는 서브모듈입니다. 디렉터리인데 Git이 "file"이라고 부르는 점이 조금 이상하지만, 서브모듈에 적용되는 특별 취급의 일부일 뿐입니다.

추가한 모든 서브모듈의 정보를 담는 `.gitmodules`라는 파일도 있습니다.

다른 사람이 이 저장소를 클론할 때 서브모듈 정보를 받을 수 있도록 이 두 파일(`git-example-repo`를 파일처럼 취급해서)을 저장소에 커밋해야 합니다.

``` {.default}
$ git commit -m "added submodule"
  [main cedea64] added submodule
   2 files changed, 4 insertions(+)
   create mode 100644 .gitmodules
   create mode 160000 git-example-repo
```

이제 준비됐습니다! 저장소를 클론하는 사람은 누구나 그 서브모듈 정보를 받습니다.

테스트 저장소로도 해 볼 수 있습니다. 테스트 저장소의 부모 디렉터리로 이동해 클론하세요.

``` {.default}
$ git clone --recurse-submodules test_repo test_repo2
```

그런 다음 `test_repo2`로 `cd`하면 서브모듈을 볼 수 있습니다.

> **로컬 저장소를 서브모듈로 만들 수 있나요?** 안 됩니다! 솔직히 자세히 읽어 보지는 않은 모종의 보안 위험 때문에 Git이 이를 금지합니다. 설정으로 제한을 재정의하는 방법이 있다고 하며, 서브모듈의 작동 원리를 이것저것 실험할 때 꽤 유용하리라 생각했지만, 2024년 말 현재 그 설정은 작동하지 않는 모양입니다. 따라서 서브모듈에는 네트워크 원격 저장소를 사용해야 합니다.

[i[Submodules-->Creating]>]

## 서브모듈의 커밋 설정하기 {#set-submodule-commit}

[i[Submodules-->Setting the commit]<]

이 절 제목은 대체 무슨 뜻일까요?

핵심은 이렇습니다. 서브모듈을 포함하는 저장소는 서브모듈 안의 특정 커밋을 가리킵니다. 즉, 서브모듈은 항상 바깥 저장소에 정의된 특정 커밋으로 체크아웃됩니다. (서브모듈의 `HEAD`가 브랜치에 붙어 있을 수도 있지만, 그렇지 않을 가능성도 아주 큽니다.)

결국 서브모듈이 있는 저장소를 만들 때 저장소가 사용할 서브모듈의 정확한 커밋을 지정할 수 있습니다. 그리고 중요한 점은 누군가 우리 저장소를 클론할 때 우리가 보는 것과 정확히 같은 서브모듈 커밋을 보게 된다는 것입니다.

덕분에 아주 특정한 라이브러리 버전을 서브모듈로 선택할 수 있으며, _서브모듈 저장소가 다른 곳에서 변경되었는지와 관계없이_ 우리 저장소를 클론한 사람은 모두 같은 버전을 받습니다. 다른 사람이 `main`을 원하는 곳으로 옮겨도, 새 `main` 커밋을 서브모듈로 페치하더라도 우리는 계속 고정해 둔 커밋 하나를 사용합니다.

사실상 서브모듈을 특정 커밋에 고정하는 것입니다. 서브모듈을 따로 개발하는 사람이 바깥 저장소의 빌드를 깨뜨리는 변경을 넣지 못하도록 이렇게 하는 것이 좋을 것입니다.

방법은 꽤 쉽습니다.

1. 서브모듈 디렉터리로 이동합니다.
2. 사용할 커밋으로 전환합니다. 브랜치 이름, 커밋 해시, 태그 등 `git switch`가 받는 무엇으로든 이 커밋을 가리킬 수 있습니다. `HEAD`를 분리한다면 `--detach`를 사용하세요.
3. 바깥 모듈의 디렉터리로 돌아갑니다.
4. 서브모듈 디렉터리를 추가합니다.
5. 커밋합니다.

GitHub의 제 테스트 저장소로 실험하려면 먼저 포크하여 쓰기 권한을 확보하세요.

앞 절에서 했던 것과 똑같이 `test_repo` 저장소를 만듭시다.

그리고 실험할 대상이 두 개 있도록 `test_repo2`로 `clone`합니다. (`--recurse-submodules` 플래그를 잊지 마세요!)

> **베어 저장소가 아닌 저장소를 클론하고 있는데, 이는 이상한 일입니다.** 클론해도 괜찮습니다. Git 경찰이 들이닥치지는 않습니다. 다만 그곳으로 푸시할 수 없을 뿐입니다. 이 시연에는 그것으로 충분합니다. 하지만 보통은 하지 않는 일입니다.

> **클론된 서브모듈 저장소의 분리된 `HEAD`도 눈여겨보세요!** `test_repo2/git-example-repo`에서 `git log`를 실행하면 첫 줄에 다음 내용이 보입니다.
>
> ``` {.default}
> (HEAD, origin/main, origin/HEAD, main)
> ```
> <!-- ` -->
>
> `HEAD`가 `main`에서 분리된 모습이 보이나요?
>
> 서브모듈의 `HEAD`가 언제 분리되는지 정확한 규칙을 안다고 하면 거짓말입니다. 하지만 드물지 않은 일입니다. 사실 보통은 분리되어 있다고 가정하고 필요하면 브랜치에 붙이는 것이 좋습니다. 뒤에서 더 설명합니다.

이제 `test_repo`에서 서브모듈 디렉터리로 들어가 서브모듈 저장소의 이전 버전을 체크아웃합시다. 여기서는 `main`의 바로 전 커밋을 체크아웃하겠습니다.

``` {.default}
$ cd test_repo/git-example-repo
$ git log
  commit cd1bf77d2ef08115b18d7f15a9c172dace1b2222
                           (HEAD -> main, origin/main, origin/HEAD)
  Author: Brian "Beej Jorgensen" Hall <beej@beej.us>
  Date:   Fri Dec 6 15:07:43 2024 -0800

      very important clarification

  commit d8481e125e6ef49e2fa8041b16b9dd3b8136b550
  Author: Brian "Beej Jorgensen" Hall <beej@beej.us>
  Date:   Fri Dec 6 15:07:13 2024 -0800

      improve functionality

  commit 433252748b7f9bf85e556a6a0196cdf38198fc43
  Author: Brian "Beej Jorgensen" Hall <beej@beej.us>
  Date:   Fri Jan 26 13:30:08 2024 -0800

      Added
```

이전 커밋으로 옮겨 봅시다.

``` {.default}
$ git switch --detach d8481e
  HEAD is now at d8481e1 improve functionality
```

여기까지 좋습니다. 이제 바깥 저장소로 `cd`해 돌아가 현재 상태를 살펴봅시다.

``` {.default}
$ cd ..
$ git status
  On branch main
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working
    directory)
	  modified:   git-example-repo (new commits)

  no changes added to commit (use "git add" and/or "git commit -a")
```

이것 보세요! 서브모듈 디렉터리가 수정된 것으로 나옵니다. "new commits"라고 하지만, 이는 "서브모듈의 내용이 전에 고정했던 커밋에서 달라졌다"고 알려 주는 것뿐입니다.

이를 추가하고 커밋합시다.

``` {.default}
$ git add git-example-repo
$ git commit -m "update submodule commit"
  [main dd69bb8] update submodule commit
   1 file changed, 1 insertion(+), 1 deletion(-)
```

이제 클론인 `test_repo2`에서 변경 사항을 풀합시다. 풀 명령의 `--recurse-submodule` 옵션에 주목하세요!

``` {.default}
$ cd ../test_repo2
$ git pull --recurse-submodules
  Fetching submodule git-example-repo
  Already up to date.
  Submodule path 'git-example-repo': checked out'
                         'd8481e125e6ef49e2fa8041b16b9dd3b8136b550'
```

이제 `test_repo2`에서 `git-example-repo` 서브모듈로 들어가 로그를 확인할 수 있습니다.

``` {.default}
$ git log
  commit d8481e125e6ef49e2fa8041b16b9dd3b8136b550 (HEAD)
  Author: Brian "Beej Jorgensen" Hall <beej@beej.us>
  Date:   Fri Dec 6 15:07:13 2024 -0800

      improve functionality

  commit 433252748b7f9bf85e556a6a0196cdf38198fc43
  Author: Brian "Beej Jorgensen" Hall <beej@beej.us>
  Date:   Fri Jan 26 13:30:08 2024 -0800

      Added
```

`HEAD`가 `test_repo`에서 설정한 것처럼 커밋 `d8481e`에 있는 것을 볼 수 있습니다. (`main`은 보이지 않습니다. 현재 `HEAD` 위치에서 보면 자식 커밋이라 로그에 나타나지 않는 것입니다. 물론 원한다면 여전히 그곳으로 전환할 수 있습니다.)

우리가 무엇을 했을까요? 한 저장소에서 서브모듈이 고정된 커밋을 바꾸고, 그 변경 사항을 다른 저장소로 풀했습니다!

[i[Submodules-->Setting the commit]>]

## 서브모듈의 최신 상태 받기 {#getting-submodule-latest}

[i[Submodules-->Getting latest]<]

다른 사람이 여러분의 저장소에서 서브모듈이 고정된 커밋을 업데이트했고, 여러분도 최신 상태를 받고 싶다고 합시다.

두 단계로 할 수 있습니다.

1. 바깥 저장소에서 `git pull`을 실행합니다. 그러면 서브모듈의 새 고정 커밋 번호를 담은 바깥 저장소의 최신 버전을 받습니다.

2. 바깥 저장소에서 다시 다음 명령을 실행합니다.

   ``` {.default}
   git submodule update --init --recursive
   ```

   서브모듈 데이터를 페치하고 올바른 커밋을 가리키도록 설정합니다.

[i[Submodules-->Getting latest]>]

## 실제 서브모듈 자체 업데이트하기 {#updating-the-actual-submodule-itself}

[i[Submodules-->Updating]<]

무슨 뜻일까요? 서브모듈에 어떤 라이브러리가 들어 있고 그 라이브러리의 버그를 고쳐야 한다고 합시다. 그리고 이 저장소를 서브모듈로 사용하거나 다른 방식으로 이용하는 사람들도 변경 사항을 받아야 합니다.

어떻게 하면 될까요?

독립된 저장소에서 작업하거나 서브모듈 디렉터리 _안에서 곧바로_ 작업할 수 있습니다.

### 다른 곳에서 서브모듈 저장소 수정하기 {#modify-the-submodule-repo-elsewhere}

제 조그만 인간 두뇌가 이해하기에 가장 쉬운 방법은 다른 어떤 저장소와도 무관하게 서브모듈을 따로 클론하는 것입니다. 즉, 전혀 서브모듈이 아닌 것처럼 클론합니다.

그러면 원하는 만큼 푸시하고 풀하고 수정할 수 있습니다.

[i[Fetch]] 모든 것을 고친 다음 서브모듈 디렉터리로 가서 `git fetch`를 실행해 새 커밋을 내려받을 수 있습니다.

이 시점에는 다음 명령을 실행하면 편리할 수 있습니다.

``` {.default}
$ git log HEAD^..origin/main
```

`HEAD`부터 `origin/main`까지 양 끝을 포함한 모든 커밋을 보여 주므로 어떤 작업이 이루어졌는지 볼 수 있습니다. (두 커밋이 관련되어 있다는 가정 아래에서 말입니다. 서로 갈라진 브랜치에 있다면 좀 더 창의력을 발휘해야 합니다.)

그런 다음 `HEAD`를 고정할 커밋을 고르고 그곳으로 전환한 뒤, 위의 [서브모듈의 커밋 설정하기](#set-submodule-commit)에서 설명한 대로 바깥 저장소에서 `add`/`commit`을 실행합니다.

### 서브모듈 디렉터리에서 서브모듈 저장소 수정하기 {#modify-the-submodule-repo-in-the-submodule-directory}

하지만 잠깐만요! 서브모듈 자체가 완전한 저장소라면 서브모듈 디렉터리에서 바로 편집할 수 있지 않을까요?

맞습니다! 정말로 할 수 있습니다.

유일하게 이상한 부분은 서브모듈의 `HEAD`가 분리되어 있을 수 있다는 점입니다. 이 방법을 택해 푸시하려면 푸시할 수 있는 브랜치를 반드시 체크아웃하세요.

예를 들면 다음과 같습니다.

``` {.default}
$ git switch main
```

그런 다음 변경하고 (서브모듈 디렉터리에서) 푸시합니다.

이 시점에도 바깥 저장소는 여전히 이전 커밋에 고정되어 있습니다. 따라서 위의 [서브모듈의 커밋 설정하기](#set-submodule-commit)에서 설명한 대로 바깥 저장소에서 `add`/`commit`을 실행해야 합니다.

[i[Submodules-->Updating]>]

## 서브모듈 상태 확인하기 {#getting-the-submodule-status}

[i[Submodules-->Status]<]

서브모듈이 어느 커밋에 고정되어 있는지 알아볼 때 매우 유용한 명령이 몇 가지 있습니다.

첫 번째는 `git submodule status`입니다. 서브모듈의 `HEAD`가 현재 어디에 있는지 알려 줍니다.

예를 들어 바깥 저장소에서 다음 명령을 실행합니다.

``` {.default}
$ git submodule status
898650e74c18cf4b30bdd07297d638de4a6fc7dd mysubmod (heads/main)
```

이는 `mysubmod` 디렉터리의 `HEAD`가 커밋 `89865`에 있다는 뜻입니다.

그런데 앞에 `+` 기호가 붙어 있다면 어떨까요?

``` {.default}
$ git submodule status
+1c10d608190194b7f9fbb9a442abd5c63c74cdfa mysubmod (heads/main)
```

`+`는 서브모듈의 `HEAD`가 커밋 `1c10d`에 있지만 바깥 저장소는 서브모듈을 _다른_ 커밋에 고정했다는 뜻입니다! 서브모듈을 풀해서 `HEAD`를 옮겼지만 바깥 저장소를 그에 맞춰 업데이트하지 않았을 때 이런 상황을 볼 수 있습니다.

`+`가 보이면 `git status`에서도 더 많은 정보를 알려 줍니다.

``` {.default}
$ git status
  On branch main
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working
    directory)
	  modified:   mysubmod (new commits)

  no changes added to commit (use "git add" and/or "git commit -a")
```

과연 그렇습니다. 서브모듈의 `HEAD`를 바꿨으므로 그 디렉터리가 `modified`로 표시됩니다.

위의 [서브모듈의 커밋 설정하기](#set-submodule-commit)에서 설명한 대로 저장소를 새 커밋에 고정하거나, 서브모듈의 `HEAD`를 바깥 모듈이 기대하는 위치로 되돌리면 더하기 기호를 없앨 수 있습니다.

> **커밋 해시 앞에 `-`가 붙을 수도 있습니다.** 이는 서브모듈이 초기화되거나
> 다운로드되지 않았다는 뜻입니다. `git
> submodule update --recursive --init`을 실행해 보세요.

바깥 모듈이 서브모듈의 `HEAD`가 어디에 있기를 기대하는지는 어떻게 알 수 있을까요? 다음 편리한 명령을 사용합니다.

``` {.default}
$ git ls-tree HEAD mysubmod
  160000 commit 898650e74c18cf4b30bdd07297d638de4a6fc7dd   mysubmod
```

[i[Submodules-->Status]>]

## 무대 뒤에서 벌어지는 몇 가지 일 {#some-behind-the-scenes}

사실 꼭 무대 뒤의 일은 아닙니다. 다만 명령줄 옵션으로 단축했던 서브모듈 초기화의 전통적인 단계를 짚고 싶었습니다.

예를 들어 처음 서브모듈이 있는 저장소를 클론할 때 다음 명령을 사용했습니다.

``` {.default}
$ git clone --recurse-submodules \
        git@github.com:beejjorgensen/git-example-submodule-repo.git
```

`--recurse-submodules`는 서브모듈을 클론하고 바로 사용할 수 있게 모두 설정하는 많은 작업을 대신했습니다.

그 옵션을 빼먹어도 다음처럼 해낼 수 있다고 했습니다.

``` {.default}
$ git clone \
        git@github.com:beejjorgensen/git-example-submodule-repo.git
$ cd git-example-submodule-repo
$ git submodule update --recursive --init
```

즉 `--recurse-submodules`가 그 일을 대신한 것입니다.

하지만 토끼 굴은 더 깊습니다! 저 `--init`도 많은 일을 대신합니다. 완전히 수작업으로 하는 과정으로 나눠 봅시다. 걱정 마세요. 두어 단계뿐입니다.

``` {.default}
$ git clone \
        git@github.com:beejjorgensen/git-example-submodule-repo.git
$ cd git-example-submodule-repo
$ git submodule init
$ git submodule update --recursive
```

결국 `git clone`의 `--recurse-submodules` 옵션은 실제로 무대 뒤에서 여러 명령을 실행하고 있었습니다.

조금 나눠 설명하겠습니다.

바깥 저장소를 처음 클론하면 서브모듈 원격 저장소의 URL과 디렉터리 이름이 적힌 `.gitmodules` 파일이 있습니다. 하지만 그 정보만으로는 부족합니다. Git이 그 파일을 분석하고 내부 장부를 설정하도록 `git submodule init`을 실행해야 합니다.

그런 다음 `git submodule update`를 실행해 사용할 서브모듈 데이터를 가져올 수 있습니다.

## 서브모듈 삭제하기 {#deleting-a-submodule}

[i[Submodules-->Deleting]<]

조금 번거롭지만 단계를 따르면 그리 어렵지 않습니다.

모든 작업은 바깥 저장소에서 이루어집니다. 이 예에서는 `mysubmod` 모듈을 삭제한다고 합시다. 다음 명령에서는 여러분의 모듈 이름으로 바꾸세요.

1. 서브모듈의 초기화를 해제합니다. 서브모듈 `HEAD`가 바깥 모듈이 기대하는 곳에 없다면 `-f`를 붙여 강제할 수 있습니다.

   ``` {.default}
   $ git submodule deinit mysubmod
   ```
   
   `git submodule init`을 부분적으로 되돌리는 단계입니다.

2. 바깥 저장소의 Git 내부에서 장부 정보를 제거합니다.

   ``` {.default}
   $ rm -rf .git/modules/mysubmod
   ```

   `git submodule init`을 되돌리는 나머지 단계입니다.

3. 서브모듈에 관한 절을 제거하여 `.gitmodules`를 정리합니다. 편집기에서 직접 해도 되고, 다음처럼 Git에 맡겨도 됩니다.

   ``` {.default}
   $ git config -f .gitmodules --remove-section submodule.mysubmod
   ```

   `git submodule add`를 되돌리는 단계입니다.

4. `.gitmodules` 파일을 스테이징 영역에 추가합니다.

   ``` {.default}
   $ git add .gitmodules
   ```

5. Git에서 서브모듈 트리를 삭제합니다. 삭제 작업도 스테이징 영역에 추가됩니다.

   ``` {.default}
   git rm --cached mysubmod
   ```

   `git submodule update`를 되돌리는 것과 어느 정도 비슷합니다.

6. `git status`를 실행해 준비가 됐는지 확인합니다.

   ``` {.default}
   $ git status
     On branch main
     Changes to be committed:
       (use "git restore --staged <file>..." to unstage)
	         modified:   .gitmodules
	         deleted:    mysubmod
   ```

   좋습니다.

7. 커밋하고 (필요하다면) 푸시합니다.

   ``` {.default}
   $ git commit -m "remove mysubmod submodule"
   $ git push
   ```

이것으로 서브모듈은 끝입니다.

[i[Submodules-->Deleting]>]

[i[Submodules]>]
