# 5절 체크표: Substitute

담당: Substitute 담당. 양식과 칸 값의 뜻은 [`../5_literature.md`](../5_literature.md).

> **AI 초안 (2026-11-19). 메모가 이후 원문 대조로 고쳐졌다: 각 논문 메모의 "원문 대조" 블록이 이 표보다 우선한다.** [`../../notes/`](../../notes)의 읽기 메모에서 기계적으로 옮긴 값이다. 원문과 대조하지 않았다. 칸 값을 원문으로 확인해 고치고 확인 열에 `이름 날짜`를 적는다. "메모에 없음"은 메모에 적혀 있지 않다는 뜻이지 논문이 보고하지 않았다는 뜻이 아니다. 고칠 곳은 [`../../notes/audit/`](../../notes/audit/README.md)에도 있다.

칸 값을 확정할 때는 수치(값과 위치) / 언급 / 미보고 / 해당 없음 중 하나로 바꾼다. 확인 칸이 빈 행은 본문에 쓸 수 없다.

## 아낀 것 (비용)

| 번호 | 논문 | teacher 연산 | 전체 학습 연산 | 실제 시간 | 준비 비용 | 저장 용량 | 측정 단위 | 하드웨어 | 본 판본 | 확인 |
|---|---|---|---|---|---|---|---|---|---|---|
| U-01 | Efficient KD from Model Checkpoints | 총 학습 시간에 포함된 것으로 보이나 내역 없음 | 메모에 없음 | 있음 (메모: 시간 관련 언급) | teacher 학습 Y (그림에서) | 메모에 없음 | teacher 학습 epoch 비율, 총 학습 시간(teacher 학습 + student 증류, TITAN Xp 기준 추정 시… | TITAN Xp | [arXiv: 2210.06458](https://arxiv.org/abs/2210.06458) | |
| U-02 | On the Efficacy of KD | 안 셈 (메모: N) | 메모에 없음 | 메모에 없음 | 안 셈 (메모: N) | 메모에 없음 | teacher 학습 epoch 수 (표의 괄호), 비용 측정 없음 | 보고 안 됨 | [arXiv: 1910.01348](https://arxiv.org/abs/1910.01348) | |
| U-03 | Learning from a Lightweight Teacher for Efficient KD | 셈 (메모: Y (FLOPs로)) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | teacher 학습 시간 N (크기 비교만) | 메모에 없음 | 모델 매개변수 수, 입력 1개당 FLOPs. 학습 시간은 보고 안 됨 | GTX 2080 Ti | [arXiv: 2005.09163](https://arxiv.org/abs/2005.09163) | |
| U-04 | Tf-KD | 안 셈 (메모: N) | 메모에 없음 | 메모에 없음 | 안 셈 (메모: N) | 메모에 없음 | 없음 (정확도만) | 보고 안 됨 | [arXiv: 1909.11723](https://arxiv.org/abs/1909.11723) | |
| U-05 | Be Your Own Teacher | 셈 (메모: Y (비교 대상의 시간에 teacher 학습과 증류가 모두 포함)) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 셈 (메모: Y (teacher 학습 시간을 비교 대상에 포함)) | 메모에 없음 | 실제 학습 시간(시간), 추론 가속 배수 | 보고 안 됨 | [arXiv: 1905.08094](https://arxiv.org/abs/1905.08094) | |
| U-06 | Deep Mutual Learning | 안 셈 (메모: N) | 메모에 없음 | 메모에 없음 | 안 셈 (메모: N) | 메모에 없음 | 없음 (정확도, mAP) | GTX 1080 1장 | [arXiv: 1706.00384](https://arxiv.org/abs/1706.00384) | |
| U-07 | Snapshot Distillation | 셈 (메모: Y (추가 시간 1/3이 그것)) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 해당 없음 (메모) | 메모에 없음 | 기준 대비 추가 학습 시간의 비율 (서술로만, 측정 표 없음) | 보고 안 됨 | [arXiv: 1812.00123](https://arxiv.org/abs/1812.00123) | |
| U-08 | Proteus | 셈 (메모: Y (30시간에 포함)) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 안 셈 (메모: N) | 메모에 없음 | 학습 데이터 수, 학습 시간(시간) | A100 8장 | [arXiv: 2407.10366](https://arxiv.org/abs/2407.10366) | |
| U-09 | SRe2L (Squeeze, Recover and Relabel) | 안 셈 (메모: N (재라벨링 시간 없음)) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 합성 비용 Y, 압축 단계 학습과 재라벨링 N | 있음 (메모: 저장 용량 수치 언급) | 합성 시간(이미지·반복당 ms), 최대 GPU 메모리(GB) | RTX 4090 1장 | [arXiv: 2306.13092](https://arxiv.org/abs/2306.13092) | |
| U-10 | RDED | 합성 시간에 포함 | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 합성 Y, soft label 저장 N | 있음 (메모: 저장 용량 수치 언급) | 합성 시간(분, 이미지당 ms), 최대 GPU 메모리(GB) | RTX 4090 1장 | [arXiv: 2312.03526](https://arxiv.org/abs/2312.03526) | |
| U-11 | Up to 100x Faster Data-free KD | 합성 시간에 포함 | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 합성 Y, teacher 학습 N | 메모에 없음 | 합성의 GPU 시간, 기준 방법 대비 배수. student 학습 시간은 평가 지표 정의에서 명시적으로 제외함 (Evaluati… | GPU 1장 기준 추정 (종류 보고 안 됨) | [arXiv: 2112.06253](https://arxiv.org/abs/2112.06253) | |
| U-12 | NAYER | 셈 (메모: Y (전체 시간에 포함)) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | teacher 학습 N | 메모에 없음 | 전체 학습 시간(시간), 기준 방법 대비 배수 | NVIDIA A100 1장 | [arXiv: 2310.00258](https://arxiv.org/abs/2310.00258) | |

## 치른 것 (성능)

| 번호 | 논문 | 평균 정확도 | 그룹별, 최저 그룹 | 클래스별, 꼬리·하위 | 보정 | teacher 일치율 | 분포 이동, 손상 | 전이, 다른 과제 | seed 수 | 표준편차 | 확인 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| U-01 | Efficient KD from Model Checkpoints | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모: 5회 평균 | 메모에 없음 | |
| U-02 | On the Efficacy of KD | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 있음 (메모: 전이) | 메모에 없음 | 메모에 없음 | |
| U-03 | Learning from a Lightweight Teacher for Efficient KD | 있음 (메모) | 있음 (메모: 그룹) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| U-04 | Tf-KD | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모: 3회 평균 | 메모에 없음 | |
| U-05 | Be Your Own Teacher | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| U-06 | Deep Mutual Learning | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| U-07 | Snapshot Distillation | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 있음 (메모: 전이) | 메모에 없음 | 메모에 없음 | |
| U-08 | Proteus | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 있음 (메모: 분포 이동·손상) | 있음 (메모: 전이) | 메모에 없음 | 메모에 없음 | |
| U-09 | SRe2L (Squeeze, Recover and Relabel) | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| U-10 | RDED | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| U-11 | Up to 100x Faster Data-free KD | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 있음 (메모: 전이) | 메모에 없음 | 메모에 없음 | |
| U-12 | NAYER | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모: 3회 평균 | 메모에 없음 | |

## 비교

| 번호 | 논문 | Full KD 기준선 | 같은 시간·연산으로 맞춘 비교 | 무작위 대조 | 확인 |
|---|---|---|---|---|---|
| U-01 | Efficient KD from Model Checkpoints | | | | |
| U-02 | On the Efficacy of KD | | | | |
| U-03 | Learning from a Lightweight Teacher for Efficient KD | | | | |
| U-04 | Tf-KD | | | | |
| U-05 | Be Your Own Teacher | | | | |
| U-06 | Deep Mutual Learning | | | | |
| U-07 | Snapshot Distillation | | | | |
| U-08 | Proteus | | | | |
| U-09 | SRe2L (Squeeze, Recover and Relabel) | | | | |
| U-10 | RDED | | | | |
| U-11 | Up to 100x Faster Data-free KD | | | | |
| U-12 | NAYER | | | | |
