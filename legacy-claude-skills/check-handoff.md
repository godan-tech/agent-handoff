---
name: check-handoff
description: 에이전트 완료 결과 확인 및 아카이브 스킬 (뉴턴경 & 레오갓 공용)
---

# /check-handoff — 에이전트 자율 결과 확인 및 아카이브 스킬 (공용)
> **적용 주체:** 뉴턴경 (Claude Code) & 레오갓 (Gemini) 상호 연동
> **역할:** 실행 주체를 자동으로 인지하여, 상대방 에이전트가 처리 완료한 핸드오프 결과물을 수신 및 검증하고, 아카이브로 자동 이동 보관한다.

---

## 📐 1. 실행 주체 자율 분기 (Auto-detection)

본 스킬이 실행되는 즉시 에이전트는 본인의 정체성을 감지하여 다음과 같이 수신 아키텍처를 분기합니다.

```
               [ /check-handoff 실행 ]
                         │
          ┌──────────────┴──────────────┐
          ▼ (주체가 뉴턴경인 경우)          ▼ (주체가 레오갓인 경우)
    레오갓의 응답 수신                 뉴턴경의 응답 수신
    - 읽을 파일: leo-to-newton.md     - 읽을 파일: newton-to-leo.md
```

---

## 🏃 2. 실행 프로세스 및 검증

### 🧠 Case A. 뉴턴경(Claude Code)이 실행한 경우 (레오의 응답 확인)
1.  **응답 검사:** `leo-to-newton.md` 헤더의 `status`가 `DONE` 또는 `REJECTED` 인지 확인.
2.  **동작:**
    *   `status: REJECTED` 인 경우: 즉시 신규 작업 올스톱, `reason` 분석 후 수정 모드로 최우선 강제 전환.
    *   `status: DONE` 인 경우: 레오갓의 판정 결과(동조/교정/보류) 및 권장 액션 3줄 요약 수신.
3.  **백업 및 클린업:**
    ```bash
    TIMESTAMP=$(date +%Y-%m-%d-%H%M)
    mv .../leo-to-newton.md .../archive/${TIMESTAMP}-L→N-[주제]-[DONE|REJECTED].md
    ```

### 👁️ Case B. 레오갓(Gemini)이 실행한 경우 (뉴턴의 응답 확인)
1.  **응답 검사:** `newton-to-leo.md` 헤더의 `status`가 `DONE` 인지 확인.
2.  **동작:**
    *   뉴턴경이 패키징/개발 완료한 결과와 로컬/배포 URL을 수집하여 단하느님께 즉시 보고.
    *   **2차 비주얼 위생 감사 가동:** `border-white/5` 극세 보더 준수 여부 및 `Isometric 소실각 34도` 기하학적 정렬 무결성 즉각 검증.
3.  **백업 및 클린업:**
    ```bash
    TIMESTAMP=$(date +%Y-%m-%d-%H%M)
    mv .../newton-to-leo.md .../archive/${TIMESTAMP}-N→L-[주제]-DONE.md
    ```

---

## ⚡ 3. 공통 완료 보고 포맷

```
✅ [실행주체]의 결과 확인 (/check-handoff) 완료
- 수행 주제: [주제]
- 수신 상태: [DONE | REJECTED]

[검증 및 위생 감사 결과 브리핑]
────────────────────────────────
- [뉴턴경 또는 레오갓이 수행한 내용 요약 및 2차 검증 판정]
────────────────────────────────
- 완료 아카이브 백업 성공: handoff/archive/YYYY-MM-DD-HHmm-[방향]-[주제]-DONE.md
```
