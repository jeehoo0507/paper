# 원문 대조 보고서: `notes/2_Skip.md` 01–10

- 대상: `$PAPER/notes/2_Skip.md` (195줄), 대조한 PDF: `$PAPER/Related Work/Efficient KD papers/2_Skip/01–10_*.pdf`
- 방법: pymupdf로 쪽별 텍스트 추출 (`verify/tools/skip_extract.py` → `verify/tools/skip_txt/NN.txt`). 그림 범례가 필요한 쪽은 이미지로 렌더링 (`verify/tools/skip_img/`).
- 쪽 번호는 PDF 첫 장을 1로 셈. "L"은 메모 파일의 줄 번호.
- 이미 있던 정정 줄 (06 L120, 09 L166, 10 L186)도 다시 대조함. 셋 다 원문과 맞음. 다만 06과 10은 정정 줄과 충돌하는 본문 줄이 메모에 남아 있음 (아래 참고).

---

## Skip-01 Distill on a Diet (IF-Beta)
- 본 판본: PDF에 arXiv 판본 표시 없음 (LNCS 형식, 36쪽, 메타데이터 제목 "Distill on a Diet: Efficient Knowledge Distillation via Learnable Data Pruning")
- 대조 결과: 일치 7건 / 수정 7건 / 추가 4건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | L19 "사전학습된 teacher만으로 영향 함수 점수 계산", 재학습 불필요 | 점수 전에 teacher를 검증 세트에서 F-SAM으로 미세조정함: "we fine-tune the pretrained teacher model θT on the validation set Dval using Sharpness-Aware Minimization"; 부록 B.2는 "1,000 iterations … batch size 128" | §3.1 p. 5; 부록 B.2 p. 24 | 수정 | 사전학습 teacher를 검증 세트에서 F-SAM으로 1,000회(batch 128) 미세조정한 뒤 영향 함수 점수를 계산함. 학습 궤적용 student 재학습은 필요 없음. (§3.1 p. 5, 부록 B.2 p. 24) |
| 2 | L21 Table 2 상대 비용 0.806× / 0.845× / 0.915×, 선택 과정 포함 | 값 일치. 캡션: "the total number of iterations IF–Beta actually consumes until surpassing … (including the pruning process)" / "the standard full–data training schedule" | Table 2 p. 12 | 일치 | — |
| 3 | L22 Table 3: 19.24 h·71.20% vs Baseline 39.86 h·69.57%, Medium-Difficulty* 36.98 h·70.92% | 값 일치. Baseline은 "standard supervised training of MobileNet … for 100 epochs without KD". 비교 대상 3개와 Medium-Difficulty*의 시간·정확도는 "directly cited from Chen et al." IF-Beta는 batch 256, α=0.3, 300K 반복(70% 부분집합 기준 약 85 epoch) | Table 3 p. 12; 부록 C.2 pp. 29–30 | 추가 | Table 3의 비교 대상 시간·정확도는 Chen et al.의 값을 그대로 인용함. IF-Beta 쪽은 batch 256, α=0.3, 300K 반복(70% 부분집합에서 약 85 epoch, 비교 대상은 100 epoch). (부록 C.2 pp. 29–30) |
| 4 | L24 ImageNet R50→R50, 50% 유지 74.02% vs 73.54% (4.1절), "제거율이 낮으면 손실 없음" | 값 일치. 단 약 30 epoch 예산 조건: "under a limited compute budget (300K iterations, approximately 30 epochs …)" | §4.1 p. 10; 부록 C.1 p. 25; Table 9 p. 26 | 수정 | 50% 제거까지는 손실 없음: ImageNet ResNet-50 → ResNet-50, 300K 반복(전체 데이터 기준 약 30 epoch) 예산에서 50% 유지 74.02% vs 전체 데이터 73.54%. (§4.1 p. 10, Table 9 p. 26) |
| 5 | L25 R101→R50, 90% 제거 60.90%, 80% 제거 68.54% vs 73.43% (Table 12) | 값 일치. 단 이질 teacher 실험은 seed 1개: "conducted with a single random seed (seed = 0)" (각주 6) | Table 12 p. 29; 각주 6 p. 27 | 추가 | Table 12 값은 seed 1개(seed 0) 결과임. (각주 6 p. 27) |
| 6 | L26 [구조] teacher의 gradient와 특징 필요 | 영향 함수는 ∇θ̃T ℓ (teacher 파라미터에 대한 gradient), 안쪽 문제는 "frozen feature space"의 선형 분류기 | §3.1 p. 5; 부록 A p. 22 | 일치 | — |
| 7 | L27 "선택 과정만의 절대 비용은 보고 안 됨" | 절대값(초·FLOPs)은 없음. 다만 epoch 단위 추정은 있음: "the total cost of obtaining the coreset from a pretrained teacher … is well under four full epochs of training". 구성: FVM 1,000회, 검증·학습 세트 각 1회 gradient 패스, 바깥 루프 20회 × 안쪽 선형 분류기 1 epoch | 부록 A "Computational Complexity" p. 22; 부록 B.2 p. 24 | 수정 | 선택 과정의 절대 시간·FLOPs는 보고 안 됨. 저자 추정치만 있음: 사전학습 teacher에서 coreset을 얻는 비용이 "전체 학습 4 epoch보다 훨씬 적음". (부록 A p. 22) |
| 8 | L27 IF-Beta 선택 비용 71.41 PFLOPs, 5,370초 (03번 Table 27) | 03번 원문 값 일치. 단 5,370 s → 94 s는 Table 27이 아니라 §I 본문에 있음 (Table 27에는 PFLOPs만 있음) | 03번 §I·Table 27 p. 30 | 수정 | 다른 논문의 측정값: ImageNet 90% 제거에서 IF-Beta 선택 비용 71.41 PFLOPs (03번 Table 27), 5,370초 (03번 §I 본문, p. 30). |
| 9 | L28 단위 = 반복 수 비율 / teacher forward: Table 2는 간접 반영 / Table 3 시간에 logit 사전 계산·선택 시간 포함 여부 명시 없음 | CIFAR는 teacher logit을 매 반복 재계산 ("we recompute the teacher logits at every iteration"), ImageNet은 1회 계산 후 재사용 ("compute the teacher logits only once at the beginning"). 즉 Table 2의 ImageNet 행은 teacher forward가 반복마다 들지 않음. 선택 비용을 반복 수로 어떻게 환산했는지는 없음. Table 3: "even after including the cost of performing IF-Beta data pruning, our total training remains substantially lower" — 19.24 h 안에 포함됐다고는 쓰지 않음 | 부록 B.2 pp. 23–24; Table 2 p. 12; 부록 C.2 p. 30 | 수정 | teacher forward 셈? Table 2는 반복 수 비율이라 직접 세지 않음. CIFAR는 teacher logit을 매 반복 재계산, ImageNet은 학습 시작 때 1회 계산 후 재사용(부록 B.2 pp. 23–24). 선택 비용을 반복 수로 환산한 방법은 명시 없음. Table 3은 선택 비용을 더해도 우위라고만 적고, 19.24 h에 포함했는지는 명시 없음(부록 C.2 p. 30). |
| 10 | L28 하드웨어 A100 1장 (Table 3) | "using a single NVIDIA A100 GPU" | p. 12; 부록 C.2 p. 29 | 일치 | — |
| 11 | L29 CIFAR batch 128, 40K 반복(약 200 epoch) | 본문은 "batch size of 128 for 40K iterations (about 200 epochs)", 부록 B.2는 "40,000 iterations (about 200 epochs …) with batch size 256". 40K×128/50K ≈ 102 epoch, 40K×256/50K ≈ 205 epoch | §4 p. 10; 부록 B.2 p. 23 | 수정 | CIFAR batch는 본문 128(p. 10)과 부록 256(p. 23)이 서로 다름. "약 200 epoch"은 batch 256일 때만 맞음(128이면 약 102 epoch). |
| 12 | L29 ImageNet R50→R50, 300K 반복(약 30 epoch) | 일치 (batch 128). teacher R50은 300K 반복·batch 256(약 60 epoch)으로 직접 학습, 나머지 teacher는 PyTorch model zoo | §4 p. 10; 부록 B.2 p. 24 | 일치 | — |
| 13 | L29 teacher를 바꾼 실험: R101, WRN-50-2, ViT-B → R50 | ImageNet은 맞음. CIFAR에도 있음: ResNet-50/101, WideResNet-28-10 → ResNet-18 (Table 10–11) | 부록 B.2 p. 23; Tables 10–11 p. 28 | 추가 | CIFAR-10/100에서도 teacher를 ResNet-50, ResNet-101, WideResNet-28-10으로 바꿈 (student ResNet-18, Table 10–11 p. 28). |
| 14 | L29 "3 seed 평균" | 동질 쌍 본 실험은 seed {0,1,2}. 이질 teacher 실험(부록 C.1)은 seed 1개 | p. 10; 각주 6 p. 27 | 수정 | 동질 쌍 본 실험은 3 seed 평균(seed 0, 1, 2). 이질 teacher 실험은 seed 1개(각주 6 p. 27). |
| 15 | L29 제거율과 무관하게 반복 수 고정 | "all models (across all pruning ratios) are trained … for 40K iterations" | p. 10 | 일치 | — |
| 16 | L30 평균 정확도 외 평가: 없음 | 학생 쪽 그룹·분포 이동 평가는 없음. 점수 추정기 진단은 있음: 사후 점수와 EL2N·AUM의 Spearman 순위 상관 (Fig. 1) | Fig. 1 p. 6 | 추가 | 학생 쪽 그룹·분포 이동 평가는 없음. 점수 추정기 진단으로 EL2N·AUM과의 Spearman 순위 상관을 봄 (Fig. 1 p. 6). |
| 17 | L31 위치 Fig. 4 p. 10, Table 1 p. 11, Table 2·3 p. 12, Table 12·부록 C.2 p. 29 | 모두 일치. 부록 C.2는 p. 30까지 이어짐 | — | 일치 | (선택) 부록 C.2 (pp. 29–30) |
| 18 | L33–35 반복 수 고정 해석, 각주 5, Table 3이 logit 재사용과 섞임 | 각주 5: "With a fixed iteration budget, training on a smaller subset effectively increases the number of passes over each sample". C.2가 절감 요인 4개(데이터 30% 감소, 약 85 vs 100 epoch, 반복 teacher 질의 없음, 부분집합 품질)를 듦 | 각주 5 p. 11; 부록 C.2 pp. 29–30 | 일치 | (보강 선택) L34에 "학습 epoch도 약 85 vs 100으로 다름" 추가 |

---

## Skip-02 Distilling the Knowledge in Data Pruning
- 본 판본: PDF에 arXiv 판본 표시 없음 (15쪽, 보충 자료 포함)
- 대조 결과: 일치 11건 / 수정 2건 / 추가 2건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | L38 기존 선택 방법 4개 + 클래스 균형 무작위, 자기 증류가 기본 | "forgetting, GraNd, EL2N and 'memorization'" + "class-balanced random pruning". "self-distillation experiments … ResNet-32 … except for ImageNet … ResNet-50" | §4 p. 6; 부록 6 p. 11 | 일치 | — |
| 2 | L40 CIFAR-100 10% 무작위 39.8 → 56.8% | "accuracy improves by 17% (from 39.8% to 56.8%)" | §1 p. 2 | 일치 | — |
| 3 | L41 ImageNet 10% top-5 무작위 82.37 → 87.19, EL2N 62.47 → 82.47 | 일치 | §1 p. 2 | 일치 | — |
| 4 | L42 무작위 30%+KD ≈ 전체 데이터 무KD, 70%는 +1.6%p | "comparable accuracy can be achieved by randomly retaining just 30% … (e.g., +1.6% using a random subset of 70%)" | §4.1 p. 6 | 일치 | — |
| 5 | L44 "전체 데이터 KD 대비 하락폭은 그림으로만 제시 (Fig. 4·5)" | 그림의 점선 기준선은 범례상 "Full dataset (teacher)", 즉 전체 데이터로 학습한 teacher(자기 증류라 무KD 전체 데이터 모델)임. 전체 데이터 KD 값은 f = 1 끝점이 그려진 패널(CIFAR-100, ImageNet)에서만 읽힘. SVHN은 f ≤ 0.4, CIFAR-10은 f ≤ 0.7까지만 그림 | Fig. 4·5 p. 6 (렌더링 확인) | 수정 | 유지 비율이 낮을수록 정확도가 떨어짐. 그림의 점선 기준선은 전체 데이터 teacher(무KD)이고, 전체 데이터 KD 대비 하락폭은 f = 1 끝점이 있는 CIFAR-100·ImageNet 패널에서만 읽힘. 숫자 표는 없음. (Fig. 4·5 p. 6) |
| 6 | L45 낮은 f에서 점수 기반 < 무작위 (CIFAR-100 f ≤ 0.4, SVHN f ≤ 0.08) | "random pruning surpasses all other methods … both with and without KD" | §4.1 p. 7 | 일치 | (선택) "KD 유무와 무관" 추가, 위치 p. 7 |
| 7 | L46 낮은 f에서 큰 teacher가 해롭고, 더 작은 teacher가 나을 수 있음 | "the student benefits more from teachers with equal or even smaller capacities" | Fig. 7 p. 8; §4.3 pp. 7–8; 결론 p. 8 | 일치 | — |
| 8 | L47 teacher 학습 비용, 점수 계산 비용, 둘 다 숫자 보고 안 됨; ImageNet EL2N 20개 모델 평균 | "released variant of the scores which was averaged over 20 models". CIFAR·SVHN 점수는 DeepCore 기본 레시피와 ResNet-34로 계산 (student는 ResNet-32). 시간·FLOPs 없음 | 부록 6.1 p. 11 | 추가 | CIFAR·SVHN 점수는 DeepCore 기본 레시피로 ResNet-34에서 계산함 (student ResNet-32와 다른 구조). ImageNet forgetting 점수는 ResNet-50에서 계산, EL2N·memorization은 공개 점수 사용. (부록 6.1 p. 11) |
| 9 | L48 비용 측정 없음, teacher forward N, 하드웨어 보고 안 됨 | GPU·시간·FLOPs 언급 없음 (본문·보충 전체 검색) | 전체 | 일치 | — |
| 10 | L49 SVHN·CIFAR 240 epoch batch 64; ImageNet 240 epoch batch 128; 손실 (1−α)CE + αKD, α = 0.5 | 일치. 온도 τ = 4 | 부록 6.2 p. 11; 식 2 p. 3 | 일치 | — |
| 11 | L49 teacher 크기 실험: ResNet-14~110, WRN-40-2 → ResNet-32·ResNet-20 | 본문 Fig. 7은 CIFAR-100 RN32·RN20, CIFAR-10 RN32. 보충 Fig. 9에 SVHN RN8·RN32, CIFAR-100 RN56, CIFAR-10 RN20 student 추가 | Fig. 7 p. 8; Fig. 9 p. 12 | 추가 | teacher 크기 실험은 보충 Fig. 9에서 SVHN(ResNet-8·ResNet-32 student), CIFAR-100(ResNet-56), CIFAR-10(ResNet-20)까지 확장. (p. 12) |
| 12 | L50 평균 정확도 외 평가: 없음 | 학생 그룹·분포 이동 평가는 없음. 정성 진단 있음: teacher·student soft label 예시 (Fig. 2), forgetting 상위 점수 샘플 (Fig. 3) | Fig. 2 p. 4; Fig. 3 p. 5 | 수정 | 그룹·분포 이동 평가는 없음. 정성 진단만 있음: soft label 예시 (Fig. 2 p. 4), forgetting 최고 점수 샘플 (Fig. 3 p. 5). |
| 13 | L51 위치 | Fig. 4·5 p. 6, Fig. 6·Table 1 p. 7, Fig. 7 p. 8, 부록 6 p. 11 모두 일치 | — | 일치 | — |
| 14 | L54 낮은 f에서 α를 키우는 것이 좋음 (Fig. 6) | "lower pruning fractions favor higher values of α". f = 0.1에서 α = 1이 α = 0.5보다 8% 이상 높고, f = 0.7에서는 α = 0.5가 1% 이상 높음. SVHN은 Fig. 8 | §3.2 pp. 4–5; Fig. 6 p. 7; Fig. 8 p. 11 | 일치 | — |
| 15 | L55 epoch 고정이라 학생 업데이트 수가 f에 비례 | 240 epoch 고정은 원문. "비례"는 고정 epoch·batch에서 나오는 추론 | 부록 6.2 p. 11 | 일치 | (선택) "추론"으로 표시 |

---

## Skip-03 When Less Data Favors Smaller Teachers (DVA)
- 본 판본: PDF에 arXiv 판본 표시 없음 ("Preprint", 30쪽)
- 대조 결과: 일치 13건 / 수정 2건 / 추가 2건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | L60 선택 비용 71.41 → 2.81 PFLOPs (25.4배), 5,370 → 94 s (57.1배) (Table 27) | 값 일치. Table 27은 PFLOPs와 감소율만. 초 단위는 §I 본문: "decreasing from 5,370 s to 94 s (57.1×)" | §I·Table 27 p. 30 | 수정 | … 5,370초 → 94초 (57.1배). (PFLOPs는 Table 27, 초는 §I 본문, p. 30) |
| 2 | L61 CIFAR-100 R34 → R18 90% 제거 46.93 → 53.76; ImageNet 50.75 → 52.47 | 일치 (Random vs DVA) | Table 2 p. 7; Table 5 p. 9 | 일치 | — |
| 3 | L62 student 학습 시간 절감 보고 안 됨 | 학습 시간·FLOPs 없음 | 전체 | 일치 | — |
| 4 | L64 ImageNet 10% 제거 71.60 → 90% 52.47; CIFAR-100 79.13 → 53.76 | 일치 (DVA 행) | Table 5 p. 9; Table 2 p. 7 | 일치 | — |
| 5 | L65 학습 궤적 기반보다 낮은 구간: CIFAR-100 90% DUAL 55.56 vs 53.76 | 일치. 같은 열에서 D2 54.49, CCS 53.77도 DVA보다 높음. ImageNet 90%에서도 D2 52.79 > DVA 52.47, 10% 제거에서는 DUAL 71.67·IF-Beta 71.64 > DVA 71.60 | Table 2 p. 7; Table 5 p. 9 | 추가 | CIFAR-100 90% 제거에서 D2 54.49%, CCS 53.77%도 DVA 53.76%보다 높음. ImageNet 90% 제거에서 D2 52.79% > DVA 52.47%, 10% 제거에서 DUAL 71.67%·IF-Beta 71.64% > DVA 71.60%. (Table 2 p. 7, Table 5 p. 9) |
| 6 | L66 [구조] 적절한 저용량 대리 teacher 필요, 제거율-난이도 대응을 미리 정함 (부록 A) | "assumes access to a suitable low-capacity proxy teacher", "relies on a pre-defined mapping between pruning rate and preferred difficulty region" | 부록 A p. 14 | 일치 | — |
| 7 | L67 대리 학습 비용 0 조건; 폭 8 대리는 폭 64의 약 1/63.3 (p. 18) | "without any additional proxy training or adaptation" (p. 30). "about 1/63.3 of the FLOPs of the width-64 teacher for both training and inference" (p. 18). p. 3은 "approximately 63.3× fewer parameters" | §I p. 30; 부록 D.3 p. 18; p. 3 | 일치 | — |
| 8 | L68 teacher forward: 선택 단계 대리 forward Y, KD 중 teacher forward N; 일회성 비용 선택 Y | 선택은 "solely from fixed outputs of the pretrained proxy". KD 중 teacher 비용은 재지 않음 | §I p. 30 | 일치 | — |
| 9 | L68 하드웨어 RTX 4090 | "All experiments are conducted on NVIDIA GeForce RTX 4090 GPUs" (장수·§I 측정 장비 별도 명시 없음) | 부록 D.1 p. 15 | 일치 | (선택) "RTX 4090 (장수와 §I 측정 장비는 따로 명시 없음)" |
| 10 | L69 CIFAR-100 설정 (240 epoch, batch 64, 10 seed) | 일치. T = 4, CE와 KD 같은 가중치 | 부록 D.1 p. 15 | 일치 | — |
| 11 | L69 ImageNet R34(timm) → R18, 대리 R10, 100 epoch, batch 512, 3 seed | 일치. T = 1 | 부록 D.1 p. 15; §5 p. 9 | 일치 | — |
| 12 | L69 Tiny-ImageNet, ViT-S → ViT-Ti (CIFAR-100) | 일치 (Table 14, Table 20) | 부록 D.1 p. 15; Table 14 p. 22 | 일치 | — |
| 13 | (없음) 0.1 제거율 처리 | "No difficulty filtering is applied at a drop rate of 0.1" | 부록 D.5 p. 18 | 추가 | 제거율 0.1에서는 난이도 필터를 쓰지 않음. (부록 D.5 p. 18) |
| 14 | L70 평균 정확도 외 평가: 없음 | 그룹·분포 이동 평가는 없음. 있는 것: 클래스별 주성분 방향 해석 (§G.1, Fig. 5), 고른 부분집합을 다른 student(VGG-11, MobileNetV2)에 재사용하는 전이 실험 (§H.3, Table 25–26) | §G.1 pp. 23–24; §H.3 p. 29 | 수정 | 그룹·분포 이동 평가는 없음. 클래스별 주성분 방향 해석(§G.1, pp. 23–24)과, 고른 부분집합을 VGG-11·MobileNetV2 student에 그대로 쓰는 전이 실험(§H.3, Table 25–26, p. 29)은 있음. |
| 15 | L71 위치 | Table 2 p. 7, Table 5 p. 9, 부록 A p. 14, 부록 D p. 15·18, Table 27 p. 30 모두 일치 | — | 일치 | — |
| 16 | L73 작은 teacher 유리 분석 (Table 11–14) | 일치. Table 13 (ImageNet, teacher 깊이): R34는 0.1–0.5, R18은 0.7·0.8, R10은 0.9에서 최고라고 본문이 적음 | §F.2 pp. 21–22 | 일치 | — |
| 17 | L75 선택 + 학습 전체 비용은 재지 않음 | 맞음 | §I p. 30 | 일치 | — |

---

## Skip-04 Few Sample KD (FSKD)
- 본 판본: arXiv:1812.01839v3 (2020-03-31)
- 대조 결과: 일치 7건 / 수정 2건 / 추가 2건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | L78 블록 끝 1×1 conv만 라벨 없는 소수 샘플로 최소제곱 추정, 앞 conv에 병합 | 식 2, BCD, "merged into the previous conv-layer without introducing additional parameters and computation cost during inference", "few unlabeled samples" | §3 pp. 3–4 | 일치 | — |
| 2 | L80 Prune-A 100장·4.8 s·92.37% vs 50,000장·1059.6 s·92.54% | 일치 | Table 2 p. 5 | 일치 | — |
| 3 | L81 Prune-C 100장·7.4 s·89.55% vs 2592.3 s·90.77% | 일치 | Table 2 p. 5 | 일치 | — |
| 4 | L83 100장 −0.17/−1.36/−1.22%p, 500장 −0.08/−0.32/−0.36%p | 계산 일치 (Full fine-tuning A 92.54, B 91.53, C 90.77 기준) | Table 2 p. 5 | 일치 | — |
| 5 | L84 [구조] 블록 단위 대응 필요, teacher를 압축해 만든 student 전제 | 주 설정은 맞음. 단 Table 8은 무작위 초기화한 손설계 student(채널 절반)로도 실험: "what if student-nets are hand-designed with random initialization" (MNIST) | §5 Table 8 p. 8 | 수정 | [구조] student가 teacher와 블록(채널) 단위로 대응해야 함. 주 설정은 teacher를 압축해 만든 student이고, 채널을 절반으로 줄인 무작위 초기화 student도 MNIST에서 시험함 (Table 8 p. 8). |
| 6 | L85 student 생성 비용 숫자 보고 안 됨, Prune-C는 2회 반복 | "iteratively runs the pruning and FSKD procedure as described in Appendix-D for 2 iterations". 생성 시간 없음 | p. 5; 부록 D | 일치 | — |
| 7 | L86 단위·하드웨어 i7-7700K + 1080 Ti 1장, teacher forward 포함 여부 명시 없음 | "Intel i7-7700K CPU and one NVidia 1080TI GPU"; 5회 평균 "averaged over 5 trials of different randomly selected images" | p. 4 | 일치 | (선택) "5회 무작위 샘플 평균" 추가 |
| 8 | L87 설정: VGG-16 → 필터 프루닝 VGG-16 (CIFAR-10), slimming·decoupling 실험 | 더 넓음: slimming은 VGG-19·ResNet-164·DenseNet-40 (CIFAR-10/100), VGG-A(VGG-11) ImageNet 1,000장 (Table 5); decoupling은 VGG-16·ResNet-18 ImageNet (Table 6) | §4.1–4.2 pp. 6–7; Tables 3–6 | 추가 | network slimming은 VGG-19·ResNet-164·DenseNet-40 (CIFAR-10/100, Table 3–4)과 VGG-A ImageNet 1,000장 (Table 5), network decoupling은 VGG-16·ResNet-18 ImageNet (Table 6)에서 실험. |
| 9 | L88 평균 정확도 외 평가: 없음. 층별 상관 (Fig. 4) | Fig. 4 맞음. 추가로 Table 7: CIFAR-10 모델을 CIFAR-100 이미지로 복구 ("no statistical difference") | Fig. 4 p. 5; Table 7 p. 8 | 추가 | 도메인 밖 입력으로 복구하는 실험 있음: CIFAR-10 모델(Prune-B)을 CIFAR-100 이미지 100·500장으로 복구해도 정확도 같음 (90.15 vs 90.17%, Table 7 p. 8). 테스트 쪽 분포 이동 평가는 없음. |
| 10 | L89 위치 Table 1·2, Fig. 3·4 p. 5, 하드웨어 p. 4 | 일치 | — | 일치 | — |
| 11 | L90 "데이터 1% 미만인 극단 사례" | 100장 = CIFAR-10 학습 세트의 0.2%, 500장 = 1.0% | Table 2 p. 5 | 수정 | 데이터 1% 미만 사례(100장 = 0.2%)로만 인용. 500장은 1.0%임. |

---

## Skip-05 PRACTISE
- 본 판본: arXiv:2202.07861v2 (2023-03-12)
- 대조 결과: 일치 8건 / 수정 1건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | L93 블록 제거, 복구 가능성으로 선택, 원 모델을 teacher로 한 특징 증류 | 식 1 (adaptor로 원 모델 출력 맞춤), 식 4 (MiR식 특징 증류, GAP 앞 특징). 점수는 복구 가능성을 지연 감소율로 나눈 값: s(Bi) = R(Bi)/τ(Bi) | 식 1–4 pp. 4–5 | 추가 | 블록 점수 = 복구 가능성 손실 ÷ 지연 감소율 (식 3). 입력·출력 차원이 같은 블록만 후보. (pp. 4, 6) |
| 2 | L94 ResNet-34, 500장, 41.7 → 32.5 ms (−22.1%), 71.75% vs MiR 68.14% | 일치. 지연은 batch 64 입력 기준: "tested the model with the 64×3×224×224 input by 500 times and take the mean" | Table 4 p. 7; 구현 p. 6 | 추가 | 지연은 64장 batch 입력을 500회 통과시킨 평균 (TITAN Xp, p. 6). |
| 3 | L96 원 모델 73.31% 대비 −1.56%p (−22.1%), −0.27%p (−8.2%) | 71.75, 73.04 → 일치 | Table 4 p. 7 | 일치 | — |
| 4 | L97 지연 감소가 크면 회복 어렵고 데이터 양 격차 커짐 (4.3절) | 일치 | §4.3, Fig. 6 p. 7 | 일치 | — |
| 5 | L98 인식 과제 한정 (결론 p. 8) | "confined to recognition" | 결론 p. 8 | 일치 | — |
| 6 | L99 선택 비용 5.8분×12, 0.4분×12, 미세조정 11.3분, 합계 85.7분 | 일치 (4.8 + 69.6 + 11.3 = 85.7). "on one Titan Xp GPU". 데이터 양·대상 모델은 표에 명시 없음 (본문은 ResNet-34 지연 42 ms 언급) | Table 10, 부록 B p. 11 | 일치 | (선택) "데이터 양 명시 없음" |
| 7 | L100 teacher forward Y (전체 과정 실측), 일회성 선택 Y, 원 모델 학습 N, TITAN Xp 1장 | 일치 | p. 11 | 일치 | — |
| 8 | L101 ImageNet-1k 50~1000장, MobileNetV2·ResNet-50, 데이터 없는·도메인 밖 설정 | 일치 (Table 2: 50, 100, 500, 1000) | Table 2 p. 6; Tables 5–7 | 일치 | — |
| 9 | L102 도메인 밖 학습 데이터 (Table 7), 합성 데이터 (Table 6); 테스트 쪽 이동 평가 없음 | 일치. 추가로 Table 9: 학습 샘플 추출 방식(1-way N-shot vs uniform) 비교 | Tables 6–7 p. 8; Table 9 p. 11 | 추가 | 학습 샘플 추출 방식 비교 (1-way N-shot vs 균등, Table 9 p. 11)도 있음. 역시 학습 쪽 조건임. |
| 10 | L103 위치 Table 4 p. 7, Table 6·7 p. 8, Table 10 p. 11 | 일치 | — | 일치 | — |
| 11 | L104 FLOPs 대신 지연 주장 (1절, Fig. 2) | 일치 ("FLOPs-accuracy tradeoff is a mis-…", Fig. 2 p. 3) | p. 2; Fig. 2 p. 3 | 수정 | 위치를 "1절 (p. 2), Fig. 2 (p. 3)"으로 명시. |
| 12 | L94 "학습 비용이 아님" | 절감 대상은 배포 모델 지연 맞음 | p. 7 | 일치 | — |

---

## Skip-06 UNIX (Computation-Efficient KD via Uncertainty-Aware Mixup)
- 본 판본: arXiv:2012.09413v1 (2020-12-17)
- 대조 결과: 일치 8건 / 수정 3건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | L107 student에 배치 전체 통과, 불확실 샘플 mixup해 teacher 호출 감소 | 식 11 설명과 일치 | §3 p. 5 | 일치 | — |
| 2 | L109 구조가 다른 쌍 평균 79.44% 연산, +1.07%p (Table 1) | k = 48 조건. 6쌍 평균 연산 79.44% (계산 확인), 정확도 차 +1.06 (표 값 평균) ≈ 논문 1.07. k = 40은 66.94%·+0.29%p | Table 1 p. 5 | 추가 | 구조가 다른 쌍 평균 (k = 48): 79.44% 연산으로 +1.07%p. k = 40이면 66.94% 연산으로 +0.29%p. (Table 1 p. 5) |
| 3 | L110 비슷한 쌍 k = 48 92.40% +0.49%p, k = 36 73.65% | 연산 값 일치. +0.49는 논문 서술값이고 표의 6쌍 평균은 +0.475. k = 36의 −0.11%p도 표 평균은 −0.125 | Table 2 p. 6 | 일치 | (선택) "논문 서술값" 표시 |
| 4 | L111 ImageNet R18 → ShuffleV2×0.5, 77.23% 연산, 45.16 → 45.11 / 53.53 → 53.64 | 일치. batch 256 중 k = 192 | Table 4 p. 6 | 추가 | ImageNet은 batch 256 중 k = 192 (Random은 k = 200). (Table 4 p. 6) |
| 5 | L113 k = 36에서 KD보다 낮은 쌍: WRN-40-1 73.95 → 73.51, resnet20 70.94 → 70.06 | 일치. VGG13 → VGG8 73.50 → 73.18도 낮음 | Table 2 p. 6 | 수정 | … WRN-40-2 → WRN-40-1 73.95 → 73.51%, resnet56 → resnet20 70.94 → 70.06%, VGG13 → VGG8 73.50 → 73.18%. (Table 2) |
| 6 | L114 연산비 3~6인 쌍에서 k = 48 연산 87~95% | Ft/Fs 2.97–6.12, 연산 87.32–95.10% | Table 2 p. 6 | 일치 | — |
| 7 | L115 불확실성용 student forward 추가, 연산 비율에 포함 | 식 11 "(N + k)·Fs" | p. 5 | 일치 | — |
| 8 | L116 단위·식 2–3, 손실 계산 제외 | "The cost in backbone forward/backward pass is counted and the cost of computing loss is neglected" (Fig. 1 캡션) | Fig. 1 p. 1; 식 2–3 p. 3 | 일치 | — |
| 9 | L116 하드웨어 "CIFAR 1장, ImageNet 4장·8장" | CIFAR-100 1장, Tiny-ImageNet 4장, ImageNet 8장 (정정 줄 L120이 맞음) | 부록 C.3 p. 11 | 수정 | 하드웨어 = TITAN X Pascal (CIFAR-100 1장, Tiny-ImageNet 4장, ImageNet 8장). 실제 시간은 보고 안 됨. (부록 C.3 p. 11) |
| 10 | L117 설정: CIFAR-100 240 epoch batch 64, ImageNet batch 256, 4회 평균 | 일치. ImageNet 90 epoch, Tiny-ImageNet 100 epoch. "Average on 4 runs"는 Table 1–2 캡션에만 있음. 본 표는 hard label 손실을 뺀 설정: "we exclude the hard label loss in conventional KD for all the methods" | p. 5; 부록 C.3 p. 11 | 추가 | 본 표(Table 1–4)는 hard label 손실을 뺀 순수 KD 설정 (hard label 포함 결과는 부록 B). ImageNet 90 epoch, Tiny-ImageNet 100 epoch. 4회 평균은 Table 1–2에 명시. |
| 11 | L118 클래스별 분석 (Fig. 5), 분포 이동·그룹 평가 없음 | Fig. 5: resnet32×4 → resnet8×4, 불확실성 vs 무작위의 클래스별 샘플 수·정확도. Fig. 6은 epoch별 샘플링 패턴 | Figs. 5–6 p. 8 | 일치 | — |
| 12 | L119 위치 | Fig. 1 p. 1, 식 2–3·Fig. 2 p. 3, Table 1 p. 5, Table 2–4 p. 6, Fig. 5 p. 8, 부록 A p. 11 모두 일치 | — | 일치 | — |
| 13 | L120 정정: 식 11, 상한 주장 부적합, 하드웨어 | 식 11 "E = N·Fs + k·(Ft + Fs + Bs) = k·Ft + (N + k)·Fs + k·Bs" 일치. 하드웨어 일치 | 식 11 p. 5; 부록 C.3 p. 11 | 일치 | — |
| 14 | L123 메모 "연산비가 작으면 절감이 사라진다 → 'teacher 비중이 절감의 상한'과 직접 연결" | L120 정정과 모순. 원문은 Ft/Fs가 작으면 "the decrease of teacher forward pass is compensated by the increase at the student end"라고만 설명. student backward도 줄므로 teacher 비중이 상한이 아님 | p. 6 | 수정 | teacher/student 연산비가 작으면 teacher forward 감소분이 student forward 증가분에 상쇄되어 절감이 작아짐 (Table 2, p. 6). 이 방법은 student backward도 줄이므로 "teacher 비중이 절감 상한"의 근거로 쓰지 않음. |

---

## Skip-07 Active Mixup
- 본 판본: arXiv:2003.13960v1 (2020-03-31)
- 대조 결과: 일치 6건 / 수정 3건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | L126 블랙박스 teacher, mixup 후보 풀, student가 가장 불확실한 후보만 질의 | 일치. 원 이미지 쌍마다 합성 이미지 최대 1장만 남김: "we only keep one synthetic image for any pair (xi, xj)" | 알고리즘 1, §3.4 p. 4 | 추가 | 원 이미지 쌍마다 가장 불확실한 합성 이미지 1장만 후보로 남긴 뒤 순위를 매김. (식 3, p. 4) |
| 2 | L128 CIFAR-10 질의 50,000 → 40,000, 실제 이미지 50,000 → 2,000 | 일치. 질의 수에는 초기 student 학습용 실제 이미지 질의가 포함됨 (알고리즘 1 1행) | Table 1 p. 6 | 일치 | — |
| 3 | L129 Places365 질의 180만 → 48만, 이미지 180만 → 8만 | 일치 | Table 1 p. 6 | 일치 | — |
| 4 | (없음) MNIST·Fashion-MNIST 질의 수 | MNIST 60,000 → 24,000, Fashion-MNIST 60,000 → 48,000 질의, 실제 이미지 각 2,000장 | Table 1 p. 6 | 추가 | MNIST 질의 6만 → 2.4만 회, Fashion-MNIST 6만 → 4.8만 회, 실제 이미지 각 2,000장. (Table 1) |
| 5 | L130 정확도 하락 CIFAR-10 80.01 → 74.60, Places365 49.01 → 45.71, Fashion-MNIST 87.79 → 80.90 | 일치. MNIST는 99.05 → 98.74 | Table 1 p. 6 | 일치 | — |
| 6 | L131 옮겨 간 비용에 "능동 학습 반복마다의 student 재학습" | Table 1은 능동 학습 1회: "we run the active learning stage for only one step (i.e., T = 1)". 여러 회 반복은 Fig. 4 실험 (CIFAR-10 T = 10, Places365 6회) | §4.1.2 p. 5; Fig. 4 p. 7 | 수정 | 옮겨 간 비용: 합성 후보 풀 생성, 후보 전체에 대한 student 추론, student 재학습 (Table 1은 초기 학습 + 능동 1회, 다회 반복은 Fig. 4). 숫자 보고 안 됨. |
| 7 | L132 단위 질의 수·실제 이미지 수, 하드웨어·연산량·시간 보고 안 됨 | 맞음 (GPU·시간·epoch 언급 없음) | 전체 | 일치 | — |
| 8 | L133 Places365 teacher ResNet-18 (53.68%), MNIST·F-MNIST LeNet-5, CIFAR-10 (teacher 미기재) | 본문 53.68%, Table 1은 53.69%. CIFAR-10 teacher는 Table 1에서 AlexNet 83.07%, 절제·도메인 밖 실험은 VGG-16 93.31% | §4.1.1 pp. 4–5; Table 1 p. 6; §4.2 p. 6 | 수정 | Places365 teacher ResNet-18 (본문 53.68%, Table 1 53.69%), MNIST·Fashion-MNIST LeNet-5 (99.29%, 90.80%), CIFAR-10 AlexNet (83.07%, Table 1). CIFAR-10 절제·도메인 밖 실험은 VGG-16 (93.31%). |
| 9 | L133 "student 구조와 epoch은 확인 안 함 (4.1절)" | 본문 전체에 student 구조·epoch 없음 | 전체 | 수정 | student 구조와 epoch은 보고 안 됨. |
| 10 | L134 평균 정확도 외: 도메인 밖 이미지 (Table 4·5), 그 외 없음 | Table 4·5 맞음 (CIFAR-100에서 CIFAR-10 클래스 제외한 2K장, 테스트는 CIFAR-10). 추가로 "distillation success rate" (student/teacher 정확도 비) 정의, 정성 그림 (Fig. 2–3) | §4.1.1 p. 4; Figs. 2–3 p. 5; Tables 4–5 p. 8 | 추가 | 정확도 비율 지표 "distillation success rate" (student 정확도 ÷ teacher 정확도)를 같이 보고 (Table 1). 정성 예시 Fig. 2–3 (p. 5). |
| 11 | L135 위치 | Table 1 p. 6, Table 2·3·Fig. 4 p. 7, Table 4·5 p. 8 일치 | — | 일치 | — |
| 12 | L136 CIFAR-10 질의 20% 감소, 정확도 5.4%p 하락 | 50K → 40K, 80.01 − 74.60 = 5.41 | Table 1 p. 6 | 일치 | — |

---

## Skip-08 Comparative KD
- 본 판본: PDF에 arXiv 판본 표시 없음 ("Preprint. Under Review.", 14쪽)
- 대조 결과: 일치 7건 / 수정 1건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | L139 teacher 추론 n회 제한, 샘플 쌍·k개 묶음 표현 차이를 맞춤, teacher 출력 저장 후 재조합 | "because teacher representations can be cached and recombined" | Fig. 1 p. 3; §3 | 일치 | — |
| 2 | L140 WRN-40-2 → WRN-16-2, n = 1600: 26.09 → 36.38; n = 3200: 43.05 → 50.14 (KD 대비) | 일치. 가장 강한 기준선 CRD는 29.37, 45.34 | Table 1 p. 5 | 추가 | 가장 강한 기준선 CRD 대비로는 n = 1600에서 29.37 → 36.38%, n = 3200에서 45.34 → 50.14%. (Table 1 p. 5) |
| 3 | L140 연산·시간 절감 수치 보고 안 됨 | 맞음 | — | 일치 | — |
| 4 | L142 n = 3200에서 50.14%, 전체 데이터 KD 기준선 Table 1에 없음 | 맞음. Table 5–6도 n ≤ 4800까지 (n = 4800 KD 51.32%) | Table 1 p. 5; Table 6 p. 14 | 일치 | — |
| 5 | L143–144 teacher 표현 저장, 숫자 보고 안 됨 | 저장량·조합 계산 비용 수치 없음. 학습 세트에서 뽑는 데이터 점 수 상한 "limited to 100,000" | 부록 A.2.1 p. 12 | 추가 | 조합으로 만드는 학습 점 수는 실험마다 최대 100,000개로 제한. 저장·조합 비용 수치는 없음. (부록 A.2.1 p. 12) |
| 6 | L145 단위 n, 12 GB GPU 1장 (주로 2080 Ti), 실행 1회 20분 (n = 1600) ~ 40분 (n = 4800) | 일치 | p. 5 | 일치 | — |
| 7 | L145 teacher forward Y (횟수로) | n은 무작위로 뽑은 n개 샘플이고 80/20으로 학습·검증에 나눔: "We split the data 80-20% for train and validation". 고정 epoch 대신 검증 손실 early stopping | §4.1 p. 4 | 수정 | 단위 = teacher 호출 수 n (뽑은 샘플 수, 학습 80%·검증 20%로 나눔). student는 고정 epoch이 아니라 검증 손실 early stopping으로 학습. (§4.1 p. 4) |
| 8 | L146 설정: CIFAR-100, 3쌍, CRD 저장소 teacher, n = 1600~4800 | 일치. 3 seed, batch 64, T = 4, epoch당 스텝 수는 원 샘플 수와 같게 맞춤 | §4.1 p. 4; 부록 A.1 p. 12 | 추가 | 3 seed, batch 64, epoch당 스텝 수를 방법 간에 같게 맞춤 (부록 A.1 p. 12). |
| 9 | L147 클래스 logit 상관 (Table 4), 표현 공간 분석 (Fig. 4), 편향 전달은 향후 과제 (p. 10) | Table 4: student·teacher 상관 행렬의 평균 절대 차이. Fig. 4: WRN student logit 표현의 SVD | p. 9; p. 10 | 일치 | — |
| 10 | L148 위치 | Table 1 p. 5, Table 2·3 p. 7, Table 4·Fig. 4 p. 9, 결론 p. 10 일치 | — | 일치 | (선택) 부록 A·Table 5–6 (pp. 12–14) 추가 |
| 11 | L149 Skip·Reuse 경계, 단위가 호출 수뿐 | 맞음 | — | 일치 | — |

---

## Skip-09 Weak-to-Strong KD Accelerates Visual Learning
- 본 판본: arXiv:2604.15451v2 (2026-04-21), 18쪽, 보충 자료 없음
- 대조 결과: 일치 9건 / 수정 3건 / 추가 2건 / 확인 불가 1건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | L152 약한 teacher 고정, 초반만 KD, student가 teacher를 넘으면 끔 | 일치. 끄는 조건은 검증 2회 연속: "once the student surpasses the frozen teacher for k consecutive validations, we set λ(u) = 0 permanently (we use k = 2)". warmup–hold–decay 가중치 | §3, 알고리즘 1 p. 5 | 추가 | student가 검증에서 2회 연속 teacher 이상이 되면 KD 가중치를 영구히 0으로 둠. 가중치는 warmup–hold–decay 일정. (알고리즘 1 p. 5) |
| 2 | L154 ImageNet R18 → R50, τ = 65%: SGD 1.62, AdamW 2.86, Muon 4.75 | 일치 | Table 1 p. 8 | 일치 | — |
| 3 | L155 CIFAR-100 최대 1.83배 | DenseNet-40 → DenseNet-100 1.83× | Table 1 p. 8 | 일치 | — |
| 4 | L156 COCO 검출 1.67배, CIFAR-10 확산 2.5배 (Table 2, 초록) | 1.67×는 RetinaNet (10 → 6 epoch, AP50 20%)만. Faster R-CNN은 4 → 3 epoch, 1.33×. 확산은 Table 2에서 16k → 6k 2.67×, 18k → 12k 1.50×; 2.5×는 초록·결론 값 | Table 2 p. 9; 초록 p. 1 | 수정 | COCO 검출: RetinaNet-R34 → R50 1.67배 (10 → 6 epoch), Faster R-CNN-R18 → R50 1.33배 (4 → 3 epoch), AP50 20% 도달 기준. CIFAR-10 확산: FID 60 도달 16k → 6k 스텝 2.67배 (nc128-rb3), 18k → 12k 1.50배 (nc160-rb3). 초록·결론의 2.5배는 표 값과 다름. (Table 2 p. 9) |
| 5 | L158 최고 정확도 하락: ViT-S/16 75.87 → 75.36, CIFAR-10 R50 SGD 95.65 → 95.08 | 일치. ImageNet MobileNetV2 → R50 Muon 77.11 → 77.09도 소폭 하락 | Table 1 p. 8 | 일치 | — |
| 6 | L159 teacher가 너무 약하거나 강하면 이득이 거의 사라짐 (Table 3, Fig. 3) | 너무 약한 teacher는 오히려 느려짐: ImageNet MobileNetV3-S → R50 19 → 37 ep (0.51×), 확산 nc64-rb1 16k → 20k (0.80×). 너무 강하면 1.00–1.11× | Table 3 p. 10 | 수정 | teacher가 너무 강하면 이득이 거의 없음 (1.00–1.11배). 너무 약하면 오히려 느려질 수 있음: ImageNet MobileNetV3-S → ResNet-50 0.51배 (19 → 37 epoch), CIFAR-10 확산 0.80배 (16k → 20k 스텝). (Table 3 p. 10) |
| 7 | L160 [구조] 적당히 약한 teacher가 이미 있어야 하고 선택 단계 필요 (한계 p. 14) | "We assume that a weak teacher is already available", "benefits from a teacher-selection step" | §6 p. 14 | 일치 | — |
| 8 | L161 teacher forward (미니배치당 1회), 약한 teacher 학습 비용이 본문 표에 없음; 재학습 포함 실제 시간은 보충 자료에 있다고만 적음 | "we run a single forward pass of the frozen teacher per mini-batch and optionally precompute/cache teacher outputs", "wall-clock accounting when teacher re-training is included in the Supplementary Material". PDF에 보충 자료 없음 | §4.1 p. 8; §6 p. 14 | 일치 | — |
| 9 | (보충 자료) teacher 재학습 포함 실제 시간 | 저장소 PDF(18쪽)에 없음 | — | 확인 불가 | 보충 자료가 저장소에 없어서 확인 못 함. |
| 10 | L162 단위 = 목표 도달 epoch(step) 비, teacher forward N, 일회성 N, 하드웨어 보고 안 됨 | 맞음 (GPU 언급 없음) | §4.1 p. 7–8 | 일치 | — |
| 11 | L163 설정 쌍·최적화기, "총 epoch은 확인 안 함" | 쌍 일치 (CIFAR-100은 DenseNet-190 student도 Fig. 2b에 있음). 총 epoch은 본문 어디에도 없음 | Table 1 p. 8; Fig. 2 p. 9 | 수정 | 총 epoch은 보고 안 됨 (본문에 없음). CIFAR-100 student에 DenseNet-190도 있음 (Fig. 2b p. 9). |
| 12 | L164 평균 정확도 외: 검출 AP50, 생성 FID. 분류 그룹·분포 이동 없음 | 그룹·분포 이동 평가는 없음. 기제 진단 있음: 두 손실 항의 gradient norm (Fig. 5), linear CKA·teacher entropy·KL (Figs. 6–7) | §5 pp. 12–14 | 추가 | 기제 진단: 초반 gradient norm (Fig. 5), teacher 강도별 linear CKA·teacher entropy·KL (Figs. 6–7, pp. 12–14). |
| 13 | L165 위치 | Table 1 p. 8, Table 2·Fig. 2 p. 9, Table 3·Fig. 3 p. 10, 한계 p. 14 일치 | — | 일치 | — |
| 14 | L166 정정: 초록 2.5배 vs Table 2 2.67배; 4.75배 | 일치 | 초록 p. 1; Table 1·3 pp. 8, 10; Table 2 p. 9 | 일치 | — |
| 15 | L168 4.8배는 epoch 기준, GPU 시간 배율로 옮기면 안 됨 | 원문 "up to 4.8× speedup measured by epochs" | 초록 p. 1 | 일치 | — |

---

## Skip-10 DearKD
- 본 판본: arXiv:2204.12997v2 (2022-04-28)
- 대조 결과: 일치 8건 / 수정 2건 / 추가 2건 / 확인 불가 1건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | L173 1단계 CNN teacher 앞쪽 중간층 → ViT, 2단계는 증류 없이 학습 | 본문 식 7은 2단계를 CE만으로 정의. 그러나 부록 C: 300 epoch 모델은 "we use Equation (6) in the second stage except that we set β to 0 and let α linearly increase to 1". 식 6: αCE + (1−α)L_logit + βL_hidden → 2단계에서도 hard-label KD가 점점 줄며 남음 | 식 4–7 p. 4; 부록 C p. 12 | 수정 | 두 단계 학습. 1단계: CNN teacher의 앞쪽 중간층 특징 + hard-label logit 증류. 2단계: 본문은 CE만(식 7)이라 하지만, 300 epoch 레시피는 특징 KD만 끄고 hard-label KD 가중치(1−α)를 0까지 선형으로 줄임 (부록 C p. 12). |
| 2 | L175–177 Table 2 10% 40.5/50.3/54.3, 25% 61.1/64.3/67.1, 100% 72.2/74.5/74.8 | 일치. 50%는 68.3/71.6/72.3 | Table 2 p. 6 | 일치 | — |
| 3 | L179 1단계 250/300이 최고 74.8, 200이면 74.3, 300 전부 74.6 | 일치 (225: 74.6, 275: 74.7) | Table 7 p. 12 | 일치 | — |
| 4 | L179 "teacher를 끄는 구간이 마지막 50 epoch(17%)뿐" | 그 50 epoch에도 hard-label KD가 (1−α)로 남음 (부록 C). 완전히 끄는 구간이라고 할 수 없음 (L186 정정과 같은 내용) | 부록 C p. 12 | 수정 | 특징 KD가 꺼지는 구간이 마지막 50 epoch(약 17%)뿐이고, 그 구간에도 hard-label KD가 점점 줄며 남음. teacher 호출 절감은 작거나 없음. hard label을 미리 계산했는지는 명시 없음. |
| 5 | L180 [구조] 블록 8개를 MHCA로 교체, head 수 증가, CNN 중간 특징 필요 | "8 MHCA and 4 MHSA blocks", heads "12, 12, 16" (Ti/S/B) | §5.1 p. 6 | 일치 | — |
| 6 | (없음) 추론 비용 | 구조 변경으로 추론 처리량이 떨어짐: DearKD-Ti 1416.7 img/s vs DeiT-Ti 2536.5 (증류 DeiT-Ti 2529.5); S 570.1 vs 940.4 (936.2); B 253.7 vs 292.3 (290.9). V100 | Table 3 p. 7 | 추가 | [실측·원논문] 추론 처리량 감소: DearKD-Ti 1416.7 img/s vs DeiT-Ti 2536.5 (약 −44%), DearKD-S 570.1 vs 940.4, DearKD-B 253.7 vs 292.3 (V100). (Table 3 p. 7) |
| 7 | L181 1단계 teacher forward (RegNetY-16GF)와 정렬 모듈, 숫자 보고 안 됨 | 학습 시간·연산 없음. aligner = depth-wise conv + LayerNorm + ReLU | §3.1 p. 4 | 일치 | — |
| 8 | L182 학습 비용 재지 않음 (추론 처리량만 Table 3), A100 8장, V100 | 일치 | p. 6; Table 3 p. 7 | 일치 | — |
| 9 | L183 RegNetY-16GF 82.9% → DearKD-Ti/S/B, 300 (1단계 250) 또는 1000 (800) epoch, batch 2048 | 일치 | §5.1 p. 6; 부록 C p. 12 | 일치 | — |
| 10 | L184 평균 정확도 외: 다운스트림 전이 (Table 4), 합성 이미지 설정 | 일치. 추가로 epoch별 평균 attention distance (Fig. 4), 합성 이미지 다양성 LPIPS (Table 6). 합성 설정은 teacher ResNet-101 (77.37%), student 학습 V100 4장, 생성은 TITAN X; 부록 E "takes a lot of time and computation costs"라고만 적고 수치 없음 | Fig. 4 p. 4; Tables 4–6 p. 8; 부록 D–E p. 12 | 추가 | 진단: epoch별 평균 attention distance (Fig. 4 p. 4), 합성 이미지 LPIPS (Table 6 p. 8). 합성 데이터 설정은 teacher ResNet-101, student 학습 V100 4장, 이미지 생성 비용은 "시간·연산이 많이 듦"이라고만 쓰고 수치 없음 (부록 D–E p. 12). |
| 11 | L185 위치 | 5.1절·Table 1·2 p. 6, Table 3 p. 7, Table 4 p. 8, Table 7 p. 12 일치 | — | 일치 | — |
| 12 | L186 정정 (부록 C 레시피) | 원문과 일치 | 식 6–7 p. 4; 부록 C p. 12 | 일치 | — |
| 13 | L189 초반 KD 후 CE 절차의 원형은 ESKD (4_Substitute 02번) | DearKD는 ESKD(Cho & Hariharan)를 인용하지 않음 (참고문헌 검색). `4_Substitute/02_On the Efficacy of KD.pdf` 파일은 존재. "원형"이라는 계보 주장은 이 PDF로 확인 안 됨 | 참고문헌 pp. 9–11 | 확인 불가 | ESKD (4_Substitute 02번)가 먼저 제안한 비슷한 절차. DearKD는 ESKD를 인용하지 않음. |

---

## 범위 밖 참고 (축 분류 메모 L193–195)

| # | 메모의 서술 | 원문 | 판정 | 고칠 문구 |
|---|---|---|---|---|
| 1 | L195 "teacher forward까지 포함해 잰 것은 06 UNIX와 01 Distill on a Diet 정도" | UNIX는 식 11로 teacher forward를 명시적으로 셈. 01 Table 2는 반복 수 비율이라 teacher forward를 직접 세지 않음 (ImageNet은 logit 1회 계산) | 수정 | teacher forward를 명시적으로 센 것은 06 UNIX (식 11). 01 Table 2는 반복 수 비율이라 간접적이고, ImageNet 행은 logit을 1회만 계산함. |
| 2 | L195 "실제 시간으로 KD 학습 전체를 비교한 논문은 01 Table 3 하나" | 압축 후 복구 단계까지 넣으면 FSKD (Table 2, 초)와 PRACTISE (Table 10, 분)도 실제 시간 보고. 08 CKD는 실행 시간 범위만 | 일치 | (선택) "압축 복구용 실제 시간은 04·05가 따로 보고" 추가 |

---

## 전체 요약

1. **가장 심각한 오류는 비용 해석 쪽에 있음.** 01번은 CIFAR batch가 본문 128, 부록 256으로 서로 다르고 (epoch 환산이 약 2배 달라짐), Table 3 19.24 h의 비교 대상 값은 다른 논문에서 인용한 것이며 학습 epoch도 85 vs 100으로 다름. 10번은 본문 줄(L173, L179)이 아직 "2단계 증류 없음 / teacher off"로 남아 있어 L186 정정과 충돌함. 06번은 L123 메모가 L120 정정과 충돌함 ("teacher 비중 = 절감 상한").
2. **수치 자체는 대부분 원문과 맞음.** 10편 전체에서 표·본문 값을 잘못 옮긴 사례는 거의 없음. 오류는 조건 누락이 대부분임: seed 수 (01 이질 teacher 1개), 비교 대상 (09 검출 1.67배는 RetinaNet만), 지연 측정 batch (05 batch 64), 위치 (03 5,370 s는 표가 아니라 본문).
3. **"없음" 판정이 너무 강함.** 「평균 정확도 외 평가: 없음」으로 쓴 01·02·03 모두 진단 분석 (Spearman 상관, soft label 예시, 주성분 해석, 다른 student 전이)이 있음. 그룹·분포 이동 평가가 없다는 판정 자체는 10편 모두 맞음.
4. **빠진 비용 수치가 있음.** 10번 DearKD는 추론 처리량이 DeiT-Ti 대비 약 44% 낮음 (Table 3). 01번은 coreset 비용을 "4 epoch 미만"으로 추정함 (부록 A). 09번은 teacher가 너무 약하면 학습이 오히려 느려짐 (0.51배). 셋 다 메모에 없음.
5. **"확인 안 함"을 "보고 안 됨"으로 바꿀 곳:** 07 student 구조·epoch, 09 총 epoch. 전문 검색으로 원문에 없음을 확인함.
