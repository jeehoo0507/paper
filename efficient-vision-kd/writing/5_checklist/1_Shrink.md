# 5절 체크표: Shrink

담당: Shrink 담당. 양식과 칸 값의 뜻은 [`../5_literature.md`](../5_literature.md).

> **AI 초안 (2026-11-19). 메모가 이후 원문 대조로 고쳐졌다: 각 논문 메모의 "원문 대조" 블록이 이 표보다 우선한다.** [`../../notes/`](../../notes)의 읽기 메모에서 기계적으로 옮긴 값이다. 원문과 대조하지 않았다. 칸 값을 원문으로 확인해 고치고 확인 열에 `이름 날짜`를 적는다. "메모에 없음"은 메모에 적혀 있지 않다는 뜻이지 논문이 보고하지 않았다는 뜻이 아니다. 고칠 곳은 [`../../notes/audit/`](../../notes/audit/README.md)에도 있다.

칸 값을 확정할 때는 수치(값과 위치) / 언급 / 미보고 / 해당 없음 중 하나로 바꾼다. 확인 칸이 빈 행은 본문에 쓸 수 없다. `*` 표시는 폴더에 PDF가 없는 후보 논문이다.

## 아낀 것 (비용)

| 번호 | 논문 | teacher 연산 | 전체 학습 연산 | 실제 시간 | 준비 비용 | 저장 용량 | 측정 단위 | 하드웨어 | 본 판본 | 확인 |
|---|---|---|---|---|---|---|---|---|---|---|
| S-01 | MaskedKD | 셈 (메모: Y (teacher만 셈, student 학습 비용은 분모에 없음)) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 안 셈 (메모: N (teacher 학습 비용 없음)) | 메모에 없음 | teacher FLOPs(학습 전체 합, PFLOPs), 처리량(img/s), GPU 메모리 | RTX A6000 1장, FP32, batch 128 (부록 A) | [arXiv: 2302.10494](https://arxiv.org/abs/2302.10494) | |
| S-02 | DMAE | 셈 (메모: Y (증류 사전학습의 GPU 시간에 포함)) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 안 셈 (메모: N (teacher 사전학습 비용 없음)) | 메모에 없음 | 메모에 없음 | NVIDIA A5000 1장. Table 7은 사전학습·미세조정·합계 G… | [arXiv: 2208.12256](https://arxiv.org/abs/2208.12256) | |
| S-03 | Dyna-DINO | 셈 (메모: Y (보고한 절감률 자체가 teacher 연산 오버헤드의 감소분, Tab…) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 안 셈 (메모: N) | 메모에 없음 | 메모에 없음 | NVIDIA L40S·RTX A6000·L40 등이 섞인 클러스터 (p.… | [arXiv: 2606.19483](https://arxiv.org/abs/2606.19483) | |
| S-04 | Asymmetric Masked Distillation (AMD) | 셈 (메모: Y (학습 전체의 실제 시간)) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 안 셈 (메모: N (teacher 사전학습 비용 없음)) | 있음 (메모: 저장 용량 수치 언급) | 메모에 없음 | A100 16장에서 200 epoch을 학습하며 시간 측정 (Table… | [arXiv: 2311.03149](https://arxiv.org/abs/2311.03149) | |
| S-05 | MaskAlign | 셈 (메모: Y로 보임 (사전학습 속도 비교, 내역 없음)) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 안 셈 (메모: N (CLIP teacher 사전학습 비용 없음)) | 메모에 없음 | 메모에 없음 | 보고 안 됨. Table 7의 시간은 절대값이 아니라 상대 배수임: 제안… | [arXiv: 2211.08887](https://arxiv.org/abs/2211.08887) | |
| S-06 | Decoupled KD (DKD) | 배치당 학습 시간을 "데이터 처리 + 순전파·역전파 + 메모리 갱신"의 합으로 정의하나 t… | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 안 셈 (메모: N) | 메모에 없음 | 메모에 없음 | 보고 안 됨. 단위는 배치당 학습 시간(ms)과 추가 매개변수 수 (Fi… | [arXiv: 2203.08679](https://arxiv.org/abs/2203.08679) | |
| S-07 | Information Theoretic Representation Distillation | 셈 (메모: Y (학습 전체의 처리량으로 측정)) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 안 셈 (메모: N) | 있음 (메모: 저장 용량 수치 언급) | 메모에 없음 | RTX 2080 Ti 1장. 메모리 8 GB 제약에 맞게 방법마다 배치… | [arXiv: 2112.00459](https://arxiv.org/abs/2112.00459) | |
| S-08 | Understanding the Role of the Projector in KD | 안 셈 (메모: N) | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 안 셈 (메모: N) | 메모에 없음 | 메모에 없음 | NVIDIA RTX A5000 1장 (p. 5). 학습 시간이나 연산량의… | [arXiv: 2303.11098](https://arxiv.org/abs/2303.11098) | |
| S-09* | Parameter-Efficient and Student-Friendly Knowledge Distillat… | 확인 안 함 (PDF가 저장소에 없어 이번 보강에서 제외). | 메모에 없음 | 있음 (메모: 시간 관련 언급) | 확인 안 함 (PDF가 저장소에 없어 이번 보강에서 제외). | 메모에 없음 | 메모에 없음 | 메모에 없음 | [원문](https://arxiv.org/abs/2205.15308) | |
| S-10* | Knowledge Distillation from A Stronger Teacher | 확인 안 함 (PDF가 저장소에 없어 이번 보강에서 제외). | 메모에 없음 | 메모에 없음 | 확인 안 함 (PDF가 저장소에 없어 이번 보강에서 제외). | 메모에 없음 | 메모에 없음 | 메모에 없음 | [원문](https://arxiv.org/abs/2205.10536) | |

## 치른 것 (성능)

| 번호 | 논문 | 평균 정확도 | 그룹별, 최저 그룹 | 클래스별, 꼬리·하위 | 보정 | teacher 일치율 | 분포 이동, 손상 | 전이, 다른 과제 | seed 수 | 표준편차 | 확인 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| S-01 | MaskedKD | 있음 (메모) | 있음 (메모: 그룹) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 있음 (메모: 분포 이동·손상) | 있음 (메모: 전이) | 메모: 3 seed | 메모에 없음 | |
| S-02 | DMAE | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| S-03 | Dyna-DINO | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 있음 (메모: 전이) | 메모에 없음 | 메모에 없음 | |
| S-04 | Asymmetric Masked Distillation (AMD) | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 있음 (메모: 전이) | 메모에 없음 | 메모에 없음 | |
| S-05 | MaskAlign | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 있음 (메모: 전이) | 메모에 없음 | 메모에 없음 | |
| S-06 | Decoupled KD (DKD) | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 있음 (메모: 전이) | 메모에 없음 | 메모에 없음 | |
| S-07 | Information Theoretic Representation Distillation | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| S-08 | Understanding the Role of the Projector in KD | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| S-09* | Parameter-Efficient and Student-Friendly Knowledge Distillat… | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 있음 (메모: teacher 일치) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | |
| S-10* | Knowledge Distillation from A Stronger Teacher | 있음 (메모) | 메모에 없음 | 메모에 없음 | 메모에 없음 | 메모에 없음 | 있음 (메모: 분포 이동·손상) | 메모에 없음 | 메모에 없음 | 메모에 없음 | |

## 비교

| 번호 | 논문 | Full KD 기준선 | 같은 시간·연산으로 맞춘 비교 | 무작위 대조 | 확인 |
|---|---|---|---|---|---|
| S-01 | MaskedKD | | | | |
| S-02 | DMAE | | | | |
| S-03 | Dyna-DINO | | | | |
| S-04 | Asymmetric Masked Distillation (AMD) | | | | |
| S-05 | MaskAlign | | | | |
| S-06 | Decoupled KD (DKD) | | | | |
| S-07 | Information Theoretic Representation Distillation | | | | |
| S-08 | Understanding the Role of the Projector in KD | | | | |
| S-09* | Parameter-Efficient and Student-Friendly Knowledge Distillat… | | | | |
| S-10* | Knowledge Distillation from A Stronger Teacher | | | | |
