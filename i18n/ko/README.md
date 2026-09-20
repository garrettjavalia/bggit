# 한국어판 작업 영역

현재는 **미번역 준비본**이다. `src/`의 Markdown은 원문 복사본이며 번역 완료를 의미하지 않는다. 한국어 번역은 이 디렉터리의 `src/`만 수정한다. 저장소 루트 `src/`와 원문 빌드 파일은 원문 업데이트용으로 유지한다.

공통 조판 저장소는 형제 디렉터리 `bgbspd-ko`를 사용한다. `make -C i18n/ko` 또는 `make -C i18n/ko docker`로 독립 빌드한다. 결과는 `i18n/ko/dist/`에 생성된다. 완역 전 기본 HTML 언어는 en이며, 한국어 본문으로 전환하면 `LANGUAGE=ko`를 지정한다.

## 원문 업데이트

`upstream-baseline.json`은 번역이 시작된 원문 커밋과 파일별 SHA-256을 기록한다. 문서 상태는 untranslated, 자산 상태는 copied-asset으로 시작한다. 편집 여부만으로 번역 완료를 자동 판정하지 않는다.

1. `upstream` 원격에서 변경을 가져와 원문 브랜치 변경을 병합한다. 번역은 `i18n/ko/`에 있어 원문 src 파일과 분리된다.
2. `make -C i18n/ko status`로 원문의 추가·변경·삭제와 번역 편집 여부를 확인한다. 이 명령은 어떤 파일도 수정하지 않는다.
3. `git diff <기준 커밋> upstream/main -- src/`로 원문 변경을 읽고 필요한 변경만 번역에 반영한다. `<기준 커밋>`은 manifest의 upstream_commit 값이다.
4. 기준 manifest는 번역 반영과 검수를 마친 뒤에만 갱신한다. 원문을 다시 복사하여 번역문을 덮어쓰지 않는다.

새 원문 그림도 한국어판 src에 의도적으로 반영한다. 독립 복사 구조이므로 원문 이미지가 바뀌어도 기존 한국어판 출력이 몰래 달라지지 않는다.

## 고정된 공통 도구

`toolchain.lock`의 bgbspd-ko 및 bgbspd 커밋을 사용한다. 공통 도구를 업데이트할 때는 두 책의 빌드와 PDF 검토 후 lock을 갱신한다.

원문 Makefile도 baseline에 기록하므로 버전·날짜만 바뀌는 업데이트를 감지한다. Difftool 절과 같은 이름의 장은 각각 `{#difftool}`, `{#difftool-1}`, Mergetool 장은 `{#mergetool}`을 한국어판 사본에 명시해 제목 번역 시 링크를 유지한다. 이 세 파일의 초기 편집 표시는 번역 완료를 뜻하지 않는다.
