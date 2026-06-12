---
name: handoff
description: 에이전트 자율 상호 위임 및 태스크 인계 스킬 (뉴턴경 & 레오갓 공용)
---

# /handoff — 에이전트 자율 상호 위임 스킬 (공용)
> **적용 주체:** 뉴턴경 (Claude Code) & 레오갓 (Gemini) 상호 연동
> **역할:** 실행 주체를 자동으로 인지하여, 상대방 에이전트에게 핀포인트 컨텍스트를 담은 위임 파일(PENDING)을 초경량으로 자동 생성한다.

---

## 📐 1. 실행 주체 자율 분기 (Auto-detection)

본 스킬이 실행되는 즉시 에이전트는 본인의 정체성을 감지하여 다음과 같이 아키텍처를 분기합니다.

```
                  [ /handoff 실행 ]
                         │
          ┌──────────────┴──────────────┐
          ▼ (주체가 뉴턴경인 경우)          ▼ (주체가 레오갓인 경우)
    뉴턴경 → 레오갓 위임             레오갓 → 뉴턴경 위임
    - 파일: newton-to-leo.md         - 파일: leo-to-newton.md
    - status: PENDING                - status: PENDING
```

---

## 🏃 2. 실행 프로세스 및 규격

### 🧠 Case A. 뉴턴경(Claude Code)이 실행한 경우 (뉴턴 → 레오)
1.  **선행 검사:** `newton-to-leo.md`가 이미 존재하고 `status: PENDING`이면 대기, `DONE`이면 아카이브 백업 이동.
2.  **파일 생성:** `/Volumes/T7/brain/dan-brain/90-system/AI-memory/handoff/newton-to-leo.md` 작성.
3.  **포맷:**
    ```markdown
    ---
    from: 뉴턴경
    to: 레오갓
    created: YYYY-MM-DD HH:mm
    status: PENDING
    priority: [P1|P2|P3]
    ---
    # 핸드오프: [주제]
    ## 요청 요약 (1~2줄)
    ## 필수 컨텍스트 (경로만 명시, 내용 붙여넣기 절대 금지)
    ## Tasks (Tier 분류 적용)
    ```

### 👁️ Case B. 레오갓(Gemini)이 실행한 경우 (레오 → 뉴턴)
1.  **선행 검사:** `leo-to-newton.md`가 이미 존재하고 `status: PENDING`이면 대기, `DONE/REJECTED`이면 아카이브 백업 이동.
2.  **파일 생성:** `/Volumes/T7/brain/dan-brain/90-system/AI-memory/handoff/leo-to-newton.md` 작성.
3.  **포맷 (v1.2.1 초경량 가치 연동):**
    ```markdown
    ---
    from: 레오갓
    to: 뉴턴경
    created: YYYY-MM-DD HH:mm
    status: PENDING
    priority: [P1|P2|P3]
    related_knowledge: "[[지식_노트_절대_경로]]"  # 뉴턴경이 읽어야 할 1줄 RAG 지식 앵커링 (필수)
    ---
    # 선제적 위임: [주제]
    ## 💡 위임 배경 및 기획 의도 (3줄 요약)
    ## 🛠️ 뉴턴경 개발 Tasks (디자인/로직 세부 명세)
    ## 📅 기대 마감 기한 (expires: YYYY-MM-DD HH:mm)
    ```

---

## ⚡ 3. 공통 완료 보고 및 인계 출력
파일 작성이 완료되면 에이전트는 상대방이 최우선 감지하여 수행할 수 있도록 다음 포맷으로 완료 브리핑을 단하느님 화면에 출력합니다.

```
✅ [실행주체]의 상호 위임 (/handoff) 완료
경로: .../handoff/[생성된 파일명] (status: PENDING)

[상대방 에이전트 수신 명령]
────────────────────────────────
[상대방 에이전트 명칭], 이 핸드오프 지침을 즉시 접수하여 수행해줘:
/Volumes/T7/brain/dan-brain/90-system/AI-memory/handoff/[생성된 파일명]
────────────────────────────────
```
