# 하네스 프롬프트 (Harness Prompt)

하네스(Harness)는 DevOps 플랫폼으로, **AI 기반 자동화**에서 프롬프트를 활용하는 두 가지 주요 영역이 있습니다.

---

## 1. Harness AI Test Automation (테스트 자동화)

AI 테스트 자동화에서 프롬프트는 **자연어로 테스트를 작성**하는 명령형 지침입니다.

### 프롬프트 구성 요소 (5가지)

| 요소 | 설명 |
|------|------|
| **Goal** | 테스트할 비즈니스 결과 (예: "프로모션 코드 적용 후 결제 완료") |
| **Context** | 애플리케이션 정보, 사용자 플로우 |
| **Actions** | 수행할 단계들 |
| **Assertions** | 검증 조건 |
| **Edge Cases** | 예외 상황 처리 |

### Prompt Enhancer

-模糊한 프롬프트를 **구체적으로 개선**해줍니다
- 예시:
  - 입력: `"men wedding attire"`
  - 개선: `"Search for 'men's wedding attire' in the website's search bar, then click the 'Search' button. Verify that the search results display suitable options"`

### 4가지 명령 유형

1. **Single-step tasks** - 개별 작업
2. **Multi-step AI tasks** - 복잡한 워크플로우
3. **Assertions** - 검증/확인
4. **Extract data** - 데이터 추출

---

## 2. Harness AI (DevOps - 파이프라인 생성)

플랫폼 자원(Pipeline, Service, Environment 등)을 **자연어로 생성**합니다.

### 작성 가이드라인

- **구체적으로**: 리포지토리, 브랜치, 커넥터 이름 포함
- **액션 워드 사용**: "Generate", "Create", "Define"으로 시작
- **입력 변수 명시**: `<+input>`으로 런타임 입력 지정
- **한 번에 하나의 자원**: 정확도 향상

### 예시

| 약한 프롬프트 | 강한 프롬프트 |
|--------------|---------------|
| `Create a pipeline` | `Generate a CI→CD pipeline that builds a Docker image and deploys to Kubernetes namespace dev` |
| `Add a secret` | `Create a Secret Text named docker-hub-token, store in default Secret Manager` |
| `Create a service` | `Create a Kubernetes service named portal using Helm with chart path /cd/chart/portal, expose variables: ENV, VERSION` |

### 리소스 범위 지정

항상 리소스가 생성될 범위(Account, Org, Project)를 명시합니다.

```
Generate a pipeline at the Project scope that uses the staging environment and GitHub connector rohan-git.
```

---

## 핵심 특징

1. **의도 기반 테스트링 (Intent-based Testing)**: 스크립트 없이 자연어로 테스트 작성
2. **Self-healing**: UI 변경에 자동으로 적응
3. **Prompt Enhancer**: 프롬프트 품질 자동 평가 및 개선 제안
4. **LLM 기반**: GPT-5.2, Claude, Gemini 등 지원
5. **다양한 명령 타입**: Extract data, Assertions, Single-step, Multi-step

---

## 참고 자료

- [Harness Developer Hub - Effective Prompting](https://developer.harness.io/docs/platform/harness-ai/effective-prompting-ai)
- [Harness AI Test Automation Overview](https://developer.harness.io/docs/ai-test-automation/get-started/overview/)
- [Prompt Enhancer 문서](https://developer.harness.io/docs/ai-test-automation/test-authoring/harness-ai-copilot/prompt-enhancer)