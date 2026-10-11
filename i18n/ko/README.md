# 한국어판 빌드

의존성은 `책 → bgbspd-ko → bgbspd` 순서다. 이 책은 공개 bgbspd-ko의 지정 커밋만 사용하며 원본 조판 엔진이나 시스템 도구의 설치 절차를 관리하지 않는다.

## 실행

저장소 루트에서:

```sh
make -C i18n/ko docker
make -C i18n/ko html
make -C i18n/ko pdf
make -C i18n/ko status
```

Git, make, Docker가 필요하다. 최초 실행 시 toolchain.lock의 bgbspd-ko 커밋을 `.toolchain/<SHA>/`에 내려받고 **그 저장소의 Dockerfile**을 직접 빌드한다. 원본 bgbspd와 Pandoc/TeX 설치는 공통 Dockerfile이 담당한다. 호스트에 별도 Python·폰트·조판 저장소를 설치할 필요는 없다.

캐시가 있으면 커밋과 작업 트리를 확인한 뒤 재사용한다. 공통 도구를 바꾸려면 toolchain.lock의 bgbspd_ko 커밋만 갱신한다. 엔진의 버전은 그 공통 커밋 내부에서 정한다. 책에는 Dockerfile·실행 어댑터·조판 로직을 복제하지 않는다.

```sh
make -C i18n/ko docker PDF_TARGETS=a4_c_1,a4_bw_2
make -C i18n/ko docker OUTPUT=/absolute/output JOBS=2
```

기본 출력은 i18n/ko/dist의 HTML과 PDF 8종이다. index.html/index-wide.html, 일반·와이드 분할본, ZIP, 로컬 폰트·그림, 예제 소스가 있는 책의 source와 소스 ZIP을 포함한다. all/build/stage도 같은 Docker 경로를 사용한다. clean/pristine은 해당 책이 생성한 출력만 정리하며 번역문이나 다운로드 캐시는 지우지 않는다.

이미지 생성에는 네트워크가 필요하지만 책 빌드 실행은 네트워크 없이 수행한다. 책을 읽기 전용으로 연결하고 출력 폴더에만 쓴다. HTML 검사와 도구 버전 확인은 공통 실행기가 자동 수행한다. tests 역시 `make -C i18n/ko test`로 공통 저장소의 테스트를 실행한다.

책별 설정은 build.json의 profile, lang, 선택적 extra_head다. lang은 실제 번역 상태에 맞춰 지정한다. 원문 루트 src와 번역 i18n/ko/src는 독립적이다.

## 번역 작업 영역과 원문 추적

현재 `i18n/ko/src/`에는 장과 부록 38개의 한국어 번역 초안이 있습니다. 초안은 Sol이 작성했으며 코드 블록, 링크, 색인 매크로, 각주, 제목 구조를 기계적으로 검사하고 일부 장을 원문과 대조해 표본 검수했습니다. 아직 사람의 전수 감수를 거치기 전이므로 출판용 확정 번역으로 간주하지 않습니다.

한국어 번역은 이 디렉터리에만 작성하고, 루트 `src/`는 원문 갱신을 위해 유지합니다. `build.json`의 언어 설정은 실제 한국어 번역 상태와 일치하도록 관리합니다.

## 원문 업데이트

`upstream-baseline.json`은 번역이 시작된 원문 커밋과 파일별 SHA-256을 기록한다. 문서 상태는 untranslated, 자산 상태는 copied-asset으로 시작한다. 편집 여부만으로 번역 완료를 자동 판정하지 않는다.

1. `upstream` 원격에서 변경을 가져와 원문 브랜치 변경을 병합한다. 번역은 `i18n/ko/`에 있어 원문 src 파일과 분리된다.
2. `make -C i18n/ko status`로 원문의 추가·변경·삭제와 번역 편집 여부를 확인한다. 이 명령은 어떤 파일도 수정하지 않는다.
3. `git diff <기준 커밋> upstream/main -- src/`로 원문 변경을 읽고 필요한 변경만 번역에 반영한다. `<기준 커밋>`은 manifest의 upstream_commit 값이다.
4. 기준 manifest는 번역 반영과 검수를 마친 뒤에만 갱신한다. 원문을 다시 복사하여 번역문을 덮어쓰지 않는다.

새 원문 그림도 한국어판 src에 의도적으로 반영한다. 독립 복사 구조이므로 원문 이미지가 바뀌어도 기존 한국어판 출력이 몰래 달라지지 않는다.

## 고정된 공통 도구

`toolchain.lock`은 bgbspd-ko 커밋 하나만 지정한다. 원본 조판 엔진은 공통 도구 내부에서 관리한다. 공통 도구를 업데이트할 때는 두 책의 빌드와 PDF 검토 후 lock을 갱신한다.

원문 Makefile도 baseline에 기록하므로 버전·날짜만 바뀌는 업데이트를 감지한다. 번역된 제목에는 원문의 자동 생성 ID를 명시 앵커로 고정해 내부 링크를 유지한다.
