# 002-마음날씨 PRD 정합성 판별 보고서

> **검증일**: 2026-01-31 (3차)
> **대상 문서**: `ideas/002-마음날씨-prd.md` (v3.0, 2026-01-31)
> **참조 문서**: `ideas/002-마음날씨.md`, `research/competitors/002-경쟁분석.md`, `research/market-data/002-시장분석.md`, `research/competitors/002-마음날씨-경쟁사-심층분석.md`
> **검증 방침**: 비판적 리뷰어 관점 — 통과 근거보다 결함 발견에 집중. "왜 실패할 수 있는지"를 먼저 탐색.

---

## 이전 리뷰 대비 변경 사항 확인

v3.0은 Stage 3 완전 재작성본이다. 2차 리뷰(v2.0 대상)에서 Minor 3건이 잔류했으며, v3.0에서의 해결 상태를 확인한다.

| # | 2차 지적 사항 | 심각도 | v3.0 해결 상태 |
|---|-------------|--------|--------------|
| 1 | PDF 생성 Edge Function 라이브러리 미명시 | Minor | **해결됨** — F-08에 "`pdf-lib` 라이브러리로 생성 → Storage 업로드 → 서명된 URL 반환" 명시 (5-2장 F-08) |
| 2 | 공유 카드 이미지 생성 기술 미명시 | Minor | **해결됨** — F-10에 "클라이언트 사이드 `react-native-view-shot`으로 UI → 이미지 캡처" 명시 (5-4장 F-10) |
| 3 | 온보딩 상세 플로우 미흡 | Minor | **해결됨** — Screen Map(8-1장)에 OnboardingStep1/2/3 각 화면 명시, F-04 AC에 "3스텝 완료 후 onboarding_completed = true", "스킵 가능" 등 구체화 |

**v3.0 주요 보강 사항:**
- 15개 섹션 완성 (기존 14개에서 Screen Map & UI 명세 추가)
- User Story 7개 → 24개 (8 Epic)
- Screen Map 네비게이션 구조 트리 + 핵심 화면별 UI 구성 신규
- DB Function 완전한 SQL 본문 (`create_emotion_record` 55줄, `get_weekly_summary` 80줄)
- Zustand Store 6개 정의 (AuthStore, EmotionRecordStore, MasterDataStore, ReportStore, SubscriptionStore, UIStore)
- TypeScript 인터페이스 6종 (CreateEmotionRecordRequest, EmotionRecordWithRelations, RecommendSelfcareRequest/Response, WeeklyReportResponse, MonthlyReportResponse, VerifySubscriptionResponse, ExportResponse)
- 기능-테이블-API 매핑표 (F-01~F-09 전체)
- 자체 점검 결과 9항목

---

## 1. 영역별 판정

### A. 사업성 검증 — **통과**

#### 시장 규모·경쟁분석 정합성: **통과**
- PRD의 TAM($61억), SAM(300~500만 명), SOM(MAU 3~5만) 수치가 2단계 시장분석(`002-시장분석.md`)과 **정확히 일치**한다.
- 경쟁사 분석(하루콩, 무디, 루빗, 라일리 하루, 데일리오, MOODA, 마인드카페, 트로스트, 마보)이 2단계 경쟁분석과 **일관**된다.
- "기록 앱 ↔ 상담 앱 사이의 공백" 포지셔닝이 Problem Statement(2장)부터 경쟁 차별화 전략(9장)까지 일관 관통.

#### 차별점 방어 가능성: **통과**
- 핵심 차별화 3요소(9-2장): 한국어 감정 어휘 체계(36종), 기록→분석→행동 완결 루프, 데이터 기반 셀프케어 개인화.
- 2단계 경쟁분석의 차별점 방어 가능성 평가와 정합. 기술적 진입장벽이 낮음을 인정하면서 **데이터 축적**(개인화 피드백 `helpful_rate * 0.6 + relevance * 0.4`)을 방어선으로 설정.
- 라일리 하루 대비 차별화(9-3장)가 비교표로 구체화: "What to do(행동 제안)" vs "What I feel(감정 분석)".

#### 수익 모델: **통과**
- 월 3,500원 / 연 27,900원 가격이 시장 벤치마크와 정합.
- 수익 전망 3개 시나리오(10-3장)가 보수적~낙관적 범위로 합리적 설정.
- 무료/유료 가치 격차(10-1장)가 기능별로 명확히 구분.
- 향후 수익 다각화(캐릭터/테마 판매, B2B, 콘텐츠 파트너십) 방안(10-4장) 제시.

#### TAM/SAM/SOM 논리적 근거: **통과**
- TAM: OpenPR/HTF Market Intelligence 출처 ($61억).
- SAM: 하루콩 200만 + 마인드카페 400만 + 기타(중복 제거) → 500만 / MZ세대 1,200만 × 이용률 65% → 800만. 도출 과정 투명.
- SOM: MOODA 수준 니치 앱으로 보수적 설정(MAU 3~5만). 과도한 낙관 없음.

---

### B. 리스크 검증 — **통과**

#### 핵심 리스크 식별 및 대응: **통과**
PRD 11장에서 6개 핵심 리스크를 체계적으로 분류:

| 리스크 | 영향도 | 발생 확률 | 대응 | 평가 |
|--------|--------|----------|------|------|
| R-01 경쟁 심화 | 높음 | 높음 | 행동 제안 차별화, 데이터 moat, 빠른 MVP 출시 | 적절. 2단계 경쟁분석과 일치 |
| R-02 낮은 유료 전환율 | 높음 | 중간 | 블러 처리, 7일 무료체험, 캐릭터/테마 병행 검토 | 적절. 벤치마크(3~5%) 기반 |
| R-03 리텐션 저하 | 높음 | 중간 | 10초 UX, 푸시 개인화, streak, 즉각적 가치 | 적절 |
| R-04 개인정보 이슈 | 높음 | 낮음 | RLS, TLS 1.3, 앱 잠금, 처리방침 공개 | 적절 |
| R-05 1인 개발 병목 | 중간 | 높음 | P0 집중, RN 라이브러리, 커뮤니티 바이럴 | 적절 |
| R-06 앱스토어 리젝 | 중간 | 낮음 | 비의료 포지셔닝, 셀프케어 명시 | 적절 |

#### 외부 API·플랫폼 의존성: **통과**
- MVP 외부 의존 최소화 원칙이 명확. AI/ML 불필요, SQL 집계 기반.
- Supabase, RevenueCat, Expo Push, 카카오 API — 각각 전제 조건(12장)에서 버전/티어 요건을 명시.
- 제약 사항(12장)에서 Supabase 전적 의존, 자체 서버 미운영을 솔직하게 기술.

---

### C. 기술 검증 — **통과**

#### 기술 스택 구현 가능성: **통과**
- React Native + TypeScript + Expo + Supabase 조합으로 모든 P0 기능 구현 가능.
- 기술 선택(7-1장)이 선택 이유와 함께 명시:
  - WatermelonDB(구조화 데이터, 오프라인, Observable) + MMKV(설정, 캐시, 토큰) — 역할 분담 명확.
  - Zustand(전역 상태) + TanStack Query(서버 상태 캐싱/동기화) — 경량 조합.
  - Victory Native(차트), react-native-calendars(캘린더), react-native-view-shot(카드 캡처) — 검증된 라이브러리.
  - RevenueCat(결제), Expo Push(알림), PostHog(분석), Sentry(크래시) — 표준 서비스.

#### 1인 개발 규모 복잡성: **통과**
- Out of Scope(13장)에서 8개 항목(LLM, 소셜, Health Kit, 웹, 다국어, 상담 연결, 멀티모달, 위젯)을 명확히 제외.
- MVP P0 기능 9개(F-01~F-09)에 집중. P1(공유 카드) 1개는 출시 후 우선.
- DB 테이블 12개, Edge Function 8개, DB Function 2개 — 1인 개발 규모로 관리 가능.

#### 3개월 MVP 범위 현실성: **통과**
- Phase 1~4 로드맵(14장)이 논리적 순서:
  - Phase 1(1~4주): 환경 세팅 → 인증/온보딩 → 감정 태깅 → 캘린더/통계
  - Phase 2(5~8주): 셀프케어 → 오프라인 → 푸시 알림 → 통합 테스트
  - Phase 3(9~12주): 프리미엄 리포트 → 결제 → 내보내기 → QA/출시
- 각 주차별 작업과 산출물이 구체적.

#### DB 스키마·API 설계 정합성: **통과**
- 7-5장 DB 스키마: 12개 테이블 전체 CREATE TABLE SQL + FK 관계 + 인덱스 정의.
- ERD 개요(7-5장)에서 테이블 간 관계 명시:
  - `emotion_records` → M:N → `emotions`/`activities` (중간 테이블)
  - `emotions` → `emotion_categories` (FK)
  - `selfcare_emotion_map` → `emotions`/`selfcare_cards` (M:N 매핑)
  - `selfcare_feedback` → `users`/`cards`/`emotions`/`records`
- RLS 정책(7-5장)이 테이블별로 CRUD 권한 상세 정의.
- 7-6장 API 설계: REST vs Edge Function 선택 기준 명시, 전체 엔드포인트 목록(인증, 감정 기록, 마스터 데이터, 셀프케어, 리포트, 결제, 내보내기, 푸시, 프로필).
- 7-8장 기능-테이블-API 매핑표가 F-01~F-09 전체를 커버.

#### 핵심 DB Function SQL 본문: **통과**
- `create_emotion_record`: 55줄 완전한 PL/pgSQL — 인증 확인, 감정 1~3개 검증, 메모 200자 검증, 3종 INSERT(emotion_records, emotion_record_emotions, emotion_record_activities), 배열 순회. 스텁이 아닌 실행 가능한 코드.
- `get_weekly_summary`: 80줄 완전한 PL/pgSQL — 총 기록 수, 기록 날 수, 감정 빈도 Top 5, 긍정/부정 비율, 활동 빈도 Top 3 집계 후 JSONB 조립. 실행 가능한 코드.

#### 핵심 API TypeScript 인터페이스: **통과**
- 7종 인터페이스 정의:
  - `CreateEmotionRecordRequest` / `EmotionRecordWithRelations`
  - `RecommendSelfcareRequest` / `RecommendSelfcareResponse`
  - `WeeklyReportResponse` / `MonthlyReportResponse`
  - `VerifySubscriptionResponse` / `ExportResponse`
- 요청/응답 구조가 DB 스키마, API 엔드포인트와 정합.

#### 상태 관리 구조: **통과**
- 7-7장에서 Zustand Store 6개 정의:
  - AuthStore(인증), EmotionRecordStore(감정 기록 CRUD+동기화), MasterDataStore(마스터 데이터 캐싱), ReportStore(리포트), SubscriptionStore(구독, MMKV 캐시), UIStore(다크모드, 온라인 상태, 동기화 상태)
- 각 Store의 주요 상태 필드, Actions, 역할이 명시됨.

#### 오프라인 우선 설계: **통과**
- 7-3장에서 완전한 오프라인 아키텍처 다이어그램:
  1. WatermelonDB 즉시 로컬 저장 (0~50ms, sync_status='pending')
  2. Optimistic UI ("저장 완료" 즉시 표시, 오프라인 시 "(오프라인 저장됨)")
  3. Sync Queue: 최대 3회 재시도, 지수 백오프 (1s→2s→4s)
  4. NetInfo로 네트워크 상태 감지: 온라인→즉시 동기화, 복구 시→배치 동기화
  5. 충돌 해결: CREATE(UUID, 충돌 없음), UPDATE(Server Wins), DELETE(소프트 삭제 병합)
- 저장소 역할 분담: WatermelonDB(구조화 데이터), MMKV(KV 데이터)
- 핵심 원칙: "사용자의 감정 기록은 절대 유실하지 않는다"

#### 장애 시나리오 대응: **통과**
- 네트워크 장애: 오프라인 우선으로 완전 대응.
- 서버 다운: WatermelonDB 로컬에서 기록/조회 가능. 동기화는 복구 시 배치 처리.
- 인앱결제 장애: RevenueCat SDK가 영수증 검증 재시도 + Webhook으로 서버 동기화.

---

### D. 문서 품질 검증 — **통과**

#### 목표·기능·기술 명세 간 모순: **통과**
- Executive Summary(1장) "기록 → 분석 → 행동"이 Problem Statement(2장), Functional Requirements(5장), Screen Map(8장), 경쟁 차별화(9장), Success Metrics(15장)에서 일관 관통.
- 기능 요구사항(5장)과 DB 스키마(7-5장), API(7-6장), 매핑표(7-8장), Screen Map(8장)이 정합.
- 수익 모델(10장)과 구독 기능(F-09), 무료/유료 분할(AC-03-2, AC-06-1, AC-07-1)이 일치.
- 내부 모순 발견되지 않음.

#### 누락된 핵심 섹션: **통과** (15개 섹션 전체 존재)
1. Executive Summary
2. Problem Statement
3. Target Users & Personas
4. User Stories (8 Epic, 24개)
5. Functional Requirements (F-01~F-15)
6. Non-Functional Requirements
7. Technical Architecture (스택, 시스템 구조도, 오프라인 설계, DB 스키마, API, 상태 관리, 매핑표, 자체 점검)
8. Screen Map & UI 명세
9. 경쟁 차별화 전략
10. 수익 모델
11. 리스크 매트릭스
12. 전제 조건 & 제약 사항
13. 범위 제외
14. MVP 로드맵
15. 성공 지표

부록 2개: 한국어 감정 어휘 체계 (36종), 셀프케어 카드 분류 (100개+ 예시)

#### KPI 구체성·측정 가능성: **통과**
- 15-1장 핵심 KPI (출시 후 6개월 기준):
  - **획득**: 총 DL 50,000+, 온보딩 완료율 ≥70%, 첫 기록 전환율 ≥60%
  - **리텐션**: D1 ≥40%, D7 ≥25%, D30 ≥15%
  - **참여**: 주간 기록 ≥4회/유저, 셀프케어 "도움됨" ≥50%
  - **수익**: 유료 전환율 ≥3%, MRR ₩5,250,000 (12개월)
  - **만족**: 앱스토어 평점 ≥4.5
- 각 지표에 측정 방법(App Store Connect, Google Play Console, PostHog, RevenueCat) 명시.
- 15-2장 실패 기준 (Pivot 검토 트리거) 4개 명시: D7 <10%, 유료 전환율 <1%, 도움됨 <30%, MAU <3,000.

#### 수용 기준(AC): **통과**
- 5장 각 P0 기능(F-01~F-09)에 AC-XX-X 형식의 검증 가능한 수용 기준 존재:
  - F-01: AC-01-1~AC-01-5 (1초 응답, 활동 태그, 200자 제한, recorded_at, 오프라인 저장)
  - F-02: AC-02-1~AC-02-3 (캘린더, 주간 요약, 날짜 탭)
  - F-03: AC-03-1~AC-03-3 (카드 표시, 무료/유료 제한, 피드백 반영)
  - F-04: AC-04-1~AC-04-4 (인증, 프로필, 온보딩, 스킵)
  - F-05: AC-05-1~AC-05-3 (리마인더, 리포트 알림, 비활성화)
  - F-06: AC-06-1~AC-06-4 (프리미엄 전용, 주간, 월간, 데이터 부족)
  - F-07: AC-07-1 (무제한, 개인화)
  - F-08: AC-08-1~AC-08-2 (PDF 내용, CSV UTF-8 BOM)
  - F-09: AC-09-1~AC-09-3 (구독 반영, 만료 차단, 무료체험)
- 모든 AC가 조건문 형태로 검증 가능.

#### User Story 커버리지: **통과** (24개, 8 Epic)
- Epic 1 온보딩 & 인증 (US-01~US-03): 가입, 감정 체계 안내, 알림 설정
- Epic 2 감정 기록 (US-04~US-08): 10초 기록, 한국어 감정, 활동 태그, 오프라인, 수정/삭제
- Epic 3 캘린더 & 통계 (US-09~US-11): 월별 캘린더, 주간 요약, 일별 상세
- Epic 4 셀프케어 (US-12~US-13): 추천, 피드백
- Epic 5 프리미엄 (US-14~US-16): 상관관계, 월간 추이, PDF 내보내기
- Epic 6 설정 (US-17~US-19): 알림 변경, 앱 잠금, 계정 삭제
- Epic 7 결제 (US-20~US-22): 무료체험, 구독, 취소/변경
- Epic 8 알림 (US-23~US-24): 기록 리마인더, 주간 리포트 알림

모든 기능 영역(온보딩, 인증, 핵심 기능, 설정, 결제) 커버. 최소 15개 기준 초과 (24개).

#### Screen Map & UI 명세: **통과**
- 8-1장 네비게이션 구조 트리: Auth Stack / Main Tab Navigator (5개 탭) / Modal Stack — 전체 화면 19개 명시.
- 8-2장 핵심 화면별 UI 구성: HomeScreen, EmotionRecordModal, CalendarScreen, ReportScreen, SettingsScreen — 각 화면의 상단/중앙/하단 구성, 데이터 소스, API 연결 명시.

#### 기능-테이블-API 매핑표: **통과**
- 7-8장에서 F-01~F-09 모든 P0 기능이 관련 테이블, API 엔드포인트, 비고와 함께 매핑.

---

## 2. 미달 목록

**Critical 사항: 없음**

**Major 사항: 없음**

**Minor 사항:**

| # | 영역 | 미달 사항 | 심각도 | 유형 |
|---|------|----------|--------|------|
| 1 | C. 기술 | Edge Function `recommend-selfcare`의 비즈니스 로직이 자연어 서술(7-6장)만 존재하며 의사코드/SQL 수준 명세가 없음. `create_emotion_record`, `get_weekly_summary`처럼 SQL 본문까지는 아니더라도 핵심 쿼리(personal_score 계산, daily_card_usage UPSERT)의 SQL/TS 스니펫이 있으면 구현 명확성이 향상됨. | Minor | 문서 결함 |
| 2 | C. 기술 | `weekly-report`, `monthly-report` Edge Function의 인사이트 생성 로직이 미명세. "인사이트 문구 1~3개 포함"이 AC이나, 문구 생성 규칙(어떤 조건에서 어떤 문구 템플릿을 사용하는지)이 없음. SQL 집계 결과 → 인사이트 문구 변환 규칙이 명시되면 구현 시 해석 여지가 줄어듦. | Minor | 문서 결함 |
| 3 | D. 문서 | `emotion_categories` 대분류가 7종(기쁨, 평온, 슬픔, 분노, 불안, 스트레스, 기대)인데, DB 스키마의 `valence` CHECK는 `('positive', 'negative', 'neutral')` 3종. 부록 A의 7대 분류 중 '평온'은 neutral, '무기력/스트레스'는 negative로 매핑될 것으로 추정되나 **대분류별 valence 매핑이 명시적으로 기재되지 않음**. 구현 시 해석 차이 가능. | Minor | 문서 결함 |

---

## 3. 수정 지침 (Minor — 구현 단계 보완 권장)

### Minor #1: recommend-selfcare 비즈니스 로직 상세화

- **대상**: 7-6장 Edge Function 비즈니스 로직: `recommend-selfcare`
- **현 상태**: 8단계 자연어 서술(1. emotion_ids 추출 → 2. is_premium 확인 → ... → 8. 반환). 핵심 산식 `personal_score = relevance_score * 0.4 + helpful_rate * 0.6`이 자연어로만 기술.
- **수정 방향**: `personal_score` 계산 SQL/TS 스니펫, `daily_card_usage` UPSERT 쿼리를 추가하면 Task Master 투입 시 해석 여지 최소화. 다만 TypeScript 인터페이스(`RecommendSelfcareRequest/Response`)와 8단계 로직 흐름이 충분히 구체적이므로 **구현 단계에서 보완 가능**.

### Minor #2: 인사이트 문구 생성 규칙

- **대상**: F-06 (AI 감정 패턴 리포트), AC-06-2 "인사이트 문구 1~3개 포함"
- **현 상태**: 인사이트 예시만 제공("화요일 오후에 '답답함'이 집중됩니다", "운동한 날은 긍정 감정이 2배 높습니다"). 문구 생성의 조건/템플릿 규칙 부재.
- **수정 방향**: 인사이트 유형(요일 집중, 활동 상관, 시간대 패턴, 전주 대비 등) × 조건(특정 요일 N% 이상 집중 시, 활동-긍정률 차이 2배 이상 시 등) × 문구 템플릿을 정의. 다만 이는 구현 단계에서 자연스럽게 결정되는 부분이므로 **PRD 수준에서 반드시 필요하지는 않음**.

### Minor #3: 감정 대분류 valence 매핑

- **대상**: 부록 A 감정 어휘 체계 / DB 스키마 `emotion_categories.valence`
- **현 상태**: 부록 A에 날씨 이모지(☀️ 기쁨, 🌤️ 평온, 🌧️ 슬픔, ⛈️ 분노, 🌫️ 불안, 🌪️ 스트레스, 🌈 기대)가 있고 스키마에 `valence IN ('positive', 'negative', 'neutral')`이 있으나, 대분류↔valence 명시적 매핑 없음.
- **수정 방향**: 부록 A 또는 DB 스키마에 아래 매핑 추가:

| 대분류 | valence |
|--------|---------|
| 기쁨 (Joy) | positive |
| 평온 (Calm) | neutral |
| 슬픔 (Sadness) | negative |
| 분노 (Anger) | negative |
| 불안 (Anxiety) | negative |
| 스트레스 (Stress) | negative |
| 기대 (Anticipation) | positive |

구현 시 이 매핑을 seed 데이터로 사용하므로 명시하는 것이 바람직하나, 맥락상 자명하므로 Critical/Major에 해당하지 않음.

---

## 4. 최종 판정

### 판정: **통과** → 5단계 채택

| 판정 근거 | 내용 |
|----------|------|
| Critical 사항 | **없음** |
| Major 사항 | **없음** |
| Minor 사항 | 3건 (recommend-selfcare 상세화, 인사이트 규칙, valence 매핑) — 모두 문서 결함, 구현 단계 보완 권장 |
| 아이디어 결함 | **없음** |

v3.0 PRD는 Stage 3 완전 재작성을 통해 **CLAUDE.md에 정의된 Task Master 투입 요건을 모두 충족**한다:

| Task Master 투입 요건 | 충족 여부 | 근거 |
|---------------------|----------|------|
| 기능 명세 (P0/P1/P2 우선순위, 입력/출력/제한사항) | **충족** | F-01~F-15, 5장에서 P0/P1/P2 분리, 각 기능별 입출력·제한사항 명시 |
| 수용 기준 (AC-XX-X 형식 조건문) | **충족** | AC-01-1~AC-09-3, 모든 P0 기능에 검증 가능한 조건문 |
| DB 스키마 (CREATE TABLE, FK, 인덱스, RLS, 핵심 Function SQL) | **충족** | 12개 테이블 SQL, FK/인덱스/제약조건, RLS 정책 요약표, `create_emotion_record`(55줄), `get_weekly_summary`(80줄) |
| API 설계 (엔드포인트, TS 인터페이스, 비즈니스 로직) | **충족** | 전체 엔드포인트 30+개, TS 인터페이스 7종, Edge Function 로직 서술 |
| 아키텍처 상세도 (기술 스택, 시스템 구조도, 상태 관리, 매핑표) | **충족** | 7-1~7-9 (스택, 구조도, 상태 관리 6종, 매핑표 F-01~F-09) |
| 화면 구조 / UI 명세 (네비게이션 트리, 화면별 UI, 화면-데이터-API) | **충족** | 8-1 네비게이션 트리(19개 화면), 8-2 핵심 화면별 UI·데이터·API |
| 오프라인/동기화 설계 (로컬 저장소, 동기화 큐, 충돌 해결) | **충족** | 7-3 다이어그램, WatermelonDB/MMKV 분담, Sync Queue(지수 백오프), Server Wins |

### 전환 안내

> **5단계 채택**: `/stage-5 002-마음날씨 채택` 을 실행하세요.
>
> Minor 3건은 구현 단계에서 보완하면 됩니다.

---

## 부록: 검증 요약 매트릭스

| 검증 영역 | 판정 | 근거 요약 |
|----------|------|----------|
| **A. 사업성** | **통과** | TAM/SAM/SOM이 2단계 산출물과 일치, 차별점·수익 모델이 벤치마크 기반, 포지셔닝 일관 |
| **B. 리스크** | **통과** | 6개 리스크(사업/기술/법률/의존성/운영/심사) 체계적 식별, 대응 구체적, 외부 의존 최소화 |
| **C. 기술** | **통과** | 12개 테이블 + 2개 DB Function SQL 본문 + 7종 TS 인터페이스 + 6개 Store + 오프라인 아키텍처 + 매핑표. Task Master 투입 가능 수준 |
| **D. 문서 품질** | **통과** | 15개 섹션 + 부록 2개. 24개 User Story (8 Epic). 모든 P0에 AC. KPI 구체적. 내부 모순 없음. Screen Map + UI 명세 존재 |
