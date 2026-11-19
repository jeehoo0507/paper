# 2_Skip 메모

Skip: 샘플, teacher 호출, 학습 구간을 건너뜀  
논문 목록과 PDF: [`../Related Work/Efficient KD papers/2_Skip/`](<../Related Work/Efficient KD papers/2_Skip>)  
작성 규칙과 항목 설명: [README](README.md)

담당: Claude 초안 (2026-11-18). 저장소 PDF 본문을 직접 읽고 채움. 사람이 다시 대조하지 않음.

## 읽기 전에

- 수치는 저장소 PDF의 표·본문에서 옮겼고, 「위치」의 쪽 번호는 그 PDF의 쪽(첫 장 = 1)임.
- 「잃는 것」 표시는 `1_Shrink.md`와 같음. **[실측·원논문]** = 논문이 보고한 관찰, **[구조]** = 방법 정의에서 나오는 제약.
- "보고 안 됨" = 논문에 해당 값이 없음. "확인 안 함" = 논문에 있을 수 있으나 이번에 읽지 않은 부분.
- 인용 전에 「위치」의 표를 한 번 더 확인할 것.

### 01. Distill on a Diet ([arXiv: 2606.25488](https://arxiv.org/abs/2606.25488))
- 효율화 방식: 학습 데이터의 일부만 써서 KD함. IF-Beta: 사전학습된 teacher만으로 영향 함수 점수를 계산하고, Beta 분포로 표현한 표본 추출 정책을 이중 최적화로 학습해 부분집합을 고름. 안쪽 루프는 teacher 특징 공간에서의 대리 학습, 바깥 루프는 정책 갱신. 학습 궤적 통계를 얻기 위한 재학습이 필요 없음.
- 얻는 것:
  - 전체 데이터 KD의 최종 정확도를 넘을 때까지 쓴 상대 비용 (전체 데이터 표준 학습 일정 대비, 선택 과정 포함): CIFAR-10 30% 제거 0.806배, CIFAR-100 10% 제거 0.845배, ImageNet 50% 제거 0.915배. (Table 2)
  - ImageNet ResNet-34 → MobileNet, 70% 유지: 19.24시간에 71.20%. 비교 대상은 KD 없는 지도학습 Baseline 39.86시간·69.57%, Medium-Difficulty* 36.98시간·70.92%. (Table 3)
- 잃는 것:
  - [실측·원논문] 제거율이 낮으면 손실 없음: ImageNet ResNet-50 → ResNet-50, 50% 유지 74.02% vs 전체 데이터 73.54%. (4.1절)
  - [실측·원논문] 제거율이 높으면 크게 떨어짐: ImageNet ResNet-101 → ResNet-50, 90% 제거 60.90%, 80% 제거 68.54% vs 전체 데이터 73.43%. (Table 12)
  - [구조] 영향 함수 추정과 대리 학습에 teacher의 gradient와 특징이 필요함. logit만 내주는 teacher에 그대로 쓸 수 있는지는 논문에 없음.
- 옮겨 간 비용: 영향 함수 점수 계산과 정책 학습(이중 최적화). Table 2의 상대 비용에는 포함했다고 명시. 선택 과정만의 절대 비용은 보고 안 됨. 다른 논문의 측정값: ImageNet 90% 제거에서 IF-Beta 선택 비용 71.41 PFLOPs, 5,370초 (03번 논문 Table 27).
- 어떻게 쟀나: 단위 = 상대 비용(목표 정확도 도달까지의 반복 수 비율), 실제 시간(h) / teacher forward 셈? Table 2는 반복 수 기준이라 간접 반영. Table 3 시간에 logit 사전 계산과 선택 시간이 들어갔는지는 명시 없음 / 일회성 비용 셈? 선택 비용 Y (Table 2), teacher 학습 N / 하드웨어 = NVIDIA A100 1장 (Table 3)
- 설정: CIFAR-10/100은 ResNet-18 → ResNet-18, batch 128, 40K 반복(전체 데이터 기준 약 200 epoch). ImageNet은 ResNet-50 → ResNet-50, 300K 반복(약 30 epoch). teacher를 바꾼 실험: ResNet-101, WideResNet-50-2, ViT-Base → ResNet-50. 3 seed 평균. 제거율과 무관하게 반복 수를 고정함.
- 평균 정확도 외 평가: 없음
- 위치: Fig. 4 (p. 10), Table 1 (p. 11), Table 2·3 (p. 12), Table 12와 부록 C.2 (p. 29)
- 메모:
  - 반복 수를 고정하므로 데이터를 줄여도 학생 업데이트 수는 그대로임. Table 2의 절감은 "목표에 더 일찍 도달"에서 나오고, 학습 일정 자체를 줄인 것이 아님.
  - Table 3의 19.24시간은 데이터 30% 제거와 teacher logit 1회 계산 후 재사용(부록 C.2)이 합쳐진 값임. Baseline도 KD 없는 지도학습임. Skip만의 절감으로 읽으면 안 됨.
  - 논문 각주 5: 반복 수가 같으면 작은 부분집합은 샘플당 반복이 늘어 그것만으로 일반화가 좋아질 수 있음. 그래서 무작위 대조(Table 1)를 같이 봐야 함.

### 02. Distilling the Knowledge in Data Pruning ([arXiv: 2403.07854](https://arxiv.org/abs/2403.07854))
- 효율화 방식: 데이터 프루닝으로 남긴 부분집합에서 학습하되, 전체 데이터로 학습한 teacher의 soft label로 KD를 함께 함. 선택 방법은 기존 방법(forgetting, EL2N, GraNd, memorization)과 클래스 균형 무작위를 비교. teacher와 student 구조가 같은 자기 증류가 기본.
- 얻는 것: 연산·시간 절감 수치는 보고 안 됨. 보고한 것은 같은 데이터 비율에서 KD를 붙였을 때의 정확도 이득 (KD 없는 학습 대비):
  - CIFAR-100, 10% 유지, 무작위 선택: 39.8 → 56.8%. (1절)
  - ImageNet, 10% 유지, top-5: 무작위 82.37 → 87.19%, EL2N 62.47 → 82.47%. (1절)
  - ImageNet에서 무작위 30% + KD가 전체 데이터·KD 없음 기준선과 비슷하고, 70%는 +1.6%p. (4.1절)
- 잃는 것:
  - [실측·원논문] 유지 비율이 낮을수록 정확도가 떨어짐. 전체 데이터 KD 대비 하락폭은 그림으로만 제시. (Fig. 4·5)
  - [실측·원논문] 유지 비율이 낮으면 점수 기반 선택이 무작위보다 나쁨 (CIFAR-100 f ≤ 0.4, SVHN f ≤ 0.08). (4.1절)
  - [실측·원논문] 유지 비율이 낮으면 큰 teacher가 정확도를 떨어뜨리고, student보다 작은 teacher가 더 나을 수 있음. (Fig. 7, 4.3절)
- 옮겨 간 비용: 전체 데이터로 teacher를 학습하는 비용 (자기 증류라 student 1회 학습과 같은 규모). 점수 기반 선택은 전체 데이터로 모델을 학습해 점수를 계산함 (ImageNet EL2N은 모델 20개 평균 점수 사용, 부록 6.1). 둘 다 숫자 보고 안 됨.
- 어떻게 쟀나: 단위 = 유지 데이터 비율 f (비용 측정 없음) / teacher forward 셈? N / 일회성 비용 셈? N / 하드웨어 = 보고 안 됨
- 설정: SVHN·CIFAR-10/100은 ResNet-32 → ResNet-32, 240 epoch, batch 64. ImageNet은 ResNet-50 → ResNet-50, 240 epoch, batch 128. teacher 크기 실험은 ResNet-14~110, WRN-40-2 → ResNet-32·ResNet-20. 손실 (1−α)·CE + α·KD, 기본 α = 0.5.
- 평균 정확도 외 평가: 없음
- 위치: Fig. 4·5 (p. 6), Fig. 6·Table 1 (p. 7), Fig. 7 (p. 8), 부록 6 (p. 11)
- 메모:
  - "무작위 + KD"가 필수 대조군이라는 근거. 다만 비용 주장은 없고 f만 보고함.
  - 유지 비율이 낮을수록 KD 가중치 α를 키우는 것이 좋다고 보고 (Fig. 6). 데이터 예산을 바꿀 때 α를 고정하면 불리한 비교가 될 수 있음.
  - 01번은 반복 수 고정, 이 논문은 epoch 수 고정이라 "데이터를 줄였다"의 의미가 다름. 여기서는 학생 업데이트 수가 f에 비례해 줄어듦.

### 03. When Less Data Favors Smaller Teachers ([arXiv: 2609.34489](https://arxiv.org/abs/2609.34489))
- 효율화 방식: KD용 데이터 선택. DVA: 작은 teacher를 대리 모델로 써서 (1) 데이터 예산에 맞는 난이도 구간의 샘플만 남기고 (2) 클래스별로 teacher 출력의 관계 정보가 겹치지 않도록 부피를 최대화하는 샘플을 고름. 학습 궤적 통계가 필요 없음.
- 얻는 것:
  - 선택 비용 (IF-Beta 대비, ImageNet 90% 제거): 71.41 → 2.81 PFLOPs (25.4배 감소), 5,370초 → 94초 (57.1배). (Table 27)
  - 같은 데이터 비율에서의 정확도 (무작위 선택 대비): CIFAR-100 ResNet-34 → ResNet-18, 90% 제거 46.93 → 53.76%. ImageNet ResNet-34 → ResNet-18, 90% 제거 50.75 → 52.47%. (Table 2·5)
  - 데이터를 줄여 얻는 student 학습 시간 절감은 보고 안 됨.
- 잃는 것:
  - [실측·원논문] 제거율이 높으면 크게 떨어짐: ImageNet 10% 제거 71.60% → 90% 제거 52.47%. CIFAR-100 79.13 → 53.76%. (Table 5·2)
  - [실측·원논문] 학습 궤적 기반 방법보다 낮은 구간이 있음: CIFAR-100 90% 제거에서 DUAL 55.56% vs DVA 53.76%. (Table 2)
  - [구조] 적절한 저용량 대리 teacher가 있어야 하고, 제거율과 난이도 구간의 대응을 미리 정해 둠. (부록 A)
- 옮겨 간 비용: 대리 teacher 준비와 선택 연산. 선택 연산은 숫자로 보고함 (Table 27, 공개 사전학습 모델을 대리로 써서 대리 학습 비용이 0인 조건). 대리 teacher를 따로 학습할 때의 비용은 FLOPs 비율로만 언급 (폭 8 대리는 폭 64 teacher의 약 1/63.3, p. 18).
- 어떻게 쟀나: 단위 = 선택 비용 PFLOPs와 실제 시간(초). student 학습 비용은 제거율로만 표시 / teacher forward 셈? 선택 단계의 대리 모델 forward는 Y, KD 학습 중 teacher forward는 N / 일회성 비용 셈? 선택 비용 Y, teacher·대리 teacher 학습 N / 하드웨어 = RTX 4090
- 설정: CIFAR-100은 ResNet-34(폭 64) → ResNet-18, 240 epoch, batch 64, 10 seed. ImageNet은 ResNet-34(timm) → ResNet-18, 대리 ResNet-10, 100 epoch, batch 512, 3 seed. Tiny-ImageNet, ViT-S → ViT-Ti(CIFAR-100)도 있음.
- 평균 정확도 외 평가: 없음
- 위치: Table 2 (p. 7), Table 5 (p. 9), 부록 A (p. 14), 부록 D (p. 15, 18), Table 27 (p. 30)
- 메모:
  - Substitute 축과 겹침. 데이터가 적을수록 작은 teacher가 유리하다는 분석이 본론 (Table 11–14).
  - 01번 IF-Beta의 선택 비용을 다른 연구가 잰 수치가 여기 있음.
  - 선택 비용만 재고, 선택 + 학습을 합친 전체 비용은 재지 않음.

### 04. Few Sample KD (FSKD) ([arXiv: 1812.01839](https://arxiv.org/abs/1812.01839))
- 효율화 방식: 프루닝·분해로 압축한 student의 각 블록 끝에 1×1 conv를 붙이고, 라벨 없는 소수 샘플로 teacher의 블록 출력에 맞도록 그 1×1 conv만 최소제곱으로 추정함. 역전파 미세조정을 대체. 추가한 층은 앞 conv에 합쳐져 추론 비용이 늘지 않음.
- 얻는 것 (압축 후 정확도 복구 단계, 전체 데이터 미세조정 대비, VGG-16·CIFAR-10):
  - Prune-A: 100장·4.8초에 92.37% vs 5만 장·1059.6초에 92.54%.
  - Prune-C: 100장·7.4초에 89.55% vs 5만 장·2592.3초에 90.77%. (Table 2)
- 잃는 것:
  - [실측·원논문] 100장 기준 전체 미세조정보다 0.17 / 1.36 / 1.22%p 낮음 (Prune-A / B / C). 500장이면 0.08 / 0.32 / 0.36%p. (Table 2)
  - [구조] student가 teacher와 블록 단위로 대응해야 함 (teacher를 압축해 만든 student 전제).
- 옮겨 간 비용: teacher를 프루닝·분해해 student를 만드는 과정. Prune-C는 프루닝과 FSKD를 2회 반복. 숫자 보고 안 됨.
- 어떻게 쟀나: 단위 = 실제 시간(초), 샘플 수, student FLOPs / teacher forward 셈? 시간에 포함됐는지 명시 없음 / 일회성 비용 셈? N / 하드웨어 = Intel i7-7700K, GTX 1080 Ti 1장
- 설정: VGG-16 → 필터 프루닝한 VGG-16 (CIFAR-10). network slimming, network decoupling으로 만든 student도 실험. epoch 개념 없음 (블록별 최소제곱).
- 평균 정확도 외 평가: 없음. 층별 출력 상관 분석은 있음 (Fig. 4).
- 위치: Table 1·2, Fig. 3·4 (p. 5), 하드웨어 (p. 4)
- 메모: 일반 KD의 학습 비용이 아니라 "압축 후 복구" 비용을 줄이는 논문. 분모가 전체 데이터 미세조정이라 다른 Skip 논문과 같은 표에 넣기 어려움. 데이터가 1% 미만인 극단 사례로만 인용.

### 05. PRACTISE ([arXiv: 2202.07861](https://arxiv.org/abs/2202.07861))
- 효율화 방식: 소수 이미지(예: 500장)만으로 네트워크를 가속함. 필터 프루닝 대신 블록을 통째로 제거하고, 제거할 블록은 "복구 가능성"(그 블록을 뺀 뒤 소량 데이터로 얼마나 회복되는가)으로 고름. 회복은 원 모델을 teacher로 한 특징 증류.
- 얻는 것: 절감 대상은 추론 지연이고 학습 비용이 아님. ResNet-34, ImageNet, 500장: 지연 41.7 → 32.5 ms (−22.1%)에서 top-1 71.75% (이전 방법 MiR 68.14%). (Table 4)
- 잃는 것:
  - [실측·원논문] 원 모델 73.31% 대비 지연 −22.1%에서 −1.56%p, −8.2%에서 −0.27%p. (Table 4)
  - [실측·원논문] 지연 감소가 크면 소량 데이터로 회복하기 어렵고 데이터 양에 따른 격차가 커짐. (4.3절)
  - [구조] 인식 과제에 한정. (결론, p. 8)
- 옮겨 간 비용: 블록마다 복구 가능성을 재는 시험 학습 5.8분 × 12, 지연 측정 0.4분 × 12, 미세조정 11.3분, 합계 85.7분. 숫자로 보고함. (Table 10)
- 어떻게 쟀나: 단위 = 추론 지연(ms), 학습 시간(분), 샘플 수 / teacher forward 셈? Y (전체 과정 실측 시간) / 일회성 비용 셈? 선택 비용 Y, 원 모델 학습 N / 하드웨어 = NVIDIA TITAN Xp 1장
- 설정: ResNet-34(원 모델이 teacher) → 블록을 뺀 ResNet-34, ImageNet-1k 50~1000장. MobileNetV2, ResNet-50도 실험. 데이터 없는 설정과 도메인 밖 데이터 설정 포함.
- 평균 정확도 외 평가: 도메인 밖 학습 데이터로 회복했을 때의 정확도 (Table 7), 합성 데이터 (Table 6). 테스트 쪽 분포 이동·그룹 평가는 없음.
- 위치: Table 4 (p. 7), Table 6·7 (p. 8), Table 10 (p. 11)
- 메모: "효율"이 배포 모델의 지연이고 KD는 회복 수단임. 효율 KD 비교표에 넣으면 분모가 다름. 다만 선택 비용을 분 단위로 항목별 보고한 드문 사례라 비용 보고 예시로 쓸 만함. FLOPs 대신 지연을 봐야 한다는 주장(1절, Fig. 2)은 비용 범위 논의에 인용 가능.

### 06. UNIX (Computation-Efficient KD via Uncertainty-Aware Mixup) ([arXiv: 2012.09413](https://arxiv.org/abs/2012.09413))
- 효율화 방식: 배치 전체를 student에 먼저 통과시켜 불확실성을 재고, 불확실한 샘플 위주로 mixup해 teacher에 보내는 이미지 수를 줄임. teacher 호출이 줄고 student forward가 조금 늘어남.
- 얻는 것 (일반 KD의 backbone 순·역전파 연산 대비):
  - CIFAR-100, 구조가 다른 쌍 평균: 79.44% 연산으로 정확도 +1.07%p. (Table 1)
  - CIFAR-100, 구조가 비슷한 쌍 평균: k=48에서 92.40% 연산으로 +0.49%p, k=36에서 73.65% 연산. (Table 2, p. 6)
  - ImageNet ResNet-18 → ShuffleNetV2×0.5: 77.23% 연산으로 top-1 45.16 → 45.11% (라벨 미사용), 53.53 → 53.64% (라벨 사용). (Table 4)
- 잃는 것:
  - [실측·원논문] 구조가 비슷한 쌍, k=36에서는 KD보다 낮은 경우가 있음: WRN-40-2 → WRN-40-1 73.95 → 73.51%, resnet56 → resnet20 70.94 → 70.06%. (Table 2)
  - [실측·원논문] teacher와 student의 연산 차이가 작으면 절감이 작음. teacher/student 연산비 3~6인 쌍에서 k=48의 연산은 87~95%. (Table 2)
- 옮겨 간 비용: 불확실성 계산을 위한 student forward 추가. 연산 비율에 포함해 보고함.
- 어떻게 쟀나: 단위 = KD 대비 연산 비율(%). teacher forward, student forward, student backward의 FLOPs에 각 단계를 통과한 샘플 수를 곱해 합함 (식 2–3). 손실 계산 비용은 제외 / teacher forward 셈? Y / 일회성 비용 셈? N / 하드웨어 = TITAN X Pascal (CIFAR 1장, ImageNet 4장·8장). 실제 시간은 보고 안 됨.
- 설정: CIFAR-100, 240 epoch, batch 64. resnet32×4 → ShuffleV2 등 구조가 다른 쌍과 WRN-40-2 → WRN-16-2 등 비슷한 쌍. ImageNet ResNet-18 → ShuffleV2×0.5, batch 256. Tiny-ImageNet. 4회 평균.
- 평균 정확도 외 평가: 클래스별 분석 있음. 불확실성 선택이 어려운 클래스에서 샘플을 더 고르는지와 클래스별 정확도를 봄 (Fig. 5). 분포 이동·그룹 평가는 없음.
- 위치: Fig. 1 (p. 1), 식 2–3·Fig. 2 (p. 3), Table 1 (p. 5), Table 2–4 (p. 6), Fig. 5 (p. 8), 부록 A (p. 11)
- 메모:
  - 비용을 (teacher 호출 수 × teacher forward) + (student forward 수 × student forward) + (student backward 수 × student backward)로 나눠 쓴 식이 우리 비용식과 같은 구조임. 선행 사례로 인용.
  - teacher/student 연산비가 작으면 절감이 사라진다는 관찰은 "teacher 비중이 절감의 상한"이라는 주장과 직접 연결됨.

### 07. Active Mixup ([arXiv: 2003.13960](https://arxiv.org/abs/2003.13960))
- 효율화 방식: 블랙박스 teacher에 대한 질의 수와 필요한 실제 이미지 수를 줄임. 소수의 라벨 없는 이미지를 mixup해 합성 후보 풀을 만들고, student가 가장 불확실해하는 후보만 골라 teacher에 질의함.
- 얻는 것 (전체 데이터 KD 대비):
  - CIFAR-10: 질의 50,000 → 40,000회, 실제 이미지 50,000 → 2,000장.
  - Places365-Standard: 질의 180만 → 48만 회, 실제 이미지 180만 → 8만 장. (Table 1)
- 잃는 것: [실측·원논문] 전체 데이터 KD 대비 정확도 하락: CIFAR-10 80.01 → 74.60%, Places365 49.01 → 45.71%, Fashion-MNIST 87.79 → 80.90%. (Table 1)
- 옮겨 간 비용: 합성 후보 풀 생성, 후보 전체에 대한 student 추론(불확실성 계산), 능동 학습 반복마다의 student 재학습. 숫자 보고 안 됨.
- 어떻게 쟀나: 단위 = teacher 질의 수, 실제 이미지 수 / teacher forward 셈? Y (질의 수로) / 일회성 비용 셈? N / 하드웨어 = 보고 안 됨. 연산량과 시간도 보고 안 됨.
- 설정: Places365-Standard는 ResNet-18 teacher (53.68%), MNIST·Fashion-MNIST는 LeNet-5 teacher, CIFAR-10. student 구조와 epoch은 확인 안 함 (4.1절).
- 평균 정확도 외 평가: 도메인 밖 이미지로 증류했을 때의 정확도 (Table 4·5). 그 외 없음.
- 위치: Table 1 (p. 6), Table 2·3·Fig. 4 (p. 7), Table 4·5 (p. 8)
- 메모: 비용 단위가 질의 수뿐이라 호출당 비용이나 student 쪽 추가 연산과 합칠 수 없음. CIFAR-10에서 질의는 20%만 줄고(5만 → 4만) 정확도는 5.4%p 떨어짐. 이 논문의 절감은 질의 수보다 실제 이미지 수 쪽에 있음.

### 08. Comparative KD ([arXiv: 2311.02253](https://arxiv.org/abs/2311.02253))
- 효율화 방식: teacher 추론 횟수가 n회로 제한된 설정에서 teacher를 더 부르지 않고 감독 신호를 늘림. 샘플 쌍(또는 k개 묶음)에 대한 teacher 표현의 차이를 student가 맞추도록 학습함. teacher 출력을 저장해 두고 여러 조합으로 다시 씀.
- 얻는 것 (같은 teacher 호출 수 n에서 일반 KD 대비 정확도, CIFAR-100 WRN-40-2 → WRN-16-2): n=1600에서 26.09 → 36.38%, n=3200에서 43.05 → 50.14%. (Table 1) 연산·시간 절감 수치는 보고 안 됨.
- 잃는 것:
  - [실측·원논문] 호출 수가 수천 회 수준이라 절대 정확도가 낮음 (n=3200에서 50.14%). 전체 데이터 KD 기준선은 Table 1에 없음.
  - [구조] teacher 표현을 저장해 두어야 함.
- 옮겨 간 비용: teacher 출력 저장과 쌍·묶음 조합 계산. 숫자 보고 안 됨.
- 어떻게 쟀나: 단위 = teacher 호출 수 n / teacher forward 셈? Y (횟수로) / 일회성 비용 셈? N / 하드웨어 = 12 GB GPU 1장 (주로 RTX 2080 Ti). 실행 1회가 20분(n=1600)~40분(n=4800)이라고만 적고 방법 간 시간 비교는 없음.
- 설정: CIFAR-100. WRN-40-2 → WRN-16-2, VGG13 → VGG8, ResNet110 → ResNet32. teacher는 CRD 저장소의 학습된 모델. n = 1600~4800.
- 평균 정확도 외 평가: 클래스 logit 간 상관이 teacher와 얼마나 맞는지 (Table 4), 표현 공간 분석 (Fig. 4). 그룹·분포 이동 평가는 없음. teacher의 편향이 student로 전달되는지를 향후 과제로 적음 (p. 10).
- 위치: Table 1 (p. 5), Table 2·3 (p. 7), Table 4·Fig. 4 (p. 9), 결론 (p. 10)
- 메모: teacher 호출 수가 곧 사용 샘플 수라서 Skip(데이터)과 Reuse(teacher 출력 저장 후 재조합)의 경계에 있음. 비용 단위가 호출 수뿐임.

### 09. Weak-to-Strong KD Accelerates Visual Learning ([arXiv: 2604.15451](https://arxiv.org/abs/2604.15451))
- 효율화 방식: student보다 약한 teacher를 고정해 두고 학습 초반에만 KD를 걸고, student가 teacher 수준을 넘으면 끔. 목적은 강한 student의 학습 가속.
- 얻는 것 (KD 없는 기준 학습 대비, 목표 정확도 τ에 처음 도달하는 epoch의 비):
  - ImageNet ResNet-18 → ResNet-50, τ = 65%: SGD 1.62배, AdamW 2.86배, Muon 4.75배.
  - CIFAR-100 최대 1.83배. (Table 1)
  - COCO 검출 1.67배, CIFAR-10 확산 생성 2.5배 (스텝 기준). (Table 2, 초록)
- 잃는 것:
  - [실측·원논문] 최고 정확도는 기준과 거의 같지만 낮아진 경우도 있음: ImageNet ViT-S/16 (teacher MobileNetV2) 75.87 → 75.36%, CIFAR-10 ResNet-50 SGD 95.65 → 95.08%. (Table 1)
  - [실측·원논문] teacher가 너무 약하거나 너무 강하면 이득이 거의 사라짐. (Table 3, Fig. 3)
  - [구조] 적당히 약한 teacher가 이미 있어야 하고 teacher 선택 단계가 필요함. (한계, p. 14)
- 옮겨 간 비용: KD 구간의 teacher forward (미니배치당 1회)와 약한 teacher의 학습 비용. 본문 표에는 둘 다 들어 있지 않음. teacher 재학습을 포함한 실제 시간은 보충 자료에 있다고 적었으나, 저장소 PDF(18쪽)에는 보충 자료가 없음.
- 어떻게 쟀나: 단위 = 목표 도달 epoch(또는 step)의 비 / teacher forward 셈? N / 일회성 비용 셈? N (본문 기준) / 하드웨어 = 보고 안 됨
- 설정: ImageNet ResNet-18·MobileNetV2 → ResNet-50, MobileNetV2 → ViT-S/16. CIFAR-100 DenseNet-40 → ResNet-18·DenseNet-100. CIFAR-10 MobileNetV2 → ResNet-50. 최적화기 SGD·AdamW·Muon. 총 epoch은 확인 안 함.
- 평균 정확도 외 평가: 다른 과제의 지표 (검출 AP50, 생성 FID). 분류에서의 그룹·분포 이동 평가는 없음.
- 위치: Table 1 (p. 8), Table 2·Fig. 2 (p. 9), Table 3·Fig. 3 (p. 10), 한계 (p. 14)
- 메모:
  - 4.8배는 epoch 기준이라 GPU 시간 배율로 옮기면 안 됨. KD 구간에서는 epoch당 시간에 teacher forward가 더해짐.
  - "목표 도달 시간"과 "최종 정확도"를 구분해야 하는 대표 사례 (5_Measurement 01번과 대비).
  - teacher를 싼 것으로 바꾼다는 점에서 Substitute와도 겹침.

### 10. DearKD ([arXiv: 2204.12997](https://arxiv.org/abs/2204.12997))
- 효율화 방식: 두 단계 학습. 1단계에서 CNN teacher의 앞쪽 중간층으로부터 귀납 편향을 ViT student에 증류하고, 2단계에서는 증류 없이 학습함. 목적은 데이터 효율 (적은 이미지로 ViT 학습).
- 얻는 것: 연산·시간 절감 수치는 보고 안 됨. 보고한 것은 데이터 효율 (ImageNet 일부만 사용, Table 2):
  - 10%: DeiT-Ti 40.5%, 증류한 DeiT-Ti 50.3%, DearKD-Ti 54.3%.
  - 25%: 61.1 / 64.3 / 67.1%.
  - 100%: 72.2 / 74.5 / 74.8%.
- 잃는 것:
  - [실측·원논문] 1단계 길이는 300 epoch 중 250일 때 최고(74.8%). 200이면 74.3%, 300 전부면 74.6%. (Table 7) teacher를 끄는 구간이 마지막 50 epoch(17%)뿐이라 teacher 호출 절감은 작음.
  - [구조] student 구조를 바꿔야 함 (블록 8개를 MHCA로 교체, head 수 증가). CNN teacher의 중간 특징이 필요함.
- 옮겨 간 비용: 1단계의 teacher forward (RegNetY-16GF)와 특징 정렬 모듈. 숫자 보고 안 됨.
- 어떻게 쟀나: 단위 = 학습 데이터 비율, epoch. 학습 비용은 재지 않음 (추론 처리량만 Table 3) / teacher forward 셈? N / 일회성 비용 셈? N / 하드웨어 = NVIDIA A100 8장 (학습), V100 (처리량 측정)
- 설정: RegNetY-16GF (82.9%) → DearKD-Ti/S/B (DeiT 기반), ImageNet-1k, 300 epoch (1단계 250) 또는 1000 epoch (1단계 800), batch 2048, DeiT 증강.
- 평균 정확도 외 평가: 다운스트림 전이 (Table 4), 실제 이미지 없이 합성 이미지로만 학습하는 설정. 그룹·분포 이동 평가는 없음.
- 위치: 5.1절·Table 1·2 (p. 6), Table 3 (p. 7), Table 4 (p. 8), Table 7 (p. 12)
- 메모:
  - 형태는 "초반만 KD"지만 동기는 비용이 아니라 데이터 효율과 정확도임. 비용 절감의 근거로 인용하면 안 됨.
  - 초반 KD 후 CE만으로 학습하는 절차의 원형은 ESKD (4_Substitute 02번, On the Efficacy of KD).

## 축 분류 메모

- 04번 FSKD, 05번 PRACTISE는 이미 압축된 student의 정확도 복구 비용을 다룸. 일반 KD 학습 비용과 분모가 다르므로 비교표에서는 따로 묶는 편이 안전함.
- 08번 Comparative KD는 teacher 출력을 저장해 재조합하므로 Reuse와 겹침. 09번 Weak-to-Strong은 약한 teacher를 쓰므로 Substitute와 겹침.
- 10편 중 KD 학습의 연산이나 시간을 teacher forward까지 포함해 잰 것은 06번 UNIX(연산 비율)와 01번 Distill on a Diet(반복 수 기준 상대 비용) 정도임. 실제 시간으로 KD 학습 전체를 비교한 논문은 01번의 Table 3 하나이고, 그 수치에는 logit 재사용이 섞여 있음.
