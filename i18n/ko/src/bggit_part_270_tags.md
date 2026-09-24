# 태그 {#tags}

[i[Tag]<]

태그는 특정 커밋에 주석을 붙이는 방법입니다. 움직이지 않는 브랜치 같은 것이라고 생각해도 좋습니다.

아주 흔한 사례는 특정 커밋에 `1.2.3` 같은 버전 번호를 붙이는 것입니다.

태그 이름에 어떤 문자를 쓸 수 있는지 규칙을 찾지는 못했지만, ASCII 대문자와 소문자, 숫자, 그리고 `.`, `-`, `_` 같은 문장 부호는 안전해 보입니다.

태그에는 두 종류가 있습니다.

* ***경량(lightweight)***: `v3.14` 같은 단순한 태그입니다.
* ***주석(annotation) 태그***: 역시 `v3.14` 같은 태그이지만 커밋처럼 메시지와 작성자를 포함합니다.

태그는 움직이지 않는다는 점을 제외하면 일반적으로 브랜치와 같은 방식으로 사용할 수 있습니다(diff를 보거나, 해당 태그로 전환하는 등).

[i[Tag-->Listing]<]

로그에서도 다른 브랜치 정보와 함께 볼 수 있습니다.

``` {.default}
commit 4fa1199d17a97990a7721eb8a73a4ee50 (HEAD -> main, tag: v3.14)
Author: User <user@example.com>
Date:   Sat Jan 25 19:04:43 2025 -0800

    Update the stuff

commit c265e0371b3fc11588e4183b35e0b96e3 (tag: v3.10)
Author: User <user@example.com>
Date:   Sat Jan 25 18:59:58 2025 -0800

    Add the stuff
```

또는 `git tag`만 실행해 모든 태그 목록을 볼 수 있습니다.

[i[Tag-->Listing]>]

## 경량 태그 {#lightweight-tags}

[i[Tag-->Creating]<]
[i[Tag-->Lightweight]<]

경량 태그를 추가하는 일은 간단합니다. 먼저 태그를 붙일 커밋으로 전환한 다음, 태그 이름을 `tagname`으로 짓는다고 가정하고 다음 명령을 실행합니다.

``` {.default}
$ git tag tagname       # Tag HEAD commit with tagname
```

커밋이나 커밋을 가리키는 무엇에든(브랜치나 다른 태그에도) 태그를 붙일 수 있습니다.

``` {.default}
$ git tag CPE1704TKS 4fa12    # tag commit 4fa12
$ git tag plover feature99    # tag branch feature99
```

[i[Tag-->Lightweight]>]

## 주석 태그 {#annotated-tags}

[i[Tag-->Annotated]<]

주석 태그도 거의 똑같이 쉽게 추가할 수 있습니다. "annotate"를 뜻하는 `-a` 옵션을 붙이세요.

주석 태그는 커밋과 더 비슷하므로 메시지를 입력하라는 프롬프트가 나타납니다. `-m`을 사용해 명령줄에서 지정할 수도 있습니다.

``` {.default}
$ git tag -a v3490     # tag HEAD commit with v3490
```

메시지를 명령줄에서 지정하려면 다음과 같이 합니다.

``` {.default}
$ git tag -a v3490 -m "tag message"
```

[i[Tag-->Annotated]>]
[i[Tag-->Creating]>]

## 태그 푸시하기 {#pushing-tags}

[i[Tag-->Pushing]<]

일반적인 푸시를 하더라도 기본적으로 태그는 로컬 저장소에만 존재합니다. 태그를 푸시하고 싶다고 명시적으로 알려 주어야 합니다.

[i[Push-->Tags]]
새 태그를 모두 푸시하려면 다음과 같이 합니다.

``` {.default}
$ git push --tags
$ git push origin --tags  # Or you can specify a remote
```

태그 하나만 푸시하려면 원격 저장소를 지정해야 합니다.

``` {.default}
$ git push origin tag3.14
```

태그가 푸시된 뒤에는 다른 협업자가 풀할 때 태그도 자동으로 받습니다.

[i[Tag-->Pushing]>]

## 태그 삭제하기 {#deleting-tags}

[i[Tag-->Deleting]<]

저장소의 태그는 다음과 같이 삭제할 수 있습니다.

``` {.default}
$ git tag -d tagname
```

충분히 쉽습니다. 다만 이미 서버에 푸시했다면 이야기가 다릅니다. 그런 경우 다음번에 풀할 때 태그를 다시 받게 됩니다.

따라서 원격 저장소의 태그도 삭제해야 하며, 원격 저장소의 이름도 명시해야 합니다.

[i[Push-->Tags]]

``` {.default}
$ git push origin -d tagname
```

이 명령은 서버의 태그를 삭제하지만 *다른 사람의 클론에서는 삭제하지 않습니다*. 사실 이를 쉽게 해낼 방법은 없습니다.

기본 원칙은 태그를 한 번 만들었으면 삭제하지 않아야 한다는 것입니다. 아직 푸시하지 않았다면 문제없습니다. 원하는 만큼 추가하고 삭제하고 바꾸세요. 하지만 일단 푸시했고 누군가 풀했다면, 태그를 바꿔야 할 때 새 태그를 만드세요.

법칙까지는 아니며 권장 지침에 가깝습니다.

[i[Tag-->Deleting]>]
[i[Tag]>]
