---
name: handoff-reply
description: 상대방 에이전트의 위임에 대해 구조화된 답장을 작성하여 이관하는 스킬 (공용)
---

# /handoff-reply — 레오갓 응답 스킬 (레오갓 전용)

## 역할
뉴턴경의 핸드오프 요청을 읽고 구조화된 응답을 작성한다.
session.md 전체 읽기 없이 핸드오프 파일만 처리.

## 실행 순서

### Phase 1: 핸드오프 파일 읽기 (이것만)
```
읽을 파일: /Volumes/T7/brain/dan-brain/90-system/AI-memory/handoff/newton-to-leo.md
```
- status가 PENDING이 아니면 → "처리할 핸드오프 없음" 출력 후 종료
- 관련 파일 경로가 명시된 경우 → 해당 파일 읽기 (session.md 전체 금지)

### Phase 2: 레오갓 역할로 처리

각 Task에 대해:
1. **판정 먼저**: 동조 / 교정 / 보류 중 1개 선택
2. **근거 2~3줄**: 팩트·논리 기반, 맹목적 동조 금지
3. **권장 액션 1줄**: 뉴턴경이 즉시 할 수 있는 것

### Phase 3: leo-to-newton.md 작성

아래 포맷으로 저장:

```markdown
---
from: 레오갓
to: 뉴턴경
created: YYYY-MM-DD HH:mm
re: [핸드오프 주제]
status: DONE | REJECTED  # 반려 시 REJECTED로 설정
reason: " border-white/5 미준수 및 소실각 34도 왜곡 감지"  # REJECTED 시 1줄로 원인 단축 명시
related_knowledge: "[[2026-05-29-AI바이브코딩-디자인한계돌파-듀얼에이전트전략]]"  # 1줄 RAG 지식 앵커링 (선택)
expires: YYYY-MM-DD HH:mm  # 데드락 방지 Failsafe 만료 시한 (선택)
---
# 응답: [핸드오프 주제]

## 결론 (3줄 이내)
[가장 중요한 판정/발견]

## Task별 결과

### [Task A] [제목]
- 판정: [동조|교정|보류]
- 핵심 근거: [2~3줄]
- 권장 액션: [1줄]

### [Task B] [제목]
- 판정: [동조|교정|보류]
- 핵심 근거: [2~3줄]
- 권장 액션: [1줄]

## 뉴턴경 즉시 실행 항목
1. [가장 중요]
2. [그 다음]
```

### Phase 4: newton-to-leo.md status 업데이트
`newton-to-leo.md` 파일의 `status: PENDING` → `status: DONE`으로 변경

### Phase 5: 완료 확인 출력
```
✅ 응답 완료
저장: .../handoff/leo-to-newton.md
뉴턴경에게 전달: "/check-handoff 실행하면 내 응답 볼 수 있어"
```

---

## 작성 원칙

| 해야 할 것 | 하지 말 것 |
|----------|----------|
| 판정 먼저 (동조/교정/보류) | "좋은 접근입니다" 시작 |
| 논리 허점 명시 | 맹목적 동조 |
| 뉴턴경 행동 항목 명확히 | 모호한 제안 |
| 50줄 이내 응답 | session.md 전체 복사 |

**목표 파일 크기**: 30~50줄 (≈500~1000 토큰)

## 연계 스킬
- `/handoff` (뉴턴경 스킬) — 핸드오프 생성
- `/check-handoff` (뉴턴경 스킬) — 이 응답 수신
- 프로토콜 규정: `.../handoff/README.md`
