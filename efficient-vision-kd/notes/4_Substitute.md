# 4_Substitute 메모

> **원문 대조 완료 (2026-11-19).** 논문마다 **원문 대조** 블록이 있다. Opus 5.5 서브에이전트 8개가 저장소 PDF(일부는 arXiv·학회판을 받아)와 메모의 수치·조건·판정을 한 줄씩 대조한 결과다. 블록이 위 서술과 다르면 블록이 맞다. 대조 보고서 원본: [`audit/verify-2026-11-19/`](audit/verify-2026-11-19). 사람이 다시 확인하지는 않았다.

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
  - [실측·원논문] 선택 알고리즘이 고정 중간 지점보다 못한 경우가 있음: WRN-40-2 → MobileNetV2에서 선택한 지점 68.58% vs 절반 지점 68.99%. (Table 3)
  - [구조] "긴 cosine 스케줄의 중간 체크포인트"와 "짧은 스케줄로 끝까지 학습한 모델"은 다른 teacher임. 앞의 것을 얻으려면 스케줄을 끝까지 정해 두어야 함.
- 옮겨 간 비용:
  - 선택 알고리즘은 체크포인트마다 복원용 디코더를 학습해 상호정보를 추정하므로 추가 연산이 듦. 논문도 선택한 지점이 중간 지점보다 비용이 높다고 적음 (부록 A.2). 숫자는 그림으로만 제시.
  - teacher–student 쌍과 체크포인트마다 온도와 손실 비율을 격자 탐색으로 다시 찾음 (부록 Table 4). 이 탐색 비용은 보고 안 됨.
- 어떻게 쟀나: 단위 = teacher 학습 epoch 비율, 총 학습 시간(teacher 학습 + student 증류, TITAN Xp 기준 추정 시간, Fig. 3과 Fig. 8의 가로축) / teacher forward 셈? 총 학습 시간에 포함된 것으로 보이나 내역 없음 / 일회성 비용 셈? teacher 학습 Y (그림에서) / 하드웨어 = TITAN Xp
- 설정: CIFAR-100 WRN-40-2·ResNet-110 → WRN-40-1·ResNet-32·MobileNetV2, teacher 200 epoch. ImageNet ResNet-34 → ResNet-18, ResNet-50 → MobileNetV2, teacher 120 epoch. Tiny-ImageNet. 5회 평균.
- 평균 정확도 외 평가: 없음 (정보 평면 분석은 있음)
- 위치: Table 1·2 (p. 5), Fig. 3 (p. 6), Table 3·Algorithm 1 (p. 10), 부록 Table 4·5 (p. 15), Fig. 8 (p. 16)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 12건 / 수정 5건 / 추가 4건 / 확인 불가 0건. 본 판본: arXiv 2210.06458v1 (12 Oct 2022). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] "선택 알고리즘이 고른 지점은 teacher마다 다름 (WRN-40-2 160 epoch, ResNet-110 120 epoch, §5 p. 9). 같은 teacher라도 student에 따라 최적 지점이 다름 (Table 3 p. 10: R110→MobileNetV2는 T0.7 69.01% > T\* 68.99%). 선택 알고리즘은 student 구조를 고려하지 않음 (6절 한계, p. 10)." ← 메모: "가장 좋은 지점은 teacher마다 다르고 (WRN-40-2 160, ResNet-110 120)… (5절, 한계)"
  - [수정] "[구조] CIFAR-100에서는 200 epoch cosine 스케줄(0.1→0)의 중간 체크포인트임 (부록 A.1 p. 14). '짧은 스케줄로 끝까지 학습한 모델'과 다른 teacher이며, 앞의 것을 얻으려면 전체 스케줄을 미리 정해야 함. ImageNet 스케줄 종류는 명시 안 됨." ← 메모: [구조] "긴 cosine 스케줄의 중간 체크포인트"
  - [수정] "단위 = 총 학습 비용(teacher 학습 + student 증류). Fig. 3은 TITAN Xp 기준 추정 시간(p. 6), Fig. 8은 '계산 비용'으로만 표기하고 하드웨어 미표기(p. 16)." ← 메모: 어떻게 쟀나: "TITAN Xp 기준 추정 시간, Fig. 3과 Fig. 8의 가로축"
  - [수정] "Tiny-ImageNet(64×64, 200클래스)은 스냅샷 앙상블 실험에만 사용, teacher 150 epoch (부록 A.1 p. 14). 5회 독립 실행 평균(체크리스트 p. 13), 단 ImageNet 수치에는 표준편차 표기 없음 (Table 1 p. 5)." ← 메모: 설정: "Tiny-ImageNet. 5회 평균."
  - [수정] "평균 정확도 외 student 평가: 없음. teacher 분석으로 정보 평면(Fig. 4·6), 클래스 상관 heatmap(Fig. 5 p. 8), 평균 logit(Fig. 10 p. 17)이 있음." ← 메모: 평균 정확도 외 평가: 없음 (정보 평면 분석은 있음)
  - [추가] "선택 알고리즘은 I(Y;F)를 **테스트 정확도**로 추정함 (부록 B.1 p. 16). 체크포인트 선택에 테스트셋을 쓰므로 Table 3의 T\* 성능은 낙관적일 수 있음."
  - [추가] "스냅샷 앙상블(T1full+T1inter)은 독립 앙상블보다 앙상블 자체 정확도는 평균 3.11%p 낮지만 증류 결과는 평균 0.52%p 높음 (§3.4, Table 2 p. 5–6)."
  - [추가] "일반 조기 종료(patience 10)로 멈춘 지점보다 최적 체크포인트가 앞쪽에 있음 (부록 A.3, Fig. 9 p. 16). 조기 종료만으로는 같은 teacher를 얻지 못함."
  - [추가] "MobileNetV2는 width 0.75 (부록 A.1 p. 14)."
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
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 11건 / 수정 5건 / 추가 3건 / 확인 불가 0건. 본 판본: arXiv 1910.01348v1 (3 Oct 2019), 부록 포함 13쪽. 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] "(2) 일찍 멈춘 teacher: 학습 epoch를 줄이고 LR 스케줄도 줄인 teacher. 저자는 '전체의 1/3~1/4'이라 표현(p. 6). 실제 CIFAR-10 35/50/65/80 epoch (기준 200), ImageNet 35/50 epoch (p. 7)." ← 메모: 일찍 멈춘 teacher: 전체 epoch의 1/3~1/4만 학습
  - [수정] "ESKD + 일찍 멈춘 teacher: ResNet-34 (50 epoch) 29.02%, ResNet-50 (35) 29.05%, ResNet-152 (35) 29.26% 오류 (Table 8 p. 6). 일찍 멈춘 teacher + 전체 KD 조합은 보고 안 됨." ← 메모: 일찍 멈춘 teacher: R34(50) 29.02%, R50(35) 29.05% (Table 8)
  - [추가] "CIFAR-10은 5회 반복 (중앙값·평균·표준편차, p. 3). ImageNet은 단일 값."
  - [수정] "위치: Table 1·2 (p. 3), Fig. 3 (p. 4), Table 3 (p. 5), Table 8 (p. 6), Fig. 4·Table 9·10 (p. 7), 부록 Table 14 (p. 10), 부록 Fig. 7 (p. 11)" ← 메모: 위치: "Table 1–3·Fig. 3 (p. 4–5)"
  - [수정] "Table 8에서 ESKD를 고정하면 조기 종료 teacher의 추가 효과를 읽을 수 있음 (ResNet-34 29.16→29.02%, ResNet-50 29.35→29.05% 오류). 조기 종료 teacher + 전체 KD 칸은 없어 두 요인의 교차 효과는 알 수 없음." ← 메모: 메모: Table 8 ESKD 행은 두 요인이 섞여 "기여를 따로 읽을 수 없음"
  - [수정] "'KD error'는 teacher–student 예측 불일치 비율이며 정답 오류율과 다름 (CIFAR-10 Table 2 p. 3). ImageNet Table 3은 KD loss를 보고함 (p. 5)." ← 메모: 메모: "KD error"는 불일치 비율 (Table 2–3)
  - [추가] "ImageNet student 학습은 약 100 epoch로 보임 (부록 Fig. 7 가로축, 본문 명시 없음)."
  - [추가] "KD는 학습 초반에는 검증 정확도를 올리고 후반에는 해침 (§5.3, Fig. 3 p. 4) → ESKD의 근거."
- 메모:
  - Table 8의 ESKD 행은 teacher 조기 종료와 증류 조기 중단이 함께 들어간 수치라 (예: ResNet-34 (50) + ESKD 29.02% 오류) 두 요인의 기여를 따로 읽을 수 없음. 표의 값은 오류율임.
  - "KD error"는 teacher–student 예측 불일치 비율이며 정답 오류율과 다름 (Table 2–3).
  - ESKD는 Skip(구간)의 원형, 일찍 멈춘 teacher는 Substitute(싼 teacher)의 원형. 한 논문이 두 축에 걸침.
  - 논문은 KD를 언제 끄는지의 정확한 epoch을 본문 표에 적지 않음 (확인 안 함). 우리 실험의 "앞 구간 비율" 손잡이와 맞추려면 공식 코드나 부록 확인 필요.
  - "KD가 후반에 정확도를 해친다"(Fig. 3)는 관찰은 ImageNet·ResNet 조건에 한정.

### 03. Learning from a Lightweight Teacher for Efficient KD ([arXiv: 2005.09163](https://arxiv.org/abs/2005.09163))
- 효율화 방식: 큰 teacher를 목표 데이터에서 학습하는 대신, 아주 작은 teacher(LeNet5)를 쉬운 합성 데이터(SynMNIST)에서 학습해 그 soft target을 씀. student 출력 분포를 teacher 출력 분포에 맞추는 적대적 손실을 더함.
- 얻는 것 (teacher의 크기와 1개 입력당 연산, 일반 KD의 teacher 대비): CIFAR-10에서 LeNet5 teacher 약 6.2만 매개변수·48만 FLOPs vs ResNet-18 teacher 1,120만 매개변수·3,720만 FLOPs. 약 두 자릿수 차이. (Table 8)
- 잃는 것:
  - [실측·원논문] 일반 KD와 비슷함: CIFAR-10 ResNet-20 student, KD(teacher ResNet-18) 92.98% vs 제안 92.94%. MobileNetV2는 91.20 vs 91.57%, ShuffleNetV2는 92.17 vs 92.48%. CIFAR-100 ResNet-20은 지도학습 68.38 → 69.39%. (Table 3)
  - [구조] 작은 teacher의 출력에서 최댓값 확률을 **정답 클래스 위치로 바꿔 넣음** (§3.2.3). 즉 정답 라벨을 써서 분포를 재배열함. 목표 데이터의 의미 관계를 전달한다기보다 label smoothing에 가까움.
- 옮겨 간 비용: 합성 데이터 생성, 작은 teacher 학습, 판별기 학습(약 4,900 매개변수). Table 8에는 판별기의 forward FLOPs만 있음. 판별기 backward와 적대적 최적화 비용은 빠져 있으므로 Table 8의 합을 전체 학습 비용으로 쓰면 안 됨. teacher forward만 비교하면 LeNet5 대 ResNet-18이 약 77배 (재계산).
- 어떻게 쟀나: 단위 = 모델 매개변수 수, 입력 1개당 FLOPs. 학습 시간은 보고 안 됨 / teacher forward 셈? Y (FLOPs로) / 일회성 비용 셈? teacher 학습 시간 N (크기 비교만) / 하드웨어 = GTX 2080 Ti
- 설정: CIFAR-10/100 student ResNet-20·MobileNetV2·ShuffleNetV2. 텍스트 분류와 UCF101 동작 인식도 실험.
- 평균 정확도 외 평가: 없음. 일부 비교에 t-검정 (p ≤ 0.05)이 있으나 그룹 성능과는 무관.
- 위치: Table 3 (p. 6), Table 7·Fig. 4 (p. 8), Table 8 (p. 9)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 7건 / 수정 3건 / 추가 3건 / 확인 불가 0건. 본 판본: arXiv 2005.09163v1 (19 May 2020). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] "작은 teacher(LeNet5, 약 6.17만 매개변수)를 쉬운 숫자 데이터에서 학습: CIFAR-10은 일반 MNIST(T1, 10 epoch), CIFAR-100은 합성 SynMNIST-100(T3, 10 epoch) (Table 2, §5.1.1 p. 6). Table 3 머리글은 둘 다 'Ours[T1]'로 표기되어 본문과 어긋남." ← 메모: 작은 teacher(LeNet5)를 합성 데이터(SynMNIST)에서 학습
  - [수정] "매개변수 약 182배, 입력 1개당 FLOPs 약 77배 차이 (Table 8 p. 9, 재계산). 원문은 둘 다 '약 두 자릿수'라고 씀 (§5.3 p. 8)." ← 메모: Table 8: LeNet5 ~61.7K·~481.7K FLOPs vs ResNet-18 ~11.2M·~37.2M, "약 두 자릿수 차이"
  - [추가] "ResNet-50 teacher KD는 CIFAR-10 세 student 모두에서 제안법보다 높음: 93.04 vs 92.94, 91.69 vs 91.57, 92.55 vs 92.48% (Table 3 p. 6)."
  - [수정] "하드웨어 = GTX 2080 Ti 2장 (§4.3 p. 6)" ← 메모: 하드웨어 = GTX 2080 Ti
  - [추가] "teacher 학습 시간은 보고 안 됨. 학습 길이만 보고: LeNet5 10 epoch (UCF101용 T4는 2 epoch) (Table 2 p. 6)."
  - [추가] "student 학습 매 단계마다 짝지은 합성 이미지에 teacher forward를 함 (Algorithm 2 p. 5). 짝이 고정(일대일)이라 teacher 출력은 미리 계산해 둘 수 있으나 논문은 그렇게 하지 않음 (추론)."
- 메모: 비용 지표가 teacher의 크기와 추론 FLOPs뿐이라 "teacher 학습이 얼마나 빨라졌나"는 직접 재지 않음. teacher가 목표 과제를 풀지 못하는데도(다른 데이터로 학습) 이득이 난다는 점에서 label smoothing 계열로 읽는 것이 맞음.

### 04. Tf-KD (Revisiting KD via Label Smoothing Regularization) ([arXiv: 1909.11723](https://arxiv.org/abs/1909.11723))
- 효율화 방식: 더 큰 teacher 없이 KD 효과를 얻음. 두 가지 변형. Tf-KD_self: student 자신을 먼저 학습해 그 모델을 teacher로 씀. Tf-KD_reg: 정답 클래스에 0.99를 준 수작업 분포를 "가상 teacher"로 씀 (모델이 필요 없음).
- 얻는 것: 비용 수치는 보고 안 됨. Tf-KD_reg는 추가 연산이 없다고 주장. 정확도 향상 (기준 학습 대비):
  - CIFAR-100 MobileNetV2: Tf-KD_self +2.58%p, Tf-KD_reg +2.50%p, 일반 KD(teacher ResNet-18) +2.67%p. (Table 5·9)
  - ImageNet ResNet-50: Tf-KD_reg +0.65%p, label smoothing +0.51%p. (Table 11)
- 잃는 것: [실측·원논문] 일반 KD보다 낮은 경우가 있음: Tiny-ImageNet ResNet-50에서 Tf-KD_reg +0.45%p vs 일반 KD +0.68%p. ImageNet에서 label smoothing과의 차이는 0.1~0.4%p로 작음. (Table 10·11)
- 옮겨 간 비용: Tf-KD_self는 student를 한 번 더 학습해야 함 (자기 자신을 teacher로 만들기 위한 선행 학습)과 학습 중 그 모델의 forward. 숫자 보고 안 됨. Tf-KD_reg는 없음.
- 어떻게 쟀나: 단위 = 없음 (정확도만) / teacher forward 셈? N / 일회성 비용 셈? N / 하드웨어 = 보고 안 됨
- 설정: CIFAR-100, Tiny-ImageNet (200 epoch), ImageNet (90 epoch). MobileNetV2, ShuffleNetV2, ResNet-18/50, DenseNet-121, ResNeXt 등. 3회 평균 (일부). Tf-KD_reg의 ImageNet 설정은 정답 확률 a = 0.99, 온도 20, α = 0.1. Tf-KD_self와 일반 KD는 하이퍼파라미터 탐색을 함.
- 평균 정확도 외 평가: 없음
- 위치: Table 1–4 (p. 4), Table 5–8 (p. 7), Table 9–11 (p. 8)
- **정정 (검수 보고서 근거)**: 표 11에서 수치와 차이가 불일치: ResNet-50 LSR 76.38, 기준 75.77이면 +0.61%p인데 표는 +0.51%p. 원점수로 계산한 Tf-KD_reg 차이는 +0.04%p, 표기된 상승폭 기준이면 +0.14%p. 인용 시 출처와 재계산을 명시. [표 11](https://www.alphaxiv.org/pdf/1909.11723v3?page=8)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 11건 / 수정 1건 / 추가 2건 / 확인 불가 1건. 본 판본: arXiv 1909.11723v3 (4 Mar 2021), 9쪽 (부록 미포함). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] "ImageNet에서 label smoothing과의 차이는 원점수 기준 0.04~0.38%p로 작음 (ResNet-18 0.22, ResNet-50 0.04, DenseNet-121 0.38, ResNeXt-101 0.09; Table 11 p. 8)." ← 메모: ImageNet에서 LSR과의 차이 0.1~0.4%p
  - [추가] "덜 학습한 teacher로도 이득이 남: CIFAR-100에서 정확도 15.48%인 ResNet-18 teacher가 MobileNetV2를 68.38→70.65% (+2.27%p)로 올림. 완전 학습 teacher는 71.05% (Table 4·Table 1 p. 4). 01번(체크포인트)과 같은 축의 근거."
  - [추가] "CIFAR-100·Tiny-ImageNet의 Tf-KD_reg τ·α는 모델마다 다름 (p. 7). 'reg는 튜닝 비용 0'으로 쓰면 안 됨."
  - [확인 불가] 부록이 PDF에 없어 τ·α 값 확인 불가
- 메모:
  - Tf-KD_reg는 teacher 비용이 0인 기준점. "teacher 연산을 0으로 줄였을 때 남는 이득"을 보여 주는 대조군으로 쓸 수 있음.
  - 두 변형의 준비 비용이 전혀 다름 (self는 선행 학습 1회, reg는 0). 한 줄로 묶어 "teacher 없음"이라고 쓰면 안 됨.

### 05. Be Your Own Teacher ([arXiv: 1905.08094](https://arxiv.org/abs/1905.08094))
- 효율화 방식: 한 네트워크 안에서 가장 깊은 분류기가 얕은 구간에 붙인 보조 분류기들을 가르침 (자기 증류). 따로 teacher를 설계하고 학습하는 단계가 없음.
- 얻는 것 (teacher 학습 + 증류의 2단계 방식 대비): CIFAR-100에서 학습 시간 26.98시간 (teacher 14.67 + 증류 12.31) → 5.87시간 (4.6배 단축). (Fig. 1, 1절) ResNet-50 정확도는 일반 KD 79.33% 대비 가장 깊은 출구 80.56% (Table 3). 본문의 81.04%는 여러 출구의 **앙상블** 값임 (Table 1).
  - 같은 그림에서 일반 학습은 4.0시간이라 자기 증류가 일반 학습보다는 느림.
- 잃는 것:
  - [구조] 학습 중 보조 분류기와 병목 층이 추가됨. 추론 때는 제거 가능.
  - [실측·원논문] 얕은 출구로 예측하면 빠르지만 정확도가 낮음: ResNet-50, 1/4 지점 출구 4.64배 가속에 67.87% (기준 77.68%). (Table 5)
- 옮겨 간 비용: 보조 분류기의 학습 연산. 숫자 보고 안 됨 (총 학습 시간에는 포함된 것으로 보임).
- 어떻게 쟀나: 단위 = 실제 학습 시간(시간), 추론 가속 배수 / teacher forward 셈? Y (비교 대상의 시간에 teacher 학습과 증류가 모두 포함) / 일회성 비용 셈? Y (teacher 학습 시간을 비교 대상에 포함) / 하드웨어 = 보고 안 됨
- 설정: CIFAR-100, ImageNet. ResNet-18/50/101/152, ResNeXt, WideResNet, VGG.
- 평균 정확도 외 평가: 잡음 섞은 가중치에서의 정확도와 손실 (평탄한 최소점 분석, Fig. 4). 그 외 없음.
- 위치: Fig. 1 (p. 2), Table 1·2 (p. 4), Table 3·4 (p. 5), Table 5 (p. 6)
- **정정 (검수 보고서 근거)**: 앙상블 81.04%, 가장 깊은 출구 80.56%가 맞음. 표 5의 정확도 열은 표 4의 DSN 행과 같음. 표 5의 4.64배 추론 가속을 표 1 성능과 한 쌍으로 인용할 근거는 약함. 학습 시간 5.87 h는 teacher 준비까지 합친 기존 KD 26.98 h보다 짧고 student 단독 4.03 h보다 긺. [Fig. 1](https://www.alphaxiv.org/pdf/1905.08094v1?page=2) · [표 4–5](https://www.alphaxiv.org/pdf/1905.08094v1?page=5)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 10건 / 수정 4건 / 추가 3건 / 확인 불가 0건. 본 판본: arXiv 1905.08094v1 (17 May 2019). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] "같은 그림에서 일반 학습은 4.03시간(77.68%)이라 자기 증류(5.87시간)가 약 1.46배 느림 (Fig. 1 p. 2)." ← 메모: 일반 학습 4.0시간, 자기 증류가 더 느림
  - [수정] "보조 분류기·병목 층의 학습 연산. 구성 요소별 수치는 없음. 총 학습 시간으로는 일반 학습 4.03 h → 자기 증류 5.87 h (+1.84 h, 약 1.46배; ResNet-50 CIFAR-100, Fig. 1 p. 2)." ← 메모: 옮겨 간 비용: 보조 분류기 학습 연산, "숫자 보고 안 됨"
  - [수정] "평균 정확도 외 평가: 잡음 섞은 가중치의 정확도·손실(Fig. 4 p. 7), 층별 기울기 크기(Fig. 5), 출구별 특징 분리도 SSE/SSB(Table 6 p. 8). 집단·최악 클래스 지표는 없음." ← 메모: 평균 정확도 외 평가: 잡음 가중치 정확도·손실 (Fig. 4), 그 외 없음
  - [수정] "표 5의 정확도 행은 ResNet-18·50·101에서 표 4 DSN 행과 같고, ResNet-152는 제안법 행과 같음(2/4 출구만 78.22 vs 78.72). 표 간 기준이 섞여 있어 4.64배 가속을 표 1 성능과 한 쌍으로 인용하지 않음." ← 메모: **정정 줄**: "표 5의 정확도 열은 표 4의 DSN 행과 같음"
  - [추가] "81.04%(앙상블)는 단일 출구보다 추론이 느림: ResNet-50 앙상블 0.93배 속도 (Table 5 p. 6)."
  - [추가] "ImageNet에서는 앙상블이 가장 깊은 출구보다 낮을 수 있음 (ResNet-18 68.93 vs 69.84%, ResNet-50 74.73 vs 75.24%; Table 2 p. 4). 학습 시간은 ImageNet에서 보고 안 됨."
  - [추가] "평균 향상 2.65%(CIFAR-100)·2.02%(ImageNet)는 어느 출구 기준인지 명시 안 됨 (§4.2 p. 5). 인용 시 출구 확인 필요."
- 메모: teacher 학습 시간까지 포함해 실제 시간으로 비교한 초기 사례. 다만 하드웨어와 측정 조건이 없음. 4.6배는 단일 출구·앙상블 구분과 비용 경계를 확인한 뒤에만 인용할 것. Table 5의 ResNet-18 값이 Table 4 DSN 행과 겹치는 등 표 간 일관성도 확인 필요 (Notion 정리에서 지적).

### 06. Deep Mutual Learning ([arXiv: 1706.00384](https://arxiv.org/abs/1706.00384))
- 효율화 방식: 사전학습된 teacher 없이 여러 student가 동시에 학습하면서 서로의 예측 분포를 맞춤 (KL 손실).
- 얻는 것: 비용 수치는 보고 안 됨. 보고한 것은 정확도: CIFAR-100 MobileNet 두 개를 함께 학습하면 한 모델이 혼자 학습 73.65 → 76.21% (Table 2). Market-1501 ImageNet 사전학습 MobileNet의 single-query mAP 60.68 → 68.83% (Table 3). 사전학습 teacher로 증류하는 것보다도 높다고 보고 (Table 4).
- 잃는 것: [구조] 학습 중 네트워크 K개를 동시에 돌려야 함. student 하나 기준으로는 학습 연산과 메모리가 K배.
- 옮겨 간 비용: teacher 사전학습 비용이 "동료 네트워크의 동시 학습" 비용으로 바뀜. 숫자 보고 안 됨.
- 어떻게 쟀나: 단위 = 없음 (정확도, mAP) / teacher forward 셈? N / 일회성 비용 셈? N / 하드웨어 = GTX 1080 1장
- 설정: CIFAR-100 (200 epoch, batch 64), ResNet-32, MobileNet, InceptionV1, WRN-28-10. Market-1501 재식별 (100,000 반복).
- 평균 정확도 외 평가: 재식별 mAP와 Rank-1 (다른 과제). 그 외 없음.
- 위치: Table 1·2 (p. 5), Table 3·4 (p. 6), Fig. 2 (p. 7)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 8건 / 수정 3건 / 추가 2건 / 확인 불가 0건. 본 판본: arXiv 1706.00384v1 (1 Jun 2017). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] "사전학습 teacher로 증류한 것보다 높음: CIFAR-100 ResNet-32 69.48 → 70.73% (WRN-28-10 teacher), Market-1501 MobileNet 49.11 → 52.87% mAP (InceptionV1 teacher, ImageNet 사전학습 없는 설정) (Table 4 p. 6)." ← 메모: 사전학습 teacher 증류보다 높다 (Table 4)
  - [수정] "[구조] 학습 중 네트워크 K개를 함께 학습하고 상호 KL을 계산함. 같은 구조 K개면 연산이 대략 K배 이상(Algorithm 1은 갱신마다 예측을 다시 계산, p. 4). 이질 cohort에서는 K배가 아님. 메모리·시간 측정 없음." ← 메모: [구조] K개 동시 실행, student 하나 기준 학습 연산·메모리 K배
  - [추가] "저자는 비용을 수치 없이 서술만 함: 네트워크별 장치 분산 가능(p. 4), 앙상블을 원래 쓰는 경우 추가 비용 최소(p. 7)."
  - [수정] "평균 정확도 외 평가: 재식별 mAP·Rank-1, 가중치 잡음에 대한 손실 변화(Fig. 3b p. 8), 사후확률 엔트로피(DML 1.7099 vs 독립 0.2602, p. 8), cohort 크기별 mAP 표준편차(Fig. 2 p. 7). 집단·최악 클래스 지표는 없음." ← 메모: 평균 정확도 외 평가: 재식별 mAP·Rank-1, 그 외 없음
  - [추가] "같은 구조 MobileNet을 사전학습 teacher로 쓴 일반 증류는 독립 학습보다 나쁨 (45.16 vs 46.07% mAP, Market-1501; Table 4 p. 6). DML은 51.26%."
- 메모: "teacher가 필요 없다"와 "비용이 적다"는 다른 말임을 보여 주는 사례. 모델 하나를 얻는 비용과 동료 전체의 비용을 구분해 적어야 함. 여러 GPU로 병렬화해 경과 시간이 줄어도 총 GPU 시간이 준다는 보장은 없음. 원래 동료 모델들도 학습할 계획이었는지가 공정한 기준선을 바꿈.

### 07. Snapshot Distillation ([arXiv: 1812.00123](https://arxiv.org/abs/1812.00123))
- 효율화 방식: 순환 학습률로 한 번의 학습을 여러 주기로 나누고, 각 주기의 마지막 스냅샷이 다음 주기의 teacher가 됨. 여러 세대를 차례로 학습하는 방식을 한 세대 안에서 해결.
- 얻는 것: 기준 학습 대비 추가 학습 시간 약 1/3. K세대 방식은 K배의 학습 시간이 든다고 대비. (1절, 결론)
- 잃는 것: [실측·원논문] 표에서는 손실 없음. ImageNet top-1 오류 ResNet-101 21.62 → 21.25%, ResNet-152 21.17 → 20.93%. (Table 4) 이득 폭은 0.3%p 안팎으로 작음.
- 옮겨 간 비용: 이전 스냅샷의 forward (학습 중 매 스텝). 이것이 추가 시간 1/3의 원인.
- 어떻게 쟀나: 단위 = 기준 대비 추가 학습 시간의 비율 (서술로만, 측정 표 없음) / teacher forward 셈? Y (추가 시간 1/3이 그것) / 일회성 비용 셈? 해당 없음 (별도 teacher 학습이 없음) / 하드웨어 = 보고 안 됨
- 설정: CIFAR-100, ImageNet (ResNet-101·152, 90 epoch). 순환 주기 2개.
- 평균 정확도 외 평가: PascalVOC 검출과 분할로의 전이 (Table 5). 그 외 없음.
- 위치: Table 1 (p. 1), 1절 (p. 2), Table 3 (p. 6), Table 4·5·Fig. 1 (p. 8)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 9건 / 수정 4건 / 추가 4건 / 확인 불가 1건. 본 판본: arXiv 1812.00123v1 (PDF p.1 하단, 1 Dec 2018). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] 손실 없음은 ImageNet Table 4 한정. CIFAR-100 DenseNet190은 최종 epoch 기준 SD가 더 나쁨: 오류 BL 16.82% vs SD 18.02%(T=2)·18.65%(T=3). 가장 좋은 epoch 기준으로만 16.06% vs 16.69%로 앞섬. (Table 3 p.6, p.7) ← 메모: "표에서는 손실 없음"
  - [수정] 이전 스냅샷 forward는 두 번째 mini-generation부터만 듦 (ImageNet은 뒤 45 epoch, CIFAR-100은 4개 중 뒤 3개). 1/3 추가 시간이 무엇에서 나오는지는 원문이 나누어 밝히지 않음. (p.2, p.5) ← 메모: 옮겨 간 비용: 이전 스냅샷 forward를 매 스텝 함. 이것이 1/3의 원인
  - [수정] teacher forward 셈? 추정 Y (1/3이 학습 전체 시간 대비 서술이므로). 원문이 내역을 밝히지 않음. ← 메모: teacher forward 셈? Y (추가 1/3이 그것)
  - [수정] 설정: CIFAR-100 (ResNet-20/32/56/110 164 epoch, DenseNet-100/190 300 epoch, 주기 4개), ImageNet (ResNet-101·152, 90 epoch, 45 epoch 주기 2개, T=2). (p.6–7) ← 메모: 설정: CIFAR-100, ImageNet (R101·152, 90 epoch). 순환 주기 2개
  - [추가] 전이: ResNet152 기준 VOC2007 검출 mAP 73.49→74.93%, VOC2012 분할 mIoU 77.53→77.97%. (Table 5 p.8)
  - [추가] 그 밖에 학습·테스트 오류 곡선 (두 번째 주기에서 학습-테스트 격차 줄어듦, Fig. 1 p.8), top-5 오류와 스냅샷 앙상블 오류 (Table 3·4).
  - [추가] 주기를 나누는 것만으로는 ImageNet에서 손해: snapshot ensemble 22.14% vs BL 21.62% (R101). SD 이득 일부는 주기 분할 손실을 메우는 몫. (Table 4 p.8)
  - [추가] Table 1은 수치가 아니라 기존 T-S 방법의 속성 비교표 (같은 구조·ImageNet 평가·한 세대 여부).
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
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 9건 / 수정 4건 / 추가 4건 / 확인 불가 0건. 본 판본: arXiv 2407.10366v2 (11 Feb 2025), ICLR 2025 판. 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] 학습 시간 비교 대상은 공식 DINOv2가 아니라 DeiT. DeiT ViT-S/16 (RegNetY teacher) 30h vs Proteus ViT-S/14 (DINOv2-B/14 teacher) 30h, IN-1K 300 epoch, A100 8장. 공식 DINOv2의 학습 시간은 보고 안 됨. (Table 19 p.18) ← 메모: 얻는 것 (공식 DINOv2 대비) 아래에 학습 시간 30h = DeiT 30h
  - [추가] ImageNet-A 격차: ViT-S 31.1 vs 34.1%, ViT-B 52.4 vs 55.5%, ViT-L 68.7 vs 71.4%. S·B는 teacher 크기도 공식 DINOv2(g)보다 작음. (Table 14 p.16)
  - [추가] [실측·원논문] 세부 분류 평균은 비슷하나 FGVC-Aircraft, Stanford Cars, SUN397에서 DINOv2-S보다 낮음 (p.6). Country211에서도 teacher 계열보다 낮음: 19.4 vs 21.8% (DINOv2), 29.3 vs 32.3% (CLIP). (Table 17 p.17)
  - [수정] 옮겨 간 비용: 학습 중 teacher forward와 정렬용 투영 헤드 (30h 안에 포함). teacher의 원 데이터 사전학습 비용은 측정 범위 밖. 바뀌는 것은 student가 보는 데이터 접근뿐. (p.18) ← 메모: 옮겨 간 비용: 없음
  - [수정] batch 1024 (ViT-L만 GPU 메모리 제약으로 256). (§3.1 p.5) ← 메모: 설정: DINOv2-S/B/L/g → ViT-Ti/S/B/L, 300 epoch, batch 1024
  - [수정] 보고된 DeiT 비교의 모든 항목에서 Proteus가 나음. 단 ADE20K·NYUd의 DeiT는 증류 없는 판이고, Im-C·NYUd는 낮을수록 좋음. ViT-Ti Im-C는 67.3 vs 67.4로 거의 같음. (Table 3 p.6) ← 메모: 평균 외 평가 Table 3, DeiT 대비 모든 항목에서 높음, ViT-B Im-A 29.6→52.4
  - [추가] 추가 평가: proxy 데이터 다양성 (Table 15), teacher가 안 본 개념으로의 일반화 (Table 17), teacher 크기 변화 (Table 18: teacher가 클수록 평균 정확도 하락). (p.16–18)
  - [추가] 공식 DINOv2-S/B/L은 DINOv2-g에서 증류해 teacher가 커서 학습 시간이 크게 늘어난다고 서술 (수치 없음). Proteus는 기본적으로 한 단계 큰 모델을 teacher로 씀 (Proteus-L만 g). (p.6)
- 메모: 부록 Table 19의 검증 시간 9시간 대 1시간은 "매 epoch 검증" 대 "학습 후 선형 평가 1회"라는 프로토콜 차이임. 비용 절감으로 읽으면 안 됨. DeiT 비교는 teacher 종류(RegNetY 대 DINOv2)까지 다르므로 순수한 데이터 대체 효과도 아님.
- 메모 (계속): 데이터를 100분의 1로 줄였지만 student 학습 시간 자체는 일반 증류와 같음. 절감되는 것은 "데이터 접근"이지 연산이 아님. 공정한 비교에는 teacher가 본 데이터와 student가 본 데이터를 따로 적어야 함.

### 09. SRe2L (Squeeze, Recover and Relabel) ([arXiv: 2306.13092](https://arxiv.org/abs/2306.13092))
- 효율화 방식: 데이터셋 응축. (1) 원본 데이터로 모델을 학습해 정보를 모델에 압축, (2) 그 모델의 BN 통계에 맞도록 합성 이미지를 복원, (3) 합성 이미지에 teacher로 soft label을 다시 붙임. 합성 단계에 원본 데이터가 필요 없어 메모리와 시간이 줄어듦.
- 얻는 것 (기존 응축 방법 대비 합성 비용, Tiny-ImageNet, RTX 4090 1장): 이미지 1장·반복 1회당 시간 MTT 12.64 ms → 0.24 ms (ConvNet-4), 0.75 ms (ResNet-18). 최대 GPU 메모리 48.9 → 4.2 GB, 7.6 GB. (Table 1)
- 잃는 것: [실측·원논문] 전체 데이터 학습과는 큰 차이가 남음: ImageNet-1K 클래스당 50장에서 ResNet-101 student 60.8%, ResNet-18 student 46.8% (전체 데이터 ResNet-18 69.76%). 60.8%는 큰 student의 값이라는 점에 주의. (1절, Table 4; RDED Table 2에서도 같은 값)
- 옮겨 간 비용: 압축 단계의 모델 학습(공개 사전학습 모델을 쓰면 0), 합성 반복, 재라벨링용 soft label 사전 생성과 저장 (FKD 방식). soft label 저장 용량은 이 논문에 숫자가 없음. 다른 논문의 측정: 응축 데이터의 30배 이상 (3_Reuse 09번).
- 어떻게 쟀나: 단위 = 합성 시간(이미지·반복당 ms), 최대 GPU 메모리(GB) / teacher forward 셈? N (재라벨링 시간 없음) / 일회성 비용 셈? 합성 비용 Y, 압축 단계 학습과 재라벨링 N / 하드웨어 = RTX 4090 1장
- 설정: Tiny-ImageNet, ImageNet-1K (224 해상도). 복원 모델 ResNet-18/50, 평가 student ResNet-18/50/101 등.
- 평균 정확도 외 평가: 다른 구조로의 일반화 (Table 5), 지속 학습 (Fig. 6). 그 외 없음.
- 위치: Fig. 1 (p. 1), Table 1 (p. 3), Table 4·5 (p. 9), 부록 Table 7 (p. 15)
- **정정 (검수 보고서 근거)**: 클래스당 50장에서 60.8%는 ResNet-101 student, ResNet-18은 46.8%. 복구 단계는 고정 모델의 순전파와 입력 이미지에 대한 역전파를 씀. 오프라인 재라벨링 시간은 보고 안 됨. [§3](https://www.alphaxiv.org/pdf/2306.13092v3?page=4) · [표 4–5](https://www.alphaxiv.org/pdf/2306.13092v3?page=9)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 8건 / 수정 4건 / 추가 4건 / 확인 불가 1건. 본 판본: arXiv 2306.13092v3 (11 Feb 2024), NeurIPS 2023. 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] 조건 추가: Tiny-ImageNet 64×64, 최대 메모리는 batch 200 기준으로 재거나 환산한 값. (Table 1 p.3) ← 메모: Table 1: MTT 12.64 ms → 0.24 (C4)·0.75 (R18), 메모리 48.9 → 4.2·7.6 GB, RTX 4090 1장
  - [추가] ImageNet-1K (224) 합성 비용: 이미지 1장·반복 1회당 ResNet-18 0.83 ms, ResNet-50 2.62 ms (RTX 4090). ablation은 2k 반복, Table 4 최고 값은 4k 반복. (Table 3·본문 p.7)
  - [수정] 위치 수정: 69.76%는 Table 4가 아니라 실험 설정 (p.6)과 부록 (p.15)의 사전학습 ResNet-18 정확도. 같은 구조 기준 격차 69.76 − 46.8 = 22.96%p (계산). ← 메모: 클래스당 50장: R101 student 60.8%, R18 46.8%, 전체 데이터 R18 69.76% (1절, Table 4)
  - [추가] [실측·원논문] ViT student에서 약함: ResNet-18 압축 데이터로 DeiT-Tiny 15.41% (ResNet-18 46.71%) (Table 5 p.9). CIFAR-10에서는 기존 방법과 뚜렷한 격차 (부록 p.17).
  - [수정] 압축 단계 학습은 ImageNet-1K에서만 공개 사전학습 모델로 대체 (새 학습 0). Tiny-ImageNet은 ResNet-18/50을 처음부터 100 epoch 학습. 그 시간은 보고 안 됨. (p.6, Table 7 p.15) ← 메모: 옮겨 간 비용: 압축 단계 모델 학습 (공개 사전학습 모델이면 0)
  - [수정] teacher forward 셈? 합성 단계 Y (복원 모델의 forward·backward가 합성 시간에 들어감), 재라벨링과 student 학습 N (시간 보고 안 됨). ← 메모: teacher forward 셈? N (재라벨링 시간 없음)
  - [추가] 설정 추가: 부록에 CIFAR-100 (Table 11)과 64·112 해상도 ImageNet (Table 9). 응축 데이터로 student를 300 epoch 학습 (Table 7b p.15).
  - [추가] Fig. 1: ImageNet-1K 클래스당 10장, 합성 시간 TESLA 12.89 ms vs SRe2L 0.83 ms (이미지·반복당). (p.1)
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
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 9건 / 수정 3건 / 추가 4건 / 확인 불가 0건. 본 판본: arXiv 2312.03526v2 (19 Mar 2024). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [추가] 헤드라인은 ResNet-18 관찰·평가, ImageNet-1K. Table 3의 이미지당 시간 × 10,000장으로 계산하면 SRe2L 약 5.9시간, RDED 약 6.6분으로 초록 수치와 맞음 (계산).
  - [수정] 조건 추가: 100장을 동시에 만들 때 이미지당 ms, 최대 메모리는 batch 100. 같은 표 ResNet-18: 2113.23 → 39.89 ms, 9.14 → 1.57 GB. (Table 3 p.7) ← 메모: Table 3: MobileNet-V2 3783.16 → 64.97 ms, 12.93 → 2.35 GB
  - [수정] 전체 데이터 기준값은 이 논문에 없음. 격차를 적으려면 다른 출처 필요 (예: 09번 SRe2L p.6의 사전학습 ResNet-18 69.76%). ← 메모: "전체 데이터 학습과의 차이는 여전히 큼"
  - [추가] [실측·원논문] SRe2L보다 낮은 경우: Tiny-ImageNet 클래스당 50장 ResNet-101 41.2 vs 42.5%. ImageNet-1K 50장 ResNet-101은 61.2 vs 60.8%로 차이 0.4%p. 클래스당 1장에서는 고해상도에서도 약하다고 저자가 인정 (p.7). MDC 대비 CIFAR-10 모든 IPC에서 낮음 (Table 6 p.14).
  - [수정] 관찰(teacher)과 평가 모델 구분: ResNet 열은 ResNet-18로 만들고 ResNet-18·101로 평가. ConvNet 열은 Conv-3~6으로 만들고 같은 모델로 평가. (Table 2 p.7) ← 메모: 설정: teacher와 평가 모델 ConvNet, R18, R101 등
  - [추가] 부록 Table 5에서 BN 없는 구조 (VGG-11, Swin-V2-Tiny)까지 일반화 평가. 여러 IPC를 한 번에 만드는 multisize 비교 (Table 6), coreset 선택 기준선 (Table 7). (p.14)
  - [추가] 단, 결과물은 원본 이미지 부분집합이 아니라 패치 4개를 이어 붙인 재구성 이미지 (부록 C.2 p.13).
- 메모: 초록의 "7분"은 응축 데이터 **제작** 시간이고 student 학습 완료까지의 시간이 아님. 원본 실제 이미지에 접근해야 하므로 데이터 없는 증류(11·12번)와 제약이 다름. teacher 점수로 고른 패치가 소수 그룹을 보존하는지는 검증 안 됨.
- 메모 (계속): 09번과 마찬가지로 soft label 저장이 숨은 비용. 실제 이미지 조각을 쓰므로 "합성 데이터"보다는 "teacher가 고른 부분집합"에 가까움 (Skip-데이터와 맞닿음).

### 11. Up to 100x Faster Data-free KD (FastDFKD) ([arXiv: 2112.06253](https://arxiv.org/abs/2112.06253))
- 효율화 방식: 원본 데이터 없이 teacher를 역으로 풀어 합성 이미지를 만드는 증류에서, 합성 단계를 빠르게 함. 여러 샘플이 공유하는 공통 특징을 메타 학습으로 재사용해 샘플당 최적화 스텝을 수 회로 줄임.
- 얻는 것 (합성에 든 GPU 시간, 기존 데이터 없는 증류 대비):
  - CIFAR-100 ResNet-34 → ResNet-18: DeepInv 42.1시간 (61.32%), CMI 19.2시간 (77.04%), 제안 10스텝 0.27시간 (74.34%).
  - ImageNet ResNet-50 → ResNet-50: DeepInv 166시간 (68.00%) → 6.28시간 (68.61%). (Table 1·2)
- 잃는 것:
  - [실측·원논문] 스텝을 줄일수록 정확도가 떨어짐: CIFAR-100 2스텝 69.76%, 5스텝 72.82%, 10스텝 74.34%. 가장 정확한 기존 방법 CMI(77.04%)보다는 낮음. (Table 1)
  - [실측·원논문] 원본 데이터로 학습한 것과 차이: ImageNet 68.61% vs 75.45%. (Table 2)
  - [실측·원논문] 같은 쌍에서도 기존 방법보다 낮은 경우: CIFAR-100 WRN-40-2 → WRN-16-2에서 CMI 68.75% vs 제안 10스텝 65.12%. "정확도 손실 없이 100배"로 일반화하면 안 됨. (Table 1)
- 옮겨 간 비용: 합성 자체가 데이터의 대체 비용. 생성기와 메타 학습의 연산은 보고한 GPU 시간에 포함.
- 어떻게 쟀나: 단위 = 합성의 GPU 시간, 기준 방법 대비 배수. student 학습 시간은 평가 지표 정의에서 명시적으로 제외함 (Evaluation Metrics, p. 5) / teacher forward 셈? 합성 시간에 포함 / 일회성 비용 셈? 합성 Y, teacher 학습 N / 하드웨어 = GPU 1장 기준 추정 (종류 보고 안 됨)
- 설정: CIFAR-10/100 여러 쌍, ImageNet ResNet-50 → ResNet-50·ResNet-18·MobileNetV2, NYUv2 분할.
- 평균 정확도 외 평가: 없음 (분할 mIoU는 다른 과제)
- 위치: Fig. 1 (p. 1), Table 1·2 (p. 6), Table 3–5 (p. 7)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 10건 / 수정 1건 / 추가 4건 / 확인 불가 1건. 본 판본: arXiv 2112.06253v2 (24 Feb 2022), AAAI. 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] 합성 **mini-batch**당 최적화 스텝을 수 회 (2·5·10, ImageNet 50)로 줄임. (Alg. 1 p.4, p.6) ← 메모: 공통 특징을 메타 학습으로 재사용해 샘플당 최적화 스텝을 수 회로
  - [추가] 조건: Fast50, 합성 140k장. DeepInv 대비 166/6.28 ≈ 26.4배 (계산). 표의 47.8배는 Generative DFD (약 300h) 기준. (Table 2 p.6)
  - [추가] [실측·원논문] ImageNet 작은 student에서는 기존 Generative DFD보다도 약간 낮음: ResNet-18 53.45 vs 54.66%, MobileNetV2 43.02 vs 43.15%. 실제 데이터 학습 68.45·70.01%와 격차 큼. (Table 2 p.6)
  - [추가] NYUv2 분할: 합성 0.82시간, mIoU 0.366 (DFAD 6.0시간 0.364). 실제 데이터 KD 0.380, student 0.375보다 낮음. (Table 3 p.7)
  - [추가] 배수 기준: CIFAR는 DeepInv2k 대비 평균 (Fast10 약 125배, Fast2 최대 655배이나 정확도 손실 큼), ImageNet은 Generative DFD 대비 47.8배. 제목의 "100×"는 헤드라인 값. (Table 1·2 p.6)
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
- **정정 (검수 보고서 근거)**: 5–15배는 DeepInv 기준. CIFAR-100, 같은 100 epoch에서 FM 대비 2.23 → 2.15 h는 약 1.04배. 합성만 잰 NYUv2에서는 두 방법 모두 0.82 h. [Fig. 1](https://www.alphaxiv.org/pdf/2310.00258v2?page=1) · [표 2](https://www.alphaxiv.org/pdf/2310.00258v2?page=7)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 9건 / 수정 3건 / 추가 3건 / 확인 불가 1건. 본 판본: arXiv 2310.00258v2 (22 Mar 2024). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [추가] 75.45%는 teacher이자 실제 데이터 student 값. ImageNet 학습 시간은 보고 안 됨 (Table 2는 CIFAR만). (Table 1 p.7)
  - [추가] [실측·원논문] 가장 빠른 E=100 설정은 일부 쌍에서 기존 방법보다 낮음: CIFAR-100 ResNet-34→18 76.29 vs 77.41% (SpaceshipNet), Tiny-ImageNet 61.71 vs 64.04%. E=300도 CIFAR-10 ResNet-34→18은 95.21 vs 95.39%. (Table 1 p.7)
  - [추가] 옮겨 간 비용 추가: 합성 이미지 메모리 버퍼 (기본은 전부 보관). 버퍼 5k에서 90.41%, 전부 보관 91.94% (Table 5 p.8). 저장 용량은 보고 안 됨.
  - [수정] 3회 평균·표준편차는 부록 Table 14의 E=300 CIFAR 6개 설정에만 있음. 본문 표는 단일 실행 값. (Table 14 p.11) ← 메모: 3회 평균 (부록)
  - [수정] 평균 외 평가: 생성기 수렴 시간과 다양성 (Table 4, 12), 데이터 없는 양자화 (Table 3 p.7), NYUv2 분할 mIoU (부록 Table 11 p.11), LTE t-SNE (p.12). ← 메모: 평균 외 평가: 다양성 지표 (Table 4). 그 외 없음
  - [수정] 전체 시간 보고는 CIFAR 분류 (Table 2)에 한정. NYUv2 분할 (Table 11)은 합성 시간만 보고해 11번과 같은 경계. (p.11) ← 메모: 메모 (계속): 11번과 달리 합성 + 증류 전체 시간 보고
- 메모: 같은 Table 2에서 기존 FM(E=100)은 2.23시간에 67.15%로, 제안 방법과의 시간 차이는 작음. 배수는 비교 대상(DeepInv)과 epoch 예산에 따라 달라짐. 클래스 임베딩에 쓰는 사전학습 텍스트 인코더(CLIP)의 비용은 계산에 없음.
- 메모 (계속): 11번과 달리 합성 + 증류의 전체 시간을 보고함. 두 논문의 "배수"는 분모(합성만 대 전체)와 하드웨어가 달라 직접 비교할 수 없음.

## 축 분류 메모

- 팀 Notion 정리(KD - Substitute)의 구분을 따르면 세 갈래: (1) teacher의 역할을 줄이거나 없앰 (04 Tf-KD, 05 BYOT, 06 DML, 07 Snapshot), (2) teacher를 일부러 약하게 만듦 (01 체크포인트, 02 조기 종료, 03 경량 teacher), (3) 데이터를 바꿔 teacher를 조정함 (08 Proteus, 09 SRe2L, 10 RDED, 11 FastDFKD, 12 NAYER). 공통 전제는 "teacher가 정확하다고 KD가 잘 되는 것은 아니다".
- 02번 On the Efficacy of KD는 ESKD(Skip-구간)와 일찍 멈춘 teacher(Substitute)를 함께 제안함.
- 05번 Be Your Own Teacher, 06번 Deep Mutual Learning, 07번 Snapshot Distillation은 "별도 teacher 없음" 계열. teacher 준비 비용은 0이지만 학습 중 추가 연산(보조 분류기, 동료 네트워크, 이전 스냅샷 forward)이 남음.
- 09–12번(데이터 응축, 데이터 없는 증류)은 절감 대상이 "원본 데이터 접근"이나 "합성 시간"이고, 일반 KD 대비 student 학습 비용을 줄였다는 수치는 없음. 비교표에서 분모를 따로 표시해야 함.
- teacher 준비(학습) 비용을 숫자나 그림으로 비교에 넣은 논문: 01번(그림), 05번(시간), 07번(추가 1/3). 나머지는 넣지 않음.
