# 5_Measurement (Q2) 메모

> **원문 대조 완료 (2026-11-19).** 논문마다 **원문 대조** 블록이 있다. Opus 5.5 서브에이전트 8개가 저장소 PDF(일부는 arXiv·학회판을 받아)와 메모의 수치·조건·판정을 한 줄씩 대조한 결과다. 블록이 위 서술과 다르면 블록이 맞다. 대조 보고서 원본: [`audit/verify-2026-11-19/`](audit/verify-2026-11-19). 사람이 다시 확인하지는 않았다.

Measurement (Q2): 측정 방식 (Q2) 참고  
논문 목록과 PDF: [`../Related Work/Efficient KD papers/5_Measurement (Q2)/`](<../Related Work/Efficient KD papers/5_Measurement (Q2)>)  
작성 규칙과 항목 설명: [README](README.md)

담당: Claude 초안 (2026-11-18). 저장소 PDF 본문을 직접 읽고 채움. 사람이 다시 대조하지 않음.

## 읽기 전에

- 이 폴더의 논문은 대부분 효율화 방법이 아니라 **비용을 어떻게 세고 비교할지**의 근거임. 그래서 「효율화 방식」에는 "무엇을 주장·측정하는 논문인가"를 적고, 「얻는 것」에는 그 논문이 보고한 비용–성능 관계를 적음.
- 수치는 저장소 PDF의 표·본문에서 옮겼고, 「위치」의 쪽 번호는 그 PDF의 쪽(첫 장 = 1)임.
- "보고 안 됨" = 논문에 해당 값이 없음. "확인 안 함" = 논문에 있을 수 있으나 이번에 읽지 않은 부분.

### 01. A Good Teacher is Patient and Consistent ([arXiv: 2106.05237](https://arxiv.org/abs/2106.05237))
- 효율화 방식: 효율화 방법이 아님. 오히려 비용을 늘리는 쪽의 권고. (1) teacher와 student가 **같은 증강 뷰**를 보게 하고 (2) 강한 증강(mixup)과 함께 (3) **매우 오래** 학습하면 증류가 가장 잘 된다는 것 (함수 맞추기).
- 얻는 것 (학습 길이에 따른 정확도, ImageNet, BiT-M-R152x2 → ResNet-50):
  - 300 / 1200 / 4800 / 9600 epoch에서 80.30 / 81.54 / 82.18 / 82.31%. (Table 1)
  - teacher를 고정된 crop으로만 돌린 설정은 일찍 포화하고 과적합함. (Fig. 5 왼쪽)
  - 소규모 데이터: Flowers-102에서 1,000 epoch 77.51% → 10,000 epoch 92.83% → 100,000 epoch 95.54%. (Table 4)
- 잃는 것: [실측·원논문] 긴 학습이 전제임. 300 epoch과 9600 epoch의 차이가 2.0%p. 최적화가 병목이라고 스스로 적고, 2차 전처리 최적화기(Shampoo)로 4800 epoch의 정확도를 1200 epoch에 얻었다고 보고. (3.4절, Fig. 5 가운데)
- 옮겨 간 비용: 해당 없음. teacher forward를 매 스텝, 매 증강 뷰마다 새로 해야 하므로 Reuse(출력 저장)와 정면으로 충돌함.
- 어떻게 쟀나: 단위 = epoch 수. 실제 시간이나 연산량은 보고 안 됨 / teacher forward 셈? N / 일회성 비용 셈? N / 하드웨어 = Google Cloud TPU (사양 보고 안 됨)
- 설정: teacher BiT-M-R152x2 (ImageNet-21k 사전학습 후 미세조정) → ResNet-50 (처음부터). ImageNet 최대 9600 epoch. Flowers-102, Pets, Food-101, SUN397은 최대 100만 epoch. 해상도 조합 224 → 224, 224 → 160, 384 → 224.
- 평균 정확도 외 평가: 도메인 밖 이미지로 증류했을 때의 정확도 (Fig. 6). 그 외 없음.
- 위치: Fig. 3 (p. 4), Fig. 4 (p. 5), Fig. 5 (p. 6), Table 1·2 (p. 7), Fig. 6 (p. 8), Table 3 (p. 12), Table 4 (p. 13)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 6건 / 수정 8건 / 추가 4건 / 확인 불가 0건. 본 판본: arXiv 2106.05237v2 (p.1 표기). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] "T224→S224 행 기준 300/1200/4800/9600 epoch에서 80.30/81.54/82.18/82.31% (teacher 83.0%). (Table 1, p.7)" ← 메모: 300/1200/4800/9600 epoch → 80.30/81.54/82.18/82.31%
  - [수정] "teacher 예측을 이미지마다 고정(사전 계산)한 설정은 ImageNet에서 1200 epoch 78.11%가 정점이고 9600 epoch 76.95%로 하락함. 같은 9600 epoch의 FunMatch 82.31%보다 5.36%p 낮음. 본문은 600 epoch 이후 과적합으로 서술. (Fig. 5 왼쪽, p.6; Table 3, p.12)" ← 메모: teacher를 고정된 crop으로만 돌린 설정은 일찍 포화·과적합 (Fig. 5 왼쪽)
  - [수정] "Flowers-102(128px)에서 1,000 / 10,000 / 100,000 / 1,000,000 epoch에 77.51 / 92.83 / 95.54 / 96.93% (teacher 97.82%). (Table 4, p.13)" ← 메모: Flowers-102 1,000 → 10,000 → 100,000 epoch: 77.51 → 92.83 → 95.54%
  - [수정] "Shampoo는 consistent teaching 기준 4800 epoch(Adam, 81.58%) 수준을 1200 epoch(81.44%)에 도달. 스텝 시간 오버헤드는 'minimal'이라는 정성 서술뿐. 'compute 75% 절감'은 FunMatch 1200 epoch 대 consistent teaching 4800 epoch의 epoch 비율. (§3.2 p.5, Fig. 5 p.6, Table 3 p.12)" ← 메모: Shampoo로 4800 epoch 정확도를 1200 epoch에
  - [수정] "작은 데이터셋 최장 학습은 Flowers-102 100만, Pets 3만, Food-101·SUN397 3,000 epoch. 입력 해상도 128px (ImageNet만 224px). (Table 4 p.13, p.3)" ← 메모: Flowers-102, Pets, Food-101, SUN397은 최대 100만 epoch
  - [수정] "student는 BiT-ResNet-50 (GroupNorm·weight standardization, 처음부터 학습). (p.2)" ← 메모: student = ResNet-50 (처음부터)
  - [수정] "도메인 밖 이미지로 증류한 정확도 (Fig. 6, p.8), distillation loss 학습·검증 곡선 (Fig. 3, p.4; Fig. 9–12, pp.14–17). 부분군 지표 없음." ← 메모: 평균 정확도 외 평가: Fig. 6 도메인 밖 증류, 그 외 없음
  - [수정] "고정 teacher 계열은 1장 resize·center crop뿐 아니라 crop 1,000개 평균(fix/ic ens)도 포함하며 모두 낮은 값에서 정체 (Fig. 3, p.4). '뷰 여러 개를 평균한 고정 목표'도 온라인 KD를 따라잡지 못한 사례로 인용 가능. ImageNet 곡선의 고정 변형은 명시 안 됨." ← 메모: 메모: Fig. 5의 고정 뷰 teacher = "뷰 1개" 조건
  - [추가] "teacher BiT-M-R152x2의 연산은 ResNet-50의 약 10배 (p.2). 매 스텝 teacher forward의 상대 비용 근거."
  - [추가] "앙상블 teacher(T224+T384 logit 평균) 9600 epoch 82.82%. 단일 teacher 82.31%와 별개 결과. (p.7, Table 3)"
  - [추가] "같은 레시피의 label 학습(증류 없음)은 1200 epoch 78.15%가 정점이고 4800 epoch 76.59%로 하락. 긴 학습 이득은 증류 조건에서만 나타남. (Table 3 p.12, Fig. 7 p.8)"
  - [추가] "논문 자체가 100 epoch 학습은 transfer 기준선보다 훨씬 나쁘다고 적음 (§3.1.2, p.5)."
- 메모:
  - "최종 정확도"에 대한 주장임. "목표 정확도에 얼마나 빨리 닿는가"와는 다른 질문 (2_Skip 09번 Weak-to-Strong과 대비).
  - 우리 실험에서 Reuse의 뷰 수를 늘리면 온라인 KD에 가까워진다는 가설의 근거. 고정 뷰 teacher가 포화한다는 Fig. 5가 "뷰 1개" 조건에 해당.
  - 소규모 데이터의 큰 KD 이득은 수천~수십만 epoch 조건의 결과라 100 epoch 실험에 그대로 기대하면 안 됨.

### 02. VanillaKD ([arXiv: 2305.15781](https://arxiv.org/abs/2305.15781))
- 효율화 방식: 효율화 방법이 아님. 강한 학습 레시피와 큰 데이터(ImageNet-1K)에서는 단순한 logit KD가 복잡한 KD 방법과 비슷하거나 낫다는 재평가. 비교군에 같은 학습 조건을 주어야 한다는 근거.
- 얻는 것 (ResNet-50 student, ImageNet-1K, V100 8장 기준 GPU 시간):
  - 300 epoch, teacher ResNet-152: vanilla KD 264 GPU시간·80.55%, DKD 265·80.49%, DIST 265·80.61%. 특징 기반 ReviewKD는 439 GPU시간·80.06%, CRD 307·79.33%. (Table 3)
  - 학습을 늘리면 계속 오름: teacher BEiTv2-B에서 300 / 600 / 1200 / 4800 epoch에 80.96 / 81.64 / 82.35 / 83.08%. (Table 8)
- 잃는 것: [실측·원논문] 특징 기반 방법은 같은 epoch에서 시간이 더 들고 정확도는 낮음 (위 수치). 긴 학습이 탄소 배출을 늘린다고 한계로 적음. (p. 10)
- 옮겨 간 비용: 해당 없음.
- 어떻게 쟀나: 단위 = 총 GPU 시간, epoch / teacher forward 셈? Y (student 1회 학습의 총 GPU 시간에 포함) / 일회성 비용 셈? N (teacher 학습 비용 없음) / 하드웨어 = V100 8장 1대
- 설정: teacher ResNet-152 (82.83%), BEiTv2-B (86.47%), BEiTv2-L (88.39%) → ResNet-50, ViT-T/S, ConvNeXtV2-T. ImageNet-1K 300~4800 epoch, 학습 전략 A1·A2.
- 평균 정확도 외 평가: ImageNet-Real, ImageNet-V2 matched frequency (Table 3), COCO 검출·분할 전이 (Table 11).
- 위치: Table 1 (p. 3), Table 3 (p. 5), Table 6·7 (p. 7), Table 8–10 (p. 8), Table 11 (p. 9)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 7건 / 수정 3건 / 추가 3건 / 확인 불가 0건. 본 판본: arXiv 2305.15781v1 (p.1 표기). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] "전략 A2·300 epoch·ResNet-50 student. GPU 시간(V100 8장 1대)은 KD 264, DKD 265, DIST 265, ReviewKD 439, CRD 307. 정확도는 ResNet-152 teacher 기준 80.55/80.49/80.61/80.06/79.33%. GPU 시간 열 하나를 두 teacher(ResNet-152, BEiTv2-L)가 공유하여 teacher별 시간은 구분 안 됨. (Table 3, p.5)" ← 메모: 300 epoch, teacher ResNet-152: KD 264h·80.55, DKD 265·80.49, DIST 265·80.61, ReviewKD 439·80.06, CRD 307·79.33
  - [수정] "특징 기반 방법은 A2·300 epoch에서 모두 정확도가 낮음. 시간은 CC 265h로 logit 방법과 같고 RKD 282, CRD 307, ReviewKD 439h로 더 큼. (Table 3, p.5)" ← 메모: 잃는 것: 특징 기반 방법은 같은 epoch에서 시간이 더 들고 정확도 낮음
  - [추가] "teacher–student 중간 특징 CKA 유사도 (Fig. 2, p.5)."
  - [수정] "logit 방법 3종의 GPU 시간 차이는 1h 이내 (264–265h). CE 단독 학습 시간이 없어 KD 손실·teacher forward의 증분은 이 표로 산출 불가. (Table 3, p.5)" ← 메모: 메모: logit 방법 264~265h → KD 손실 연산은 총비용에서 무시할 만
  - [추가] "vanilla KD GPU 시간은 300/600/1200 epoch에 264/529/1057h로 epoch에 거의 비례. (Table 3, p.5)"
  - [추가] "ConvNeXtV2-T에서 vanilla KD 300 epoch 653 GPU시간 84.42%, FCMIM 누적 3393 GPU시간 83.89% (약 1/5 시간). 비교 대상의 사전학습 단계까지 시간에 넣은 사례. (Table 10, p.8)"
- 메모:
  - 방법별 총 GPU 시간을 같은 장비에서 재서 정확도 옆에 둔 표(Table 3)는 우리가 만들려는 표와 같은 형식의 선행 사례.
  - logit 기반 방법들은 GPU 시간이 264~265로 사실상 같음. KD 손실 자체의 연산은 총비용에서 무시할 만하다는 근거 (1_Shrink 06–10번의 "KD 오버헤드 절감"이 총비용에서 얼마나 작은지).
  - 큰 teacher가 항상 낫지 않음: BEiTv2-B와 BEiTv2-L teacher의 결과가 거의 같음 (Table 9).

### 03. DeiT ([arXiv: 2012.12877](https://arxiv.org/abs/2012.12877))
- 효율화 방식: 효율화 방법이 아님. ViT를 ImageNet-1K만으로 학습하는 레시피와, distillation token을 통한 증류를 제안. 이후 ViT 증류 연구의 기준 설정.
- 얻는 것 (증류의 정확도 이득, ImageNet, 300 epoch, teacher RegNetY-16GF):
  - DeiT-Ti: 증류 없음 72.2%, 일반 soft 증류 72.2%, hard 증류 74.3%, distillation token 74.5%.
  - DeiT-B: 81.8 / 81.8 / 83.0 / 83.4%. (Table 3)
  - 증류한 모델은 학습을 늘릴수록 계속 오르고, 증류 없는 모델은 400 epoch 이후 포화. (Fig. 3)
- 잃는 것: [실측·원논문] ViT에서는 일반 soft 증류가 이득이 없음 (72.2 → 72.2%). 이득은 hard label 증류에서 나옴. (Table 3)
- 옮겨 간 비용: 해당 없음. 증류로 늘어나는 학습 비용(teacher forward)은 따로 보고 안 됨.
- 어떻게 쟀나: 단위 = 추론 처리량(img/s), 학습 시간(증류 없는 DeiT-B: 8-GPU 1대에서 사전학습 53시간) / teacher forward 셈? N (증류 시의 학습 시간 없음) / 일회성 비용 셈? N / 하드웨어 = V100 16 GB 1장 (처리량), 8-GPU 1대 (학습)
- 설정: teacher RegNetY-16GF (84M 매개변수) → DeiT-Ti/S/B. ImageNet-1K, 300 epoch (긴 학습은 최대 1000).
- 평균 정확도 외 평가: ImageNet Real, ImageNet V2 matched frequency (Table 5), 전이 학습 (Table 7), teacher와의 판단 불일치 비율 (Table 4).
- 위치: Table 2 (p. 9), Table 3 (p. 10), Table 4 (p. 11), Table 5 (p. 12), Fig. 3 (p. 13), Table 7 (p. 14)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 10건 / 수정 3건 / 추가 2건 / 확인 불가 0건. 본 판본: arXiv 2012.12877v2 (p.1 표기). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] "DeiT-Ti(224, 300 epoch): 증류 없음 72.2, soft 72.2, hard 74.3, distillation token(class+distil. late fusion) 74.5% (distil. head 단독 74.6%). (Table 3, p.10)" ← 메모: DeiT-Ti: 72.2 / 72.2 / 74.3 / distillation token 74.5%
  - [수정] "DeiT-B에서 hard·token 증류는 300→800 epoch에 오르고 800 이후 평탄, ↑384 token은 1000 epoch까지 상승. soft 증류는 긴 학습에서도 오르지 않음 (81.6–82.2%). 증류 없음은 400 epoch 이후 포화 (점선). (Fig. 3, p.13)" ← 메모: 증류 모델은 학습을 늘릴수록 계속 오르고 증류 없는 모델은 400 epoch 이후 포화 (Fig. 3)
  - [수정] "위치에 '학습 시간 §6 (p.17), 서론 (p.2)' 추가." ← 메모: 위치: Table 2 p.9 … Table 7 p.14
  - [추가] "teacher RegNetY-16GF의 추론 처리량 334.7 img/s는 DeiT-B student(290.9 img/s)보다 높음. teacher forward가 student보다 가볍지 않은 조합. (Table 5, p.12, V100 16GB, 추론)"
  - [추가] "teacher RegNetY-16GF는 저자들이 직접 학습했고 같은 조건 학습이 DeiT-B보다 20% 느리다고만 적음. teacher 학습 비용은 증류 비교에 넣지 않음. (p.9, p.17)"
- 메모:
  - 표 5의 처리량은 **추론** 속도임. 학습 속도나 증류 비용과 섞어 인용하면 안 됨.
  - 일반 soft 증류가 ViT에서 이득이 없었다는 결과는 우리 실험의 "KD가 CE보다 나은가" 확인이 왜 필요한지의 근거. 이 결과는 CNN teacher·처음부터 학습 조건.

### 04. Distillation Scaling Laws ([arXiv: 2502.08606](https://arxiv.org/abs/2502.08606))
- 효율화 방식: 효율화 방법이 아님. 언어 모델에서 연산 예산과 그 배분(teacher 대 student)에 따라 증류된 student의 성능을 예측하는 스케일링 법칙을 맞춤. 비용을 누가 부담하는지에 따라 최적 배분이 달라짐을 보임.
- 얻는 것: 증류의 총연산을 세 항으로 쓴 식. student 학습 + teacher logit 추론 + teacher 사전학습. 뒤의 두 항은 셀지 말지를 0/1 스위치로 둠. (식 9)
  - 네 가지 시나리오 (Table 2): (a) 둘 다 안 셈 = teacher가 완전히 상각됨, (b) teacher 추론만 셈 = teacher가 이미 있음, (c) teacher 사전학습만 셈 = logit을 저장해 재사용, (d) 둘 다 셈.
  - 결론: teacher가 이미 있거나 student가 여럿이면 증류가 지도학습보다 유리한 연산 구간이 있고, student가 하나이고 teacher도 새로 학습해야 하면 대체로 지도학습이 낫다. (초록)
- 잃는 것: 해당 없음.
- 옮겨 간 비용: 해당 없음. 다만 (c) 시나리오가 "teacher 추론 비용이 logit 저장으로 옮겨 가는" 경우를 식으로 표현함.
- 어떻게 쟀나: 단위 = FLOPs (모델 크기와 토큰 수의 함수로 근사) / teacher forward 셈? 시나리오에 따라 Y·N을 명시 / 일회성 비용 셈? 시나리오에 따라 Y·N을 명시 / 하드웨어 = 확인 안 함
- 설정: 언어 모델 사전학습 (비전 아님). teacher와 student의 크기·학습 토큰 수를 넓게 바꿈.
- 평균 정확도 외 평가: 없음 (교차 엔트로피)
- 위치: 초록 (p. 1), 식 8·9·Table 2 (p. 6)
- **정정 (검수 보고서 근거)**: 회계 계수는 [0,1] 범위이고 네 시나리오가 0과 1 끝점을 씀. 식 9에 캐시 저장·입출력 비용은 없음. teacher 출력 재사용 시나리오는 그 비용을 상각한 경우. 주 목표는 validation cross-entropy지만 부록에서 ARC·MMLU 등도 평가. [식 9·표 2](https://arxiv.org/pdf/2502.08606v2#page=6) · [부록 E.1](https://arxiv.org/pdf/2502.08606v2#page=47)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 6건 / 수정 7건 / 추가 1건 / 확인 불가 1건. 본 판본: PDF 첫 쪽에 arXiv 판본 표기 없음. "Proceedings of the 42nd ICML, PMLR 267, 2025" 카메라 레디 판. 정정 줄이 링크한 v2와 같은 판인지는 확인 불가 (쪽 번호는 정정 줄의 p.6, p.47과 일치). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] "식 9: 3F(N_S)D_S(student 학습) + F(N_T)·δ^Lgt·D_S(teacher logit) + F(N_T)·δ^Pre·3D_T(teacher 학습). δ는 [0,1] 계수이고 Table 2 네 시나리오는 0·1 끝점 사용. (p.6)" ← 메모: 식 9: student 학습 + teacher logit 추론 + teacher 사전학습, 뒤 두 항은 0/1 스위치
  - [수정] "teacher 학습까지 compute에 넣으면 최적 student의 cross-entropy가 항상 지도학습보다 높음 (§5.3, p.8)." ← 메모: 잃는 것: 해당 없음
  - [수정] "식 9에는 저장 비용 항이 없음. 부록 G.4는 전체 logit 저장을 토큰당 약 129KB(float32, 어휘 32,168), C4 약 2T 토큰 기준 약 260 PB로 추정하고 top-k·top-p 절단을 대안으로 실험. (pp.62–63)" ← 메모: 옮겨 간 비용 해당 없음; (c)가 logit 저장으로 옮겨 가는 경우를 식으로 표현
  - [수정] "forward FLOPs/토큰은 2N이 아니라 2N(1 + c1N^−1/3 + c2N^−2/3)로 근사 (p.6, App. H.1)." ← 메모: 단위 = FLOPs (모델 크기와 토큰 수의 함수로 근사)
  - [수정] "하드웨어 = 보고 안 됨 (AXLearn으로 학습했다는 서술만, p.68)." ← 메모: 하드웨어 = 확인 안 함
  - [수정] "C4 영어, 143M–12.6B 매개변수, 최대 512B 토큰. teacher·student는 서로 다른 C4 분할로 학습. (pp.2, 4, 69)" ← 메모: 설정: 언어 모델, 크기·토큰 수 넓게
  - [수정] 본문 줄을 "주 목표는 validation cross-entropy. 부록 E.1에서 하위 과제 평가 (GSM8K·MMLU는 거의 무작위 수준). (p.47)"로 교체 ← 메모: 평균 정확도 외: 없음 (교차 엔트로피) — 정정 줄로 보정됨
  - [추가] "최적 배분은 compute 규모에 따라 teacher 사전학습 위주에서 student 학습·teacher 추론 균등으로 이동 (Table 3, p.8)."
  - [확인 불가] 판본 비교 불가 (저장소 PDF에 arXiv 번호 없음)
- 메모:
  - 우리 비용식의 "일회성 비용 ÷ 재사용 횟수" 항과 "무엇을 세는가에 따른 네 범위"의 가장 직접적인 선행 사례. 단 도메인이 언어 모델이라 수치는 옮길 수 없고 회계 원리만 인용.
  - 식 9에서 student 학습을 forward의 3배로 근사함 (forward + backward). 우리 FLOPs 측정과 대응시킬 때 참고.

### 05. Is KD Actually Greener? ([arXiv: 2602.09691](https://arxiv.org/abs/2602.09691))
- 효율화 방식: 효율화 방법이 아님. 기계번역에서 KD의 환경 비용을 생애주기(teacher 준비, 증류, 배포 추론) 전체로 평가함. student가 teacher보다 이득이 되려면 얼마나 많이 배포되어야 하는지(손익분기 추론량)를 계산.
- 얻는 것:
  - 증류된 student는 고정 비용(증류 오버헤드) 때문에 출발점이 teacher보다 높고, 추론 비용이 낮아 기울기가 작음. 손익분기점 아래에서는 KD가 손해. (4.2절, Fig. 4)
  - 배치 추론을 쓰면 손익분기 추론량이 수억~수십억 토큰으로 올라감 (예: batch 256에서 2.9억~10.9억 토큰, Table 1). teacher 추론도 같이 빨라지기 때문.
- 잃는 것: [실측·원논문] 작은 student(16M)는 손익분기에 빨리 닿지만 번역 품질 상한이 낮음. (4.2절)
- 옮겨 간 비용: 해당 없음. 문장 단위 KD는 teacher가 전체 데이터를 한 번 번역해야 해 생산 비용이 늘고 추론 비용은 그대로라고 분석.
- 어떻게 쟀나: 단위 = 탄소 발자국(kgCO2e), 손익분기 추론 토큰 수. 생애주기 평가 도구(MLCA) 사용 / teacher forward 셈? Y / 일회성 비용 셈? Y (teacher 학습 비용을 넣는 경우와 이미 치른 비용으로 보는 경우를 계수로 구분) / 하드웨어 = 확인 안 함
- 설정: 기계번역 (EN→IS 등), 전용 번역 모델과 LLM (Qwen3-8B·4B → 0.6B). 비전 아님.
- 평균 정확도 외 평가: 해당 없음 (번역 품질 지표)
- 위치: 초록 (p. 1), 4.2절·Table 1 (p. 6)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 7건 / 수정 5건 / 추가 2건 / 확인 불가 0건. 본 판본: arXiv 2602.09691v2 (p.1 표기). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] "문장 단위(배치 없음) EN→IS 추론에서 teacher 대비: student는 절편이 높고 기울기가 작음. (Fig. 4 p.5, §4.2 p.6)" ← 메모: student는 고정 비용으로 출발점이 높고 기울기가 작음; 손익분기 아래는 손해 (4.2절, Fig. 4)
  - [수정] "옮겨 간 비용: offline Word-KD는 학습 중 teacher forward를 teacher 전처리·저장으로 옮김 (p.2). LLM 실험은 top-20 확률(8-bit)과 인덱스를 저장 (App. G.2, p.18). 전처리 연산은 생산 비용에 포함, 저장 용량은 보고 안 됨 (저장은 시스템 경계 밖, p.3)." ← 메모: 옮겨 간 비용 해당 없음; 문장 단위 KD는 teacher가 전체 데이터를 번역
  - [수정] "하드웨어 = NVIDIA V100 1장. 단계별 wall-clock·이용률·에너지 기록 (p.5). 4-GPU 증류는 탄소 약 3배(65M)·5배(16M)로 단일 GPU에 지배됨 (App. F, p.17)." ← 메모: 하드웨어 = 확인 안 함
  - [추가] "회계 가정: PUE 1.24, 0.033 kgCO2e/kWh, GPU 제조 150 kgCO2e, 수명 5년, 이용률 0.8/0.2. 에너지 ≈ 평균 GPU 전력 × wall-clock. (App. D, pp.15–16)"
  - [수정] "본 실험: TF-Big(205M) → 65M·16M, EN→IS/HI/ES (문장쌍 23M/13M/6M). LLM: Qwen3-8B·4B → 0.6B, EN→IS/ES 각 10만 쌍, LoRA. (pp.4–6, 17–18)" ← 메모: 설정: EN→IS 등, Qwen3-8B·4B → 0.6B
  - [수정] "위치: 초록 (p.1), Fig. 4 (p.5), §4.2·Table 1 (pp.5–6), App. D (pp.15–16)" ← 메모: 위치: 초록 p.1, 4.2절·Table 1 p.6
  - [추가] "같은 크기의 No-KD와 KD는 추론 기울기가 같아 손익분기가 없음. 비교는 생산 비용 대 품질 이득으로만. (p.3)"
- 메모:
  - "손익분기는 모델의 고유 속성이 아니라 서빙 조건에 따라 달라진다"는 결론은 우리 Reuse 본전 지점 분석(재사용 횟수 N)에 그대로 적용되는 논리. 단위와 작업이 달라 수치는 인용 불가.
  - teacher 비용을 "이미 치른 것"으로 볼지를 명시적 계수로 둔 점은 04번의 스위치와 같은 발상.

### 06. Are Coreset Selection Methods Worth Their Cost? ([arXiv: 2609.22894](https://arxiv.org/abs/2609.22894))
- 효율화 방식: 효율화 방법이 아님. 데이터 선택(코어셋) 방법을 **선택 시간 + 학습 시간**의 한 축에서 평가하는 벤치마크. 데이터셋 4종, 선택 방법 11종, 비율 5단계, seed 3개, 1,674회 실행.
- 얻는 것:
  - CIFAR-10과 Tiny ImageNet의 예산 지점 8개씩에서, 정교한 선택 방법이 이긴 지점이 하나도 없음. 승자는 클래스 균형 무작위, 반복 무작위, 전체 데이터 학습 중 하나. (초록)
  - ImageNet-1K의 비슷한 비용 비교에서는 **전체 데이터를 더 적은 epoch 학습**한 것이 가장 정확함. (초록)
  - 선택 비용은 전체 데이터를 한 번 훑는 고정 비용이 지배적이라 작은 비율을 고른다고 줄지 않음. 재사용 횟수 K가 늘면 실행당 선택 비용은 1/K로 줄지만 무작위보다 싸지지는 않음. (Fig. 5a, p. 9)
- 잃는 것: [실측·원논문] 낮은 비율에서 1 epoch짜리 GraNd·EL2N은 무작위보다 크게 나쁨 (CIFAR-10 5%에서 EL2N −46.9%p). (5절)
- 옮겨 간 비용: 선택 연산 (전체 데이터 훑기). 이 논문의 측정 대상 자체.
- 어떻게 쟀나: 단위 = 실제 시간 (통제된 시간 측정 실행으로 추정한 GPU 시간) / teacher forward 셈? 해당 없음 (KD 아님) / 일회성 비용 셈? Y (선택 시간을 총비용에 포함, 재사용 횟수로 나눈 경우도 분석) / 하드웨어 = RTX 1장 (모델명은 확인 안 함). 비용은 단일 장비 추정치라고 한계에 적음.
- 설정: CIFAR-10, Tiny ImageNet, ImageNet-1K 등 4종, ResNet-18 (일부 ResNet-50 전이), 200 epoch 기준 레시피. KD 없음.
- 평균 정확도 외 평가: 없음
- 위치: 초록 (p. 1), Fig. 5·5절 (p. 9)
- **정정 (검수 보고서 근거)**: ImageNet-1K 기본 레시피는 90 epoch (200 epoch는 작은 데이터셋 기본값). 비용은 선택 시간을 직접 재고 학습+평가 시간을 프로브로 추정한 값. "같은 비용에서 전체 데이터 단축 학습이 이겼다"는 실험한 비교점에 한정. [표 4](https://arxiv.org/pdf/2609.22894v2#page=14) · [부록 I](https://arxiv.org/pdf/2609.22894v2#page=20)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 8건 / 수정 5건 / 추가 3건 / 확인 불가 0건. 본 판본: arXiv 2609.22894v2 (p.1 표기). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] "epoch-1 GraNd·EL2N(1-epoch probe 10개 평균 점수)은 낮은 비율에서 무작위보다 크게 나쁨. CIFAR-10 f=0.05에서 EL2N이 무작위 평균보다 46.9%p 낮음. (§3 p.4, §5 p.9)" ← 메모: 잃는 것: 1 epoch GraNd·EL2N, CIFAR-10 5%에서 EL2N −46.9%p
  - [수정] "선택 시간은 직접 측정, 학습·평가 시간은 probe(warmup 2, 측정 10 epoch, test 3회)로 추정. f=1은 외삽이며 직접 측정과 +0.40~+1.79% 차이. (pp.4–5, App. I p.20)" ← 메모: 단위 = 실제 시간 (통제된 측정 실행으로 추정한 GPU 시간)
  - [수정] "하드웨어 = 유휴 RTX 4090 1대에서 비용 측정. 정확도 실행은 같은 세대 여러 호스트. (p.4)" ← 메모: 하드웨어 = RTX 1장 (모델명 확인 안 함)
  - [수정] "데이터셋: CIFAR-10, Tiny ImageNet, CUB-200, ImageNet-1K (+Retinal OCT 부록). 하위 학습 200 epoch (ImageNet 90). 선택기 probe 40 epoch (ImageNet 10). (pp.3–4)" ← 메모: 설정: CIFAR-10, Tiny ImageNet, ImageNet-1K 등 4종, ResNet-18, 200 epoch
  - [수정] "부분군 지표 없음. recovery (Fig. 3), 평균 순위와 bootstrap 구간 (Table 2), 무작위 30개 중 최악값과의 비교 (p.6, p.19) 보고." ← 메모: 평균 정확도 외 평가: 없음
  - [추가] "선택 하한: CIFAR-10 약 84–241초, Tiny ImageNet 약 10–31분, Herding 고정 성분 ImageNet 약 19,518초. ImageNet f=0.05에서 선택이 전체 시간의 62–77%. (pp.5, 7, 20)"
  - [추가] "비용 교차점 K* = 0.07–0.31로 K=1에서도 전체 데이터 200-epoch 학습보다 싸지만 정확도는 낮음. 교차는 비용에서만. (pp.8–9, Table 9)"
  - [추가] "선택기 probe 40 epoch만으로 200-epoch 전체 학습의 20% (p.7)."
- 메모:
  - 우리 실험의 "자명한 기준선(전체 데이터를 짧게, 무작위 부분집합)을 같은 비용 축에 놓는다"는 설계의 직접 근거. KD 설정에서 같은 결론이 나오는지가 우리 질문.
  - 선택 비용의 상각 분석(Fig. 5a)은 Reuse의 재사용 횟수 분석과 같은 틀.
  - 흔히 쓰는 코드베이스의 오류 9건을 고쳤고 그중 하나가 Herding 기준선을 5.9%p 바꿨다고 보고. 기준선 구현 검증의 필요성.

### 07. Canonical KD for Semantic Segmentation ([arXiv: 2604.25530](https://arxiv.org/abs/2604.25530))
- 효율화 방식: 효율화 방법이 아님. 의미 분할 KD에서 방법들을 **같은 반복 수**가 아니라 **같은 실제 연산 시간**으로 비교하면 순위가 바뀐다는 것을 보임.
- 얻는 것:
  - 같은 40k 반복 (Cityscapes): CIRKD 73.5 mIoU에 30.2 GPU시간, 단순 특징 KD는 73.7 mIoU에 9.2 GPU시간. (4.2절)
  - 같은 30 GPU시간: CIRKD 73.5, 단순 logit KD 76.4, 단순 특징 KD 75.4 mIoU. (Fig. 1)
  - 오래 학습하면 PSPNet ResNet-18 student가 ResNet-101 teacher의 99%에 도달 (Cityscapes 79.0 vs 79.8 mIoU). (초록)
- 잃는 것: 해당 없음. 표 1의 방법 간 비교는 코드베이스마다 기준선이 달라 직접 비교가 안 된다고 스스로 적음.
- 옮겨 간 비용: 해당 없음.
- 어떻게 쟀나: 단위 = GPU 시간, 반복 수 / teacher forward 셈? Y (학습의 실제 시간에 포함) / 일회성 비용 셈? N / 하드웨어 = 확인 안 함
- 설정: PSPNet ResNet-101 → PSPNet ResNet-18, Cityscapes·ADE20K. 40k~160k 반복, 최대 300 GPU시간.
- 평균 정확도 외 평가: 해당 없음 (mIoU)
- 위치: 초록 (p. 1), Table 1·Fig. 1·4.2절 (p. 3)
- **정정 (검수 보고서 근거)**: 40k iteration / 30 GPU시간 비교와 장기 비교는 다른 프로토콜. 짧은 비교는 CIRKD 설정이고 teacher는 DeepLabV3-R101. 2×A100은 장기 그래프 기준. 반지도 확장은 별도로 320 GPU시간까지 감. [§4.2–4.4](https://arxiv.org/pdf/2604.25530v2#page=3)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 10건 / 수정 2건 / 추가 3건 / 확인 불가 0건. 본 판본: arXiv 2604.25530v2 (p.1 표기). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] "하드웨어: 장기 학습은 A100 2장 (Fig. 2–3). 짧은 비교(40k, 30 GPU시간)의 장비는 명시 안 됨." ← 메모: 하드웨어 = 확인 안 함
  - [수정] "위치: 초록 (p.1), §3.2 (p.2), Table 1·2, Fig. 1·2, §4.2–4.3 (p.3), Fig. 3·§4.4 (p.4)" ← 메모: 위치: 초록 p.1, Table 1·Fig. 1·4.2절 p.3
  - [추가] "복잡한 분할 KD는 반복당 비용이 최대 3배 (p.1). 40k 반복에서 CIRKD 30.2 대 특징 KD 9.2 GPU시간 (p.3)."
  - [추가] "큰 teacher(Mask2Former, 83.7%)는 student를 77.7→77.8%로만 올리고 학습 시간은 89→296 GPU시간 (3배). teacher forward 비용이 총비용을 좌우하는 사례. (p.4)"
  - [추가] "Cityscapes 최고점 도달 GPU시간: 지도 67, CIRKD 44, logit 89, Pure KD 160, 특징 KD 55h (Table 2, p.3)."
- 메모: "반복 수가 같다고 예산이 같은 것이 아니다"의 가장 분명한 사례. 스텝당 비용이 다른 방법들을 epoch 기준으로 비교하면 안 된다는 근거로 3절에 인용. 분류가 아니라 분할이라는 점은 명시.

## 정리 메모

- 비용을 세는 범위에 대한 근거: 04번(네 시나리오, 식 9), 05번(생애주기와 손익분기), 06번(선택 시간 포함, 재사용 횟수로 상각).
- 같은 비용에서 비교해야 한다는 근거: 02번(GPU 시간을 정확도 옆에), 06번(시간 축 하나), 07번(반복 수 대 실제 시간).
- 학습 길이가 결론을 바꾼다는 근거: 01번, 02번 Table 8, 03번 Fig. 3. 세 논문 모두 "증류는 길게 학습할수록 계속 오른다"고 보고함. 짧은 학습에서의 비교가 긴 학습에서 유지되는지는 별개.
- 7편 중 비전 분류 KD의 학습 비용을 teacher forward까지 포함해 실측한 것은 02번뿐임.
