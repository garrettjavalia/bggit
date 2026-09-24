# 부록: 연습장 만들기 {#making-playground}

[i[Playground]<]

프로그래밍 분야에서 일반적으로 *연습장(playground)*은 실제 운영 시스템을 망가뜨릴 걱정 없이 코드와 기술을 이것저것 만져 볼 수 있는 곳입니다.

온라인에도 이런 곳이 있지만, Git에서는 로컬 저장소를 직접 만드는 것도 똑같이 쉽다고 생각합니다.

현재 디렉터리에 `playground`라는 새 로컬 저장소를 만드는 방법은 다음과 같습니다. (이때 Git 저장소 안에 **있으면 안 됩니다**. 기존의 다른 저장소 바깥에 연습장을 만드세요.)

``` {.default}
$ git init playground
  Initialized empty Git repository in /user/playground/.git/
```

`playground`는 특별한 이름이 아닙니다. `foo`든 무엇이든 원하는 이름을 쓸 수 있습니다. 이 예에서는 그저 playground를 사용하겠습니다.

이 명령은 `playground`라는 새 하위 디렉터리를 만들고 그 안에 Git 저장소를 만들었습니다.

결말부터 살펴봅시다. 저장소는 어떻게 삭제할까요? 디렉터리를 지우기만 하면 됩니다.

```
$ rm -rf playground   # delete the playground repo
```

다시 만들어 봅시다.

``` {.default}
$ git init playground
```

우리에게는 모든 권한이 있습니다!

> **이 저장소는 이 컴퓨터에만 존재합니다.** 원격 저장소가 없고 푸시할 방법도 없습니다. 원한다면 나중에 그런 것을 추가할 수 있지만, 연습장은 보통 무언가를 시험해 보는 임시 공간입니다.

연습장으로 들어가 살펴봅시다.

``` {.default}
$ cd playground
$ ls -la
  total 4
  drwxr-xr-x  3 user group   18 Jul 13 14:43 .
  drwxr-xr-x 22 user group 4096 Jul 13 14:43 ..
  drwxr-xr-x  7 user group  119 Jul 13 14:43 .git
```

모든 메타데이터가 담긴 `.git`이라는 디렉터리가 있습니다.

> **이 디렉터리를 Git 저장소에서 평범한 디렉터리로 바꾸고 싶다면** 다음 명령을 실행할 수 있습니다.
>
> ``` {.default}
> $ rm -rf .git       # Delete the .git directory
> ```
>
> <!-- ` -->
> 다시 말하지만 우리에게는 모든 권한이 있습니다! 하지만 자제력을 발휘해서 아직은 이 명령을 실행하지 맙시다.

무엇을 할 수 있을까요?

무엇인들 *못* 하겠습니까? 파일을 만들고 현재 상태를 확인해 봅시다.

``` {.default}
$ echo "Hello, world" > hello.txt   # Create a file

$ ls -l

  total 4
  -rw-r--r-- 1 user group 13 Jul 13 14:47 hello.txt

$ git status

  On branch main

  No commits yet

  Untracked files:
    (use "git add <file>..." to include in what will be committed)
      hello.txt

  nothing added to commit but untracked files present (use "git
  add" to track)
```

이제 추적되지 않는 파일이 하나 생겼습니다.

`git add`로 추가하고 `git commit`으로 커밋할 수 있습니다. 브랜치를 만들고 병합하고, 충돌을 만들어 해결하고, `git rebase`와 `git reset`을 실행하는 등 온갖 일을 할 수 있습니다.

원격 저장소가 없으므로 할 수 없는 것은 푸시와 풀에 관련된 일뿐입니다.

그런데 이것조차 가능하게 만들 수 있습니다! 방법을 살펴봅시다.

## 베어 저장소 클론하기 {#cloning-bare-repos}

[i[Bare repo]] [i[Playground-->Cloning]]

*베어 저장소*는 작업 트리가 없는 저장소입니다. 일반적인 의미의 파일이 그 안에 존재하지 않으므로 들어가 파일을 볼 수 없습니다. 메타데이터와 커밋 스냅숏만 있습니다.

[i[Clone]]

베어 저장소도 클론하고 푸시하고 풀할 수 있습니다.

`--bare` 명령줄 옵션에 주목하면서 하나 만들어 봅시다(이 역시 원하는 이름을 붙여도 됩니다).

``` {.default}
$ git init --bare origin_repo
  Initialized empty Git repository in /user/origin_repo/
```

그 안을 들여다보면(분명히 말하지만 그럴 이유는 없습니다) 메타데이터와 디렉터리만 보입니다.

사용하려면 먼저 클론하는 편이 좋습니다. 편의를 위해 이를 만든 바로 그 디렉터리에서 다음 작업을 하겠습니다.

``` {.default}
$ git clone origin_repo playground
  Cloning into 'playground'...
  warning: You appear to have cloned an empty repository.
  done.
```

당연히 비어 있습니다. 아직 어떤 커밋도 만들지 않았기 때문입니다.

이제 이 디렉터리에는 저장소가 두 개 있습니다.

* `origin_repo`: 우리가 클론한 베어 저장소
* `playground`: 그 베어 저장소로부터 만든 클론

그 안으로 들어가 무슨 상태인지 봅시다.

``` {.default}
$ cd playground
$ git remote -v
  origin    /user/origin_repo (fetch)
  origin    /user/origin_repo (push)
```

원격 저장소가 있습니다! 당연하지요. 이 저장소를 클론했고 Git이 `origin` 원격 저장소를 자동으로 설정했기 때문입니다.

`origin`은 어떤 방식으로 식별되는 원격 저장소의 별칭일 뿐이라는 점을 기억하세요. `https`나 `ssh`로 시작하는 원격 저장소는 익숙하지만, 여기서는 디스크의 다른 하위 디렉터리가 곧 원격 저장소인 예를 보고 있습니다.

[i[Pushing]]

파일을 만들어 커밋하고 푸시할 수 있는지 확인해 봅시다!

``` {.default}
$ echo "Hello, world" > hello.txt
$ git add hello.txt

$ git commit -m added
  [main (root-commit) 4a82a14] added
   1 file changed, 1 insertion(+)
   create mode 100644 hello.txt

$ git push
  Enumerating objects: 3, done.
  Counting objects: 100% (3/3), done.
  Writing objects: 100% (3/3), 907 bytes | 907.00 KiB/s, done.
  Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
  To /user/origin_repo
   * [new branch]      main -> main

$ git branch -va
  * main                4a82a14 added
    remotes/origin/main 4a82a14 added
```

파일을 `origin`으로 성공적으로 푸시했습니다.

마지막으로 클론을 하나 더 만듭시다. 먼저 `origin_repo`가 있는 곳으로 `cd`해 돌아간 뒤, 이번에는 `playground2`로 다시 클론합니다.

``` {.default}
$ git clone origin_repo playground2
  Cloning into 'playground2'...
  done.
```

그곳으로 `cd`해서 무엇이 있는지 봅시다. 저장소의 클론이므로 앞서 `playground`에서 푸시한 `hello.txt`가 보여야 합니다.

``` {.default}
$ cd playground2 

$ ls
  hello.txt

$ cat hello.txt 
  Hello, world
```

*짜잔!* 파일이 있습니다!

`playground`와 `playground2`는 같은 저장소의 클론이므로 한쪽에서 푸시하고 다른 쪽에서 풀해 변경 사항을 받을 수 있습니다.

[i[Pulling]]

서로 충돌하는 변경 사항을 만들고 `git pull`이나 `git pull --rebase`를 시도하여 어떻게 잘못되는지, 또 어떻게 고치는지 살펴볼 수도 있습니다.

모든 것이 완전히 엉망이 되면 디렉터리를 지우고 다시 시작하면 됩니다. 연습장이니까요!

## 연습장 빌드 자동화하기 {#automating-playground-builds}

[i[Playground-->Automating]<] [i[Shell scripts]]

학습에 사용하는 저장소를 계속 없애고 다시 만드는 일은 지루할 수 있습니다. 실행할 명령을 담은 텍스트 파일인 _셸 스크립트_에 명령들을 넣기를 권합니다.

`buildrepo.sh`라는 새 텍스트 파일을 만들고 다음 내용을 넣는다고 합시다.

``` {.default}
rm -rf playground    # Remove old playground
git init playground  # Create a new one
cd playground
echo "Hello, world!" > hello.txt   # Create hello.txt
echo "foobar" > foobar.txt         # Create foobar.txt
git add hello.txt foobar.txt
git commit -m added
echo "foobar again" >> foobar.txt  # Append text
git add foobar.txt
git commit -m updated
```

그저 셸 명령을 모아 놓은 것입니다. 하지만 재미있는 점은 `buildrepo.sh`를 인수로 주어 `sh`(셸)를 실행하면 그 명령을 모두 순서대로 실행한다는 것입니다!

``` {.default}
$ sh buildrepo.sh
  Initialized empty Git repository in /user/playground/.git/
  [main (root-commit) 2239237] added
   2 files changed, 2 insertions(+)
   create mode 100644 foobar.txt
   create mode 100644 hello.txt
  [main 0533186] updated
   1 file changed, 1 insertion(+)
```

> **셸 스크립트를 디버깅하려면** `sh -x
> buildrepo.sh`처럼 실행하세요. 실행 중인 명령을 보여 줍니다.

그 뒤 그곳으로 `cd`해서 무슨 일이 일어났는지 볼 수 있습니다.

``` {.default}
$ cd playground
$ git log
  commit 05331869d77973dfbac38a31c40a44f99225e85d
  Author: User Name <user@example.com>
  Date:   Sat Jul 13 15:19:42 2024 -0700

      updated

  commit 2239237cc44d11e9479dcc610e5d02ad283766ce
  Author: User Name <user@example.com>
  Date:   Sat Jul 13 15:19:41 2024 -0700

      added

$ cat foobar.txt 
  foobar
  foobar again
```

초기화 명령을 셸 스크립트에 넣으면 그 시점의 "저장된 게임"이 생기는 것과 비슷합니다. 똑같이 설정된 연습장이 필요할 때마다 셸 스크립트를 다시 실행하기만 하면 됩니다.

[i[Playground-->Automating]>]

[i[Playground]>]
