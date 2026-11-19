# 5절 체크표: Skip

담당: Skip 담당. 양식과 칸 값의 뜻은 [`../5_literature.md`](../5_literature.md).

> **AI 초안 (2026-11-19). 메모가 이후 원문 대조로 고쳐졌다: 각 논문 메모의 "원문 대조" 블록이 이 표보다 우선한다.** [`../../notes/`](../../notes)의 읽기 메모에서 기계적으로 옮긴 값이다. 원문과 대조하지 않았다. 칸 값을 원문으로 확인해 고치고 확인 열에 `이름 날짜`를 적는다. "메모에 없음"은 메모에 적혀 있지 않다는 뜻이지 논문이 보고하지 않았다는 뜻이 아니다. 고칠 곳은 [`../../notes/audit/`](../../notes/audit/README.md)에도 있다.

칸 값을 확정할 때는 수치(값과 위치) / 언급 / 미보고 / 해당 없음 중 하나로 바꾼다. 확인 칸이 빈 행은 본문에 쓸 수 없다.

## 아낀 것 (비용)

| 번호 | 논문 | teacher 연산 | 전체 학습 연산 | 실제 시간 | 준비 비용 | 저장 용량 | 측정 단위 | 하드웨어 | 본 판본 | 확인 |
|---|---|---|---|---|---|---|---|---|---|---|
| K-01 | Distill on a Diet | Table 2는 반복 수 기준이라 간접 반영. Table 3 시간에 logit 사전 계산과… | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 선택 비용 Y (Table 2), teacher 학습 N | 메모에 없음 | 상대 비용(목표 정확도 도달까지의 반복 수 비율), 실제 시간(h) | NVIDIA A100 1장 (Table 3) | [arXiv: 2606.25488](https://arxiv.org/abs/2606.25488) | |
| K-02 | Distilling the Knowledge in Data Pruning | 안 셈 (메모: N) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 안 셈 (메모: N) | 메모에 없음 | 유지 데이터 비율 f (비용 측정 없음) | 보고 안 됨 | [arXiv: 2403.07854](https://arxiv.org/abs/2403.07854) | |
| K-03 | When Less Data Favors Smaller Teachers | 선택 단계의 대리 모델 forward는 Y, KD 학습 중 teacher forward는… | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 선택 비용 Y, teacher·대리 teacher 학습 N | 메모에 없음 | 선택 비용 PFLOPs와 실제 시간(초). student 학습 비용은 제거율로만 표시 | RTX 4090 | [arXiv: 2609.34489](https://arxiv.org/abs/2609.34489) | |
| K-04 | Few Sample KD (FSKD) | 시간에 포함됐는지 명시 없음 | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 안 셈 (메모: N) | 메모에 없음 | 실제 시간(초), 샘플 수, student FLOPs | Intel i7-7700K, GTX 1080 Ti 1장 | [arXiv: 1812.01839](https://arxiv.org/abs/1812.01839) | |
| K-05 | PRACTISE | 셈 (메모: Y (전체 과정 실측 시간)) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 선택 비용 Y, 원 모델 학습 N | 메모에 없음 | 추론 지연(ms), 학습 시간(분), 샘플 수 | NVIDIA TITAN Xp 1장 | [arXiv: 2202.07861](https://arxiv.org/abs/2202.07861) | |
| K-06 | UNIX | 셈 (메모: Y) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 안 셈 (메모: N) | 메모에 없음 | KD 대비 연산 비율(%). teacher forward, student forward, student backward의 FL… | TITAN X Pascal (CIFAR 1장, ImageNet 4장·8장… | [arXiv: 2012.09413](https://arxiv.org/abs/2012.09413) | |
| K-07 | Active Mixup | 셈 (메모: Y (질의 수로)) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 안 셈 (메모: N) | 메모에 없음 | teacher 질의 수, 실제 이미지 수 | 보고 안 됨. 연산량과 시간도 보고 안 됨. | [arXiv: 2003.13960](https://arxiv.org/abs/2003.13960) | |
| K-08 | Comparative KD | 셈 (메모: Y (횟수로)) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 안 셈 (메모: N) | 있음 (메모: 저장 용량 수치 언급) | teacher 호출 수 n | 12 GB GPU 1장 (주로 RTX 2080 Ti). 실행 1회가 20… | [arXiv: 2311.02253](https://arxiv.org/abs/2311.02253) | |
| K-09 | Weak-to-Strong KD Accelerates Visual Learning | 안 셈 (메모: N) | 메모에 없음 | 메모에 없음 | 안 셈 (메모: N (본문 기준)) | 메모에 없음 | 목표 도달 epoch(또는 step)의 비 | 보고 안 됨 | [arXiv: 2604.15451](https://arxiv.org/abs/2604.15451) | |
| K-10 | DearKD | 안 셈 (메모: N) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 안 셈 (메모: N) | 메모에 없음 | 학습 데이터 비율, epoch. 학습 비용은 재지 않음 (추론 처리량만 Table 3) | NVIDIA A100 8장 (학습), V100 (처리량 측정) | [arXiv: 2204.12997](https://arxiv.org/abs/2204.12997) | |

## 치른 것 (성능)

| 번호 | 논문 | 평균 정확도 | 그룹별, 최저 그룹 | 클래스별, 꼬리·하위 | 보정 | teacher 일치율 | 분포 이동, 손상 | 전이, 다른 과제 | seed 수 | 표준편차 | 확인 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| K-01 | Distill on a Diet | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모: 3 seed | 메모에 없음 | |
| K-02 | Distilling the Knowledge in Data Pruning | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| K-03 | When Less Data Favors Smaller Teachers | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모: 10 seed | 메모에 없음 | |
| K-04 | Few Sample KD (FSKD) | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| K-05 | PRACTISE | 있음 (메모) | 있음 (메모: 그룹) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 있음 (메모: 분포 이동·손상) | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| K-06 | UNIX | 있음 (메모) | 있음 (메모: 그룹) | 있음 (메모: 클래스) | 메모에 없음 | 메모에 없음 | 있음 (메모: 분포 이동·손상) | 메모에 없음 | 메모: 4회 평균 | 메모에 없음 | |
| K-07 | Active Mixup | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| K-08 | Comparative KD | 있음 (메모) | 있음 (메모: 그룹) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 있음 (메모: 분포 이동·손상) | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| K-09 | Weak-to-Strong KD Accelerates Visual Learning | 있음 (메모) | 있음 (메모: 그룹) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 있음 (메모: 분포 이동·손상) | 있음 (메모: 전이) | 메모에 없음 | 메모에 없음 | |
| K-10 | DearKD | 있음 (메모) | 있음 (메모: 그룹) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 있음 (메모: 분포 이동·손상) | 있음 (메모: 전이) | 메모에 없음 | 메모에 없음 | |

## 비교

| 번호 | 논문 | Full KD 기준선 | 같은 시간·연산으로 맞춘 비교 | 무작위 대조 | 확인 |
|---|---|---|---|---|---|
| K-01 | Distill on a Diet | | | | |
| K-02 | Distilling the Knowledge in Data Pruning | | | | |
| K-03 | When Less Data Favors Smaller Teachers | | | | |
| K-04 | Few Sample KD (FSKD) | | | | |
| K-05 | PRACTISE | | | | |
| K-06 | UNIX | | | | |
| K-07 | Active Mixup | | | | |
| K-08 | Comparative KD | | | | |
| K-09 | Weak-to-Strong KD Accelerates Visual Learning | | | | |
| K-10 | DearKD | | | | |
