CLAUDE.md의 규칙을 준수하며 3단계 PRD 문서를 작성한다.

대상 아이디어: $ARGUMENTS

## 목적
채택된 아이디어에 대해 구체적인 제품 요구사항 문서를 작성한다.

## 입력
- `ideas/NNN-아이디어명.md` — 평가 완료 아이디어 문서
- `research/competitors/NNN-경쟁분석.md` — 경쟁사 분석
- `research/market-data/NNN-시장분석.md` — 시장 분석

## 사용 도구
| 도구 | 역할 |
|------|------|
| `/create-prd` | PRD 초안 빠른 생성 (Executive Summary, Problem Statement, User Stories, Requirements 등) |
| `/sc-design --type architecture` | React Native + Supabase 기반 시스템 아키텍처 설계 |
| `/sc-design --type database` | Supabase PostgreSQL DB 스키마 설계 |
| `/sc-design --type api` | API 엔드포인트 명세 설계 |
| `content-research-writer` (스킬) | PRD 각 섹션 리서치 기반 보강, 인용 추가, 반복 개선 |
| `/sc-research` | PRD 작성 중 추가 데이터 필요 시 보충 리서치 |

## 절차
1. `/create-prd`로 PRD 초안 생성
2. `/sc-design`으로 기술 설계 섹션 보강 (아키텍처, DB, API)
3. `content-research-writer`로 각 섹션 리서치 기반 보강
4. 2단계 산출물(시장 분석, 경쟁사 분석)을 PRD에 반영
5. PRD 문서 완성 후 사용자 리뷰

## 산출물
| 파일 | 위치 |
|------|------|
| PRD 문서 | `ideas/NNN-아이디어명-prd.md` |

## 전환
- PRD 전체 섹션 작성 완료, 사용자 확인 → "4단계: `/stage-4 NNN-아이디어명`" 안내
- 작성 중 기술적 실현 불가 판단 → "2단계 회귀: `/stage-2 NNN-아이디어명`" 안내
