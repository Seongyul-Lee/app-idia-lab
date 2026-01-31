# 005-절약챌린지 PRD 정합성 판별 보고서

> **판별일**: 2026-01-31
> **대상 문서**: `ideas/005-절약챌린지-prd.md` (v1.0)
> **참조 문서**: `ideas/005-절약챌린지.md`, `research/competitors/005-경쟁분석.md`, `research/market-data/005-시장분석.md`

---

## A. 사업성 검증 — **통과**

### A-1. 시장 규모·경쟁분석 정합성

PRD의 시장 수치(TAM 990만 명, SAM 119만 명, SOM 2.38만 유료/47.6만 총 사용자)는 2단계 시장분석 산출물과 **완전히 일치**한다. 인구 통계(20~39세 1,237만 명), 절약 관심 비율(80.7%), 무지출 챌린지 적극 참여층(12%) 등 산출 논거가 동일하다.

경쟁분석도 PRD 섹션 9(Competitive Differentiation)에 2단계 산출물의 경쟁자 3사(뱅크샐러드 샐러드게임, 카카오페이 카드 절약 챌린지, 챌린저스)와 간접 경쟁자(거지방, 인스타그램 등)가 모두 반영되어 있다. 각 경쟁사의 강점/약점 분석이 일관적이다.

### A-2. 차별점 방어 가능성

"극단적 단순화(1탭 인증) + 스트릭 기반 장기 습관 형성 + 앱 내 절약 커뮤니티 + 독립 전용 앱"이라는 차별점은 2단계 경쟁분석에서 도출된 전략과 일치한다. 다만 이 차별점들은 기술적 해자가 아닌 UX/포지셔닝 차별점이므로 **모방 장벽이 낮다**는 리스크가 존재하지만, PRD 섹션 9에서 경쟁 대응 전략(커뮤니티 네트워크 효과로 방어)을 명시하고 있어 인지하고 있음을 확인.

### A-3. 수익 모델

프리미엄 구독(월 2,900원) + 광고 하이브리드 모델이 구체적으로 기술되어 있다. SOM 기준 연 11.6억 원(구독 8.3억 + 광고 3.3억) 전망은 2단계 시장분석의 SOM 합계 13억 원과 대체로 일치(PRD에서 구독 단가를 2,900원으로 확정하면서 소폭 조정). 절약 앱 사용자의 과금 민감성 리스크도 섹션 11(Risk Matrix)에서 대응 방안과 함께 명시.

### A-4. TAM/SAM/SOM 논리적 근거

- TAM: 행정안전부 인구통계 + 한국방송광고진흥공사 조사 → 논거 있음
- SAM: TAM의 12%(카카오톡 거지방 운영 규모, 챌린저스 171만 사용자 등 근거) → 합리적
- SOM: SAM의 2%(1인 개발 앱의 현실적 침투율) → 보수적이고 현실적

**판정: 통과**

---

## B. 리스크 검증 — **통과**

### B-1. 핵심 리스크 식별 및 대응

PRD 섹션 11(Risk Matrix)에 10개 리스크가 사업/기술/운영/법률/의존성 분류별로 식별되어 있다:

| 분류 | 리스크 수 | 대응 방안 유무 |
|------|-----------|--------------|
| 사업 | 3개 | 모두 있음 |
| 기술 | 2개 | 모두 있음 |
| 운영 | 2개 | 모두 있음 |
| 법률 | 1개 | 있음 |
| 의존성 | 2개 | 모두 있음 |

핵심 리스크(대기업 진출, 유료 전환 저항, 커뮤니티 콜드 스타트)에 대한 대응 방안이 구체적으로 기술되어 있다.

### B-2. 외부 의존성 관리

섹션 12(Assumptions & Constraints)에 외부 의존성 5개(Supabase, RevenueCat, Expo Push, App Store/Play Store, Kakao OAuth)가 리스크 수준과 함께 명시. Supabase는 오픈소스 기반 셀프호스팅 전환 가능, RevenueCat은 react-native-iap 직접 구현으로 대체 가능 등 대안이 제시되어 있다.

**판정: 통과**

---

## C. 기술 검증 — **통과 (Minor 1건)**

### C-1. 기술 스택 구현 가능성

React Native (Expo Managed Workflow) + TypeScript + Supabase로 명세된 모든 기능이 구현 가능하다. 캘린더(react-native-calendars), 애니메이션(Lottie), 결제(RevenueCat), 로컬 DB(op-sqlite), KV 저장소(MMKV) 등 모든 라이브러리 선택이 적절하다.

### C-2. 1인 개발 규모 적정성

MVP 범위가 P0 9개 기능 + P1 7개로 구성되어 있으며, 복잡한 실시간 기능(채팅 등)은 명시적으로 제외되어 있다. 커뮤니티는 비동기 피드 방식으로 설계되어 1인 개발에 적합하다.

### C-3. 3개월 MVP 범위 현실성

12주 로드맵이 주 단위로 상세 기술되어 있다. Phase 1(기반, 3주) → Phase 2(핵심 기능, 4주) → Phase 3(소셜+마무리, 5주) 구조는 현실적이다. 마일스톤 체크포인트 5개로 진행 관리가 가능하다.

### C-4. DB 스키마·API 정합성

11개 테이블(profiles, daily_records, challenges, challenge_participants, badge_definitions, user_badges, community_posts, post_likes, post_comments, reports, xp_levels)이 정의되어 있다. FK 관계, 인덱스, CHECK 제약조건이 모두 명시. RLS 정책이 CRUD 권한 매트릭스와 함께 완전한 SQL로 작성되어 있다.

API 엔드포인트 25개가 Supabase REST + DB Function(RPC) + Edge Function으로 적절히 분배되어 있으며, 기능-테이블-API 매핑표(섹션 7-8)로 모든 P0 기능이 매핑되어 있다.

### C-5. 장애 시나리오 대응

오프라인 동작(인증·캘린더·스트릭), 네트워크 복귀 시 동기화, SyncQueue 재시도 로직(지수 백오프, 최대 5회), 실패 시 사용자 알림 등이 명세되어 있다. Supabase 장애 시 오프라인 우선 설계로 핵심 기능 로컬 동작, RevenueCat 장애 시 grace period 등 대응이 기술되어 있다.

### C-6. 오프라인 우선 설계

- 저장소별 역할 분담: MMKV(KV 데이터), op-sqlite(관계형 데이터), Supabase Storage(이미지) — 명확히 분리
- 동기화 큐: MMKV 기반 SyncQueue 구조(id, type, table, payload, created_at, retry_count, next_retry_at) — 구체적
- 충돌 해결: Server Wins 정책 + updated_at 기반 비교 — 명시적

### C-7. 핵심 DB Function SQL 본문

6개 RPC(upsert_daily_record, get_monthly_summary, join_challenge, update_challenge_progress, toggle_post_like, handle_new_user)가 **완전한 SQL 본문**으로 작성되어 있다. 스텁이 아님. 비즈니스 로직(스트릭 계산, 배지 부여, XP 갱신, 레벨업 등)이 SQL 내에서 처리된다.

### C-8. TypeScript 인터페이스

핵심 API의 요청/응답 인터페이스가 모두 작성되어 있다: Profile, DailyRecord, UpsertDailyRecordRequest/Response, Challenge, ChallengeParticipant, JoinChallengeRequest/Response, BadgeDefinition, UserBadge, CommunityPost, PostComment, MonthlySummary 등.

### C-9. 상태 관리 구조

Zustand Store 6개(AuthStore, RecordStore, ChallengeStore, CommunityStore, BadgeStore, SyncStore)가 필드·타입·역할과 함께 정의되어 있다.

### Minor 발견 사항

- **M-01** (Minor, 문서 결함): `upsert_daily_record` RPC의 스트릭 계산 로직에서, 윈도우 함수를 사용한 서브쿼리(951행)를 시도한 뒤 "동작하지 않을 수 있으므로"라며 반복문으로 재구현하는 코드가 공존한다. 최종적으로 반복문이 실행되어 동작에는 문제가 없으나, 첫 번째 서브쿼리의 결과(`v_new_streak`)가 즉시 0으로 덮어써지므로 **불필요한 쿼리가 먼저 실행**된다. 성능에 미미한 영향이지만 코드 정리가 권장된다.

**판정: 통과** (Minor 1건, 구현 시 정리하면 충분)

---

## D. 문서 품질 검증 — **통과**

### D-1. 목표·기능·기술 명세 간 모순

- Executive Summary의 핵심 기능 3가지(무지출 캘린더 & 스트릭, 절약 챌린지 시스템, 절약 커뮤니티)가 Functional Requirements(FR-02~FR-07)와 정확히 대응
- 기술 스택 선택이 기능 요구사항과 정합 (예: react-native-calendars → FR-03, Lottie → FR-06 축하 애니메이션)
- 수익 모델(프리미엄 월 2,900원)이 기능 요구사항(FR-15, FR-11 커스텀 챌린지 유료)과 일치
- **모순 발견되지 않음**

### D-2. 15개 섹션 존재 여부

| # | 섹션 | 존재 |
|---|------|------|
| 1 | Executive Summary | O |
| 2 | Problem Statement | O |
| 3 | Target Users & Personas | O |
| 4 | User Stories | O |
| 5 | Functional Requirements | O |
| 6 | Non-functional Requirements | O |
| 7 | Technical Architecture | O |
| 8 | Screen Map & UI 명세 | O |
| 9 | Competitive Differentiation | O |
| 10 | Monetization Strategy | O |
| 11 | Risk Matrix | O |
| 12 | Assumptions & Constraints | O |
| 13 | Out of Scope | O |
| 14 | MVP Roadmap | O |
| 15 | Success Metrics | O |

**15개 섹션 모두 존재.**

### D-3. KPI 구체성·측정 가능성

섹션 15에 3개 카테고리(사용자/비즈니스/제품)의 KPI가 수치 목표·측정 방법과 함께 정의:
- 사용자: MAU 10,000명, DAU 3,000명, DAU/MAU 30%, D7 40%, D30 20%
- 비즈니스: 프리미엄 전환율 3%, MRR 300만 원, ARPU 300원
- 제품: 일일 인증률 70%, 평균 스트릭 5일, 챌린지 참여율 50%
- North Star Metric: 주간 무지출 인증 횟수

모두 **구체적이고 측정 가능**하다.

### D-4. P0 수용 기준 검증

| P0 기능 | AC 번호 | 조건문 형태 |
|---------|---------|------------|
| FR-01: 소셜 로그인 & 온보딩 | AC-01-1 ~ AC-01-4 | O |
| FR-02: 무지출 인증 | AC-02-1 ~ AC-02-4 | O |
| FR-03: 무지출 캘린더 | AC-03-1 ~ AC-03-4 | O |
| FR-04: 스트릭 시스템 | AC-04-1 ~ AC-04-4 | O |
| FR-05: 공식 챌린지 참여 | AC-05-1 ~ AC-05-4 | O |
| FR-06: 배지 & 레벨 | AC-06-1 ~ AC-06-4 | O |
| FR-07: 절약방 | AC-07-1 ~ AC-07-4 | O |
| FR-08: 일일 리마인더 | AC-08-1 ~ AC-08-3 | O |
| FR-09: 계정 관리 | AC-09-1 ~ AC-09-4 | O |

**모든 P0 기능에 AC-XX-X 형식의 검증 가능한 조건문이 존재.**

### D-5. User Story 커버리지

8개 Epic, 24개 User Story:
- Epic 1: 온보딩 & 인증 (4개) ✓
- Epic 2: 무지출 캘린더 & 스트릭 (6개) ✓
- Epic 3: 절약 챌린지 시스템 (5개) ✓
- Epic 4: 절약 커뮤니티 (4개) ✓
- Epic 5: 리포트 & 통계 (2개) ✓
- Epic 6: 알림 & 리마인더 (3개) ✓
- Epic 7: 설정 & 계정 관리 (4개) ✓
- Epic 8: 결제 & 프리미엄 (3개 — US-08-3은 무료 사용자 관점) ✓

온보딩·인증·핵심 기능·설정·결제 등 **모든 기능 영역을 커버** (최소 15개 요건 충족, 실제 24개).

### D-6. Screen Map & 핵심 화면 UI

- 네비게이션 구조 트리: Expo Router 기반 (auth) → (main) → 5개 탭 + Modal Stack 구조가 명시
- 핵심 화면 12개(로그인, 온보딩, 홈, 지출 입력 모달, 캘린더, 챌린지 목록/상세, 절약방 피드/상세, 프로필/설정, 배지함, 배지 획득 모달)의 UI 구성(영역별 구성 요소, 데이터, API)이 기술

### D-7. 기능-테이블-API 매핑표

섹션 7-8에 매핑표 존재. 모든 P0 기능(FR-01~FR-09)이 관련 테이블, 관련 API, 비고와 함께 매핑 완료.

**판정: 통과**

---

## 미달 목록

| # | 심각도 | 유형 | 영역 | 내용 |
|---|--------|------|------|------|
| M-01 | Minor | 문서 결함 | C (기술) | `upsert_daily_record` RPC 내 스트릭 계산에서 불필요한 윈도우 함수 서브쿼리가 먼저 실행된 뒤 즉시 0으로 덮어써지는 중복 코드 존재. 동작에 영향 없으나 정리 권장. |

---

## 최종 판정

### **통과** → 5단계 채택

**근거:**
- A~D 4개 영역 모두 통과
- Critical/Major 미달 사항 없음
- Minor 1건(M-01)은 구현 단계에서 코드 정리로 해결 가능하며, PRD 수정이 필수적이지 않음
- PRD가 15개 섹션을 모두 포함하고, Task Master 투입 요건(기능 명세, 수용 기준, DB 스키마, API 설계, 아키텍처 상세도, 화면 구조, 오프라인/동기화 설계)을 충족함
- 2단계 산출물(시장분석, 경쟁분석)과의 정합성이 확인됨

---

## 다음 단계

> **5단계: `/stage-5 005-절약챌린지 채택`**
