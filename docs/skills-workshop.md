# Hermes 스킬로 조사부터 웹 배포까지 맡기기

비개발자와 초급 개발자 | 웹 프로젝트 105분, 메모리와 영상 30분, 심화 선택 10분, 휴식 10분

오늘은 팀원이 내 프로젝트를 써 보고 의견을 남기는 “프로젝트 피드백 보드”를 만듭니다. 오류와 개선 제안을 DB에 저장하고, 팀원에게 실제 웹 주소를 공유합니다. 스킬별 실습은 독립적으로 할 수 있고, 앞의 결과물이 있으면 이어서 사용합니다.

## 01. 스킬을 고르고 실습 준비하기

에이전트에게 “앱을 만들어 줘”라고만 요청하면 문서 버전, 데이터 접근, 배포 대상, 확인 방법까지 매번 설명해야 합니다. 스킬은 그 업무를 처리할 순서와 판단 기준을 묶어 필요할 때 읽게 합니다. 설치만으로 계정 권한이나 실행 도구가 생기지는 않습니다. Supabase 스킬을 읽는 것과 내 DB에 연결하는 것은 별개의 준비입니다.

| 구분 | 뜻 | 오늘의 예 |
|---|---|---|
| builtin | Hermes가 기본 제공하는 스킬 | 전체 스킬 목록의 Source가 builtin인 항목 |
| 설치형 | 공식 저장소나 검토한 공개 저장소에서 추가하는 스킬 | `context7-cli`, `supabase`, `vercel-cli`, `hyperframes-cli`, `reverse-engineer-anything` |
| 직접 만든 스킬 | 자신의 반복 업무 기준을 SKILL.md로 정리한 것. 공개하면 다른 사람도 설치 가능 | `hermes-hermory` |

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

### 사용 중인 Hermes에서 시작하기

현재 프로젝트의 터미널에서 수업 프로필로 대화를 엽니다. 이미 같은 프로필의 대화창을 열었다면 그대로 사용하세요. 진행 중인 코드가 없으면 피드백 보드 예제로 시작하고, 새 파일의 저장 위치는 Hermes가 제안한 경로를 확인합니다.

```bash
hermes -p jiwon chat
```

Hermes 대화창에서 상태를 봅니다.

```text hermes
/status
```

`Path:`가 사용할 프로필인지, `Model:`이 사용할 모델인지 확인합니다. `docs/...`는 현재 프로젝트에 남길 문서 위치입니다. 저장에 필요한 하위 폴더는 Hermes가 만들게 하고, 같은 이름의 파일이 있으면 변경 전에 확인합니다. 새 대화가 필요한 단계에서는 `/exit`로 나간 뒤 같은 터미널에서 `hermes -p jiwon chat`으로 다시 엽니다.

확인용 터미널에서 전체 스킬을 먼저 봅니다. 각 모듈에서도 이 목록에서 해당 이름과 `enabled`를 확인합니다.

```bash
hermes -p jiwon skills list
```

`skills_list`는 에이전트 내부 도구 이름입니다. 대화창에서 “skills_list로 현재 스킬을 확인해 주세요”라고 요청합니다. `/스킬이름 요청`은 대화창에, `hermes ...`는 터미널에 입력합니다.

설치 시 출처와 검사 결과를 읽고 승인합니다. 같은 이름이 있으면 재설치하지 않고, 설치 후 새 대화를 엽니다. `--source local`은 전체 설치 확인에 쓰지 않습니다. `--force`로 검사나 충돌을 우회하지 않습니다.

실습하면서 현재 프로젝트에 다음 결과를 남깁니다. 각 모듈에서 필요한 결과만 만드세요.

| 결과 | 저장 위치 |
|---|---|
| 폼 저장과 목록 조회에 사용할 API | `docs/library-notes.md` |
| 테이블과 접근 정책 | `docs/db-plan.md` |
| 피드백 입력 폼과 목록 | `index.html` |
| 실제 배포 URL와 확인 결과 | `docs/deploy.md` |
| 팀원용 시연 안내 | `docs/demo.md` |
| 선택한 소개 영상 | `promo/` |
| 선택한 코드 분석 | `docs/reverse-notes.md` |

기본 사례는 프로젝트 피드백 보드, 두 번째 사례는 스터디 질문함입니다. 이미 만드는 서비스가 있다면 같은 요청에 자신의 화면과 데이터를 넣어도 됩니다. 공개 실습에는 아래 가상 의견만 사용하고 이름, 연락처와 고객 자료는 넣지 않습니다.

## 02. context7-cli: 폼 저장에 필요한 최신 API 찾기 (15분)

모델이 기억하는 예전 API로 폼을 만들면 버튼은 있지만 저장이 되지 않을 수 있습니다. Context7은 색인된 문서를 가져오는 스킬입니다. 문서를 많이 받기보다 이번 기능에 쓸 호출과 오류 처리까지 골라 코드에 연결합니다. 새 프로젝트는 사용할 v2 문서를 찾고, 이미 패키지가 있으면 실제 설치 버전과 맞춥니다.

설치와 확인, 터미널:

```bash
hermes -p jiwon skills install upstash/context7/skills/context7-cli
hermes -p jiwon skills list
```

목록에서 `context7-cli`를 확인합니다. 핵심 패턴은 새 기능에 필요한 문서 찾기와 기존 코드 수정입니다.

5분 미니 실습, Hermes 대화창:

```text hermes
/context7-cli Supabase JavaScript v2로 피드백을 저장하고 최신순으로 읽으려 해요. insert, select, order와 실패 처리의 공식 예제를 찾아 docs/library-notes.md에 사용할 코드와 출처를 정리해 주세요. 기존 패키지가 있다면 버전을 맞춰 주세요.
```

기대 결과는 “어떤 문서를 읽었는가” 목록이 아니라 다음 구현에서 가져다 쓸 호출입니다. 아래는 결과 형식의 예시이며 실제 조회 결과는 자신의 문서에서 확인합니다.

```text output
저장: feedbacks에 category, content를 insert
목록: select 뒤 created_at 내림차순으로 정렬
실패: 반환한 error를 확인하고 입력한 내용을 유지
주의: insert는 기본적으로 저장한 행을 반환하지 않음
출처: 확인한 공식 문서 URL
```

화면이 이미 있다면 아래 요청으로 고칠 코드를 받습니다.

```text hermes
/context7-cli 현재 피드백 저장 코드를 공식 문서와 대조해 고쳐 주세요. 저장이 성공하면 목록을 다시 읽고, 실패하면 입력을 유지한 채 이유를 보여 주게 해 주세요. 바꾼 파일과 사용한 문서 URL도 알려 주세요.
```

막히면: 라이브러리를 찾지 못하면 `Supabase JavaScript v2`와 만들 기능을 함께 말합니다. 요청 제한은 안내된 시간 뒤 재시도합니다. 출처가 없으면 문서 URL를 요구합니다. 버전을 확인할 패키지가 없으면 미설치로 적고 코드를 만든 뒤 대조합니다.

<details>
<summary>다른 사례: 스터디 질문함</summary>

```text hermes
/context7-cli Supabase JavaScript v2로 질문을 등록하고 미답변 질문만 읽는 예제를 찾아 주세요. 질문 내용과 answered 값을 쓸 예정이에요. 실제 호출과 오류 처리, 출처를 docs/library-notes.md에 남겨 주세요.
```

</details>

다음에 더 찾아볼 것: [Context7 CLI 공식 문서](https://github.com/upstash/context7/blob/master/docs/clients/cli.mdx).

완료 확인: 자신의 기능에 쓸 호출과 오류 처리, 출처, 버전 상태가 남았습니다.

## 03. supabase: 팀원의 피드백을 실제 DB에 저장하기 (25분)

화면의 배열에만 넣으면 새로고침할 때 의견이 사라집니다. Supabase의 Database에 저장하면 다시 읽을 수 있고, Auth는 사용자를 구분하며, Edge Functions는 서버 처리가 필요할 때 씁니다. 스킬 설치와 내 계정 연결은 별개입니다. 기본 실습은 Dashboard와 브라우저용 publishable key로 진행합니다.

설치와 확인, 터미널:

```bash
hermes -p jiwon skills install supabase/agent-skills/skills/supabase
hermes -p jiwon skills list
```

목록에서 `supabase`를 확인합니다. 기본은 공개 실습용 의견을 읽고 등록하는 보드입니다. 수정과 삭제는 내가 Dashboard에서 합니다. 실제 고객 의견을 익명 공개 보드에 넣지 않습니다.

| 필드 | 용도 |
|---|---|
| id | 행 식별자, 자동 생성 |
| category | 오류, 개선, 칭찬 중 하나 |
| content | 의견 내용, 1자부터 300자까지 |
| status | 기본값 미처리, Dashboard에서 완료로 변경 |
| created_at | 등록 시각, 자동 생성 |

핵심 패턴은 DB와 CRUD, Auth, Edge Function 세 가지입니다. 먼저 DB를 연결합니다.

```text hermes
/supabase feedbacks 생성 SQL을 docs/db-plan.md에 써 주세요. id는 자동 UUID, category는 오류·개선·칭찬, content는 앞뒤 공백 제거 후 1자부터 300자, status는 미처리 기본값, created_at은 자동 시각입니다. DB 제약도 넣고 같은 테이블이 있으면 멈추세요. 아직 실행하지 마세요.
```

이어서 공개 실습 범위의 권한을 같은 SQL에 반영합니다.

```text hermes
/supabase docs/db-plan.md에 권한과 RLS를 추가해 주세요. 기본 테이블 쓰기 권한을 회수하고 공개 SELECT와 INSERT(category,content)만 허용하세요. 등록 시 status는 미처리여야 하며 익명 수정·삭제는 금지합니다. 아직 실행하지 마세요.
```

Dashboard 상단의 프로젝트가 내 실습 계정인지 확인하고 SQL의 변경 범위를 읽습니다. 승인한 SQL만 SQL Editor → New query에서 실행합니다. 기존 테이블을 지우거나 RLS를 끄는 SQL이면 실행하지 말고 다시 요청하세요. Table Editor에서 `feedbacks`를 열고 가상 의견을 넣습니다.

| category | content | status |
|---|---|---|
| 오류 | 모바일에서 등록 버튼이 키보드에 가려져요 | 미처리 |
| 개선 | 오류와 개선 의견을 따로 보고 싶어요 | 미처리 |
| 칭찬 | 팀원 의견을 한 화면에서 보니 편해요 | 완료 |

같은 프로젝트의 Connect 또는 API 설정에서 Project URL와 publishable key를 확인해 대화에 전달합니다. secret key와 service_role 키는 전달하지 않습니다. 그다음 요청합니다.

```text hermes
방금 전달한 공개 연결값으로 feedbacks 입력 폼과 목록을 index.html에 만들어 주세요. 유형과 내용을 받아 저장하고 새로고침해도 남게 해 주세요. 목록에 처리 상태도 표시하고 빈 내용과 300자 초과는 막아 주세요. 저장 실패 시 입력을 유지하세요. 기존 파일은 변경안을 먼저 보여 주세요.
```

화면 파일을 만든 뒤 로컬 실행을 요청합니다.

```text hermes
현재 피드백 보드를 브라우저에서 열 수 있게 로컬 서버를 실행해 주세요. 실행 중인 서버가 있으면 재사용하고, 제가 열 실제 주소를 알려 주세요. 새 패키지 설치가 필요하면 먼저 알려 주세요.
```

5분 미니 실습: 방금 받은 실제 로컬 주소를 엽니다. 가상 의견 “등록 후 입력창이 비워지면 좋겠어요”를 등록한 뒤 새로고침하고 Table Editor에서도 같은 행을 찾습니다. Dashboard에서 그 행을 완료로 바꾸고 웹 목록을 새로 읽습니다. 별도로 만든 `삭제 연습` 행만 지우면 추가·조회·수정·삭제를 모두 경험합니다.

등록이 되면 입력 제한도 확인합니다. 다음 요청은 현재 실습 테이블만 대상으로 합니다.

```text hermes
publishable key의 anon 권한으로 feedbacks의 공백·301자 등록, status·id·created_at 지정 등록, 수정·삭제가 거부되는지 확인해 주세요. “삭제 연습” 행만 대상으로 하고 결과는 docs/db-plan.md에 남겨 주세요.
```

기대 결과는 “저장했습니다”라는 문구와 DB의 행이 일치하는 것입니다. DB가 준비되지 않았다면 가상 의견으로 화면만 만들고 `샘플 데이터`를 표시합니다.

Auth 선택 확장에서는 로그인한 사람의 의견만 보이도록 바꿉니다.

```text hermes
/supabase 피드백 보드를 작성자별 비공개로 바꾸려 해요. 이메일 로그인과 user_id, 본인 의견만 읽고 수정하는 정책을 제안해 주세요. 현재 공개 정책에서 바뀔 부분을 먼저 보여 주고 아직 적용하지 마세요.
```

Edge Function 선택 확장에서는 브라우저 밖에서도 같은 입력 기준을 지킵니다.

```text hermes
/supabase 피드백 본문의 공백과 300자 제한을 서버에서도 검사하는 Edge Function 초안을 써 주세요. 빈 내용은 400, 정상 내용은 검사 통과로 응답하게 해 주세요. Dashboard에서 실행할 테스트 입력도 주고 아직 배포하지 마세요.
```

막히면: 조회가 안 되면 SELECT 권한과 RLS를, 등록만 안 되면 INSERT 컬럼 권한과 정책을 함께 확인합니다. publishable key 자체가 권한을 통제하지는 않습니다. 실제 서비스 공개 전에는 로그인과 등록 남용 방지를 설계해야 합니다. Auth와 Edge Function은 기본 완주 뒤 확장합니다.

<details>
<summary>다른 사례: 스터디 질문함</summary>

```text hermes
/supabase 스터디 질문함에 question, answered, created_at이 필요해요. 공개 질문 등록·조회와 Dashboard의 답변 완료 처리를 위한 SQL과 권한을 제안해 주세요. 가상 질문 3개도 주고 실행 전 변경안을 보여 주세요.
```

</details>

다음에 더 찾아볼 것: [Supabase RLS](https://supabase.com/docs/guides/database/postgres/row-level-security).

완료 확인: 웹에서 등록한 행이 DB에 있고 새로고침 후에도 보입니다. 수정과 삭제는 실습 행만 대상으로 했습니다.

## 04. vercel-cli: 팀원이 써 볼 수 있는 웹 주소 만들기 (20분)

로컬 화면을 캡처해서 보내면 팀원은 직접 써 볼 수 없습니다. Vercel 스킬로 현재 프로젝트를 Preview에 배포하고 실제 주소를 공유합니다. 도구와 계정 로그인이 먼저 준비돼 있어야 합니다. Preview는 운영 배포와 구분되는 별도 주소입니다.

설치와 확인, 터미널:

```bash
hermes -p jiwon skills install vercel/vercel/skills/vercel-cli
hermes -p jiwon skills list
```

목록에서 `vercel-cli`를 확인합니다. 핵심 패턴은 배포, 환경변수, 오류 복구 세 가지입니다.

단독 모듈이라 화면이 없다면 03의 샘플 데이터 경로로 입력 폼과 목록을 먼저 만듭니다. 샘플 화면도 Preview로 공유할 수 있습니다.

5분 미니 실습, 배포할 현재 프로젝트에서 요청합니다.

```text hermes
/vercel-cli 팀원이 피드백 보드를 써 볼 수 있게 현재 프로젝트를 Preview로 배포해 주세요. 계정·대상 프로젝트·공개 파일을 먼저 보여 주고 제 승인 뒤 진행하세요. 실제 URL와 배포 결과는 docs/deploy.md에 남겨 주세요.
```

대상을 확인하고 승인한 뒤, 반환한 실제 URL를 시크릿 창이나 다른 기기에서 엽니다. 가상 의견 하나를 등록하고 새로고침해 같은 의견이 남는지 봅니다. Deployment Protection이 있다면 팀원에게 필요한 접근 방법을 확인하세요. 공유를 위해 보호 설정을 무작정 끄지 않습니다.

기대 결과 예시입니다. 주소는 자신의 배포가 반환한 실제 값이어야 합니다.

```text output
확인할 주소: 도구가 반환한 실제 Preview URL
다른 창에서 피드백 등록 → 새로고침 후 같은 의견 확인
DB 상태: 연결 또는 샘플 중 실제 상태 기록
남은 준비: 방문자 접근 권한이 필요한지 확인
```

환경변수는 현재 코드가 그 값을 읽는 방식과 함께 다룹니다. 정적 HTML에 publishable 설정을 넣는 것과 서버 비밀키 보관은 다릅니다. `VITE_` 같은 공개 접두사는 브라우저에 전달됩니다.

```text hermes
/vercel-cli 로컬에서는 열리는데 Preview에서 DB 연결이 실패해요. 현재 코드의 설정 읽는 방식을 조사하고 Preview에 빠진 설정만 찾아 주세요. 값은 출력하지 말고 수정안을 보여 주세요. 승인 후 바꾸고 새 배포 주소에서 다시 확인해 주세요.
```

실제 배포 URL와 실패 메시지를 먼저 전달한 뒤 로그를 읽게 합니다.

```text hermes
/vercel-cli 방금 전달한 배포의 실패 로그에서 가장 먼저 고칠 원인을 찾아 주세요. 현재 코드의 해당 부분만 수정안을 보여 주고, 승인 후 고쳐 새 Preview로 배포해 주세요. 원인과 변경 파일, 실제 URL를 남겨 주세요.
```

막히면: 다른 앱이 나오면 현재 프로젝트와 `.vercel` 연결을 확인합니다. Build Logs는 빌드 실패, Runtime Logs는 서버 실행 오류를 조사할 때 씁니다. 정적 HTML의 브라우저 오류는 콘솔에서 봅니다. 환경변수 수정은 기존 배포에 소급 적용되지 않으므로 새 배포를 확인합니다.

<details>
<summary>다른 사례: 스터디 질문함</summary>

```text hermes
/vercel-cli 지금 만든 스터디 질문함을 Preview로 배포해 주세요. 공개할 파일과 연결된 프로젝트를 먼저 보여 주고 제 승인 뒤 진행하세요. 실제 주소와 다른 창에서 질문 등록을 확인할 순서를 남겨 주세요.
```

</details>

다음에 더 찾아볼 것: [Vercel deploy](https://vercel.com/docs/cli/deploy).

완료 확인: 다른 창에서 실제 URL를 열어 의견 등록과 재조회를 확인했습니다. 샘플이면 DB 연결 완료로 기록하지 않았습니다.

## 05. hermes-hermory: 매번 말하던 작업 원칙 기억시키기 (15분)

에이전트가 새 대화마다 내 기준을 잊으면 수정 요청을 할 때마다 다시 설명해야 합니다. 메모리는 여러 작업에 계속 쓸 사실과 선호를 담고, 스킬은 반복할 절차를 담습니다. 오늘 저장할 것은 일회성 피드백이 아니라 “어떤 수정부터 처리할 것인가”라는 기준입니다.

설치와 확인, 터미널:

```bash
hermes -p jiwon skills install kimsanguine/hermes-hermory/skills/hermes-hermory
hermes -p jiwon skills list
```

목록에서 `hermes-hermory`를 확인합니다. 이 스킬은 현재 프로필의 메모리 도구를 사용하며 별도 메모리 서비스를 설치하지 않습니다. 실제 도구가 없으면 [메모리 설정](https://teamsparta.habix.ai/teamsparta/10th/setup/hermes-memory)으로 돌아갑니다.

핵심 패턴은 기준 저장, 기준 변경, 기억 점검입니다. 아래 기준은 수업용 예시입니다. 자신의 기준이 다르면 그 문장을 바꿔 연습하세요.

```text hermes
/hermes-hermory 앞으로 수정안을 제안할 때 “사용을 막는 오류부터 해결하고 색과 간격은 나중에 다룬다”를 제 작업 원칙으로 기억하고 싶어요. 현재 프로필의 메모리 도구를 확인하고 저장할 변경안을 보여 주세요. 다른 기억은 출력하지 마세요.
```

변경안이 맞으면 다음 요청으로 저장합니다.

```text hermes
방금 제안한 작업 원칙 한 줄을 저장하는 데 동의합니다. 현재 프로필에 그 한 줄만 저장하고 실제 저장값을 다시 읽어 결과를 알려 주세요.
```

5분 미니 실습: 저장 후 `/exit`로 나가 같은 프로필의 새 대화를 엽니다. 앞 대화의 문장을 다시 붙이지 않고 다음을 요청합니다.

```text hermes
피드백 보드에 “모바일에서 등록 버튼이 안 눌려요”와 “카드 색이 밋밋해요”라는 의견이 왔어요. 제가 저장한 작업 원칙을 적용해 무엇부터 고칠지 정해 주세요. 적용한 원칙 한 줄과 다음 행동을 함께 알려 주세요.
```

기대 결과는 저장한 기준을 근거로 등록 오류를 먼저 처리하는 제안입니다. 기준이 안 보인다면 저장한 프로필과 새 대화의 프로필을 비교하고, 실제 저장 상태를 다시 읽게 하세요. 이전 대화가 기억에 남았다는 말만으로 저장 완료를 판단하지 않습니다.

업무 기준이 바뀌었다면 전체 메모리를 새로 쓰지 말고 해당 항목만 수정합니다.

```text hermes
/hermes-hermory 방금 저장한 수정 우선순위를 “사용을 막는 오류, 접근성 문제, 색과 간격 순으로 처리한다”로 바꿀 변경안을 보여 주세요. 기존 한 항목만 바꾸고 다른 기준은 유지해 주세요.
```

승인 뒤 실제 수정값을 확인합니다. 기억 점검은 읽기 전용으로 시작합니다.

```text hermes
/hermes-hermory 이번에 저장한 작업 원칙만 점검해 주세요. 반복해서 쓸 기준인지, 특정 프로젝트의 임시 할 일인지 구분해 주세요. 수정이나 삭제가 필요하면 이유와 변경안을 주고 아직 반영하지 마세요.
```

막히면: 용량이 부족하면 도구가 보고한 예산과 중복 후보를 확인하고, 승인한 항목만 정리합니다. 고정 글자 수를 가정하지 않습니다. API 키, 고객 자료와 오늘의 할 일을 오래 기억할 기준으로 넣지 않습니다. 연습 문장을 더 쓰지 않을 때는 해당 한 줄의 삭제안을 검토하고 직접 승인합니다.

<details>
<summary>다른 사례: 업무 보고 기준 기억시키기</summary>

```text hermes
/hermes-hermory 업무 보고를 만들 때 “결정 사항, 담당자, 기한을 포함하고 모르는 값은 미정으로 둔다”를 기억하고 싶어요. 저장할 변경안을 보여 주세요. 고객 정보나 기존 기억은 출력하지 마세요.
```

승인 후 새 대화에 “금요일까지 안내문 수정, 담당자 미정”을 주고 보고를 요청합니다. 담당자를 만들어 채우지 않는지 확인하세요.

</details>

다음에 더 찾아볼 것: [hermes-hermory 공개 스킬](https://github.com/kimsanguine/hermes-hermory).

완료 확인: 내 작업 원칙을 저장하고 새 대화의 판단에 적용했습니다. 변경했다면 해당 한 줄의 실제 수정값도 확인했습니다.

## 06. hyperframes-cli: 팀원에게 보여 줄 서비스 소개 영상 만들기 (15분)

완성한 웹앱을 소개하려면 여러 화면을 설명해야 합니다. HyperFrames는 HTML 장면을 시간표로 구성해 특정 시점의 프레임과 영상 파일로 만듭니다. 웹앱 배포와 영상 퍼블리시는 서로 다른 결과입니다. 이번 결과는 “무엇이 편해지는가”를 보여 주는 6초 소개 영상입니다.

터미널에서 목록과 설치 전 검사를 확인합니다.

```bash
hermes -p jiwon skills list
hermes -p jiwon skills inspect heygen-com/hyperframes/skills/hyperframes-cli
```

검사 결과를 읽고 설치할 경우의 명령입니다. 차단이면 강제로 설치하지 않고 아래 대안을 사용합니다.

```bash
hermes -p jiwon skills install heygen-com/hyperframes/skills/hyperframes-cli
```

목록에서 `hyperframes-cli`를 확인합니다. Node.js와 브라우저가 필요하며 MP4 렌더에는 FFmpeg 준비도 확인합니다. 핵심 패턴은 장면 구성, 프레임과 렌더, 공유입니다.

먼저 현재 프로젝트의 피드백 보드 화면을 캡처해 대화에 첨부합니다. 웹앱이 없다면 첫 요청의 “첨부 화면”을 “피드백 입력 폼의 모형 화면”으로 바꿉니다.

```text hermes
/hyperframes-cli 피드백 보드 소개 영상을 promo에 새로 만들어 주세요. 0초부터 2초는 “팀원 의견이 흩어져 있나요?”, 2초부터 4초는 첨부 화면과 “한곳에 모으세요”, 4초부터 6초는 “고칠 순서까지 명확하게”로 구성해 주세요. 기존 파일이 있으면 먼저 알려 주세요.
```

5분 미니 실습: 실제 생성 파일과 장면별 시작 시각·길이를 받아 총 6초인지 읽습니다. 이어서 장면을 확인합니다.

```text hermes
/hyperframes-cli 방금 만든 promo의 0.5초, 2.5초, 4.5초를 PNG로 저장해 주세요. 실제 경로를 알려 주세요. 문구가 잘리거나 소개할 화면이 안 보이는 장면은 수정하고 PNG를 다시 만들어 주세요. 아직 업로드하지 마세요.
```

기대 결과는 문제 제기, 실제 화면, 사용 이유가 보이는 세 장면입니다. PNG를 직접 열어 읽은 뒤 영상으로 만듭니다.

```text hermes
/hyperframes-cli 확인한 promo를 promo.mp4로 렌더해 주세요. 실제 생성 파일 경로와 재생 길이를 알려 주세요. 영상이 6초이며 문구와 화면이 장면 순서대로 바뀌는지 확인하겠습니다. 공개 업로드는 아직 하지 마세요.
```

공유가 필요하면 파일 공개 범위와 계정을 확인한 뒤 선택합니다.

```text hermes
/hyperframes-cli 이 소개 영상을 팀원에게 링크로 보여 주고 싶어요. 공유할 프로젝트와 계정, 공개 범위를 먼저 보여 주세요. 제가 승인하면 퍼블리시하고 실제 열리는 주소를 알려 주세요.
```

설치 검사에서 환경변수 일괄 출력이나 경로 접근 등으로 차단되면 `--force`로 우회하지 않습니다. 스킬 대신 공식 도구로 같은 결과를 요청합니다.

```text hermes
HyperFrames 공식 문서를 읽고 현재 도구로 6초 HTML 영상을 만들 수 있는지 확인해 주세요. 필요한 준비가 없으면 알려 주세요. 새 설치나 업로드는 아직 하지 마세요.
```

준비가 끝나면 아래 요청으로 이어갑니다.

```text hermes
HyperFrames 공식 도구로 promo를 새로 만들어 주세요. 0초부터 2초는 “흩어진 의견”, 2초부터 4초는 “한곳에 모으기”, 4초부터 6초는 “고칠 순서 정하기”로 구성하고 장면 시간표와 실제 경로를 알려 주세요. 같은 폴더가 있으면 멈춰 주세요.
```

```text hermes
HyperFrames 공식 도구로 promo의 0.5초, 2.5초, 4.5초를 PNG로 저장해 주세요. 실제 경로를 알려 주세요. 필요한 도구가 없으면 알려 주고 새 설치나 공개 업로드는 하지 마세요.
```

막히면: 렌더 오류를 전달하고 브라우저·FFmpeg 중 빠진 준비를 찾게 합니다. HTML이 열린 것과 타임라인이 재생되는 것은 다릅니다. 시간에 따라 장면이 바뀌는지 봅니다. 마치면 다음 작업은 원래 프로젝트에서 이어 갑니다.

<details>
<summary>다른 사례: 스터디 질문함 소개</summary>

```text hermes
/hyperframes-cli 스터디 질문함을 소개하는 6초 영상을 만들어 주세요. “질문 남기기 → 함께 답하기 → 해결한 질문 모으기”를 각 2초로 보여 주고 0.5초, 2.5초, 4.5초 PNG와 시간표를 남겨 주세요. 기존 promo가 있으면 새 이름을 먼저 제안하세요.
```

</details>

다음에 더 찾아볼 것: [HyperFrames 공식 가이드](https://github.com/heygen-com/hyperframes/blob/main/docs/developers/cli.mdx).

완료 확인: 장면 시간표와 실제 PNG를 열었습니다. 렌더했다면 MP4 재생을 확인했고 스킬 사용과 공식 도구 직접 사용을 구분했습니다.

## 07. reverse-engineer-anything: 동작을 추측하지 않고 수정 위치 찾기 (심화 선택, 10분)

화면만 보고 내부 동작을 추측하면 엉뚱한 파일을 고치기 쉽습니다. 이 스킬은 REA 도구로 앱과 코드의 근거를 수집해 관찰한 사실, 추정, 아직 모르는 것을 구분합니다. 이번에는 피드백이 어디서 저장되는지 따라가며 중복 등록이 생길 위치를 찾습니다.

설치와 확인, 터미널:

```bash
hermes -p jiwon skills install morluto/rea/.agents/skills/reverse-engineer-anything
hermes -p jiwon skills list
```

목록에서 `reverse-engineer-anything`을 확인합니다. 스킬 설치와 REA 도구 등록은 별개입니다. 내 코드나 분석 허가가 있는 공개 코드만 대상으로 합니다. 이 실습 때문에 바이너리 분석 도구를 설치할 필요는 없습니다.

분석할 코드가 없다면 03에서 만든 입력 폼을 사용하거나, 분석 허가가 있는 공개 예제의 URL 또는 폴더를 먼저 전달합니다.

5분 미니 실습:

```text hermes
/reverse-engineer-anything 현재 피드백 보드에서 등록 버튼을 누르면 DB 저장까지 어떤 코드가 실행되는지 따라가 주세요. REA 도구 유무를 먼저 확인하고 중복 클릭을 막는 코드가 있는지 파일·함수 근거를 docs/reverse-notes.md에 남겨 주세요. 실행·수정은 하지 마세요.
```

기대 결과는 “중복 등록 가능”이라는 추측만이 아니라 이벤트 처리, 저장 호출, 버튼의 처리 중 상태가 어느 파일에 있는지 적힌 문서입니다. 근거가 없으면 중복이 발생한다고 단정하지 않습니다. REA 도구가 없다면 일반 코드 읽기만 수행했다고 구분하고 모듈을 마칩니다.

두 번째 패턴은 다른 공개 예제에서 적용할 구조를 찾는 것입니다. 먼저 자신이 선택한 예제의 URL 또는 코드 폴더를 전달합니다.

```text hermes
/reverse-engineer-anything 방금 전달한 공개 예제에서 폼의 중복 제출과 저장 실패를 처리하는 방식을 분석해 주세요. 라이선스와 근거 파일을 적고, 내 피드백 보드에 적용할 한 가지를 제안해 주세요. 코드는 실행하거나 수정하지 마세요.
```

막히면: 화면만 접근할 수 있을 때 서버·DB 구조까지 안다고 쓰지 않습니다. 읽을 수 있는 코드가 없으면 그 범위를 미확인으로 남깁니다. 로그인 우회와 비공개 자료 추출은 하지 않습니다.

<details>
<summary>다른 사례: 질문 등록 뒤 목록에 안 보이는 문제</summary>

```text hermes
/reverse-engineer-anything 현재 질문함에서 저장 성공 후 목록을 갱신하는 경로를 찾아 주세요. insert 뒤 조회를 다시 하는지 실제 파일과 함수로 설명하고, 없는 동작은 추측하지 마세요. 실행·수정 없이 docs/reverse-notes.md에 남겨 주세요.
```

</details>

다음에 더 찾아볼 것: [REA 공식 저장소](https://github.com/morluto/rea).

완료 확인: 수정할 위치와 그 근거가 남았습니다. 코드로 확인한 사실과 추정, REA 도구 사용 여부를 구분했습니다.

## 08. 세 스킬을 연결해 팀원에게 실습 결과 공유하기 (30분)

Context7으로 저장·조회 API를 찾고, Supabase로 의견을 보관하고, Vercel로 팀원에게 주소를 전달합니다. 기본 결과는 “다른 창에서 의견을 등록하고 새로고침해도 같은 의견이 남는 보드”입니다.

| 단계 | 실제 행동 | 남길 결과 |
|---|---|---|
| 1. 문서, 5분 | 현재 코드에 맞는 저장·조회 호출과 오류 처리 찾기 | library-notes.md |
| 2. DB와 화면, 15분 | 가상 의견 등록, DB 행 확인, 새로고침 후 재조회 | feedbacks의 행과 입력 폼 |
| 3. 공유, 10분 | 대상을 승인하고 Preview 배포, 다른 창에서 의견 등록 | deploy.md의 실제 URL |

각 모듈을 끝낸 뒤에는 팀원에게 무엇을 해 달라고 할지도 남깁니다.

```text hermes
지금 피드백 보드를 팀원에게 보여 줄 1분 시연 안내를 docs/demo.md에 써 주세요. 실제 Preview URL, 가상 의견 등록, 새로고침 후 확인 순서를 넣어 주세요. DB 미연결 등 아직 안 되는 기능은 된다고 쓰지 마세요.
```

완료 확인: 실제 문서 출처가 있는가, 웹 입력이 DB에 남는가, 새로고침 후 같은 의견이 보이는가, 다른 창에서 실제 URL가 열리는가. DB 연결이 안 됐다면 샘플 화면 배포까지만 완료로 적고 연결 준비로 돌아갑니다.

메모리를 연습했다면 저장한 작업 원칙으로 고칠 순서를 정해 보고, 영상을 만들었다면 실제 PNG나 MP4를 함께 공유합니다. 심화 분석에서는 “다음에 수정할 파일 한 곳”을 근거와 함께 설명합니다.

제출물: 실제 Preview URL와 docs의 작업 문서. 키, 로그인 정보와 개인 메모리 파일은 제출하지 않습니다. 실제로 연결·실행한 결과와 샘플 화면을 구분하세요.

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
- [hermes-hermory](https://github.com/kimsanguine/hermes-hermory)
- [Hermes Memory](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory)
- [Context7 CLI](https://github.com/upstash/context7/blob/master/docs/clients/cli.mdx)
- [Supabase insert](https://supabase.com/docs/reference/javascript/insert)
- [Supabase RLS](https://supabase.com/docs/guides/database/postgres/row-level-security)
- [Supabase Edge Functions](https://supabase.com/docs/guides/functions/quickstart-dashboard)
- [Vercel deploy](https://vercel.com/docs/cli/deploy)
- [HyperFrames CLI](https://github.com/heygen-com/hyperframes/blob/main/docs/developers/cli.mdx)
- [REA](https://github.com/morluto/rea)
