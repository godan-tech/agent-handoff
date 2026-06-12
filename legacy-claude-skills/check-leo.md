# /check-leo — 레오갓 응답 수신 스킬

## 역할
레오갓이 작성한 `leo-to-newton.md`를 읽고 핵심만 정리한 후 아카이브한다.
session.md 전체 읽기 없이 응답만 빠르게 수신.

## 실행 순서

### Phase 1: 응답 파일 존재 확인
```bash
ls -la /Volumes/T7/brain/dan-brain/90-system/AI-memory/handoff/leo-to-newton.md
grep "status:" .../leo-to-newton.md
```
- 파일 없음 → "레오갓 응답 아직 없음"
- status: PENDING → "레오갓이 아직 작성 중"
- status: DONE → Phase 2 진행

### Phase 2: 응답 내용 읽기 + 요약

`leo-to-newton.md` 전체 읽기 후:
1. **결론 3줄** 먼저 출력
2. **즉시 실행 항목** 번호로 정리
3. **교정 사항** 있으면 강조 표시

### Phase 3: 뉴턴경 판단 요청

```
레오갓 응답 요약:
━━━━━━━━━━━━━━━━━━━━
[결론]
[즉시 실행 항목]
━━━━━━━━━━━━━━━━━━━━
다음 중 선택하세요:
1. 아카이브 (처리 완료)
2. newton/session.md에 핵심만 이관 후 아카이브
3. 재요청 (레오에게 추가 질문)
```

### Phase 4: 아카이브 (선택 시)

```bash
TIMESTAMP=$(date +%Y-%m-%d-%H%M)
TOPIC=$(grep "^# 응답:" leo-to-newton.md | sed 's/# 응답: //')

# 양쪽 파일 아카이브
cp newton-to-leo.md archive/${TIMESTAMP}-N→L-${TOPIC}.md
cp leo-to-newton.md archive/${TIMESTAMP}-L→N-${TOPIC}.md

# 활성 파일 상태 업데이트
# newton-to-leo.md와 leo-to-newton.md는 다음 핸드오프까지 유지
```

### Phase 5: session.md 이관 (선택 시)

레오 응답에서 **핵심 결정만** newton/session.md에 추가:

```markdown
## 📨 레오갓 검증 결과 (YYYY-MM-DD)
- 핸드오프: [주제]
- 판정: [동조/교정 사항 1~2줄]
- 즉시 실행: [항목]
> 상세 → handoff/archive/[파일명]
```

---

## 원칙

- session.md에는 **결론만** 이관 (레오 응답 전문 복사 금지)
- 상세 내용은 archive/ 파일이 SSoT
- 재요청 시 `/handoff` 재실행으로 새 newton-to-leo.md 생성

## 연계 스킬
- `/handoff` — 레오에게 새 작업 위임
- 프로토콜 규정: `.../handoff/README.md`
