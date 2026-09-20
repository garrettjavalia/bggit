# `.gitignore`로 파일 무시하기 {#ignoring-files-with-.gitignore}

[i[`.gitignore` file]<]

하위 디렉터리에 Git이 조금도 신경 쓰지 않았으면 하는 파일이 있다면 어떨까요?
예를 들어 저장소에서 보고 싶지 않은 임시 파일이 있을 수 있습니다. 또는 C
프로젝트에서 빌드한 실행 파일이 있는데, 엄청나게 엄격한 강사가 저장소에 빌드
결과물이 하나라도 있으면 프로젝트를 채점하지 않아서 체크인하고 싶지 않을 수도
있습니다. 어디까지나 예를 들자면요.

이 안내서의 이 부분에서는 바로 그 문제를 다룹니다.

## `.gitignore` 파일 추가하기 {#adding-a-.gitignore-file}

Git 저장소의 어느 디렉터리에든 `.gitignore`("닷 깃이그노어") 파일을 추가할 수
있습니다.

무시할 파일 이름 목록이 들어 있는 간단한 텍스트 파일입니다.

"doom"이라는 실행 파일을 빌드하는 C 프로젝트가 있다고 합시다. 이 파일은 소스가
아니고 디스크 공간만 많이 차지하는 커다란 바이너리이므로 소스 저장소에
체크인하고 싶지 않습니다.

하지만 상태를 확인할 때마다 Git이 이 파일을 두고 불평하는 모습은 성가십니다.

``` {.default}
$ git status
  On branch main
  Untracked files:
    (use "git add <file>..." to include in what will be committed)
	  doom

  nothing added to commit but untracked files present (use "git
  add" to track)
```

그래서 그 디렉터리의 `.gitignore` 파일을 편집해 다음 한 줄을 추가합니다.

``` {.default}
doom
```

이제 상태를 다시 확인합니다.

``` {.default}
$ git status
  On branch main
  Untracked files:
    (use "git add <file>..." to include in what will be committed)
	  .gitignore

  nothing added to commit but untracked files present (use "git
  add" to track)
```

뭐죠? 똑같다고요? 꼭 그렇지는 않습니다! 작은 글씨를 읽어 보세요!

전에는 `doom`이 추적되지 않는다고 불평했지만 이제는 불평하지 않습니다.
`.gitignore`가 작동한 것입니다. 야호!

하지만 Git은 새로 만든 `.gitignore`라는 또 다른 추적되지 않는 파일을
찾았습니다. 따라서 이 파일은 저장소에 추가해야 합니다.

그러지 말아야 할 확실한 이유가 없다면 `.gitignore` 파일은 항상 저장소에
넣으세요. 그러면 모든 clone에 이 파일이 존재하므로 편리합니다.

``` {.default}
$ git add .gitignore
$ git commit -m Added
  [main 07582ad] Added
   1 file changed, 1 insertion(+)
   create mode 100644 .gitignore
```

이제 상태를 확인합니다.

``` {.default}
$ git status
  On branch main
  nothing to commit, working tree clean
```

모두 깨끗합니다. `doom` 파일은 여전히 작업 트리에 있지만 `.gitignore`에
들어 있으므로 Git은 신경 쓰지 않습니다.

## `.gitignore`에 하위 디렉터리를 지정할 수 있나요? {#can-i-specify-subdirectories-in-.gitignore}

[i[`.gitignore` file-->With subdirectories]]

네!

파일 일치 규칙은 원하는 만큼 구체적으로도, 포괄적으로도 만들 수 있습니다.

다음 `.gitignore`는 아주 구체적인 파일을 찾습니다.

``` {.default}
subdir/subdir2/foo.txt
```

이 규칙은 저장소 어디에서든 일치합니다. 저장소 루트를 기준으로 특정 파일에만
일치시키려면 앞에 슬래시를 붙일 수 있습니다.

``` {.default}
/subdir/subdir2/foo.txt
```

여기서 `subdir`은 전체 파일 시스템의 루트 디렉터리가 아니라 _저장소_ 루트에
있는 해당 디렉터리를 뜻한다는 점에 유의하세요.

`.gitignore`에 다음 내용을 넣으면

``` {.default}
foo.txt
```

저장소의 모든 하위 디렉터리에 있는 `foo.txt`를 무시합니다.


## `.gitignore`는 어디에 두나요? {#where-do-i-put-the-.gitignore}

[i[`.gitignore` file-->Location]]

저장소의 어느 하위 디렉터리에든 `.gitignore` 파일을 추가할 수 있습니다. 다만
동작 방식은 파일의 위치에 따라 달라집니다.

규칙은 이렇습니다. *각 `.gitignore` 파일은 파일이 들어 있는 디렉터리와 그
아래의 모든 하위 디렉터리에 적용됩니다*.

따라서 저장소 루트 디렉터리의 `.gitignore`에 `foo.txt`를 넣으면 저장소의 모든
하위 디렉터리에 있는 `foo.txt`가 빠짐없이 무시됩니다.

저장소 **어디에서도** 원하지 않는 것이 확실하다면 가장 상위의 `.gitignore`
파일로 차단하세요.

하위 디렉터리에 `.gitignore` 파일을 더 추가하면 해당 하위 디렉터리와 그
아래에만 적용됩니다.

저장소 루트에는 가장 넓게 적용되는 무시 파일 규칙을 두고, 하위 디렉터리로
내려갈수록 더 구체적으로 만드는 방식입니다.

간단한 저장소라면 저장소 루트 디렉터리에 `.gitignore` 하나만 있어도 충분합니다.

곧 `.gitignore` 항목을 덮어쓰는 방법도 이야기하겠습니다.

## 와일드카드 {#wildcards}

[i[`.gitignore` file-->Wildcards]]

원하지 않는 파일을 `.gitignore`에 하나하나 전부 나열해야 할까요? 너무
귀찮습니다!

다행히 Git은 무시할 파일 이름에 _와일드카드_를 지원합니다.

예를 들어 `.tmp`나 `.swp`(Vim의 임시 파일 이름) 확장자로 끝나는 모든 파일을
차단하려면 `*`("별표") 와일드카드를 사용할 수 있습니다. 이 파일들을 차단하는
`.gitignore`를 만들어 봅시다.

``` {.default}
*.tmp
*.swp
```

이제 `.tmp`나 `.swp`로 끝나는 모든 파일이 무시됩니다.

알고 보니 Vim에는 `.swp`와 `.swo`, 두 종류의 스왑 파일이 있습니다. 그렇다면
다음과 같이 추가해도 될까요?

``` {.default}
*.tmp
*.swo
*.swp
```

물론입니다! 잘 작동하지만, 대괄호 안의 문자 중 하나와 일치시키라고 Git에
알리는 더 짧은 방법이 있습니다. 다음 규칙은 위와 같습니다.

``` {.default}
*.tmp
*.sw[op]
```

마지막 줄은 이렇게 읽을 수 있습니다. "임의의 문자들이 이어진 뒤 `.sw`가
나오고, 그 뒤에 `o` 또는 `p`가 나오는 파일 이름과 일치시킨다."

## 부정된 `.gitignore` 규칙 {#negated-.gitignore-rules}

[i[`.gitignore` file-->Negated rules]]

루트 `.gitignore`가 저장소 전체에서 `*.tmp` 파일을 무시한다고 합시다. 여기까지는
문제없습니다.

그런데 개발 도중 깊숙한 하위 디렉터리에 Git에 꼭 넣어야 하는 `needed.tmp`
파일이 생겼습니다.

하지만 나쁜 소식입니다. 루트 수준에서 `*.tmp`를 무시하므로 저장소의 모든 하위
디렉터리에 적용됩니다! 고칠 수 있을까요?

네! `needed.tmp`가 있는 하위 디렉터리에 다음 내용의 새 `.gitignore`를 추가할
수 있습니다.

``` {.default}
!needed.tmp
```

이 규칙은 Git에 이렇게 말합니다. "이봐요, 상위의 무시 규칙 때문에
`needed.tmp`를 무시하고 있었다면 이제 그만 무시해 주세요."

`needed.tmp`는 루트 수준의 무시 파일 때문에 무시되고 있었지만, 더 구체적인 이
파일이 해당 규칙을 덮어씁니다.

이 하위 디렉터리의 모든 `.tmp` 파일을 허용해야 한다면 와일드카드를 사용할 수
있습니다.

``` {.default}
!*.tmp
```

그러면 이 하위 디렉터리의 모든 `.tmp` 파일이 무시되지 않습니다.

## 몇몇 파일만 빼고 모두 무시하려면 어떻게 하나요? {#how-to-ignore-all-files-except-a-few}

부정 규칙을 사용하면 됩니다.

다음 `.gitignore`는 `*.c` 또는 `Makefile`이라는 파일만 빼고 모두 무시합니다.

``` {.default}
*
!*.c
!Makefile
```

첫 줄은 모든 것을 무시합니다. 다음 두 줄은 특정 파일에 대해 그 규칙을
부정합니다.


## 미리 만들어진 `.gitignore` 파일 구하기 {#getting-premade-.gitignore-files}

[i[`.gitignore` file-->Boilerplate]]

[fl[이 저장소|https://github.com/github/gitignore]]에 아주 많이 모여 있습니다.

필요에 따라 직접 만들어도 됩니다. `git status`를 자주 실행해 무시하고 싶은
파일이 있는지 확인하세요.

GitHub에서 새 저장소를 만들 때 내용이 미리 채워진 `.gitignore`를 선택할 수도
있습니다. **경고!** 기존 저장소를 새로 만든 이 GitHub 저장소에 push할 계획이
없을 때만 선택하세요. 기존 저장소를 push할 계획이라면 GitHub의 `.gitignore`가
방해됩니다.

[i[`.gitignore` file]>]
