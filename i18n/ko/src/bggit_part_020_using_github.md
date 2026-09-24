# GitHub: 사용 방법 {#github-how-to-use-it}

[i[GitHub]<]

앞에서 GitHub(Microsoft가 소유하고 운영하는 Git용 독점 웹
프런트엔드)는 Git이 아니라고 했는데, 사실입니다. Git을 사용하는 데
GitHub를 건드릴 필요조차 전혀 없다는 말도 사실입니다.

그렇기는 해도 사람들이 GitHub를 사용하는 일이 *정말* 흔하므로, 이
장에서는 GitHub를 설정하겠습니다.

여기서는 새 GitHub 계정을 만들고 인증이 어떻게 작동하는지 살펴봅니다.
한 번만 하면 되는 설정이 몇 가지 있습니다.

이미 GitHub 계정이 있다면 해당 절은 건너뛰어도 됩니다.

GitHub CLI나 SSH 키로 이미 인증을 설정했다면 그 절도 건너뛰어도 됩니다.

GitHub를 사용할 필요가 없다면 이 장 전체를 건너뛰어도 됩니다!

## GitHub 계정 만들기 {#making-a-github-account}

[i[GitHub-->Account creation]]

[fl[GitHub|https://github.com/]]로 가서 `Sign Up`을 클릭하세요. 안내에
따라 진행합니다.

마지막에는 홈 화면 대시보드에 도착합니다.

## GitHub에 새 저장소 만들기 {#creating-a-new-repo-on-github}

[i[GitHub-->Repo creation]]

여기서는 자신이 소유하는 저장소를 GitHub에 만듭니다. 로컬 저장소를
만드는 것은 아닙니다. 로컬 저장소를 만들려면 저장소를 클론해야 하며,
그 작업은 나중에 하겠습니다.

GitHub 대시보드 왼쪽에는 초록색 `New` 버튼이 있습니다.

또한 오른쪽 위 가운데에 `+` 드롭다운이 있고 그 안에는 "New
Repository" 옵션이 있습니다. 둘 중 하나를 클릭하세요.

그다음 페이지에서 다음과 같이 합니다.

1. "Repository name"을 입력합니다. 이미 같은 이름의 저장소만 없다면
   어떤 이름이든 괜찮습니다. 이 예제에서는 `test-repo`를 사용합시다.

2. "Add a README file" 체크박스를 선택합니다.

   (나중에는 이 새 저장소로 푸시할 로컬 저장소가 이미 있을 수도
   있습니다. 그런 경우에는 이 체크박스를 선택하지 **마세요**. 선택하면
   푸시가 되지 않습니다.)

3. 아래쪽의 `Create repository`를 클릭합니다.

이것으로 끝입니다.

## 인증 {#authentication}

[i[GitHub-->Authentication]]

클론으로 넘어가기 전에 인증에 관해 이야기해 봅시다. 도입부의 앞 장에서
사용자 이름과 비밀번호를 이용한 로그인이 비활성화되었다는 것을
보았으므로, 다른 방법을 써야 합니다.

몇 가지 선택지가 있습니다.

* GitHub CLI라는 도구 사용하기
* SSH 키 사용하기
* 개인 인증 토큰 사용하기

GitHub CLI가 아마 더 쉽습니다. SSH 키는 괴짜미가 넘칩니다. 저는 개인
액세스 토큰으로 인증할 수 있다는 사실을 최근에야 알았기 때문에 그에
대해서는 사실 별로 할 말이 없습니다.

개인적으로 저는 SSH 키를 사용합니다. 하지만 다른 사람들은...
안 쓰기도 합니다. 선택은 여러분의 몫입니다.

GitHub 인증이 이미 작동한다면 이 절들은 건너뛰세요.

그렇지 않다면 이 중 하나(가령 SSH)를 골라 사용하세요.

### GitHub CLI {#github-cli}

[i[GitHub-->GitHub CLI setup]]

GitHub를 위한 명령줄 인터페이스입니다. 여러 가지 일을 하는데, 그중
하나는 원격 저장소에 실제로 푸시하는 등의 작업을 할 수 있도록 인증
도우미를 제공하는 것입니다.

[fl[GitHub CLI 페이지를 방문하고|https://cli.github.com/]] 설치 안내에
따르세요. WSL, Linux 또는 다른 Unix 계열을 사용한다면 다른 플랫폼용
[fl[설치 안내|https://github.com/cli/cli#installation]]를 참고하세요.

설치하고 나면 'gh --version'을 실행해 다음과 같은 버전 정보를 볼 수
있어야 합니다.

``` {.default}
$ gh --version
  gh version 2.42.1 (2024-01-15)
  https://github.com/cli/cli/releases/tag/v2.42.1
```

그런 다음 다음 두 명령을 실행해야 합니다.

``` {.default}
$ gh auth login
$ gh auth setup-git
```

첫 번째 명령(`login`)은 로그인 과정을 안내합니다. 로그아웃하면 이
과정을 다시 거쳐야 합니다.

SSH와 HTTP 중 어느 것을 사용할지 물을 것입니다. SSH 키를 설정해
두었다면 SSH를 권합니다. 설정하지 않았다면 질문이 나올 때 "HTTP"를
선택하세요. 주된 차이는 HTTP를 사용하면 로그인 자격 증명이 암호화되지
않은 채 저장되지만, SSH를 사용하면 암호로 보호할 수 있다는 점입니다.
SSH 키 설정 방법은 아래를 참고하세요.

로그인 방법을 물으면 웹 브라우저로 로그인하는 것이 가장 쉽습니다.

두 번째 명령(`setup-git`)은 한 번만 실행하면 됩니다. 인증에 도움이
되는 몇 가지 설정을 전역 구성에 추가할 뿐입니다.

### SSH 키 {#ssh-keys}

[i[GitHub-->SSH setup]]

GitHub CLI를 설치하고 사용하고 싶지 않다면 이 방법을 대신 택할 수
있습니다. 과정은 더 복잡하지만 괴짜 세계에서는 더 인정받습니다. 제가
쓰는 방법이기도 합니다.

SSH 키 쌍이 이미 있다면 키 생성 단계는 건너뛰어도 됩니다. `ls ~/.ssh`를
실행했을 때 `id_rsa.pub`이나 `id_ed25519.pub` 같은 파일이 보인다면 키
쌍이 있는 것입니다.

새 키 쌍을 만들려면 다음 명령을 실행하세요.

``` {.default}
$ ssh-keygen -t ed25519 -C youremail@example.com
```

(`-C`는 키에 "주석"을 설정합니다. 어떤 내용이든 쓸 수 있지만 이메일
주소를 흔히 사용합니다.)

그러면 많은 프롬프트가 나오지만 모두 ENTER만 눌러도 됩니다.

> **모범 사례는 이 키에 접근할 때 비밀번호를 사용하는 것입니다.**
> 그렇지 않으면 비밀 키에 접근할 수 있는 누구나 여러분 행세를 하며
> GitHub 계정과 이 키를 사용하도록 설정한 다른 모든 계정에 접근할 수
> 있습니다. 하지만 키를 쓸 때마다(명령줄에서 GitHub 관련 작업을 할
> 때마다) 비밀번호를 입력하는 것은 번거롭습니다. 그래서 사람들은
> 비밀번호를 한동안 기억하는 *키 에이전트*를 사용합니다.
>
> 키에 비밀번호를 설정하지 않는다면 컴퓨터에 저장된 키의 비밀 부분을
> 아무도 복사해 가지 못할 것이라는 사실에 의존하는 셈입니다. 자신의
> 컴퓨터가 안전하다고 확신한다면 키에 비밀번호를 둘 필요가 없습니다.
> 운이 좋을 것 같나요?
>
> 키 에이전트 설정은 이 문서의 범위를 벗어나며, 저자는 WSL에서 이것이
> 대체 어떻게 작동하는지도 확신하지 못합니다. [fl[GitHub에 이 주제에
> 관한 문서가
> 있습니다|https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent]].
>
> 이 시연에서는 비밀번호를 비워 두겠습니다. 나중에 원한다면 비밀번호가
> 있는 새 키를 만들어 이 모든 과정을 다시 할 수 있습니다.

어쨌든 모든 프롬프트에서 ENTER만 누르면 다음과 같은 결과가 나옵니다.

``` {.default}
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/user/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_ed25519
Your public key has been saved in id_ed25519.pub
The key fingerprint is:
SHA256:/lrT43BQBRPJpUXxpTBFInhdtZSQjQwxU4USwt5c0Lw user@locahost
The key's randomart image is:
+--[ED25519 256]--+
|        .o.X^%^=+|
|        ..oo*^.=o|
|        ..o = o..|
|         . + E   |
|        S .      |
|       .   o     |
|        . + +    |
|         o = .   |
|        ... .    |
+----[SHA256]-----+
```

> **키에 기본값이 아닌 다른 파일 이름을 선택했다면** GitHub에서
> 작동하도록 [fl[몇 가지 추가
> 설정|https://www.baeldung.com/linux/ssh-private-key-git-command]]을
> 해야 합니다.

> **이상한 문자가 가득한 랜덤아트라는 건 뭔가요?** 해당 키를 시각적으로
> 표현한 것입니다. 로그인할 때마다 랜덤아트를 보도록 SSH를 설정하는
> 방법도 있습니다. 어느 날 모양이 달라 보인다면 보안상 무언가 잘못됐을
> 수 있다는 발상입니다. 하지만 대부분의 사람은 랜덤아트가 생성된 뒤
> 다시 보는 일조차 없을 것 같군요.

이제 `ls ~/.ssh`를 입력하면 다음과 같은 내용이 보여야 합니다.

``` {.default}
id_ed25519    id_ed25519.pub
```

첫 번째 파일은 *비밀 키*입니다. 절대로 누구와도 공유해서는 안 됩니다.
복사할 이유조차 없습니다.

두 번째 파일은 *공개 키*입니다. 누구와든 자유롭게 공유할 수 있으며,
이 키로 로그인할 수 있도록 잠시 후 GitHub와 공유하겠습니다.

> **다음 하위 절에서 문제가 생긴다면** 다음 두 명령을 실행해 보세요.
>
> ``` {.default}
> $ chmod 700 ~/.ssh
> $ chmod 600 ~/.ssh/*
> ```
> <!-- ` -->
>
> 한 번만 하면 되지만, 해당 파일의 권한이 엄격하게 제한되어 있지 않으면
> SSH가 조금 까다롭게 굴 수 있습니다.

이제 이 방법이 작동하도록 자신의 공개 키가 무엇인지 GitHub에 알려
주어야 합니다.

먼저 공개 키를 클립보드에 복사하세요. ***반드시 `.pub` 확장자가 있는
파일을 가져와야 합니다!***

``` {.default}
$ cat ~/.ssh/id_ed25519.pub
```

다음과 같은 내용이 보일 것입니다.

``` {.default}
ssh-ed25519 AAAC3N[a bunch of letters]V+znpoO youremail@example.com
```

전체 내용을 클립보드에 복사해 두었다가 나중에 붙여 넣으세요.

이제 GitHub로 가서 오른쪽 위에 있는 아바타 아이콘을 클릭합니다.

"Settings"를 선택합니다.

그런 다음 왼쪽에서 "SSH and GPG keys"를 선택합니다.

"New SSH Key"를 클릭합니다.

제목에는 "My laptop key"처럼 알아볼 수 있는 내용을 입력합니다.

키 유형은 "Authentication Key"입니다.

그런 다음 "Key" 필드에 키를 붙여 넣습니다.

그리고 "Add SSH key"를 클릭합니다.

나중에 SSH를 사용해 URL에서 클론할 것입니다. 기억해 두세요.

### 개인 액세스 토큰 사용하기 {#using-personal-access-tokens}

지난 장에서 명령줄로 HTTPS 저장소 URL을 클론하려다가 작동하지 않는
사용자 이름과 비밀번호를 입력하라는 프롬프트가 나왔던 것을 기억하나요?

그 경우에 실제로 *작동하는* 새 비밀번호를 만들 수 있습니다. 이를
*개인 액세스 토큰*이라고 합니다.

[fl[GitHub에는 이에 관한 문서가 많이
있지만|https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens]],
요점은 "내 저장소를 읽고 쓸 수 있는 권한" 같은 특정 접근 권한을
나타내는 _토큰_을 만들고 명령줄에서 비밀번호 대신 사용한다는 것입니다.

그러면 앞에서 실패한 예제는 다음과 같은 모습이 됩니다.

``` {.default}
Username for 'https://github.com': [MY USERNAME]
Password for 'https://beejjorgensen@github.com': [MY TOKEN]
```

다시 말하면 다음과 같습니다.

1. 토큰을 생성합니다.
2. 그 토큰을 비밀번호로 사용합니다.

컴퓨터가 이 자격 증명을 자동으로 저장해 매번 입력하지 않아도 될 수
있습니다. 아닐 수도 있고요.

개인 액세스 토큰이 주는 주된 이점 중 하나는 접근 권한을 세밀하게
제어할 수 있다는 것입니다. 읽기 전용으로 제한하거나 특정 저장소에만
접근하도록 제한하는 등의 설정이 가능합니다.

또한 토큰으로 GitHub CLI 인증을 할 수도 있습니다. 표준 입력으로 토큰을
전달하기만 하면 됩니다. 토큰이 `mytoken.txt`라는 파일에 있다고 해
봅시다. 다음처럼 GitHub CLI로 인증할 수 있습니다.

``` {.default}
$ gh auth login --with-token < mytoken.txt
```

SSH 키와 마찬가지로 특정 액세스 토큰을 사용하는 노트북을 잃어버렸다면,
나쁜 사람들이 그 토큰을 쓰지 못하도록 GitHub UI에서 해당 토큰을
무효화하기만 하면 됩니다.

## 저장소의 로컬 클론 만들기 {#make-a-local-clone-of-the-repo}

[i[GitHub-->Cloning]]

저장소를 클론할 수 있도록 저장소 URL을 알아내야 합니다.

오른쪽 위의 아이콘을 클릭한 다음 "My Repositories"를 클릭하면 자신의
모든 저장소가 있는 페이지가 보일 것입니다. 지금은 `test-repo` 저장소
하나뿐일 수도 있습니다. 그 이름을 클릭하세요.

그러면 저장소 페이지로 이동합니다. 여기서 파일을 둘러보는 등 여러 일을
할 수 있지만, 우리가 정말 원하는 것은 클론 URL입니다.

커다란 파란색 "Code" 버튼을 클릭합니다.

다음에 할 일은 GitHub CLI와 SSH 키 중 어느 것을 사용하는지에 따라
달라집니다.

### GitHub CLI로 GitHub에서 클론하기 {#cloning-from-github-with-github-cli}

[i[GitHub-->Cloning with GitHub CLI]]

두 가지 선택지가 있습니다.

* **선택지 1**: 앞에서 `gh auth login`으로 인증할 때 HTTPS와 SSH 중
  무엇을 선택했는지 기억해 두라고 했습니다. 선택한 방식에 해당하는
  탭을 이 창에서 선택해야 합니다.

  URL을 복사합니다.

  명령줄로 가서 `git clone [URL]`을 실행합니다. 여기서 `[URL]`은 복사한
  내용입니다. 따라서 HTTPS의 경우 다음과 같습니다.

  ``` {.default}
  $ git clone https://github.com/user/test-repo.git
  ```

  SSH의 경우에는 다음과 같습니다.

  ``` {.default}
  $ git clone git@github.com:user/test-repo.git
  ```

* **선택지 2**: "GitHub CLI" 탭을 선택합니다. 표시된 명령을 그대로
  실행하면 다음과 비슷할 것입니다.

  ``` {.default}
  $ gh repo clone user/test-repo
  ```

### SSH 키로 GitHub에서 클론하기 {#cloning-from-github-with-ssh-keys}

[i[GitHub-->Cloning with SSH]]

앞에서 SSH 키를 설정했다면 이 방법을 사용할 수 있습니다.

초록색 "Code" 버튼을 누른 다음 "SSH" 탭이 선택되어 있는지 확인합니다.

그 URL을 복사합니다.

명령줄로 가서 `git clone [URL]`을 실행합니다. 여기서 `[URL]`은 복사한
내용입니다. 따라서 다음과 비슷할 것입니다.

``` {.default}
$ git clone git@github.com:user/test-repo.git
```

## 변경하고 푸시하기! {#make-changes-and-push}

[i[Push]]

이제 저장소를 클론했으므로 `cd`로 해당 디렉터리에 들어가 파일을
편집하고, `git add`로 스테이징 영역에 추가한 다음 `git commit -m
message`로 커밋을 만들 수 있어야 합니다...

그런 다음 `git push`로 GitHub에 있는 클론에 다시 푸시합니다!

그 뒤 GitHub의 저장소 페이지로 가서 새로 고침하면 그곳에서 변경
사항을 볼 수 있어야 합니다!

이제 그 표준적인 일반 작업 흐름으로 돌아왔습니다.

1. _원격_ 저장소를 *클론*합니다.
2. 로컬 변경 사항을 만듭니다.
3. 그 변경 사항을 *스테이징 영역*에 추가합니다.
4. 변경 사항을 *커밋*합니다.
5. 변경 사항을 원격 저장소로 다시 *푸시*합니다.
6. 2단계로 돌아갑니다.

## GitHub에서 협업하기 {#collaboration-on-github}

두 가지 주요 방법이 있습니다.

1. 포크/풀 리퀘스트
2. 협업자 추가

첫 번째 방법은 나중에 이야기하겠습니다.

[i[GitHub-->Adding collaborators]]
지금으로서는 협업자를 추가하는 가장 쉬운 방법은 자신의 저장소에 바로
추가하는 것입니다.

GitHub 저장소 페이지에서 "Settings"를 선택한 다음 왼쪽의
"Collaborators"를 선택합니다.

인증을 마치면 "Add people"을 클릭할 수 있습니다. 함께 작업하고 싶은
사람의 사용자 이름을 입력하세요.

상대방이 GitHub 받은 편지함에서 초대를 수락해야 하지만, 수락하고 나면
저장소에 접근할 수 있습니다.

반드시 신뢰하는 사람에게만 이렇게 하세요!

[i[GitHub]>]
