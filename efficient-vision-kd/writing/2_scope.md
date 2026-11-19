# 2절 Scope & Prior Surveys 지시서

**분량**: 본문 한 쪽 안팎 + 표 2개. **담당**: 한 명이 절 전체.

이 절이 답하는 질문: 어떤 연구를 모았고, 기존 서베이에 무엇을 더하는가.

## 정리할 것

### (1) 범위 문단

아래 한 줄은 고정이다. 이 줄을 풀어서 3–4문장으로 쓴다.

> 학습 비용을 줄이는 증류만 다룬다. 추론을 빠르게 하는 모델 압축과 구분한다.

- 포함: 증류 학습 과정의 비용(teacher 호출, 데이터, 학습 길이, 저장)을 줄이는 방법.
- 제외: student 추론 효율만 다루는 연구, 정확도 향상만 목표인 KD 손실 설계.
- 비전 밖 논문(검출, 언어 모델)을 넣었다면 어떤 자격으로 넣었는지 한 줄.

### (2) 수집 기준 문단과 표

**지어내지 않는다.** 실제로 어떻게 모았는지는 지후에게 확인하고 쓴다.

- 검색 경로, 포함 기준, 제외 기준.
- 축 담당에게 받을 것: 자기 축 논문 수, 검토했다가 뺀 논문과 이유, 두 축에 걸친 논문.

| 축 | 편수 | 하위 갈래 | 제외한 논문 수 |
|---|---|---|---|
| Shrink | | | |
| Skip | | | |
| Reuse | | | |
| Substitute | | | |
| 측정 | | | |
| 평가 | | | |

### (3) 기존 서베이 비교표

대상: [`../Related Work/survey paper/survey - en.md`](<../Related Work/survey paper/survey - en.md>)의 PART 2 (KD 서베이)와 PART 3 중 관련 있는 것.

| 서베이 | 연도 | 분류 기준 | 학습 비용을 다루나 (절, 쪽) | 자체 실험 | 대상 분야 |
|---|---|---|---|---|---|
| | | | | | |

- "학습 비용을 다루나" 칸은 **직접 확인한 위치**를 적는다. 다루는 절이 있으면 있다고 적는다.
- "자체 실험" 칸: 없음 / 논문 수치 모음 / 같은 조건에서 직접 실행.

### (4) 가까운 선행 연구

논문마다 "한 것 / 안 한 것 / 우리와의 차이"를 한 줄씩.

| 논문 | 한 것 | 안 한 것 |
|---|---|---|
| ShiftKD (arXiv 2312.16242) | | |
| Knowledge Distillation Must Account for What It Loses (2604.25110) | | |
| End-to-End Energy Accounting of Distillation Pipelines (2605.13981) | | |
| Is Knowledge Distillation Actually Greener? (2602.09691) | | |
| The Surprising Effectiveness of Canonical KD for Semantic Segmentation (2604.25530) | | |

### (5) 측정·평가 논문 체크표

`5_Measurement (Q2)`, `6_Evaluation (Q3)` 폴더의 논문을 [5_literature.md](5_literature.md) 양식으로 채운다.

## 쓰지 않는 부분

"그래서 이 논문은 무엇을 더하는가" 문단은 지후가 쓴다. (3)과 (4)의 표가 그 근거가 된다.
