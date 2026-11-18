# 지금까지 진행 과정

## 1. 주제 정하기

- 기존 KD 서베이는 지식 종류·학습 방식으로 분류하고 **증류 학습 비용** 축이 없음.
- Efficient ViT 서베이는 student 추론 비용만 다룸.
- 가장 가까운 건 LM 쪽 On-Policy Distillation 비용 서베이 (언어 한정).
- → 주제: **비전 KD에서 증류 학습 자체의 비용을 줄이는 방법** 서베이.

## 2. 참고 서베이 정리

잘 쓴 서베이(dLLM, 디퓨전, Efficient Transformers, Sparsity 등)에서 배울 점 정리 → `Related Work/survey paper/survey - en.md` (PART 1~3), 랩실 국문 논문 → `survey- ko.md` (PART 4).

## 3. 분류 축 (Q1)

비용을 **어떻게 줄이나** 기준 4축: Shrink (teacher 입력 축소) / Skip (데이터·구간 생략) / Reuse (teacher 출력 저장·재사용) / Substitute (teacher·데이터 대체).
논문 62편을 축별 폴더로 정리 → `Related Work/Efficient KD papers/`.

## 4. 논문 본문에서 확인한 것 (Q2)

| 관찰 | 수치 |
|---|---|
| teacher 학습 비용을 센 논문 | 0 / 18 (Shrink + Skip) |
| 저장 공간을 보고한 논문 | 0 / 18 |
| teacher 추론을 세지 않은 논문 | 5 / 18 |
| 일회성 비용을 숫자로 보고 (Reuse) | 3 / 14 |
| MaskedKD teacher FLOPs −51% → 전체 학습 기준 | 약 −27% (우리 환산, 가정 있음) |

→ 논문마다 비용을 세는 범위가 달라 숫자를 직접 비교할 수 없음. 비용 계산 방식 후보는 `README.md` Q2 (초안, 확정 아님).

## 5. 평가 방식 (Q3)

평균 정확도 외 지표 8개 후보 (WGA, 최악 클래스, 분포 이동, 손상, 전이, teacher 일치율·ECE, 학습 곡선). 상세는 `experiments/PLAN.md`.

## 6. 랩실 피드백 반영

- 방향 좋음. 공정한 단위 측정은 오래 걸림.
- 논문에 적힌 장단점으로 효율·이득·손실을 먼저 쓰고, 논문끼리 숫자를 그냥 합치지 말 것.
- 기존 실험으로 먼저 쓰고 실험은 나중에 추가.
- → 라이팅은 문헌 기반으로 먼저, 실험은 병행해서 보강.

## 7. 현재 상태

| 항목 | 상태 |
|---|---|
| 목차 | 초안 (`our paper.md`) |
| 읽기 목록 | 우선 17편 (`Related Work/Efficient KD paper.md`) |
| 읽기 메모 | 템플릿만 (`notes/`) |
| 실험 | 계획만 (`experiments/PLAN.md`), 미실행 |
| 발표 자료 | `presentation/index.html` (브라우저로 열고 ←/→) |

## 다음

1. 17편 읽고 `notes/`에 메모
2. 서버에서 실험 세팅 (B0로 시간 확인부터)
3. 절별 초안 작성 후 Slack 공유
