---
name: review-report
description: 6개의 전문 에이전트를 통한 종합 코드 리뷰 실행
arguments:
  - name: target
    description: 리뷰 대상 경로 (기본값: 현재 디렉토리)
    required: false
  - name: type
    description: 특정 리뷰 타입만 실행 (screen, side-effect, quality, impact, qa, user-flow)
    required: false
---

# 종합 코드 리뷰

6개의 전문 코드 리뷰 에이전트를 사용하여 체계적인 코드 리뷰를 수행합니다.

## 리뷰 대상

{{#if target}}
대상 경로: `{{target}}`
{{else}}
대상: 최근 변경된 파일들 (git diff 또는 현재 디렉토리)
{{/if}}

## 실행 지침

{{#if type}}
### 단일 에이전트 실행
`{{type}}` 타입의 리뷰만 실행합니다.

해당 에이전트를 실행하여 리뷰 결과를 제공하세요:
{{#eq type "screen"}}
- `screen-component-reviewer` 에이전트 실행
{{/eq}}
{{#eq type "side-effect"}}
- `side-effect-reviewer` 에이전트 실행
{{/eq}}
{{#eq type "quality"}}
- `code-quality-reviewer` 에이전트 실행
{{/eq}}
{{#eq type "impact"}}
- `impact-analyzer` 에이전트 실행
{{/eq}}
{{#eq type "qa"}}
- `qa-checklist-reviewer` 에이전트 실행
{{/eq}}
{{#eq type "user-flow"}}
- `user-flow-reviewer` 에이전트 실행
{{/eq}}
{{else}}
### 전체 리뷰 실행

다음 6개의 에이전트를 **반드시 병렬로** 실행하세요.
**중요**: 단일 메시지에서 6개의 Task 도구 호출을 동시에 수행해야 합니다.

1. **screen-component-reviewer**: 화면/컴포넌트/기능 단위 분석
2. **side-effect-reviewer**: 사이드 이펙트 분석
3. **code-quality-reviewer**: 코드 품질 평가
4. **impact-analyzer**: 영향 범위 분석
5. **qa-checklist-reviewer**: QA 체크리스트 검증
6. **user-flow-reviewer**: 유저 플로우 관점 분석

각 에이전트에게 전달할 프롬프트:
```
{{#if target}}
리뷰 대상 경로: {{target}}
{{else}}
git diff를 사용하여 최근 변경사항을 분석하거나, 현재 디렉토리의 주요 파일들을 리뷰하세요.
{{/if}}

변경된 코드를 분석하고 해당 관점에서 리뷰 결과를 제공하세요.
```
{{/if}}

## 결과 종합

모든 에이전트의 리뷰가 완료되면, 다음 형식으로 종합 리포트를 작성하세요:

```markdown
# 종합 코드 리뷰 리포트

## 개요
- 리뷰 일시: [현재 시간]
- 리뷰 대상: [대상 경로 또는 변경사항]
- 실행된 에이전트: [에이전트 목록]

## 에이전트별 주요 발견사항

### 1. 화면/컴포넌트/기능 분석
[screen-component-reviewer 결과 요약]

### 2. 사이드 이펙트 분석
[side-effect-reviewer 결과 요약]

### 3. 코드 품질 평가
[code-quality-reviewer 결과 요약]

### 4. 영향 범위 분석
[impact-analyzer 결과 요약]

### 5. QA 체크리스트 검증
[qa-checklist-reviewer 결과 요약]

### 6. 유저 플로우 분석
[user-flow-reviewer 결과 요약]

## 종합 평가

### 전체 점수
| 항목 | 점수 (1-5) |
|------|-----------|
| 코드 품질 | |
| 사이드 이펙트 위험 | |
| 영향 범위 | |
| QA 준수 | |
| UX 영향 | |
| **종합** | |

### 필수 수정 사항
1. [우선순위 높은 항목]

### 권장 개선 사항
1. [권장 항목]

### 다음 단계
- [ ] 필수 수정 사항 해결
- [ ] 테스트 수행
- [ ] 코드 리뷰 반영
```
