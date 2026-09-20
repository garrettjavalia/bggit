# 머리말 {#foreword}
<!-- Beej's guide to Git
# vim: ts=4:sw=4:nosi:et:tw=72
-->

<!-- No hyphenation -->
<!-- [nh[scalbn]] -->

<!-- Index see alsos -->
[is[Configuration-->Alias==>see Alias]]
[is[Branch-->on GitHub==>see GitHub, Branches]]
[is[Branch-->Diff==>see Diff, Between branches]]
[is[Detached `HEAD`==>see `HEAD`, Detached]]
[is[Fast-forward==>see Merge, Fast-forward]]
[is[Ignoring files==>see `.gitignore`]]
[is[Index==>see Stage]]
[is[`main`==>see Branch, `main`]]
[is[`master`==>see Branch, `master`]]
[is[`origin`==>see Remotes, `origin`]]
[is[`origin/main`==>see Branch, Remote tracking]]
[is[Recursion==>see Recursion]]
[is[Rename==>see Move]]
[is[Undelete==>see Remove, Undelete]]

[is[`git add`==>see Add]]
[is[`git add -p`==>see Patch mode]]
[is[`git blame`==>see Blame]]
[is[`git branch`==>see Branch]]
[is[`git checkout`==>see Checkout]]
[is[`git cherry-pick`==>see Cherry-pick]]
[is[`git clone`==>see Clone]]
[is[`git commit`==>see Commit]]
[is[`git config`==>see Configuration]]
[is[`git diff`==>see Diff]]
[is[`git fetch`==>see Fetch]]
[is[`git log`==>see Log]]
[is[`git merge`==>see Merge]]
[is[`git mv`==>see Move]]
[is[`git pull`==>see Pull]]
[is[`git push`==>see Push]]
[is[`git push --set-upstream`==>see Branch, Set upstream]]
[is[`git push -u`==>see Branch, Set upstream]]
[is[`git rebase`==>see Rebase]]
[is[`git reflog`==>see Reflog]]
[is[`git remote`==>see Remote]]
[is[`git reset`==>see Reset]]
[is[`git reset -p`==>see Patch mode]]
[is[`git restore`==>see Restore]]
[is[`git revert`==>see Revert]]
[is[`git rm`==>see Remove]]
[is[`git stash`==>see Stash]]
[is[`git status`==>see Status]]
[is[`git switch`==>see Switch]]
[is[`git tag`==>see Tag]]
[is[`git worktree`==>see Worktree]]

여러분, 다시 만났군요! 업계 전문가에서 대학 강사로 전향한 저는 Git 때문에 애를 먹는 학생들을 정말 많이 봅니다.

그 학생들을 누가 탓할 수 있을까요? Git은 함정과 병합 충돌, 분리된 헤드, 원격 저장소, 체리픽, 리베이스, 그리고 무슨 일을 하는지 알 수 없는 끝없는 명령이 가득한, 보기에는 지나치게 복잡한 시스템입니다.

그래서 곧바로 이 책의 목표가 나옵니다. 이 모든 것을 이해하고 Git 완전 초보에서 중급자로 올라가 봅시다! 명령과 약간의 동작 원리를 섞어 (적어도 말로는) 쉬운 내용부터 시작합니다. 그리고 Git 내부에서 무슨 일이 일어나는지 이해하는 것이 Git을 올바르게 사용하는 데 매우 중요하다는 사실을 살펴볼 것입니다.

이 안내서를 어느 정도 읽고 나면 실제로 Git을 높이 평가하고 즐겨 쓰게 될 가능성이 분명히 있다고 제가 *약속*합니다.

저는 Git을 여러 해 사용해 왔고(지금도 이 안내서의 소스 코드에 사용하고 있습니다), 시간이 지날수록 쉬워지다가 마침내는 제2의 천성이 된다고 자신 있게 말할 수 있습니다.

하지만 먼저, 몇 가지 기본 안내부터 하겠습니다!

## 독자 {#audience}

이 안내서의 초안은 제가 강사로 일했던(여러분이 이 글을 읽는 시점에 따라 지금도 일하고 있을지 모르는) 대학의 학생들을 위해 온라인에 공개했습니다. 그러니 제가 그들을 독자로 염두에 두었다고 보는 것이 자연스럽습니다.

하지만 대학생이 아닌 여러분을 생각해 좀 더 일반적인 관점에서 썼으며, 이 안내서에서 쓸모 있는 무언가를 얻는 다른 분도 충분히 많기를 바랍니다.

이 안내서는 여러분에게 다음과 같은 기본적인 POSIX 셸(즉 Bash, Zsh 등) 사용 능력이 있다고 가정합니다.

* `cd`, `ls`, `mkdir`, `cp` 같은 기본 명령을 알고 있습니다.
* 소프트웨어를 추가로 설치할 수 있습니다.

또한 POSIX 셸을 갖춘 Linux, BSD, Unix, macOS, WSL 같은 유닉스 계열 환경에 있다고 가정합니다. 여기서 멀어질수록(가령 PowerShell이나 Commodore 64를 쓸수록) 여러분이 직접 바꾸어 적용해야 할 내용이 많아집니다.

여기서 걸림돌은 당연히 Windows입니다. 다행히 Git for Windows에는 Git Bash라는 Bash 셸 변형이 들어 있습니다. [fl[WSL|https://learn.microsoft.com/en-us/windows/wsl/]]을 설치해 Windows 컴퓨터에서 Linux 환경을 실행할 수도 있습니다. 유닉스 계열 시스템은 해커에게 끝내주게 멋지므로 해커 기질이 있는 분께 이를 진심으로 권합니다. 더 나아가 여러분 모두 해커가 되기를 권합니다.

## 공식 홈페이지 {#official-homepage}

이 문서의 공식 위치는 (현재) [fl[https://beej.us/guide/bggit/|https://beej.us/guide/bggit/]]입니다.

## 이메일 방침 {#email-policy}

저는 보통 이메일 질문을 도울 수 있으니 편하게 보내셔도 되지만, 답장을 보장할 수는 없습니다. 상당히 바쁘게 살다 보니 여러분의 질문에 도저히 답할 수 없는 때가 있습니다. 그럴 때는 보통 메시지를 그냥 삭제합니다. 개인적인 감정은 없습니다. 여러분에게 필요한 자세한 답을 드릴 시간이 앞으로도 나지 않을 뿐입니다.

대체로 질문이 복잡할수록 제가 답할 가능성은 낮아집니다. 메일을 보내기 전에 질문의 범위를 좁히고, 관련 정보(플랫폼, 컴파일러, 나타나는 오류 메시지, 문제 해결에 도움이 되리라 생각하는 그 밖의 정보)를 빠짐없이 넣으면 답을 받을 가능성이 훨씬 커집니다.

답장을 받지 못했다면 조금 더 파고들어 답을 찾아보세요. 그래도 찾기 어렵다면 알아낸 정보를 담아 다시 메일을 보내 주세요. 그러면 제가 도울 만큼 충분한 정보가 되기를 바랍니다.

메일을 어떻게 쓰고 쓰지 말아야 하는지 잔소리를 늘어놓았으니, 이제 이 안내서가 여러 해 동안 받은 모든 찬사에 제가 _진심으로_ 감사한다는 말씀도 드리고 싶습니다. 사기가 크게 오르며, 좋은 일에 쓰인다는 소식을 들으면 기쁩니다! `:-)` 감사합니다!

## 미러링 {#mirroring}

이 사이트는 공개든 비공개든 얼마든지 미러링하셔도 됩니다. 공개 미러를 만들고 메인 페이지에서 링크해 주기를 원한다면 [`beej@beej.us`](mailto:beej@beej.us)로 연락해 주세요.

## 번역자를 위한 안내 {#note-for-translators}

[i[Translations]<]
이 안내서를 다른 언어로 번역하고 싶다면 [`beej@beej.us`](mailto:beej@beej.us)로 메일을 보내 주세요. 메인 페이지에서 번역본으로 링크하겠습니다. 번역본에 번역자의 이름과 연락처를 자유롭게 덧붙여도 됩니다.

아래 저작권 및 배포 절의 라이선스 제한에 유의하세요.
[i[Translations]>]

## 저작권 및 배포 {#copyright-and-distribution}

Beej's Guide to Git의 저작권은 © 2024 Brian "Beej Jorgensen" Hall에게 있습니다.

아래에 명시한 소스 코드와 번역에 관한 예외를 제외하면, 이 저작물은 크리에이티브 커먼즈 저작자표시-비영리-변경금지 3.0 라이선스에 따라 이용할 수 있습니다. 라이선스 사본은 [`https://creativecommons.org/licenses/by-nc-nd/3.0/`](https://creativecommons.org/licenses/by-nc-nd/3.0/)에서 보거나 Creative Commons, 171 Second Street, Suite 300, San Francisco, California, 94105, USA로 편지를 보내 요청할 수 있습니다.

라이선스의 “변경금지” 조항에는 다음과 같은 명시적인 예외가 하나 있습니다. 번역이 정확하고 안내서 전체를 빠짐없이 다시 게재한다면 이 안내서를 어떤 언어로든 자유롭게 번역할 수 있습니다. 원본 안내서와 동일한 라이선스 제한이 번역본에도 적용됩니다. 번역본에는 번역자의 이름과 연락처를 포함해도 됩니다.

이 문서에 제시된 프로그래밍 소스 코드는 이로써 퍼블릭 도메인에 기증하며, 어떠한 라이선스 제한도 전혀 받지 않습니다.

교육자는 이 안내서를 학생들에게 자유롭게 추천하거나 사본을 제공하시기 바랍니다.

자세한 내용은 [`beej@beej.us`](mailto:beej@beej.us)로 문의하세요.

## 헌사 {#dedication}

이런 안내서를 쓸 때 가장 어려운 일은 다음과 같습니다.

* 설명할 수 있을 만큼 충분히 자세하게 내용을 배우기
* 명확하게 설명할 최선의 방법을 찾아내기. 끝이 없어 보이는 반복 과정입니다
* 사실은 다른 모든 사람과 마찬가지로 이 모든 것을 이해하려 애쓰는 평범한 사람일 뿐인데도, 이른바 _권위자_로서 자신을 세상에 내놓기
* 수많은 다른 일이 관심을 끄는 와중에도 계속해 나가기

많은 분이 이 과정에서 저를 도왔습니다. 이 책이 나올 수 있게 해 준 분들께 감사를 표하고 싶습니다.

* 어떤 형태로든 지식을 나누기로 한 인터넷의 모든 분. 유익한 정보를 자유롭게 나누는 일이 인터넷을 훌륭한 곳으로 만듭니다.
* 오해를 부르는 설명부터 오타까지 온갖 문제에 수정 사항과 풀 리퀘스트를 보내 주신 모든 분.

감사합니다! ♥
