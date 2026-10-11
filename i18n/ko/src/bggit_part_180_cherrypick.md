# 체리픽: 특정 커밋 가져오기 {#cherry-pick-bringing-in-specific-commits}

[i[Cherry-pick]<]

`branch1`에서 작업 중이고 `branch2`에서 버그를 수정했다고 합시다.
`branch2`의 변경 사항을 _모두_ `branch1`에 병합할 준비는 되지 않았지만,
그 버그 수정만큼은 꼭 가져오고 싶습니다.

다행히 방법이 있습니다! `git cherry-pick`을 사용하면 커밋 하나만 내 브랜치에
병합할 수 있습니다. 어떤 커밋을 가져올지만 알려 주면 됩니다.

## 체리픽 예시 {#cherry-pick-example}

`main` 브랜치에 `foo.txt`라는 파일이 있고, 초기 커밋에 다음 내용이 저장돼
있다고 합시다.

``` {.default}
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
```

참으로 흥미진진한 파일입니다.

이제 다른 브랜치로 전환합시다. 영감 넘치게도 이름은 `branch`라고 하겠습니다.

이 브랜치에서 몇 가지 작업을 합니다. 먼저 끝에 두 줄을 추가하고 커밋합니다.

``` {.default}
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
Branch: Line 101
Branch: Line 102
```

그런 다음 중간에 한 줄을 추가하고 다시 커밋합니다.

``` {.default}
Line 1
Line 2
Line 3
Line 4
BRANCH: INSERTED LINE 5
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
Branch: Line 101
Branch: Line 102
```

**추가로** 데모를 조금 쉽게 만들기 위해 여기서 `checkpoint`라는 브랜치를
만듭시다. 꼭 해야 하는 일은 아니지만, 커밋 해시 대신 브랜치 이름으로 이
커밋을 체리픽할 수 있게 됩니다. 이 단계를 건너뛰고 해시를 사용해도 됩니다.

``` {.default}
$ git branch checkpoint
```

> **이 명령은 브랜치를 전환하지 않습니다.** 현재 커밋에 새 브랜치만 만듭니다.
> `HEAD`는 이전과 마찬가지로 여전히 `branch`를 가리킵니다.

마지막으로 끝에 몇 줄을 더 추가하고 한 번 더 커밋합시다.

이제 `branch`에 있는 파일은 다음과 같습니다.

``` {.default}
Line 1
Line 2
Line 3
Line 4
BRANCH: INSERTED LINE 5
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
Branch: Line 101
Branch: Line 102
Branch: Line 103
Branch: Line 104
```

로그를 살펴보고 무엇이 있는지 확인합시다.

``` {.default}
commit 9336292f73b4ace717644336f72458681c1bb761 (HEAD -> branch)
Author: Branch User Name <branch-user@example.com>
Date:   Sun Oct 20 13:08:30 2024 -0700

    branch: added line 103-104

commit 407f212f12f79902818431a174706cfdc30d509b (checkpoint)
Author: Branch User Name <branch-user@example.com>
Date:   Sun Oct 20 13:08:30 2024 -0700

    branch: inserted line 5

commit 9533e0bdd5cba7d65401c3180b34b01700a7906e
Author: Branch User Name <branch-user@example.com>
Date:   Sun Oct 20 13:08:30 2024 -0700

    branch: added line 101-102

commit d6953bd746c813f5ba545cf0fd6044fd78e2c617 (main)
Author: User Name <user@example.com>
Date:   Sun Oct 20 13:08:30 2024 -0700

    added
```

좋습니다. 데모 준비가 끝났습니다. 이제 체리픽할 시간입니다!

이 데모에서는 `main`으로 돌아간 다음 중간에 5번 줄을 삽입한 커밋 하나만
체리픽하겠습니다. 언제든 커밋 해시(`407f2`)를 사용할 수 있지만, 대신 쓸 수
있도록 `checkpoint` 브랜치를 남겨 뒀습니다.

해 봅시다.

``` {.default}
$ git switch main
  Switched to branch 'main'

$ git cherry-pick checkpoint
  Auto-merging foo.txt
  [main 9254663] branch: inserted line 5
   Date: Sun Oct 20 13:08:30 2024 -0700
   1 file changed, 1 insertion(+)
```

이 명령은 새로 삽입된 5번 줄만 가져오고 다른 변경 사항은 하나도 가져오지
*않았어야* 합니다. `main`의 `foo.txt`를 살펴봅시다.

``` {.default}
Line 1
Line 2
Line 3
Line 4
BRANCH: INSERTED LINE 5
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
```

바로 저기 있군요!

> **잠깐, 이건 그냥 병합 아닌가요?** 꼭 그렇지는 않습니다! `branch`에서
> 5번 줄을 삽입하기 *전에* 101~102번 줄을 추가했다는 점에 주목하세요. 그런데
> 그 앞선 커밋은 `main`에 반영되지 않았습니다. 앞뒤의 다른 커밋은 무시하고,
> 커밋 흐름에서 5번 줄이 든 커밋 하나만 *체리픽*한 것입니다!

이제 `main`에서 `git log`를 살펴봅시다.

``` {.default}
commit 92546636d05fa85218ca18a0cd705ddc14fa8b64 (HEAD -> main)
Author: Branch User Name <branch-user@example.com>
Date:   Sun Oct 20 13:08:30 2024 -0700

    branch: inserted line 5

commit d6953bd746c813f5ba545cf0fd6044fd78e2c617
Author: User Name <user@example.com>
Date:   Sun Oct 20 13:08:30 2024 -0700

    added
```

여기서 눈여겨볼 놀라운 점이 두 가지 있습니다.

1. 로그 메시지에 작성자 정보가 보존됐습니다. 체리픽을 실행한 사람은 후자지만
   `user@example.com`이 아니라 `branch-user@example.com`이라는 점에
   주목하세요. 아주 놀라운 일은 아닐 수도 있지만...

2. 체리픽한 커밋의 해시는 `main`과 `branch`에서 서로 다릅니다! `branch`에서는
   `407f2`이고 여기서는 `92546`입니다. 내용이 완전히 새로우므로 **반드시**
   그래야 합니다. 즉, 커밋 그래프 어디에도 `foo.txt`가 이런 모습인 다른
   커밋이 없으므로 고유한 커밋 해시를 가져야 합니다[^5472].

[^5472]: 변경 사항이 같더라도 해시는 온갖 다른 메타데이터까지 고려하므로
    커밋 해시는 여전히 달라집니다.

하지만 모든 체리픽이 이렇게 순조롭지는 않습니다!

## 체리픽 충돌 {#cherry-pick-conflicts}

[i[Cherry-pick-->Conflicts]<]

물론 체리픽에서도 충돌이 생길 수 있습니다. 체리픽하려는 커밋과 같은 줄을
일부 변경했거나, 체리픽한 커밋에는 있지만 현재 브랜치에는 없는 주변 코드 줄이
있기 때문일 수 있습니다.

어떤 경우든 충돌 해결은 `merge`나 `rebase` 때와 거의 같은 방식으로
진행됩니다. 필요하다면 해당 장의 내용을 다시 익히세요.

하지만 이제 이 과정이 익숙하게 느껴지길 바랍니다. 먼저 파일을 _올바르게_
만든 다음 추가하고, `git cherry-pick --continue`로 (`rebase` 때처럼)
_계속_ 진행합니다. 모든 것이 깔끔하게 병합될 때까지 이 과정을 반복하세요.

[i[Cherry-pick-->Conflicts]>]

[i[Cherry-pick]>]
