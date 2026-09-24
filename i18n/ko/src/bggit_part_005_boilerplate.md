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

학생들의 잘못은 아닙니다. Git은 함정과 병합 충돌, 분리된 헤드, 원격 저장소, 체리픽, 리베이스, 그리고 무슨 일을 하는지 알 수 없는 끝없는 명령이 가득한, 지나치게 복잡한 시스템입니다.

그러니 목표를 이렇게 잡아봅시다. 이 모든 것을 이해하고 Git 완전 초보에서 중급자로 올라가 보는 것이지요! 명령과 약간의 동작 원리를 섞어 쉽다고 알려진 내용부터 시작합니다. 그리고 Git 내부에서 무슨 일이 일어나는지 이해하는 것이 Git을 올바르게 사용하는 데 매우 중요하다는 사실을 알게 될 것입니다.

이 안내서를 어느 정도 읽고 나면 실제로 Git의 유용함을 이해하고 작업에 활용하는 것을 좋아하게 될 가능성이 분명히 있다고 제가 *약속*합니다.

저는 Git을 오랫동안 사용해 왔고(지금도 이 안내서의 소스 코드에 사용하고 있습니다), 쓰면 쓸수록 쉬워지고 습관이 될 것이라 확실히 보장할 수 있습니다.

하지만 먼저, 관례적인 안내를 하겠습니다!

(역자 주: 이제부터 Git이라는 영어 단어와 깃이라는 한국어 음차 표현을 편의에 따라 혼용하겠습니다.)

## 독자 {#audience}

이 안내서의 초안은 제가 강사로 일했던(여러분이 이 글을 읽는 시점에 따라 지금도 일하고 있을지 모르는) 대학의 학생들을 위해 온라인에 공개했습니다. 그러니 제가 대학생을 독자로 염두에 두었다고 보는 것이 자연스럽습니다.

그러나 읽는 사람이 대학생이 아니더라도 이 안내서가 유용하기를 바라며, 그런 독자들도 염두에 두고 좀 더 일반적인 관점에서 안내서를 작성했습니다.

이 안내서는 여러분에게 다음과 같은 기본적인 POSIX 셸(즉 Bash, Zsh 등) 사용 능력이 있다고 가정합니다. 예를 들자면,

* `cd`, `ls`, `mkdir`, `cp` 같은 기본 명령을 알고 있습니다.
* 소프트웨어를 추가로 설치할 수 있습니다.

또한 POSIX 셸을 갖춘 Linux, BSD, Unix, macOS, WSL 등의 유닉스 또는 그와 유사한 환경에 있다고 가정합니다. 여기서 멀어질수록(예를 들어 PowerShell이나 Commodore 64) 여러분이 직접 바꾸어 적용해야 할 내용이 많아집니다.

Windows는 당연히 걸림돌이 됩니다. 다행히 Git for Windows에는 Git Bash라는 Bash 셸 변형이 들어 있습니다. 또한 [fl[WSL|https://learn.microsoft.com/en-us/windows/wsl/]]을 설치해 Windows 컴퓨터에서 Linux 환경을 구성할 수도 있습니다. 유닉스 계열 시스템은 해커에게 정말 편하므로 해커 기질이 있는 분께 이를 진심으로 권합니다. 더 나아가 여러분 모두 해커가 되기를 권합니다. (역자 주: 여기서 해커는 컴퓨터를 능숙하게 활용하는 개발자라는 의미입니다.)

## 공식 홈페이지 {#official-homepage}

이 문서의 공식 위치는 (현재) [fl[https://beej.us/guide/bggit/|https://beej.us/guide/bggit/]]입니다.

## 이메일 정책 {#email-policy}

저는 보통 이메일에 답하고자 하니 편하게 보내셔도 되지만, 답장을 보장할 수는 없습니다. 상당히 바쁘게 살다 보니 여러분의 질문에 도저히 답할 수 없는 때가 있습니다. 그럴 때는 보통 메시지를 그냥 삭제합니다. 개인적인 감정은 없습니다. 여러분에게 필요한 자세한 답을 드릴 시간이 앞으로도 나지 않을 뿐입니다.

대체로 질문이 복잡할수록 제가 답할 가능성은 낮아집니다. 메일을 보내기 전에 질문의 범위를 좁히고, 관련 정보(플랫폼, 컴파일러, 나타나는 오류 메시지, 문제 해결에 도움이 되리라 생각하는 그 밖의 정보)를 빠짐없이 넣으면 답을 받을 가능성이 훨씬 커집니다.

답장을 받지 못했다면 조금 더 파고들어 답을 찾아보세요. 그래도 찾기 어렵다면 알아낸 정보를 담아 다시 메일을 보내 주세요. 어쩌면 제가 도와드리기에 충분할지도 모릅니다.

메일을 어떻게 써야 하는지 잔소리를 늘어놓았으니, 이제 이 안내서가 여러 해 동안 받은 모든 찬사에 제가 _진심으로_ 감사한다는 말씀도 드리고 싶습니다. 사기를 올려주고, 좋은 일에 쓰인다는 소식을 들으면 기쁩니다! `:-)` 감사합니다!

## 미러링 {#mirroring}

이 사이트는 공개든 비공개든 얼마든지 미러링하셔도 됩니다. 공개 미러를 만들고 메인 페이지에서 링크해 주기를 원한다면 [`beej@beej.us`](mailto:beej@beej.us)로 연락해 주세요.

## 번역자를 위한 안내 {#note-for-translators}

[i[Translations]<]
이 안내서를 다른 언어로 번역하고 싶다면 [`beej@beej.us`](mailto:beej@beej.us)로 메일을 보내 주세요. 메인 페이지에 번역본의 링크를 추가하겠습니다. 번역본에 번역자의 이름과 연락처를 자유롭게 덧붙여도 됩니다.

아래 저작권 및 배포 절의 라이선스 지침에 유의하세요.
[i[Translations]>]

## 저작권 및 배포 {#copyright-and-distribution}

(역자 주: 이 절은 법적 정보 보존을 위해 번역하지 않았습니다.)
(Translator's note : This section has not been translated to keep its legal information.)

Beej's Guide to Git is Copyright © 2024 Brian "Beej Jorgensen" Hall.

With specific exceptions for source code and translations, below, this
work is licensed under the Creative Commons Attribution-Noncommercial-No
Derivative Works 3.0 License. To view a copy of this license, visit
[`https://creativecommons.org/licenses/by-nc-nd/3.0/`](https://creativecommons.org/licenses/by-nc-nd/3.0/)
or send a letter to Creative Commons, 171 Second Street, Suite 300, San
Francisco, California, 94105, USA.

One specific exception to the "No Derivative Works" portion of the
license is as follows: this guide may be freely translated into any
language, provided the translation is accurate, and the guide is
reprinted in its entirety. The same license restrictions apply to the
translation as to the original guide. The translation may also include
the name and contact information for the translator.

The programming source code presented in this document is hereby granted
to the public domain, and is completely free of any license restriction.

Educators are freely encouraged to recommend or supply copies of this
guide to their students.

Contact [`beej@beej.us`](mailto:beej@beej.us) for more information.

## 헌사 {#dedication}

이런 안내서를 쓸 때 가장 어려운 일은 다음과 같습니다.

* 설명할 수 있을 만큼 충분히 자세하게 소재를 배우기
* 명확하게 설명할 최선의 방법을 찾아내기. 끝이 없어 보이는 반복 과정입니다
* 사실은 다른 모든 사람과 마찬가지로 이 모든 것을 이해하려 애쓰는 평범한 사람일 뿐이지만 이른바 _권위자_로서 자신을 세상에 내놓기
* 수많은 다른 일이 관심을 끄는 와중에도 계속해 나가기

이 과정에서 많은 분들이 저를 도와주셨고, 이 책이 나올 수 있게 해 준 분들께 감사를 표하고 싶습니다.

* 어떤 형태로든 지식을 나누기로 한 인터넷의 모든 분. 유익한 정보를 자유롭게 나누는 일이 인터넷을 지금과 같이 훌륭한 곳으로 만듭니다.
* 오해를 부르는 설명부터 오타 등 모든 것에 수정 사항과 풀 리퀘스트를 보내 주신 모든 분.

감사합니다! ♥

## 옮긴이의 말: 2026년 10월 번역, 원문 1.2.8 기반 한국어판 {#translators-message-202610}

AI가 발전하면서 많은 도구가 가치를 잃기도 하고, 반대로 대중화되기도 하는 시대입니다. 소스 코드 이력 관리 도구인 깃은 원래 개발자들의 전유물이었습니다. 그러나 최근에는 비개발자들도 그들의 업무에서 개발을 많이 하고, 그에 따라 개발 도구도 많이 활용하고 있습니다. 자연스럽게 예전보다 많은 사람들이 깃을 사용하게 되었습니다.

한편 개발자인 저도 최근에는 깃의 명령을 직접 입력하는 일이 거의 없어졌습니다. 그리고 깃의 내부 구조와 동작 원리를 이해하는 일은 깃을 활용하는 많은 사람들에게 대부분의 경우에 이전보다 더 불필요한 일이 되었습니다.

그래서 깃의 구조를 이해하고 복잡한 협업 상황에서 깃을 현명하게 다룰 수 있는 능력은 이전보다 더 희소해진 듯하고, 그 가치도 희박해진 듯합니다. 하지만 아직은 깃을 효율적으로 사용할 필요가 있고, 복잡한 협업 상황의 문제를 해결해야 하며, 누군가는 깃을 이해해야 다음 버전의 깃이 세상에 나올 수 있을 것입니다. 그렇기에 가치 있는 지식이 적어지는 이 시대에도 깃에 대한 지식을 담은 이 안내서와 그 번역에 가치가 있으리라 생각합니다.

이 번역은 AI를 통해 초안을 만드는 것으로 시작되었으며, 초안의 모든 부분을 원문과 비교 검토해서 자연스럽고 이해하기 쉬운 한국어 문장을 내놓기 위해 최선을 다했습니다. 제가 쓸 수 있는 모든 지식을 동원해 최선의 결과물을 만들었기에, AI의 시대에도 이 번역본을 세상에 내놓는 일에 작은 자부심을 느끼고, 결과물에 애정을 느낍니다.

여러분이 어떤 이유로 깃을 다루든, 이 안내서가 도움이 되리라 믿습니다. 또한 제가 이 번역본을 낼 때 그러했듯이, 여러분도 자부심과 애정을 느낄 수 있는 결과물을 만드실 수 있기를 바랍니다.

읽어주셔서 감사합니다. - 2026년 10월 30일, 정민석.
