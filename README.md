# Platform Test Console

Team Bravo의 DB 이전·컷오버와 에이전트 리허설을 실제 AWS·RKE2에서 시험하기 위한 독립 테스트베드다. 웹에서 실행 준비 상태를 확인하고 테스트를 시작하며, 진행 과정과 측정 결과를 본다. 고도화 후보를 전용 자원에서 반복 비교하는 용도로도 사용한다.

현재는 프로젝트 맥락과 개발 기준을 준비한 단계다. 웹 앱·서버·클라우드 연결·배포는 아직 없다.

## Orca에서 시작하기

이 저장소를 Orca 프로젝트로 추가한 뒤 `AGENTS.md` → `docs/context.md` → `docs/implementation-plan.md` → `docs/upstream.md` 순서로 읽는다. Orca 고유 설정 형식은 아직 확인하지 않았으므로 임의 설정 파일을 만들지 않았다.

첫 작업은 플랫폼 최신 규칙과 담당 이슈 확인, 실행할 플랫폼 버전 확인, 읽기 전용 환경 준비 상태 조회다. 이 문서 준비는 실환경 테스트 실행 승인이 아니다. 공유 DB 전환·장애 주입은 대상과 영향 범위를 확인하고 기존 실행 승인 경로로 수행한다.

## 기준과 문서

- 규칙·계약·기본 설정의 SOT: [platform](https://github.com/Softbank-Hackathon-Finals/platform/tree/develop)
- [요구사항과 인계 맥락](docs/context.md)
- [구현 순서와 완료 기준](docs/implementation-plan.md)
- [업스트림 재사용·갱신·이전 방법](docs/upstream.md)
- [테스트 실행과 준비 상태 모델](docs/testbed.md)
- [플랫폼 버전과 기준 경로](config/upstream.json)

플랫폼의 기존 `develop`·`main`에는 이 저장소 초기화로 변경을 보내지 않는다. 새 저장소에서는 `develop`에서 기능 브랜치를 만들고 `develop`으로 PR을 올린다. `main`은 검증한 릴리스에 사용한다. GitHub 보호 규칙과 필수 CI는 아직 구성하지 않았다.
