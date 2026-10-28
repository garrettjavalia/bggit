# 신원 변경하기 {#changing-identity}

[i[Identity]<]

Git으로 작업할 때 사용자를 식별하는 방법은 몇 가지가 있습니다.

그 신원 정보는 다음 항목에 담깁니다.

* `user.name`과 `user.email` 구성 변수
* GitHub 같은 원격 서버에서 인증할 때 사용하는 SSH 키
* 커밋 서명에 사용하는 GPG 키(드물게 사용)

언제나 신원 하나만 사용한다면 아무 문제도 없지만, 때로는 서로 다른 신원을
쓰고 싶을 수 있습니다. 예를 들어 개인적인 재미로 하는 작업에는 한 신원과
SSH 키를 사용하다가 계약직 일을 맡게 되어 업무용 이메일을 쓰고, 다른 SSH
키로 다른 서버에 연결해야 할 수도 있습니다.

이 모든 항목의 기본값이 무엇인지, 그리고 저장소별로 어떻게 바꾸는지
살펴봅시다.

## 사용자 구성 변수 변경하기 {#changing-the-user-configuration-variables}

[i[Configuration-->Name and email]]

업무용 저장소와 개인용 저장소가 따로 있거나, 저장소마다 사용하고 싶은 업무용
또는 개인용 이메일이 여러 개라면 이렇게 설정하고 싶을 것입니다.

> **저를 포함해 누구나 회사에서 지급한 노트북을 개인 용도로 쓰면 안 된다고
> 말할 것입니다.** 특히 그 "개인 용도"가 다른 유료 업무라면 정말 더더욱
> 그러면 안 됩니다. 다만 계약자는 흔히 여러 일을 동시에 진행하고(하드웨어도
> 본인 소유입니다), 개인적으로 재미 삼아 작업하는 사람도 프로젝트마다 다른
> 이메일을 쓰고 싶을 때가 있습니다.

구성 장에서 이미 다뤘지만, 각 커밋에 붙는 로컬 신원은 쉽게 바꿀 수 있습니다.
`user.name`과 `user.email`을 원하는 값으로 변경하기만 하면 됩니다.

해당 저장소에서 로컬 구성을 설정해 전역 구성을 덮어쓰세요.

``` {.default}
$ git config set user.name "My Alter Ego User Name"
$ git config set user.email "alterego@example.com"
```

그러면 이 저장소에서 커밋을 만들 때 그 신원이 커밋에 연결됩니다. 다른
저장소의 커밋은 여전히 전역 사용자 이름과 이메일을 따릅니다(그 저장소에서도
따로 덮어쓰지 않았다면 말이죠).

## SSH 인증 키 변경하기 {#changing-the-ssh-authentication-key}

[i[Configuration-->SSH identity]<]

누군가의 비공개 원격 저장소(예를 들어 직접 운영하는 Gitea 사이트 등)에
연결하면서, 그 사이트에 접근할 때만 다른 SSH 키를 사용해야 한다면 이 설정이
필요합니다. 하지만 개인 GitHub에는 기존 SSH 키를 계속 쓰고 싶습니다. 모든
저장소에는 GitHub용 키를 쓰면서 이 저장소 하나에만 다른 SSH 키를 쓰려면
어떻게 해야 할까요?

이 작업은 조금 더 복잡하고 [fl[방법도 몇 가지가
있지만|https://superuser.com/questions/232373/how-to-tell-git-which-private-key-to-use]],
여기서는 제가 가장 좋아하는 방법을 소개하겠습니다.

먼저 배경부터 살펴봅시다. `ssh` 명령은 실행될 때 어떤 신원으로 실행되는지
알아야 합니다. 다른 신원을 지정하지 않으면 기본 신원을 사용하며, 이 신원은
`id_ed25519`처럼 `~/.ssh/id_something`이라는 파일에 들어 있습니다.

`ssh`의 `-i` 스위치를 사용해 명령줄에서 다른 신원을 지정할 수 있습니다.

`.ssh` 디렉터리에 `id_ed25519`와 `id_alterego_ed25519`라는 비밀 키 두 개가
있다고 합시다. 첫 번째 키가 SSH의 기본 키입니다. 다른 키를 쓰고 싶다면
다음처럼 지정할 수 있습니다.

``` {.default}
$ ssh -i ~/.ssh/id_alterego_ed25519 example.com
```

솔직히 매번 입력하기에는 귀찮은 명령이라, 어떤 사람들은 특정 호스트 이름에
특정 키를 쓰도록 SSH 구성을 설정합니다. 하지만 여기서는 그 방법을 택하지
않겠습니다.

대신 이 저장소의 `core.sshCommand` 변수를 로컬로 설정해 Git에 특정 신원을
사용하라고 알려 줍시다. 이 변수에는 Git이 연결할 때 사용하는 SSH 명령이
들어 있으며, 보통은 `ssh`입니다. 다음과 같이 덮어씁니다.

``` {.default}
$ git config set core.sshCommand \
    "ssh -i ~/.ssh/id_alterego_ed25519 -F none"
```

(위 명령은 책 여백에 맞추려고 두 줄로 나눴습니다. 보통은 한 줄이며, `\`는
Bash의 줄 연속 문자입니다.)

그런데 잠깐만요. 저기 있는 `-F none`은 무엇일까요? SSH에 기본 구성 파일을
무시하라고 지시하는 안전장치일 뿐입니다. 앞에서 어떤 사람들은 SSH 구성에서
도메인별 신원을 설정한다고 했던 것을 기억하나요? 여기서 하려는 일이 바로
덮어쓰기이므로, 이 옵션이 그런 설정을 덮어씁니다.

제가 이 방법을 좋아하는 이유는 저장소별로 쉽게 설정할 수 있고, 구성이 환경
변수나 다소 관련이 먼 SSH 구성 대신 저장소와 함께 저장되기 때문입니다.

[i[Configuration-->SSH identity]>]

## GPG 서명 키 변경하기 {#gpg-signing}

[i[Configuration-->GPG signing key]<]

서명에 [fl[GPG 키|https://www.gnupg.org/]]를 사용한다면 그 키의 지문을
알아내어 사용할 키를 지정할 수 있습니다(GPG가 인식하는 이메일이나 다른 고유
식별자도 아마 사용할 수 있습니다).

죄송하지만 GPG 키 쌍을 설정하는 방법은 이 책의 범위를 벗어납니다. 다만
Git 쪽에서 한 번 해 둘 설정은 다음과 같습니다.

``` {.default}
$ git config gpg.format gpg
$ git config commit.gpgsign true
```

먼저 사용하려는 비밀 키를 찾습니다.

``` {.default}
$ gpg --list-secret-keys --keyid-format LONG
  /user/.gnupg/pubring.kbx
  ------------------------------
  sec   rsa4096/0123456789ABCDEF 2022-01-01 [SC] [expires: 2025-01-
        9993456789ABCDEF0123456789ABCDEF01234567
  uid     [ultimate] Personal User Name <personal@example.com>
  ssb   rsa4096/9993456789ABCDEF 2022-01-01 [E] [expires: 2025-01-0
  sec   ed25519/ABCDEF0123456789 2022-01-01 [SC] [expires: 2025-01-
        FFFDEF0123456789ABCDEF0123456789ABCDEF01
  uid     [ultimate] Professional User Name <professional@example.c
  ssb   rsa4096/FFFDEF0123456789 2022-12-06 [E] [expires: 2024-12-0
```

(책에 들어가도록 출력의 오른쪽을 잘랐습니다.)

사용하려는 신원을 찾으세요. 여기서는 "Professional User Name"을 사용한다고
합시다. 이 신원과 연결된 `sec` 줄(신원 위쪽)을 찾고, 비밀 키 줄에서 암호화
유형(보통 `rsa4096` 또는 `ed25519`) 뒤에 있는 부분을 복사합니다. 이 가상
예시에서는 `ABCDEF0123456789`입니다.

그런 다음 이 저장소가 그 키를 사용하도록 로컬 구성을 설정합니다.

``` {.default}
git config set user.signingkey ABCDEF0123456789
```

이후 커밋에 서명하면 이 키가 사용됩니다.

[i[Configuration-->GPG signing key]>]

## SSH 서명 키 변경하기 {#changing-your-ssh-signing-key}

[i[Configuration-->SSH signing key]<]

GPG뿐 아니라 SSH 키로도 커밋에 서명할 수 있습니다. `ssh-keygen`으로 서명
키를 만들 수 있습니다.

``` {.default}
$ ssh-keygen -t ed25519
```

아직 하지 않았다면 일회성 구성을 몇 가지 해야 합니다. 모든 저장소에 이
설정을 적용하려면 `--global`을 사용하세요. 다음 명령은 Git에 SSH 키를
사용하고 항상 커밋에 서명하라고 지시합니다.

``` {.default}
$ git config gpg.format ssh
$ git config commit.gpgsign true
``` 

이제 서명에 사용할 키를 설정할 수 있습니다. 아래 경로를 공개 키를 가리키도록
바꾸세요.

``` {.default}
$ git config user.signingkey ~/.ssh/id_ed25519_signing.pub
``` 

마지막으로 `allowed_signers` 파일에 사용자 정보를 추가해야 합니다. 이 파일은
어디에 있어도 됩니다. 이 예시에서는 `~/.ssh/`에 두지만, 원한다면 저장소마다
별도로 둘 수도 있습니다.

이 마지막 단계에서 맨 먼저 할 일은 `allowed_signers` 파일의 위치를 Git에
알려 주는 것입니다.

``` {.default}
$ git config gpg.ssh.allowedSignersFile "~/.ssh/allowed_signers"
```

이 파일의 내용에는 적어도 두 필드가 있어야 합니다. 첫째는 커밋에 사용할
`user.email` 구성 변수의 이메일 주소입니다. 둘째는 `user.signingkey` 변수에
지정한 공개 키의 사본입니다. 파일 이름이 아니라 그 파일의 *내용*이
필요하다는 점에 유의하세요.

`allowed_signers` 파일의 한 줄은 다음과 같습니다(서식을 맞추려고 줄을
잘랐습니다).

``` {.default}
user@example.com ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAmaTS47vRmsKy
```

여러 신원을 쓰려면 이 파일에 여러 줄을 넣을 수 있습니다.

[i[Configuration-->SSH signing key]>]

[i[Identity]>]
