# AI 에이전트 스킬 설치와 활용

AX랩/KDT | 비개발자와 초급 개발자 | 기본 155분, 선택 모듈 10분, 휴식 10분

오늘은 “공개 학습 링크 보관함”을 만듭니다. 반복해서 찾는 문서 링크를 저장하고, 다른 사람에게 웹 주소로 보여 주는 작은 프로젝트입니다. 스킬별 실습은 독립적으로 할 수 있고, 앞의 결과물이 있으면 이어서 사용합니다.

## 서론. 스킬을 설치하면 무엇이 달라질까? (15분)

에이전트에게 “앱을 만들어 줘”라고만 요청하면 문서 버전, 데이터 접근, 배포 대상, 확인 방법까지 매번 설명해야 합니다. 스킬은 그 업무를 처리할 순서와 판단 기준을 묶어 필요할 때 읽게 합니다. 설치만으로 계정 권한이나 실행 도구가 생기지는 않습니다. Supabase 스킬을 읽는 것과 내 DB에 연결하는 것은 별개의 준비입니다.

| 구분 | 뜻 | 오늘의 예 |
|---|---|---|
| builtin | Hermes가 기본 제공하는 스킬. 설치 상태와 활성 여부는 환경마다 확인 | `dogfood` |
| 설치형 | 공식 저장소나 검토한 공개 저장소에서 추가하는 스킬 | `context7-cli`, `supabase`, `vercel-cli`, `hyperframes-cli`, `reverse-engineer-anything` |
| 직접 만든 스킬 | 자신의 반복 업무 기준을 SKILL.md로 정리한 것. 공개하면 다른 사람도 설치 가능 | `hermes-memory-operations` |

이 교안의 기준 환경은 Hermes입니다. 기존 수업 프로필 `jiwon`을 유지합니다. 다른 프로필로 시작했다면 모든 터미널 명령에서 `-p jiwon`을 빼고 현재 프로필을 쓰세요. Claude Code에서는 설치 경로와 사용 가능한 도구가 다르며, Hermes 전용 메모리 모듈을 그대로 적용하지 않습니다.

수업 전 Hermes 대화, Node.js 22.13 이상과 npm, Supabase와 Vercel 계정 로그인을 준비합니다. 영상 렌더에는 브라우저와 FFmpeg도 필요합니다. 계정과 도구 자체의 최초 설치는 사전 준비입니다.

확인용 터미널에서 전체 스킬을 먼저 봅니다. 각 모듈에서도 이 목록에서 해당 이름과 `enabled`를 확인합니다.

```bash
hermes -p jiwon skills list
```

`skills_list`는 에이전트 내부 도구 이름입니다. 대화창에서 “skills_list로 현재 스킬을 확인해 주세요”라고 요청합니다. `/스킬이름 요청`은 대화창에, `hermes ...`는 터미널에 입력합니다.

설치 시 출처와 검사 결과를 읽고 승인합니다. 같은 이름이 있으면 재설치하지 않고, 설치 후 새 대화를 엽니다. `--source local`은 전체 설치 확인에 쓰지 않습니다. `--force`로 검사나 충돌을 우회하지 않습니다.

오늘의 결과물은 다음과 같습니다. 완성 파일 묶음을 먼저 외우지 말고 필요한 단계에서 하나씩 만듭니다.

```text
link-shelf/
├── index.html              # 공개 학습 링크를 보여 주는 화면
├── docs/
│   ├── library-notes.md     # 문서 출처와 사용할 API
│   ├── db-plan.md           # 테이블, 공개 범위, 선택 확장
│   ├── deploy.md            # 실제 배포 URL와 확인 결과
│   └── reverse-notes.md     # 선택 분석 결과
├── promo/                  # 선택한 소개 영상 프로젝트
└── dogfood-output/
    ├── screenshots/
    └── report.md
```

검증 범위는 [검증 기록](verification.md)에 있습니다. 자연어는 요청 예시이며 기대 결과는 정답 보장이 아닙니다. `install`은 격리된 설치 내부 함수로 검증하고, CLI 런처까지의 완주는 확인 필요입니다. 계정 실습과 메모리 쓰기, 실제 앱 QA도 별도 확인 필요입니다.

## 1. context7-cli: 최신 문서를 읽고 시작하기 (15분)

없으면 모델이 기억하는 예전 API로 코드를 만들고, 실행 오류가 난 뒤에야 버전 차이를 찾게 됩니다. Context7은 현재 색인된 문서를 가져옵니다. 내 프로젝트의 설치 버전과 문서 버전이 같은지는 별도로 확인합니다.

설치와 확인, 터미널. 설치 내부 함수 검증, CLI 런처 완주는 확인 필요:

```bash
hermes -p jiwon skills install upstash/context7/skills/context7-cli
hermes -p jiwon skills list
```

목록에서 `context7-cli`를 확인합니다. 스킬과 별개로 CLI는 아래처럼 npm에서 실행합니다. 이 교안의 직접 확인 버전은 `ctx7@0.11.0`입니다.

핵심 패턴은 두 가지입니다.

1. 라이브러리 이름을 검색해 정확한 ID를 받습니다.

```bash
npx -y ctx7@0.11.0 library supabase "JavaScript v2 select insert update delete"
```

2. 검색 결과에 나온 ID로 한 가지 질문을 묻습니다. 제작 시 아래 ID가 실제 반환됐습니다.

```bash
npx -y ctx7@0.11.0 docs /supabase/supabase "JavaScript v2 createClient select insert update delete"
```

5분 미니 실습, Hermes 대화창의 요청 예시:

```text
/context7-cli Supabase JavaScript v2에서 공개 링크 목록을 읽는 방법을 찾아 주세요. 출처 URL와 실제로 사용할 호출을 docs/library-notes.md에 적고, 설치 버전과 다른 예시는 제외하세요. 아직 앱은 만들지 마세요.
```

기대 결과: 출처 URL, `select()` 사용법, 버전 확인 항목이 담긴 문서 한 개입니다. 직접 조회에서는 v2의 `insert()`와 `update()`가 변경한 행을 돌려주려면 `.select()`를 붙이는 설명과 실제 예제가 반환됐습니다. API 호출 성공과 데이터 반환은 다를 수 있다는 점을 확인하세요.

막히면: `docs supabase ...`처럼 제품 이름을 ID로 쓰지 말고 검색이 반환한 `/org/project` 형태를 씁니다. 요청 제한에 걸리면 짧은 주제 한 개로 줄이고 안내된 재시도 시간을 따르세요. 로그인은 더 높은 한도가 필요할 때 검토합니다.

다음에 더 찾아볼 것: [Context7 CLI 공식 문서](https://github.com/upstash/context7/blob/master/docs/clients/cli.mdx).

## 2. supabase: 링크를 저장하고 다시 읽기 (25분)

없으면 새로고침할 때 데이터가 사라지거나, 화면의 가짜 배열을 DB가 연결된 결과로 착각하기 쉽습니다. Supabase는 데이터를 보관하는 Database, 사용자 확인을 맡는 Auth, 서버 코드를 실행하는 Edge Functions를 제공합니다.

설치와 확인, 터미널. 설치 내부 함수 검증, CLI 런처 완주는 확인 필요:

```bash
hermes -p jiwon skills install supabase/agent-skills/skills/supabase
hermes -p jiwon skills list
```

목록에서 `supabase`를 확인합니다. 스킬 설치는 Supabase 계정 로그인이나 MCP 연결을 대신하지 않습니다. 기본 실습은 Dashboard의 Table Editor와 브라우저용 publishable key로 진행합니다. SQL과 접근 정책은 실행 전에 에이전트가 변경안을 보여 주게 합니다.

핵심 패턴은 세 가지입니다.

1. DB와 CRUD. 내 실습 프로젝트에 `learning_links` 테이블을 만들고 `id`, `title`, `url`, `note`를 둡니다. 기본 경로는 공개 링크만 누구나 읽고, 추가와 수정과 삭제는 내가 Dashboard에서 합니다.

```text
/supabase 공개 학습 링크 보관함의 learning_links 테이블과 읽기 전용 웹 연결을 설계해 주세요. RLS와 필요한 SELECT 권한, 작성 권한은 Dashboard에만 두는 변경안을 docs/db-plan.md에 먼저 보여 주세요. 기존 테이블은 바꾸지 마세요.
```

2. Auth, 선택 확장. “내 링크만 쓰기”를 추가할 때 로그인과 소유권이 필요합니다.

```text
/supabase 이 보관함의 비공개 확장에 이메일 로그인을 연결하려 합니다. user_id와 본인 행만 읽고 쓰는 RLS 변경안을 먼저 보여 주세요. 승인 후 내 실습 계정으로 연결하고 로그인 전후 조회 결과를 비교해 주세요. 토큰은 출력하지 마세요.
```

3. Edge Functions, 선택 확장. 브라우저에 둘 수 없는 키나 서버 처리가 생길 때 사용합니다.

```text
/supabase URL 형식을 검사하는 Edge Function의 변경안을 보여 주세요. 승인하면 내 실습 프로젝트의 Dashboard에서 만들고 테스트하겠습니다. https://context7.com은 정상, “잘못된주소”는 오류로 구분해 주세요. 기존 앱 연결은 아직 바꾸지 마세요.
```

5분 미니 실습, 계정 실습 확인 필요: 승인한 DB 계획을 내 실습 프로젝트에 적용합니다. Table Editor에서 `Context7 / https://context7.com / 문서 조회` 행을 추가하고 새로고침해 읽습니다. note를 `최신 문서 확인`으로 바꾼 뒤, 별도로 만든 `삭제 연습` 행 하나만 지웁니다. 기대 결과는 추가, 조회, 수정, 삭제를 각각 한 번 확인한 것입니다. 다른 자료는 삭제하지 않습니다.

이어서 에이전트에게 “index.html 하나로 공개 목록을 읽어 카드로 보여 주세요. publishable key만 사용하고 빈 목록, 연결 실패, 잘못된 URL을 구분해 주세요”라고 요청합니다. DB가 없어 단독 실습한다면 가상 링크 3개를 보여 주는 HTML로 계속하되 화면에 `샘플 데이터`라고 표시합니다. DB 연결 성공으로 기록하지 않습니다.

막히면: 데이터가 없을 때 RLS를 끄지 말고 해당 역할의 SELECT 권한과 RLS 정책을 함께 확인합니다. publishable key는 공개 클라이언트용이며 RLS가 접근을 통제합니다. `service_role`이나 secret key는 브라우저와 저장소에 넣지 않습니다. Auth와 Edge Function은 3시간 기본 완주에 포함하지 않습니다.

다음에 더 찾아볼 것: [Supabase Row Level Security](https://supabase.com/docs/guides/database/postgres/row-level-security).

## 3. vercel-cli: 다른 사람이 열 수 있는 주소 만들기 (20분)

없으면 내 컴퓨터에서는 열리지만 다른 사람에게 보여 줄 주소가 없습니다. Vercel 스킬은 배포뿐 아니라 어느 프로젝트를 바꾸는지, 환경변수를 어디에 두는지, 오류 로그를 어디서 보는지도 안내합니다.

설치와 확인, 터미널. 설치 내부 함수 검증, CLI 런처 완주는 확인 필요:

```bash
hermes -p jiwon skills install vercel/vercel/skills/vercel-cli
hermes -p jiwon skills list
```

목록에서 `vercel-cli`를 확인합니다. CLI가 없다면 설치와 로그인을 먼저 준비합니다. 아래 명령의 도움말은 Vercel CLI 52.0.0에서 직접 확인했습니다. 계정 작업 자체는 확인 필요입니다.

핵심 패턴은 세 가지입니다.

1. 프로젝트와 Preview 배포, Hermes 요청 예시:

```text
/vercel-cli 현재 link-shelf 폴더를 배포하려 합니다. 연결된 계정과 프로젝트, 공개될 파일을 먼저 확인해 주세요. 내가 대상 확인 후 승인하면 Preview로 배포하고 실제 URL를 docs/deploy.md에 남겨 주세요.
```

2. 환경변수. 지금의 정적 HTML에서 publishable 설정을 쓰는 것과 서버 비밀키 보관은 다릅니다. 빌드 도구를 추가한 경우 `VITE_` 같은 공개 접두사는 브라우저에 전달됩니다. 비밀키를 숨기는 기능으로 쓰지 않습니다.

```text
/vercel-cli 이 프로젝트의 설정값이 브라우저에 공개되는지 먼저 구분해 주세요. 필요한 환경변수 이름과 Preview 적용 대상을 정리하고, 값은 출력하지 마세요. 변경 후 재배포가 필요한지도 알려 주세요.
```

3. 로그. 배포 실패는 Build Logs, 배포 후 서버 실행 오류는 Runtime Logs에서 확인합니다. 정적 HTML의 브라우저 오류는 브라우저 콘솔에서 봅니다.

```bash
vercel logs --help
```

5분 미니 실습, 계정 실습 확인 필요: `index.html`이 있는 link-shelf 폴더에서 대상을 확인한 뒤 Preview 배포합니다. 단독 모듈이라면 “공개 학습 링크 3개를 보여 주는 index.html을 만들어 주세요”로 시작합니다. 터미널에서 직접 실행하는 명령은 다음과 같습니다.

```bash
vercel deploy
```

기대 결과: CLI가 반환한 실제 Preview URL를 열었을 때 링크 카드가 나타나고 링크 하나가 열립니다. 배포 메시지만 보지 말고 브라우저에서 확인하세요. DB 미연결 샘플이라면 그 상태도 deploy.md에 적습니다.

막히면: 다른 앱이 배포되면 현재 폴더와 `.vercel` 연결부터 확인합니다. Preview에 로그인 화면이 보이면 Deployment Protection과 방문자 접근 권한을 확인합니다. 보호를 무작정 끄지 않습니다. 환경변수 수정은 이미 만들어진 배포에 소급 적용되지 않으므로 새 배포와 해당 URL를 확인합니다.

다음에 더 찾아볼 것: [Vercel deploy 공식 문서](https://vercel.com/docs/cli/deploy).

## 4. hermes-memory-operations: 기억할 기준을 직접 고르기 (15분)

없으면 매번 같은 선호를 설명하거나, 일회성 요청을 오래 적용할 기준으로 저장할 수 있습니다. 메모리는 “앞으로도 유효한 사실과 선호”, 스킬은 “다시 실행할 절차”를 담습니다.

이 모듈은 강사의 기존 운영 원칙을 공개 교육용으로 정리한 설치형 스킬을 사용합니다. 개인 메모리, 다른 프로필, 외부 메모리 서비스는 다루지 않습니다.

설치와 확인, 터미널. 설치 내부 함수 검증, CLI 런처 완주는 확인 필요:

```bash
hermes -p jiwon skills install kimsanguine/hermes-memory-operations/skills/hermes-memory-operations
hermes -p jiwon skills list
```

목록에 `hermes-memory-operations`가 있는지 확인하고 새 대화를 엽니다. 이 스킬을 설치해도 메모리 도구가 자동으로 추가되지는 않습니다. 현재 Hermes가 실제 제공하는 메모리 도구와 스키마를 먼저 확인합니다. Claude Code의 메모리 방식과도 다릅니다.

핵심 패턴은 세 가지입니다. 모두 Hermes 대화창의 요청 예시입니다.

```text
/hermes-memory-operations 현재 프로필에 “실습용 선호: 설명 뒤에 예시 한 개를 붙인다”를 기억시키고 싶어요. 메모리 도구가 있는지 확인하고 변경안만 보여 주세요. 기존 메모리 전체는 출력하지 마세요.
```

```text
/hermes-memory-operations 방금 저장한 실습용 선호에서 “예시 한 개”를 “짧은 체크리스트”로 바꾸는 변경안을 보여 주세요. 다른 항목은 수정하지 마세요.
```

```text
/hermes-memory-operations 이번 실습에서 추가한 항목만 점검해 주세요. 오래 보관할 가치가 있는지, 임시 결과인지 설명하고 아직 수정하거나 지우지 마세요.
```

5분 미니 실습, 메모리 쓰기 확인 필요: 첫 변경안을 읽고 맞으면 “지금 보여 준 실습용 선호 한 줄을 현재 프로필에 저장해 주세요”라고 승인합니다. 도구의 실제 성공 결과와 재조회를 확인합니다. 두 번째 변경안도 같은 방식으로 승인합니다. 기대 결과는 가상 선호가 한 줄 추가된 뒤 그 한 줄만 바뀐 상태입니다. 마지막에 삭제할지 직접 결정하세요. 저장안이나 에이전트의 약속만 출력됐다면 미저장입니다.

막히면: 용량 초과 시 기존 기억을 자동으로 지우지 말고 중복 항목과 정리안을 먼저 받습니다. 새 세션의 메모리 스냅샷과 현재 저장 상태가 다를 수 있으니 실제 저장 상태를 확인합니다. `/skills approval on`은 스킬 파일 쓰기 승인 설정이며 메모리 변경 승인과는 다릅니다.

다음에 더 찾아볼 것: [공개 Hermes Memory Operations 저장소](https://github.com/kimsanguine/hermes-memory-operations).

## 5. hyperframes-cli: 프로젝트를 소개하는 짧은 영상 만들기 (15분)

없으면 화면을 설명하는 영상도 편집 도구에서 처음부터 만들어야 합니다. HyperFrames는 HTML로 장면을 구성하고 타임라인의 특정 시점을 확인한 뒤 영상 파일로 렌더합니다. 웹앱 배포와 영상 퍼블리시는 서로 다른 결과물입니다.

설치 확인, 터미널:

```bash
hermes -p jiwon skills list
```

목록에서 `hyperframes-cli`를 확인합니다. 현재 최신 스킬은 제작 시 자동 보안 검사에서 환경변수 일괄 출력과 경로 접근 등의 패턴으로 설치가 차단됐습니다. 신규 설치는 보류하고 `--force`를 사용하지 않습니다. 설치가 검토된 기존 환경은 스킬을 사용하고, 미설치자는 아래 공식 CLI 경로로 실습합니다. Node.js, 브라우저, FFmpeg는 별도 준비입니다.

핵심 패턴은 세 가지입니다. link-shelf 안에서 시작합니다.

1. 스캐폴딩과 타임라인, 직접 확인 버전 0.8.206:

```bash
npx -y hyperframes@0.8.206 init promo --non-interactive --skip-skills
cd promo
npx -y hyperframes@0.8.206 timeline --json
```

`--skip-skills`는 이 생성 명령이 다른 도구의 스킬까지 추가하지 않게 합니다. 필요하면 영상 저작용 `hyperframes` 스킬을 별도로 검토해 추가합니다.

2. 프레임 확인과 렌더. 스냅샷의 PNG를 먼저 보고, 문제가 없을 때 MP4로 만듭니다.

```bash
npx -y hyperframes@0.8.206 snapshot --at 0
```

렌더 명령은 도움말만 확인했습니다. 실제 MP4 생성은 확인 필요입니다.

```bash
npx -y hyperframes@0.8.206 render --output promo.mp4
```

3. 퍼블리시, 선택. 계정과 공개 범위를 확인한 뒤 스킬에 요청합니다.

```text
/hyperframes-cli 이 소개 영상을 웹 링크로 공유하려 합니다. 현재 프로젝트, 계정, 비공개와 공개의 차이를 먼저 알려 주세요. 아직 업로드하지 마세요.
```

5분 미니 실습, 이미 스킬이 설치된 환경의 요청 예시:

```text
/hyperframes-cli promo의 기존 스캐폴딩을 유지하고 “링크를 찾고, 저장하고, 공유합니다”를 보여 주는 6초 소개 영상으로 바꿔 주세요. 0초, 2초, 4초의 프레임을 확인하고 파일 경로를 알려 주세요. 아직 퍼블리시하지 마세요.
```

미설치자는 패턴1과 2의 공식 CLI로 기본 스캐폴딩을 만들고 타임라인과 0초 PNG를 확인합니다. 기대 결과는 실제 생성 파일과 프레임 경로이며, 이를 스킬 활용 성공으로 기록하지 않습니다. 스킬 설치 환경의 기대 결과는 서로 다른 세 프레임입니다. MP4 렌더와 퍼블리시는 선택이며 실제 완성 영상 생성은 별도 확인 필요입니다.

막히면: 렌더가 실패하면 먼저 `doctor --help`로 진단 명령을 확인하고 브라우저와 FFmpeg 누락을 점검합니다. HTML 파일을 열었다고 타임라인 재생이 검증된 것은 아닙니다. 정지 프레임뿐 아니라 시간에 따라 내용이 바뀌는지 확인합니다.

다음에 더 찾아볼 것: [HyperFrames CLI 공식 가이드](https://github.com/heygen-com/hyperframes/blob/main/docs/developers/cli.mdx).

## 6. dogfood: 직접 써 보고 재현 가능한 문제 남기기 (20분)

없으면 첫 화면이 열리는지만 보고 완료라고 판단하기 쉽습니다. dogfood는 사용자의 순서로 앱을 탐색하고, 예상과 실제의 차이를 로그와 화면 증거로 남깁니다. 버그 수를 채우는 작업이 아닙니다.

추가 설치 없이 기본 스킬을 먼저 확인합니다.

```bash
hermes -p jiwon skills list
```

`dogfood`와 `enabled`를 확인하세요. 기본판이 없는 환경이라면 설정에서 비활성 여부와 해당 Hermes 버전의 bundled skills 목록을 확인합니다. 다른 동명 스킬을 임의로 설치하거나 기존 것을 덮어쓰지 않습니다. 기본판은 브라우저 도구가 필요하므로 실제 사용 가능 여부도 확인합니다.

핵심 패턴은 두 가지입니다. 첫 요청에서 URL 한 곳과 테스트할 흐름을 지정합니다.

```text
/dogfood 내가 방금 배포한 링크 보관함의 실제 Preview URL를 사용해 주세요. 목록, 링크 열기, 모바일 줄바꿈을 확인하고 dogfood-output/report.md에 예상과 실제, 재현 순서, 화면 증거를 남겨 주세요. DB 쓰기와 외부 계정 로그인은 하지 마세요.
```

```text
/dogfood 방금 발견한 문제 중 사용을 가장 막는 한 개만 다시 확인해 주세요. 같은 조건으로 재현되는지, 수정 후 달라졌는지 증거를 비교하고 확인하지 못한 부분도 적어 주세요.
```

5분 미니 실습, 실제 앱 QA 확인 필요: 첫 요청에 자신의 실제 URL를 전달합니다. 첫 카드 열기, 브라우저 뒤로 가기, 좁은 화면에서 카드와 버튼 읽기를 확인합니다. 단독 모듈이면 자신이 소유한 기존 웹앱을 쓰세요. 기대 결과는 `report.md` 한 개와 확인한 화면의 실제 증거입니다. 버그가 없으면 “이 세 동작에서 발견 못 함”과 확인 범위를 적습니다. 캡처나 로그를 못 얻으면 이유를 밝히고 없는 증거를 만들지 않습니다.

막히면: “브라우저 도구 없음”은 스킬 부재와 다릅니다. 도구를 활성화하거나 사람이 같은 세 동작을 수행하고 캡처를 제공하되 이를 수동 QA라고 표시합니다. Preview 인증 화면에서 막히면 앱 버그로 단정하지 말고 접근 권한부터 확인합니다. 로그에는 토큰과 계정 정보가 없는지 확인하고 저장합니다.

다음에 더 찾아볼 것: [Hermes builtin dogfood 원문](https://github.com/NousResearch/hermes-agent/blob/main/skills/software-development/dogfood/SKILL.md).

## 7. reverse-engineer-anything: 관찰과 추정을 구분해 읽기 (심화 선택, 10분)

없으면 화면이 비슷하다는 이유로 내부 동작도 같다고 추정하기 쉽습니다. 이 스킬은 REA 도구로 앱과 코드의 근거를 수집해, 관찰한 사실과 추정과 아직 모르는 것을 구분합니다.

설치와 확인, 터미널. 설치 내부 함수 검증, CLI 런처 완주는 확인 필요:

```bash
hermes -p jiwon skills install morluto/rea/.agents/skills/reverse-engineer-anything
hermes -p jiwon skills list
```

목록에서 `reverse-engineer-anything`을 확인합니다. 스킬 설치는 REA 실행 도구나 MCP 등록과 다릅니다. 기본 선택 실습은 자신이 만든 link-shelf 코드의 읽기이며, 바이너리 분석 도구는 설치하지 않습니다.

핵심 패턴은 두 가지입니다.

```text
/reverse-engineer-anything 내가 만든 link-shelf 폴더에서 링크 카드를 읽고 URL를 여는 경로를 분석해 주세요. REA 도구가 있는지 먼저 확인하고, 실제 코드 근거와 추정, 미확인을 docs/reverse-notes.md에 구분해 주세요. 코드는 실행하거나 수정하지 마세요.
```

```text
/reverse-engineer-anything 내가 제공한 공개 오픈소스 예제와 이 앱의 링크 열기 방식을 비교해 주세요. 파일과 함수 근거가 있는 차이만 적고 서버 내부 동작은 추정하지 마세요.
```

5분 미니 실습, REA 실제 분석 확인 필요: 첫 요청을 실행해 링크 카드에서 URL가 어디서 오는지 따라갑니다. 기대 결과는 진입 파일, 이벤트 또는 링크 요소, 데이터 출처, 확인하지 못한 범위가 적힌 문서입니다. REA 도구가 없다면 “일반 코드 읽기만 가능, REA 분석 미실시”로 표시하고 이 모듈을 종료합니다. 존재하지 않는 분석 도구 호출을 만들지 않습니다.

막히면: 접근 가능한 공개 화면만으로 서버 코드나 DB 구조를 안다고 쓰지 않습니다. 남이 만든 앱을 분석하려면 허가받은 코드나 공개 라이선스 예제를 직접 지정하세요. 로그인 우회나 비공개 데이터 추출을 이 실습에 포함하지 않습니다.

다음에 더 찾아볼 것: [REA 공식 저장소](https://github.com/morluto/rea).

## 마무리. 스킬 세 개 이상으로 작은 프로젝트 마치기 (30분)

“공개 학습 링크를 새로고침해도 다시 볼 수 있고, 다른 사람이 URL로 열 수 있다”가 기본 목표입니다. 아래 네 단계에서 context7-cli, supabase, vercel-cli, dogfood를 연결합니다. DB가 준비되지 않았다면 샘플 화면 배포와 QA까지 마치고 DB 연결은 미완료로 표시하세요.

| 단계 | 입력과 요청 | 남길 결과 |
|---|---|---|
| 1. 문서, 5분 | 모듈1의 요청으로 사용 API와 출처 확인 | library-notes.md |
| 2. 저장과 화면, 10분 | 승인한 공개 읽기 계획으로 링크 3개를 등록하고 index.html에서 조회 | DB의 3개 행과 실제 카드 목록 |
| 3. 공유, 5분 | 대상 폴더와 프로젝트를 확인하고 Preview 배포 | deploy.md의 실제 URL |
| 4. 사용, 10분 | 그 URL의 카드, 링크 열기, 모바일 줄바꿈 확인 | report.md와 실제 증거 |

에이전트에게 한꺼번에 전부 맡기지 말고 아래 첫 요청으로 시작합니다.

```text
공개 학습 링크 보관함을 마무리하겠습니다. 현재 파일과 실제 DB 연결 상태부터 확인하고, 문서 조회, 저장, Preview 배포, QA 순서로 진행해 주세요. 변경할 DB와 배포 대상을 내가 확인하기 전에는 실행하지 마세요. 샘플과 실제 연결 결과를 구분해 주세요.
```

완료 확인은 다섯 가지입니다. 실제 문서 출처가 남았는가, DB를 다시 읽어도 같은 링크가 나오는가, Preview URL에서 같은 목록이 보이는가, 링크 하나가 열리는가, QA 보고서가 실제 확인과 미확인을 구분하는가. 배포 주소만 있고 사용 결과가 없으면 마지막 단계가 남은 상태입니다.

시간이 남으면 자신의 설명 선호를 메모리로 저장하고, 같은 프로젝트의 소개 영상을 만드세요. 심화 선택 모듈까지 참여했다면 코드 분석에서 배운 한 가지를 설명합니다. 모든 스킬을 억지로 한 요청에 넣을 필요는 없습니다.

제출물: 실제 Preview URL, docs 폴더, dogfood-output/report.md. 키와 로그인 정보, 개인 메모리 파일은 제출하지 않습니다. 설치 확인, 실제 DB 연결, 배포, QA, 영상 렌더의 상태를 각각 적으면 다음 실습의 출발점을 알 수 있습니다.
