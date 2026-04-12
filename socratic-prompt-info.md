# Socratic Prompt 정보 정리

> 최종 업데이트: 2026-04-12

---

## 개요

**Socratic Prompt (소크라테스 프롬프트)**는 고대 철학자 소크라테스의 문답법(Socratic Method)을 현대 LLM 프롬프트 엔지니어링에 적용한 기술입니다.

Stanford University의 Edward Y. Chang이 2023년 논문 "Prompting Large Language Models with the Socratic Method"에서 체계화했습니다.

---

## 기존 58개 프롬프트 기술 분류에서의 위치

| 구분 | 내용 |
|------|------|
| **The Prompt Report (2024)** | 58개 텍스트 기반 프롬프트 기술에 **미포함** |
| **이유** | 2023년에 학술적으로 체계화되어, 기존 분류 작성 이후에 나타난 기술 |

### 분류 시도

| 분류 기준 | 범주 |
|----------|------|
| PromptPrism (2026) | Reasoning & Planning |
| 기능적 분류 | Metacognitive / Self-Reflection |
| 학술적 관점 | Reasoning Enhancement |

---

## 6가지 핵심 기술 (Edward Chang, 2023)

| 번호 | 기술 | 설명 | 유사 프롬프트 기술 |
|------|------|------|------------------|
| 1 | **Definition** | 용어 정의 명확화 | Rephrase and Respond (RaR) |
| 2 | **Elenchus** | 반박, 교차심문 | Self-Consistency |
| 3 | **Dialectic** | 변증법적 대립 분석 | Tree of Thought |
| 4 | **Maieutics** | 조산술 (스스로 답 찾도록 유도) | Scaffolding Prompt |
| 5 | **Generalization** | 구체적 예시에서 일반 원칙 도출 | - |
| 6 | **Counterfactual Reasoning** | 반사실적 추론 ("만약 ~이었다면") | - |

---

## Chain-of-Thought와의 비교

| 측면 | Chain-of-Thought | Socratic Prompt |
|-------|--------------|---------------|
| 목표 | 추론 과정 시각화 | 질문으로 맥락 명확화 |
| 방식 | "단계별로 생각해라" | "답하기 전에 질문하라" |
| 출력 | reasoning → answer | question → question → answer |
| 적절한 상황 | 수학, 논리 문제 | 모호한 요구사항, 설계 작업 |

---

## 실제 사용 템플릿

```markdown
# Goal
[수행할 작업]

# CRITICAL RULES
1. 아직 정답을 생성하지 마세요.
2. 요구사항, 제약조건, 규모를 명확히 하기 위해 질문을 하세요.
3. 한 번에 2-3개만 질문하고, 답변을 기다리세요.
4. 충분한 정보가 있을 때까지 이 피드백 루프를 계속하세요.
```

---

## 참고 자료

- Chang, E.Y. (2023). "Prompting Large Language Models with the Socratic Method" - Stanford University
- The Prompt Report (2024) - 1,565개 논문 분석, 58개 프롬프트 기술 분류
- PromptPrism (2026) - linguistically-inspired taxonomy

---

## 정리 메모

- 기존 58개 기술 분류에 미포함 (작성 시점 차이)
- Reasoning & Planning 또는 Metacognitive 범주에 가장 근접
- 독립적 메타 프롬프트로 분류되는趋向
- Chain-of-Thought와 기능적으로 가장 유사하나, 추론 전 질문에 초점