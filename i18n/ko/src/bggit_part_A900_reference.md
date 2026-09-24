# 빠른 참조 {#quick-reference}

하려는 작업에 맞는 명령을 빠르게 찾아보세요! 다만 이 목록은 터무니없이
불완전합니다! 자세한 내용은 매뉴얼 페이지를 보세요!

이 참조 절에서는 다음과 같은 대체 표기를 사용합니다.

* `URL`: SSH, HTTP, 심지어 로컬 파일 등 어떤 URL. 보통 클론한 원본 URL입니다.
* `FILE`: 파일 경로. 예: `foo/bar.txt` 등
* `DIR`: 디렉터리 경로. 예: `foo/` 등
* `PATH`: 디렉터리 또는 파일 경로
* `BRANCH`: 브랜치 이름. 예: `main` 등
* `REMOTE`: 원격 저장소 이름. 예: `origin`, `upstream` 등
* `HASH`: 커밋 해시. `git log`나 `git reflog`에서 얻을 수 있습니다.
* `CMMT`: 커밋 해시, 브랜치 등 커밋을 가리키는 모든 것. 공식적으로는
  _tree-ish_라고 하지만, 계속 입력하기에는 글자가 너무 많았습니다.
* `VARIABLE`: Git 설정 변수 이름. 보통 마침표로 단어를 구분합니다.
* `VALUE`: Git 설정에 사용할 임의의 값
* `TAG`: 태그 이름

또한 `$`는 셸 프롬프트이므로 입력하지 마세요. `#` 뒤의 내용은 모두
주석입니다. 줄 끝의 백슬래시 `\`는 다음 줄에 이어진다는 뜻입니다.

## 용어집 {#glossary}

* **클론**: 보통 로컬에서 사용하려고 원격 저장소를 복제한 것, 또는 복제하는 일
* **커밋**: 특정 시점에 저장소에 있는 모든 파일의 스냅숏
* **포크**: 다른 사람의 GitHub 저장소를 자신의 GitHub 계정 아래에 클론하는 GitHub 기능
* **`HEAD`**: 현재 체크아웃하거나 전환한 커밋
* **인덱스**: *스테이징 영역*의 다른 이름
* **`main`**: 처음 생성되는 브랜치에 흔히 쓰는 이름
* **`master`**: 처음 생성되는 브랜치에 흔히 쓰는 또 다른 이름
* **`origin`**: 이 저장소를 클론해 온 원격 저장소의 기본 이름
* **풀 리퀘스트**: 저장소의 포크에서 만든 변경 사항을 원래 포크해 온 저장소로 돌려보내는 방법
* **원격 저장소**: 다른 저장소의 URL에 붙인 별칭. 보통 HTTP나 SSH URL입니다.
* **스테이징 영역**: 커밋으로 묶을 파일을 모으는 곳
* **`upstream`**: 포크해 온 원격 저장소에 관례적으로 붙이는 이름. 자동으로 설정되지는 않습니다.
* **작업 트리**: 눈으로 볼 수 있는 파일의 모음. `HEAD` 커밋과 달라진 내용이 있을 수 있습니다.
* **WT**: 작업 트리의 줄임말

## 파일 상태 {#file-states-1}

* **추적되지 않음**에서:
  * 수정되지 않음으로: `git add FILE`
* **수정되지 않음**에서:
  * 수정됨으로: 편집기로 편집하고 저장
  * 추적되지 않음/삭제됨으로: `git rm --cached FILE`
* **수정됨**에서:
  * 스테이징됨으로: `git add FILE`
  * 수정되지 않음으로: `git restore FILE` (변경 사항 폐기)
  * 추적되지 않음/삭제됨으로: `git rm --cached FILE`
* **스테이징됨**에서:
  * 수정되지 않음으로: `git commit FILE` (커밋 완료)
  * 수정됨으로: `git restore --staged FILE` (스테이징 해제)
  * 양쪽에서 수정됨으로: `git checkout --merged FILE` (병합 중)

## 설정 {#configuration-1}

[i[Configuration]i<]

모든 `git config` 명령에서 전체에 적용하려면 `--global`을 지정하고, 이
저장소에만 값을 설정하려면 생략합니다.

``` {.default}
$ git config set VARIABLE VALUE
$ git config get VARIABLE
$ git config list
$ git config unset VARIABLE
$ git config --edit
```

이전 버전에서 쓰던 구식 명령은 다음과 같습니다.

``` {.default}
git config user.email                     # Get
git config user.email "user@example.com"  # Set
git config --unset user.email             # Delete
git config --list                         # List
git config --edit                         # Edit
```

### 신원 설정하기 {#set-identity}

[i[Configuration-->Name and email]i]
사용자 이름과 이메일:

``` {.default}
$ git config set --global user.name "Your Name"
$ git config set --global user.email "your-email@example.com"
```

[i[Configuration-->SSH identity]i]
SSH 신원:

``` {.default}
$ git config set core.sshCommand \
    "ssh -i ~/.ssh/id_alterego_ed25519 -F none"
```

### 기본 브랜치 설정하기 {#set-default-branch}

[i[Configuration-->Default branch]i]

새 저장소를 만들 때 처음 생성되는 브랜치입니다.

``` {.default}
$ git config set --global init.defaultBranch BRANCH
```

흔한 이름은 `main`, `master`, `trunk`, `development`입니다. 이 안내서에서는
`main`을 사용합니다.

### 기본 풀 동작을 병합 또는 리베이스로 설정하기 {#set-default-pull-behavior-to-merge-or-rebase}

[i[Configuration-->Pull rebase behavior]i]
``` {.default}
$ git config set --global pull.rebase false   # Merge
$ git config set --global pull.rebase true    # Rebase
```

### 기본 편집기, difftool, mergetool 설정하기 {#set-default-editor-difftool-and-mergetool}

기본 편집기를 Vim으로, 기본 mergetool과 difftool을 Vimdiff로 설정하고,
도구 실행 전 질문과 mergetool 백업을 끕니다.

[i[Configuration-->Editor]i]
[i[Configuration-->Difftool]i]
[i[Configuration-->Mergetool]i]
``` {.default}
$ git config set core.editor vim
$ git config set diff.tool vimdiff
$ git config set difftool.prompt false
$ git config set difftool.vimdiff.cmd 'vimdiff "$LOCAL" "$REMOTE"'
$ git config set merge.tool=vimdiff
$ git config set mergetool.vimdiff.cmd \
                             'vimdiff "$LOCAL" "$REMOTE" "$MERGED"'
$ git config --global set mergetool.keepBackup false
```

### 다채로운 Git 출력 {#colorful-git-output}

[i[Configuration-->Color output]i]
``` {.default}
$ git config set color.ui true   # Or false
```

### 자동 교정 {#autocorrect}

[i[Configuration-->Autocorrect]i]
자동 교정은 사용자가 의도했다고 판단한 명령을 자동으로 실행합니다. 예를
들어 `git poush`를 입력하면 `git push`를 뜻한다고 가정합니다.

``` {.default}
$ git config set help.autocorrect 0   # Ask "Did you mean...?"
$ git config set help.autocorrect 7   # Wait 0.7 seconds before run

$ git config set help.autocorrect immediate  # Just guess and go
$ git config set help.autocorrect prompt     # Prompt then go
$ git config set help.autocorrect never      # Turn autocorrect off
```

### 줄바꿈 변환 {#newline-translation}

줄바꿈 자동 변환을 처리합니다. Windows(WSL 제외)에서는 true, 그 밖의
환경에서는 false로 설정하기를 권합니다.

[i[Configuration-->Newline translation]i]
``` {.default}
$ git config set core.autocrlf true  # Windows (non-WSL)
$ git config set core.autocrlf false # WSL, Linux, Mac, C64, etc.
```

### 별칭 {#aliases}

[i[Alias]i<]

별칭을 설정하는 몇 가지 예입니다.

``` {.default}
$ git config set --global alias.logn 'log --name-only'
$ git config set alias.aa "add --all"
$ git config set alias.logc "log --oneline --graph --decorate"
$ git config set alias.diffs "diff --staged"
$ git config set alias.lol "log --graph"\
" --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s"\
" %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

별칭 가져오기:

``` {.default}
$ git config get alias.logx
$ git config get --all --show-names --regexp '^alias\.'
$ git config set alias.aliases \
    "config get --all --show-names --regexp '^alias\.'"
```

별칭 실행 추적하기:

``` {.default}
$ GIT_TRACE=1 git logx
```

[i[Alias]i>]
[i[Configuration]i>]

## 저장소 만들기와 클론하기 {#creating-and-cloning-repos}

[i[Clone]i]

``` {.default}
$ git clone URL       # Clone a URL
$ git clone URL DIR   # Clone a URL to directory
$ git init DIR        # Init repo at directory
$ git init .          # Init repo in the current directory
```

## 추가, 이름 변경, 삭제, 커밋 {#adding-renaming-deleting-committing}

[i[Add]i]
[i[Move]i]
[i[Remove]i]
[i[Add]i]
[i[Commit]i<]
``` {.default}
$ git add PATH             # Add PATH to the repo
$ git mv FILE1 FILE2       # Rename ("Move") FILE1 to FILE2
$ git mv FILE2 FILE1       # Undo the above rename
$ git rm FILE              # Delete ("Remove") FILE
$ git add -p FILE          # Add file in patch mode

$ git commit               # Commit files on stage
$ git commit -m "message"  # Commit with a message
```

커밋 수정하기—무슨 일이 벌어질지 잘 알지 못한다면 이미 푸시한 커밋을
수정하지 마세요!

[i[Commit-->Amending]i]
``` {.default}
$ git commit --amend               # Amend last commit
$ git commit --amend -m "message"  # Amend with commit message
$ git commit --amend --no-edit     # Don't change commit message
```

[i[Remove-->Unstaging]i]
스테이징된 파일의 삭제를 취소하려면 다음 두 명령을 순서대로 실행합니다.

``` {.default}
$ git restore --staged FILE
$ git restore FILE
```

삭제된 파일을 복구하려면 이전 커밋에서 직접 가져오거나, 파일을 삭제한
커밋을 되돌릴 수 있습니다.

## 상태 확인하기 {#getting-status}

[i[Status]i]
[i[Log]i]
``` {.default}
$ git status             # Show current file states
$ git log                # Show the commit logs
$ git log --name-only    # Also list changed files
$ git log CMMT           # Show log from a specific branch
$ git log CMMT1 CMMT2    # Show logs from multiple branches

$ git log CMMT1..CMMT2   # Show logs from CMMT2 since it
                         # diverged from CMMT1
$ git log CMMT1...CMMT2  # Show logs from CMMT1 and CMMT2
                         # since they diverged
```

## Diff 확인하기 {#getting-a-diff}

[i[Diff]i<]
``` {.default}
$ git diff                # Diffs between working tree and stage
$ git diff HEAD^          # Diff from the previous commit to here
$ git diff HEAD^^         # Diff from the 2nd last commit to here
$ git diff HEAD~3 HEAD~2  # Diff from 3rd last to 2nd last commit
$ git diff CMMT           # Diff between CMMT and now
$ git diff CMMT1 CMMT2    # Diff between two commits (older first)

$ git diff CMMT1...CMMT2  # Diff between CMMT2 and the common
                          # ancestor of CMMT1 and CMMT2

$ git diff HEAD~3^!       # Diff between HEAD~3 and its parent
$ git diff -- FILE        # Run a diff just for a specific file
$ git diff HEAD^ -- FILE  # Run a diff just for a specific file

$ git diff -U5          # Show 5 lines of context
$ git diff -w           # Ignore whitespace
$ git diff --name-only  # Only show filenames of changed files
$ git diff --staged     # Diffs between stage and repo
$ git difftool          # Diffs using the configured difftool
```
[i[Diff]i>]

## 브랜치 {#branches}

로컬 브랜치는 `branchname`처럼 보입니다. 원격 추적 브랜치는
`remote/branchname`처럼 보입니다.

[i[Switch]i<]

``` {.default}
$ git switch BRANCH         # Switch to a branch
$ git switch --detach HASH  # Detach HEAD to a commit
$ git switch -              # Switch back to previous commit
```

``` {.default}
$ git switch --detach HEAD^   # Switch to previous commit
$ git switch --detach HEAD^^  # Switch to 2 commit ago
$ git switch --detach HEAD~3  # Switch to 3 commits ago
$ git switch --detach HEAD~99 # Switch to 99 commits ago
```

``` {.default}
$ git switch main   # Reattach HEAD to main
```

[i[Branch]i<]
``` {.default}
$ git branch -v   # List all branches
$ git branch -va  # List all including remote tracking branches
```

``` {.default}
$ git switch -c BRANCH        # Create and switch to BRANCH
$ git branch BRANCH           # Create BRANCH at HEAD
$ git branch BRANCH1 BRANCH2  # Create BRANCH1 at BRANCH2
```
[i[Switch]i>]

``` {.default}
$ git branch -d BRANCH   # Delete fully merged branch
$ git branch -D BRANCH   # Force delete unmerged branch
```

[i[Branch]i>]

구식 방식(가능하면 `switch`를 사용하세요):

[i[Checkout]i]
``` {.default}
$ git checkout CMMT      # Detach HEAD to a commit
$ git checkout HEAD^     # Detach HEAD to previous commit
$ git checkout HEAD~2    # Detach HEAD to second previous commit
```

## 풀, 푸시, 페치 {#pulling-and-pushing-and-fetching}

[i[Pull]i]
``` {.default}
$ git pull               # Pull from remote and merge or rebase
$ git pull --ff-only     # Only allow fast-forward merges
$ git pull --rebase      # Force a rebase on pull
$ git pull --no-rebase   # Force a merge on pull
```

[i[Push]i]
``` {.default}
$ git push                     # Push this branch to its remote

$ git push REMOTE BRANCH       # Create remote tracking branch and
                               # push to remote

$ git push -u REMOTE BRANCH    # Create remote tracking branch and
                               # push to remote, and use subsequent
                               # `git push` commands for this local
                               # branch

$ git push -u origin branch99  # Example

$ git push --tags              # Push all tags to origin
$ git push REMOTE --tags       # Push all tags to specific remote
$ git push REMOTE tag3.14      # Push single tag
```

[i[Fetch]i]
``` {.default}
$ git fetch        # Get data from remote but don't merge or rebase
$ git fetch REMOTE # Same, for a specific remote

```
## 병합 {#merging}

[i[Merge]i]
``` {.default}
$ git merge CMMT     # Merge commit or branch into HEAD
$ git merge --abort  # Rollback the current merge
$ git mergetool      # Run mergetool to resolve a conflict

$ git checkout --merged FILE   # Unstage resolved files
```

충돌이 발생하면 언제든 `--abort`할 수 있습니다. 계속하려면 다음과 같이 합니다.

1. 충돌을 해결합니다.
2. 고친 파일을 추가합니다.
3. 커밋하여 병합을 완료합니다.

## 원격 저장소 {#remotes}

[i[Remote]i]
``` {.default}
$ git remote -v                       # List remotes
$ git remote set-url REMOTE URL       # Change remote's URL
$ git remote add REMOTE URL           # Add a new remote
$ git remote rename REMOTE1 REMOTE2   # Rename REMOTE1 to REMOTE2
$ git remote remove REMOTE            # Delete REMOTE
```

## 파일 무시하기 {#ignoring-files}

[i[`.gitignore` file]i<]

저장소에 `.gitignore` 파일을 추가합니다. 이 디렉터리와 그 아래에 있는
서브모듈이 아닌 모든 하위 디렉터리에 적용됩니다. 무시할 파일을 나타내는
패턴을 이 파일에 추가하세요. `#` 뒤에는 주석을 쓸 수 있고 빈 줄은 무시됩니다.

`.gitignore` 예:

``` {.default}
foo.aux     # Ignore specific file "foo.aux"
foo.*       # Ignore all files that start with "foo."
*.tmp       # Ignore all files that end with ".tmp"
frotz/      # Ignore all files in the "frotz" directory
foo[12].txt # Ignore "foo1.txt" and "foo2.txt"
foo?        # Ignore "foo" followed by any single character
frotz/bar   # Ignore file "bar" in directory "frotz"
*           # Ignore everything
```

앞서 나온 규칙의 예외입니다. 하위 디렉터리의 `.gitignore` 파일에서 부모
디렉터리의 규칙을 재정의할 때도 유용합니다.

``` {.default}
*.txt       # Ignore all text files
!keep.txt   # Except "keep.txt"
```
[i[`.gitignore` file]i>]

## 리베이스 {#rebasing}

[i[Rebase]i]
``` {.default}
$ git rebase CMMT        # Rebase changes onto commit

$ git rebase -i CMMT     # Interactive rebase (squashing commits)

$ git rebase --continue  # Continue processing from conflict
$ git rebase --skip      # Skip a conflicting commit
$ git rebase --abort     # Bail out of rebasing
```

[i[Pull-->Force rebase or merge]i]
```
$ git pull --rebase      # Force a rebase on pull
$ git pull --no-rebase   # Force a merge on pull
```

## 스태시 {#stashing}

스태시는 스택에 저장됩니다.

[i[Stash]i]
``` {.default}
$ git stash push    # Stash changed files
$ git stash         # Effectively the same as "push"
$ git stash FILE    # Stash a specific file
$ git stash pop     # Replay stashed files on working tree
$ git stash list    # List stashed files

$ git stash pop 'stash@{1}'   # Pop stash at index 1
$ git stash pop --index 1     # Same thing
$ git stash drop 'stash@{1}'  # Drop stash at index 1
$ git stash drop --index 1    # Same thing
```

## 되돌리기 {#reverting}

[i[Revert]i]
``` {.default}
$ git revert CMMT     # Revert a specific commit
$ git revert -n CMMT  # Revert but don't commit (yet)

$ git revert CMMT1 CMMT2    # Revert multiple commits
$ git revert CMMT1^..CMMT2  # Revert a range (oldeest first)

$ git revert --continue  # Continue processing from conflict
$ git revert --skip      # Skip a conflicting commit
$ git revert --abort     # Bail out of reverting
```

## 리셋 {#resetting}

[i[Reset]i]
모든 리셋은 `HEAD`와 현재 체크아웃한 브랜치를 지정한 커밋으로 옮깁니다.

``` {.default}
$ git reset --mixed CMMT  # Set stage to CMMT, don't change WT
$ git reset CMMT          # Same as --mixed
$ git reset --soft CMMT   # Don't change stage or working tree
$ git reset --hard CMMT   # Set stage and WT to CMMT

$ git reset -p CMMT       # Reset file in patch mode
```

구식 사용법:

``` {.default}
$ git reset FILE   # Same as "git restore --staged FILE"
```

## Reflog {#the-reflog}

[i[Reflog]i]
``` {.default}
$ git reflog      # Look at the reflog
```

…인정합니다. 이 절에는 정보가 조금 더 필요하겠네요.

## 체리픽 {#cherry-pick}

[i[Cherry-pick]i]
``` {.default}
$ git cherry-pick CMMT   # Cherry-pick a particular commit
```

## Blame {#blame}

[i[Blame]i]
``` {.default}
$ git blame FILE                # Who is responsible for each line
$ git blame --date=short FILE   # Same, shorter date format
```

## 서브모듈 {#submodules-1}

[i[Submodules]i<]
``` {.default}
$ git clone --recurse-submodules URL       # Clone with submodules
$ git submodule update --recursive --init  # If you cloned without

$ git submodule add URL              # Add submodule
$ git add DIR                        # Add to repo
$ git pull --recurse-submodules      # Pull including submodules

$ git submodule status               # Submodule status
$ git ls-tree HEAD DIR               # Show submod pinned commit
$ git submodule init                 # Set up bookeeeping
$ git submodule update               # Bring in missing submods
$ git submodule update --recursive   # Handle submods of submods
```

[i[Submodules-->Deleting]i]
서브모듈을 삭제하려면 다음 명령을 순서대로 실행합니다. 이 예에서 DIR은
서브모듈 디렉터리의 이름입니다.

``` {.default}
$ git submodule deinit DIR
$ rm -rf .git/modules/DIR
$ git config -f .gitmodules --remove-section submodule.DIR
$ git add .gitmodules
$ git rm --cached DIR
$ git commit -m "remove DIR submodule"
```
[i[Submodules]i>]

## 태그 {#tags-1}

[i[Tag]i<]

``` {.default}
$ git tag     # List tags
$ git tag -l  # List tags
```

``` {.default}
$ git tag TAG          # Create a tag on HEAD
$ git tag TAG CMMT     # Create a tag on a specific commit
$ git tag -a TAG       # Create an annotated tag
$ git tag -a TAG CMMT  # On a specific commit
$ git tag -a TAG -m "message" # Add a message to the tag
```

``` {.default}
$ git push --tags          # Push all tags to origin
$ git push REMOTE --tags   # Push all tags to specific remote
$ git push REMOTE tag3.14  # Push specific tag
```

``` {.default}
$ git tag -d tagname          # Delete a tag locally
$ git push REMOTE -d tagname  # Delete a tag on a remote
```

[i[Tag]i>]

## 작업 트리 {#worktrees-1}

[i[Worktree]i<]

``` {.default}
$ git worktree list   # List worktrees

$ git worktree add DIR CMMT            # Add worktree at CMMT
$ git worktree add --detach DIR CMMT   # Add, detach head at CMMT
$ git worktree add DIR HASH            # Add, detach head at HASH

$ git worktree remove DIR          # Remove worktree
$ git worktree remove --force DIR  # Remove, lose uncommitted mods
```

[i[Worktree]i>]
