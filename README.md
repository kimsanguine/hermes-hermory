# Hermes Memory Operations

에이전트가 다음 대화에도 사용할 기준을 저장하고, 바꾸고, 점검하는 Hermes 스킬입니다. AX랩/KDT 수업에서는 가상의 학습 선호 한 줄로 저장과 수정의 차이를 연습합니다.

현재 검증: 공개 저장소에서 Hermes 설치 내부 함수를 호출해 새 격리 폴더에 다운로드, 보안 검사 SAFE, 설치 파일 일치까지 확인했습니다. CLI 런처 완주는 확인 필요이며 메모리 도구를 통한 실제 쓰기는 실행하지 않았습니다. [검증 기록](docs/verification.md)에 범위를 구분했습니다.

## 설치

아래는 도움말과 설치 내부 함수에 대응하는 CLI 명령입니다. CLI 런처를 포함한 새 환경 완주는 확인 필요입니다. 기존 `jiwon` 프로필에서 실행합니다. 이미 다른 프로필을 쓰고 있다면 실제 ID를 확인하고 모든 `-p jiwon`을 `-p 실제ID`로 바꿉니다. `-p`를 생략하지 않습니다. 대화창의 `/status`로 같은 프로필인지 확인하세요. 새 프로필을 만들 필요는 없습니다.

```bash
hermes -p jiwon skills list
hermes -p jiwon skills inspect kimsanguine/hermes-hemory/skills/hermes-memory-operations
hermes -p jiwon skills install kimsanguine/hermes-hemory/skills/hermes-memory-operations
hermes -p jiwon skills list
```

같은 이름이 이미 있으면 기존 파일을 먼저 확인하고 설치를 멈춥니다. `--force`로 덮어쓰지 않습니다. 설치 후 새 대화에서 아래 요청을 입력합니다.

```text
hermes-memory-operations 스킬을 읽어 주세요. 현재 프로필의 메모리 도구가 있는지 먼저 확인하고, 기존 내용을 노출하지 말고 다음 한 줄을 저장할 변경안만 보여 주세요. 아직 저장하지 마세요.
“실습용 선호: 설명 뒤에 예시 한 개를 붙인다.”
```

## 저장, 수정, 감사

| 요청 | 스킬의 행동 |
|---|---|
| “이 기준을 기억해 줘” | 현재 프로필, 보관할 사실, 저장 대상을 확인하고 변경안을 보여 줍니다. |
| “내가 승인한 이 한 줄만 바꿔 줘” | 최신 저장값을 다시 읽고 지정한 항목만 수정합니다. |
| “메모리를 점검해 줘” | 민감값을 출력하지 않고 중복, 오래된 기준, 불필요한 임시값을 보고합니다. |

```mermaid
flowchart LR
  A[보관할 기준 제안] --> B[현재 프로필과 변경안 확인]
  B --> C[사용자 승인]
  C --> D[사용 가능한 메모리 도구로 반영]
  D --> E[실제 저장 상태 재조회]
```

메모리는 여러 작업에 계속 적용할 사실이나 선호를 담고, 스킬은 다시 실행할 업무 절차를 담습니다. API 키, 고객 자료, 일회성 결과는 기억에 넣지 않습니다. 스킬 설치 자체는 메모리 도구나 외부 메모리 서비스를 설치하지 않습니다.

## 교안

- [팀스파르타 Hermes 스킬 6개 설치와 활용, 실제 교안](https://teamsparta.habix.ai/teamsparta/10th/setup/hermes-skill-toolkit)
- [설치와 실행 검증 범위](docs/verification.md)
- 다음에 더 찾아볼 것: [Hermes Memory](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory)

## 출처와 라이선스

사용자의 기존 `hermes-memory-operations` 운영 원칙에서 현재 프로필 확인, 읽고 수정하기, 용량 확인, 읽기 전용 감사를 추려 공개 교육용으로 재작성했습니다. 개인 환경의 경로, 운영 수치, 다른 프로필 정보와 외부 메모리 서비스 설정은 포함하지 않습니다. MIT 라이선스로 배포합니다.
