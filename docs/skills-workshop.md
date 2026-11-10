# Hermes 스킬 6개 설치와 활용

비개발자와 초급 개발자 | 웹 프로젝트 105분, 메모리와 영상 30분, 심화 선택 10분, 휴식 10분

오늘은 “공개 학습 링크 보관함”을 만듭니다. 반복해서 찾는 문서 링크를 저장하고, 다른 사람에게 웹 주소로 보여 주는 작은 프로젝트입니다. 스킬별 실습은 독립적으로 할 수 있고, 앞의 결과물이 있으면 이어서 사용합니다.

## 01. 스킬을 고르고 실습 준비하기

에이전트에게 “앱을 만들어 줘”라고만 요청하면 문서 버전, 데이터 접근, 배포 대상, 확인 방법까지 매번 설명해야 합니다. 스킬은 그 업무를 처리할 순서와 판단 기준을 묶어 필요할 때 읽게 합니다. 설치만으로 계정 권한이나 실행 도구가 생기지는 않습니다. Supabase 스킬을 읽는 것과 내 DB에 연결하는 것은 별개의 준비입니다.

| 구분 | 뜻 | 오늘의 예 |
|---|---|---|
| builtin | Hermes가 기본 제공하는 스킬 | 전체 스킬 목록의 Source가 builtin인 항목 |
| 설치형 | 공식 저장소나 검토한 공개 저장소에서 추가하는 스킬 | `context7-cli`, `supabase`, `vercel-cli`, `hyperframes-cli`, `reverse-engineer-anything` |
| 직접 만든 스킬 | 자신의 반복 업무 기준을 SKILL.md로 정리한 것. 공개하면 다른 사람도 설치 가능 | `hermes-memory-operations` |

이 교안은 기존 수업 프로필 `jiwon`을 사용합니다. [Hermes 설치와 첫 대화](https://teamsparta.habix.ai/teamsparta/10th/setup/hermes), [jiwon 도우미 준비](https://teamsparta.habix.ai/teamsparta/10th/setup/hermes-soul)를 먼저 마칩니다. 다른 프로필을 이미 쓰고 있다면 그 실제 ID를 확인하고 모든 `-p jiwon`을 `-p 실제ID`로 바꿉니다. `-p`를 생략하지 않습니다. 새 프로필을 만들 필요는 없습니다. Claude Code는 설치 경로와 도구가 다르므로 Hermes 전용 명령을 그대로 쓰지 않습니다.

문서 조회부터 웹 배포까지가 105분 경로입니다. 메모리와 영상 미니 실습을 더하면 135분, 심화 분석과 휴식을 포함하면 155분입니다. 웹 기본 경로는 01 → 02 → 03 → 04 → 08이며, 메모리 05와 영상 06을 더해 전체를 연습합니다. 연결이 준비되지 않은 모듈은 결과를 꾸미지 말고 준비 링크로 돌아갑니다. 모델과 클라우드 사용료는 선택한 계정의 플랜을 따릅니다.

| 실습 선택 | 필요한 준비 |
|---|---|
| 공통 | 같은 프로필의 첫 답변, Node.js 22.13 이상과 npm |
| DB와 웹 배포 | Supabase와 Vercel 개인 실습 계정 로그인 |
| 메모리 | 현재 프로필의 메모리 도구 |
| 영상 | HyperFrames 실행용 브라우저, 렌더할 때 FFmpeg |
| 심화 분석 | 내 코드 또는 허가받은 공개 예제, REA 도구 |

[Node.js 준비](https://teamsparta.habix.ai/teamsparta/10th/setup/node) | [Supabase 계정과 연결](https://teamsparta.habix.ai/teamsparta/10th/setup/supabase) | [Vercel CLI와 로그인](https://teamsparta.habix.ai/teamsparta/10th/setup/vercel)

저는 스킬을 고를 때 설치한 개수보다 어떤 일이 쉬워졌는지를 먼저 봅니다. 오늘도 한 가지를 실행하고 결과를 남긴 뒤 다음 도구로 넘어갑니다.

### 내 실습 폴더에서 대화 열기

문서(Documents) 폴더에 비어 있는 `link-shelf` 폴더를 만듭니다. 같은 이름이 이미 있으면 덮어쓰지 말고 새 이름을 선택합니다. 이후 본문과 요청의 `link-shelf`는 내가 선택한 폴더명으로 바꿔 읽습니다. VS Code의 파일 → 폴더 열기로 그 폴더를 열고 터미널 → 새 터미널을 선택합니다. 폴더 이름이 현재 작업 위치에 보이는지 확인하세요.

새 폴더의 터미널에서 macOS와 Windows PowerShell 모두 아래 순서로 실행합니다. `docs`는 이번 결과 문서를 담을 폴더입니다.

```bash
mkdir docs
hermes -p jiwon chat
```

열린 Hermes 대화창에서 상태를 봅니다.

```text hermes
/status
```

`Path:`가 사용할 프로필인지, `Model:`이 사용할 모델인지 확인합니다. 이어서 프로젝트 위치를 확인합니다. 프로필 저장 위치와 작업 폴더는 서로 다릅니다.

```text hermes
현재 작업 폴더의 절대경로와 바로 아래 파일, 폴더 이름만 보여 주세요. link-shelf와 그 안의 docs 폴더가 맞는지 확인하고 파일은 바꾸지 마세요.
```

같은 폴더에서 새 대화가 필요하면 대화창에서 나간 뒤 터미널에서 다시 엽니다. 각 코드 블록은 해당 창에 하나씩 입력합니다.

```text hermes
/exit
```

```bash
hermes -p jiwon chat
```

새 대화에서도 `/status`와 작업 폴더를 확인합니다. 다른 프로젝트 경로라면 멈추고 VS Code에서 실습 폴더를 다시 여세요. 앞으로 `docs/...`, `index.html`, `promo`는 이 폴더를 기준으로 합니다.

확인용 터미널에서 전체 스킬을 먼저 봅니다. 각 모듈에서도 이 목록에서 해당 이름과 `enabled`를 확인합니다.

```bash
hermes -p jiwon skills list
```

`skills_list`는 에이전트 내부 도구 이름입니다. 대화창에서 “skills_list로 현재 스킬을 확인해 주세요”라고 요청합니다. `/스킬이름 요청`은 대화창에, `hermes ...`는 터미널에 입력합니다.

설치 시 출처와 검사 결과를 읽고 승인합니다. 같은 이름이 있으면 재설치하지 않고, 설치 후 새 대화를 엽니다. `--source local`은 전체 설치 확인에 쓰지 않습니다. `--force`로 검사나 충돌을 우회하지 않습니다.

오늘의 결과물은 다음과 같습니다. 완성 파일 묶음을 먼저 외우지 말고 필요한 단계에서 하나씩 만듭니다.

```text reference
link-shelf/
├── index.html              # 공개 학습 링크를 보여 주는 화면
├── docs/
│   ├── library-notes.md     # 문서 출처와 사용할 API
│   ├── db-plan.md           # 테이블, 공개 범위, 선택 확장
│   ├── deploy.md            # 실제 배포 URL와 확인 결과
│   └── reverse-notes.md     # 선택 분석 결과
└── promo/                  # 선택한 소개 영상 프로젝트
```

기본 사례는 공개 학습 링크 보관함, 두 번째 사례는 공개 행사 자료실입니다. 문서, 저장, 공유, 확인의 기준은 유지하고 제목과 자료만 바꿉니다. 설치 후 목록 확인과 실제 도구 실행을 구분해 각자 완료 여부를 판단하세요.

## 02. context7-cli: 최신 문서를 읽고 시작하기 (15분)

없으면 모델이 기억하는 예전 API로 코드를 만들고, 실행 오류가 난 뒤에야 버전 차이를 찾게 됩니다. Context7은 현재 색인된 문서를 가져옵니다. 내 프로젝트의 설치 버전과 문서 버전이 같은지는 별도로 확인합니다.

설치와 확인, Hermes를 설치한 터미널:

```bash
hermes -p jiwon skills install upstash/context7/skills/context7-cli
hermes -p jiwon skills list
```

목록에서 `context7-cli`를 확인합니다. 라이브러리 이름과 하고 싶은 일을 자연어로 알려 주면 스킬이 문서를 찾습니다. 수강생은 사용할 버전, 출처, 내 작업에 적용할 내용을 확인합니다.

핵심 패턴은 문서 찾기와 기존 코드 대조, 두 가지입니다. 이번처럼 처음 만드는 프로젝트는 사용할 v2 문서부터 찾습니다. 아직 패키지가 없다면 설치 버전은 미설치로 적고, 코드가 생긴 뒤 두 번째 패턴으로 다시 대조합니다.

5분 미니 실습, Hermes 대화창의 요청 예시:

```text hermes
/context7-cli Supabase JavaScript v2에서 공개 링크 목록을 읽는 방법을 찾아 주세요. 출처 URL와 실제로 사용할 호출을 docs/library-notes.md에 적고, 설치 버전과 다른 예시는 제외하세요. 아직 앱은 만들지 마세요.
```

기대 결과: 출처 URL, `select()` 사용법, 버전 확인 항목이 담긴 문서 한 개입니다. v2에서 `insert()`와 `update()`로 바꾼 행을 돌려받으려면 `.select()`를 붙입니다. API 호출 성공과 데이터 반환은 다를 수 있습니다. 아래는 메모 형식을 보여 주는 예시입니다.

```text output
문서: Supabase JavaScript v2, select()
출처: https://supabase.com/docs/reference/javascript/select
적용: learning_links의 id, title, url, note 읽기
버전 확인: 실제 프로젝트의 패키지 버전과 문서를 대조
```

이미 작성한 코드가 있다면 문서와 다른 점만 요청합니다. 아래는 index.html이 생긴 뒤에 쓰는 두 번째 패턴입니다.

```text hermes
/context7-cli 현재 index.html의 Supabase 사용법을 해당 버전의 공식 문서와 비교해 주세요. 바꿀 부분과 이유, 출처만 정리하고 코드는 아직 수정하지 마세요. 파일이나 버전이 확인되지 않으면 먼저 알려 주세요.
```

막히면: 어떤 라이브러리인지 찾지 못했다면 이름과 하려는 일을 한 문장으로 좁힙니다. 요청 제한에 걸리면 안내된 시간을 기다립니다. 결과에 출처가 없으면 문서 URL를 추가해 달라고 요청합니다.


<details>
<summary>다른 사례로 해보기: 공개 행사 자료실</summary>

```text hermes
/context7-cli 공개 행사 자료실에서 Supabase JavaScript v2로 행사 링크를 읽는 방법을 찾아 주세요. 사용할 호출과 출처를 docs/library-notes.md에 적어 주세요.
```

이벤트 자료도 같은 select 호출을 사용하며, 문서 출처와 버전 확인 기준이 유지됩니다.

</details>

다음에 더 찾아볼 것: [Context7 CLI 공식 문서](https://github.com/upstash/context7/blob/master/docs/clients/cli.mdx).

완료 확인: 문서 출처, 사용할 API와 버전을 library-notes.md에 남겼습니다. 기존 패키지가 있다면 설치 버전과 대조했고, 없다면 미설치로 표시했습니다.

## 03. supabase: 링크를 저장하고 다시 읽기 (25분)

없으면 새로고침할 때 데이터가 사라지거나, 화면의 가짜 배열을 DB가 연결된 결과로 착각하기 쉽습니다. Supabase는 데이터를 보관하는 Database, 사용자 확인을 맡는 Auth, 서버 코드를 실행하는 Edge Functions를 제공합니다.

설치와 확인, Hermes를 설치한 터미널:

```bash
hermes -p jiwon skills install supabase/agent-skills/skills/supabase
hermes -p jiwon skills list
```

목록에서 `supabase`를 확인합니다. 스킬 설치는 Supabase 계정 로그인이나 MCP 연결을 대신하지 않습니다. 기본 실습은 Dashboard의 Table Editor와 브라우저용 publishable key로 진행합니다. SQL과 접근 정책은 실행 전에 에이전트가 변경안을 보여 주게 합니다.

핵심 패턴은 세 가지입니다.

1. DB와 CRUD. 내 실습 프로젝트에 `learning_links` 테이블을 만들고 `id`, `title`, `url`, `note`를 둡니다. 기본 경로는 공개 링크만 누구나 읽고, 추가와 수정과 삭제는 내가 Dashboard에서 합니다.

```text hermes
/supabase learning_links 테이블과 공개 읽기 전용 웹 연결을 설계해 주세요. RLS, 필요한 SELECT 권한과 실행할 SQL을 docs/db-plan.md에 적고 변경안을 먼저 보여 주세요. 작성은 Dashboard에서만 하며 기존 테이블은 바꾸지 마세요.
```

2. Auth, 선택 확장. “내 링크만 쓰기”를 추가할 때 로그인과 소유권이 필요합니다.

```text hermes
/supabase 이 보관함의 비공개 확장에 이메일 로그인을 연결하려 합니다. user_id와 본인 행만 읽고 쓰는 RLS 변경안을 먼저 보여 주세요. 승인 후 내 실습 계정으로 연결하고 로그인 전후 조회 결과를 비교해 주세요. 토큰은 출력하지 마세요.
```

3. Edge Functions, 선택 확장. 브라우저에 둘 수 없는 키나 서버 처리가 생길 때 사용합니다.

```text hermes
/supabase URL 형식을 검사하는 Edge Function의 변경안을 보여 주세요. 승인하면 내 실습 프로젝트의 Dashboard에서 만들고 테스트하겠습니다. https://context7.com은 정상, “잘못된주소”는 오류로 구분해 주세요. 기존 앱 연결은 아직 바꾸지 마세요.
```

5분 미니 실습: Dashboard 상단에서 내 실습 프로젝트 이름을 확인합니다. `db-plan.md`의 테이블, 읽기 정책과 권한을 검토한 뒤 SQL Editor → New query에 승인한 SQL만 붙여 넣고 Run을 누릅니다. 기존 테이블 삭제나 전체 권한 해제가 들어 있으면 실행하지 말고 수정안을 받습니다. Table Editor를 새로고침해 `learning_links`를 연 뒤 `Context7 / https://context7.com / 문서 조회` 행을 추가하고 새로고침해 읽습니다. note를 `최신 문서 확인`으로 바꾼 뒤, 별도로 만든 `삭제 연습` 행 하나만 지웁니다. 기대 결과는 추가, 조회, 수정, 삭제를 각각 한 번 확인한 것입니다. 다른 자료는 삭제하지 않습니다.

화면을 연결하려면 같은 프로젝트의 Connect 또는 API 설정에서 Project URL와 publishable key를 확인합니다. 이 두 공개 연결값과 테이블명 `learning_links`를 현재 대화에 전달합니다. secret key와 service_role 키는 전달하지 않습니다. 연결 대상을 확인한 다음 요청합니다.

```text hermes
방금 확인한 Project URL와 publishable key로 learning_links를 읽는 index.html을 만들어 주세요. 카드에 title, note, url을 표시하고 빈 목록, 연결 실패를 구분하세요. 다른 키나 테이블은 쓰지 마세요.
```

파일을 저장한 위치를 확인하고 웹브라우저로 열어 Table Editor의 행과 같은지 대조합니다. 파일 직접 열기로 모듈 로딩이 막히면 에이전트에게 이 폴더의 로컬 서버 시작 방법을 요청하고 반환한 실제 주소를 엽니다. DB 없이 단독 실습한다면 가상 링크 3개의 HTML을 만들고 화면에 `샘플 데이터`라고 표시합니다. 기본 공개 읽기를 마친 뒤 Auth와 Edge Function을 확장합니다.

막히면: 데이터가 없을 때 RLS를 끄지 말고 해당 역할의 SELECT 권한과 RLS 정책을 함께 확인합니다. publishable key는 공개 클라이언트용이며 RLS가 접근을 통제합니다. `service_role`이나 secret key는 브라우저와 저장소에 넣지 않습니다. Auth와 Edge Function은 3시간 기본 완주에 포함하지 않습니다.


<details>
<summary>다른 사례로 해보기: 공개 행사 자료실</summary>

```text hermes
/supabase 공개 행사 자료실의 title, url, note 구조를 제안해 주세요. 공개 읽기와 Dashboard 작성 권한을 구분해 docs/db-plan.md에 적어 주세요. 실행 전 변경안을 보여 주세요.
```

행사 안내, 다시보기, 발표자료 중 공개된 링크 3개를 넣습니다. 삭제 연습용 행 하나를 따로 만들고 그 행만 지웁니다.

</details>

다음에 더 찾아볼 것: [Supabase Row Level Security](https://supabase.com/docs/guides/database/postgres/row-level-security).

완료 확인: 실습 행을 추가하고 다시 읽고 수정했으며, 삭제 연습용 행만 지웠습니다. 웹 연결 또는 샘플 상태를 구분했습니다.

## 04. vercel-cli: 다른 사람이 열 수 있는 주소 만들기 (20분)

없으면 내 컴퓨터에서는 열리지만 다른 사람에게 보여 줄 주소가 없습니다. Vercel 스킬은 배포뿐 아니라 어느 프로젝트를 바꾸는지, 환경변수를 어디에 두는지, 오류 로그를 어디서 보는지도 안내합니다.

설치와 확인, Hermes를 설치한 터미널:

```bash
hermes -p jiwon skills install vercel/vercel/skills/vercel-cli
hermes -p jiwon skills list
```

목록에서 `vercel-cli`를 확인합니다. Vercel 도구와 로그인이 준비돼 있어야 실제로 배포할 수 있습니다. 수강생은 명령 옵션 대신 배포할 폴더, 프로젝트와 공개 범위를 확인합니다.

핵심 패턴은 세 가지입니다.

1. 프로젝트와 Preview 배포. 배포할 폴더는 01에서 연 link-shelf입니다. Hermes 요청 예시:

```text hermes
/vercel-cli 현재 link-shelf 폴더를 배포하려 합니다. 연결된 계정과 프로젝트, 공개될 파일을 먼저 확인해 주세요. 내가 대상 확인 후 승인하면 Preview로 배포하고 실제 URL를 docs/deploy.md에 남겨 주세요.
```

2. 환경변수. 지금의 정적 HTML에서 publishable 설정을 쓰는 것과 서버 비밀키 보관은 다릅니다. 빌드 도구를 추가한 경우 `VITE_` 같은 공개 접두사는 브라우저에 전달됩니다. 비밀키를 숨기는 기능으로 쓰지 않습니다.

```text hermes
/vercel-cli 이 프로젝트의 설정값이 브라우저에 공개되는지 먼저 구분해 주세요. 필요한 환경변수 이름과 Preview 적용 대상을 정리하고, 값은 출력하지 마세요. 변경 후 재배포가 필요한지도 알려 주세요.
```

3. 로그. 배포 실패는 Build Logs, 배포 후 서버 실행 오류는 Runtime Logs에서 확인합니다. 정적 HTML의 브라우저 오류는 브라우저 콘솔에서 봅니다.

실패한 배포의 실제 URL와 오류 메시지를 먼저 전달한 뒤 요청합니다.

```text hermes
/vercel-cli 방금 전달한 배포 실패를 조사해 주세요. 빌드 단계인지 실행 단계인지 구분하고 해당 로그에서 원인과 다음에 고칠 한 가지를 알려 주세요. 설정은 아직 바꾸지 마세요.
```

5분 미니 실습: `index.html`이 있는 link-shelf 폴더에서 대상을 확인한 뒤 Preview 배포합니다. 단독 모듈이라면 “공개 학습 링크 3개를 보여 주는 index.html을 만들어 주세요”로 시작합니다. 앞의 배포 요청으로 연결된 프로젝트와 공개 파일을 확인하고 승인합니다. 반환된 실제 Preview URL를 직접 엽니다.

기대 결과: 에이전트가 배포 도구에서 받은 실제 Preview URL를 열었을 때 링크 카드가 나타나고 링크 하나가 열립니다. 배포 메시지만 보지 말고 브라우저에서 확인하세요. DB 미연결 샘플이라면 그 상태도 deploy.md에 적습니다.

막히면: 다른 앱이 배포되면 현재 폴더와 `.vercel` 연결부터 확인합니다. Preview에 로그인 화면이 보이면 Deployment Protection과 방문자 접근 권한을 확인합니다. 보호를 무작정 끄지 않습니다. 환경변수 수정은 이미 만들어진 배포에 소급 적용되지 않으므로 새 배포와 해당 URL를 확인합니다.


<details>
<summary>다른 사례로 해보기: 공개 행사 자료실</summary>

```text hermes
/vercel-cli 공개 행사 자료실의 index.html을 Preview로 배포할 대상을 확인해 주세요. 프로젝트와 공개 파일을 먼저 보여 주고, 승인 뒤 배포해 실제 URL를 docs/deploy.md에 남겨 주세요.
```

공개 행사 링크가 표시되는 Preview URL가 남습니다. 수강생에게는 이 실제 주소를 전달합니다.

</details>

다음에 더 찾아볼 것: [Vercel deploy 공식 문서](https://vercel.com/docs/cli/deploy).

완료 확인: 실제 Preview URL를 열어 같은 링크 목록과 링크 열기를 확인하고 deploy.md에 남겼습니다.

## 05. hermes-memory-operations: 기억할 기준을 직접 고르기 (15분)

없으면 매번 같은 선호를 설명하거나, 일회성 요청을 오래 적용할 기준으로 저장할 수 있습니다. 메모리는 “앞으로도 유효한 사실과 선호”, 스킬은 “다시 실행할 절차”를 담습니다.

이 모듈은 강사의 기존 운영 원칙을 공개 교육용으로 정리한 설치형 스킬을 사용합니다. 개인 메모리, 다른 프로필, 외부 메모리 서비스는 다루지 않습니다.

설치와 확인, Hermes를 설치한 터미널:

```bash
hermes -p jiwon skills install kimsanguine/hermes-hemory/skills/hermes-memory-operations
hermes -p jiwon skills list
```

목록에 `hermes-memory-operations`가 있는지 확인하고 새 대화를 엽니다. 이 스킬을 설치해도 메모리 도구가 자동으로 추가되지는 않습니다. 현재 Hermes가 실제 제공하는 메모리 도구와 스키마를 먼저 확인합니다. Claude Code의 메모리 방식과도 다릅니다.

핵심 패턴은 세 가지입니다. 모두 Hermes 대화창의 요청 예시입니다.

```text hermes
/hermes-memory-operations 현재 프로필에 “실습용 선호: 설명 뒤에 예시 한 개를 붙인다”를 기억시키고 싶어요. 메모리 도구가 있는지 확인하고 변경안만 보여 주세요. 기존 메모리 전체는 출력하지 마세요.
```

```text hermes
/hermes-memory-operations 방금 저장한 실습용 선호에서 “예시 한 개”를 “짧은 체크리스트”로 바꾸는 변경안을 보여 주세요. 다른 항목은 수정하지 마세요.
```

```text hermes
/hermes-memory-operations 이번 실습에서 추가한 항목만 점검해 주세요. 오래 보관할 가치가 있는지, 임시 결과인지 설명하고 아직 수정하거나 지우지 마세요.
```

5분 미니 실습: 첫 변경안을 읽고 맞으면 “지금 보여 준 실습용 선호 한 줄을 현재 프로필에 저장해 주세요”라고 승인합니다. 도구의 실제 성공 결과와 재조회를 확인합니다. 두 번째 변경안도 같은 방식으로 승인합니다. 기대 결과는 가상 선호가 한 줄 추가된 뒤 그 한 줄만 바뀐 상태입니다. 저장안이나 에이전트의 약속만 출력됐다면 미저장입니다. 수정 후에는 01의 순서로 새 대화를 열고 다음 요청을 보냅니다.

```text hermes
내가 저장한 설명 선호를 적용해 공개 학습 링크 보관함을 소개해 주세요. 기존 기억 전체를 출력하지 말고 소개 결과만 주세요.
```

짧은 체크리스트가 붙는지 읽습니다. 선호가 적용되지 않으면 저장 상태와 새 세션 적용을 확인합니다. 마지막에 이 실습용 기억을 지울지 직접 결정합니다.

막히면: 용량 초과 시 기존 기억을 자동으로 지우지 말고 중복 항목과 정리안을 먼저 받습니다. 새 세션의 메모리 스냅샷과 현재 저장 상태가 다를 수 있으니 실제 저장 상태를 확인합니다. `/skills approval on`은 스킬 파일 쓰기 승인 설정이며 메모리 변경 승인과는 다릅니다. 메모리 도구가 없다면 [기억 준비 안내](https://teamsparta.habix.ai/teamsparta/10th/setup/hermes-memory)로 돌아가세요. 준비 전에는 변경안만 남기고 실제 저장은 다음으로 미룹니다.


<details>
<summary>다른 사례로 해보기: 발표 준비의 선호</summary>

```text hermes
/hermes-memory-operations “실습용 선호: 발표자료는 한 장에 핵심 세 줄로 쓴다”를 저장할 변경안만 보여 주세요. 현재 프로필을 확인하고 다른 기억은 출력하지 마세요.
```

승인 뒤 그 한 줄만 저장합니다. 다음 발표 요청에서 적용됐는지 읽고, 일회성 발표 내용은 기억에 넣지 않습니다.

</details>

다음에 더 찾아볼 것: [공개 Hermes Memory Operations 저장소](https://github.com/kimsanguine/hermes-hemory).

완료 확인: 실습용 선호 한 줄의 추가와 수정을 도구 결과로 확인했거나, 저장을 막는 구체적인 원인을 남겼습니다.

## 06. hyperframes-cli: 프로젝트를 소개하는 짧은 영상 만들기 (15분)

없으면 화면을 설명하는 영상도 편집 도구에서 처음부터 만들어야 합니다. HyperFrames는 HTML로 장면을 구성하고 타임라인의 특정 시점을 확인한 뒤 영상 파일로 렌더합니다. 웹앱 배포와 영상 퍼블리시는 서로 다른 결과물입니다.

설치 확인, 터미널:

```bash
hermes -p jiwon skills list
```

최초 설치를 준비할 때는 아래 저장소를 미리 읽습니다. 검사 결과가 차단이면 설치를 진행하지 말고 아래 대체 요청을 사용하세요.

```bash
hermes -p jiwon skills inspect heygen-com/hyperframes/skills/hyperframes-cli
```

검사 결과를 읽고 설치를 진행할 경우의 명령입니다.

```bash
hermes -p jiwon skills install heygen-com/hyperframes/skills/hyperframes-cli
```

목록에서 `hyperframes-cli`를 확인합니다. 설치 검사에서 환경변수 일괄 출력이나 경로 접근 등으로 차단되면 멈추고 안내를 읽습니다. `--force`로 우회하지 않습니다. 설치가 검토된 기존 환경은 스킬을 사용하고, 미설치자는 아래 대체 요청으로 공식 도구의 사용 가능 여부를 먼저 확인합니다. Node.js, 브라우저, FFmpeg는 별도 준비입니다.

핵심 패턴은 스캐폴딩, 프레임과 렌더, 퍼블리시 세 가지입니다. 앞의 웹앱 없이도 01에서 만든 빈 폴더와 소개할 문구로 시작할 수 있습니다.

1. 스캐폴딩. 에이전트가 필요한 폴더와 기본 파일을 만들게 합니다.

```text hermes
/hyperframes-cli 현재 프로젝트 안에 promo라는 새 영상 스캐폴딩을 만들어 주세요. 6초 영상의 기본 파일과 장면별 시작 시각, 길이, 실제 폴더 경로를 알려 주세요. 같은 폴더가 이미 있으면 덮어쓰지 마세요.
```

2. 프레임과 렌더. 먼저 장면을 보고, 확인한 장면을 영상으로 만듭니다.

```text hermes
/hyperframes-cli promo에서 0초, 2초, 4초 장면을 PNG로 남겨 주세요. 각 파일의 실제 경로와 보이는 문구를 알려 주세요. 아직 MP4로 만들지 마세요.
```

PNG를 열어 문구와 잘림을 확인한 다음 렌더를 요청합니다.

```text hermes
/hyperframes-cli 방금 확인한 promo를 promo.mp4로 렌더해 주세요. 실제 생성한 파일 경로와 재생 길이를 알려 주세요. 아직 업로드하지 마세요.
```

3. 퍼블리시, 선택. 계정과 공개 범위를 확인한 뒤 스킬에 요청합니다.

```text hermes
/hyperframes-cli 이 소개 영상을 웹 링크로 공유하려 합니다. 현재 프로젝트, 계정, 비공개와 공개의 차이를 먼저 알려 주세요. 아직 업로드하지 마세요.
```

5분 미니 실습, 이미 스킬이 설치된 환경의 요청 예시:

```text hermes
/hyperframes-cli promo의 기존 스캐폴딩을 유지하고 “링크를 찾고, 저장하고, 공유합니다”를 보여 주는 6초 소개 영상으로 바꿔 주세요. 0초, 2초, 4초의 프레임을 확인하고 파일 경로를 알려 주세요. 아직 퍼블리시하지 마세요.
```

기대 결과는 서로 다른 세 장면의 PNG와 실제 경로입니다. 문구가 잘 보이는지 읽고 MP4 렌더로 이어갑니다.

스킬 설치가 차단됐다면 스킬 호출 대신 아래 요청을 사용합니다. 공식 도구를 직접 사용하는 경로이며 스킬 설치 성공으로 기록하지 않습니다.

```text hermes
HyperFrames 공식 문서를 기준으로 현재 컴퓨터에서 새 영상 스캐폴딩과 장면 PNG를 만들 수 있는지 확인해 주세요. 필요한 도구가 빠졌으면 무엇이 없는지 먼저 알려 주세요. 새 설치나 공개 업로드는 아직 하지 마세요.
```

필요한 준비가 끝나고 도구를 사용할 수 있다면 다음 요청으로 새 프로젝트를 만듭니다.

```text hermes
HyperFrames 공식 도구로 현재 프로젝트 안에 promo를 새로 만들어 주세요. “링크를 찾고, 저장하고, 공유합니다”를 6초 장면으로 나누고 시작 시각과 길이, 실제 폴더 경로를 알려 주세요. 같은 폴더가 있으면 덮어쓰지 마세요.
```

장면별 시작 시각과 길이가 총 6초인지 읽은 뒤 프레임을 만듭니다. 스킬 설치 환경에서도 앞서 받은 시간표를 같은 기준으로 확인합니다.

```text hermes
HyperFrames 공식 도구로 방금 만든 promo의 0초, 2초, 4초 장면을 PNG로 저장해 주세요. 실제 파일 경로를 알려 주세요. 새 설치가 필요하면 멈추고 알려 주고, 아직 MP4 렌더나 공개 업로드는 하지 마세요.
```

마치면 에이전트에게 현재 작업 폴더가 원래 프로젝트 폴더인지 확인하게 합니다.

막히면: 렌더 오류 메시지를 전달하고 브라우저나 FFmpeg 등 필요한 도구가 빠졌는지 설명해 달라고 요청합니다. [HyperFrames CLI 준비 안내](https://github.com/heygen-com/hyperframes/blob/main/docs/developers/cli.mdx)에서 필요한 구성 요소를 확인합니다. HTML 파일을 열었다고 타임라인 재생이 검증된 것은 아닙니다. 정지 프레임뿐 아니라 시간에 따라 내용이 바뀌는지 확인합니다.


<details>
<summary>다른 사례로 해보기: 공개 행사 자료실 소개</summary>

```text hermes
/hyperframes-cli promo 스캐폴딩으로 “행사 안내, 다시보기, 발표자료”를 보여 주는 6초 영상을 만들어 주세요. 0초, 2초, 4초 PNG를 남기고 아직 업로드하지 마세요.
```

스킬 설치 환경에서는 세 장면을 확인합니다. 미설치자는 공식 도구로 만든 기본 프레임까지만 기록하고 설치 성공과 구분합니다.

</details>

다음에 더 찾아볼 것: [HyperFrames CLI 공식 가이드](https://github.com/heygen-com/hyperframes/blob/main/docs/developers/cli.mdx).

완료 확인: 스캐폴딩, 타임라인과 실제 PNG를 확인했습니다. 스킬 사용, 공식 도구 직접 사용, 렌더 여부를 구분했습니다.

## 07. reverse-engineer-anything: 관찰과 추정을 구분해 읽기 (심화 선택, 10분)

없으면 화면이 비슷하다는 이유로 내부 동작도 같다고 추정하기 쉽습니다. 이 스킬은 REA 도구로 앱과 코드의 근거를 수집해, 관찰한 사실과 추정과 아직 모르는 것을 구분합니다.

설치와 확인, Hermes를 설치한 터미널:

```bash
hermes -p jiwon skills install morluto/rea/.agents/skills/reverse-engineer-anything
hermes -p jiwon skills list
```

목록에서 `reverse-engineer-anything`을 확인합니다. 스킬 설치는 REA 실행 도구나 MCP 등록과 다릅니다. 기본 선택 실습은 자신이 만든 link-shelf 코드의 읽기이며, 바이너리 분석 도구는 설치하지 않습니다.

핵심 패턴은 두 가지입니다.

```text hermes
/reverse-engineer-anything 내가 만든 link-shelf 폴더에서 링크 카드를 읽고 URL를 여는 경로를 분석해 주세요. REA 도구가 있는지 먼저 확인하고, 실제 코드 근거와 추정, 미확인을 docs/reverse-notes.md에 구분해 주세요. 코드는 실행하거나 수정하지 마세요.
```

```text hermes
/reverse-engineer-anything 내가 제공한 공개 오픈소스 예제와 이 앱의 링크 열기 방식을 비교해 주세요. 파일과 함수 근거가 있는 차이만 적고 서버 내부 동작은 추정하지 마세요.
```

5분 미니 실습: 첫 요청을 실행해 링크 카드에서 URL가 어디서 오는지 따라갑니다. 기대 결과는 진입 파일, 이벤트 또는 링크 요소, 데이터 출처, 확인하지 못한 범위가 적힌 문서입니다. REA 도구가 없다면 “일반 코드 읽기만 가능, REA 분석 미실시”로 표시하고 이 모듈을 종료합니다. 존재하지 않는 분석 도구 호출을 만들지 않습니다.

막히면: 접근 가능한 공개 화면만으로 서버 코드나 DB 구조를 안다고 쓰지 않습니다. 남이 만든 앱을 분석하려면 허가받은 코드나 공개 라이선스 예제를 직접 지정하세요. 로그인 우회나 비공개 데이터 추출을 이 실습에 포함하지 않습니다.


<details>
<summary>다른 사례로 해보기: 다른 사람이 만든 공개 예제</summary>

```text hermes
/reverse-engineer-anything 내가 제공한 공개 예제의 라이선스와 진입 파일부터 확인해 주세요. 링크 클릭이 데이터와 연결되는 경로만 읽고 근거 파일과 추정, 미확인을 나눠 주세요. 실행하거나 수정하지 마세요.
```

공개 예제의 실제 URL 또는 폴더를 먼저 제공합니다. 원문 코드를 보지 못하면 서버와 DB의 동작은 미확인으로 남깁니다.

</details>

다음에 더 찾아볼 것: [REA 공식 저장소](https://github.com/morluto/rea).

완료 확인: 분석한 파일과 코드 근거, 추정, 미확인을 reverse-notes.md에 구분했습니다.

## 08. 스킬 세 개 이상으로 프로젝트 마치기 (30분)

“공개 학습 링크를 새로고침해도 다시 볼 수 있고, 다른 사람이 URL로 열 수 있다”가 기본 목표입니다. 아래 세 단계에서 context7-cli, supabase, vercel-cli를 연결합니다. DB가 준비되지 않았다면 샘플 화면 배포와 링크 열기까지 마치고 DB 연결은 미완료로 표시하세요.

| 단계 | 입력과 요청 | 남길 결과 |
|---|---|---|
| 1. 문서, 5분 | 02 Context7의 요청으로 사용 API와 출처 확인 | library-notes.md |
| 2. 저장과 화면, 15분 | 승인한 공개 읽기 계획으로 링크 3개를 등록하고 index.html에서 조회 | DB의 3개 행과 실제 카드 목록 |
| 3. 공유, 10분 | 대상 폴더와 프로젝트를 확인하고 Preview 배포 | deploy.md의 실제 URL |

에이전트에게 한꺼번에 전부 맡기지 말고 아래 첫 요청으로 시작합니다.

```text hermes
공개 학습 링크 보관함의 현재 파일과 DB 연결 상태만 확인해 주세요. 샘플과 실제 연결을 구분하고, 문서, DB와 화면, Preview 배포 중 다음에 할 한 단계와 필요한 입력을 제안하세요. DB 변경이나 배포는 아직 하지 마세요.
```

완료 확인은 다섯 가지입니다. 실제 문서 출처가 남았는가, DB를 다시 읽어도 같은 링크가 나오는가, Preview URL에서 같은 목록이 보이는가, 링크 하나가 열리는가, 다시 실행할 때 필요한 순서가 docs에 남았는가. 배포 주소만 있고 사용 결과가 없으면 마지막 단계가 남은 상태입니다.

메모리와 영상 모듈도 진행했다면 저장한 선호와 실제 생성한 프레임을 프로젝트와 함께 정리하세요. 심화 선택 모듈까지 참여했다면 코드 분석에서 배운 한 가지를 설명합니다. 모든 스킬을 억지로 한 요청에 넣을 필요는 없습니다.

제출물: 실제 Preview URL와 docs 폴더. 키와 로그인 정보, 개인 메모리 파일은 제출하지 않습니다. 설치 확인, 실제 DB 연결, 배포와 영상 렌더의 상태를 각각 적으면 다음 실습의 출발점을 알 수 있습니다.

## 용어집

| 용어 | 쉬운 뜻 |
|---|---|
| Skill | 특정 업무에서 읽을 절차와 판단 기준 |
| builtin | Hermes가 기본 제공하는 스킬 |
| CLI | 터미널에서 명령으로 도구를 사용하는 방식 |
| CRUD | 추가, 조회, 수정, 삭제 |
| Auth | 사용자가 누구인지 확인하는 기능 |
| RLS | 어떤 사용자가 어느 행을 읽고 쓸지 정하는 규칙 |
| Edge Function | 서버에서 실행하는 함수 |
| Preview | 운영 반영 전 따로 확인하는 배포 |
| Render | 장면을 실제 영상 파일로 만드는 작업 |

## 출처

- [Hermes Skills](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills)
- [공개 메모리 운영 스킬](https://github.com/kimsanguine/hermes-hemory)
- [Context7 CLI](https://github.com/upstash/context7/blob/master/docs/clients/cli.mdx)
- [Supabase RLS](https://supabase.com/docs/guides/database/postgres/row-level-security)
- [Supabase Edge Functions](https://supabase.com/docs/guides/functions/quickstart-dashboard)
- [Vercel deploy](https://vercel.com/docs/cli/deploy)
- [HyperFrames CLI](https://github.com/heygen-com/hyperframes/blob/main/docs/developers/cli.mdx)
- [REA](https://github.com/morluto/rea)
