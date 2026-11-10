# hermes-hermory

에이전트가 다음 대화에도 사용할 기준을 저장하고, 바꾸고, 점검하는 Hermes 스킬입니다. AX랩/KDT 수업에서는 기능을 추가하기 전에 확인할 질문 같은 프로젝트 진행 원칙을 저장하고 새 대화에서 적용합니다.

현재 검증: 공개 저장소에서 Hermes 설치 내부 함수를 호출해 새 격리 폴더에 다운로드, 보안 검사 SAFE, 설치 파일 일치까지 확인했습니다. CLI 런처 완주는 확인 필요이며 메모리 도구를 통한 실제 쓰기는 실행하지 않았습니다. [검증 기록](docs/verification.md)에 범위를 구분했습니다.

## 설치

아래는 도움말과 설치 내부 함수에 대응하는 CLI 명령입니다. CLI 런처를 포함한 새 환경 완주는 확인 필요입니다. 기존 `jiwon` 프로필에서 실행합니다. 이미 다른 프로필을 쓰고 있다면 실제 ID를 확인하고 모든 `-p jiwon`을 `-p 실제ID`로 바꿉니다. `-p`를 생략하지 않습니다. 대화창의 `/status`로 같은 프로필인지 확인하세요. 새 프로필을 만들 필요는 없습니다.

```bash
hermes -p jiwon skills list
hermes -p jiwon skills inspect kimsanguine/hermes-hermory/skills/hermes-hermory
hermes -p jiwon skills install kimsanguine/hermes-hermory/skills/hermes-hermory
hermes -p jiwon skills list
```

같은 이름이 이미 있으면 기존 파일을 먼저 확인하고 설치를 멈춥니다. `--force`로 덮어쓰지 않습니다. 설치 후 새 대화에서 아래 요청을 입력합니다.

```text
/hermes-hermory 내가 새 기능을 만들자고 하면 누가 언제 불편한지, 지금은 어떻게 해결하는지, 이번에 꼭 필요한지를 먼저 물어봐줘. 이걸 내 프로젝트 진행 원칙으로 기억해줘. 다른 기억은 바꾸지 마.
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

- [팀스파르타 Hermes 스킬로 조사부터 웹 배포까지 맡기기, 실제 교안](https://teamsparta.habix.ai/teamsparta/10th/setup/hermes-skill-toolkit)
- [설치와 실행 검증 범위](docs/verification.md)
- 다음에 더 찾아볼 것: [Hermes Memory](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory)

## 출처와 라이선스

강사의 메모리 운영 원칙에서 현재 프로필 확인, 읽고 수정하기, 용량 확인, 읽기 전용 감사를 추려 공개 교육용으로 재작성했습니다. 개인 환경의 경로, 운영 수치, 다른 프로필 정보와 외부 메모리 서비스 설정은 포함하지 않습니다. MIT 라이선스로 배포합니다.
