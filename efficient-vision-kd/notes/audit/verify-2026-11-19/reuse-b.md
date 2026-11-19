# 원문 대조 보고서 — `notes/3_Reuse.md` 08–14번

- 대조 대상: 저장소 사본 `notes/3_Reuse.md` 152–244행 (08–14번 블록, 09번 팀원 정리 줄 포함)
- 원문: `Related Work/Efficient KD papers/3_Reuse/08_…pdf` ~ `14_…pdf` (저장소 PDF, 쪽 번호는 PDF 쪽, 첫 장 = 1)
- 추출 텍스트: `verify/tools/reuseb_txt/NN.txt` (쪽 표시 포함). TCS 표 1은 이미지로 렌더링해 확인 (`verify/tools/reuseb_txt/14_p5.png`).
- 메모의 08–14번에는 `**정정 (…)**` 줄이 없음. `notes/audit/paper-by-paper.md` 130–136행에 이 7편의 검수 결과가 있으나 메모에 반영되지 않은 상태. 이 보고서는 그 검수 결과도 원문으로 다시 확인함 (검수 결과와 일치하는 지점은 "검수 보고서와 같음"으로 표시).

---

## Reuse-08 비최대 억제를 통한 밀집 객체 검출을 위한 지식 저장 증류 (손수호·송병철)
- 본 판본: 전자공학회논문지 Vol.62 No.1 (2025.1), pp. 95–98, doi 10.5573/ieie.2025.62.1.95 (PDF 4쪽)
- 대조 결과: 일치 11건 / 수정 3건 / 추가 5건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | NMS로 고른 핵심 특징만 미리 저장, 학습 때 재사용 | "Soft-NMS-Gaussian[4]을 도입"; "과 을 증강 정보와 함께 저장"; 학습 시 "저장한 증강 케이스 중에서 무작위로 하나의 케이스를 불러온다" | §II.1–2, p. 2 | 수정 | 효율화 방식: teacher 검출기의 FPN 특징 맵에서 Soft-NMS-Gaussian으로 고른 탐지 상자 위치의 특징만, 전경·배경으로 나누고 좌표·증강 정보와 함께 미리 저장. 학습 때는 저장된 증강 케이스 하나를 무작위로 불러와 student 특징을 같은 위치에서 맞춤. 학습 중 teacher forward 없음. (§II.1–2, p. 2) |
| 2 | GFL 제안 1.05배, FGD 1.68, PGD 1.51, BCKD 1.42 | 표 1 그대로 | 표 1, p. 3 | 일치 | – |
| 3 | FCOS 제안 1.06배, FGD 1.61배 | 표 1: FGD 1.61x, PGD 1.52x, BCKD 1.41x, Ours 1.06x | 표 1, p. 3 | 추가 | FCOS: 제안 1.06배, FGD 1.61배, PGD 1.52배, BCKD 1.41배. (표 1, p. 3) |
| 4 | 학습 시간 배수 = student 단독 1배 기준 | 표 1 GFL-R50(S), FCOS-R50(S) 행이 "1x" | 표 1, p. 3 | 일치 | – |
| 5 | GFL 43.8 > FGD 43.4, FCOS 39.6 < FGD 41.0 (1.4 낮음) | 표 1 그대로. FCOS에서 PGD 39.2, BCKD 39.1보다는 높음 | 표 1, p. 3 | 추가 | FCOS에서 FGD(41.0)보다 1.4 낮지만 PGD(39.2)·BCKD(39.1)보다는 높음. FCOS FGD는 teacher(40.8)보다도 높음. (표 1, p. 3) |
| 6 | 저장 용량 GFL 12 GB, FCOS 6.8 GB | 표 1 그대로 | 표 1, p. 3 | 일치 | – |
| 7 | 전경만 8 GB 43.6, 배경만 4 GB 42.7 | 표 3 그대로. 제거 실험은 GFL 기준: "제거 실험에서는 기준 검출기로 GFL 검출기를 사용한다" | 표 3 p. 3, §III.3 p. 4 | 수정 | 저장 영역을 전경만으로 줄이면 8 GB에 43.6 mAP, 배경만이면 4 GB에 42.7 mAP (GFL, 표 3, p. 3). (검수 보고서와 같음) |
| 8 | (빠짐) 특징 맵을 그대로 저장하면 필요한 용량 | "COCO 훈련 데이터셋에 대한 교사 모델의 중간 특징을 그대로 저장했을 때 4.0TB의 저장 공간이 필요하다" (MMDetection 기본 증강 = 랜덤 플립 가정) | 서론, p. 1 | 추가 | 선택 없이 teacher 중간 특징을 그대로 저장하면 COCO에서 4.0 TB 필요 (랜덤 플립 증강 기준, p. 1). 제안 방법의 12 GB는 그 약 1/330. |
| 9 | (빠짐) NMS 방법별 저장량 | 표 2: Greedy-NMS 13.6 GB/43.6, Soft-NMS-linear 14.4 GB/43.7, Soft-NMS-Gaussian 12 GB/43.8, DIoU-NMS 14.4 GB/43.6 | 표 2, p. 3 | 추가 | NMS 방법별 저장량과 mAP (GFL): Greedy 13.6 GB·43.6, Soft-linear 14.4 GB·43.7, Soft-Gaussian 12 GB·43.8, DIoU 14.4 GB·43.6. (표 2, p. 3) |
| 10 | 특징 추출·저장 시간 보고 안 됨 | 본문·표 어디에도 저장 단계 시간 없음 | 전체 | 일치 | – |
| 11 | teacher forward 셈? Y (비교 대상 학습 시간에 포함) | 표 1 비교 대상은 온라인 KD라 teacher forward가 시간에 들어간다고 볼 수 있으나 논문이 따로 밝히지 않음. 비효율성 정의: "매 반복마다 이미지를 교사 모델에 전달하는 과정" | 서론 p. 1, 표 1 p. 3 | 일치 (추론) | "Y (비교 대상의 학습 시간에 포함된 것으로 해석, 논문에 별도 분해 없음)"로 표기 권장 |
| 12 | 일회성 비용: 저장 용량 Y, 저장 단계 시간 N | 위와 같음 | – | 일치 | – |
| 13 | 하드웨어 RTX A6000 8장 | "모든 검출기를 8개의 RTX A6000 GPU에서 배치 크기 32" | §III.1, p. 3 | 일치 | – |
| 14 | 설정: COCO 120k/5k, 12 epoch, batch 32 | "120k개의 학습 이미지 … 5k개의 검증 이미지"; "1X(12 에폭)"; 초기 학습률 0.02 | §III.1, p. 3 | 일치 | – |
| 15 | 평균 정확도 외 평가: 없음 (mAP, AP50, AP75) | 그림 2 특징 맵 시각화(teacher·student·제안 방법 비교)가 있음 | 그림 2, p. 3 | 수정 | 평균 정확도 외 평가: 정량 지표는 mAP·AP50·AP75뿐. 정성 평가로 특징 맵 시각화 (그림 2, p. 3). 강건성·보정 등은 없음. |
| 16 | 위치: 표 1–3 (p. 3) | 표 1–3 p. 3, 표 4 p. 4, 본문 결과 해석 p. 4 | – | 추가 | 위치: 방법 §II (p. 2), 표 1–3·그림 2 (p. 3), 표 4·결과 해석 (p. 4) |
| 17 | "5% 추가 시간으로 3.8 mAP" 류 주장 | "5%의 추가적인 시간 소비로 3.8mAP의 성능 향상" (GFL, 12 GB) | §III.2, p. 4 | 일치 | (메모에는 없지만 1.05배와 같은 내용) |
| 18 | 표 3은 저장 예산 곡선의 예 | 표 3은 영역 선택 3가지(전경/배경/둘 다)의 점 3개. 분리 여부는 같은 12 GB | 표 3, p. 3 | 일치 | (점이 3개뿐이라 "곡선"보다 "저장량–mAP 점 3개"가 정확) |
| 19 | 분류가 아닌 검출, 국내 사례 | 그대로 | – | 일치 | – |

---

## Reuse-09 Are Large-scale Soft Labels Necessary for Large-scale Dataset Distillation? (LPLD)
- 본 판본: arXiv 2410.15919v2 (3 Nov 2024), NeurIPS 2024
- 대조 결과: 일치 13건 / 수정 5건 / 추가 7건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 클래스별 배치로 클래스 내 다양성 ↑, 무작위로 솎아도 유지 | 세 요소: 클래스 내 배치(re-batch), class-wise BN supervision, 개선된 label pool(다른 epoch의 batch로 새 epoch 구성, batch 단위 무작위 선택) | 초록 p. 1, §3.3 p. 6–7, Table 4 p. 8 | 수정 | 효율화 방식: 데이터셋 증류(합성 이미지)의 사후 학습용 soft label 저장량을 줄임. 합성 단계에서 같은 클래스끼리 배치를 묶고 클래스별 BN 통계로 감독해 클래스 내 다양성을 높임. 이후 label을 batch 단위로 무작위 솎아내고, 여러 epoch의 batch를 섞은 label pool에서 무작위로 다시 씀. (§3.3, p. 5–7) |
| 2 | IN-1K IPC200: 113 GB → 2.8 GB (40배), +2.6%p. "(초록, Table 7)" | 초록: "from 113 GB to 2.8 GB (40× compression) with a 2.6% performance gain". 113.33 GB는 Table 24. 정확도는 Table 3: LPLD 40× 59.6% vs 압축 안 한 SRe2L 57.0%. Table 7은 21K-P | 초록 p. 1, Table 3 p. 7, Table 24 p. 21 | 수정 | ImageNet-1K IPC200: soft label 113.33 GB → 약 2.8 GB (40배). ResNet-18 정확도 59.6% vs 압축 안 한 SRe2L 57.0% (+2.6%p). (초록 p. 1, Table 3 p. 7, Table 24 p. 21) |
| 3 | 21K-P IPC20: 1285 GB → 32 GB | Table 7: L 1285G(1×) → 32G(40×); 정확도 29.4% vs SRe2L 20.5%(1×). 본문 "8.9% accuracy improvement" | Table 7·본문, p. 9 | 추가 | ImageNet-21K-P IPC20: 1,285 GB → 32 GB (40배), 정확도 29.4% vs 압축 안 한 SRe2L 20.5%. (Table 7, p. 9) |
| 4 | 압축률↑ 하락: R-50 IPC50 62.2 → 61.2 → 56.2 → 44.7 | Table 6b: 1× 62.2, 10× 61.2, 30× 56.2, 100× 44.7 | Table 6b, p. 9 | 일치 | – |
| 5 | 전체 데이터 76.13% vs IPC50 62.24% | Table 6c ResNet-50 Full Acc 76.13, 1× 62.24 | Table 6c, p. 9 | 일치 | – |
| 6 | 클래스별 BN 통계 +36.64 MB, 합성 단계에만 필요, Table 25 | Table 25 Diff. 36.64 MB (IN-1K, ResNet-18). "one-time necessity during the recovery phase". 절 번호는 E.4 | Table 25·App. E.4, p. 23 | 일치 | (위치를 "부록 E.4"로 적으면 더 정확. E.5는 연산 자원 절) |
| 7 | (빠짐) BN 통계 계산 비용 | "we compute class-wise BN statistics by training for one epoch with model parameters kept frozen"; IN-1K 5,005 BN updates | §4.1 p. 7, Table 10 p. 16 | 추가 | squeeze 단계에서 모델을 고정한 채 원본 학습 데이터 1 epoch(IN-1K 5,005 업데이트)를 돌려 클래스별 BN 통계를 계산. 시간은 보고 안 됨. (p. 7, Table 10 p. 16) |
| 8 | 합성 자체 비용은 기존 방법과 같은 틀 | recover 4,000 iteration, "follows CDA except by changing the batch size to an IPC-dependent size" | Table 11·C.1, p. 17 | 일치 | – |
| 9 | teacher forward 셈? N / 일회성 비용 셈? N | 합성·재라벨링·학습 시간 수치 없음 (전문 검색) | 전체 | 일치 | – |
| 10 | 하드웨어 = A100 80 GB 4장 | "we use 4 GPU cards for ImageNet-1K and ImageNet-21K-P experiments and 1 GPU card for all Tiny-ImageNet experiments" | App. E.5, p. 23 | 수정 | 하드웨어 = A100 80 GB, ImageNet-1K·21K-P는 4장, Tiny-ImageNet은 1장. (App. E.5, p. 23) (검수 보고서와 같음) |
| 11 | teacher·평가 student 주로 ResNet-18 | "Pytorch pretrained ResNet-18 … as both the recovery and relabeling model"; 표의 검증 모델 ResNet-18. 21K-P는 Timm 사전학습 모델 | §4.1 p. 7, C.1 p. 17 | 일치 | (검증 학습 300 epoch, Table 12 p. 17 추가 가능) |
| 12 | 평균 정확도 외 평가: 없음 (다른 구조 일반화만, Table 6c) | student 평가는 top-1뿐. 합성 데이터 자체의 다양성 지표(feature cosine similarity Table 1, MMD Fig. 3)와 시각화(Fig. 2, App. F)는 있음 | Table 1 p. 4, Fig. 3, Fig. 2 p. 2 | 추가 | 평균 정확도 외 평가: student 쪽은 없음 (top-1과 교차 구조 Table 6c뿐). 합성 데이터의 클래스 내 다양성은 특징 코사인 유사도(Table 1, p. 4)와 MMD(Fig. 3)로 잼. |
| 13 | 위치: 초록·Fig.1 p.1, Table 2·3 p.7, Table 6–9 p.9, Table 21–24 p.20–21, Table 25·E.5 p.23 | 모두 해당 쪽에 있음 | – | 일치 | – |
| 14 | 팀원: +2.6%p는 가장 약한 기준선(압축 안 한 SRe2L 57.0%) 대비 | Table 3 IPC200 1×: SRe2L 57.0, CDA 63.3, Ours 62.6 | Table 3, p. 7 | 일치 | – |
| 15 | (빠짐) 1×에서는 CDA보다 낮음 | IPC200 1×: CDA 63.3% > LPLD 62.6%. IPC200 40×의 LPLD 59.6%는 압축 안 한 CDA보다 3.7%p 낮음 | Table 3, p. 7 | 추가 | 압축 없는 1×에서는 CDA(63.3%)가 LPLD(62.6%)보다 높음 (IN-1K IPC200, Table 3, p. 7). 40배 압축 LPLD(59.6%)는 압축 안 한 CDA보다 3.7%p 낮음. |
| 16 | 팀원: 같은 압축률 IPC200 40×에서 CDA 54.4 vs LPLD 59.6 (+5.2%p) | Table 3 그대로 | Table 3, p. 7 | 일치 | – |
| 17 | 팀원: IPC10에서 1× 34.6 → 40× 20.2 | Table 3 그대로 | Table 3, p. 7 | 일치 | – |
| 18 | 팀원: squeeze 단계에서 원본 데이터 1 epoch 더 돌아 BN 통계 계산 | §4.1 그대로 (모델 고정) | p. 7 | 일치 | – |
| 19 | 팀원: 생성 비용 그대로, 학습 epoch 그대로라 학습 시간도 줄지 않음 | "the process for generating the soft labels is still necessary"; "reducing the required labels may not directly lead to a reduced training speed, as the total training epochs remain the same" — 측정 없음 | App. E.6, p. 23 | 수정 | label 전체를 만든 뒤 고르는 방식이라 생성 비용(teacher forward)은 그대로. 학습 epoch도 그대로라 학습 시간이 줄지 않을 수 있다고 저자가 적음. 시간 측정은 없음. (App. E.6, p. 23) |
| 20 | 메모: "학습 속도가 줄지는 않는다고 스스로 적음 (p. 23)" | 위와 같음 ("may not") | App. E.6, p. 23 | 수정 | label 수를 줄여도 학습 epoch이 그대로라 학습 시간이 줄지 않을 수 있다고 저자가 적음 (측정 없음, App. E.6, p. 23). |
| 21 | 메모: 113 → 2.8 GB는 응축 데이터의 label 저장량 | Table 24: 이미지 2.98 GB, label 113.33 GB (IPC200) | Table 24, p. 21 | 일치 | – |
| 22 | (빠짐) label 대 이미지 저장 비율 | Table 24: IN-1K label/image 37–38배, Tiny 21–22배, 21K-P 214–257배 | Table 24, p. 21 | 추가 | 압축 전 label 저장량은 이미지의 37–38배 (IN-1K), 21–22배 (Tiny-ImageNet), 214–257배 (21K-P). (Table 24, p. 21) |
| 23 | (빠짐) 전체 ImageNet 대비 총 저장량 | Table 20: IPC400 30× 총 13 GB에 65.2%, 전체 ImageNet-1K 138 GB에 69.8% (ResNet-18) | Table 20, p. 19 | 추가 | IPC400·30배 압축은 이미지+label 총 13 GB에 65.2%, 전체 ImageNet-1K(138 GB)는 69.8% (ResNet-18, Table 20, p. 19). |
| 24 | (빠짐) FKD 양자화와의 비교 | Table 23: IPC10에서 LPLD 10× 32.70% vs FKD Re-Norm K=50 8.2× 18.10%. FKD는 logit 차원만 줄이고, 증강 정보 1–5번 성분은 압축하지 않음 (Table 21) | Table 21–23, p. 20 | 추가 | FKD 식 label 양자화와 비교: IN-1K IPC10에서 LPLD 10배 32.70% vs FKD 8.2배 18.10%. FKD는 logit 차원만 줄이고, 이 방법은 증강 수를 줄임. (Table 21–23, p. 20) |
| 25 | 30배 압축 시 저장량 예 | 본문: "the storage is reduced from 29G to 0.87G" (IN-1K IPC50, 30× SRe2L 1× 초과) | §4.2, p. 8 | 일치 | (메모에는 없음. 필요하면 인용 가능) |

---

## Reuse-10 TinyCLIP
- 본 판본: arXiv 2309.12314v1 (21 Sep 2023)
- 대조 결과: 일치 9건 / 수정 3건 / 추가 4건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 가중치 상속 + 유사도(affinity) 모방, 여러 단계 | 초록·§3 그대로. 수동/자동(학습 마스크) 상속 | 초록 p. 1, §3 p. 3–5, Alg. 1 p. 5 | 일치 | – |
| 2 | OpenCLIP ViT-B/32 LAION-400M 32 epoch 약 287 GPU일 | "takes around 287 GPU days when trained on LAION-400M for 32 epochs" | Fig. 6·본문, p. 8 | 일치 | – |
| 3 | 75% 모델 7.8배, 50% 모델 1.4배 | "Our models with 75% and 50% parameters reach the similar accuracy … speed up of 7.8× and 1.4×" | p. 8 | 일치 | – |
| 4 | (빠짐) teacher와 비교 기준의 학습 데이터 차이 | teacher: "OpenCLIP ViT-B/32 pre-trained on LAION-2B" (65.6%/Table 1 65.7%). 비교 기준: LAION-400M에서 처음부터 학습한 OpenCLIP ViT-B/32 (62.9%) | §4.1 p. 5, Table 1 p. 6, Fig. 6 p. 8 | 추가 | 주의: teacher는 LAION-2B로 학습한 OpenCLIP ViT-B/32(ImageNet 제로샷 65.7%)이고, 7.8배·1.4배의 비교 기준은 LAION-400M에서 처음부터 학습한 OpenCLIP ViT-B/32(62.9%)임. (§4.1 p. 5, Table 1 p. 6) |
| 5 | 62.9% → ViT-45M/32 61.2% (Table 5) | Table 5 ImageNet 열 62.9 → 61.2 (Elevater 평가). Table 1 같은 모델은 61.4% | Table 5 p. 8, Table 1 p. 6 | 추가 | 같은 ViT-45M/32가 Table 1에서는 61.4%, Table 5(Elevater)에서는 61.2%. |
| 6 | 한 번에 25%로 47.0%, 두 단계 49.0% (Table 6) | Table 6: 1단계 2 epoch 47.0; 2단계 각 1 epoch, 최종 49.0 | Table 6, p. 8 | 일치 | (조건: LAION-400M 총 2 epoch) |
| 7 | 마스크 학습이 추가됨. 둘 다 절감 계산에 넣지 않음 | Alg. 1: 각 단계 첫 LM=3,000 iteration에서 마스크와 가중치를 teacher 증류 손실로 같이 최적화. "updated in the first 3,000 training iterations" — 학습 일정 안에 있음. 분리된 시간 보고는 없음 | Alg. 1·§4.1, p. 5 | 수정 | 옮겨 간 비용: teacher(LAION-2B 학습) 비용은 이미 있다고 가정, 절감 계산에 없음. 자동 상속의 마스크 학습은 각 단계 첫 3,000 iteration 안에서 이루어져 학습 일정에 포함되나, 따로 분리해 보고하지 않음. (Alg. 1, p. 5) (검수 보고서와 같음) |
| 8 | 단위 GPU일, 추론 처리량 | 처리량: "measured on Nvidia V100 … batch size is 1,024" | p. 5 | 일치 | – |
| 9 | teacher forward 셈? 포함으로 보이나 명시 없음 | Alg. 1 두 단계 모두 "distillation loss between origin teacher model" 계산 → 매 스텝 teacher forward 있음. GPU일의 분해는 없음 | Alg. 1, p. 5 | 일치 | – |
| 10 | 하드웨어 V100 또는 A100 32장 | "All models are trained on 32 Nvidia V100 or A100" | p. 5 | 일치 | – |
| 11 | 설정: 2단계 6+16 epoch | Fig. 6 비교만 6+16. 전체 LAION 일정은 3단계 6+16+16, YFCC-15M은 2단계 25+25 | §4.1 p. 5, Fig. 6 p. 8 | 수정 | 설정: LAION-400M 3단계 6+16+16 epoch (100→75→50→25%), YFCC-15M 2단계 25+25 epoch. Fig. 6의 학습 시간 비교는 앞 2단계(6+16 epoch). (p. 5, p. 8) |
| 12 | student 크기 63M+31M, 45M+18M | Fig. 6 범례 그대로 | Fig. 6, p. 8 | 일치 | – |
| 13 | 평균 외 평가: 강건성 Table 7, 23개 데이터셋 제로샷·선형 Table 5 | 그대로. 추가로 Flickr30k·MSCOCO 검색 R@1 (Table 1) | Table 1 p. 6, Table 5·7 p. 8 | 추가 | 평균 정확도 외 평가: 제로샷 강건성 (IN-V2·A·R·ObjectNet·Sketch, Table 7), 23개 데이터셋 제로샷·선형 평가 (Table 5), 이미지–텍스트 검색 R@1 (Flickr30k·MSCOCO, Table 1). |
| 14 | 위치: 초록 p.1, Table 1 p.6, Table 3·4 p.7, Table 5–7·Fig.6 p.8 | 모두 맞음. Alg. 1과 학습 설정은 p. 5 | – | 수정 | 위치: 초록 (p. 1), Alg. 1·학습 설정 (p. 5), Table 1 (p. 6), Table 2–4 (p. 7), Table 5–7·Fig. 6 (p. 8) |
| 15 | 메모: 재사용 대상이 가중치, teacher forward 그대로, 분모는 처음부터 학습 | 그대로 | p. 5, p. 8 | 일치 | – |
| 16 | (빠짐) 추론 처리량 수치 | Table 1: ViT-B/32 2,452 pairs/s → ViT-45M/32(auto) 3,682 (1.5×), ViT-22M/32 5,504 (2.2×) | Table 1, p. 6 | 추가 | 추론 처리량 (V100, batch 1,024): OpenCLIP ViT-B/32 2,452 pairs/s → TinyCLIP ViT-45M/32 3,682 (1.5배). (Table 1, p. 6) |

---

## Reuse-11 Initializing Models with Larger Ones (Weight Selection)
- 본 판본: arXiv 2311.18823v1 (30 Nov 2023)
- 대조 결과: 일치 10건 / 수정 2건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 큰 모델 가중치 일부를 균등하게 골라 초기화. 증류 손실·teacher forward 없음 | 세 단계: "layer selection, component mapping, and element selection"; 층은 first-N 선택이 기본(Table 6), 원소는 균등 선택 | p. 2, Fig. 2 p. 3, Table 6 p. 8 | 수정 | 효율화 방식: 같은 계열의 사전학습된 큰 모델에서 층 선택(앞쪽 N개), 구성요소 대응, 원소 선택(균등 간격)의 세 단계로 가중치 일부를 골라 작은 모델을 초기화. 증류 손실도 teacher forward도 없음. (p. 2–4, Fig. 2 p. 3) |
| 2 | CIFAR-100 ViT-T 1/3 epoch으로 같은 정확도 (Fig. 4a) | "the same performance on CIFAR-100 can be obtained with only 1/3 epochs"; Fig. 4a: 100 epoch 73.0% vs 무작위 300 epoch 72.3% | Fig. 4a, p. 7 | 일치 | (수치 73.0 vs 72.3 덧붙이면 좋음) |
| 3 | IN-1K 사전학습+미세조정 대비 6.12배, 사전학습 60 epoch | "it takes 60 epochs of pretraining on ImageNet-1K to achieve the same performance … weight selection is 6.12x faster"; Fig. 4b: 60 epoch 81.5% vs WS 81.4%; 미세조정은 CIFAR-100 300 epoch | Fig. 4b, p. 7–8 | 일치 | (6.12배의 계산식은 원문에 없음) |
| 4 | IN-1K ViT-T 73.9 → 75.5, KD 74.8, 합치면 76.0 (Table 4) | Table 4 그대로. 이 실험의 teacher는 "ImageNet-1K pretrained ViT-S model from DeiT"(KD와 WS 공통). Table 2에서는 같은 설정 WS가 75.6% | Table 4 p. 7, §4.4 p. 6, Table 2 p. 5 | 추가 | Table 4의 IN-1K 실험 teacher는 IN-1K 사전학습 ViT-S(DeiT)로, KD와 가중치 선택에 같이 씀. 같은 WS 수치가 Table 2에서는 75.6%로 적혀 있음. CIFAR-100 특징 증류: 72.3 → KD 78.4, WS 81.4, 합 83.9%. (Table 4, p. 7) |
| 5 | 충분히 사전학습한 모델보다 낮음: 300 epoch 85.7 vs 81.4 | Fig. 4b 그대로 | Fig. 4b, p. 7 | 일치 | – |
| 6 | 같은 계열 필요, teacher가 클수록 좋은 것 아님 (Table 8) | Table 8: ViT-S 81.4, ViT-B 77.6, ViT-L 76.9 | Table 8, p. 8 | 일치 | – |
| 7 | 선택 자체 추가 연산 없다고 적음, 숫자 보고 안 됨 | "adds no extra computational cost compared to training from scratch" | p. 2 | 일치 | – |
| 8 | 하드웨어 보고 안 됨 | 본문·부록에서 GPU 표기 없음 (전문 검색) | 전체 | 일치 | – |
| 9 | teacher forward 해당 없음, 일회성 N | 그대로 | – | 일치 | – |
| 10 | 설정: IN-21K ViT-S → ViT-T, ConvNeXt-T → ConvNeXt-F | Table 1·§4.1 그대로 | Table 1, p. 5 | 일치 | – |
| 11 | ImageNet-1K와 소규모 데이터셋 8종 | 원문은 "9 image classification datasets"라 적고, 실제 목록·Table 2는 ImageNet-1K 포함 10개 (CIFAR-10/100, Flowers, Pets, STL-10, Food-101, DTD, SVHN, EuroSAT) | §4.1·Table 2, p. 5 | 수정 | 설정: ImageNet-1K와 소규모 데이터셋 9종 (CIFAR-10·100, Flowers, Pets, STL-10, Food-101, DTD, SVHN, EuroSAT). 원문은 "9 datasets"로 적었으나 표 2에는 10개. (p. 5) |
| 12 | 평균 외 평가: 선형 평가 (Table 9). 그 외 없음 | Table 9 (p. 9) 있음. 추가로 self-attention 시각화(Fig. 5, p. 10) | Table 9 p. 9, Fig. 5 p. 10 | 추가 | 평균 정확도 외 평가: 선형 평가 (CIFAR-100, Table 9, p. 9), self-attention 가중치 시각화 (Fig. 5, p. 10). |
| 13 | 위치 Table 2 p.5, Fig.3·Table 3 p.6, Table 4·Fig.4 p.7, Table 5–8 p.8 | 모두 맞음. Table 9–11 p. 9, Table 12·Fig. 5 p. 10 | – | 추가 | 위치에 "Table 9–11 (p. 9), Table 12·Fig. 5 (p. 10)" 추가 |
| 14 | 메모: Table 4는 가중치 재사용과 증류 기여를 분리 | 그대로 | Table 4, p. 7 | 일치 | – |
| 15 | (확인) 더 긴 학습에서도 이득 유지 | Table 11: 600 epoch, ViT-T 76.3 → 77.4 | Table 11, p. 9 | 일치 | (메모에 없음, 필요 시 인용) |

---

## Reuse-12 SimKD (Knowledge Distillation with the Reused Teacher Classifier)
- 본 판본: PDF에 arXiv 번호·학회 표기 없음 (본문 8쪽 + 부록, 총 15쪽)
- 대조 결과: 일치 8건 / 수정 3건 / 추가 2건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 투영층 + teacher 분류기 재사용, 손실은 L2 하나 | "reuse the discriminative classifier … train a student encoder through feature alignment with a single ℓ2 loss"; Eq. 3 제곱 ℓ2 | 초록 p. 1, Eq. 3 p. 4 | 일치 | – |
| 2 | 학습 비용 절감 수치 보고 안 됨 | 시간·연산 수치 없음 (전문 검색) | 전체 | 일치 | – |
| 3 | 1/4 epoch KD 52.75 vs 61.73, 1/2 66.69 vs 69.26, 전체 71.29 vs 71.66 | Table 3 그대로. 전체 일정 = 120 epoch ("total 120 training epochs") | Table 3 p. 6, p. 4 | 수정 | 적은 epoch에서의 정확도 (ImageNet, ResNet-50 → ResNet-18, 전체 120 epoch): 30 epoch(1/4) KD 52.75% vs SimKD 61.73%, 60 epoch(1/2) 66.69 vs 69.26%, 120 epoch 71.29 vs 71.66%. (Table 3, p. 6) |
| 4 | 투영층이 추론에도 남아 매개변수 증가, "프루닝 비율 비용" (Fig. 6·7) | "our added projector only incurs less than 1% pruning ratio cost in most cases (10/14)"; 초록 "less than 3%" | Fig. 6·7, p. 7 | 추가 | 투영층으로 늘어나는 매개변수는 프루닝 비율 비용으로 14쌍 중 10쌍에서 1% 미만, 기본 r=2. (Fig. 7, p. 7) |
| 5 | 새 분류기 학습 시 크게 떨어짐: ResNet-8x4 51.97 vs 78.08 | Table 4 "Sequential" 열: 정렬된 인코더를 고정하고 새 분류기를 학습. ResNet-8x4(teacher ResNet-32x4) 51.97 vs 78.08 | Table 4, p. 6 | 수정 | [실측·원논문] 정렬된 student 인코더를 고정하고 새 분류기를 처음부터 학습하면 크게 떨어지는 조합이 있음 (CIFAR-100 ResNet-32x4 → ResNet-8x4: 51.97 vs 78.08%; ShuffleNetV2 21.56 vs 78.39%). (Table 4, p. 6) |
| 6 | teacher forward 매 스텝 필요 | Eq. 3 목표가 teacher 특징 fᵗ | Eq. 3, p. 4 | 일치 | – |
| 7 | 단위 = epoch, 추가 매개변수 비율 | Table 3, Fig. 6·7 | – | 일치 | – |
| 8 | 하드웨어 = RTX 2080 Ti 8장 (CIFAR) | "CIFAR-100 … eight NVIDIA GeForce RTX 2080Ti"; "ImageNet experiments … four NVIDIA A40 GPUs" | App. A.1, p. 9 | 수정 | 하드웨어 = CIFAR-100은 RTX 2080 Ti 8장, ImageNet은 A40 4장. (부록 A.1, p. 9) (검수 보고서와 같음) |
| 9 | 설정: CIFAR-100 여러 쌍, ImageNet ResNet-50 → ResNet-18 | "The top-1 test accuracy of the teacher model (ResNet-50)"; Table 3 student ResNet-18 | p. 9, Table 3 p. 6 | 일치 | – |
| 10 | 평균 외 평가: NLL (Fig. 5), t-SNE. 그 외 없음 | Fig. 5 NLL p. 6, Fig. 3 t-SNE p. 4. p. 8에는 다중 teacher (Table 6)·data-free (Table 7) 정확도 실험 | Fig. 3 p. 4, Fig. 5 p. 6, Table 6·7 p. 8 | 일치 | (t-SNE 위치 "Fig. 3, p. 4" 추가 권장) |
| 11 | 위치: Table 1·2 p.5, Table 3·4·Fig.5 p.6, Fig.6·7 p.7, 부록 p.9–10 | 모두 맞음. 부록은 p. 9–15 | – | 추가 | 위치: Fig. 3 (p. 4), Table 1·2 (p. 5), Table 3·4·Fig. 5 (p. 6), Fig. 6·7 (p. 7), Table 5–7 (p. 8), 부록 (p. 9–15) |
| 12 | 메모: 분류기 재사용이라 teacher forward 생략과 무관 | 그대로 | – | 일치 | – |
| 13 | 적은 epoch 결과는 Skip 결합 근거로만 | 해석. 원문 "SimKD achieves faster convergence in training" | p. 4 | 일치 | – |

---

## Reuse-13 MobileSAM (Faster Segment Anything)
- 본 판본: arXiv 2306.14289v2 (1 Jul 2023; 첫 쪽 날짜 July 4, 2023)
- 대조 결과: 일치 8건 / 수정 3건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 이미지 인코더만 증류, 디코더는 그대로, teacher 임베딩 미리 저장 | "save the image embeddings beforehand so that we only need to run the forward process once"; 디코더 미세조정은 선택이며 "we omit this step" | Fig. 3 p. 4, p. 5–6 | 일치 | (디코더 미세조정은 이번 판에서 생략했다는 점 덧붙이면 좋음) |
| 2 | 1% 미만 자원으로 mIoU 0.75 vs 0.72. 결합 GPU 128·180k·1,100만, 분리 GPU 2·55k·1.1만 (Table 2, ViT-B) | Table 2: GPU 128/2, batch 128/4, iteration 180k/55k, 데이터 11M/11K, mIoU 0.72/0.75 ("averaged on 200 samples") | Table 2·본문, p. 5 | 추가 | Table 2는 ViT-B 인코더 비교: 결합 GPU 128장·batch 128·180k 반복·1,100만 장 vs 분리 GPU 2장·batch 4·55k 반복·1.1만 장(SA-1B의 0.1%), mIoU 0.72 vs 0.75 (200개 표본 평균). (p. 5) |
| 3 | MobileSAM 전체 학습 GPU 1장 하루 미만 | "With a single GPU, we can obtain our MobileSAM in less than a day" — 임베딩 저장 단계 포함 여부는 적지 않음 | p. 5 | 일치 | (임베딩 저장 시간이 포함됐는지 불명, 주석 권장) |
| 4 | mIoU 0.71~0.74 (Table 4) | Table 4: 0.7057 / 0.7286 / 0.7447 (batch 4·2 epoch·50k, 8·4·50k, 8·8·100k) | Table 4, p. 7 | 일치 | – |
| 5 | 표준 분할 벤치마크 수치는 본문에서 확인 안 함 | 전문에 사람 정답 기반 벤치마크 없음. 평가는 모두 원래 SAM 마스크와의 mIoU ("assuming that the mask generated by ViT-H is ground-truth") | p. 5–7 | 수정 | 원래 SAM과의 차이를 사람 정답 기반 분할 벤치마크로 잰 수치는 보고 안 됨. 모든 mIoU는 원래 SAM 마스크를 정답으로 본 일치도. (p. 5–7) |
| 6 | 옮겨 간 비용: 임베딩 사전 계산·저장, 숫자 보고 안 됨 | 저장 용량·시간 수치 없음 | p. 5 | 일치 | – |
| 7 | 단위 = GPU 수 × 반복 수 × 데이터 수 (대략적 비교) | 원문은 결합 단위를 정의하지 않음. GPU 수, batch, 반복, 데이터 수를 나열하고 "less than 1% of the computation resources"라고만 적음 | Table 2, p. 5 | 수정 | 단위 = GPU 수·batch·반복·데이터 수를 나란히 적고 "1% 미만"으로 요약 (결합 단위 정의 없음), 추론 시간(ms). (Table 2·3, p. 5) (검수 보고서와 같음) |
| 8 | teacher forward 저장 단계 1회, 시간 N / 일회성 N | 그대로 | p. 5 | 일치 | – |
| 9 | 하드웨어 GPU 1장 (종류 보고 안 됨) | GPU 종류 표기 없음. 인용된 원래 SAM 학습만 "68 hours on 256 A100 GPUs" | p. 3, p. 5 | 일치 | – |
| 10 | 설정: ViT-H (632M) → TinyViT (5.78M), SA-1B 1%로 8 epoch | Table 1·3, "1% of the SA-1B dataset … for 8 epochs on a single GPU" | Table 1 p. 2, Table 3·본문 p. 5 | 일치 | – |
| 11 | (빠짐) 추론 시간 | Table 3: 이미지 인코더 452 ms → 8 ms. 초록: 전체 약 10 ms (인코더 8 + 디코더 4). Table 6은 12 ms vs FastSAM 64 ms인데 본문은 10 ms vs 40 ms로 적어 원문 안에서 수치가 다름 | Table 3 p. 5, Table 6 p. 7 | 추가 | 추론 시간 (GPU 1장): 이미지 인코더 452 ms → 8 ms (Table 3, p. 5). FastSAM 비교는 Table 6(12 vs 64 ms)과 본문(10 vs 40 ms)의 수치가 서로 다름 (p. 7). |
| 12 | 평균 외 평가: 없음 (mIoU, 정성 비교) | point/box 정성 결과 (Fig. 4–5), segment everything 정성 비교 (Fig. 6), FastSAM과 prompt 거리별 mIoU (Table 7) | p. 6–8 | 추가 | 평균 정확도 외 평가: 없음. 정량은 원래 SAM과의 mIoU뿐 (FastSAM과의 prompt 거리별 비교 Table 7, p. 7). 정성 비교 Fig. 4–6 (p. 6–8). |
| 13 | 위치: 초록 p.1, Table 2·3 p.5, Table 4–7 p.7 | 맞음. 방법(Fig. 2·3) p. 4 | – | 수정 | 위치: 초록 (p. 1), 방법·Fig. 2·3 (p. 3–4), Table 2·3 (p. 5), Table 4–7 (p. 7) |
| 14 | 메모: 재사용 두 겹 (디코더 가중치 + teacher 임베딩) | Fig. 3 "copy" 디코더, 임베딩 저장 | p. 4–5 | 일치 | – |

---

## Reuse-14 All You Need in Knowledge Distillation Is a Tailored Coordinate System (TCS)
- 본 판본: arXiv 2412.09388v2 (12 Feb 2025), AAAI 2025
- 대조 결과: 일치 9건 / 수정 3건 / 추가 4건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | PCA로 좌표계 한 번, 증강 없는 teacher forward 한 번, 이후 teacher 불필요 | "extracting the teacher's features through the training dataset … does not involve data augmentation, which means only one forward pass is needed". 지도 teacher일 때 eLSH 해시 목표 h도 증강 없이 한 번 계산 ("only one forward pass is needed to compute h") | p. 2, Fig. 2 p. 3, p. 4 | 추가 | 효율화 방식 보충: 좌표계 계산은 학습 데이터 전체에 대한 증강 없는 teacher forward 1회. 지도 teacher에서는 eLSH 모듈의 표본별 해시 목표도 같은 1회 forward로 미리 계산. SSL teacher(few-shot)에서는 eLSH를 쓰지 않음. (p. 2–5) |
| 2 | CIFAR-100: epoch당 KD 49.90 → TCS 32.14초 (처음부터 29.34) | Table 1 (ViT-S teacher) 그대로 | Table 1, p. 5 | 일치 | – |
| 3 | GPU 메모리 KD 7.5 → TCS 5.8 GB (처음부터 3.5) | Table 1 그대로 | Table 1, p. 5 | 일치 | – |
| 4 | ImageNet DeiT-T teacher: KD 415.33 → TCS 328.24초 | Table 2 그대로. 메모리 7.5 → 5.8 GB. Swin-T 701.80 → 321.27, Mixer-B 869.97 → 341.88 | Table 2, p. 5 | 추가 | ImageNet-1K epoch당 시간: DeiT-T teacher KD 415.33 → TCS 328.24초, Swin-T 701.80 → 321.27초, Mixer-B 869.97 → 341.88초. 메모리 KD 7.5–7.6 → TCS 5.8 GB. (Table 2, p. 5) |
| 5 | 정확도 손실 없음 (KD 77.26 → TCS 81.37) | Table 1·2에서는 그대로. few-shot Table 3에는 예외: IN-1K DINOv2 1-shot TCS(ViT-B) 34.2 vs baseline 37.0·IbM2 37.2, CUB DINOv2 2-shot 64.5 vs baseline 66.0 | Table 1 p. 5, Table 3 p. 6 | 수정 | [실측·원논문] 일반 KD 표(Table 1·2)에서는 정확도 손실 없음 (CIFAR-100 KD 77.26% → TCS 81.37%). few-shot에서는 1·2-shot에서 기준보다 낮은 경우가 있음 (IN-1K DINOv2 1-shot 34.2 vs 37.0, Table 3, p. 6). 메모리는 처음부터 학습보다 2.3 GB 많음 (Table 1). |
| 6 | 옮겨 간 비용: forward 1회 + PCA, "무시할 만하다"고 적고 숫자 없음 | "the cost involving the teacher is in fact negligible"; 시간·저장량 수치 없음 | p. 2 | 일치 | – |
| 7 | 단위·teacher forward Y·일회성 N | Table 1 KD 열은 teacher forward 포함 온라인 KD | Table 1, p. 5 | 일치 | – |
| 8 | 하드웨어 = RTX 3090 1장 | Table 1 캡션: "All metrics were measured on a single NVIDIA 3090 GPU". Table 2(ImageNet) 캡션에는 하드웨어 표기 없음 | Table 1·2, p. 5 | 수정 | 하드웨어 = RTX 3090 1장 (CIFAR-100 Table 1 캡션). ImageNet Table 2의 장치는 명시 없음. (p. 5) |
| 9 | (빠짐) 비교 정확도의 출처 | Table 1 캡션: "♯ indicates results run by us, and other results are directly obtained from respective papers" | Table 1, p. 5 | 추가 | Table 1의 기존 방법 정확도는 LSHL2(♯)만 저자가 직접 돌렸고 나머지는 각 논문에서 가져옴. (p. 5) |
| 10 | 설정: teacher ViT-S·ConvNeXt-T·Mixer-B/16·DeiT-T·Swin-T → ResNet-18, CIFAR-100 224 확대, IN-1K 100 epoch | 그대로 | Table 1·2·본문, p. 5 | 일치 | – |
| 11 | few-shot은 MoCo v3·DINOv2 사전학습 모델 | Table 3: teacher ViT-B/ViT-L, student ViT-S. CUB·IN-1K, 3개 무작위 부분집합 평균 | Table 3, p. 6 | 일치 | – |
| 12 | 평균 외 평가: few-shot (Table 3). 그 외 없음 | few-shot은 top-1과 3회 표준편차. PCA 데이터 출처 실험(Table 5)은 정확도 | Table 3 p. 6, Table 5 p. 7 | 일치 | – |
| 13 | 위치: Fig.1 p.2, Table 1·2 p.5, Table 3 p.6, Table 4·5 p.7 | 맞음. 방법 그림 Fig. 2는 p. 3 | – | 수정 | 위치: Fig. 1 (p. 2), 방법·Fig. 2 (p. 3–4), Table 1·2 (p. 5), Table 3 (p. 6), Table 4·5 (p. 7) |
| 14 | 메모: ViT-S teacher KD = 처음부터의 1.7배, Mixer-B/16 2.4배 | 49.90/29.34 = 1.70, 71.34/29.34 = 2.43 — 계산 맞음. 이 비는 KD 전체(teacher forward + 증류 손실)와 처음부터 학습의 비이며 teacher forward만 따로 잰 것은 아님 | Table 1, p. 5 | 일치 | ("teacher forward 몫을 분리한 값은 아님" 주석 권장) |
| 15 | (빠짐) 원문 안의 메모리 절감 표현 차이 | 초록: "roughly half of their training time and GPU memory costs"; 본문: "GPU memory footprint is reduced by roughly 25%" (7.5 → 5.8 GB = 약 23%) | 초록 p. 1, p. 6 | 추가 | 메모리 절감은 초록의 "절반"이 아니라 표 기준 약 23% (7.5 → 5.8 GB). 본문도 "roughly 25%"로 적음. (p. 1, p. 6) |
| 16 | (확인) 자기 증류 비교 | Table 2 캡션: self-distillation 70.63%, 22 GB, 3,207.45 s/epoch | Table 2, p. 5 | 일치 | (메모에 없음, 필요 시 인용) |

---

## 축 분류 메모와 관련된 확인 (08–14번 범위)

- "저장 용량만 보고: 08, 09" — 원문 확인. 08은 저장량(표 1–3, 4.0 TB 언급)만 있고 저장 단계 시간 없음. 09는 label·이미지·BN 통계 저장량(Table 7, 20, 24, 25)만 있고 생성·학습 시간 없음.
- "둘 다 없음: 13 MobileSAM, 14 TCS" — 원문 확인. 13은 임베딩 저장량·시간 모두 없음 ("하루 미만"은 전체 학습). 14는 forward 1회 비용을 "negligible"로만 적음.
- 14 TCS를 "출력을 저장하는" 묶음에 넣는 근거는 eLSH 해시 목표 사전 계산뿐이며, 원문에 디스크 캐시라는 표현은 없음.

## 전체 요약

1. **가장 심각한 오류**: 09 LPLD의 "113 → 2.8 GB, +2.6%p" 출처를 Table 7(21K-P 표)로 적은 것. 정확도 비교 상대가 압축 안 한 SRe2L이라는 점도 빠짐. 1배 조건에서는 CDA(63.3%)가 LPLD(62.6%)보다 높다는 사실도 메모에 없음.
2. **비용 서술 오류**: 10 TinyCLIP의 "마스크 학습은 절감 계산에 넣지 않음"은 원문과 맞지 않음. 마스크 학습은 각 단계 첫 3,000 iteration으로 학습 일정 안에 있음. 또 teacher는 LAION-2B로 학습했고, 7.8배의 비교 기준은 LAION-400M에서 처음부터 학습한 모델이라는 점이 빠짐.
3. **조건 누락 패턴**: 하드웨어가 실험마다 다른 경우 하나만 적음 (09 Tiny 1장, 12 ImageNet A40 4장, 14 ImageNet 미표기). 학습 일정은 비교용 일부만 적음 (10번 6+16 vs 전체 6+16+16).
4. **"평균 외 평가 없음"과 위치 누락**: 정성 시각화(08 그림 2, 11 Fig. 5)와 검색 지표(10 Table 1)가 빠진 곳이 있음. 위치에서 방법 쪽(Alg. 1, Fig. 2·3)이 대체로 빠짐.
5. 수치 자체(표의 값 옮겨 적기)는 7편 모두 정확함. 틀린 것은 수치의 출처·비교 대상·조건임. 08–14번에는 정정 줄이 없고, audit 보고서(130–136행)의 지적을 원문으로 다시 확인하니 모두 맞았음.
