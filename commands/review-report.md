---
name: review-report
description: 4개의 전문 에이전트를 통해 코드 변경사항을 종합 분석
arguments:
  - name: target
    description: 리뷰 대상 경로 (기본값: 현재 디렉토리)
    required: false
  - name: type
    description: 특정 리뷰 타입만 실행 (screen, side-effect, quality, qa)
    required: false
---

# 코드 리뷰 리포트 생성

4개의 전문 분석 에이전트를 통해 코드 변경사항을 분석합니다.

## 리뷰 대상

{{#if target}}
대상 경로: `{{target}}`
{{else}}
대상: 최근 변경된 파일들 (git diff 또는 현재 디렉토리)
{{/if}}

## 실행 지침

{{#if type}}
### 단일 에이전트 실행
`{{type}}` 타입의 분석만 실행합니다.

해당 에이전트를 실행하여 분석 결과를 제공하세요:
{{#eq type "screen"}}
- `screen-component-reviewer` 에이전트 실행: 페이지별 컴포넌트/기능 변경사항 분석
{{/eq}}
{{#eq type "side-effect"}}
- `side-effect-reviewer` 에이전트 실행: Side Effect 분석
{{/eq}}
{{#eq type "quality"}}
- `code-quality-reviewer` 에이전트 실행: 코드 품질 리뷰 (개발자용)
{{/eq}}
{{#eq type "qa"}}
- `qa-checklist-reviewer` 에이전트 실행: QA 테스트 체크리스트 생성
{{/eq}}
{{else}}
### 전체 분석 실행

다음 4개의 에이전트를 **반드시 병렬로** 실행하세요.
**중요**: 단일 메시지에서 4개의 Task 도구 호출을 동시에 수행해야 합니다.

1. **screen-component-reviewer**: 유저 관점에서 페이지별 컴포넌트/기능 변경사항 분석
2. **side-effect-reviewer**: 수정된 코드의 Side Effect 분석
3. **code-quality-reviewer**: 개발자 관점 코드 품질 리뷰
4. **qa-checklist-reviewer**: QA가 확인해야 할 테스트 체크리스트

각 에이전트에게 전달할 프롬프트:
```
{{#if target}}
리뷰 대상 경로: {{target}}
{{else}}
git diff를 사용하여 최근 변경사항을 분석하거나, 현재 디렉토리의 주요 파일들을 리뷰하세요.
{{/if}}

코드 변경사항을 분석하고 각 관점에서 리포트를 작성하세요.
```
{{/if}}

## 결과 종합

모든 에이전트의 분석이 완료되면, 다음 형식으로 **종합 리뷰 리포트**를 작성하세요:

```markdown
# 📋 코드 리뷰 리포트

## 개요
- **분석 일시**: [현재 시간]
- **분석 대상**: [대상 경로 또는 변경사항 요약]
- **변경 요약**: [무엇이 변경되었는지 1-2문장]

---

## 📱 페이지별 변경사항
[screen-component-reviewer 결과 요약]

| 페이지 | 변경된 컴포넌트 | 주요 변경 |
|--------|---------------|----------|
| ... | ... | ... |

---

## ⚠️ Side Effect 주의사항
[side-effect-reviewer 결과 요약]

### 주의 필요
- [Side Effect 항목]

### 고려 필요
- [Side Effect 항목]

---

## 🔍 코드 품질 (개발자용)
[code-quality-reviewer 결과 요약]

### 개선 필요
- [개선 필요 항목]

### 권장 사항
- [권장 사항]

---

## ✅ QA 테스트 체크리스트
[qa-checklist-reviewer 결과 요약]

### 기능 테스트
- [ ] [테스트 항목]

### UI 테스트
- [ ] [테스트 항목]

### 시나리오 테스트
- [ ] [테스트 항목]

---

## 💡 참고사항
- [추가 참고 정보]
```
