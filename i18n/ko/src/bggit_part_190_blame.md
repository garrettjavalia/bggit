# 이 코드는 누구 탓일까요? {#whos-to-blame-for-this-code}

[i[Blame]<]

공유 코드베이스에서 명백히 잘못된 무언가를 발견했다고 해 봅시다.
조금 너그럽게 말하자면, _흥미로운_ 무언가를 발견한 셈입니다.

그리고 그 _놀라운_ 코드를 누구 탓으로 돌려야 할지 알고 싶습니다.

이럴 때 간단한 Git 명령 하나가 답을 밝혀 줍니다.

다음은 예시 출력입니다. 책의 여백 안에 들어가도록 일부를 잘랐습니다.

``` {.default}
$ git blame --date=short foo.py
  8c96991f (Alice 2024-10-08  4) def encode_data(message, value):
  8c96991f (Alice 2024-10-08  5)     encoded_message = message.enco
  8c96991f (Alice 2024-10-08  6)     encoded_value = value.to_bytes
  3b0b0e76 (Chris 2024-10-09  7)     length = len(encoded_message) 
  3b0b0e76 (Chris 2024-10-09  8)     encoded_length = length.to_byt
  8c96991f (Alice 2024-10-08  9)
  8c96991f (Alice 2024-10-08 10)     data = encoded_length + encode
  8c96991f (Alice 2024-10-08 11)
  8c96991f (Alice 2024-10-08 12)     return data
```

책에 들어가도록 출력을 더 줄이려고 `--date=short` 스위치도 넣었습니다.
이 스위치가 없으면 전체 타임스탬프가 표시됩니다.

이 가상 예시를 보면 Alice가 이 함수의 대부분을 체크인했지만, 다음 날
Chris가 와서 중간의 몇 줄을 수정하거나 추가했습니다.

이제 범인을 알았습니다.

## 더 화려하게 탓하기 {#fancier-blaming}

[i[Blame-->Fancier output]<]

`--color-lines` 스위치를 쓰면 커밋마다 색이 번갈아 나오는 컬러 출력을
볼 수 있습니다. 아주 신나는 일이죠. 언제나 이렇게 표시하고 싶다면
`color.blame.repeatedLines` 구성 옵션을 설정하면 됩니다.

날짜를 조금 줄여 주는 `--date=short`는 이미 살펴봤습니다.

`-e` 또는 `--show-email`로 기여자의 이메일 주소를 표시할 수 있습니다.

`-M`을 사용하면 파일 안에서 이동하거나 복사한 줄을 안정적으로 감지할 수
있습니다. `-C`를 사용하면 여러 파일에 걸쳐 같은 작업을 할 수 있습니다.

마지막으로 VS Code 같은 IDE는 자체 기능이나 확장 기능을 통해 blame을
지원할 수도 있습니다. 어떤 사람들은 이 기능을 늘 켜 놓기도 합니다.

[i[Blame-->Fancier output]>]

[i[Blame]>]
