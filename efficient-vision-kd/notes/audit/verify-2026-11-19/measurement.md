# 원문 대조 보고서: `notes/5_Measurement (Q2).md` 01–07

- 대조 대상: `$PAPER/notes/5_Measurement (Q2).md` (01–07 블록, 정정 줄 포함)
- 원문: `$PAPER/Related Work/Efficient KD papers/5_Measurement (Q2)/0N_*.pdf` (쪽 번호 = PDF 쪽, 첫 장 = 1)
- 추출 텍스트: `verify/tools/meas_txt/0N.txt`, 표 렌더링: `verify/tools/meas_img/`
- 기존 검수(`notes/audit/paper-by-paper.md` "Measurement · 7개 항목")와 겹치는 지적은 원문에서 다시 확인한 뒤 적음. 검수 보고서에만 있고 원문에서 확인 못 한 것은 적지 않음.

---

## Measurement-01 A Good Teacher is Patient and Consistent
- 본 판본: arXiv 2106.05237v2 (p.1 표기)
- 대조 결과: 일치 6건 / 수정 8건 / 추가 4건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 같은 뷰 + mixup + 매우 긴 학습 = 함수 맞추기 | "(i) make sure that teacher and student always get identical inputs … (ii) … mixup and (iii) use very long training schedules" | §5, p.8; §3.1, p.3 | 일치 | – |
| 2 | 300/1200/4800/9600 epoch → 80.30/81.54/82.18/82.31% | 수치 맞음. Table 1의 **T224→S224** 행. 같은 표에 T224→S160, T384→S224 행이 따로 있음. teacher 단독 83.0% | Table 1, p.7; §3.2, p.5 | 수정 | "T224→S224 행 기준 300/1200/4800/9600 epoch에서 80.30/81.54/82.18/82.31% (teacher 83.0%). (Table 1, p.7)" |
| 3 | teacher를 고정된 crop으로만 돌린 설정은 일찍 포화·과적합 (Fig. 5 왼쪽) | fixed teacher = "teacher's predictions are constant for a given image (precomputed target)". crop 하나로 한정되지 않음 (fix/rs, fix/cc, fix/ic ens). ImageNet 본문은 "starts overfitting after 600 epochs", Table 3 수치는 1200 epoch 78.11%가 정점이고 9600 epoch 76.95%로 떨어짐 | §3.1.1, p.4; §3.2, p.5; Table 3, p.12 | 수정 | "teacher 예측을 이미지마다 고정(사전 계산)한 설정은 ImageNet에서 1200 epoch 78.11%가 정점이고 9600 epoch 76.95%로 하락함. 같은 9600 epoch의 FunMatch 82.31%보다 5.36%p 낮음. 본문은 600 epoch 이후 과적합으로 서술. (Fig. 5 왼쪽, p.6; Table 3, p.12)" |
| 4 | Flowers-102 1,000 → 10,000 → 100,000 epoch: 77.51 → 92.83 → 95.54% | 수치 맞음. 같은 표의 최장 설정은 **1,000,000 epoch 96.93%** (teacher 97.82%). 해상도 128px | Table 4, p.13; App. A, p.11 | 수정 | "Flowers-102(128px)에서 1,000 / 10,000 / 100,000 / 1,000,000 epoch에 77.51 / 92.83 / 95.54 / 96.93% (teacher 97.82%). (Table 4, p.13)" |
| 5 | 300 vs 9600 epoch 차이 2.0%p | 82.31 − 80.30 = 2.01%p | Table 1, p.7 | 일치 | – |
| 6 | Shampoo로 4800 epoch 정확도를 1200 epoch에 | Fig. 5 가운데는 **consistent teaching**(Adam) 4800 epoch 대 consistent teaching+Shampoo 1200 epoch 비교 (Table 3: 81.58 대 81.44%). "with minimal step time overhead"는 정성 서술, 시간 수치 없음. 별도로 Fig. 5 왼쪽에서 FunMatch 1200 epoch가 consistent teaching 4800 epoch와 같아 "saving 75% compute resource"라고 적음 (epoch 기준 환산) | §3.2, p.5; §3.4·Fig. 5, p.6; Table 3, p.12 | 수정 | "Shampoo는 consistent teaching 기준 4800 epoch(Adam, 81.58%) 수준을 1200 epoch(81.44%)에 도달. 스텝 시간 오버헤드는 'minimal'이라는 정성 서술뿐. 'compute 75% 절감'은 FunMatch 1200 epoch 대 consistent teaching 4800 epoch의 epoch 비율. (§3.2 p.5, Fig. 5 p.6, Table 3 p.12)" |
| 7 | 옮겨 간 비용 해당 없음; 매 스텝 teacher forward라 Reuse와 충돌 | "it is tempting to precompute the teacher's activations … offline once to save compute … this fixed teacher approach does not work well." 저장 용량·시간 수치 없음 | §1, p.2 | 일치 | – |
| 8 | 단위 = epoch, teacher forward N, 일회성 N, Google Cloud TPU (사양 없음) | "We also report our batch sizes, epochs or total number of update steps, which allow to estimate resource requirements." TPU 종류·수, 실제 시간, FLOPs 없음. Shampoo 부록도 "TPU cores"만 언급 | §2, p.3; App. D, p.11 | 일치 | (보강 가능) "batch 4096(ImageNet)·512(그 외), epoch·update step만 보고. (p.3)" |
| 9 | Flowers-102, Pets, Food-101, SUN397은 최대 100만 epoch | 최장 학습: Flowers 1,000,000 / Pets 30,000 / Food101 3,000 / SUN397 3,000 epoch. 작은 데이터셋은 128×128 입력 | Table 4, p.13; §2, p.3 | 수정 | "작은 데이터셋 최장 학습은 Flowers-102 100만, Pets 3만, Food-101·SUN397 3,000 epoch. 입력 해상도 128px (ImageNet만 224px). (Table 4 p.13, p.3)" |
| 10 | student = ResNet-50 (처음부터) | "we use a BiT-ResNet-50 variant" — BatchNorm 대신 GroupNorm + weight standardization | §2, p.2 | 수정 | "student는 BiT-ResNet-50 (GroupNorm·weight standardization, 처음부터 학습). (p.2)" |
| 11 | 해상도 224→224, 224→160, 384→224 | 맞음 (ImageNet 한정) | Table 1, p.7 | 일치 | – |
| 12 | 평균 정확도 외 평가: Fig. 6 도메인 밖 증류, 그 외 없음 | 그 외에도 train/validation distillation loss 곡선 (Fig. 3, 부록 Fig. 9–12)과 label 학습 기준선 과적합 비교 (Fig. 7) 있음. 부분군·최악 클래스 지표는 없음 | Fig. 3 p.4; Fig. 7 p.8; App. C p.11, pp.14–17 | 수정 | "도메인 밖 이미지로 증류한 정확도 (Fig. 6, p.8), distillation loss 학습·검증 곡선 (Fig. 3, p.4; Fig. 9–12, pp.14–17). 부분군 지표 없음." |
| 13 | 위치: Fig. 3 p.4, Fig. 4 p.5, Fig. 5 p.6, Table 1·2 p.7, Fig. 6 p.8, Table 3 p.12, Table 4 p.13 | 전부 맞음 | – | 일치 | – |
| 14 | 메모: Fig. 5의 고정 뷰 teacher = "뷰 1개" 조건 | 고정 teacher 계열에는 1,000개 inception crop 예측을 평균한 fix/ic ens도 있음. Flowers(Fig. 3)에서 이 변형도 consistent 설정보다 낮게 정체. ImageNet 고정 teacher 변형은 명시 안 됨 | §3.1.1, p.4; Fig. 3, p.4 | 수정 | "고정 teacher 계열은 1장 resize·center crop뿐 아니라 crop 1,000개 평균(fix/ic ens)도 포함하며 모두 낮은 값에서 정체 (Fig. 3, p.4). '뷰 여러 개를 평균한 고정 목표'도 온라인 KD를 따라잡지 못한 사례로 인용 가능. ImageNet 곡선의 고정 변형은 명시 안 됨." |
| 15 | (추가) teacher 연산량 | "requires roughly 10x more compute than the standard ResNet-50" | §2, p.2 | 추가 | "teacher BiT-M-R152x2의 연산은 ResNet-50의 약 10배 (p.2). 매 스텝 teacher forward의 상대 비용 근거." |
| 16 | (추가) 앙상블 teacher | T384+224 앙상블 teacher, 9600 epoch **82.82%** | §3.6, p.7; Table 3, p.12 | 추가 | "앙상블 teacher(T224+T384 logit 평균) 9600 epoch 82.82%. 단일 teacher 82.31%와 별개 결과. (p.7, Table 3)" |
| 17 | (추가) 증류 없는 긴 학습 기준선 | "Best from labels" 1200 epoch 78.15% 정점, 4800 epoch 76.59%로 하락 | Table 3, p.12; Fig. 7, p.8 | 추가 | "같은 레시피의 label 학습(증류 없음)은 1200 epoch 78.15%가 정점이고 4800 epoch 76.59%로 하락. 긴 학습 이득은 증류 조건에서만 나타남. (Table 3 p.12, Fig. 7 p.8)" |
| 18 | (추가) 메모 3번 "100 epoch 실험에 그대로 기대하면 안 됨" | "training for a relatively short but common duration of 100 epochs leads to much worse performance than the transfer baseline." | §3.1.2, p.5 | 추가 | "논문 자체가 100 epoch 학습은 transfer 기준선보다 훨씬 나쁘다고 적음 (§3.1.2, p.5)." |

---

## Measurement-02 VanillaKD
- 본 판본: arXiv 2305.15781v1 (p.1 표기)
- 대조 결과: 일치 7건 / 수정 3건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 효율화 방법 아님; 강한 레시피·대규모 데이터에서 vanilla KD 재평가 | 맞음 | 초록 p.1; §3, pp.3–5 | 일치 | – |
| 2 | 300 epoch, teacher ResNet-152: KD 264h·80.55, DKD 265·80.49, DIST 265·80.61, ReviewKD 439·80.06, CRD 307·79.33 | 수치 전부 맞음. 조건: 학습 전략 **A2**. "GPU hours are evaluated on a single machine with 8 V100 GPUs." GPU hours 열은 하나이고 ResNet-152·BEiTv2-L 두 teacher 블록이 함께 씀 → teacher별 시간 구분 없음 | Table 3, p.5 | 수정 | "전략 A2·300 epoch·ResNet-50 student. GPU 시간(V100 8장 1대)은 KD 264, DKD 265, DIST 265, ReviewKD 439, CRD 307. 정확도는 ResNet-152 teacher 기준 80.55/80.49/80.61/80.06/79.33%. GPU 시간 열 하나를 두 teacher(ResNet-152, BEiTv2-L)가 공유하여 teacher별 시간은 구분 안 됨. (Table 3, p.5)" |
| 3 | BEiTv2-B teacher, 300/600/1200/4800 epoch: 80.96/81.64/82.35/83.08% | 맞음 (ResNet-50, 전략 A1, teacher 86.47%) | Table 8, p.8 | 일치 | (보강) "ConvNeXtV2-T는 1200 epoch까지만 (85.03%)." |
| 4 | 잃는 것: 특징 기반 방법은 같은 epoch에서 시간이 더 들고 정확도 낮음 | 정확도는 A2·300 epoch에서 특징 기반 전부 낮음. 시간은 방법마다 다름: CC 265h(=logit 방법), RKD 282, CRD 307, ReviewKD 439. 600 epoch에서 CC 529h = logit 방법 529h | Table 3, p.5 | 수정 | "특징 기반 방법은 A2·300 epoch에서 모두 정확도가 낮음. 시간은 CC 265h로 logit 방법과 같고 RKD 282, CRD 307, ReviewKD 439h로 더 큼. (Table 3, p.5)" |
| 5 | 긴 학습이 탄소 배출을 늘린다고 한계로 적음 (p.10) | "longer training times, which inevitably contribute to increased carbon emissions" — 측정값 없음 | p.10 | 일치 | (보강) "탄소 배출량 측정값 없음." |
| 6 | 단위 = 총 GPU 시간·epoch; teacher forward Y; 일회성 N; V100 8장 1대 | "along with the total GPU training time for each student". teacher forward 포함은 온라인 KD 학습 범위에서의 추론이고 분리 측정 없음. teacher 학습 시간 없음 | §3.2·Table 3, p.5 | 일치 | (보강) "teacher forward 포함은 추론 (student 학습 총 GPU 시간에 묶임, 분리 측정 없음)." |
| 7 | 설정: teacher ResNet-152 82.83, BEiTv2-B 86.47, BEiTv2-L 88.39; 300~4800 epoch, A1·A2 | 맞음 | Table 3 p.5; §3.6 p.8 | 일치 | – |
| 8 | 평균 정확도 외: IN-Real, IN-V2 (Table 3), COCO (Table 11) | 맞음. 그 외 teacher–student 특징 유사도 CKA 분석 (Fig. 2) | Table 3 p.5; Fig. 2 p.5; Table 11 p.9 | 추가 | "teacher–student 중간 특징 CKA 유사도 (Fig. 2, p.5)." |
| 9 | 위치: Table 1 p.3, Table 3 p.5, Table 6·7 p.7, Table 8–10 p.8, Table 11 p.9 | 전부 맞음 | – | 일치 | – |
| 10 | 메모: logit 방법 264~265h → KD 손실 연산은 총비용에서 무시할 만 | 원문에 CE만 쓴 학습 시간이나 손실 단위 프로파일 없음. 확인되는 것은 logit 방법 세 개 사이 차이가 1h 이내라는 것뿐 | Table 3, p.5 | 수정 | "logit 방법 3종의 GPU 시간 차이는 1h 이내 (264–265h). CE 단독 학습 시간이 없어 KD 손실·teacher forward의 증분은 이 표로 산출 불가. (Table 3, p.5)" |
| 11 | 메모: BEiTv2-B와 L teacher 결과가 거의 같음 (Table 9) | KD 80.96/80.89, 81.64/81.68, 82.35/82.27%로 거의 같음. DIST는 1200 epoch에서 82.33 대 81.82 (0.51%p) | Table 9, p.8 | 일치 | (보강) "방법에 따라 다름: DIST 1200 epoch는 B 82.33 대 L 81.82%." |
| 12 | (추가) 학습 길이에 따른 GPU 시간 | 1200 epoch: DKD 1059, DIST 1058, KD 1057h. 300→600→1200 epoch에 KD 264→529→1057h로 epoch에 거의 비례 | Table 3, p.5 | 추가 | "vanilla KD GPU 시간은 300/600/1200 epoch에 264/529/1057h로 epoch에 거의 비례. (Table 3, p.5)" |
| 13 | (추가) MIM과 시간 비교 | vanilla KD 300 epoch 653h·84.42%, 1200 epoch 2612h·85.03%. FCMIM은 사전학습 1334h부터 누적 3393h(IN-21K 경유)·83.89%, 1902h(IN-1K 미세조정)·82.94%. "consuming only one-fifth of the training time". 장비는 Table 10에 명시 안 됨 | Table 10·§3.7, p.8 | 추가 | "ConvNeXtV2-T에서 vanilla KD 300 epoch 653 GPU시간 84.42%, FCMIM 누적 3393 GPU시간 83.89% (약 1/5 시간). 비교 대상의 사전학습 단계까지 시간에 넣은 사례. (Table 10, p.8)" |

---

## Measurement-03 DeiT
- 본 판본: arXiv 2012.12877v2 (p.1 표기)
- 대조 결과: 일치 10건 / 수정 3건 / 추가 2건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 효율화 방법 아님; ImageNet-1K만으로 ViT 학습 + distillation token | 맞음 | 초록 p.1 | 일치 | – |
| 2 | DeiT-Ti: 72.2 / 72.2 / 74.3 / distillation token 74.5% | 수치 맞음. 74.5%는 **class+distillation 두 head의 late fusion**. distillation embedding 단독은 74.6% | Table 3, p.10 | 수정 | "DeiT-Ti(224, 300 epoch): 증류 없음 72.2, soft 72.2, hard 74.3, distillation token(class+distil. late fusion) 74.5% (distil. head 단독 74.6%). (Table 3, p.10)" |
| 3 | DeiT-B: 81.8 / 81.8 / 83.0 / 83.4% | 맞음 (83.4 = class+distil.) | Table 3, p.10 | 일치 | – |
| 4 | 증류 모델은 학습을 늘릴수록 계속 오르고 증류 없는 모델은 400 epoch 이후 포화 (Fig. 3) | 증류 없음은 수평 점선 기준선 ("as it saturates after 400 epochs"). soft 증류 곡선은 81.8→82.2→81.6→81.8%로 오르지 않음. hard·token(224)은 800→1000 epoch에서 평탄. ↑384 token만 1000 epoch까지 오름 | Fig. 3, p.13 | 수정 | "DeiT-B에서 hard·token 증류는 300→800 epoch에 오르고 800 이후 평탄, ↑384 token은 1000 epoch까지 상승. soft 증류는 긴 학습에서도 오르지 않음 (81.6–82.2%). 증류 없음은 400 epoch 이후 포화 (점선). (Fig. 3, p.13)" |
| 5 | 잃는 것: ViT에서 soft 증류 이득 없음 (72.2→72.2) | 224에서 Ti/S/B 모두 동일. B↑384에서 83.1→83.2% | Table 3, p.10 | 일치 | (보강) "B↑384에서만 +0.1%p." |
| 6 | 옮겨 간 비용 해당 없음; teacher forward 학습 비용 따로 보고 안 됨 | 증류 학습의 시간 측정 없음 (본문·부록 검색: hour, time, cost, GPU) | – | 일치 | – |
| 7 | 학습 시간: 증류 없는 DeiT-B, 8-GPU 1대 53시간 | "A typical training of 300 epochs takes 37 hours with 2 nodes or 53 hours on a single node for the DeiT-B." 384 미세조정 20시간 (25 epoch). DeiT-S/Ti는 4 GPU에서 3일 미만. "a similar training with a RegNetY-16GF … is 20% slower" | §6, p.17; §1, p.2 | 일치 | (보강) "2노드면 37시간. 384 해상도 미세조정 1노드 20시간. (p.17)" |
| 8 | 하드웨어: V100 16 GB 1장 (처리량), 8-GPU 1대 (학습) | 처리량: "one 16GB V100 GPU … largest possible batch size … average time over 30 runs". 학습 GPU 모델은 p.17에 명시 안 됨 | Table 5 caption, p.12; p.17 | 일치 | (보강) "학습 GPU 모델은 명시 안 됨." |
| 9 | 설정: teacher RegNetY-16GF (84M) → DeiT-Ti/S/B, 300 epoch (최대 1000) | 맞음. teacher는 저자들이 "trained with the same data and same data-augmentation as DeiT", 82.9% | §5.1, p.9 | 일치 | (보강) "teacher는 저자들이 같은 데이터·증강으로 직접 학습 (82.9%)." |
| 10 | 평균 정확도 외: Real, V2 (Table 5), 전이 (Table 7), 판단 불일치 (Table 4) | 맞음 | pp.11–14 | 일치 | – |
| 11 | 위치: Table 2 p.9 … Table 7 p.14 | 표 위치 맞음. 학습 시간 서술(p.2, p.17)의 위치가 빠짐 | p.2, p.17 | 수정 | "위치에 '학습 시간 §6 (p.17), 서론 (p.2)' 추가." |
| 12 | 메모: Table 5 처리량은 추론 속도 | 맞음 | Table 5, p.12 | 일치 | – |
| 13 | (추가) teacher 추론 처리량 | RegNetY-16GF 334.7 im/s, DeiT-B 292.3 im/s, DeiT-B⚗ 290.9 im/s, DeiT-S⚗ 936.2, DeiT-Ti⚗ 2529.5 im/s | Table 5, p.12 | 추가 | "teacher RegNetY-16GF의 추론 처리량 334.7 img/s는 DeiT-B student(290.9 img/s)보다 높음. teacher forward가 student보다 가볍지 않은 조합. (Table 5, p.12, V100 16GB, 추론)" |
| 14 | (추가) teacher 준비 비용 | teacher는 저자들이 직접 학습. 비슷한 학습이 DeiT-B 학습보다 "20% slower". teacher 학습 시간 절대값은 없음 | p.9; p.17 | 추가 | "teacher RegNetY-16GF는 저자들이 직접 학습했고 같은 조건 학습이 DeiT-B보다 20% 느리다고만 적음. teacher 학습 비용은 증류 비교에 넣지 않음. (p.9, p.17)" |

---

## Measurement-04 Distillation Scaling Laws
- 본 판본: PDF 첫 쪽에 arXiv 판본 표기 없음. "Proceedings of the 42nd ICML, PMLR 267, 2025" 카메라 레디 판. 정정 줄이 링크한 v2와 같은 판인지는 확인 불가 (쪽 번호는 정정 줄의 p.6, p.47과 일치)
- 대조 결과: 일치 6건 / 수정 7건 / 추가 1건 / 확인 불가 1건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 언어 모델 증류 스케일링 법칙; 비용 부담 주체에 따라 최적 배분이 달라짐 | 맞음 | 초록 p.1; §5.3 p.8 | 일치 | – |
| 2 | 식 9: student 학습 + teacher logit 추론 + teacher 사전학습, 뒤 두 항은 0/1 스위치 | FLOPs ≈ 3F(N_S)D_S + F(N_T)(δ_T^Lgt D_S + δ_T^Pre 3D_T), "δ ∈ [0, 1]". 0/1은 Table 2 시나리오가 쓰는 끝점 | 식 9, p.6 | 수정 | "식 9: 3F(N_S)D_S(student 학습) + F(N_T)·δ^Lgt·D_S(teacher logit) + F(N_T)·δ^Pre·3D_T(teacher 학습). δ는 [0,1] 계수이고 Table 2 네 시나리오는 0·1 끝점 사용. (p.6)" |
| 3 | 네 시나리오 (a)–(d) | (a) 0,0 "fully amortized teacher", (b) 1,0 "teacher already exists … server model", (c) 0,1 "we store the logits for reuse … sufficiently many students", (d) 1,1 "worst case" | Table 2, p.6 | 일치 | – |
| 4 | 결론: teacher가 있거나 student가 여럿이면 증류 유리 구간, student 하나+teacher 새로 학습이면 지도학습 | 맞음. 유리 구간의 상한 compute가 "scales predictably with student size" | 초록 p.1; §5.3 p.8 | 일치 | (보강) "유리 구간의 상한 compute는 student 크기에 따라 커짐." |
| 5 | 잃는 것: 해당 없음 | "When teacher training is included in the compute, the best student cross-entropy is always higher than in the supervised setting." | §5.3, p.8 | 수정 | "teacher 학습까지 compute에 넣으면 최적 student의 cross-entropy가 항상 지도학습보다 높음 (§5.3, p.8)." |
| 6 | 옮겨 간 비용 해당 없음; (c)가 logit 저장으로 옮겨 가는 경우를 식으로 표현 | 식 9에 저장·입출력 항 없음 (정정 줄과 일치). 단 부록 G.4가 저장 용량을 숫자로 추정: V=32168, float32이면 "each token requires … ≈129KB", C4 전체(약 2T 토큰) "approximately 260 Petabytes". top-k·top-p 절단으로 "reduce repeated inference and storage costs" | App. G.4, pp.62–63; 각주 5, p.6 | 수정 | "식 9에는 저장 비용 항이 없음. 부록 G.4는 전체 logit 저장을 토큰당 약 129KB(float32, 어휘 32,168), C4 약 2T 토큰 기준 약 260 PB로 추정하고 top-k·top-p 절단을 대안으로 실험. (pp.62–63)" |
| 7 | 단위 = FLOPs (모델 크기와 토큰 수의 함수로 근사) | F(N) ≈ 2N이 아니라 "F(N) ≈ 2N(1 + c1N^−1/3 + c2N^−2/3)" (어텐션 기여 반영) | p.6; App. H.1 | 수정 | "forward FLOPs/토큰은 2N이 아니라 2N(1 + c1N^−1/3 + c2N^−2/3)로 근사 (p.6, App. H.1)." |
| 8 | 하드웨어 = 확인 안 함 | 본문·부록 검색(GPU, TPU, accelerator, hardware, hours) 결과 장비 명시 없음. "trained using AXLearn"만 | App. I, p.68 | 수정 | "하드웨어 = 보고 안 됨 (AXLearn으로 학습했다는 서술만, p.68)." |
| 9 | 설정: 언어 모델, 크기·토큰 수 넓게 | 143M–12.6B, 최대 512B 토큰, C4 영어. teacher와 student는 C4의 다른 분할(각 90B 고유 토큰) | p.2; §4.1 p.4; p.69 | 수정 | "C4 영어, 143M–12.6B 매개변수, 최대 512B 토큰. teacher·student는 서로 다른 C4 분할로 학습. (pp.2, 4, 69)" |
| 10 | 평균 정확도 외: 없음 (교차 엔트로피) — 정정 줄로 보정됨 | 부록 E.1: ARC Easy/Challenge, HellaSwag, PIQA, SciQ, WinoGrande, LAMBADA, TriviaQA, WebQS, GSM8K, MMLU. GSM8K·MMLU는 "near-random" | App. E.1·Fig. 35, p.47 | 수정 | 본문 줄을 "주 목표는 validation cross-entropy. 부록 E.1에서 하위 과제 평가 (GSM8K·MMLU는 거의 무작위 수준). (p.47)"로 교체 |
| 11 | 위치: 초록 p.1, 식 8·9·Table 2 p.6 | 맞음 | p.1, p.6 | 일치 | – |
| 12 | 정정 줄: 계수 [0,1], 네 시나리오는 끝점, 식 9에 저장 비용 없음, (c)는 상각, 부록 E.1 ARC·MMLU | 전부 원문과 맞음 | p.6, p.47 | 일치 | (보강) "단 저장 용량 추정은 부록 G.4에 숫자로 있음 (위 6번)." |
| 13 | 메모: student 학습 = forward의 3배 | 3F(N_S)D_S. teacher 학습도 3F(N_T)D_T, teacher logit은 F(N_T)D_S | 식 9, p.6 | 일치 | – |
| 14 | (추가) 최적 배분 경향 | Table 3: 작은 student·작은 compute는 teacher 사전학습에 대부분, 큰 compute는 student 학습·teacher 추론에 고르게 | Table 3, p.8 | 추가 | "최적 배분은 compute 규모에 따라 teacher 사전학습 위주에서 student 학습·teacher 추론 균등으로 이동 (Table 3, p.8)." |
| 15 | 정정 줄 링크 판본 (v2) | 저장소 PDF는 ICML 판이라 arXiv 판본 표기 없음 | p.1 | 확인 불가 | 판본 비교 불가 (저장소 PDF에 arXiv 번호 없음) |

---

## Measurement-05 Is KD Actually Greener?
- 본 판본: arXiv 2602.09691v2 (p.1 표기)
- 대조 결과: 일치 7건 / 수정 5건 / 추가 2건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 기계번역 KD의 생애주기 환경 비용, 손익분기 추론량 | 맞음 | 초록 p.1; §2.2 p.3 | 일치 | – |
| 2 | student는 고정 비용으로 출발점이 높고 기울기가 작음; 손익분기 아래는 손해 (4.2절, Fig. 4) | 맞음. 조건: **unbatched sentence-level, EN→IS, teacher 단독 서빙 대비**. Fig. 4는 p.5 | Fig. 4 p.5; §4.2 p.6 | 수정 | "문장 단위(배치 없음) EN→IS 추론에서 teacher 대비: student는 절편이 높고 기울기가 작음. (Fig. 4 p.5, §4.2 p.6)" |
| 3 | batch 256에서 2.9억~10.9억 토큰 (Table 1) | 386.97M, 285.07M, 733.37M, 1.09B. "selected Pareto-optimal configurations (EN→IS)". 배치 크기에 단조롭지 않음 ("not perfectly monotonic") | Table 1, p.6 | 일치 | (보강) "EN→IS 선택 Pareto 설정 4개 기준. 배치 크기에 단조 증가하지 않음." |
| 4 | 작은 student(16M)는 손익분기에 빨리 닿지만 품질 상한 낮음 | "The 16M student breaks even earlier … but this comes at a stronger quality ceiling" | §4.2, p.6 | 일치 | – |
| 5 | 옮겨 간 비용 해당 없음; 문장 단위 KD는 teacher가 전체 데이터를 번역 | Seq-KD 부분 맞음. 그러나 원문이 옮겨 감을 명시: offline Word-KD는 "shifts part of the Word-KD cost from training to teacher-side preprocessing and storage". LLM 실험은 top-20 확률을 8-bit로 저장 (NumPy 파일). 저장 용량 수치 없음. 데이터 저장은 시스템 경계 밖 ("data acquisition/storage are excluded") | §2.1 p.2; §2.2 p.3; App. G.2 p.18 | 수정 | "옮겨 간 비용: offline Word-KD는 학습 중 teacher forward를 teacher 전처리·저장으로 옮김 (p.2). LLM 실험은 top-20 확률(8-bit)과 인덱스를 저장 (App. G.2, p.18). 전처리 연산은 생산 비용에 포함, 저장 용량은 보고 안 됨 (저장은 시스템 경계 밖, p.3)." |
| 6 | 단위 kgCO2e·손익분기 토큰; teacher forward Y; 일회성 Y (계수로 구분) | 맞음. 본 실험 λ_T = 1, LLM 실험 λ_T = 0, 각주 1: teacher를 K개 student에 재사용하면 λ_T = 1/K | §2.2 p.3; §4.3 p.6 | 일치 | (보강) "λ_T: 본 실험 1, LLM 실험 0, 재사용 시 1/K (각주 1, p.3)." |
| 7 | 하드웨어 = 확인 안 함 | "All experiments are run on a single NVIDIA V100 GPU"; 단계별 wall-clock·이용률·에너지 기록. 4-GPU 증류는 탄소가 65M에서 약 3배, 16M에서 약 5배 | §3 p.5; Table 3 p.15; App. F p.17 | 수정 | "하드웨어 = NVIDIA V100 1장. 단계별 wall-clock·이용률·에너지 기록 (p.5). 4-GPU 증류는 탄소 약 3배(65M)·5배(16M)로 단일 GPU에 지배됨 (App. F, p.17)." |
| 8 | (추가) 회계 가정값 | PUE 1.24, 전력 배출계수 0.033 kgCO2e/kWh (핀란드 Puhti), GPU 제조 150 kgCO2e, 수명 5년, AUR 학습 0.8·추론 0.2. 에너지 직접 측정이 없으면 평균 전력 × wall-clock | App. D, pp.15–16 | 추가 | "회계 가정: PUE 1.24, 0.033 kgCO2e/kWh, GPU 제조 150 kgCO2e, 수명 5년, 이용률 0.8/0.2. 에너지 ≈ 평균 GPU 전력 × wall-clock. (App. D, pp.15–16)" |
| 9 | 설정: EN→IS 등, Qwen3-8B·4B → 0.6B | 본 실험: TF-Big teacher(205M) → TF-Base 65M·TF-Tiny 16M, EN→IS/HI/ES (23M/13M/6M 문장쌍). LLM: EN→IS/ES, 방향별 10만 문장쌍, student LoRA | §3 p.4; §4.3 p.6; Fig. 4 p.5; App. G pp.17–18 | 수정 | "본 실험: TF-Big(205M) → 65M·16M, EN→IS/HI/ES (문장쌍 23M/13M/6M). LLM: Qwen3-8B·4B → 0.6B, EN→IS/ES 각 10만 쌍, LoRA. (pp.4–6, 17–18)" |
| 10 | 평균 정확도 외: 해당 없음 (번역 품질 지표) | 품질 지표는 FLORES+ devtest의 COMET, paired bootstrap 95% CI. 부분군 지표 없음 | §3 p.4; Fig. 3 p.4 | 일치 | (보강) "COMET(FLORES+ devtest), bootstrap 95% CI." |
| 11 | 위치: 초록 p.1, 4.2절·Table 1 p.6 | Table 1 p.6 맞음. Fig. 4는 p.5, §4.2는 pp.5–6 | p.5, p.6 | 수정 | "위치: 초록 (p.1), Fig. 4 (p.5), §4.2·Table 1 (pp.5–6), App. D (pp.15–16)" |
| 12 | 메모: 손익분기는 서빙 조건에 따라 달라짐 | "Break-even should therefore be estimated under the target serving configuration rather than treated as an intrinsic property" | §4.2, p.6 | 일치 | – |
| 13 | 메모: teacher 비용 계수 = 04번 스위치와 같은 발상 | λ_T ∈ [0,1] | p.3 | 일치 | (보강) "student 학습 재사용 횟수 K(λ_T = 1/K)와 서빙 손익분기 X는 다른 양." |
| 14 | (추가) KD 대 No-KD 비교 | "Since KD and No-KD deploy the same student architecture, they have the same per-token inference footprints and therefore no deployment-volume break-even." 품질 조건 비교만 성립 | §2.2, p.3 | 추가 | "같은 크기의 No-KD와 KD는 추론 기울기가 같아 손익분기가 없음. 비교는 생산 비용 대 품질 이득으로만. (p.3)" |

---

## Measurement-06 Are Coreset Selection Methods Worth Their Cost?
- 본 판본: arXiv 2609.22894v2 (p.1 표기)
- 대조 결과: 일치 8건 / 수정 5건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 데이터셋 4종, 선택 11종, 비율 5단계, seed 3, 1,674회 | 맞음 | 초록, p.1 | 일치 | – |
| 2 | CIFAR-10·Tiny ImageNet 예산 지점 8개씩, 정교한 선택이 이긴 곳 없음 | 맞음. 승자는 Uniform, RS2, Full | 초록 p.1; §4.1 p.5 | 일치 | – |
| 3 | ImageNet-1K 비슷한 비용 비교에서 전체 데이터 단축 학습이 가장 정확 | "among the operating points we probe". 두 탐색 그룹 약 10·18 GPU시간, 단일 seed 탐색 후 일부 3 seed | 초록 p.1; §3 p.4 | 일치 | (보강) "약 10·18 GPU시간 두 그룹의 탐색 지점에 한정." |
| 4 | 선택 비용은 전체 훑기 고정 비용이 지배, K회 재사용 시 실행당 1/K, 무작위보다 싸지지 않음 | 맞음. Herding 4점 적합에서 고정 성분 95.3–99.8%. "extra selection cost per run shrinks as 1/K but never becomes a time saving relative to those baselines" | §4.4 p.7; §4.5·Fig. 5a p.9; App. I p.20 | 일치 | – |
| 5 | 잃는 것: 1 epoch GraNd·EL2N, CIFAR-10 5%에서 EL2N −46.9%p | −46.9%p는 무작위 평균 대비, CIFAR-10 f=0.05 맞음. "epoch-1" 변형 = 독립 초기화한 1-epoch probe 10개의 점수 평균 | §5 p.9; §3 p.4 | 수정 | "epoch-1 GraNd·EL2N(1-epoch probe 10개 평균 점수)은 낮은 비율에서 무작위보다 크게 나쁨. CIFAR-10 f=0.05에서 EL2N이 무작위 평균보다 46.9%p 낮음. (§3 p.4, §5 p.9)" |
| 6 | 옮겨 간 비용: 선택 연산 (전체 데이터 훑기) | 맞음 | §4.4, p.7 | 일치 | – |
| 7 | 단위 = 실제 시간 (통제된 측정 실행으로 추정한 GPU 시간) | 선택은 직접 측정 ("Selection is timed directly"), 학습은 warmup 2 + 측정 10 epoch + test 3회로 추정. f=0.5 보간, f=1 외삽, 직접 전체 데이터 확인과 +0.40~+1.79% 차이 | §3 pp.4–5; App. I pp.20 | 수정 | "선택 시간은 직접 측정, 학습·평가 시간은 probe(warmup 2, 측정 10 epoch, test 3회)로 추정. f=1은 외삽이며 직접 측정과 +0.40~+1.79% 차이. (pp.4–5, App. I p.20)" |
| 8 | 하드웨어 = RTX 1장 (모델명 확인 안 함) | "one idle RTX 4090 host". 정확도 실행은 여러 호스트·GPU 종류 (같은 세대) | §3, p.4 | 수정 | "하드웨어 = 유휴 RTX 4090 1대에서 비용 측정. 정확도 실행은 같은 세대 여러 호스트. (p.4)" |
| 9 | 설정: CIFAR-10, Tiny ImageNet, ImageNet-1K 등 4종, ResNet-18, 200 epoch | 4종 = CIFAR-10, Tiny ImageNet, **CUB-200**, ImageNet-1K (+부록 Retinal OCT). 200 epoch, ImageNet만 90 epoch (정정 줄과 일치). 선택기 학습 40 epoch (ImageNet 10) | Table 1 p.3; §3 p.4 | 수정 | "데이터셋: CIFAR-10, Tiny ImageNet, CUB-200, ImageNet-1K (+Retinal OCT 부록). 하위 학습 200 epoch (ImageNet 90). 선택기 probe 40 epoch (ImageNet 10). (pp.3–4)" |
| 10 | 평균 정확도 외 평가: 없음 | 부분군·최악 클래스 지표는 없음. 대신 recovery(전체 데이터 대비 정규화, Fig. 3), 평균 순위와 bootstrap 구간 (Table 2), Spearman 순위 상관, 무작위 부분집합 30개 중 최악과 비교 (EL2N 14.98% 대 55.52%) | Fig. 3 p.6; Table 2 p.8; App. H p.19 | 수정 | "부분군 지표 없음. recovery (Fig. 3), 평균 순위와 bootstrap 구간 (Table 2), 무작위 30개 중 최악값과의 비교 (p.6, p.19) 보고." |
| 11 | 위치: 초록 p.1, Fig. 5·5절 p.9 | 맞음 | p.1, p.9 | 일치 | – |
| 12 | 정정 줄: ImageNet 90 epoch, 선택 직접·학습 probe, 비교점 한정, 표 4·부록 I | 전부 맞음 (Table 4 p.14, App. I p.20) | p.14, p.20 | 일치 | – |
| 13 | 메모: 코드베이스 오류 9건, Herding 5.9%p | 맞음. CIFAR-10 f=0.1, 3 seed | 초록 p.1; p.3; App. A.2 p.13 | 일치 | (보강) "CIFAR-10 f=0.1 셀 기준." |
| 14 | (추가) 선택 비용 절대값 | 선택 하한: CIFAR-10 약 84–241초 (가장 싼 격자 63초), Tiny ImageNet 약 10–31분. Herding 고정 성분 CIFAR-10 ≈234초, ImageNet ≈19,518초. ImageNet f=0.05에서 선택이 전체 시간의 62–77% | §4.1 p.5; §4.4 p.7; App. I p.20 | 추가 | "선택 하한: CIFAR-10 약 84–241초, Tiny ImageNet 약 10–31분, Herding 고정 성분 ImageNet 약 19,518초. ImageNet f=0.05에서 선택이 전체 시간의 62–77%. (pp.5, 7, 20)" |
| 15 | (추가) 재사용 교차점 | K* = c_sel/(T(1) − T(f)) = 0.07–0.31 (f=0.1, 0.3, 학습된 선택기 4종) → K=1에서 이미 200-epoch 전체 학습보다 쌈, 정확도는 낮음 | §4.5 p.8–9; Table 9 p.18 | 추가 | "비용 교차점 K* = 0.07–0.31로 K=1에서도 전체 데이터 200-epoch 학습보다 싸지만 정확도는 낮음. 교차는 비용에서만. (pp.8–9, Table 9)" |
| 16 | (추가) 선택기 학습의 상대 크기 | "the 40 selector probe epochs alone are 20% of the canonical 200-epoch full-data training count" | §4.4, p.7 | 추가 | "선택기 probe 40 epoch만으로 200-epoch 전체 학습의 20% (p.7)." |

---

## Measurement-07 Canonical KD for Semantic Segmentation
- 본 판본: arXiv 2604.25530v2 (p.1 표기)
- 대조 결과: 일치 10건 / 수정 2건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 반복 수가 아니라 실제 연산 시간으로 비교하면 순위가 바뀜 | 맞음 | 초록 p.1 | 일치 | – |
| 2 | 40k 반복: CIRKD 73.5 mIoU·30.2 GPU시간, 특징 KD 73.7·9.2 GPU시간 | 맞음. CIRKD 공식 코드베이스, DeepLabV3-R101 teacher (정정 줄과 일치) | §4.2 p.3; §3.2 p.2 | 일치 | – |
| 3 | 30 GPU시간: CIRKD 73.5, logit 76.4, 특징 75.4 | 맞음. 같은 그림에 지도학습 73.3 | Fig. 1, p.3 | 일치 | (보강) "지도학습 73.3." |
| 4 | 장기 학습: R18이 R101의 99% (79.0 대 79.8) | 맞음. Table 2: 1200 epoch, 55 GPU시간. ADE20K는 41.1 대 44.4 (92%), 157 GPU시간. Fig. 2 teacher 표기는 79.74 | 초록 p.1; Table 2 p.3 | 일치 | (보강) "55 GPU시간 (Table 2, p.3)." |
| 5 | 잃는 것: 표 1 방법 간 비교는 기준선이 달라 직접 비교 불가 | "As the baselines vary across codebases, the results are not directly comparable." | Table 1, p.3 | 일치 | – |
| 6 | 옮겨 간 비용: 해당 없음 | 맞음 | – | 일치 | – |
| 7 | teacher forward Y (학습 실제 시간에 포함), 일회성 N | GPU 시간은 학습 실행 시간이라 teacher forward 포함은 추론, 분리 측정 없음. teacher 준비 비용은 결론에서 "the benefit of KD must be weighed against the cost of training or acquiring a suitable teacher"라고만 적음 | §4.2 p.3; §5 p.4 | 일치 | (보강) "teacher forward 포함은 추론. teacher 학습 비용은 세지 않았다고 결론에서 언급 (p.4)." |
| 8 | 하드웨어 = 확인 안 함 | 장기 학습 그림 축: "Compute Budget (GPU Hours, 2×A100)". 짧은 비교(40k·30h)의 장비는 명시 없음 | Fig. 2 p.3; Fig. 3 p.4 | 수정 | "하드웨어: 장기 학습은 A100 2장 (Fig. 2–3). 짧은 비교(40k, 30 GPU시간)의 장비는 명시 안 됨." |
| 9 | 설정: PSPNet R101 → R18, 40k~160k 반복, 최대 300 GPU시간 | 정정 줄대로 짧은 비교는 DeepLabV3-R101 teacher. Table 1은 80k(Cityscapes)·160k(ADE20K). 장기 300 GPU시간, 반지도 확장 320 GPU시간 | §3.2 p.2; §4.3 p.3; §4.4 p.4 | 일치 | (정정 줄 그대로 유효) |
| 10 | 평균 정확도 외: 해당 없음 (mIoU) | 클래스별·부분군 지표 없음 | – | 일치 | – |
| 11 | 위치: 초록 p.1, Table 1·Fig. 1·4.2절 p.3 | 맞음. Table 2·Fig. 2(p.3), Fig. 3·§4.4(p.4), 프로토콜 §3.2(p.2) 위치가 빠짐 | pp.2–4 | 수정 | "위치: 초록 (p.1), §3.2 (p.2), Table 1·2, Fig. 1·2, §4.2–4.3 (p.3), Fig. 3·§4.4 (p.4)" |
| 12 | 정정 줄: 짧은·장기 프로토콜 구분, DeepLabV3-R101, 2×A100, 320 GPU시간 | 전부 원문과 맞음 | pp.2–4 | 일치 | – |
| 13 | (추가) 스텝당 비용 차이 | "can incur up to 3× higher per-iteration cost". 40k에서 CIRKD 30.2 대 특징 KD 9.2 GPU시간 = 3.3배 | §1 p.1; §4.2 p.3 | 추가 | "복잡한 분할 KD는 반복당 비용이 최대 3배 (p.1). 40k 반복에서 CIRKD 30.2 대 특징 KD 9.2 GPU시간 (p.3)." |
| 14 | (추가) teacher 크기와 비용 | Mask2Former(Swin) teacher 83.7%는 student 77.8 대 77.7%로 거의 같고 "at 3× the compute (296 vs. 89 GPU hours)" | §4.4, p.4 | 추가 | "큰 teacher(Mask2Former, 83.7%)는 student를 77.7→77.8%로만 올리고 학습 시간은 89→296 GPU시간 (3배). teacher forward 비용이 총비용을 좌우하는 사례. (p.4)" |
| 15 | (추가) 장기 학습 방법별 GPU 시간 | Cityscapes: 지도학습 76.3·67h, CIRKD 77.0·44h, logit KD 77.7·89h, Pure KD 78.1·160h, 특징 KD 79.0·55h | Table 2, p.3 | 추가 | "Cityscapes 최고점 도달 GPU시간: 지도 67, CIRKD 44, logit 89, Pure KD 160, 특징 KD 55h (Table 2, p.3)." |

---

## 범위 밖 참고: 「정리 메모」 (01–07에 의존하는 줄)

| 줄 | 메모의 서술 | 판정 | 고칠 문구 |
|---|---|---|---|
| 134 | 01·02·03 "세 논문 모두 증류는 길게 학습할수록 계속 오른다" | 수정 | "긴 학습은 FunMatch(01)와 vanilla KD(02)에서 계속 이득. DeiT(03)는 hard·token 증류만 이득이고 800 epoch 이후 평탄, soft 증류는 이득 없음 (Fig. 3, p.13)." |
| 135 | 비전 분류 KD 학습 비용을 teacher forward까지 실측한 것은 02뿐 | 일치 (단서 필요) | "02의 GPU 시간은 teacher forward를 포함한 student 학습 총시간이며 teacher forward를 따로 재지는 않음. 분할은 07이 같은 방식으로 보고." |

---

## 전체 요약

1. **가장 심각한 오류: 하드웨어 "확인 안 함"이 4편(04·05·06·07)에서 틀림.** 05는 V100 1장, 06은 RTX 4090 1대, 07은 장기 학습에 A100 2장이 원문에 명시됨. 04는 장비가 정말 없음(AXLearn만). 측정 방식을 다루는 폴더인데 측정 장비 칸이 비어 있었음.
2. **"옮겨 간 비용: 해당 없음" 판정 2건이 틀림.** 04 부록 G.4에 전체 logit 저장 용량 추정(토큰당 약 129KB, C4 전체 약 260 PB)이 숫자로 있음. 05는 offline Word-KD가 비용을 teacher 전처리·저장으로 옮긴다고 명시함(저장 용량 수치는 없음, 저장은 시스템 경계 밖). Reuse 축의 직접 근거인데 빠져 있었음.
3. **조건 누락 패턴:** 수치 자체는 거의 다 맞음. 빠진 것은 조건임. 01 Table 1의 T224→S224 행, 02 전략 A2와 두 teacher가 공유하는 GPU 시간 열, 03의 late-fusion head(74.5 대 74.6), 06의 epoch-1 = probe 10개 평균. 01의 "작은 데이터셋 모두 최대 100만 epoch"는 Flowers만 해당되는 사실 오류.
4. **과일반화 패턴:** 03 Fig. 3의 "증류는 계속 오른다"(soft 증류는 오르지 않고 hard·token도 800 epoch 이후 평탄), 02의 "KD 손실 연산은 무시할 만"(CE 단독 시간이 없어 산출 불가), 01 Shampoo 비교 대상(consistent teaching이지 FunMatch가 아님).
5. **빠진 핵심 비용 수치:** 03의 teacher 추론 처리량(334.7 img/s로 DeiT-B student보다 빠름)과 teacher 학습이 20% 더 느리다는 서술, 07의 Mask2Former teacher 3배 비용(296 대 89 GPU시간), 02 Table 10의 사전학습 포함 누적 GPU 시간, 06의 선택 비용 절대값(ImageNet 약 19,518초, f=0.05에서 전체의 62–77%). 기존 정정 줄 3개(04·06·07)는 원문과 모두 맞음.
