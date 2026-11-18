# 4_Substitute 메모

Substitute: teacher나 데이터를 싼 대역으로  
논문 목록과 PDF: [`../Related Work/Efficient KD papers/4_Substitute/`](<../Related Work/Efficient KD papers/4_Substitute>)  
작성 규칙과 항목 설명: [README](README.md)

담당: Claude 초안 (2026-11-18). 저장소 PDF 본문을 직접 읽고 채움. 사람이 다시 대조하지 않음.

## 읽기 전에

- 수치는 저장소 PDF의 표·본문에서 옮겼고, 「위치」의 쪽 번호는 그 PDF의 쪽(첫 장 = 1)임.
- 「잃는 것」 표시는 `1_Shrink.md`와 같음. **[실측·원논문]** = 논문이 보고한 관찰, **[구조]** = 방법 정의에서 나오는 제약.
- "보고 안 됨" = 논문에 해당 값이 없음. "확인 안 함" = 논문에 있을 수 있으나 이번에 읽지 않은 부분.
- 이 축은 바꾸는 대상이 둘임. 01–07번은 **teacher**를 싼 것으로 바꿈 (덜 학습한 teacher, 작은 teacher, teacher 없음, 자기 자신, 동료). 08–12번은 **데이터**를 바꿈 (대리 데이터, 합성·응축 데이터, 데이터 없음).

### 01. Efficient KD from Model Checkpoints ([arXiv: 2210.06458](https://arxiv.org/abs/2210.06458))
- 효율화 방식: 끝까지 학습한 teacher 대신 학습 도중의 체크포인트를 teacher로 씀. 여러 teacher의 앙상블 대신 한 학습 궤적의 체크포인트들을 묶은 스냅샷 앙상블을 씀. 가장 좋은 체크포인트를 고르는 선택 알고리즘(입력·출력과의 상호정보 합이 최대인 지점)도 제안.
- 얻는 것: teacher 학습 비용이 절반인 중간 지점 teacher로도 증류 성능이 같거나 높음 (끝까지 학습한 teacher 대비).
  - ImageNet (teacher 120 epoch 중 60 epoch 지점): ResNet-34 → ResNet-18 70.66 → 70.86%, ResNet-50 → MobileNetV2 66.64 → 66.79%.
  - CIFAR-100 (200 epoch 중 100 지점): WRN-40-2 → WRN-40-1 72.68 → 72.76%, WRN-40-2 → MobileNetV2 68.03 → 68.99%. (Table 1)
- 잃는 것:
  - [실측·원논문] 너무 이른 체크포인트는 떨어짐: 30% 지점에서 ResNet-110 → ResNet-32 70.74% vs 끝까지 학습 72.48%. (Table 3)
  - [구조] 가장 좋은 지점은 teacher마다 다르고 (WRN-40-2는 160 epoch, ResNet-110은 120 epoch), 선택 알고리즘은 student 구조를 고려하지 않음. (5절, 한계)
- 옮겨 간 비용:
  - 선택 알고리즘은 체크포인트마다 복원용 디코더를 학습해 상호정보를 추정하므로 추가 연산이 듦. 논문도 선택한 지점이 중간 지점보다 비용이 높다고 적음 (부록 A.2). 숫자는 그림으로만 제시.
  - teacher–student 쌍과 체크포인트마다 온도와 손실 비율을 격자 탐색으로 다시 찾음 (부록 Table 4). 이 탐색 비용은 보고 안 됨.
- 어떻게 쟀나: 단위 = teacher 학습 epoch 비율, 총 학습 시간(teacher 학습 + student 증류, TITAN Xp 기준 추정 시간, Fig. 3과 Fig. 8의 가로축) / teacher forward 셈? 총 학습 시간에 포함된 것으로 보이나 내역 없음 / 일회성 비용 셈? teacher 학습 Y (그림에서) / 하드웨어 = TITAN Xp
- 설정: CIFAR-100 WRN-40-2·ResNet-110 → WRN-40-1·ResNet-32·MobileNetV2, teacher 200 epoch. ImageNet ResNet-34 → ResNet-18, ResNet-50 → MobileNetV2, teacher 120 epoch. Tiny-ImageNet. 5회 평균.
- 평균 정확도 외 평가: 없음 (정보 평면 분석은 있음)
- 위치: Table 1·2 (p. 5), Fig. 3 (p. 6), Table 3·Algorithm 1 (p. 10), 부록 Table 4·5 (p. 15), Fig. 8 (p. 16)
- 메모:
  - teacher 준비 비용을 총비용에 넣어 비교한 드문 사례. 다만 수치가 그림으로만 있음.
  - 체크포인트마다 온도·손실 비율을 다시 튜닝했다는 점은 "같은 튜닝 규칙" 원칙과 다름. 우리 실험에서는 고정 값으로 돌리므로 결과가 다를 수 있음.
  - 절감되는 것은 일회성인 teacher 학습 비용이고, 증류 중 teacher forward 비용은 그대로임.

### 02. On the Efficacy of KD ([arXiv: 1910.01348](https://arxiv.org/abs/1910.01348))
- 효율화 방식: 본래 목적은 효율이 아니라 "큰 teacher가 좋은 teacher가 아닌 이유" 분석. 그 해결책 두 가지가 비용과 관련됨. (1) ESKD: student 학습 초반에만 KD를 하고 이후는 CE만으로 학습. (2) 일찍 멈춘 teacher: teacher를 전체 epoch의 1/3~1/4만 학습.
- 얻는 것: 비용 절감 수치는 보고 안 됨. 보고한 것은 오류율 (ImageNet, ResNet-18 student, top-1 오류):
  - 처음부터 학습 30.24%. 전체 KD는 teacher ResNet-18/34/50에서 30.57 / 30.79 / 30.95%로 오히려 나쁨.
  - ESKD는 29.01 / 29.16 / 29.35%. (Table 3)
  - 일찍 멈춘 teacher: ResNet-34 (50 epoch) 29.02%, ResNet-50 (35 epoch) 29.05%. (Table 8)
- 잃는 것:
  - [실측·원논문] ESKD를 써도 teacher가 클수록 student가 나빠지는 경향은 남음 (ResNet-152 teacher 29.45%). (Table 8)
  - [실측·원논문] 일찍 멈춘 teacher 자체의 정확도는 낮음 (ResNet-50 23.85 → 27.01% 오류, 부록 Table 14).
- 옮겨 간 비용: 없음. 두 방법 모두 연산을 줄이는 쪽 (KD 구간 단축, teacher 학습 단축).
- 어떻게 쟀나: 단위 = teacher 학습 epoch 수 (표의 괄호), 비용 측정 없음 / teacher forward 셈? N / 일회성 비용 셈? N / 하드웨어 = 보고 안 됨
- 설정: CIFAR-10 WRN·DenseNet 여러 쌍 (200 epoch, teacher는 35/50/65/80/200 epoch). ImageNet ResNet-18/34/50/152 → ResNet-18. 손실 α = 0.9, 온도 4.
- 평균 정확도 외 평가: Places365로의 전이 미세조정 (Table 9). 그 외 없음.
- 위치: Table 1–3·Fig. 3 (p. 4–5), Table 8 (p. 6), Fig. 4·Table 9·10 (p. 7), 부록 Table 14 (p. 10)
- 메모:
  - ESKD는 Skip(구간)의 원형, 일찍 멈춘 teacher는 Substitute(싼 teacher)의 원형. 한 논문이 두 축에 걸침.
  - 논문은 KD를 언제 끄는지의 정확한 epoch을 본문 표에 적지 않음 (확인 안 함). 우리 실험의 "앞 구간 비율" 손잡이와 맞추려면 공식 코드나 부록 확인 필요.
  - "KD가 후반에 정확도를 해친다"(Fig. 3)는 관찰은 ImageNet·ResNet 조건에 한정.

### 03. Learning from a Lightweight Teacher for Efficient KD ([arXiv: 2005.09163](https://arxiv.org/abs/2005.09163))
- 효율화 방식: 큰 teacher를 목표 데이터에서 학습하는 대신, 아주 작은 teacher(LeNet5)를 쉬운 합성 데이터(SynMNIST)에서 학습해 그 soft target을 씀. student 출력 분포를 teacher 출력 분포에 맞추는 적대적 손실을 더함.
- 얻는 것 (teacher의 크기와 1개 입력당 연산, 일반 KD의 teacher 대비): CIFAR-10에서 LeNet5 teacher 약 6.2만 매개변수·48만 FLOPs vs ResNet-18 teacher 1,120만 매개변수·3,720만 FLOPs. 약 두 자릿수 차이. (Table 8)
- 잃는 것: [실측·원논문] 일반 KD와 비슷하거나 약간 낮음: CIFAR-10 ResNet-20 student, KD(teacher ResNet-18) 92.98% vs 제안 92.94%. MobileNetV2는 91.20 vs 91.57%, ShuffleNetV2는 92.17 vs 92.48%. (Table 3)
- 옮겨 간 비용: 합성 데이터 생성, 판별기 학습(약 4,900 매개변수). Table 8에 판별기 크기와 FLOPs를 보고.
- 어떻게 쟀나: 단위 = 모델 매개변수 수, 입력 1개당 FLOPs. 학습 시간은 보고 안 됨 / teacher forward 셈? Y (FLOPs로) / 일회성 비용 셈? teacher 학습 시간 N (크기 비교만) / 하드웨어 = GTX 2080 Ti
- 설정: CIFAR-10/100 student ResNet-20·MobileNetV2·ShuffleNetV2. 텍스트 분류와 UCF101 동작 인식도 실험.
- 평균 정확도 외 평가: 없음
- 위치: Table 3 (p. 6), Table 7·Fig. 4 (p. 8), Table 8 (p. 9)
- 메모: 비용 지표가 teacher의 크기와 추론 FLOPs뿐이라 "teacher 학습이 얼마나 빨라졌나"는 직접 재지 않음. teacher가 목표 과제를 풀지 못하는데도(다른 데이터로 학습) 이득이 난다는 점에서 label smoothing 계열로 읽는 것이 맞음.

### 04. Tf-KD (Revisiting KD via Label Smoothing Regularization) ([arXiv: 1909.11723](https://arxiv.org/abs/1909.11723))
- 효율화 방식: 더 큰 teacher 없이 KD 효과를 얻음. 두 가지 변형. Tf-KD_self: student 자신을 먼저 학습해 그 모델을 teacher로 씀. Tf-KD_reg: 정답 클래스에 0.99를 준 수작업 분포를 "가상 teacher"로 씀 (모델이 필요 없음).
- 얻는 것: 비용 수치는 보고 안 됨. Tf-KD_reg는 추가 연산이 없다고 주장. 정확도 향상 (기준 학습 대비):
  - CIFAR-100 MobileNetV2: Tf-KD_self +2.58%p, Tf-KD_reg +2.50%p, 일반 KD(teacher ResNet-18) +2.67%p. (Table 5·9)
  - ImageNet ResNet-50: Tf-KD_reg +0.65%p, label smoothing +0.51%p. (Table 11)
- 잃는 것: [실측·원논문] 일반 KD보다 낮은 경우가 있음: Tiny-ImageNet ResNet-50에서 Tf-KD_reg +0.45%p vs 일반 KD +0.68%p. ImageNet에서 label smoothing과의 차이는 0.1~0.4%p로 작음. (Table 10·11)
- 옮겨 간 비용: Tf-KD_self는 student를 한 번 더 학습해야 함 (자기 자신을 teacher로 만들기 위한 선행 학습)과 학습 중 그 모델의 forward. 숫자 보고 안 됨. Tf-KD_reg는 없음.
- 어떻게 쟀나: 단위 = 없음 (정확도만) / teacher forward 셈? N / 일회성 비용 셈? N / 하드웨어 = 보고 안 됨
- 설정: CIFAR-100, Tiny-ImageNet (200 epoch), ImageNet. MobileNetV2, ShuffleNetV2, ResNet-18/50, DenseNet-121, ResNeXt 등. 3회 평균 (일부).
- 평균 정확도 외 평가: 없음
- 위치: Table 1–4 (p. 4), Table 5–8 (p. 7), Table 9–11 (p. 8)
- 메모:
  - Tf-KD_reg는 teacher 비용이 0인 기준점. "teacher 연산을 0으로 줄였을 때 남는 이득"을 보여 주는 대조군으로 쓸 수 있음.
  - 두 변형의 준비 비용이 전혀 다름 (self는 선행 학습 1회, reg는 0). 한 줄로 묶어 "teacher 없음"이라고 쓰면 안 됨.

### 05. Be Your Own Teacher ([arXiv: 1905.08094](https://arxiv.org/abs/1905.08094))
- 효율화 방식: 한 네트워크 안에서 가장 깊은 분류기가 얕은 구간에 붙인 보조 분류기들을 가르침 (자기 증류). 따로 teacher를 설계하고 학습하는 단계가 없음.
- 얻는 것 (teacher 학습 + 증류의 2단계 방식 대비): CIFAR-100에서 학습 시간 26.98시간 → 5.87시간 (4.6배 단축), 정확도 79.33 → 81.04% (ResNet-50). (Fig. 1, 1절)
- 잃는 것:
  - [구조] 학습 중 보조 분류기와 병목 층이 추가됨. 추론 때는 제거 가능.
  - [실측·원논문] 얕은 출구로 예측하면 빠르지만 정확도가 낮음: ResNet-50, 1/4 지점 출구 4.64배 가속에 67.87% (기준 77.68%). (Table 5)
- 옮겨 간 비용: 보조 분류기의 학습 연산. 숫자 보고 안 됨 (총 학습 시간에는 포함된 것으로 보임).
- 어떻게 쟀나: 단위 = 실제 학습 시간(시간), 추론 가속 배수 / teacher forward 셈? Y (비교 대상의 시간에 teacher 학습과 증류가 모두 포함) / 일회성 비용 셈? Y (teacher 학습 시간을 비교 대상에 포함) / 하드웨어 = 보고 안 됨
- 설정: CIFAR-100, ImageNet. ResNet-18/50/101/152, ResNeXt, WideResNet, VGG.
- 평균 정확도 외 평가: 잡음 섞은 가중치에서의 정확도와 손실 (평탄한 최소점 분석, Fig. 4). 그 외 없음.
- 위치: Fig. 1 (p. 2), Table 1·2 (p. 4), Table 3·4 (p. 5), Table 5 (p. 6)
- 메모: teacher 학습 시간까지 포함해 실제 시간으로 비교한 초기 사례. 다만 하드웨어와 측정 조건이 없어 26.98시간의 내역(teacher 학습 대 증류)은 알 수 없음.

### 06. Deep Mutual Learning ([arXiv: 1706.00384](https://arxiv.org/abs/1706.00384))
- 효율화 방식: 사전학습된 teacher 없이 여러 student가 동시에 학습하면서 서로의 예측 분포를 맞춤 (KL 손실).
- 얻는 것: 비용 수치는 보고 안 됨. 보고한 것은 정확도: 혼자 학습하는 것보다 높고, 사전학습 teacher로 증류하는 것보다도 높다고 보고 (CIFAR-100, Market-1501; Table 2·4).
- 잃는 것: [구조] 학습 중 네트워크 K개를 동시에 돌려야 함. student 하나 기준으로는 학습 연산과 메모리가 K배.
- 옮겨 간 비용: teacher 사전학습 비용이 "동료 네트워크의 동시 학습" 비용으로 바뀜. 숫자 보고 안 됨.
- 어떻게 쟀나: 단위 = 없음 (정확도, mAP) / teacher forward 셈? N / 일회성 비용 셈? N / 하드웨어 = GTX 1080 1장
- 설정: CIFAR-100 (200 epoch, batch 64), ResNet-32, MobileNet, InceptionV1, WRN-28-10. Market-1501 재식별 (100,000 반복).
- 평균 정확도 외 평가: 재식별 mAP와 Rank-1 (다른 과제). 그 외 없음.
- 위치: Table 1·2 (p. 5), Table 3·4 (p. 6), Fig. 2 (p. 7)
- 메모: "teacher가 필요 없다"와 "비용이 적다"는 다른 말임을 보여 주는 사례. 모델 하나를 얻는 비용과 동료 전체의 비용을 구분해 적어야 함.

### 07. Snapshot Distillation ([arXiv: 1812.00123](https://arxiv.org/abs/1812.00123))
- 효율화 방식: 순환 학습률로 한 번의 학습을 여러 주기로 나누고, 각 주기의 마지막 스냅샷이 다음 주기의 teacher가 됨. 여러 세대를 차례로 학습하는 방식을 한 세대 안에서 해결.
- 얻는 것: 기준 학습 대비 추가 학습 시간 약 1/3. K세대 방식은 K배의 학습 시간이 든다고 대비. (1절, 결론)
- 잃는 것: [실측·원논문] 표에서는 손실 없음. ImageNet top-1 오류 ResNet-101 21.62 → 21.25%, ResNet-152 21.17 → 20.93%. (Table 4) 이득 폭은 0.3%p 안팎으로 작음.
- 옮겨 간 비용: 이전 스냅샷의 forward (학습 중 매 스텝). 이것이 추가 시간 1/3의 원인.
- 어떻게 쟀나: 단위 = 기준 대비 추가 학습 시간의 비율 (서술로만, 측정 표 없음) / teacher forward 셈? Y (추가 시간 1/3이 그것) / 일회성 비용 셈? 해당 없음 (별도 teacher 학습이 없음) / 하드웨어 = 보고 안 됨
- 설정: CIFAR-100, ImageNet (ResNet-101·152, 90 epoch). 순환 주기 2개.
- 평균 정확도 외 평가: PascalVOC 검출과 분할로의 전이 (Table 5). 그 외 없음.
- 위치: Table 1 (p. 1), 1절 (p. 2), Table 3 (p. 6), Table 4·5·Fig. 1 (p. 8)
- 메모: teacher 준비 비용은 0이지만 teacher forward 비용은 남는다는 점을 스스로 밝힌 사례. 03–06번과 달리 추가 비용의 크기(1/3)를 적음.

### 08. Proteus (Accessing Vision Foundation Models via ImageNet-1K) ([arXiv: 2407.10366](https://arxiv.org/abs/2407.10366))
- 효율화 방식: 파운데이션 모델(DINOv2 등)을 그 모델의 원래 학습 데이터(비공개 1억 4,200만 장) 대신 ImageNet-1K(120만 장)에서 증류함. 데이터셋 편향을 줄이려고 토큰·패치·특징 수준의 목표를 함께 씀.
- 얻는 것 (원래 데이터로 증류한 공식 DINOv2 대비): 데이터 1억 4,200만 장 → 120만 장으로 비슷한 성능이라고 주장 (Fig. 1). 학습 시간은 DeiT 증류와 같음: ViT-S, ImageNet-1K 300 epoch, A100 8장에서 둘 다 30시간. (부록 A.8)
- 잃는 것: [실측·원논문] 공식 DINOv2보다 ImageNet-A에서 뒤짐. ImageNet-Sketch·R은 비슷하고 ImageNet-C는 앞섬. (부록 Table 14)
- 옮겨 간 비용: 없음. teacher(파운데이션 모델)의 사전학습 비용은 이미 치른 것으로 전제.
- 어떻게 쟀나: 단위 = 학습 데이터 수, 학습 시간(시간) / teacher forward 셈? Y (30시간에 포함) / 일회성 비용 셈? N / 하드웨어 = A100 8장
- 설정: DINOv2-S/B/L/g → ViT-Ti/S/B/L (patch 14), ImageNet-1K 300 epoch, batch 1024. CLIP, SynCLR teacher도 실험.
- 평균 정확도 외 평가: 강건성 (ImageNet-Sketch, A, R, C), 세부 분류 12종 평균, 분할(ADE20K), 깊이 추정(NYUd) (Table 3). DeiT 증류 대비 모든 항목에서 높음 (예: ViT-B에서 ImageNet-A 29.6 → 52.4%).
- 위치: Fig. 1·2 (p. 2), Table 1·2 (p. 5), Table 3 (p. 6), Table 4–9 (p. 7–8), 부록 Table 14 (p. 16), 부록 A.8 (p. 18)
- 메모: 데이터를 100분의 1로 줄였지만 student 학습 시간 자체는 일반 증류와 같음. 절감되는 것은 "데이터 접근"이지 연산이 아님. 공정한 비교에는 teacher가 본 데이터와 student가 본 데이터를 따로 적어야 함.

### 09. SRe2L (Squeeze, Recover and Relabel) ([arXiv: 2306.13092](https://arxiv.org/abs/2306.13092))
- 효율화 방식: 데이터셋 응축. (1) 원본 데이터로 모델을 학습해 정보를 모델에 압축, (2) 그 모델의 BN 통계에 맞도록 합성 이미지를 복원, (3) 합성 이미지에 teacher로 soft label을 다시 붙임. 합성 단계에 원본 데이터가 필요 없어 메모리와 시간이 줄어듦.
- 얻는 것 (기존 응축 방법 대비 합성 비용, Tiny-ImageNet, RTX 4090 1장): 이미지 1장·반복 1회당 시간 MTT 12.64 ms → 0.24 ms (ConvNet-4), 0.75 ms (ResNet-18). 최대 GPU 메모리 48.9 → 4.2 GB, 7.6 GB. (Table 1)
- 잃는 것: [실측·원논문] 전체 데이터 학습과는 큰 차이가 남음: ImageNet-1K 클래스당 50장에서 최고 60.8% (전체 데이터 ResNet-18 69.76%, ResNet-50 76.13%). (1절, 2절)
- 옮겨 간 비용: 압축 단계의 모델 학습(공개 사전학습 모델을 쓰면 0), 합성 반복, 재라벨링용 soft label 사전 생성과 저장 (FKD 방식). soft label 저장 용량은 이 논문에 숫자가 없음. 다른 논문의 측정: 응축 데이터의 30배 이상 (3_Reuse 09번).
- 어떻게 쟀나: 단위 = 합성 시간(이미지·반복당 ms), 최대 GPU 메모리(GB) / teacher forward 셈? N (재라벨링 시간 없음) / 일회성 비용 셈? 합성 비용 Y, 압축 단계 학습과 재라벨링 N / 하드웨어 = RTX 4090 1장
- 설정: Tiny-ImageNet, ImageNet-1K (224 해상도). 복원 모델 ResNet-18/50, 평가 student ResNet-18/50/101 등.
- 평균 정확도 외 평가: 다른 구조로의 일반화 (Table 5), 지속 학습 (Fig. 6). 그 외 없음.
- 위치: Fig. 1 (p. 1), Table 1 (p. 3), Table 4·5 (p. 9), 부록 Table 7 (p. 15)
- 메모: "데이터를 대체해 student 학습을 싸게 한다"는 쪽이지만 응축 과정 자체가 teacher에 크게 의존함 (BN 통계, soft label). 응축 데이터로 학습할 때의 student 학습 비용과 응축을 만드는 비용은 서로 다른 단계라 따로 적어야 함.

### 10. RDED ([arXiv: 2312.03526](https://arxiv.org/abs/2312.03526))
- 효율화 방식: 최적화 없이 응축 데이터를 만듦. 실제 이미지에서 teacher 점수가 높은 대표 패치를 골라 여러 개를 한 장으로 이어 붙이고, 영역별 soft label을 붙임.
- 얻는 것 (SRe2L 대비, ImageNet-1K, RTX 4090 1장):
  - 클래스당 10장 응축: 7분에 ResNet-18 42.0% vs 6시간에 21.3%. (초록, Table 2)
  - 이미지 1장당 합성 시간 MobileNet-V2 3783.16 → 64.97 ms, 최대 GPU 메모리 12.93 → 2.35 GB. (Table 3)
- 잃는 것: [실측·원논문] 전체 데이터 학습과의 차이는 여전히 큼: ImageNet-1K 클래스당 50장 ResNet-18 56.5%, ResNet-101 61.2%. 저해상도·소규모 데이터셋에서는 기존 방법보다 낮은 경우가 있음 (CIFAR-10 클래스당 1장 23.5% vs MTT 46.3%). (Table 2)
- 옮겨 간 비용: 패치 점수 계산용 teacher forward, 영역별 soft label 생성과 저장 (FKD 방식, 부록). 저장 용량은 보고 안 됨.
- 어떻게 쟀나: 단위 = 합성 시간(분, 이미지당 ms), 최대 GPU 메모리(GB) / teacher forward 셈? 합성 시간에 포함 / 일회성 비용 셈? 합성 Y, soft label 저장 N / 하드웨어 = RTX 4090 1장
- 설정: CIFAR-10/100, ImageNette, ImageWoof, Tiny-ImageNet, ImageNet-100, ImageNet-1K. teacher와 평가 모델은 ConvNet, ResNet-18, ResNet-101 등.
- 평균 정확도 외 평가: 다른 구조로의 일반화 (Table 4). 그 외 없음.
- 위치: 초록 (p. 1), Table 1 (p. 3), Table 2·3 (p. 7), Table 4 (p. 8), 부록 (p. 13)
- 메모: 09번과 마찬가지로 soft label 저장이 숨은 비용. 실제 이미지 조각을 쓰므로 "합성 데이터"보다는 "teacher가 고른 부분집합"에 가까움 (Skip-데이터와 맞닿음).

### 11. Up to 100x Faster Data-free KD (FastDFKD) ([arXiv: 2112.06253](https://arxiv.org/abs/2112.06253))
- 효율화 방식: 원본 데이터 없이 teacher를 역으로 풀어 합성 이미지를 만드는 증류에서, 합성 단계를 빠르게 함. 여러 샘플이 공유하는 공통 특징을 메타 학습으로 재사용해 샘플당 최적화 스텝을 수 회로 줄임.
- 얻는 것 (합성에 든 GPU 시간, 기존 데이터 없는 증류 대비):
  - CIFAR-100 ResNet-34 → ResNet-18: DeepInv 42.1시간 (61.32%), CMI 19.2시간 (77.04%), 제안 10스텝 0.27시간 (74.34%).
  - ImageNet ResNet-50 → ResNet-50: DeepInv 166시간 (68.00%) → 6.28시간 (68.61%). (Table 1·2)
- 잃는 것:
  - [실측·원논문] 스텝을 줄일수록 정확도가 떨어짐: CIFAR-100 2스텝 69.76%, 5스텝 72.82%, 10스텝 74.34%. 가장 정확한 기존 방법 CMI(77.04%)보다는 낮음. (Table 1)
  - [실측·원논문] 원본 데이터로 학습한 것과 차이: ImageNet 68.61% vs 75.45%. (Table 2)
- 옮겨 간 비용: 합성 자체가 데이터의 대체 비용. 생성기와 메타 학습의 연산은 보고한 GPU 시간에 포함.
- 어떻게 쟀나: 단위 = 합성의 GPU 시간, 기준 방법 대비 배수. student 학습 시간은 따로 보고 안 됨 / teacher forward 셈? 합성 시간에 포함 / 일회성 비용 셈? 합성 Y, teacher 학습 N / 하드웨어 = GPU 1장 기준 추정 (종류 보고 안 됨)
- 설정: CIFAR-10/100 여러 쌍, ImageNet ResNet-50 → ResNet-50·ResNet-18·MobileNetV2, NYUv2 분할.
- 평균 정확도 외 평가: 없음 (분할 mIoU는 다른 과제)
- 위치: Fig. 1 (p. 1), Table 1·2 (p. 6), Table 3–5 (p. 7)
- 메모: "100배"의 분모는 일반 KD가 아니라 가장 느린 데이터 없는 증류의 합성 시간임. 가속 대상이 합성 단계뿐이고 증류 전체가 아니라는 점을 적어야 함.

### 12. NAYER ([arXiv: 2310.00258](https://arxiv.org/abs/2310.00258))
- 효율화 방식: 데이터 없는 증류의 합성 단계에서, 무작위 잡음을 입력으로 쓰는 대신 클래스 이름의 텍스트 임베딩을 입력으로 쓰고 잡음은 생성기 안의 한 층으로 옮김. 생성기가 빨리 수렴해 전체 학습 시간이 줄어듦.
- 얻는 것 (전체 학습 시간, A100 1장, WRN-40-2 → WRN-16-2):
  - CIFAR-100: DeepInv 31.23시간 (61.34%), CMI 24.01시간 (68.75%), FM 2.23시간 (67.15%), 제안 2.15시간 (70.20%).
  - DeepInv 대비 평균 4.5~14.9배 (학습 길이 설정에 따라). 초록에서는 기존 방법보다 5~15배 빠르다고 적음. (Table 2)
- 잃는 것: [실측·원논문] 원본 데이터로 학습한 student보다 낮음: CIFAR-100 WRN-16-2 73.56% vs 71.72% (300 설정). ImageNet ResNet-50 68.92% vs teacher 75.45%. (Table 1)
- 옮겨 간 비용: 텍스트 인코더로 클래스 임베딩을 만드는 일회성 계산, 생성기 학습. 학습 시간에 포함해 보고.
- 어떻게 쟀나: 단위 = 전체 학습 시간(시간), 기준 방법 대비 배수 / teacher forward 셈? Y (전체 시간에 포함) / 일회성 비용 셈? teacher 학습 N / 하드웨어 = NVIDIA A100 1장
- 설정: CIFAR-10/100, Tiny-ImageNet, ImageNet. ResNet-34 → ResNet-18, WRN-40-2 → WRN-16-1/16-2/40-1, VGG-11 → ResNet-18. 3회 평균 (부록).
- 평균 정확도 외 평가: 합성 데이터의 다양성 지표 (Table 4). 그 외 없음.
- 위치: Fig. 1 (p. 1), Table 1·2 (p. 7), Table 4–7 (p. 8), 부록 Table 14 (p. 11)
- 메모: 11번과 달리 합성 + 증류의 전체 시간을 보고함. 두 논문의 "배수"는 분모(합성만 대 전체)와 하드웨어가 달라 직접 비교할 수 없음.

## 축 분류 메모

- 02번 On the Efficacy of KD는 ESKD(Skip-구간)와 일찍 멈춘 teacher(Substitute)를 함께 제안함.
- 05번 Be Your Own Teacher, 06번 Deep Mutual Learning, 07번 Snapshot Distillation은 "별도 teacher 없음" 계열. teacher 준비 비용은 0이지만 학습 중 추가 연산(보조 분류기, 동료 네트워크, 이전 스냅샷 forward)이 남음.
- 09–12번(데이터 응축, 데이터 없는 증류)은 절감 대상이 "원본 데이터 접근"이나 "합성 시간"이고, 일반 KD 대비 student 학습 비용을 줄였다는 수치는 없음. 비교표에서 분모를 따로 표시해야 함.
- teacher 준비(학습) 비용을 숫자나 그림으로 비교에 넣은 논문: 01번(그림), 05번(시간), 07번(추가 1/3). 나머지는 넣지 않음.
