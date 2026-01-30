CLAUDE.md의 규칙을 준수하며 2단계 사업성/기술/경쟁력 평가를 수행한다.

대상 아이디어: $ARGUMENTS

## 목적
`evaluation/criteria.md`의 6개 항목(MVP 출시 가능성, Pain Point 명확성, 기술 스택 적합성, 시장 규모, 경쟁 강도, 수익 모델 명확성)을 정량 평가한다.

## 입력
- `ideas/`에서 대상 아이디어 문서를 읽는다
- 기존 `research/market-data/` 리서치가 있으면 참고한다

## 사용 도구
| 도구 | 역할 |
|------|------|
| `market-research` (스킬) | MBB 방법론 기반 TAM/SAM/SOM 추정, 경쟁 환경 분석, 고객 니즈 검증 |
| `/sc-business-panel` | Porter·Christensen·Drucker 등 다중 전문가의 비즈니스 관점 분석. 기본 `--mode discussion`, 쟁점이 있으면 `--mode debate` |
| `/sc-research --depth deep` | 경쟁사 상세 조사, 시장 규모 데이터, 사용자 리뷰 분석 |
| `competitive-ads-extractor` (스킬) | 경쟁 앱의 광고 메시징·크리에이티브 전략 분석 (광고 집행하는 경우만) |
| MCP: `naver-search` | 네이버 쇼핑/블로그/카페 데이터로 한국 시장 수요 검증 |
| MCP: `serpapi` | 시장 규모, 경쟁사 정보, 뉴스 수집 |
| MCP: `fetch` | 앱 스토어 정보, 경쟁 앱 랜딩페이지 크롤링 |

## 절차
1. `market-research` 스킬로 시장 규모·경쟁·고객 분석 실행
2. MCP 서버들로 경쟁사 구체 정보 수집 (앱 스토어, 리뷰, 랜딩페이지)
3. `competitive-ads-extractor`로 경쟁사 광고 전략 분석 (경쟁 앱이 광고를 집행하는 경우)
4. `/sc-business-panel`로 다중 전문가 비즈니스 분석
5. `evaluation/criteria.md` 기준으로 6개 항목 점수 기입
6. 총점에 따른 판정 결과를 사용자에게 보고

## 산출물
| 파일 | 위치 |
|------|------|
| 평가 완료 아이디어 문서 | `ideas/NNN-아이디어명.md` (점수 기입 완료) |
| 경쟁사 분석 | `research/competitors/NNN-경쟁분석.md` |
| 시장 분석 | `research/market-data/NNN-시장분석.md` |

## 전환
- 24-30점 채택 → "3단계: `/stage-3 NNN-아이디어명`" 안내
- 18-23점 보류 → 미달 항목 보고 후 사용자 판단 요청 (추가 조사 / 피벗 / 탈락 / 예외 채택)
- ~17점 탈락 → "5단계: `/stage-5 NNN-아이디어명 탈락`" 안내 (사용자 동의 필수)

### 보류 흐름 (18-23점)
1. 미달 항목을 식별하여 사용자에게 보고
2. 사용자 판단에 따라:
   - **추가 조사**: 미달 항목을 보강한 뒤 2단계 재평가
   - **피벗**: 아이디어 방향을 수정하여 1단계로 회귀
   - **탈락 처리**: 사용자 동의 후 5단계로 이동
   - **예외 채택**: 사용자가 명시적으로 점수와 무관하게 3단계 진행 결정
