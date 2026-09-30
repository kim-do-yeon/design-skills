# design-skills

Claude Code용 UI 디자인 스킬 5종 묶음입니다. 한 번 설치하면 이후 UI 작업에서 알맞은 스킬이 자동으로 골라져 적용됩니다.

## 설치

Claude Code에서 아래 두 줄을 실행하세요.

```
/plugin marketplace add kim-do-yeon/design-skills
/plugin install design-pipeline@design-skills
```

터미널에서 설치하려면 다음 두 명령을 실행하세요.

```bash
claude plugin marketplace add kim-do-yeon/design-skills
```

```bash
claude plugin install design-pipeline@design-skills
```

`design-pipeline`을 설치하면 5개 스킬이 의존성으로 함께 설치됩니다. 설치 후 새 세션을 열면 적용돼요.

## 들어 있는 스킬

| 스킬 | 역할 | 원본 | 라이선스 |
|---|---|---|---|
| `hallmark` | AI가 만든 티가 안 나는 페이지 설계, 재설계, 점검(audit) | [Nutlope/hallmark](https://github.com/Nutlope/hallmark) | MIT |
| `frontend-design` | 개성 있는 시각 방향 잡기. Anthropic 공식 스킬 | [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/frontend-design) | Apache-2.0 |
| `ui-ux-pro-max` | 스타일·팔레트·폰트·UX 규칙 데이터베이스 검색 | [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) | MIT |
| `emil-design-eng` | 애니메이션, 인터랙션, 컴포넌트 완성도 | [emilkowalski/skills](https://github.com/emilkowalski/skills) | MIT |
| `web-design-guidelines` | Vercel 웹 인터페이스 가이드라인으로 코드 점검 | [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) | MIT |

이 레포는 스킬 파일을 복사해 두지 않고, 각 원본 저장소의 특정 커밋을 가리키기만 합니다. 원본 저장소가 바뀌어도 여기서 커밋을 올리기 전까지는 확인된 버전만 설치돼요.

## 자동 적용 방식

`design-pipeline`은 세션을 시작할 때 스킬 선택 규칙을 Claude에게 넣어 줍니다 ([routing.md](plugins/design-pipeline/hooks/routing.md)). 그래서 스킬 이름을 말하지 않아도 요청에 맞는 스킬이 골라집니다.

| 요청 | 자동으로 쓰이는 스킬 |
|---|---|
| "랜딩 페이지 만들어줘", "프로토타입 만들어줘" | `hallmark` → 필요하면 `ui-ux-pro-max` |
| "이 화면 다시 디자인해줘" | `hallmark` redesign |
| "색이랑 폰트 추천해줘", "폼 UX 개선해줘" | `ui-ux-pro-max` |
| "모달 애니메이션 자연스럽게 해줘" | `emil-design-eng` |
| "UI 리뷰해줘", "접근성 검사해줘" | `web-design-guidelines` + `hallmark` audit |

색 하나 바꾸는 정도의 작은 수정에는 스킬을 쓰지 않거나 하나만 씁니다.

### 전체 단계를 직접 실행하기

```
/design-pipeline:design-pipeline 차용증 작성 도우미 페이지
```

아래 순서로 진행하고, 단계마다 무엇이 바뀌었는지 표로 보고합니다.

1. `hallmark`: 방향 잡기와 구현
2. `ui-ux-pro-max`: 색·타입·UX 선택을 데이터로 검증하고 대비 측정
3. `emil-design-eng`: 모션과 인터랙션 다듬기
4. `web-design-guidelines`: 최종 점검과 수정
5. 도메인 검증: 계산, 날짜, 예외 입력. 어떤 스킬도 이 부분은 확인하지 않습니다.

## 효과 테스트 결과

같은 페이지(차용증 작성 도우미)를 스킬 없이 만든 다음, 스킬을 하나씩 추가하면서 측정했습니다. 블라인드 점수는 버전 이름을 가린 채 별도 에이전트가 50점 만점으로 매긴 점수입니다.

| 버전 | 추가한 스킬 | 블라인드 점수 (/50) | 기능 테스트 (/12) | Lighthouse 접근성 |
|---|---|---|---|---|
| v0 | 없음 | 9 | 2 | 77 |
| v1 | hallmark | 24 | 5 | 100 |
| v2 | ui-ux-pro-max | 30 | 7 | 100 |
| v3 | emil-design-eng | 34 | 8 | 100 |
| v4 | web-design-guidelines | 39 | 12 | 100 |

- 각 스킬은 자기 영역의 점수를 올렸습니다. hallmark는 시각, ui-ux-pro-max는 폼 UX, emil-design-eng는 모션, web-design-guidelines는 정확성입니다.
- 모든 스킬을 적용한 뒤에도 원 단위 반올림 오차, 월말 날짜 표기처럼 도메인에 따른 문제가 남았습니다. 그래서 파이프라인 마지막에 도메인 검증 단계를 넣었습니다.
- 과제 하나로 한 실험이라 결과를 일반화하기에는 한계가 있습니다.

## 업데이트

원본 스킬을 새 버전으로 올리려면 [marketplace.json](.claude-plugin/marketplace.json)에서 해당 항목의 `sha`를 바꾸고 `design-pipeline`의 `version`을 올리면 됩니다. 사용자는 아래 명령으로 받아요.

```bash
claude plugin marketplace update design-skills
```

## 삭제

```bash
claude plugin marketplace remove design-skills
```

마켓플레이스를 제거하면 여기서 설치된 플러그인도 함께 제거됩니다.

## 라이선스

이 레포의 코드(`design-pipeline`)는 MIT입니다. 포함된 스킬은 각 원본 저장소의 라이선스를 따르며, 위 표에 적어 두었습니다.
