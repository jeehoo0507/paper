# 5절 체크표: 평가

담당: 2절 담당. 양식과 칸 값의 뜻은 [`../5_literature.md`](../5_literature.md).

> **AI 초안 (2026-11-19).** [`../../notes/`](../../notes)의 읽기 메모에서 기계적으로 옮긴 값이다. 원문과 대조하지 않았다. 칸 값을 원문으로 확인해 고치고 확인 열에 `이름 날짜`를 적는다. "메모에 없음"은 메모에 적혀 있지 않다는 뜻이지 논문이 보고하지 않았다는 뜻이 아니다. 고칠 곳은 [`../../notes/audit/`](../../notes/audit/README.md)에도 있다.

칸 값을 확정할 때는 수치(값과 위치) / 언급 / 미보고 / 해당 없음 중 하나로 바꾼다. 확인 칸이 빈 행은 본문에 쓸 수 없다.

## 아낀 것 (비용)

| 번호 | 논문 | teacher 연산 | 전체 학습 연산 | 실제 시간 | 준비 비용 | 저장 용량 | 측정 단위 | 하드웨어 | 본 판본 | 확인 |
|---|---|---|---|---|---|---|---|---|---|---|
| E-01 | Teacher's Pet (정정 있음: 메모 참고) | 해당 없음 (메모) | 메모에 없음 | 메모에 없음 | 해당 없음 (메모) | 메모에 없음 | 해당 없음 (비용 측정 없음) | 확인 안 함 | [arXiv: 2106.10494](https://arxiv.org/abs/2106.10494) | |
| E-02 | DeDiER (정정 있음: 메모 참고) | 해당 없음 (메모) | 메모에 없음 | 메모에 없음 | 해당 없음 (메모) | 메모에 없음 | 해당 없음 | 확인 안 함 | [arXiv: 2310.18590](https://arxiv.org/abs/2310.18590) | |
| E-03 | Debiased Distillation by Transplanting the Last Layer (정정 있음: 메모 참고) | 해당 없음 (메모) | 메모에 없음 | 메모에 없음 | 해당 없음 (메모) | 메모에 없음 | 해당 없음 | 확인 안 함 | [arXiv: 2302.11187](https://arxiv.org/abs/2302.11187) | |
| E-04 | What is Left After Distillation? | 해당 없음 (메모) | 메모에 없음 | 메모에 없음 | 해당 없음 (메모) | 메모에 없음 | 해당 없음 | 확인 안 함 | [arXiv: 2410.08407](https://arxiv.org/abs/2410.08407) | |
| E-05 | When Masking Helps or Hurts Robustness in Compressed CLIP (정정 있음: 메모 참고) | 해당 없음 (메모) | 메모에 없음 | 메모에 없음 | 해당 없음 (메모) | 메모에 없음 | 이미지당 추론 지연(ms) | NVIDIA A100 1장 | [arXiv: 2609.39704](https://arxiv.org/abs/2609.39704) | |
| E-06 | Later Is Better: Token Reduction under Distribution Shift | 해당 없음 (메모) | 메모에 없음 | 메모에 없음 | 해당 없음 (메모) | 메모에 없음 | 추론 GFLOPs (같은 GFLOPs에서 비교) | 확인 안 함 | [arXiv: 2610.07758](https://arxiv.org/abs/2610.07758) | |
| E-07 | DRoP: Distributionally Robust Data Pruning (정정 있음: 메모 참고) | 해당 없음 (메모) | 메모에 없음 | 메모에 없음 | 안 셈 (메모: N) | 메모에 없음 | 남긴 데이터 비율 | 확인 안 함 | [arXiv: 2404.05579](https://arxiv.org/abs/2404.05579) | |
| E-08 | Impact of Coreset Selection on Spurious Correlations | 해당 없음 (메모) | 메모에 없음 | 메모에 없음 | 안 셈 (메모: N) | 메모에 없음 | 선택 비율 | 확인 안 함 | [arXiv: 2507.11690](https://arxiv.org/abs/2507.11690) | |
| E-09 | Group DRO (Waterbirds) | 해당 없음 (메모) | 메모에 없음 | 메모에 없음 | 해당 없음 (메모) | 메모에 없음 | 해당 없음 | 확인 안 함 | [arXiv: 1911.08731](https://arxiv.org/abs/1911.08731) | |
| E-10 | Last Layer Re-Training is Sufficient | 해당 없음 (메모) | 메모에 없음 | 메모에 없음 | 해당 없음 (메모) | 메모에 없음 | 해당 없음 | 확인 안 함 | [arXiv: 2204.02937](https://arxiv.org/abs/2204.02937) | |
| E-11 | Group Robustness is in the Details | 해당 없음 (메모) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 해당 없음 (메모) | 메모에 없음 | 해당 없음 | 확인 안 함 | [arXiv: 2407.13957](https://arxiv.org/abs/2407.13957) | |

## 치른 것 (성능)

| 번호 | 논문 | 평균 정확도 | 그룹별, 최저 그룹 | 클래스별, 꼬리·하위 | 보정 | teacher 일치율 | 분포 이동, 손상 | 전이, 다른 과제 | seed 수 | 표준편차 | 확인 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| E-01 | Teacher's Pet (정정 있음: 메모 참고) | 있음 (메모) | 있음 (메모: 그룹) | 있음 (메모: 클래스) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| E-02 | DeDiER (정정 있음: 메모 참고) | 있음 (메모) | 있음 (메모: 그룹) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| E-03 | Debiased Distillation by Transplanting the Last Layer (정정 있음: 메모 참고) | 있음 (메모) | 있음 (메모: 그룹) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| E-04 | What is Left After Distillation? | 있음 (메모) | 메모에 없음 | 있음 (메모: 클래스) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| E-05 | When Masking Helps or Hurts Robustness in Compressed CLIP (정정 있음: 메모 참고) | 있음 (메모) | 있음 (메모: 그룹) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| E-06 | Later Is Better: Token Reduction under Distribution Shift | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 있음 (메모: 분포 이동·손상) | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| E-07 | DRoP: Distributionally Robust Data Pruning (정정 있음: 메모 참고) | 있음 (메모) | 메모에 없음 | 있음 (메모: 클래스) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모: 3 seed | 메모에 없음 | |
| E-08 | Impact of Coreset Selection on Spurious Correlations | 있음 (메모) | 있음 (메모: 그룹) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| E-09 | Group DRO (Waterbirds) | 있음 (메모) | 있음 (메모: 그룹) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| E-10 | Last Layer Re-Training is Sufficient | 있음 (메모) | 있음 (메모: 그룹) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모: 5회 평균 | 메모에 없음 | |
| E-11 | Group Robustness is in the Details | 있음 (메모) | 있음 (메모: 그룹) | 있음 (메모: 클래스) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모: 3 seed | 메모에 없음 | |

## 비교

| 번호 | 논문 | Full KD 기준선 | 같은 시간·연산으로 맞춘 비교 | 무작위 대조 | 확인 |
|---|---|---|---|---|---|
| E-01 | Teacher's Pet (정정 있음: 메모 참고) | | | | |
| E-02 | DeDiER (정정 있음: 메모 참고) | | | | |
| E-03 | Debiased Distillation by Transplanting the Last Layer (정정 있음: 메모 참고) | | | | |
| E-04 | What is Left After Distillation? | | | | |
| E-05 | When Masking Helps or Hurts Robustness in Compressed CLIP (정정 있음: 메모 참고) | | | | |
| E-06 | Later Is Better: Token Reduction under Distribution Shift | | | | |
| E-07 | DRoP: Distributionally Robust Data Pruning (정정 있음: 메모 참고) | | | | |
| E-08 | Impact of Coreset Selection on Spurious Correlations | | | | |
| E-09 | Group DRO (Waterbirds) | | | | |
| E-10 | Last Layer Re-Training is Sufficient | | | | |
| E-11 | Group Robustness is in the Details | | | | |
