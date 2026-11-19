# 원문 대조 보고서: `notes/6_Evaluation (Q3).md` 01–11

- 대상 메모: `$PAPER/notes/6_Evaluation (Q3).md` (검수 당시 사본)
- 원문: `$PAPER/Related Work/Efficient KD papers/6_Evaluation (Q3)/01–11_*.pdf`
- 추출: `tools/skip_extract.py` → `tools/eval_txt/NN.txt` (쪽 표시 `=====PAGE n=====`). 표 깨짐 확인용 렌더링: `tools/render_eval.py` → `tools/render/06_p9.png`
- 쪽 번호: 저장소 PDF의 1부터 센 쪽. 메모의 기존 `**정정**` 줄과 `notes/audit/paper-by-paper.md`(Evaluation 절)도 함께 다시 대조함.
- 판본: 01·02·09는 PDF에 arXiv 표기 없음 → PDF 생성일로 적음.

---

## 05–08번: "증류 학습에서 teacher 예산을 줄인 경우"를 다루는가 (명시 판정)

| # | 판정 | 근거 |
|---|---|---|
| 05 Compressed CLIP | **다루지 않음** | 증류·teacher 없음. 학습 없이 OpenCLIP 추론 때 토큰을 줄이는 MARS·FastV 등의 평가. 원문 "without labels or fine-tuning" (초록, p. 1). 전문 검색에서 "teacher"·"distill" 0건. |
| 06 Later Is Better | **다루지 않음** | 학습 없는 추론 시 토큰 축소의 깊이 일정 비교. 백본 DeiT가 "Supervised, distilled"로 분류될 뿐(Table 4, p. 9) 증류 학습이나 teacher 비용은 변수로 다루지 않음. TTA 6종도 teacher 없음. |
| 07 DRoP | **다루지 않음** | KD 없는 데이터 프루닝. 쓰는 사전 모델은 query model(클래스별 검증 오류 추정용)이지 teacher가 아님. KD는 관련 연구에서 "robust knowledge distillation (Wang et al., 2023; Lukasik et al., 2022)" 한 줄 언급뿐 (p. 3). |
| 08 Coreset Selection | **다루지 않음** | KD 없는 코어셋 선택. 쓰는 사전 모델은 점수 계산용 surrogate model. "distillation"은 참고문헌의 dataset distillation 제목뿐. 또한 하류 학습 반복 수를 고정(epoch = N/s)해 학습 연산량 자체도 줄지 않음 (p. 5, 부록 B.4 p. 17). |

→ 메모 「정리 메모」의 "11편 중 효율 KD에서 그룹·하위 클래스 지표를 본 논문은 없음"은 이 범위(01–11)에서 원문과 **일치**. 01–04번도 teacher 출력을 매 단계 전부 쓰는 일반 KD(또는 teacher 마지막 층 이식)만 다룸.

---

## Eval-01 Teacher's Pet
- 본 판본: PDF 첫 쪽 "July 9, 2021", arXiv 판 표기 없음 (PDF 생성일 2021-07-09)
- 대조 결과: 일치 6건 / 수정 4건 / 추가 3건 / 확인 불가 2건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | teacher 오류가 student로 옮겨 가 증폭; AdaAlpha·AdaMargin은 "증류 비중을 낮추는 보정" | 원인은 가설 수준: "This points at a potential source"; "Such inaccuracy may in fact be amplified by the student". AdaAlpha = 클래스별 혼합 가중 αy, AdaMargin = 클래스 쌍별 margin ρyy′ (비중 축소 아님) | §3.6 p. 6; §4.1–4.2 p. 8 | 수정 | "teacher가 확신하며 틀리는 클래스가 student로 옮겨 간다는 설명을 '잠재적 원인'으로 제시(§3.6, p. 6). 보정은 AdaAlpha(클래스별 teacher 혼합 가중 αy, 음수 margin이면 0)와 AdaMargin(클래스 쌍별 손실 margin)의 두 종류(§4, p. 8)." |
| 2 | ImageNet R34 자기 증류 평균 +0.42, 하위 10개 −2.60 | 34/34: +0.42 / −2.60 | Table 2, p. 5 | 일치 | – |
| 3 | R50→R34 +0.39/−2.05, R34→R18 +0.15/−3.60 | 동일 | Table 2, p. 5 | 일치 | – |
| 4 | CIFAR-10/100 평균·최악 함께 오름, CIFAR-100 +1.93/+3.33 | CIFAR-100 +1.93 / worst-1 +3.33, CIFAR-10 +0.55/+0.90 (ResNet-56 자기 증류) | Table 3, p. 5 | 일치 | (선택) "+3.33은 worst-1 기준." |
| 5 | 정정 줄: CIFAR-100-LT −1.46은 표 3 worst-1, 표 5는 worst-1 0/0, worst-10 2.33→0.87 | Table 3 "CIFAR-100-LT +2.17 −1.46"(worst-1 열); Table 5 one-hot 43.22/0.00/2.33, Distillation 45.39/0.00/0.87 → 평균 +2.17, worst-10 −1.46 | Table 3 p. 5; Table 5 p. 10 | 일치 (정정 줄 맞음) | – |
| 6 | 정정 줄의 "[표 3](…v2#page=6)" 링크 | 저장소 PDF에서는 Table 3이 **p. 5** | p. 5 | 확인 불가 (저장소 PDF에 판 표기가 없어 v2 쪽 번호와 같은지 판단 불가) | "저장소 PDF 기준 표 3은 p. 5." 병기 권장 |
| 7 | AdaAlpha가 ImageNet 하위 10개를 22.02→23.22로, 증류 없음 23.64엔 못 미침 | 수치 일치. 단 Table 5는 **ResNet-50 자기 증류**, "non-early stopped teachers", 80/20 분할 dev로 α 추정. Table 2(ResNet-34, early-stopped teacher)와 다른 실험 | Table 5 p. 10; §5 setup p. 9 | 수정 (조건 추가) | "Table 5의 ImageNet 행은 ResNet-50→ResNet-50 자기 증류, 조기 종료 없는 teacher 기준(§5, p. 9). Table 2(ResNet-34, 조기 종료 teacher)와 다른 실험. AdaMargin은 23.20%." |
| 8 | 옮겨 간 비용: 해당 없음 | Ada 방법은 "We train an initial teacher on the new train slice of data, and estimate its per-class performance on the holdout dev slice" → teacher 1회 추가 학습. 비용 수치 없음. 체크리스트: "Our new techniques do not increase the complexity of training or inference" | §5 p. 9; 체크리스트 p. 15 | 추가 | "AdaAlpha·AdaMargin은 학습셋 80%로 초기 teacher를 따로 학습해 dev 20%에서 클래스별 성능을 추정(§5, p. 9). 이 추가 학습의 비용 수치는 보고 안 됨." |
| 9 | 하드웨어 = 확인 안 함 | 체크리스트 3(d) "[No] … our focus is not on computational aspects" | p. 15 | 수정 | "하드웨어·총 연산량 보고 안 함 (체크리스트 3(d) [No], p. 15)." |
| 10 | 설정 목록 (ImageNet R50/34/18, CIFAR R56, LT, iNaturalist, Adult) | 모두 있음. iNaturalist는 부록 Table 8(R50 자기 증류, worst-20) | §3 pp. 4–7; Table 8 p. 18 | 추가 | "ImageNet KD는 α=0.2, 온도 0.9 고정, 90 epoch; CIFAR는 450 epoch, 온도 {1..5} 중 선택(부록 A.2, p. 16). §3 ImageNet 분석은 조기 종료 teacher." |
| 11 | Adult: 속성 하위집단 정확도 | Table 4: 평균 +3.10, Asian Male −5.94, Black Female −2.38 (랜덤 포레스트) | Table 4, p. 7 | 추가 | "UCI Adult(랜덤 포레스트): 평균 +3.10%p, Asian Male −5.94%p, Black Female −2.38%p (Table 4, p. 7)." |
| 12 | 위치 Fig. 1 p. 2, Table 1 p. 4, Fig. 2·T2·T3 p. 5, T4 p. 7, T5·6 p. 10 | 모두 맞음 | – | 일치 | – |
| 13 | 메모: "ImageNet은 클래스당 검증 이미지 50장" | 원문에 해당 서술 없음 | – | 확인 불가 (원문 밖 사실) | "50장/클래스는 ILSVRC 검증셋 구성에서 온 추론이며 논문에 적힌 내용 아님." |
| 14 | 메모: "seed 수와 구간은 확인 안 함" | 체크리스트 "error bars [Yes]"이나 ± 는 Table 6(CIFAR-100 ablation)뿐. ImageNet Table 2·5에는 seed 수·구간 없음 | Table 6 p. 10; p. 14 | 수정 | "ImageNet 표(2·5)에는 seed 수·오차 구간 없음. ±는 CIFAR-100 ablation(Table 6)에만 있음." |
| 15 | 균형·클래스 적은 경우 손해 안 보임 | CIFAR-10/100에서 worst-1 개선(Table 3). §3.4 "distillation appears a complete win" | Table 3 p. 5; §3.4 | 일치 | – |

## Eval-02 DeDiER (Using Early Readouts to Mediate Featural Bias)
- 본 판본: arXiv 표기 없음 (PDF 생성일 2023-11-10)
- 대조 결과: 일치 7건 / 수정 2건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | Waterbirds WGA: 지도학습 student 33.8, KD(λ=0.5) 67.9, KD(λ=1) 84.9, DeTT 87.5, 제안 89.8, teacher 91.4 | 동일. 33.8은 "ERM" 행(회색 = 지도학습) | Table 1, p. 7 | 일치 | (선택) "33.8%는 Table 1의 ERM 행." |
| 2 | KD(0.5) 평균 87.9 / WGA 67.9 vs teacher 92.6 / 91.4 | 동일 | Table 1, p. 7 | 일치 | – |
| 3 | SimKD (물새, 육지 배경) 71.4 vs teacher 91.7, 20%p | 71.4±2.15 vs 91.7 (차이 20.3) | Table 2, p. 8 | 일치 | – |
| 4 | 옮겨 간 비용: 보조 분류기 학습·forward, 숫자 없음 | 숫자 없음 확인(GPU·time·hour 검색 결과 없음). 보조층은 "ResNet depth-1 block", L=R=1로 **매 epoch 끝에 1 epoch 재학습** | §5.4 p. 7; Alg. 1 p. 5 | 추가 | "보조 분류기(ResNet 블록 1개)를 매 epoch 끝에 학습셋 전체로 1 epoch 재학습(L=R=1, §5.4, p. 7). 그 비용 수치는 보고 안 됨." |
| 5 | 하드웨어 = 확인 안 함 | 원문 전체에 GPU·시간 표기 없음 | 전문 검색 | 수정 | "하드웨어·학습 시간 보고 안 함." |
| 6 | 설정: teacher는 "최저 그룹 정확도가 높은 모델" | "ResNet-50 GroupDRO trained model as the teacher"; 텍스트도 "Bert GroupDRO trained model" | 부록 A.2, p. 11 | 수정 | "teacher는 그룹 라벨로 Group DRO 학습한 ResNet-50(시각)·BERT(텍스트) (부록 A.2, p. 11)." |
| 7 | (빠짐) Table 1 teacher 행의 Annotation 열 | Teacher 행 "Annotation: No" 표기. 그런데 부록은 Group DRO teacher라고 함 | Table 1 p. 7 vs p. 11 | 추가 | "Table 1의 teacher 행은 'Annotation No'로 표기되나 부록상 Group DRO 학습 모델이라 그룹 라벨을 씀. 표의 주석 열은 student 학습 단계 기준으로 읽을 것." |
| 8 | 평균 정확도 외 평가: WGA·그룹별·주석 열 | 있음 | Table 1·2 pp. 7–8 | 일치 | – |
| 9 | (빠짐) 평균 정확도의 정의 | "Average Accuracy is the percentage of correctly predicted data points in the test set" → 가중 없음. 09·10번(학습 그룹 비율 가중)과 다름 | §5.2, p. 6 | 추가 | "평균 정확도는 테스트셋 전체 정답 비율(가중 없음, §5.2 p. 6). 09·10번의 학습 그룹 비율 가중 평균과 다른 집계." |
| 10 | 정정 줄: 학습 손실엔 그룹 라벨 없음 + 검증·튜닝·테스트엔 그룹 라벨, 테스트 WGA | "we use group information of validation data for tuning hyperparameters, and group information of test data for measuring"; "We employ validation worst group accuracy for early stopping" | §5.2 p. 6; §5.4 p. 7 | 일치 (정정 줄 맞음) | – |
| 11 | 위치: Fig. 1 p. 2, T1 p. 7, T2·Fig. 5 p. 8, T3–5 pp. 11–12 | 맞음 | – | 일치 | – |
| 12 | 메모: λ=1일수록 WGA 좋음은 저편향 teacher에서의 이야기 | 이 설정(Group DRO teacher)에서 관찰된 두 λ 비교 | Table 1 | 일치 | – |

## Eval-03 Debiased Distillation by Transplanting the Last Layer (DeTT)
- 본 판본: arXiv:2302.11187v1
- 대조 결과: 일치 6건 / 수정 3건 / 추가 4건 / 확인 불가 0건

| # | 메모의 서술 | 원문 | 위치 | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | Waterbirds WGA: teacher 90.8, KD(0.5) 67.9, KD(1) 84.9, SimKD 71.4, DeTT 87.5 | 동일 | Table 1, p. 6 | 일치 | – |
| 2 | CelebA: teacher 90.7, KD(0.5) 62.0, DeTT 89.5 | 동일 | Table 1, p. 6 | 일치 | – |
| 3 | λup 5에서 76.6, 50에서 87.5 | 동일 (20에서 84.7) | Table 3, p. 7 | 일치 | – |
| 4 | 옮겨 간 비용: 투영층 + 편향 식별 모델 선행 학습, 숫자 없음 | 숫자 없음 확인. 정성 서술: "introducing a nonnegligible overhead in terms of the training time and computing" | §6, p. 8 | 추가 | "저자 스스로 편향 식별 모델 별도 학습이 'nonnegligible overhead in terms of the training time and computing'이라고 적음(§6, p. 8). 수치는 없음." |
| 5 | 설정: "teacher는 편향을 줄인 모델" | "the teacher classifiers are trained with group DRO … using the group annotations" | §5.1, p. 5 | 수정 | "teacher는 전체 그룹 라벨로 Group DRO 학습(§5.1, p. 5). ResNet-50→ResNet-18(ImageNet 사전학습), BERT→DistilBERT. 3회 평균, KD 온도 τ=4(p. 6)." |
| 6 | 설정: 투영층 | 학생 크기 맞춤을 위해 투영층을 KD 기준선에도 붙임: "we also attach the projectors and use teacher-sized last layer to the baseline student" | p. 6 | 추가 | "공정 비교를 위해 투영층·teacher 크기 마지막 층을 KD 기준선에도 붙임(p. 6). 이식한 마지막 층은 동결." |
| 7 | 평균 외: WGA·그룹별(Table 4·5)·주석 범위 | 있음 | Tables 1, 4, 5 | 일치 | – |
| 8 | 위치: Fig. 1 p. 2, T1 p. 6, T2–5 p. 7, T6 p. 8 | 맞음 | – | 일치 | – |
| 9 | 메모: 02번과 KD·SimKD 수치 동일 | KD·SimKD·ERM·JTT·Group DRO 행은 같음. **teacher 행 다름**(91.9/90.8 vs 92.6/91.4), MultiNLI DeTT 74.8/70.2 vs 02번 78.9/77.9 | Table 1 p. 6 vs 02 Table 1 p. 7 | 수정 | "02번과 KD·SimKD 등 기준선 행은 같으나 teacher 행과 MultiNLI DeTT 행(70.2% vs 77.9%)은 다름." |
| 10 | 메모: SimKD가 그룹 지표에서 약함 | Waterbirds 71.4로 약하나 CelebA 89.0으로 DeTT 89.5와 비슷 | Table 1 p. 6 | 수정 | "SimKD의 약점은 Waterbirds(71.4%)에 한정. CelebA는 89.0%로 DeTT 89.5%와 비슷." |
| 11 | (빠짐) 이식 없이 업웨이팅만 한 KD | KD(α=1)+업웨이팅 Waterbirds 86.3 / CelebA 89.1 — DeTT 87.5/89.5에 근접 | Table 2, p. 7 | 추가 | "이식 없이 KD(α=1)+업웨이팅만으로 Waterbirds 86.3%, CelebA 89.1%(Table 2, p. 7). Waterbirds 개선의 대부분은 업웨이팅에서 옴." |
| 12 | (빠짐) 최저 그룹의 seed 의존성 | "the worst-performing group differs by the random seed" | Table 4 캡션, p. 7 | 추가 | "Waterbirds에서 최저 그룹 자체가 seed마다 바뀜(Table 4 캡션, p. 7)." |
| 13 | 정정 줄: Group DRO teacher, 테스트 WGA | 맞음 | §5.1 p. 5 | 일치 | – |

(참고: Table 3의 CelebA teacher worst 90.6은 Table 1의 90.7과 0.1 차이. 원문 내부 불일치, 영향 작음.)

## Eval-04 What is Left After Distillation?
- 본 판본: arXiv:2410.08407v2
- 대조 결과: 일치 6건 / 수정 4건 / 추가 2건 / 확인 불가 0건

| # | 메모의 서술 | 원문 | 위치 | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | ImageNet R50→R18: 유의하게 달라진 클래스 수 T=2 77개, T=10 408개(약 41%) | #SC 77, 408 일치. 단 검정 대상은 "class-wise accuracy … normalized by their respective overall model accuracy", 클래스별 two-tailed Welch t-test, p≤0.05, 5회 | Table 1 p. 6; §4 p. 5 | 수정 (조건 추가) | "전체 정확도로 정규화한 클래스별 정확도의 변화를 클래스마다 Welch t-검정(p≤0.05, 5회 반복)으로 판정한 수(§4 p. 5, Table 1 p. 6). 하위 k개 평균이 아님." |
| 2 | 평균 68.64 → 68.93~70.83 | NDS 68.64, DS T=2 68.93 … T=10 70.83 | Table 1, p. 6 | 일치 | – |
| 3 | CIFAR-100 R56→R20: T=2 5개, T=10 22개 | 동일 | Table 1, p. 6 | 일치 | – |
| 4 | (빠짐) ViT 쌍 | ViT-Base→TinyViT T=10 #SC 426 | Table 1, p. 6 | 추가 | "ViT-Base→TinyViT는 T=10에서 426/1000개(Table 1, p. 6). 41%는 ResNet 쌍 수치." |
| 5 | CelebA: 온도↑ EOD·DPD↓ (6.11 → T=4 5.36) | 수치 일치. 과제 = smiling, 민감 속성 = Young. 감소는 단조 아님(T=5 EOD 5.39 > T=4 5.36; DPD T=6 9.38 > T=5 9.30), T=20·30·40에선 추세 유지 안 됨 | Table 2 p. 10; p. 8, p. 10 | 수정 | "CelebA(smiling 라벨, Young 속성, ResNet-50→ResNet-18)에서 EOD 6.11(NDS)→5.36(T=4)→4.98(T=10), 단조 감소 아님. T=20–40에선 추세가 유지 안 됨(p. 10)." |
| 6 | 하드웨어 = 확인 안 함 | 부록 C: 소형 CNN RTX A4000 16GB, ImageNet·Tiny ResNet 4×RTX 3090(ImageNet 약 3일, Tiny 하루 미만), ViT 4×A100 40GB(ViT-Base 약 2일) | 부록 C, p. 18 | 수정 | "학습 하드웨어: RTX A4000, 4×RTX 3090(ImageNet 약 3일), 4×A100(ViT-Base 약 2일)(부록 C, p. 18). 증류 대 비증류 비용 비교는 아님." |
| 7 | 설정: 데이터셋·모델 쌍·온도 2~10·5회 | 일치. α=0.8 고정(p. 6), 공정성 표는 T=1부터 | p. 6; Table 2 | 추가 | "모든 실험 α=0.8(p. 6). 공정성 표는 T=1–10, 부록에 T=20·30·40." |
| 8 | 공정성: CelebA, Trifeature, HateXplain / 개별 공정성 | 있음 (Table 2, 3, 4) | pp. 10–12 | 일치 | – |
| 9 | 잃는 것: 맥락에 따라 판단 | "it's important to interpret these changes based on the domain-specific impact" (공정성 지표 맥락) | p. 10 | 일치 | – |
| 10 | 위치: Fig. 1·T1 p. 6, Fig. 3 p. 8, T2 p. 10, T3·4 pp. 11–12 | 맞음 | – | 일치 | – |
| 11 | 「읽기 전에」: 04를 "하위 k개 클래스" 단위로 분류 | 04는 하위 k 평균을 쓰지 않음(유의 변화 클래스 수, EOD·DPD) | §4 p. 5 | 수정 | "04번은 '정규화한 클래스별 변화의 유의성 검정' 단위. 하위 k개 묶음에서 분리." |
| 12 | 메모: EOD·DPD는 WGA와 다른 지표 | 맞음 | – | 일치 | – |

## Eval-05 When Masking Helps or Hurts Robustness in Compressed CLIP
- 본 판본: arXiv:2609.39704v1
- 대조 결과: 일치 8건 / 수정 4건 / 추가 3건 / 확인 불가 1건

| # | 메모의 서술 | 원문 | 위치 | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | "의미 기반 마스킹으로 토큰을 줄였을 때" | 평가 대상은 저자들의 **MARS**: 비지도 전경/배경 분할(PCA+k-means) + 배경 픽셀 마스킹 + 토큰 128/256 유지 | §1 p. 1; §3 p. 3 | 수정 | "평가한 마스킹은 저자들이 만든 MARS(비지도 분할 → 배경 픽셀 0 → 상위 50% 토큰으로 재인코딩)(§3, p. 3)." |
| 2 | Table 1: FastV +2.3, EViT −4.3, PatchRank −5.2, 텍스트 −36.7, 마스킹 −31.6 | 동일 (FiCoCo +2.3, PACT −7.1도 있음) | Table 1, p. 6 | 일치 | – |
| 3 | "8개 데이터셋 평균" | 캡션은 8개 평균이라 하나 MARS −31.6은 Table 5의 정의된 **7개** 상대 변화 평균((25.6+82.5−19.6−39.7−70.0−100−100)/7=−31.6). ImageNet-D는 0/0으로 정의 안 됨 | Table 1 p. 6; Table 5 p. 17 | 수정 | "MARS −31.6%는 상대 변화가 정의되는 7개 데이터셋 평균(ImageNet-D는 0/0). 표 캡션의 '8개 평균'과 다름." |
| 4 | Table 2: Waterbirds 50.8→67.3, CelebA 79.6→64.7, IN-9 87.5→26.3, Pets 52.6→0.0 | 동일 | Table 2, p. 7 | 일치 | – |
| 5 | 원인: 배경 패치의 텍스트 유사도 역전 | "background patches receive higher CLIP text-similarity than the true object" | 초록 p. 1; Fig. 1 p. 3 | 일치 | – |
| 6 | 옮겨 간 비용: 분할 병목 3.5× → 1.75× | 일치. 구성: 첫 전체 forward ≈15.7 + CPU 분할 28.5 + 마스킹 후 128토큰 forward 11.0 = 55.2 ms; GPU 분할 0.81 ms는 **batch 128**에서 잰 값, 나머지는 batch 1 | §5.5 p. 8; Table 7 p. 17 | 추가 | "MARS는 forward 2회(256토큰 전체 + 128토큰 재인코딩)가 필수. 27.5 ms는 batch 128의 GPU 분할(0.81 ms)과 batch 1 forward를 합친 값(Table 7, p. 17)." |
| 7 | 단위 = 이미지당 지연(ms), 하드웨어 = A100 1장 | "Per-image latency (ms) on a single NVIDIA A100" | Table 7, p. 17 | 일치 | – |
| 8 | 일회성 비용 셈? 해당 없음 | SIM은 데이터셋당 한 번 계산하는 "fixed, one-time overhead amortized to zero" — 추론 비용에서 제외, 수치 없음 | §5.5, p. 8 | 수정 | "일회성 비용 셈? N — SIM 보정 계산은 일회성이라 0으로 상각한다고 적고 시간은 보고 안 함(§5.5, p. 8)." |
| 9 | 설정: "OpenCLIP ViT-L/14 등 CLIP 6종", "가짜 상관 벤치마크 8종" | 주 결과는 OpenCLIP ViT-L/14 laion2b_s32b_b82k 하나. 6종은 §5.4 일반화 실험. OxfordPets·Caltech101·ImageNet-D는 "define worst-group accuracy over class identity" | §4 p. 5; §5.4 p. 7 | 수정 | "주 결과(Table 1·2)는 OpenCLIP ViT-L/14(LAION-2B) 1종, 6종은 별도 일반화 실험. 8종 중 OxfordPets·Caltech101·ImageNet-D는 클래스를 그룹으로 씀(§4, p. 5)." |
| 10 | (빠짐) seed | 3 seed는 Waterbirds·UrbanCars·CelebA만, 나머지는 단일 패스. UrbanCars·CelebA의 분산은 분할 알고리즘 민감도 | §4 p. 5; 부록 A p. 12 | 추가 | "3 seed는 Waterbirds·UrbanCars·CelebA만(UrbanCars·CelebA는 같은 표본, k-means 초기화 분산). 나머지 5종은 1회." |
| 11 | 평균 외: WGA 상대 변화, 나빠진/좋아진 수 | 있음. 평균 정확도도 MARS 48.2 vs 기준 79.9 | Table 1, p. 6 | 일치 | – |
| 12 | 정정 줄: 7/8 p=.035, 동률 제외 6/7 p=.063; 지연 55.2/27.5/15.7 | 원문 그대로 | §5.2 p. 6; Table 7 p. 17 | 일치 (정정 줄 맞음) | – |
| 13 | 정정 줄: 절대 WGA 표와 상대 변화 표 불일치 | Table 2로 계산: Waterbirds +32.5%, UrbanCars +76.8%, CelebA −18.7% vs Table 5 +25.6, +82.5, −19.6 | Table 2 p. 7; Table 5 p. 17 | 일치 (정정 줄 맞음) | – |
| 14 | (빠짐) 아키텍처 표의 수치는 주 표와 다름 | Table 4의 ViT-L/14(laion) Waterbirds MARS WG 59.0 vs Table 2 67.3. 부호 판정 31/48(p=0.030) | Table 4 p. 16; §5.4 p. 8 | 추가 | "아키텍처 일반화 표(Table 4)의 WGA는 주 표와 다른 평가값(같은 ViT-L/14 laion에서 Waterbirds MARS 59.0 vs 67.3). 두 표를 섞지 않음." |
| 15 | 위치: T1 p. 6, Fig. 3·T2 p. 7, T3 p. 12, T7·8 p. 17 | 맞음 | – | 일치 | – |
| 16 | 메모: MaskedKD 대비, "우리 Waterbirds 15%에서 38.6% 대 62.2%" | 이 논문 밖의 내용 | – | 확인 불가 (프로젝트 수치·타 논문) | 실험 출처(run) 링크 필요 |

## Eval-06 Later Is Better: Token Reduction under Distribution Shift
- 본 판본: PDF에 arXiv 표기 없음 ("Preprint"), 메모상 2610.07758
- 대조 결과: 일치 6건 / 수정 3건 / 추가 4건 / 확인 불가 0건

| # | 메모의 서술 | 원문 | 위치 | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | DeiT-S, IN-C 강도 5, 15종 평균, 연산 −26%: 37.99→39.16(+1.17), 압축 전 39.40 | 동일. 조건: **ToMe**, rbase=8, γ=2. late 3.15G vs flat 3.20G vs 압축 전 4.25G (−26%는 압축 전 대비) | Table 1 p. 7; Fig. 6 p. 7 | 수정 (조건 추가) | "ToMe, rbase=8, γ=2 기준. late 3.15 GFLOPs ≤ flat 3.20 GFLOPs, −26%는 압축 전 4.25G 대비(Table 1·Fig. 6, p. 7)." |
| 2 | 연산 −7%에서 +0.26 | rbase=2, 3.97G, +0.26 | Table 1, p. 7 | 일치 | – |
| 3 | 잃는 것: flat −7% 39.14 → −37% 36.67 | 동일 | Table 1, p. 7 | 일치 | – |
| 4 | 옮겨 간 비용: 해당 없음 (추가 추론 비용 없음) | "at no extra inference cost" (초록) | p. 1 | 일치 | – |
| 5 | (빠짐) 실측 지연 | DeiT-S batch 64 단일 GPU: 압축 전 67.1 ms/batch(954 img/s), flat·late 모두 54.6 ms(1172 img/s), 차이 0.5% 이내 | Table 33, p. 35 (부록 V) | 추가 | "실측: batch 64, 단일 GPU에서 flat·late 모두 54.6 ms/batch(1172 img/s), 압축 전 67.1 ms(954 img/s)(Table 33, p. 35)." |
| 6 | 하드웨어 = 확인 안 함 | "single GPU", 종류 미표기 | p. 35 | 수정 | "하드웨어: 단일 GPU, 종류 미표기(부록 V, p. 35)." |
| 7 | 설정: "추론만" | 주 비교는 추론 전용("inference-only … unless a TTA method is stated"). 그 밖에 TTA 6종(Tent, EATA, SAR, FOA, SPA, NEO), NaBirds-C·DomainNet-126은 미세조정 모델, 비디오·VQA | §5 p. 7; §5.5 p. 9 | 수정 | "주 비교는 추론 전용. 별도로 TTA 6종 위 적용, NaBirds-C·DomainNet-126(미세조정), 비디오(VideoMAE)·VQA(LLaVA)도 평가(§5, pp. 7–9)." |
| 8 | 평균 외: IN-C 15종×강도, 3DCC, A/R/V2/Sketch/DomainNet-126, NaBirds-C | 모두 있음. 빠진 것: **ImageNet-C̄**(+1.45). Table 5 이미지 설정은 DeiT-S가 아닌 **ViT-S** | Table 5, p. 9 (렌더링 확인) | 추가 | "Table 5에 ImageNet-C̄(flat 47.28 → late 48.74)도 있음. Table 5 이미지 설정의 백본은 ViT-S(p. 9)." |
| 9 | (빠짐) 강도별 이득 | 깨끗한 데이터 +0.28 → 강도 5 +1.17 (4.2×) | Table 2, p. 8 | 추가 | "이득은 강도에 따라 단조 증가: clean +0.28 → 강도 5 +1.17%p(Table 2, p. 8)." |
| 10 | 단위 = GFLOPs | 맞음 | – | 일치 | – |
| 11 | 위치: T1 p. 7, T2·3 p. 8, T4·5 p. 9 | 맞음 | – | 일치 | – |
| 12 | 메모: "얼마나"와 "어떻게" 분리 | early < flat < late, early는 연산·토큰을 더 쓰고도 최악 | §5.1 pp. 7–8 | 일치 | – |
| 13 | (빠짐) TTA와 합칠 때 비용 | 적응 단계 GFLOPs −26%(±0.9pp), wall-clock −17.5~24.7% | 부록 K, p. 27 | 추가 | "TTA와 함께 쓸 때 적응 단계 GFLOPs −26%, wall-clock −17.5~24.7%(부록 K, p. 27)." |

## Eval-07 DRoP: Distributionally Robust Data Pruning
- 본 판본: arXiv:2404.05579v4
- 대조 결과: 일치 6건 / 수정 4건 / 추가 2건 / 확인 불가 0건

| # | 메모의 서술 | 원문 | 위치 | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | CIFAR-100 30%: 무작위 24.7 → +DRoP 34.3, 전체 35.8, 50%에서 45.3 | Table 2 동일. 본문은 "35.8% to 45.4%" | Table 2 p. 22; §5 p. 7 | 일치 | (선택) "본문 p. 7은 45.4%, 표는 45.3%." |
| 2 | 평균 59.1 → 57.2 | 동일 | Table 2, p. 22 | 일치 | – |
| 3 | GraNd 30% 평균 40.4 / 최악 12.3; TinyImageNet GraNd·Forgetting 1.0 | 동일 | Table 2, p. 22 | 일치 | – |
| 4 | 옮겨 간 비용: 선행 학습, "숫자는 확인 안 함" | query model을 전체 학습 길이의 10%로 학습(VGG-19 CIFAR-100 16 epoch, ResNet-18 TinyImageNet 9 epoch — Table 1의 200 epoch 기준으로는 4.5%라 원문 불일치). 검증 = 테스트셋 50% 떼어 씀. 전체 연구 약 45,000 GPU-시간 | 부록 A p. 16; 부록 E p. 23 | 수정 | "DRoP는 전체 데이터로 query model을 학습 길이의 약 10%만큼 학습(16 epoch/CIFAR-100, 9 epoch/TinyImageNet; 후자는 Table 1의 200 epoch와 맞지 않음)해 클래스별 검증 오류를 잼. 검증셋은 원 테스트셋의 50%(최종 평가 제외). 연구 전체 약 45,000 GPU-시간(부록 A·E, pp. 16, 23)." |
| 5 | 일회성 비용 셈? N | 저자는 기존 프루닝도 같은 사전학습이 필요해 "does not introduce any additional computational overhead"라고 주장, 예산에 포함 안 함 | 부록 E, p. 23 | 일치 | – |
| 6 | 하드웨어 = 확인 안 함 | "NVIDIA RTX8000 GPUs" | 부록 A, p. 16 | 수정 | "하드웨어: RTX8000 클러스터(부록 A, p. 16)." |
| 7 | 설정: "ResNet 계열, 3 seed" | VGG-16(CIFAR-10), VGG-19(CIFAR-100), ResNet-18(TinyImageNet), ResNet-50(ImageNet·Waterbirds), WRN-101. 3 seed 맞음. 0.3/0.5/0.8은 Table 2 한정 | Table 1, p. 16 | 수정 | "모델: CIFAR-10 VGG-16, CIFAR-100 VGG-19, TinyImageNet ResNet-18, ImageNet·Waterbirds ResNet-50(Table 1, p. 16). 0.3/0.5/0.8은 Table 2의 밀도." |
| 8 | 정정 줄: Waterbirds WGA(Fig. 9, 그룹 고려 선택, ResNet-50) | 있음. Waterbirds 평균 정확도는 학습 그룹 비율 가중 | Fig. 9, p. 10 | 일치 (정정 줄 맞음) | – |
| 9 | (빠짐) Waterbirds 평균의 가중 방식 | "average accuracy as a sum of group accuracies weighted by the original training group proportions" → 저밀도에서 평균 급락 설명 | Fig. 9 캡션, p. 10 | 추가 | "Waterbirds 평균 정확도는 원 학습 그룹 비율 가중(Fig. 9 캡션, p. 10)." |
| 10 | 메모: "무작위가 점수 기반보다 최악 클래스에서 안전" | CIFAR-100 0.3에서 Forgetting 26.3 > 무작위 24.7; CIFAR-10 0.3에서 GraNd 77.0·Forgetting 79.1 > 무작위 75.5 | Table 2, p. 22 | 수정 | "무작위가 늘 안전한 것은 아님: CIFAR-100 30%에서 Forgetting 26.3% > 무작위 24.7%. 안전한 것은 '무작위 + DRoP' 조합(Table 2)." |
| 11 | 위치: T1 p. 16, T2 p. 22 | 맞음 | – | 일치 | – |
| 12 | (빠짐) 평균 밖 지표 세트 | 클래스 편향 지표 3종: "(1) worst-class accuracy mink rk, (2) difference between the maximum and the minimum recall …, and (3) standard deviation of recalls" | §2, p. 3 | 추가 | "클래스 편향 지표는 최악 클래스 정확도, 최대−최소 recall 차, recall 표준편차의 3종(§2, p. 3)." |

## Eval-08 Impact of Coreset Selection on Spurious Correlations
- 본 판본: arXiv:2507.11690v2
- 대조 결과: 일치 6건 / 수정 4건 / 추가 2건 / 확인 불가 0건

| # | 메모의 서술 | 원문 | 위치 | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | Waterbirds 40%: 무작위 58.2, R-Gbal 83.3, EL2N 어려운 74.6, 쉬운 30.2 | 동일 | Table 1, p. 8 | 일치 | – |
| 2 | CelebA hair: 쉬운 79.0 > 어려운 38.3 | EL2N 기준 동일 | Table 1, p. 8 | 일치 | – |
| 3 | 벤치마크 10종, 점수 5종, 정책 5종 | 점수 EL2N·Uncertainty·Forgetting·SelfSup·SupProto; 정책 Difficult·Easy·Stratified·Median + Random, 별도로 R-Gbal 오라클 | pp. 4–5 | 일치 | (선택) "정책 4종 + Random, R-Gbal은 오라클 기준선." |
| 4 | 잃는 것: 어려운 샘플이 편향 최저이나 아주 작은 비율에선 WGA 하락 | 맞음. 단 "least biased"는 학습 기반 점수에 한정 | §4.2–4.3 pp. 7–9 | 수정 | "편향이 가장 낮은 부분집합은 학습 기반 점수(EL2N 등)의 Difficult 선택에서 나옴. 작은 비율 결과는 Fig. 3·Table 4." |
| 5 | 옮겨 간 비용: 점수 계산, 숫자 없음 | surrogate 학습 epoch 명시(EL2N·Uncertainty 20 또는 5 epoch, Forgetting 200 또는 20 epoch), RTX-3090 24GB, 전체 약 7,500 GPU-시간 | 부록 B, pp. 16–17 | 수정 | "점수용 surrogate 학습 epoch는 부록 B에 명시. 하드웨어 RTX-3090 24GB, 연구 전체 약 7,500 GPU-시간(p. 17). 선택 자체의 비용 비교는 없음." |
| 6 | (빠짐, 중요) 학습 반복 수 고정 | "keep the number of training iterations fixed … setting the number of training epochs to N/s" → 40% 코어셋도 학습 연산량은 전체와 같음 | §3 p. 5; 부록 B.4 p. 17 | 추가 | "하류 학습 반복 수를 전체 데이터 기준으로 고정(epoch = N/s, p. 5). 40% 선택이 학습 연산 60% 절감을 뜻하지 않음." |
| 7 | 하드웨어 = 확인 안 함 | RTX-3090 | p. 17 | 수정 | (5번 문구에 포함) |
| 8 | 일회성 비용 셈? N | 점수 계산을 선택 예산에 포함 안 함 | – | 일치 | – |
| 9 | 평균 외: WGA, 편향 수준, 평균 10%p 붕괴 표시 | 있음 ("10.0 worse than the corresponding random baseline") | Table 1 캡션 p. 8 | 일치 | – |
| 10 | (빠짐) 평균 정확도 정의·seed | 평균 = 그룹 정확도를 원 학습 그룹 비율로 재가중. seed 수 표기 없음 | p. 5; 전문 검색 | 추가 | "평균 정확도는 원 학습 그룹 비율 가중(p. 5). seed 수·체크포인트 선택 규칙 표기 없음." |
| 11 | 위치: T1 p. 8, T2 p. 16, T3–5 pp. 19–20 | 맞음 | – | 일치 | – |
| 12 | 메모: R-Gbal = 그룹 라벨을 쓰는 상한, 무작위(58.2)가 비교 대상 | 저자 표현은 "R-Gbal acts as an upper bound"이나 수치상 상한 아님: cMNIST 84.4 < EL2N Diff 87.2, CivilComments 72.0 < SelfSup Easy 79.3, Nico 40.0 < 44.0, MultiNLI 57.6 < 65.6. 무작위는 **클래스 균형** 무작위 | Table 1 p. 8; p. 5 | 수정 | "R-Gbal은 그룹 라벨을 쓰는 오라클 기준선(저자 표현 'upper bound', 수치상 상한 아님: cMNIST 84.4 < 87.2). 무작위는 클래스 균형 무작위(p. 5) — 우리 샘플러와 같은지 확인 필요." |

## Eval-09 Group DRO (Waterbirds)
- 본 판본: arXiv 표기 없음 (ICLR 2020, PDF 생성일 2020-04-03)
- 대조 결과: 일치 5건 / 수정 2건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 | 원문 | 위치 | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 표준: ERM 97.3/60.0, DRO 97.4/76.9 | Waterbirds Test 행 동일 | Table 1, p. 5 | 일치 | – |
| 2 | 강한 L2: ERM 95.7/21.3, DRO 96.6/84.6 | 동일 | Table 1, p. 5 | 일치 | – |
| 3 | 조기 종료: ERM 93.8/6.7, DRO 93.2/86.0 | 수치 동일. 여기서 "조기 종료"는 **고정 1 epoch 학습**("we train each ResNet50 model for 1 epoch"). 표준은 Waterbirds 300 epoch, 강한 L2는 λ=1.0·lr 1e-5로 학습률도 바뀜 | 부록 C.2, pp. 18–19 | 수정 (조건 추가) | "Table 1의 조기 종료 = 1 epoch 고정 학습, 검증 기반 선택 아님. 표준 = 300 epoch, 강한 L2 = λ=1.0, lr 1e-5(부록 C.2, p. 19). 모두 테스트 결과." |
| 4 | 옮겨 간 비용: 해당 없음 | 알고리즘 실행 시간 "less than a 5% difference" vs SGD | §5, p. 9 | 추가 | "그룹 DRO 온라인 알고리즘의 실행 시간은 같은 epoch의 SGD와 5% 미만 차이(§5, p. 9)." |
| 5 | 하드웨어 = 확인 안 함 | 원문에 GPU 표기 없음 | 전문 검색 | 일치 (보고 없음) | (선택) "보고 안 함." |
| 6 | 설정: Waterbirds·CelebA(ResNet-50, 사전학습), MultiNLI(BERT) | "torchvision implementation … starting from pretrained weights" | 부록 C.2 p. 18 | 일치 | – |
| 7 | 평균 외: 최저 그룹 표준 정의 | 맞음 | §2 | 일치 | – |
| 8 | (빠짐) val/test 구성과 평균 가중 | Waterbirds val·test는 라벨 안에서 배경 균형; 평균은 "weighted average, with weights corresponding to the relative proportion of each group in the (skewed) training dataset" | 부록 C.1, p. 18 | 추가 | "Waterbirds 검증·테스트는 라벨 안에서 배경 균형, 평균 테스트 정확도는 학습 그룹 비율 가중(부록 C.1, p. 18)." |
| 9 | (빠짐) Table 3 벤치마크의 선택 규칙 | "All benchmark models were evaluated at the best early stopping epoch (as measured by robust validation accuracy)" | 부록 C.2, p. 19 | 추가 | "Table 3 벤치마크는 검증 WGA 최고 epoch에서 평가(그룹 라벨 사용)." |
| 10 | 메모: 60.0→6.7 → "마지막 epoch, 조기 종료 없음 고정"의 근거 | 6.7%는 1 epoch 학습의 ERM. 최종 epoch 보고가 낫다는 근거는 아님. Fig. 2는 CelebA | Table 1, Fig. 2 p. 5 | 수정 | "같은 ERM이 학습 길이·정규화에 따라 60.0%(300 epoch)에서 6.7%(1 epoch)까지 바뀜. 종료 규칙을 고정·명시해야 한다는 근거이지, 마지막 epoch가 낫다는 근거는 아님." |

## Eval-10 Last Layer Re-Training is Sufficient (DFR)
- 본 판본: arXiv:2204.02937v2
- 대조 결과: 일치 6건 / 수정 2건 / 추가 2건 / 확인 불가 0건

| # | 메모의 서술 | 원문 | 위치 | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | Waterbirds ERM 74.9/98.1 → DFR 92.9/94.2 (GDRO 91.4/93.5); CelebA 46.9/95.3 → 88.3/91.3 | 동일 | Table 2, p. 7 | 일치 | – |
| 2 | (빠짐) 변형 이름·절차 | DFR^Val_Tr: 검증 절반으로 정규화 강도 선택 → 검증 전체의 그룹 균형 부분집합으로 로지스틱 회귀 10회 학습 후 가중 평균. 5회 독립 실행 평균±표준편차 | §6, p. 7 | 추가 | "Table 2는 DFR^Val_Tr: 검증셋 절반으로 ℓ1 강도 튜닝, 검증 전체의 그룹 균형 부분집합으로 10회 적합 평균, 5회 실행(p. 7)." |
| 3 | 잃는 것: 그룹 라벨 검증 데이터로 재학습, 평균 98.1→94.2 | 맞음 | Table 2 | 일치 | – |
| 4 | 옮겨 간 비용: 수 분/GPU 1장 (초록) | 초록 "minutes of training on a single GPU". 조건: 1.2M Stylized ImageNet, "after extracting the embeddings on all of the datapoints, which only needs to be done once". GPU 종류 미표기 | 초록 p. 1; §8 p. 10 | 수정 | "'수 분/GPU 1장'은 Stylized ImageNet 1.2M에 대한 마지막 층 학습 시간이며, 임베딩 1회 추출 이후 기준(p. 10). 추출 비용·GPU 종류는 보고 안 됨." |
| 5 | 일회성 비용 셈? 해당 없음 | 임베딩 추출은 일회성, 수치 없음 | p. 10 | 수정 | "일회성 비용 셈? N(임베딩 추출을 시간에 포함 안 함)." |
| 6 | 설정: ResNet-50 ImageNet 사전학습, BERT, 5회 | 맞음 | p. 7 | 일치 | – |
| 7 | 평균 외: WGA, 그룹 라벨 사용 열, 배경·질감 | 맞음 (Group Info Train/Val 열, §7) | Table 2 p. 7; §7 | 일치 | – |
| 8 | 평균 = 학습 그룹 비율 가중 | "weight the group accuracies according to their prevalence in the training data" | Table 2 캡션 | 일치 | – |
| 9 | 위치: Table 2 p. 7 | 맞음 | – | 일치 | – |
| 10 | (빠짐) 기반 ERM의 종료 규칙 | "We do not use early stopping." Waterbirds 100 epoch, CelebA 50 epoch | 부록, p. 25 | 추가 | "기반 ERM은 조기 종료 없이 Waterbirds 100 epoch, CelebA 50 epoch 학습(p. 25) — 마지막 epoch 체크포인트 위에 DFR 적용." |

## Eval-11 Group Robustness is in the Details
- 본 판본: arXiv:2407.13957v2
- 대조 결과: 일치 4건 / 수정 4건 / 추가 1건 / 확인 불가 0건

| # | 메모의 서술 | 원문 | 위치 | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 업샘플링·손실 가중은 epoch이 지날수록 WGA 하락 (초록) | 초록 일치. 붕괴는 **CelebA·CivilComments**에서 관찰, "optimally tuned early-stopping appears to mitigate the collapse"; CelebA는 표준보다 3× 긴 학습 | §3.1 pp. 4–5; Fig. 1 | 수정 (조건 추가) | "붕괴는 클래스 불균형이 큰 CelebA·CivilComments에서 긴 학습(조기 종료 없음) 중 관찰. 조기 종료를 잘 맞추면 완화(§3.1, p. 5)." |
| 2 | 최악 클래스 선택이 WGA 선택과 "대부분" 일치 (81.1, 74.1, 77.6) | 수치 일치. 최악 클래스 정확도는 3개 데이터셋 **모두** 같은 선택(Mixture 2:1, Subsetting, Mixture 3:1). 편향 비지도 점수는 Waterbirds에서 Upsampling(79.9)로 다름 | Table 1, p. 7 | 수정 | "검증 최악 클래스 정확도는 3개 모두 WGA 검증과 같은 선택(Waterbirds Mixture 2:1 81.1%, CelebA Subsetting 74.1%, CivilComments Mixture 3:1 77.6%). 편향 비지도 점수만 Waterbirds에서 Upsampling 79.9%." |
| 3 | 모델 키우면 적절한 클래스 균형화와 함께일 때만 유리 | 맞음 | §4, p. 7 | 일치 | – |
| 4 | 설정: "모델 구조는 확인 안 함", 3 seed | ResNet·ConvNeXt-V2(주)·Swin(ImageNet-1K 사전학습), BERT; 3 seed; "We do not utilize early-stopping … train models to completion" | §2 p. 4 | 수정 | "모델: ConvNeXt-V2(주)·ResNet·Swin(ImageNet-1K), BERT. 3 seed, 조기 종료 없이 수렴까지 학습(p. 4)." |
| 5 | 하드웨어 = 확인 안 함 | 부록 A.2 "four … P100" vs Compute 절 "two … P100", + 2× RTX A5000; GCP 5,000달러 미만 재현 추정 | p. 16; p. 25 | 추가 | "하드웨어: Tesla P100(4장 p. 16 / 2장 p. 25로 원문 불일치) + RTX A5000 2장." |
| 6 | 평균 외: WGA와 그룹 라벨 없는 대용 지표 검증 | 맞음 | Table 1 | 일치 | – |
| 7 | 위치: Table 1 p. 7 | 맞음 | – | 일치 | – |
| 8 | 메모: 우리 실험은 선택에 그룹 라벨을 안 쓰니 "그룹 주석 없음" 행과 같은 조건 | ✗ 행도 검증 최악 클래스 정확도 또는 편향 비지도 점수로 균형화 방법을 **고름**. 선택 없이 고정 설정·마지막 epoch를 쓰는 것과 다름. 테스트 WGA는 그룹 라벨로 잼 | Table 1, p. 7 | 수정 | "✗ 행은 그룹 라벨 없는 검증 지표로 방법을 고른 결과. 고정 설정·마지막 epoch 보고와 같은 조건 아님. 공통점은 이 논문도 조기 종료 없이 끝까지 학습한다는 점." |
| 9 | 메모: epoch에 따라 WGA가 내려갈 수 있어 "마지막 epoch만" 설계의 위험 | 맞음(1번 조건과 함께) | §3.1 | 일치 | – |

---

## 「읽기 전에」·「정리 메모」 교차 항목

| # | 메모의 서술 | 원문 | 판정 | 고칠 문구 |
|---|---|---|---|---|
| X1 | 비교 단위: 하위 k개 클래스(01, 04, 07) | 04는 정규화 클래스별 변화의 유의성 검정; 07은 최악 클래스 + Waterbirds WGA | 수정 | "하위 k개 클래스(01), 최악 클래스(07, Waterbirds WGA 포함), 유의 변화 클래스 수(04)." |
| X2 | 속성으로 정의한 최저 그룹(02, 03, 05, 08–11) | 05는 8종 중 3종이 클래스 = 그룹 | 수정 | "05는 5종만 속성 그룹, 3종(Pets·Caltech101·ImageNet-D)은 클래스 = 그룹." |
| X3 | 11편 중 효율 KD에서 그룹 지표를 본 논문 없음 | 위 05–08 판정과 일치 | 일치 | – |
| X4 | 02·03의 teacher는 편향을 줄인 모델 | 둘 다 Group DRO teacher (02 p. 11, 03 p. 5) | 일치 | "편향을 줄인 = 그룹 라벨로 Group DRO 학습." |

---

## 전체 요약

- **가장 심각한 오류:** 08번에서 비용 조건이 빠짐. 하류 학습 반복 수를 전체 데이터 기준으로 고정했으므로(epoch = N/s) 40% 코어셋이 학습 연산을 줄이지 않음. 이 사실이 없으면 "예산 축소" 근거로 잘못 읽힘. 다음으로 05번의 MARS −31.6%가 캡션의 "8개 평균"이 아니라 정의되는 7개의 평균이라는 점, 09번의 6.7%가 1 epoch 고정 학습 결과라 "마지막 epoch 고정"의 근거가 되지 않는다는 점이 큼.
- **패턴 1 — 조건 누락:** 표의 수치 자체는 11편 모두 정확함(전 수치 재확인, 오기 0건). 틀린 곳은 거의 모두 *어느 실험·teacher·분할의 수치인지*임. 예: 01 Table 5는 ResNet-50과 조기 종료 없는 teacher, 02·03 teacher는 Group DRO, 04는 정규화 유의성 검정, 06은 ToMe·γ=2, 11의 ✗ 행은 그래도 검증 지표로 고른 결과.
- **패턴 2 — "하드웨어 확인 안 함"·"숫자 보고 안 됨"이 실제로는 보고된 경우가 많음:** 04(RTX 3090·A100, 일수), 06(Table 33 지연), 07(RTX8000, 45,000 GPU-시간), 08(RTX 3090, 7,500 GPU-시간), 11(P100·A5000, 장수 불일치). 01은 체크리스트에서 미보고를 명시. 정성적 비용 진술도 빠짐: 03 "nonnegligible overhead", 09 "<5%".
- **패턴 3 — 평균 집계 방식이 섞임:** 02는 가중 없는 테스트 평균, 07(Waterbirds)·08·09·10·11(Waterbirds)은 학습 그룹 비율 가중 평균. 메모는 09·10에만 표기함.
- **기존 정정 줄:** 01·02·03·05·07의 정정 줄은 원문과 맞음. 01 정정 줄의 v2 쪽 링크(#page=6)만 저장소 PDF(표 3 = p. 5)와 쪽이 다를 수 있음(판 표기가 없어 확인 불가).
