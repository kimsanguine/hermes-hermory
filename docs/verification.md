# 검증 범위

2026-11-10에 확인했습니다. 설치 확인, 도구 실행, 실제 계정 실습을 구분합니다. 고객 자료와 실제 메모리 파일은 읽거나 공개하지 않았습니다.

## 공개 저장소와 메모리 스킬

- GitHub 저장소를 PUBLIC으로 생성하고 main을 push했습니다. 최초 스킬 소스 커밋은 d3e0bf45c6b1bc2d50d71404dc19bfcfc2bc1739입니다.
- description은 공백 포함 51자입니다. 개인 절대경로, 토큰 패턴, 다른 프로필 이름을 포함하지 않는지 검사했습니다.
- 실제 기존 jiwon의 스킬 파일과 설정을 덮어쓰지 않았습니다. 읽기 전용 목록 확인만 했습니다. 새 스킬은 공개 교육판이며 사용자의 비공개 운영 문서 원본과 구분합니다.
- 새 임시 HERMES_HOME과 기존 Python 실행 환경에서 Hermes의 do_install 내부 함수를 호출했습니다. 원격 다운로드, 격리, security scan SAFE, 허용 판정, SKILL.md 설치를 확인했습니다. 저장소 원문과 설치 파일 바이트를 대조했습니다.
- 정상 CLI의 --help와 jiwon skills list는 실행했습니다. 임시 HERMES_HOME에서 CLI 런처 전체를 실행한 검증은 런처의 별도 의존성 준비 중 PyPI DNS 오류와 no committed environment로 중단됐습니다. 내부 함수 설치 성공을 CLI 전체 성공으로 주장하지 않습니다. 기존 설치를 복구하거나 업데이트하지 않았습니다.

## 설치형 스킬 확인

공개 upstream을 대상으로 동일한 격리 설치 내부 함수를 사용했습니다. 상세 상태와 원고 해시는 install-checks.json에 있습니다.

| 스킬 | 실제 확인 결과 |
|---|---|
| context7-cli | 다운로드, 검사, 격리 설치 성공 |
| supabase | 다운로드, 검사, 격리 설치 성공 |
| vercel-cli | 다운로드, 검사, 격리 설치 성공 |
| hermes-memory-operations | 공개한 저장소에서 다운로드, SAFE, 격리 설치 성공 |
| hyperframes-cli | 최신 소스 보안 검사에서 차단, 설치 안 됨. 강제 설치하지 않음 |
| dogfood | 기존 jiwon 파일과 Hermes bundled 원문이 같고, 목록에 있음. 재배포하지 않음 |
| reverse-engineer-anything | 다운로드, 검사, 격리 설치 성공 |

HyperFrames 최신 소스 커밋 1a758fd60fac90cd3ce370f5af1cb47e4b610128에서 scanner가 aws_dir_access, backtick_subshell, dump_all_env, hardcoded_ip_port, path_traversal을 보고했습니다. 이것은 패턴 기반 검사 결과이며 악성 코드라고 확정하는 분석은 아닙니다. 추가 실행이나 --force 우회는 하지 않았습니다. 교안은 이미 검토된 설치 환경과 공식 CLI 직접 실행을 구분합니다.

## CLI 실행과 문서 대조

- Node.js 22.22.3, ctx7 0.11.0, HyperFrames 0.8.206, Vercel CLI 52.0.0, Supabase CLI 2.90.0을 확인했습니다. 최신 버전이라는 뜻은 아닙니다.
- ctx7의 library 검색과 /supabase/supabase docs 조회가 실제 결과를 반환했습니다. v2의 insert/update 결과 반환과 select 설명을 확인했습니다.
- HyperFrames init --non-interactive --skip-skills로 새 기본 프로젝트를 만들고 timeline --json으로 10초 타임라인을 읽었습니다. snapshot --at 0이 0초와 자동 추가된 종료 근처 PNG 경로를 반환했습니다. 이는 수강생의 6초 소개 영상 완성이나 MP4 렌더 성공을 뜻하지 않습니다.
- Vercel deploy, logs, env add의 도움말을 읽었습니다. env add --help와 deploy --help가 사용법을 출력하고 exit 2를 반환하는 버전 동작은 실제 배포 실패와 구분했습니다.
- 설치 스킬의 원문과 Context7, Supabase RLS와 Edge Functions, Vercel deploy, Hermes memory, HyperFrames CLI, REA의 공식 문서를 대조했습니다.

## 남은 확인

실제 모델 프롬프트 실행, 메모리 저장과 수정, Supabase 계정의 CRUD/Auth/Edge Functions, Vercel 앱 배포와 로그 조회, 실제 앱 dogfood, REA 분석, 영상 렌더와 퍼블리시, Windows 실행과 수강생 완주는 미실시입니다. 해당 부분은 교안에서 확인 필요로 표시했습니다. 독립 에이전트 3축 채점이나 90점 이상 점수는 이번 작업에서 부여하지 않았습니다.

## Computer Use 화면 확인

- 실시: 최신 Markdown을 변환한 http://localhost:3148/ 를 Aside에서 열었습니다. 서론의 builtin/설치형 구분, 폴더 트리, 4번의 공개 저장소 설치 명령과 세 자연어 요청, 5번의 설치 차단과 CLI 대체 경로, 6번의 추가 설치 없는 기본판 확인 안내를 시각적으로 확인하고 화면을 캡처했습니다. 캡처는 이번 Computer Use 도구 결과에 있습니다.
- 로컬 인쇄 미리보기에서 최종 9페이지, 서론 1페이지와 각 모듈 및 마무리의 페이지 구분을 확인했습니다. 로컬 HTML의 A4 인쇄 스타일 기준이며 다른 Markdown 뷰어의 기본 인쇄 결과까지 보장하는 수치가 아닙니다. PDF 파일은 만들지 않았습니다.
- 미실시: 모든 참고 링크의 브라우저 클릭, 모바일 기기의 실제 읽기, 전체 실습 완주. 별도 점수로 환산하지 않았습니다.
- 교안과 스킬은 GitHub 공개 자료입니다. Habix 프로덕션 교안에는 아직 반영하지 않았습니다.
