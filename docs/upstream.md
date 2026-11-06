# 플랫폼 재사용·갱신·이전

## SOT와 실행 버전

기본 규칙과 계약의 원문은 platform `develop`이다. `config/upstream.json`은 원본 위치와 초기 기준 SHA를 기록한다. 실행과 재현에는 검증한 SHA·이미지 digest를 고정한다. `develop`을 자동 추종해 실행 코드를 바꾸지 않는다. 최신 규칙을 확인하는 것과 실행 버전을 올리는 것은 별도 작업이다.

`.gitignore`와 `.gitattributes`는 초기 기준 SHA의 원본을 사용하며 `.gitignore` 끝에는 이 저장소 로컬 산출물 제외만 추가했다. CI·봇·CODEOWNERS·클라우드 배포 설정은 원본을 조사해 새 레포의 실제 경로와 자격에 맞게 연결한다. 기존 봇의 플랫폼 작업 보드 변경이나 자동 배포가 이 저장소에서 실행되지 않도록 트리거·대상 레포를 확인한다.

## 읽기용 소스 준비

저장소 루트에서 다음과 같이 체크아웃한다. `.upstream`은 추적하지 않는다.

```sh
mkdir -p .upstream
git clone --no-checkout https://github.com/Softbank-Hackathon-Finals/platform.git .upstream/platform
git -C .upstream/platform remote set-url --push origin DISABLED
# config/upstream.json의 execution_ref를 아래에 지정한다.
git -C .upstream/platform checkout --detach <execution_ref>
```

이미 체크아웃했다면 원격이 공식 platform인지 확인한 뒤 `git fetch origin develop`로 변경을 읽는다. 새 SHA의 계약·생산자·소비자·검증 결과를 확인하고 `execution_ref`를 명시적으로 갱신한다. 쓰기 URL 비활성화는 실수 방지이며 GitHub 권한 통제를 대신하지 않는다. 플랫폼으로 push하는 동기화 스크립트나 자동 병합은 만들지 않는다.

## 재사용과 이전

기존 API가 있으면 API를 사용한다. 소스 사용이 필요하면 기존 패키지 경계·import·의존성 선언·테스트를 보존하고 기존 플랫폼 경로로 배치 가능한 단위로 구현한다. `.upstream`의 절대경로나 개발자의 로컬 형제 디렉터리에 런타임을 의존시키지 않는다. 관련 없는 플랫폼 코드 전체를 vendoring하지 않는다.

독립 웹·제어 모듈과 플랫폼 어댑터를 분리하되 새로운 판정 계약을 만들지 않는다. 초기 앱 생성 전에 platform의 `services/console`, `services/platform-events`, `services/rehearsal-runner`, `services/cutover-executor` 패키지 구성을 조사하고 이전 대상 경로를 문서화한다.

플랫폼 반영 시 모듈·테스트·의존성 선언과 사용 문서를 함께 옮기고 대상 규약에 맞는 PR을 만든다. 새 레포의 CI 통과를 플랫폼 CI 통과로 대신하지 않는다. 기존 작성자 출처와 제3자 라이선스는 보존한다. 이 저장소만의 라이선스나 별도 사용 제한은 추가하지 않는다. GitHub 접근 권한이나 기존 제3자 의무가 없어지는 것은 아니다.
