# 원문 대조 보고서 — `notes/4_Substitute.md` 01–06

- 대조 대상: `$PAPER/notes/4_Substitute.md` 18–114행 (01–06번 블록)
- 원문: `$PAPER/Related Work/Efficient KD papers/4_Substitute/01–06_*.pdf` (저장소 PDF, 쪽 = PDF 물리 쪽, 첫 장 = 1)
- 추출: `verify/tools/subA/extract.py` (pymupdf, 쪽 표시 포함), 그림은 `render.py`로 이미지 렌더링 후 확인 (02 p.11 Fig. 7, 05 p.2 Fig. 1)
- 기존 검수(`notes/audit/paper-by-paper.md` Substitute 1–6)와 메모 안 **정정** 줄도 원문으로 다시 확인함

---

## Substitute-01 Efficient KD from Model Checkpoints
- 본 판본: arXiv 2210.06458v1 (12 Oct 2022)
- 대조 결과: 일치 12건 / 수정 5건 / 추가 4건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 중간 체크포인트 teacher, 스냅샷 앙상블, I(X;F)+I(Y;F) 최대 지점 선택 | Eq. 11 "max_F {I(X;F)+I(Y;F)}", Algorithm 1 | §5, p. 9–10 | 일치 | — |
| 2 | teacher 학습 비용 절반인 중간 지점 | "The training cost of all intermediate teachers is only half that of the full teachers" | §3.3, p. 4 | 일치 | — |
| 3 | ImageNet 60/120 epoch: R34→R18 70.66→70.86, R50→MBv2 66.64→66.79 | Table 1 동일. "60th epoch are adopted as T inter on ImageNet" | Table 1 p. 5, p. 4 | 일치 | — |
| 4 | CIFAR-100 100/200: WRN-40-2→WRN-40-1 72.68→72.76, →MBv2 68.03→68.99 | Table 1 동일 (±0.10/0.24, ±0.34/0.12) | Table 1, p. 5 | 일치 | — |
| 5 | "같거나 높음" (끝까지 학습 대비) | Table 1의 6쌍 모두 T inter ≥ T full (ResNet-110 쌍 72.48→72.49, 68.63→68.79 포함) | Table 1, p. 5 | 일치 | — |
| 6 | 30% 지점 R110→R32 70.74 vs 72.48 | Table 3 T0.3(=60 epoch) 70.74±0.18, T full 72.48±0.22 | Table 3, p. 10 | 일치 | — |
| 7 | "가장 좋은 지점은 teacher마다 다르고 (WRN-40-2 160, ResNet-110 120)… (5절, 한계)" | 160·120 epoch는 "selected" T\* (선택 알고리즘 결과). Table 3에서 T\*가 최고가 아닌 쌍 있음: WRN-40-2→MBv2 T0.5 68.99 > T\* 68.58, R110→MBv2 T0.7 69.01 > T\* 68.99. student 구조 무시는 "Conclusion and Limitations" 절 | §5 p. 9, Table 3 p. 10, §6 p. 10 | 수정 | "선택 알고리즘이 고른 지점은 teacher마다 다름 (WRN-40-2 160 epoch, ResNet-110 120 epoch, §5 p. 9). 같은 teacher라도 student에 따라 최적 지점이 다름 (Table 3 p. 10: R110→MobileNetV2는 T0.7 69.01% > T\* 68.99%). 선택 알고리즘은 student 구조를 고려하지 않음 (6절 한계, p. 10)." |
| 8 | 선택 지점 68.58 vs 절반 68.99 (WRN-40-2→MBv2) | Table 3 동일 | Table 3, p. 10 | 일치 | — |
| 9 | [구조] "긴 cosine 스케줄의 중간 체크포인트" | cosine은 CIFAR-100만 명시 ("cosine learning rate schedule … from 0.1 to 0"). ImageNet은 "standard PyTorch practice but train teacher models for 120 epochs" (스케줄 종류 미명시) | 부록 A.1, p. 14 | 수정 | "[구조] CIFAR-100에서는 200 epoch cosine 스케줄(0.1→0)의 중간 체크포인트임 (부록 A.1 p. 14). '짧은 스케줄로 끝까지 학습한 모델'과 다른 teacher이며, 앞의 것을 얻으려면 전체 스케줄을 미리 정해야 함. ImageNet 스케줄 종류는 명시 안 됨." |
| 10 | 선택 알고리즘의 디코더 학습 비용, 선택 지점이 더 비쌈, 숫자는 그림으로만 | A.2 "T\* needs higher computational cost (though lower than T1full + T2full)… needs additional computational cost". 디코더: B.1, Table 6 | 부록 A.2 p. 14, Fig. 8 p. 16, B.1 p. 16, Table 6 p. 17 | 일치 | — |
| 11 | 쌍·체크포인트마다 τ·α 격자 탐색 (Table 4), 탐색 비용 보고 안 됨 | "we search for the optimal hyperparameters … to each teacher-student pair as shown in Table 4". 탐색 비용 수치 없음 | 부록 A.1 p. 14, Table 4 p. 15 | 일치 | — |
| 12 | 어떻게 쟀나: "TITAN Xp 기준 추정 시간, Fig. 3과 Fig. 8의 가로축" | Fig. 3 캡션만 "We estimate the training hours on TITAN Xp". Fig. 8은 "Computational cost"라고만 쓰고 하드웨어 미표기. 총비용 = "training teachers and distilling the student" | Fig. 3 p. 6, Fig. 8 p. 16 | 수정 | "단위 = 총 학습 비용(teacher 학습 + student 증류). Fig. 3은 TITAN Xp 기준 추정 시간(p. 6), Fig. 8은 '계산 비용'으로만 표기하고 하드웨어 미표기(p. 16)." |
| 13 | 설정: "Tiny-ImageNet. 5회 평균." | Tiny-ImageNet은 스냅샷 앙상블 실험(Table 2)에만 쓰이고 150 epoch 학습. 5회는 체크리스트 "averagely evaluated in five independent experiments" (ImageNet 행에는 ± 없음) | 부록 A.1 p. 14, 체크리스트 p. 13, Table 1 p. 5 | 수정 | "Tiny-ImageNet(64×64, 200클래스)은 스냅샷 앙상블 실험에만 사용, teacher 150 epoch (부록 A.1 p. 14). 5회 독립 실행 평균(체크리스트 p. 13), 단 ImageNet 수치에는 표준편차 표기 없음 (Table 1 p. 5)." |
| 14 | 평균 정확도 외 평가: 없음 (정보 평면 분석은 있음) | 정보 평면(Fig. 4, 6) 외에 클래스 상관 heatmap(Fig. 5), 평균 logit 그림(Fig. 10), 조기 종료 위치 분석(Fig. 9) 있음. 모두 teacher 분석이며 student의 평균 밖 지표는 없음 | Fig. 5 p. 8, Fig. 9 p. 16, Fig. 10 p. 17 | 수정 | "평균 정확도 외 student 평가: 없음. teacher 분석으로 정보 평면(Fig. 4·6), 클래스 상관 heatmap(Fig. 5 p. 8), 평균 logit(Fig. 10 p. 17)이 있음." |
| 15 | 위치: Table 1·2 p. 5, Fig. 3 p. 6, Table 3·Alg. 1 p. 10, Table 4·5 p. 15, Fig. 8 p. 16 | 모두 해당 쪽에 있음 | — | 일치 | — |
| 16 | 메모: 체크포인트마다 τ·α 재튜닝, "같은 튜닝 규칙"과 다름 | Table 4: 예) WRN-40-2→WRN-40-1 T100 τ=7,α=0.7 / T200 τ=9,α=0.5. "intermediate models at the later training stages tend to choose a larger τ and a smaller α" | Table 4 p. 15, A.1 p. 14 | 일치 | — |
| 17 | 메모: 절감은 teacher 학습뿐, 증류 중 teacher forward 그대로 | 방법 정의와 일치 (중간 teacher도 증류 중 forward 필요) | §3.3 p. 4 | 일치 | — |
| 18 | (누락) 선택 알고리즘의 I(Y;F) 추정 방식 | "we … estimate I(Y;F) with the test accuracy" | 부록 B.1, p. 16 | 추가 | "선택 알고리즘은 I(Y;F)를 **테스트 정확도**로 추정함 (부록 B.1 p. 16). 체크포인트 선택에 테스트셋을 쓰므로 Table 3의 T\* 성능은 낙관적일 수 있음." |
| 19 | (누락) 스냅샷 앙상블 수치 | "the accuracy of Full Ensembles is 3.11% higher than that of Snapshot Ensembles, but the distillation accuracy of the former is 0.52% lower" (평균). Table 2 Average: 증류 EN1 66.11 / EN2 66.61 / EN3 66.63 | §3.4 p. 5–6, Table 2 p. 5 | 추가 | "스냅샷 앙상블(T1full+T1inter)은 독립 앙상블보다 앙상블 자체 정확도는 평균 3.11%p 낮지만 증류 결과는 평균 0.52%p 높음 (§3.4, Table 2 p. 5–6)." |
| 20 | (누락) 조기 종료와의 관계 | "The optimal checkpoints are located on the left of the positions of early stopping" (patience=10) | 부록 A.3 p. 14–15, Fig. 9 p. 16 | 추가 | "일반 조기 종료(patience 10)로 멈춘 지점보다 최적 체크포인트가 앞쪽에 있음 (부록 A.3, Fig. 9 p. 16). 조기 종료만으로는 같은 teacher를 얻지 못함." |
| 21 | (누락) student 설정 | "we use MobileNetV2 with its width multiplier set to 0.75" | 부록 A.1, p. 14 | 추가 | "MobileNetV2는 width 0.75 (부록 A.1 p. 14)." |

---

## Substitute-02 On the Efficacy of KD
- 본 판본: arXiv 1910.01348v1 (3 Oct 2019), 부록 포함 13쪽
- 대조 결과: 일치 11건 / 수정 5건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 일찍 멈춘 teacher: 전체 epoch의 1/3~1/4만 학습 | "only a third to fourth of the total number of epochs are needed" (저자 표현). 실제 예산: CIFAR-10 n∈{35,50,65,80} vs 200, LR을 ⌊(n−5)/3⌋마다 0.2배. ImageNet n∈{35,50}, LR 감소 (15,25,30)/(20,35,45) | §5.5 p. 6–7 | 수정 | "(2) 일찍 멈춘 teacher: 학습 epoch를 줄이고 LR 스케줄도 줄인 teacher. 저자는 '전체의 1/3~1/4'이라 표현(p. 6). 실제 CIFAR-10 35/50/65/80 epoch (기준 200), ImageNet 35/50 epoch (p. 7)." |
| 2 | 처음부터 30.24%, Full KD R18/34/50 30.57/30.79/30.95% | Table 1, Table 3 동일 | Table 1 p. 3, Table 3 p. 5 | 일치 | — |
| 3 | ESKD 29.01/29.16/29.35% | Table 3 동일 | Table 3, p. 5 | 일치 | — |
| 4 | 일찍 멈춘 teacher: R34(50) 29.02%, R50(35) 29.05% (Table 8) | Table 8에서 두 행 모두 Method = "ESKD" (조기 종료 teacher + ESKD). R152(35) 29.26%도 있음 | Table 8, p. 6 | 수정 | "ESKD + 일찍 멈춘 teacher: ResNet-34 (50 epoch) 29.02%, ResNet-50 (35) 29.05%, ResNet-152 (35) 29.26% 오류 (Table 8 p. 6). 일찍 멈춘 teacher + 전체 KD 조합은 보고 안 됨." |
| 5 | ESKD도 teacher 클수록 나빠짐, R152 29.45% | Table 8 ESKD R18/34/50/152 = 29.01/29.16/29.35/29.45 | Table 8, p. 6 | 일치 | — |
| 6 | 일찍 멈춘 teacher 자체 정확도 낮음 R50 23.85→27.01% (부록 Table 14) | Table 14 동일. R34 26.70→27.72, R152 21.69→23.58 | Table 14, p. 10 | 일치 | (보강 선택) "ResNet-34 26.70→27.72%, ResNet-152 21.69→23.58% 오류 (Table 14 p. 10)." |
| 7 | 옮겨 간 비용: 없음 | 비용·시간 수치 없음. 방법 정의상 KD 구간 단축·teacher 학습 단축 | 전체 검색 (GPU/hour/time/cost) | 일치 | — |
| 8 | 단위 = epoch 수, teacher forward N, 일회성 N, 하드웨어 보고 안 됨 | 하드웨어·시간 언급 없음 ("cheap"이라는 서술만, p. 6) | 전체 | 일치 | — |
| 9 | 설정: CIFAR-10 200 epoch, teacher 35/50/65/80/200; ImageNet R18/34/50/152→R18; α 0.9, τ 4 | 동일 (p. 3, p. 7, 부록 §7 p. 10) | p. 3, p. 7, p. 10 | 일치 | — |
| 10 | (누락) CIFAR-10 반복 횟수 | "The same experiment was repeated 5 times and median, mean, and standard deviation are reported" | §4, p. 3 | 추가 | "CIFAR-10은 5회 반복 (중앙값·평균·표준편차, p. 3). ImageNet은 단일 값." |
| 11 | 평균 정확도 외 평가: Places365 전이 (Table 9), 그 외 없음 | Table 9 Places365 top-1/top-5. Table 10 τ=20 (정확도). 평균 밖 지표 없음 | Table 9·10, p. 7 | 일치 | — |
| 12 | 위치: "Table 1–3·Fig. 3 (p. 4–5)" | Table 1·2는 p. 3, Fig. 3은 p. 4, Table 3은 p. 5 | p. 3–5 | 수정 | "위치: Table 1·2 (p. 3), Fig. 3 (p. 4), Table 3 (p. 5), Table 8 (p. 6), Fig. 4·Table 9·10 (p. 7), 부록 Table 14 (p. 10), 부록 Fig. 7 (p. 11)" |
| 13 | 위치: Table 8 p. 6, Fig. 4·Table 9·10 p. 7, Table 14 p. 10 | 해당 쪽 확인 | — | 일치 | — |
| 14 | 메모: Table 8 ESKD 행은 두 요인이 섞여 "기여를 따로 읽을 수 없음" | Table 8에는 ESKD + 완전 teacher(R34 29.16, R50 29.35)와 ESKD + 조기 종료 teacher(29.02, 29.05)가 모두 있음 → ESKD 고정 시 조기 종료 teacher 효과는 읽힘. 없는 것은 조기 종료 teacher + 전체 KD | Table 8, p. 6 | 수정 | "Table 8에서 ESKD를 고정하면 조기 종료 teacher의 추가 효과를 읽을 수 있음 (ResNet-34 29.16→29.02%, ResNet-50 29.35→29.05% 오류). 조기 종료 teacher + 전체 KD 칸은 없어 두 요인의 교차 효과는 알 수 없음." |
| 15 | 메모: "KD error"는 불일치 비율 (Table 2–3) | Table 2(CIFAR-10)는 KD error = "fraction of examples for which the student and teacher predictions differ". Table 3(ImageNet)은 KD loss | Table 2 p. 3, Table 3 p. 5 | 수정 | "'KD error'는 teacher–student 예측 불일치 비율이며 정답 오류율과 다름 (CIFAR-10 Table 2 p. 3). ImageNet Table 3은 KD loss를 보고함 (p. 5)." |
| 16 | 메모: KD를 끄는 epoch 본문 표에 없음 (확인 안 함) | 본문·부록 어디에도 끄는 epoch 수치 없음. 부록 Fig. 7은 곡선만 | 본문 전체, Fig. 7 p. 11 | 일치 | "(확인 안 함)" → "본문·부록 모두 명시 없음 (Fig. 7 p. 11 포함)." |
| 17 | (누락) ImageNet student 학습 길이 | Fig. 7 가로축이 0–100 epoch, LR 감소가 30·60 epoch 부근 (그림에서 읽음, 본문 명시 없음) | Fig. 7, p. 11 | 추가 | "ImageNet student 학습은 약 100 epoch로 보임 (부록 Fig. 7 가로축, 본문 명시 없음)." |
| 18 | 메모: Fig. 3 관찰은 ImageNet·ResNet 조건 | Fig. 3 = ResNet18 student, ResNet34 teacher, ImageNet | Fig. 3, p. 4 | 일치 | — |
| 19 | (누락) 학생 초기 이득이 KD 후반 손해로 뒤집힘의 근거 | "while the KD loss improves validation accuracy initially, it begins to hurt accuracy towards the end of training" | §5.3, p. 4 | 추가 | "KD는 학습 초반에는 검증 정확도를 올리고 후반에는 해침 (§5.3, Fig. 3 p. 4) → ESKD의 근거." |

---

## Substitute-03 Learning from a Lightweight Teacher for Efficient KD
- 본 판본: arXiv 2005.09163v1 (19 May 2020)
- 대조 결과: 일치 7건 / 수정 3건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 작은 teacher(LeNet5)를 합성 데이터(SynMNIST)에서 학습 | CIFAR-10은 T1 = LeNet5를 **일반 MNIST**(60,000장, 10 epoch)에서 학습. CIFAR-100은 T3 = SynMNIST-100. "T1 and T3 in Table 2 are taken as the two lightweight teachers exploited by LW-KD for CIFAR10 and CIFAR100, respectively". Table 3 머리글은 두 데이터셋 모두 "Ours[T1]"로 인쇄 | Table 2 p. 6, §5.1.1 p. 6, Table 3 p. 6 | 수정 | "작은 teacher(LeNet5, 약 6.17만 매개변수)를 쉬운 숫자 데이터에서 학습: CIFAR-10은 일반 MNIST(T1, 10 epoch), CIFAR-100은 합성 SynMNIST-100(T3, 10 epoch) (Table 2, §5.1.1 p. 6). Table 3 머리글은 둘 다 'Ours[T1]'로 표기되어 본문과 어긋남." |
| 2 | 판별기 적대 손실 추가 | Eq. 7–8, "two-layered fully-connected neural network is adopted as the discriminator" | §3.2.4, p. 5 | 일치 | — |
| 3 | Table 8: LeNet5 ~61.7K·~481.7K FLOPs vs ResNet-18 ~11.2M·~37.2M, "약 두 자릿수 차이" | 수치 동일. 원문 "about two orders of magnitude smaller … faster". 실제 비율: 매개변수 약 182배, FLOPs 약 77배 | Table 8 p. 9, §5.3 p. 8 | 수정 | "매개변수 약 182배, 입력 1개당 FLOPs 약 77배 차이 (Table 8 p. 9, 재계산). 원문은 둘 다 '약 두 자릿수'라고 씀 (§5.3 p. 8)." |
| 4 | CIFAR-10 R20: KD(R18) 92.98 vs 제안 92.94; MBv2 91.20 vs 91.57; ShuffleV2 92.17 vs 92.48; CIFAR-100 R20 68.38→69.39 | Table 3 동일 | Table 3, p. 6 | 일치 | — |
| 5 | (누락) KD의 ResNet-50 teacher 결과 | Table 3 KD 열 ResNet50 행: 93.04 / 91.69 / 92.55 — 세 student 모두 LW-KD(92.94/91.57/92.48)보다 높음 | Table 3, p. 6 | 추가 | "ResNet-50 teacher KD는 CIFAR-10 세 student 모두에서 제안법보다 높음: 93.04 vs 92.94, 91.69 vs 91.57, 92.55 vs 92.48% (Table 3 p. 6)." |
| 6 | [구조] 최댓값 확률을 정답 위치로 바꿔 넣음 (§3.2.3) | Eq. 3, "shift(·,·) means swapping the values of the two positions" | §3.2.3 Eq. 3, p. 4 | 일치 | — |
| 7 | 판별기 ~4.9K, Table 8엔 forward FLOPs(~9.7K)만, backward·적대 최적화 빠짐, teacher forward 약 77배 | Table 8: Discriminator ~4.9K / ~9.7K. "computational cost in terms of FLOPs needed to generate prediction for each data instance" | Table 8 p. 9, §5.3 p. 8 | 일치 | — |
| 8 | 하드웨어 = GTX 2080 Ti | "on two NVIDIA GTX 2080Ti GPUs" | §4.3, p. 6 | 수정 | "하드웨어 = GTX 2080 Ti 2장 (§4.3 p. 6)" |
| 9 | 일회성 비용 셈? teacher 학습 시간 N (크기 비교만) | 시간 없음. 다만 teacher 학습 epoch는 보고: T1–T3 10 epoch, T4(UCF101용) 2 epoch | Table 2, p. 6 | 추가 | "teacher 학습 시간은 보고 안 됨. 학습 길이만 보고: LeNet5 10 epoch (UCF101용 T4는 2 epoch) (Table 2 p. 6)." |
| 10 | 설정: CIFAR-10/100 student R20·MBv2·ShuffleV2, 텍스트·UCF101 | 동일. VGG19·GoogLeNet student도 있음. CIFAR 200 epoch, LR 0.1×(100,150) | Table 3 p. 6, §5.1.1 p. 6 | 일치 | — |
| 11 | 평균 정확도 외 평가: 없음, t-검정 (p ≤ 0.05) | Table 3 캡션 "\* represents LW-KD is significantly better than Tf-KD under t-test" — AVG 행 82.99\* 하나. 그 외 ablation(Table 7)뿐 | Table 3 p. 6, Table 7 p. 8 | 일치 | — |
| 12 | 위치: Table 3 p. 6, Table 7·Fig. 4 p. 8, Table 8 p. 9 | 해당 쪽 확인 | — | 일치 | — |
| 13 | (누락) 학습 중 teacher forward 위치 | Algorithm 2 line 8: 매 단계 "p^lt(c\|x_syn) ← T(x_syn;θt)", line 3 "one-to-one mapping between real instances and synthetic examples" | Algorithm 2, p. 5 | 추가 | "student 학습 매 단계마다 짝지은 합성 이미지에 teacher forward를 함 (Algorithm 2 p. 5). 짝이 고정(일대일)이라 teacher 출력은 미리 계산해 둘 수 있으나 논문은 그렇게 하지 않음 (추론)." |

---

## Substitute-04 Tf-KD (Revisiting KD via Label Smoothing Regularization)
- 본 판본: arXiv 1909.11723v3 (4 Mar 2021), 9쪽 (부록 미포함)
- 대조 결과: 일치 11건 / 수정 1건 / 추가 2건 / 확인 불가 1건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | Tf-KD_self (선행 학습한 자기 자신), Tf-KD_reg (정답 0.99 수작업 분포) | Eq. 7–9. 정의는 "We always set a ≥ 0.9", 실험은 a = 0.99, 고온 τ ≥ 20으로 softening | §4 p. 6, §5.2 p. 7–8 | 일치 | (보강 선택) "a = 0.99는 온도 softening 전 값 (Eq. 8–9 p. 6)." |
| 2 | Tf-KD_reg 추가 연산 없다고 주장 | "significantly improves the performance … without extra computation cost" (p. 8). 시간 측정 없음 | §5.2, p. 8; 초록 p. 1 | 일치 | — |
| 3 | CIFAR-100 MBv2: self +2.58, reg +2.50, KD(R18) +2.67 | Table 5: 70.96 (+2.58), +2.67 [R18]; Table 9: 70.88 (+2.50), 71.05 (+2.67) | Table 5 p. 7, Table 9 p. 8 | 일치 | — |
| 4 | ImageNet R50: reg +0.65, LSR +0.51 | Table 11 인쇄값 동일 | Table 11, p. 8 | 일치 | — |
| 5 | Tiny-ImageNet R50: reg +0.45 vs KD +0.68 | Table 10: 67.92 (+0.45), 68.15 (+0.68) [ResNeXt29] | Table 10, p. 8 | 일치 | — |
| 6 | ImageNet에서 LSR과의 차이 0.1~0.4%p | Table 11 원점수 차이: R18 70.24−70.02 = 0.22, R50 76.42−76.38 = 0.04, D121 75.62−75.24 = 0.38, RX101 79.76−79.67 = 0.09 | Table 11, p. 8 | 수정 | "ImageNet에서 label smoothing과의 차이는 원점수 기준 0.04~0.38%p로 작음 (ResNet-18 0.22, ResNet-50 0.04, DenseNet-121 0.38, ResNeXt-101 0.09; Table 11 p. 8)." |
| 7 | **정정 줄**: LSR 76.38−75.77 = +0.61인데 표는 +0.51, reg 차이 원점수 0.04 / 표기 기준 0.14 | 재계산 결과 동일 | Table 11, p. 8 | 일치 | — |
| 8 | 옮겨 간 비용: self는 선행 학습 + 그 모델 forward, 숫자 보고 안 됨; reg는 없음 | 시간 수치 없음 (전체 검색). reg는 teacher 모델·forward 없음 | §4 p. 6, 전체 | 일치 | (보강 선택) "Tf-KD_reg도 KL 항 계산은 남음 (Eq. 9)." |
| 9 | 단위 없음, teacher forward N, 일회성 N, 하드웨어 보고 안 됨 | 하드웨어 언급 없음 | 전체 | 일치 | — |
| 10 | 설정: Tiny-ImageNet 200 epoch, ImageNet 90 epoch, 3회 평균(일부), ImageNet reg a=0.99·τ=20·α=0.1, self·KD 하이퍼파라미터 탐색 | 동일. 3회는 Table 1·2 캡션("mean±std … over 3 runs"). 탐색은 "grid search from 70 epochs training (200 epochs)". CIFAR-100도 200 epoch | Table 1·2 p. 4, §5.1 p. 6–7, §5.2 p. 7–8 | 일치 | — |
| 11 | 평균 정확도 외 평가: 없음 | 없음 | 전체 | 일치 | — |
| 12 | 위치: Table 1–4 p. 4, 5–8 p. 7, 9–11 p. 8 | 해당 쪽 확인 | — | 일치 | — |
| 13 | (누락) 덜 학습한 teacher 실험 (De-KD) | Table 4: CIFAR-100에서 정확도 15.48%인 "Poorly-trained" ResNet-18 teacher로도 MobileNetV2 68.38→70.65% (+2.27). 완전 학습 teacher KD 71.05% (Table 1) | Table 4, p. 4 | 추가 | "덜 학습한 teacher로도 이득이 남: CIFAR-100에서 정확도 15.48%인 ResNet-18 teacher가 MobileNetV2를 68.38→70.65% (+2.27%p)로 올림. 완전 학습 teacher는 71.05% (Table 4·Table 1 p. 4). 01번(체크포인트)과 같은 축의 근거." |
| 14 | (누락) Tf-KD_reg 하이퍼파라미터도 모델별 상이 | "The temperature τ and α in Eq. (9) are different for different baseline models (see Supplementary Material)" | §5.2, p. 7 | 추가 | "CIFAR-100·Tiny-ImageNet의 Tf-KD_reg τ·α는 모델마다 다름 (p. 7). 'reg는 튜닝 비용 0'으로 쓰면 안 됨." |
| 15 | 부록 하이퍼파라미터 값 | 저장소 PDF는 9쪽이며 Supplementary Material 미포함 | — | 확인 불가 | 부록이 PDF에 없어 τ·α 값 확인 불가 |

---

## Substitute-05 Be Your Own Teacher
- 본 판본: arXiv 1905.08094v1 (17 May 2019)
- 대조 결과: 일치 10건 / 수정 4건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 가장 깊은 분류기가 얕은 보조 분류기를 가르침 | Eq. 2–5, "λ and α for the deepest classifier are zero" | §3.2, p. 4–5 | 일치 | — |
| 2 | CIFAR-100 학습 시간 26.98 h (14.67 + 12.31) → 5.87 h, 4.6배 | Fig. 1: ResNet152 teacher 14.67 h, ResNet50 KD 12.31 h, 자기 증류 ResNet50 5.87 h. 본문 "from 26.98 hours to 5.87 hours … a 4.6X" | Fig. 1, p. 2 | 일치 | (보강) "teacher ResNet-152 → student ResNet-50 조건 (Fig. 1)." |
| 3 | R50 KD 79.33 vs 가장 깊은 출구 80.56 (Table 3), 81.04는 앙상블 (Table 1) | Table 3: KD 79.33, Ours 80.56. Table 1: Classifier 4/4 80.56, Ensemble 81.04. Fig. 1은 5.87 h에 81.04%를 붙임 | Table 1 p. 4, Table 3 p. 5, Fig. 1 p. 2 | 일치 | — |
| 4 | 일반 학습 4.0시간, 자기 증류가 더 느림 | Fig. 1 "Baseline: ResNet50 (Standard Training) Training time: 4.03h, Accuracy: 77.68%" | Fig. 1, p. 2 | 수정 | "같은 그림에서 일반 학습은 4.03시간(77.68%)이라 자기 증류(5.87시간)가 약 1.46배 느림 (Fig. 1 p. 2)." |
| 5 | [구조] 보조 분류기·병목 층 추가, 추론 때 제거 가능 | "all the newly added layers … are only applied during training. They exert no influence during inference" | §3, p. 4 | 일치 | — |
| 6 | 1/4 출구 4.64배 가속에 67.87% (기준 77.68%) (Table 5) | Table 5 ResNet50 행 동일. 단 이 정확도 행은 Table 4 DSN 행(67.87/73.80/74.54/80.27/80.67)과 같고, 자기 증류 1/4 출구는 68.23 (Table 1·4) | Table 5 p. 6, Table 4 p. 5 | 일치 | 전사는 맞음. 인용 시 정정 줄 주의 문구 유지 |
| 7 | 옮겨 간 비용: 보조 분류기 학습 연산, "숫자 보고 안 됨" | Fig. 1에서 자기 증류 5.87 h vs 일반 학습 4.03 h → 보조 구조 포함 학습 추가분 1.84 h가 읽힘 (ResNet-50, CIFAR-100) | Fig. 1, p. 2 | 수정 | "보조 분류기·병목 층의 학습 연산. 구성 요소별 수치는 없음. 총 학습 시간으로는 일반 학습 4.03 h → 자기 증류 5.87 h (+1.84 h, 약 1.46배; ResNet-50 CIFAR-100, Fig. 1 p. 2)." |
| 8 | 하드웨어 = 보고 안 됨 | "All the experiments are implemented by PyTorch on GPU devices" (기종 없음) | §4, p. 5 | 일치 | — |
| 9 | teacher forward Y, 일회성 Y (비교 대상 시간에 teacher 학습 포함) | Fig. 1 KD 경로 = teacher 학습 14.67 h + 증류 12.31 h | Fig. 1, p. 2 | 일치 | — |
| 10 | 설정: CIFAR-100, ImageNet, ResNet-18/50/101/152, ResNeXt, WRN, VGG | 동일 + PyramidNet101-240 (Table 1) | Table 1·2 p. 4 | 일치 | — |
| 11 | 평균 정확도 외 평가: 잡음 가중치 정확도·손실 (Fig. 4), 그 외 없음 | Fig. 4 외에 층별 기울기 크기(Fig. 5 p. 7–8), 분류기별 특징 PCA(Fig. 6)·군집 분리도 SSE/SSB(Table 6 p. 8) 있음 | Fig. 4–5 p. 7, Fig. 6·Table 6 p. 8 | 수정 | "평균 정확도 외 평가: 잡음 섞은 가중치의 정확도·손실(Fig. 4 p. 7), 층별 기울기 크기(Fig. 5), 출구별 특징 분리도 SSE/SSB(Table 6 p. 8). 집단·최악 클래스 지표는 없음." |
| 12 | 위치: Fig. 1 p. 2, Table 1·2 p. 4, Table 3·4 p. 5, Table 5 p. 6 | 해당 쪽 확인 | — | 일치 | — |
| 13 | **정정 줄**: "표 5의 정확도 열은 표 4의 DSN 행과 같음" | ResNet-18·50·101 행은 Table 4 DSN 행과 완전히 같음. ResNet-152 행(68.84/78.22/81.43/81.61/82.29)은 Table 4 "Our approach"(68.84/78.72/81.43/81.61/82.29)와 2/4 출구만 다름 | Table 4 p. 5, Table 5 p. 6 | 수정 | "표 5의 정확도 행은 ResNet-18·50·101에서 표 4 DSN 행과 같고, ResNet-152는 제안법 행과 같음(2/4 출구만 78.22 vs 78.72). 표 간 기준이 섞여 있어 4.64배 가속을 표 1 성능과 한 쌍으로 인용하지 않음." |
| 14 | 정정 줄 나머지: 앙상블 81.04, 최심 80.56, 5.87 h < 26.98 h, > 4.03 h | 원문과 일치 | Fig. 1 p. 2, Table 1 p. 4 | 일치 | — |
| 15 | (누락) 앙상블의 추론 비용 | Table 5 Ensemble Acceleration 0.93X (ResNet-18·50), 0.96X (101), 0.98X (152). "only 0.05% penalty for computation" (본문) | Table 5 p. 6, §4.5 p. 6 | 추가 | "81.04%(앙상블)는 단일 출구보다 추론이 느림: ResNet-50 앙상블 0.93배 속도 (Table 5 p. 6)." |
| 16 | (누락) ImageNet 결과 | Table 2: ResNet-50 baseline 73.56 → 4/4 출구 75.24, 앙상블 74.73. ResNet-18 앙상블 68.93 < 4/4 출구 69.84 | Table 2, p. 4 | 추가 | "ImageNet에서는 앙상블이 가장 깊은 출구보다 낮을 수 있음 (ResNet-18 68.93 vs 69.84%, ResNet-50 74.73 vs 75.24%; Table 2 p. 4). 학습 시간은 ImageNet에서 보고 안 됨." |
| 17 | (누락) 평균 향상 수치의 기준 | "increment of 2.65% in CIFAR100 and 2.02% in ImageNet on average" — 출구 지정 없이 표 1·2 기반 | §4.2, p. 5 | 추가 | "평균 향상 2.65%(CIFAR-100)·2.02%(ImageNet)는 어느 출구 기준인지 명시 안 됨 (§4.2 p. 5). 인용 시 출구 확인 필요." |

---

## Substitute-06 Deep Mutual Learning
- 본 판본: arXiv 1706.00384v1 (1 Jun 2017)
- 대조 결과: 일치 8건 / 수정 3건 / 추가 2건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 사전학습 teacher 없이 여러 student가 서로 KL로 맞춤 | Eq. 4–5, 8 | §2, p. 3–4 | 일치 | — |
| 2 | CIFAR-100 MobileNet 두 개: 73.65 → 76.21% | Table 2 MobileNet+MobileNet: DML 76.21 / 76.10, 독립 73.65 | Table 2, p. 5 | 일치 | (보강) "두 번째 MobileNet은 76.10%." |
| 3 | Market-1501 ImageNet 사전학습 MobileNet single-query mAP 60.68 → 68.83% | Table 3 동일. "the averaged performance of the two networks in the cohort is reported" | Table 3 p. 6, §3.3 p. 5 | 일치 | (보강) "두 동료의 평균값." |
| 4 | 사전학습 teacher 증류보다 높다 (Table 4) | Table 4 CIFAR-100: WRN-28-10 distills R32 69.48 vs DML 70.73; MBv2 distills R32 69.12 vs 71.10. Market-1501 InceptionV1→MBv2 49.11 vs 52.87 mAP — Market 비교는 사전학습 없는 설정(독립 MBv2 46.07) | Table 4, p. 6 | 수정 | "사전학습 teacher로 증류한 것보다 높음: CIFAR-100 ResNet-32 69.48 → 70.73% (WRN-28-10 teacher), Market-1501 MobileNet 49.11 → 52.87% mAP (InceptionV1 teacher, ImageNet 사전학습 없는 설정) (Table 4 p. 6)." |
| 5 | [구조] K개 동시 실행, student 하나 기준 학습 연산·메모리 K배 | 원문은 비용 측정 없음. Algorithm 1은 각 네트워크 갱신 전 두 모델 예측을 다시 계산 (steps 1, 3). 실험 cohort는 이질적 (WRN-28-10 36.5M + ResNet-32 0.5M 등) | Algorithm 1 p. 4, Table 1 p. 5 | 수정 | "[구조] 학습 중 네트워크 K개를 함께 학습하고 상호 KL을 계산함. 같은 구조 K개면 연산이 대략 K배 이상(Algorithm 1은 갱신마다 예측을 다시 계산, p. 4). 이질 cohort에서는 K배가 아님. 메모리·시간 측정 없음." |
| 6 | 옮겨 간 비용: teacher 사전학습 → 동료 동시 학습, 숫자 보고 안 됨 | 시간·메모리 수치 없음 (전체 검색) | 전체 | 일치 | — |
| 7 | (누락) 저자의 비용 관련 서술 | "It can be distributed by learning each network on one device and passing the small probability vectors" (p. 4); "minimal additional cost if ensembles are already used" (p. 7); "more of them can fit on one GPU" (p. 2) | §2.3 p. 4, §3.5 p. 7, p. 2 | 추가 | "저자는 비용을 수치 없이 서술만 함: 네트워크별 장치 분산 가능(p. 4), 앙상블을 원래 쓰는 경우 추가 비용 최소(p. 7)." |
| 8 | 단위 없음, teacher forward N, 일회성 N, 하드웨어 GTX 1080 1장 | "conduct all experiments on an NVIDIA GeForce GTX 1080 GPU" | §3.1, p. 5 | 일치 | — |
| 9 | 설정: CIFAR-100 200 epoch, batch 64, R32·MobileNet·InceptionV1·WRN-28-10; Market-1501 100,000 반복 | 동일 (Market batch 16, Adam lr 2e-4) | §3.1, p. 5 | 일치 | — |
| 10 | 평균 정확도 외 평가: 재식별 mAP·Rank-1, 그 외 없음 | 그 외: 가중치 잡음에 대한 학습 손실 변화(Fig. 3b), 사후확률 엔트로피 1.7099 vs 0.2602(p. 8), cohort 크기별 mAP 평균·표준편차(Fig. 2), DML_e 예측 엔트로피(Fig. 4) | Fig. 2 p. 7, Fig. 3·4 p. 8 | 수정 | "평균 정확도 외 평가: 재식별 mAP·Rank-1, 가중치 잡음에 대한 손실 변화(Fig. 3b p. 8), 사후확률 엔트로피(DML 1.7099 vs 독립 0.2602, p. 8), cohort 크기별 mAP 표준편차(Fig. 2 p. 7). 집단·최악 클래스 지표는 없음." |
| 11 | 위치: Table 1·2 p. 5, Table 3·4 p. 6, Fig. 2 p. 7 | 해당 쪽 확인. Fig. 3·4는 p. 8 | — | 일치 | (보강) "Fig. 3·4 (p. 8)" |
| 12 | 메모: "teacher 불필요" ≠ "비용 적음" | 방법 정의와 일치 (추론) | — | 일치 | — |
| 13 | (누락) 동일 구조 증류는 오히려 나쁨 | "using the same network as teacher in model distillation actually makes the student worse than independent learning (… 45.16 vs … 46.07)" | Table 4, p. 6 | 추가 | "같은 구조 MobileNet을 사전학습 teacher로 쓴 일반 증류는 독립 학습보다 나쁨 (45.16 vs 46.07% mAP, Market-1501; Table 4 p. 6). DML은 51.26%." |

---

## 전체 요약

1. **가장 심각한 오류**: 03번 효율화 방식에서 CIFAR-10 teacher를 "SynMNIST에서 학습"이라고 씀. 실제로는 **일반 MNIST**(T1)이고, SynMNIST-100(T3)은 CIFAR-100에만 씀. 01번은 알고리즘이 고른 160·120 epoch를 "가장 좋은 지점"으로 적었는데, Table 3에서 T\*가 최고가 아닌 쌍이 둘 있음. 02번 메모는 Table 8로 "두 요인을 따로 읽을 수 없다"고 썼으나, ESKD를 고정하면 조기 종료 teacher의 효과가 읽힘 (29.16→29.02, 29.35→29.05%).
2. **"숫자 보고 안 됨" 판정 오류**: 05번 옮겨 간 비용은 Fig. 1의 4.03 h → 5.87 h로 보조 구조의 학습 부담(+1.84 h, 약 1.46배)이 읽히는데 "보고 안 됨"으로 적음.
3. **"그 외 없음" 과소 기재 패턴**: 01·05·06번 모두 평균 정확도 외 분석(heatmap, 기울기, SSE/SSB, 엔트로피, 잡음 강건성)을 빠뜨림. 집단·최악 클래스 지표가 없다는 결론 자체는 6편 모두 맞음.
4. **조건·범위 누락 패턴**: 하드웨어 개수(03번 2080 Ti 2장), Fig. 8 하드웨어 미표기(01번), cosine 스케줄 범위(01번), Market 비교의 비사전학습 설정(06번), "1/3~1/4"이 저자 표현일 뿐이라는 점(02번). Tf-KD의 "LSR과 0.1~0.4%p 차이"는 원점수로 0.04~0.38%p.
5. **기존 정정 줄**: 04번 정정(Table 11 불일치)은 원문과 맞음. 05번 정정의 "표 5 = 표 4 DSN 행"은 ResNet-152 행에서는 틀림(제안법 행과 일치). 다듬을 필요 있음. 04번 부록은 저장소 PDF에 없어 하이퍼파라미터는 확인 불가.
