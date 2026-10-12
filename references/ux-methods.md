# UX·전략 컨설팅 방법론 앵커

> 이 스킬이 쓰는 모든 방법론의 **정의·수행절차·척도·출처**. 작업 시작 시 먼저 읽고, 방법론을 인용할 때마다 출처를 같이 댄다. 기억으로 지어내지 말 것. (링크 확인일: 2026-10-12. 「2차 해설」 표시가 붙은 링크는 원저자·원기관이 아닌 해설 자료)

핵심 통찰: **"생각은 아래에서 위로, 말은 위에서 아래로"** — 분석은 데이터에서, 보고는 결론부터.

---

## A. 발산-수렴 프로세스

### Double Diamond (UK Design Council)
문제 탐색(발산)→문제 정의(수렴)→해법 탐색(발산)→해법 전달(수렴).

| 단계 | 방향 | 활동 |
|---|---|---|
| Discover | 발산 | 가정 없이 이해. 인터뷰·관찰·리서치 |
| Define | 수렴 | 인사이트를 HMW(How Might We)로 과제 재정의 |
| Develop | 발산 | 다양한 해법 생성·공동설계·프로토타이핑 |
| Deliver | 수렴 | 소규모 테스트→개선 반복→출시 |

출처: https://www.designcouncil.org.uk/resources/the-double-diamond/

### Design Thinking 5단계 (Stanford d.school — Hasso Plattner Institute of Design)
Empathize(공감)→Define(POV 작성: "사용자는 ___가 필요하다, 왜냐하면 ___")→Ideate(판단 없이 발산)→Prototype(저렴·빠르게)→Test(다시 공감으로 회귀). 비선형 반복.
출처(2차 해설): https://ixdf.org/literature/article/5-stages-in-the-design-thinking-process

---

## B. UX 진단·평가

### 휴리스틱 평가 (Heuristic Evaluation) — Jakob Nielsen / NN/g
전문가가 10원칙에 비춰 독립 점검. 평가자 **3~5명** 최적.
절차: ①평가자 선정 → ②각자 1~2시간 독립 평가 → ③severity 독립 채점 → ④집계·토론.
출처: https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/

**Severity Rating 0~4** (출시 의사결정 척도):
- 0 = 사용성 문제 아님
- 1 = 미관상 (여유 있을 때)
- 2 = 경미 (낮은 우선순위)
- 3 = 주요 (높은 우선순위)
- 4 = 재앙적 (출시 전 반드시 수정)

severity = 빈도·영향·지속성·시장영향을 종합해 하나의 점수로 매기는 판단.
출처: https://www.nngroup.com/articles/how-to-rate-the-severity-of-usability-problems/

**Nielsen 10 휴리스틱** (대조 체크리스트):
1. 시스템 상태의 가시성 2. 시스템-현실 일치 3. 사용자 통제·자유(취소·되돌리기) 4. 일관성·표준 5. 오류 예방 6. 인식 > 회상(기억 부담↓) 7. 사용의 유연성·효율 8. 미니멀 디자인 9. 오류 인지·진단·복구 지원 10. 도움말·문서
출처: https://www.nngroup.com/articles/ten-usability-heuristics/

### Cognitive Walkthrough (인지적 워크스루)
신규 사용자 관점에서 태스크 각 단계를 걸으며 4질문. 복잡한 신규 플로우·키오스크용 (표준 패턴엔 과분).
각 단계 4질문: Q1 사용자가 올바른 결과를 시도하려는가? Q2 올바른 행동이 보이는가? Q3 그 행동이 결과로 이어진다고 인식하는가? Q4 행동 후 진행 피드백을 인식하는가? 하나라도 No면 Fail.
출처: https://www.nngroup.com/articles/cognitive-walkthroughs/

### UX 성숙도 6단계 (NN/g, 조직 자가진단 메타툴)
1 Absent → 2 Limited → 3 Emergent → 4 Structured → 5 Integrated → 6 User-Driven. 4축: Strategy·Culture·Process·Outcomes. 단계 건너뛰기 불가.
출처: https://www.nngroup.com/articles/ux-maturity-model/

---

## C. 사용자·여정 도구

### Empathy Map — Says / Thinks / Does / Feels
질적 리서치 수집 → 사분면 작성 → 군집화·갭 확인. 페르소나 생성 전, 팀 정렬용.
출처: https://www.nngroup.com/articles/empathy-mapping/

### Journey Map — Phases / Actions / Thoughts / Emotions
특정 페르소나의 특정 목표 달성 과정을 시간순 추적. 감정 곡선의 이탈점(저점)이 개입 포인트.
출처: https://www.nngroup.com/articles/ux-mapping-cheat-sheet/

### Service Blueprint (옴니채널·다부서 서비스)
5레이어: 물리적 증거 / 고객 행동 / 프론트스테이지(보이는 직원·시스템) / 백스테이지(안 보이는 내부) / 지원 프로세스. 3구분선: 상호작용선·가시성선·내부상호작용선. 저니맵의 후속.
출처: https://www.nngroup.com/articles/service-blueprints-definition/

### JTBD (Jobs-to-be-Done) — Christensen
"사용자는 인구통계가 아니라 특정 상황의 Job 때문에 제품을 고용한다." 3유형: 기능적·사회적·감성적.
예: "1/4인치 드릴을 원한다"가 아니라 "1/4인치 구멍을 원한다"(Levitt/Christensen) → 사용자가 고용하려는 진짜 결과를 설계 대상으로.
출처: https://hbr.org/2016/09/know-your-customers-jobs-to-be-done
드릴 예시 출처: Christensen, Cook, Hall, "Marketing Malpractice: The Cause and the Cure", HBR 2005 — https://hbr.org/2005/12/marketing-malpractice-the-cause-and-the-cure (본문 유료, 초록: https://pubmed.ncbi.nlm.nih.gov/16334583/)

---

## D. 정보구조(IA)

### 카드 소팅 — 멘탈 모델 발견 (IA 초반)
Open(그룹명도 사용자가 생성·신규설계) / Closed(기존 카테고리에 분류·검증) / Hybrid(일부 카테고리만 미리 주고 추가 허용).
출처: https://www.nngroup.com/articles/card-sorting-definition/

### 트리 테스트 — IA 검증 (카드소팅 후)
디자인 없는 텍스트 트리에서 "X는 어디 있나?" 태스크. 성공률·소요 시간·경로(directness) 분석.
출처: https://www.nngroup.com/articles/card-sorting-tree-testing-differences/ , https://www.nngroup.com/articles/tree-testing/

> 주의: 실제 카드소팅·트리테스트 실행은 실사용자 대상 팀 몫. 이 스킬은 **설계·권고**까지.

---

## E. 성공지표

### HEART (Google) + Goals-Signals-Metrics
Happiness(만족·NPS) / Engagement(참여·DAU·세션깊이) / Adoption(신규·신기능채택) / Retention(유지·이탈률) / Task Success(완료율·오류·시간).
프로세스: 목표(Goal)→사용자행동 신호(Signal)→정량지표(Metric)로 각 항목 매핑.
출처: Rodden, Hutchinson, Fu, "Measuring the User Experience on a Large Scale", CHI 2010 — https://research.google.com/pubs/archive/36299.pdf (저자 소개 페이지: https://kerryrodden.com/heart/)

### North Star Metric (Amplitude)
고객이 받는 핵심 가치를 포착하는 단 하나의 지표. 수익의 선행지표 + 팀이 영향 가능 + 사용자 가치 표현. Input Metrics 3~5개 동반.
출처: https://amplitude.com/blog/product-north-star-metric

### NN/g UX Strategy 3구성요소
Vision(사용자중심 미래상·차별화) + Goals & Measures(측정가능 목표+KPI, 수익모델 연동) + Plan(목표별 로드맵). 지원: People·Process·Tools(DesignOps).
출처: https://www.nngroup.com/articles/ux-strategy/

---

## F. 문제 구조화 (전략 컨설팅)

### MECE
서로 겹치지 않고(ME) 합치면 전체 포괄(CE). Barbara Minto가 피라미드 원칙과 함께 정리한 개념으로 알려짐. 실전은 80% 수준으로 반복 정제 — 완벽 달성 아닌 지향점.
출처(2차 해설): https://deckary.com/blog/pyramid-principle-consulting , https://www.betterup.com/blog/minto-pyramid (Minto 공식 사이트 https://www.barbaraminto.com/ 에서는 MECE 언급 확인 안 됨)

### 이슈 트리 / 가설 트리
최상단 SMART 질문 → 1단계 3~5개 MECE 가지 → 세분화 → MECE 검증 → 80/20 우선순위. 방향 잡히면 이슈트리(탐색)→가설트리(이것이 원인)로 전환.
3분해법: 이해관계자 / 프로세스 / 수식(수익=가격×거래량). McKinsey식으로 알려진 방법.
출처(2차 해설, 케이스 인터뷰 준비 사이트): https://www.hackingthecaseinterview.com/pages/issue-trees

### 가설 기반 접근
Day-1 가설 먼저(근거 부족해도) → 분석으로 증명/기각 → 기각되면 즉시 다음 가지. 좋은 가설: 테스트가능·반박가능·결과영향·행동연결·이미 아는 것 아님. McKinsey식으로 알려진 방법.
출처(2차 해설, 템플릿 판매 사이트): https://slideworks.io/resources/mckinsey-problem-solving-process

---

## G. 커뮤니케이션 (피라미드 원칙 — Barbara Minto)

3층: 정점(핵심 권고=Governing Thought) → 중간(MECE 3~5 논거) → 하단(데이터).

**SCQA** (오프닝):
- Situation: 청중이 이미 아는 현재 상태
- Complication: 무엇이 바뀌었나/위협
- Question: 거기서 떠오르는 질문
- Answer: 피라미드 정점 = 당신의 권고

**원칙**: Answer First (결론부터, 연역적). So-What 테스트(권고에 직결 안 되면 삭제). Action Title(슬라이드 제목은 주제 아닌 결론 문장 — "고객만족 현황" ✗ / "만족도 하락으로 연 12억 매출 위험" ✓).
출처: Barbara Minto, *The Pyramid Principle* (저서) · https://www.barbaraminto.com/
2차 해설: https://deckary.com/blog/pyramid-principle-consulting , https://www.betterup.com/blog/minto-pyramid

---

## H. 우선순위·의사결정

| 상황 | 도구 |
|---|---|
| 5~20개, 워크숍 현장 | Impact/Effort 2x2 |
| 30개+, Reach 편차 큼 | RICE |
| 다기준 고위험 | Weighted Scoring |

**RICE** = (Reach × Impact × Confidence%) / Effort
- Reach: 분기 영향 사용자 수 / Impact: 3=대형·2=높음·1=보통·0.5=낮음·0.25=미미 / Confidence: High100%·Med80%·Low50% / Effort: person-months
출처: https://www.intercom.com/blog/rice-simple-prioritization-for-product-managers/

**Value Proposition Canvas** (Strategyzer): 고객 프로파일(Jobs·Pains·Gains) ↔ 가치 맵(제품·Pain Relievers·Gain Creators)을 짝지어 fit을 확인.
출처: https://www.strategyzer.com/library/the-value-proposition-canvas

---

## I. 컨설팅 7단계 (전체 흐름 — McKinsey식으로 알려진 방법)
1 Define(문제 정의) → 2 Disaggregate(이슈트리로 분해) → 3 Prioritize(우선순위·가지치기) → 4 Workplan(작업계획) → 5 Analyze(분석) → 6 Synthesize(결과 종합) → 7 Tell the story(설득하는 스토리로 전달). 7단계에서 쓰는 SCQA·피라미드는 Minto에서 온 것(G 참조).
출처(7단계 이름): Charles Conn & Robert McLean, *Bulletproof Problem Solving* (Wiley, 2019) — https://www.wiley.com/en-us/Bulletproof+Problem+Solving%3A+The+One+Skill+That+Changes+Everything-p-9781119553021
참고(2019 McKinsey 팟캐스트): https://www.mckinsey.com/capabilities/strategy-and-corporate-finance/our-insights/how-to-master-the-seven-step-problem-solving-process
