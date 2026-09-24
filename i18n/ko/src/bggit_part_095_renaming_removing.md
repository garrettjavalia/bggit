# 파일 이름 변경과 삭제 {#renaming-and-removing-files}

이 장은 [파일 상태](#file-states)를 다루는 내용의 연장이므로 먼저 그 장을 꼭
읽으세요!

또한 여기서는 *이름 변경*과 *이동*이라는 용어를 같은 뜻으로 섞어 쓰겠습니다.
이동은 이름을 바꿀 뿐 아니라 파일을 다른 디렉터리로 옮길 수도 있으므로 개념상
조금 더 강력합니다. 이름 변경 명령이 `git mv`라는 점에서 알아둘 만합니다.

## 파일 이름 변경하기 {#renaming-files}

[i[Move]<]

운영 체제의 이름 변경 명령으로 파일 이름을 바꿀 수도 있지만, Git 저장소 안의
파일이라면 Git이 상황을 완전히 파악하도록 `git mv`를 사용하는 편이 좋습니다.

Git은 이 부분에서 혼란스러울 만큼 불친절합니다.

`foo.txt`의 이름을 `bar.txt`로 바꾸고 상태를 확인해 봅시다.

``` {.default}
$ git mv foo.txt bar.txt
  On branch main
  Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
	  renamed:    foo.txt -> bar.txt
```

Git은 파일 이름이 바뀌었다는 것을 알고 있으며, 파일은 스테이징 영역으로
이동했습니다. 다음과 같습니다.

* **수정되지 않음** → `git mv foo.txt bar.txt` → **스테이징됨** ("이름 변경됨"으로)

살펴보면 디렉터리에서도 파일 이름이 실제로 `bar.txt`로 바뀌었습니다.

이 시점에 커밋하면 저장소에서 파일 이름이 변경됩니다. 끝입니다.

[i[Move-->Reverting]]
하지만 이름 변경을 취소하고 싶다면 어떻게 할까요?

Git은 구원투수로 `git restore --staged`를 제안합니다... 하지만 이전 이름과 새
이름 중 어느 것을 써야 할까요? 그다음에는요? 알고 보면 `git restore` 뒤에
다른 명령 여러 개를 실행해 취소할 *수는* 있지만, 이 경우에는 Git의 조언을
무시하고 다음 절을 읽어야 합니다.

## 스테이징 영역에서 파일 이름 변경 취소하기 {#unrenaming-files-from-the-stage}

[i[Move-->Unstaging]<]
이것만 기억하세요. **스테이징된 이름 변경을 취소하는 가장 쉬운 방법은 반대로
이름을 다시 바꾸는 것입니다.**

이름을 변경해 다음 상태가 됐다고 합시다.

``` {.default}
$ git mv foo.txt bar.txt    # Rename foo.txt to bar.txt
$ git status
  On branch main
  Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
	  renamed:    foo.txt -> bar.txt
```

이 변경을 되돌리는 가장 쉬운 방법은 다음과 같습니다.

``` {.default}
$ git mv bar.txt foo.txt    # Rename it back to foo.txt
$ git status
  On branch main
  nothing to commit, working tree clean
```

됐습니다.

정리하면 파일 이름을 바꾸는 방법은 다음과 같습니다.

* **수정되지 않음** → `git mv foo.txt bar.txt` → **스테이징됨**
* **스테이징됨** → `git commit` → **수정되지 않음**

스테이징된 이름 변경을 취소하려면 원래 이름으로 다시 바꿉니다.

* **스테이징됨** → `git mv bar.txt foo.txt` → **수정되지 않음**

[i[Move-->Unstaging]>]
[i[Move]>]

## 파일 삭제하기 {#removing-files}

[i[Remove]<]

운영 체제의 삭제 명령으로 파일을 지울 수도 있지만, Git 저장소 안의 파일이라면
Git이 상황을 완전히 파악하도록 `git rm`을 사용하는 편이 좋습니다.

그리고 일어나는 일이 조금 이상하게 느껴질 수 있습니다.

이미 커밋한 `foo.txt` 파일이 있는데 삭제하기로 했다고 합시다.

``` {.default}
$ git rm foo.txt
  rm 'foo.txt'         # This is Git's output
```

이 명령은 파일을 실제로 삭제합니다. 디렉터리를 살펴보면 사라져 있습니다.

하지만 상태를 확인해 봅시다.

``` {.default}
$ git status
  On branch main
  Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
	  deleted:    foo.txt
```

말하자면 이제 삭제된 파일이 스테이징됨 상태에 있습니다. 파일이 사라진 작업
트리와 파일이 여전히 존재하는 스테이징 영역 사이에 "차이"가 생겼으니 말이
됩니다.

여기서 커밋하면 파일이 삭제됩니다. 끝입니다.

## 스테이징 영역에서 파일 삭제 취소하기 {#unremoving-files-from-the-stage}

[i[Remove-->Unstaging]<]
그런데 방금 삭제한 파일의 스테이징을 취소하고 싶다면 어떻게 할까요? 늘 그렇듯
`git restore --staged`로 되찾는 방법이 힌트에 나와 있습니다.

해 봅시다.

``` {.default}
$ git restore --staged foo.txt
$ git status
  On branch main
  Changes not staged for commit:
    (use "git add/rm <file>..." to update what will be committed)
    (use "git restore <file>..." to discard changes in working
    directory)
	  deleted:    foo.txt

  no changes added to commit (use "git add" and/or "git commit -a")
```

흠. "커밋하도록 스테이징되지 않은 변경 사항"은 수정됨 상태의 파일입니다. 즉,
`foo.txt`가 "수정됐다"는 뜻인데 이 맥락에서는 "삭제됐다"를 조금 더 친절하게
말한 것입니다.

스테이징됨 상태에서 수정됨 상태로 한 걸음 물러났습니다. 하지만 살펴보면 파일은
여전히 사라진 상태입니다! 제 파일을 돌려주세요!

파일을 수정되지 않음 상태로 되돌리고 싶습니다. Git은 다시 한번 상태 출력에서
그 방법인 `git restore`를 알려 줍니다. 해 봅시다.

``` {.default}
$ git restore foo.txt
$ git status
  On branch main
  nothing to commit, working tree clean
```

Git은 수정된 파일이 없다고 알려 줍니다. 직접 살펴봅시다.

``` {.default}
$ ls foo.txt
  foo.txt
```

파일이 무사히 돌아왔습니다.

따라서 커밋된 파일을 삭제하는 과정은 이미 살펴본 과정의 변형입니다.

* **수정되지 않음** → `git rm foo.txt` → **스테이징됨**
* **스테이징됨** → `git commit` → 이제 파일이 사라짐

삭제를 아직 커밋하지 않았다면 다른 파일 상태를 취소하는 것과 같은 방식으로
파일 삭제를 취소할 수 있습니다.

* **스테이징됨** → `git restore --staged foo.txt` → **수정됨**
* **수정됨** → `git restore foo.txt` → **수정되지 않음**

[i[Remove-->Unstaging]>]

## 이전 커밋에서 삭제된 파일 되살리기 {#unremoving-files-from-earlier-commits}

[i[Remove-->Unremoving from earlier commits]<]
얼마 전에 삭제한 파일을 되찾고 싶다면 어떻게 할까요?

1. 파일이 존재하는 커밋의 해시를 찾습니다.
2. 그 커밋에서 파일을 복원합니다.

아마 파일 이름을 알고 있겠지만 모른다면 찾을 때까지 `git log --name-only`로
로그를 고생스럽게 샅샅이 살펴봐야 합니다.

하지만 이름을 안다고 합시다. 이 경우 삭제된 파일의 로그를 다음과 같이 볼 수
있습니다(원하는 파일이 `foo.txt`라고 가정합니다).

``` {.default}
$ git log -- foo.txt
  commit 97bdb61727f7515d6953c965f56ef8329585f348
  Author: User <user@example.com>
  Date:   Sun Jan 12 11:08:33 2025 -0800

      Removed due to horrid commit messages

  commit 1c9bf4514ee90a0e65fb9b0a916765bb6c78dee6
  Author: User <user@example.com>
  Date:   Sun Jan 12 11:08:33 2025 -0800

      Add the barfsplant

  commit cc7a1940f13fca9092dbe9ce4a8e9012babd9314
  Author: User <user@example.com>
  Date:   Sun Jan 12 11:08:33 2025 -0800

      Initial splungification
```

명망 높은 `foo.txt`의 역사가 보입니다.

커밋 `97bdb`에서 삭제된 것을 볼 수 있습니다. 그러니 그 커밋에서 복원해 봐야
소용없습니다(그때는 이미 사라졌으니까요!). 하지만 그 _바로 전_ 커밋인
`1c9bf`에는 삭제 전 `foo.txt`의 가장 최신 버전이 있습니다. 아마 이것이 원하는
파일일 것입니다. (시간을 더 거슬러 올라가고 싶다면 더 이전 버전을 골라도
됩니다. 법으로 금지된 일은 아니니까요.)

복원은 상당히 쉽습니다.

``` {.default}
$ git restore --source=1c9bf foo.txt
$ ls foo.txt
  foo.txt
```

파일이 돌아왔습니다! 그런데 어떤 상태일까요?

``` {.default}
$ git status
  On branch main
  Untracked files:
    (use "git add <file>..." to include in what will be committed)
	  foo.txt
```

아직 추가조차 되지 않았습니다. 다시 살려 내고 싶다면 완전히 새 파일인 것처럼
추가하고 커밋해야 합니다.

[i[Remove-->Unremoving from earlier commits]>]

## 비밀 정보 삭제에 관한 참고 사항 {#a-note-on-removing-secrets}

[i[Remove-->Secrets]<]

무언가를 커밋한 뒤 삭제해도 다시 되살릴 수 있다는 것을 살펴봤습니다.

이제 다음과 같은 코드를 커밋했다고 합시다.

``` {.default}
MASTER_PASSWORD_FOR_THE_ENTIRE_COMPANY=pencil
```

그리고 이 코드를 push하는 끔찍한 실수를 저지릅니다.

그것만으로도 충분히 나쁘지만, 설상가상으로 공개된 GitHub 저장소에 push했다고
합시다.

이제 전 세계가 여러분의 비밀번호를 갖게 됐습니다! 완전히 망했습니다!

"하지만 잠깐만요! 재빨리 삭제하고 push하면 아무도 눈치채지 못할 거예요!"

안 됩니다. 회사는 그런 위험을 감수할 수 없습니다. 누구든 저장소를 clone하고
기록을 거슬러 올라가 삭제된 파일을 얻을 수 있습니다. 유일한 대책은 그
비밀번호를 즉시 바꾸는 것입니다. 회사 전체에서요. **반드시** 그렇게 해야
합니다. 관리자는 전혀 즐겁지 않습니다.

> **절대로, 정말 절대로 비밀 정보를 Git에 커밋하지 마세요.** 닷 env[^e749]
> 파일이나, 비밀 정보를 커밋하는 것만 아니라면 문자 그대로 무엇이든 사용하세요.

[^e749]: 닷 env(`.env`) 파일은 로그인 인증 정보 같은 비밀 정보를 담으며
    저장소에는 절대로 커밋하지 않는 파일입니다. 확실히 하기 위해 보통
    `.gitignore`에 넣습니다. 프로젝트 관계자가 별도 통신 경로를 통해 파일에
    무엇을 넣어야 하는지 알려 주므로 올바르게 인증할 수 있습니다. 다양한
    언어와 프레임워크가 `.env` 파일을 지원합니다. 심각한 문제이므로 저도 지금
    진지한 목소리로 말하고 있습니다!

위반의 심각성을 조금 낮춰 봅시다. GitHub에 push했지만 비공개 저장소였다고
합시다. 그래도 여전히 좀 나쁩니다. 접근 권한이 있는 모든 사람을 믿어야 하고,
저장소의 clone이 회사 외부 사람이나 불만을 품은 직원의 손에 절대 들어가지
않으리라고 믿어야 합니다. 유일한 대책은 그 비밀번호를 바꾸는 것입니다.

좋습니다. 심각성을 더욱 *낮춰* 봅시다. 저장소에 비밀번호를 커밋했지만 *아직
push하지 않았다*고 합시다.

이제는 여러분 외에 다른 사람이 코드를 봤을 가능성이 없으므로 무언가 조치할 수
있습니다. push하지 않았으니 아무도 pull할 수 없었습니다. 하지만 여기서는 그
방법을 다루지 않겠습니다. push하기 전에 파일을 고치는 방법은 [커밋
수정하기](#amend) 장을 보세요.

[i[Remove-->Secrets]>]

[i[Remove]>]
