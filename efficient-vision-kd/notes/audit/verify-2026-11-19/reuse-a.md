# 원문 대조 보고서: `notes/3_Reuse.md` 01–07

- 대조자: Claude (delegate), 2026-11-19
- 대상: 메모 본문 + "팀원 정리에서 추가"(NGC1788) 줄 + **정정** 줄
- PDF: `Related Work/Efficient KD papers/3_Reuse/01–07`. 추출 텍스트는 `verify/tools/reuse-a/NN.txt`. 표가 깨진 곳은 쪽 이미지를 렌더링해서 확인함 (ReLabel p.7, DR p.1, CLIP-Embed-KD p.4).
- FKD 정정 줄을 확인하려고 ECCV 2022판도 받았음: `verify/tools/reuse-a/dl_fkd/eccv.pdf` (ecva.net, 17쪽). arXiv에는 v1만 있음.
- 쪽 번호는 저장소 PDF 기준 (첫 장 = 1).

---

## 3_Reuse-01 FKD (A Fast KD Framework for Visual Recognition)
- 본 판본: arXiv v1 (2 Dec 2021, 11쪽). 보조로 ECCV 2022판 대조.
- 대조 결과: 일치 18건 / 수정 6건 / 추가 4건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 일반 학습 대비 학습 시간 −0.5%, ReLabel +0.5% | Table 3 "↓0.5%", ReLabel "↑0.5% [52]" (ReLabel 값은 ReLabel 논문 인용) | Table 3, p.6 | 일치 | — |
| 2 | MEAL V2 대비 0.3배 | Table 4 Speedup 0.3× (♥FKD 0.5×, MobileNetV3 0.4×). 본문 "speed up 2∼4×" | Table 4, p.6; §4.1 p.7 | 일치 | — |
| 3 | DeiT 증류 대비 ViT-T 0.15배 | "only require 0.15× resources than DeiT distillation protocol" | Table 5, p.7 | 일치 | — |
| 4 | (누락) SReT-LT | Table 5: SReT-LT w/ FKD 78.7% (KD 77.7%), 0.14× | Table 5, p.7 | 추가 | SReT-LT도 원래 KD 77.7% → FKD 78.7%, 학습 자원 0.14배 (Table 5, p.7). |
| 5 | S2-BNN 대비 0.3~0.4배 | Table 8: ReActNet 0.4×, ResNet-50 0.3× | Table 8, p.8 | 일치 | — |
| 6 | MEAL V2 80.67→80.70, ViT-T 74.5→75.2 | 원문 값 같음 | Table 4·5, p.6–7 | 일치 | — |
| 7 | FunMatch 80.5%보다 0.7%p 낮음 (79.8%) | v1 §4.1: "79.8% w/o extra augmentations, which is 0.7% lower than FunMatch (80.5%)". ECCV판 §4.1은 "After employing CutMix … our result (80.9%) outperforms FunMatch by 0.4%" | v1 p.7; ECCV p.11 | 수정 | FunMatch(80.5%) 대비: arXiv v1은 추가 증강 없이 79.8%로 0.7%p 낮다고 보고(§4.1, p.7). ECCV판은 CutMix를 넣은 80.9%로 0.4%p 높다고 보고(§4.1). 판본에 따라 결론이 반대. |
| 8 | label 압축: 전체 79.48 vs MS(K=5) 79.51, MEAL V2 80.65 vs 80.58 | Table 6 값 같음. 조건: 미니배치 안 같은 이미지 crop m=8 | Table 6, p.7 | 일치 | (조건 보충 권장: "m=8 조건") |
| 9 | crop 8개 79.51% → 32개 74.61% | Table 7 같음. m은 "한 미니배치 안에 같은 이미지에서 뽑은 crop 수". 최고는 m=4 79.76% | Table 7, p.7 | 일치 | (보충 권장: "최고 m=4, 79.76%") |
| 10 | [구조] random crop·flip만 사용 | "we did not involve extra data augmentations (beyond the basic random crop and random horizontal flip) such as RandomAug, MixUp, CutMix" | §4, p.6 | 일치 | — |
| 11 | 저장 0.9 TB / hard 5.3 GB / MS top-5 13.3 GB / top-10 22.2 GB (200 crop) | Table 2 같음. 단 200 crop은 "we choose 200 crops as an example to calculate the space consumption"이라 계산 예시. 실험에 실제로 몇 crop을 저장했는지는 v1 본문에 없음 | Table 2, p.6 | 수정 | 저장 공간은 이미지당 200 crop을 **예시로 가정한 계산값**: 전체 약 0.9 TB, hard 5.3 GB, top-5 MS 13.3 GB, top-10 22.2 GB (Table 2, p.6). 실험에서 실제로 저장한 crop 수는 v1에 없음. |
| 12 | label 생성 시간은 "느림"만, 숫자 없음 | Table 1 "Generating label: Slow". v1 전체에서 GPU·hour 검색 0건. ECCV판에도 생성 시간 수치 없음 | Table 1, p.1 | 일치 | — |
| 13 | 05번 Table 9 추정 200 crop 904, 500 crop 2260 GPU시간 | DR Table 9 "904∗", "2260∗", "∗Our estimates". 같은 표 raw 저장 13.6 GB(200), 34.0 GB(500) | 05 Table 9, p.9 | 일치 | (보충 가능: "DR 재현 기준 저장 13.6 GB(200 crop), 34.0 GB(500 crop)") |
| 14 | 하드웨어 보고 안 됨 | v1은 맞음. ECCV판 §4.1: "FunMatch needs 10× more budget for training than FKD (2 days vs. 20 days) with the same number of GPUs (e.g., 8 V100)" | ECCV p.11–12 | 수정 | 하드웨어: arXiv v1은 보고 안 됨. ECCV판은 FunMatch와 비교하며 V100 8장 기준 FKD 2일, FunMatch 20일이라고 적음(§4.1). label 생성 시간은 이 비교에서도 빠짐. |
| 15 | teacher EfficientNet-L2-ns-475 | "Except for experiments on MEAL V2 … For MEAL V2 … SENet154 + ResNet152 v1s (gluon version) ensemble" | §4, p.6 | 수정 | teacher: EfficientNet-L2-ns-475 (MEAL V2 실험만 SENet154 + ResNet152 v1s 앙상블, §4 p.6). |
| 16 | ImageNet-1k 300 epoch 처음부터, MEAL V2는 사전학습 가중치에서 미세조정 | Table 10 Epoch 300. Table 4 캡션 "trained from the pre-trained initialization" | Table 10 p.11; Table 4 p.6 | 일치 | — |
| 17 | 손실은 soft label 항만 | "we simply minimize the soft predictions between teacher and student" | §1, p.1 | 일치 | — |
| 18 | 평균 외: ReaL·V2 (T12), COCO (T9), 자기지도 선형 평가 (T8) | 같음. 그 밖에 Grad-CAM 정성 비교 (Fig. 6·7) | p.8, p.11 | 일치 | — |
| 19 | 위치 목록 | Table 1 p.1, Fig.1 p.2, Fig.4 p.5, T2–4 p.6, T5–7 p.7, T8·9 p.8, T10–12 p.11 | — | 일치 | — |
| 20 | [팀원] FunMatch 대비 3~5배 | v1 서론 "3∼5× faster than oracle KD with similar performance". v1에는 FunMatch 학습 시간을 잰 표가 없음. ECCV판 §4.1은 "10× more budget (2 days vs. 20 days)" | v1 p.2; ECCV p.11–12 | 수정 | FunMatch 대비 3~5배는 v1 서론의 주장이고, 근거가 되는 측정 표는 v1에 없음(p.2). ECCV판은 V100 8장 기준 2일 대 20일(10배)로 바꿔 적음(§4.1). |
| 21 | [팀원] 같은 설정 MEAL V2 대비 2~4배 | §4.1 "FKD can speed up 2∼4× for MEAL V2" | p.7 | 일치 | — |
| 22 | [팀원] 배수는 label 생성 비용 뺀 학습 단계만 | 원문에 명시 문장은 없음. 생성 시간 숫자가 없으므로 구조상 그렇게 읽힘 | — | 일치 | ("원문 명시 아님, 추론"이라고 표시 권장) |
| 23 | [팀원] ReLabel 대비 78.9→79.8 (+0.9), 80.7→81.7 (+1.0), 학습 시간 약간 짧음 | Table 3 같음 | Table 3, p.6 | 일치 | — |
| 24 | [팀원] 생성 단계 "느리다"고만, GPU 시간 없음 | 같음 | Table 1, p.1 | 일치 | — |
| 25 | **정정**: v1 표3 79.8 / ECCV 표3 79.8·80.1 / §4.1 CutMix 80.9 / DR 표9 500 crop 80.1 | ECCV Table 3: "FKD (Ours) w/o warmup&colorj 79.8", "FKD (Ours) 80.1+1.2" (ResNet-101은 81.7 / 81.9). ECCV §4.1 80.9 확인. DR Table 9의 80.1%는 FKD 공개 데이터(이미지당 500 표본)를 **DR 쪽 학습 레시피로 다시 학습**한 값 ("We report results of training with our code on the released datasets of ReLabel and FKD", 300 epoch) | ECCV p.10–11; DR §3.4 p.7–9 | 수정 | (정정 줄 끝에 덧붙임) DR 표 9의 80.1%는 FKD가 공개한 500 crop 데이터를 DR 쪽 코드·레시피로 300 epoch 다시 학습한 값임 (DR §3.4, p.7). |
| 26 | 메모: crop 수와 epoch 수 관계 확인 안 함 | v1 §3.3: "when iterating over the entire dataset once, we actually train the dataset #crop epochs with the same learning rate"(미니배치 multi-crop 설명). 저장 crop 수 대 epoch 수는 v1에 없음 | §3.3, p.5 | 추가 | 미니배치에 이미지당 crop을 #crop개 넣으면 데이터셋 1회 순회가 #crop epoch에 해당한다고 적음 (§3.3, p.5). 저장 crop 수와 학습 epoch 수의 관계는 v1에 없음. |
| 27 | (누락) 비교 분모 | 학습 시간 −0.5%는 KD 없는 일반 학습(Vanilla♦, 같은 프로토콜 재학습) 대비 | Table 3, p.6 | 추가 | −0.5%의 기준은 같은 프로토콜로 다시 학습한 KD 없는 ResNet-50 (Table 3의 Vanilla♦). |
| 28 | (누락) FKD 정확도가 논문마다 다름 | 같은 ResNet-50 300 epoch FKD가 v1 79.8, ECCV 80.1, DR 재현 80.1, FerKD Table 8 80.5, FerKD Table 12 80.1, FerKD Table 7 80.2 | 각 표 | 추가 | FKD ResNet-50 수치는 인용처마다 79.8~80.5%로 다름. 인용할 때 판본과 표를 같이 적음. |

---

## 3_Reuse-02 ReLabel (Re-labeling ImageNet)
- 본 판본: arXiv v2 (22 Jul 2021)
- 대조 결과: 일치 16건 / 수정 3건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 15×15×1000 label map, RoIAlign, teacher forward 이미지당 1회 | §3.3 "L ∈ R15×15×1000". 부록 B: 475×475로 **crop 없이 resize**해 1회 forward | §3.2–3.3 p.4; 부록 B p.13 | 일치 | — |
| 2 | 학습 시간 +0.5% | "leads to only 0.5% additional training time" | §3.3, p.4 | 일치 | — |
| 3 | B7 KD 79.0% 78 GPU일, B5 41 GPU일, ReLabel 78.8~78.9% 13.6 GPU일 | "79.0% for KD with B7 and 78.8% for ReLabel … 41 and 78 GPU days … mere 13.6" | §4.1, p.7; Fig. 6 | 일치 | — |
| 4 | (누락) KD 비용 추가 수치 | "KD with EfficientNet-B7 teacher takes more than four times the original training time"; "KD with EfficientNet-L2 … would take 160 GPU days, beyond our computational capacity" | §3.3 p.4; §4.1 p.6–7 | 추가 | B7 teacher KD는 원래 학습 시간의 4배 넘게 걸림 (§3.3, p.4). 주석자와 같은 L2 teacher로 KD하면 160 GPU일이라 실험하지 않음 (§4.1, p.6–7). |
| 5 | 온라인 KD보다 0.1~0.2%p 낮음 | B7 79.0 대비 78.8~78.9 | p.7 | 일치 | — |
| 6 | 원래 라벨과 섞으면 78.9→78.3 | "0.5Lours + 0.5Lgt … degrades from 78.9% to 78.3%" | p.7 | 일치 | — |
| 7 | [구조] color jitter 미반영 "(01번 FKD가 지적한 불일치)" | FKD v1이 든 불일치는 (i) teacher는 random-crop-resize로 학습됐는데 label map은 전체 이미지로 얻음, (ii) RoIAlign이 teacher 출력과 같지 않음. color jitter는 언급 안 함 | FKD v1 p.2 | 수정 | [구조] label map을 전체 이미지 한 장으로 한 번만 만들고 crop label은 RoIAlign으로 근사함. FKD가 지적한 불일치는 이 두 가지 (01번 FKD p.2). color jitter 등 다른 증강도 label에 반영되지 않음 (구조상 추론). |
| 8 | 생성 약 10 GPU시간 (328 GPU시간의 3.3%), 1 TB → top-5 10 GB (원본의 약 10%) | 같음 | §3.3, p.4 | 일치 | — |
| 9 | 하드웨어 V100 4장 | 각주 3 "300 epochs on four NVIDIA V100 GPUs". KD 비교 학습도 V100 4장 (부록 D.3) | p.4; 부록 D.3 p.14 | 일치 | (보충 가능: "KD 비교도 V100 4장") |
| 10 | 기계 주석자 학습 비용 N (공개 가중치) | "we have adopted the open-source trained weights as the machine annotators" | §3.1, p.3 | 일치 | — |
| 11 | 설정: L2(475) → ResNet-50 등, 300 epoch; KD 비교 B1/B3/B5/B7 | 같음 (SGD, lr 0.1, bs 1024, cosine) | §4.1 p.5; Fig. 6 | 일치 | — |
| 12 | (누락) KD 비교 프로토콜 | 부록 D.3: teacher에 큰 이미지(240–600)를 넣고 student는 224, 정답 라벨 항·온도 없음, 300 epoch | 부록 D.3, p.14 | 추가 | KD 비교 조건: teacher는 큰 해상도 입력, student 224, 정답 라벨 항과 온도 없음, 300 epoch, V100 4장 (부록 D.3). |
| 13 | 평균 외: V2·ReaL·다중 라벨 (T3) | Table 3: V2 80.5, ReaL 85.0, Shankar 86.1 | Table 3, p.6 | 일치 | — |
| 14 | 강건성 FGSM/A/C/BGC +5.6/+2.2/+0.2/+8.7 | Table 7 같음 | Table 7, p.7 | 일치 | — |
| 15 | 세부 분류·COCO 검출·분할 (T8·9) | 같음 | p.8 | 일치 | — |
| 16 | (누락) 그 밖의 평가 | Table A1 ImageNetV2 세 변형 (+1.5/+2.1/+1.7), Table 10 COCO 다중 라벨 분류 (+3.7/+2.4 mAP), Fig. 5 crop IoU 대 확신도 | p.8, p.13 | 추가 | 그 밖에 ImageNetV2 세 변형 (Table A1, p.13), COCO 다중 라벨 분류 (Table 10, p.8). |
| 17 | 위치 목록 | 3.3절 p.4, T3 p.6, Fig.6·T6·T7 p.7, T8·9 p.8 | — | 일치 | (보충 가능: Fig. 4 p.4) |
| 18 | [팀원] B1 annotator 76.1% < 원래 77.5% | "EfficientNet-B1 … severe performance drop (76.1%)", 원래 77.5% | §3.3, p.4 | 일치 | — |
| 19 | [팀원] k=1 78.2, k=5 78.9, k=10 78.7 | Table 6 왼쪽은 **그래프**만 있음. 본문은 "We explore k ∈ {1, 3, 5, 10}", "good trade-off when k = 5"만 적음. 숫자는 k=5의 78.9만 표에 있음 | Table 6, p.7 | 수정 | 저장 class 수 k: k ∈ {1, 3, 5, 10}을 실험했고 k=5(78.9%)가 가장 좋음 (Table 6, p.7). k=1·10 값은 그래프에서 읽은 근사값(약 78.2, 78.7%)이라 인용하지 않음. |
| 20 | [팀원] 본문 숫자와 그림 값이 조금 다름 | Fig. 6에서 ReLabel 점은 79.0 근처로 보여 본문 78.8과 차이 (눈대중) | Fig. 6, p.7 | 일치 | — |
| 21 | 메모: 생성·저장·학습을 "모두 같은 단위로" 보고 | 생성·학습 시간은 GPU시간 (KD 비교는 GPU일), 저장은 GB | §3.3, p.4 | 수정 | 생성 시간과 학습 시간을 같은 GPU시간 단위로, 저장 용량을 GB로 함께 보고한 드문 사례. |
| 22 | Table 6 16비트 78.8, 8비트 78.6 | 같음 | Table 6, p.7 | 일치 | — |

---

## 3_Reuse-03 FerKD
- 본 판본: arXiv v1 (29 Dec 2023)
- 대조 결과: 일치 10건 / 수정 3건 / 추가 2건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 가장 쉬운·어려운 crop은 버리거나 보정, SelfMix | §4.2: "we discard examples in [0, 0.15) and (0.95, 1.0], meanwhile, calibrate examples in [0.15, 0.3]". 보정 = 정답 hard label (1−ε)로 바꿈 (Eq. 4) | §3.2 p.4; §4.2 p.6 | 수정 | teacher 확신도 [0, 0.15)와 (0.95, 1.0]의 crop은 버리고, [0.15, 0.3]의 crop은 soft label을 정답 hard label(1−ε)로 바꿈 (§4.2, p.6). SelfMix는 같은 이미지 안에서만 섞음. |
| 2 | 300→200 epoch에서 정확도 비슷, 그림으로만 | "with a training budget reduced by 2/3 (200 epochs), the achieved accuracy is comparable"; 숫자 없음 | Fig. 6, §3.3 p.5 | 일치 | — |
| 3 | 시간·연산 수치 보고 안 됨 | 부록까지 GPU·hour·GB·storage 검색에서 수치 0건 | 전체 | 일치 | — |
| 4 | 보정 구간 [0,0.3] 80.42 → [0,0.6] 79.66 | Table 4 같음. 조건: 단일 teacher, ResNet-50, SelfMix 없음 | Table 4, p.6 | 일치 | (조건 보충 권장) |
| 5 | ViT-G14 (89.59) → 79.03, RegNetY-128GF (88.24) → 80.34 | Table 3 같음 (ResNet-50 student, 3회 평균) | Table 3, p.6 | 일치 | — |
| 6 | 보정은 오프라인, 숫자 없음 | "our calibration process will not involve additional training cost since it can be done offline in advance" | §3.3, p.5 | 일치 | — |
| 7 | 하드웨어 A100 80 GB 클러스터, 장수 없음 | "A100-GPU High-Flyer cluster with 80GB on each" | §4, p.6 | 일치 | — |
| 8 | 설정: L2-475 등 → ResNet-50, ViT-S/16, 300 epoch, 온도 1.0 | 같음. 이미지당 crop 4개, AdamW, bs 1024 (Table 13) | §4 p.6; Table 13 p.11 | 일치 | — |
| 9 | 3회 평균 | "We run three trials and report the means"는 Table 2·3 캡션에만 있음 | Table 2·3, p.6 | 수정 | Table 2·3은 3회 평균. 다른 표는 반복 횟수 표기 없음. |
| 10 | 평균 외: ReaL·V2 (T12), COCO (T10) | 같음 | p.8 | 일치 | — |
| 11 | 세부 분류 전이 (Table 11) | Table 11은 iNat 2019(224·336)와 Places365, EVA ViT-G14 대비 | Table 11, p.8 | 수정 | 분류 전이: iNaturalist 2019, Places365 (ViT-G14, EVA 대비, Table 11). |
| 12 | 위치 목록 | Fig.6 p.5, T2–5 p.6, T6–9 p.7, T10–12 p.8 | — | 일치 | — |
| 13 | (누락) 주요 결과 | Table 8: ResNet-50 300 epoch FerKD 81.2% (FKD 80.5%, FunMatch 80.5%), 600 epoch 81.4%. Table 7: 앙상블 없이 FerKD 80.7 vs 무작위 FKD 80.2 | Table 7·8, p.7 | 추가 | ResNet-50 300 epoch 81.2% (같은 표의 FKD 80.5%), 600 epoch 81.4% (Table 8, p.7). 앙상블 없이 비교하면 FKD 80.2 → FerKD 80.7% (Table 7). |
| 14 | (누락) 앙상블 teacher 비용 | 최종 결과는 teacher 4개 앙상블 (Table 6 최고 80.74). teacher 수만큼 soft label 생성이 늘지만 수치 없음 | Table 5·6, p.6–7; §4.5 p.8 | 추가 | 최종 설정은 teacher 4개 앙상블 (Effi L2 475, RegY 128GF, ViT L16, ViT H14; §4.5). 생성 비용이 teacher 수만큼 늘지만 숫자 보고 안 됨. |
| 15 | 메모: Reuse 위에 Skip, 근거가 epoch·수렴 곡선뿐 | 해석. 원문 근거(Fig. 6만)와 모순 없음 | — | 일치 | — |

---

## 3_Reuse-04 TinyViT
- 본 판본: arXiv v1 (21 Jul 2022)
- 대조 결과: 일치 11건 / 수정 3건 / 추가 3건 / 확인 불가 2건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 상위 K logit + 증강 정보 부호화 저장 | 증강은 난수 seed 4바이트로 저장 ("only needs 4 bytes to store a random seed"), 매 iteration의 (A, ŷ) 쌍 저장 | §2 p.4; §3.1 p.5–6 | 일치 | — |
| 2 | 총비용 −29.8%: 92.4 + 140.0 = 232.4 vs 330.9 GPU일 | 부록 B 같음. 330.9는 "due to limited batch size" | 부록 B, p.20 | 일치 | — |
| 3 | (누락) 29.8%의 분모 | 330.9 GPU일 = Table 3 Florence 행 7,942 GPU시간, 140.0 GPU일 = 증류 없는 행 3,360 GPU시간. 온라인 쪽은 teacher 메모리 때문에 배치가 줄어든 상태. 부록 B는 GPU 종류를 따로 적지 않음 (Table 3은 V100) | Table 3 p.12; 부록 B p.20 | 추가 | 330.9 GPU일은 Table 3의 Florence 온라인 증류 7,942 GPU시간과 같은 값이고, teacher 메모리 탓에 배치가 줄어든 상태의 비용 ("due to limited batch size", 부록 B). |
| 4 | 학습 중 시간·메모리는 증류 없는 학습과 같음; 온라인은 1.7~2.4배, 2.7~10.7 GB | Table 3: 1.9× / 1.7× / 2.1× / 2.4×, 3.9 / 6.8 / 2.7 / 10.7 GB. "같음"은 본문 주장 ("does not require additional GPU memory and has the same training time as #0"), 따로 잰 행은 없음 | Table 3·p.12 | 수정 | 저장 logit으로 학습하면 추가 GPU 메모리가 없고 학습 시간이 증류 없는 학습과 같다고 **주장** (p.12, 측정 행 없음). teacher를 매번 돌리면 1.7~2.4배 시간, 2.7~10.7 GB 메모리 추가 (Table 3, V100). |
| 5 | K가 작으면 낮고 키우면 포화, 저장은 K에 비례 (Fig. 5) | 같음. teacher Swin-L | Fig. 5, p.11 | 일치 | (조건 보충: "Swin-L teacher") |
| 6 | [구조] 증강 파이프라인 고정 | 구조상 맞음 | — | 일치 | — |
| 7 | 저장 16 GB (IN-1k 300 ep, K=10), 481 GB (IN-21k 90 ep, K=100) | "K=10 (1.0% logits) on IN-1k for 300 epochs and K=100 (0.46% logits) on IN-21k for 90 epochs using 16 GB/481 GB" | p.11 | 일치 | — |
| 8 | 하드웨어 V100 | Table 3 캡션 "on NVIDIA V100 GPUs". Table 4 처리량도 V100 | p.12–13 | 일치 | — |
| 9 | 설정: teacher 4종 → TinyViT-5/11/21M, IN-21k 90 ep 후 IN-1k 30 ep 미세조정 | 같음 | §5.1 p.9–10 | 일치 | — |
| 10 | (누락) teacher 준비 비용 | "CLIP-ViT-L/14 and Florence are finetuned on IN-21k for 30 epochs to serve as teachers" — 수치 없음 | §5.1, p.10 | 추가 | CLIP-ViT-L/14와 Florence는 IN-21k에서 30 epoch 미세조정해 teacher로 씀 (§5.1, p.10). 이 비용은 29.8% 계산에 없음. |
| 11 | 평균 외: Real·V2 (T3), 선형·few-shot (T5), COCO (T6) | 같음 (Table 5·6 모두 p.14) | p.12, p.14 | 일치 | — |
| 12 | (누락) 처리량 | Table 4·Fig. 6에 처리량 (images/s, V100)과 MACs | Table 4, p.13 | 추가 | 효율 지표로 V100 처리량(images/s)과 MACs 보고 (Table 4, p.13). |
| 13 | 위치 목록 | Fig.5 p.11, T3 p.12, T4 p.13, T5·6 p.14, 부록 B p.20 | — | 일치 | — |
| 14 | [팀원] 실제 몇 개 student에 썼는지 나오지 않음 | 저장 logit으로 TinyViT-5/11/21M (Table 4, CLIP-ViT-L/14 teacher)과 DeiT-Ti/S·Swin-T (Table 2)를 학습함. 그러나 student 수로 나눈 비용은 없음. "the saved logits can be reused for arbitrary student models" | Table 2 p.10; Table 4 p.13; 부록 B p.20 | 수정 | 저장 logit으로 여러 student(TinyViT-5/11/21M, DeiT-Ti/S, Swin-T)를 학습했지만, student 수로 나눈 비용이나 재사용 횟수별 비용은 보고하지 않음. |
| 15 | [팀원] 최고 결과와 비용 계산에 쓴 Florence | 최고 결과 (Table 4·Fig. 6의 ⚗)는 "using CLIP-ViT-L/14 as the teacher". Florence는 Table 3 #4와 부록 B 비용 계산에만 씀 | p.12; 부록 B | 수정 | 비용 계산(부록 B)과 teacher 비교(Table 3 #4)는 Florence, 대표 결과(Table 4·Fig. 6)는 CLIP-ViT-L/14 teacher. |
| 16 | [팀원] Florence는 공개되지 않음 | 원문에 공개 여부 언급 없음 | — | 확인 불가 | 원문에 없음. 외부 근거가 필요함. |
| 17 | [팀원] 저장할 때와 학습할 때 로더·증강 구현·라이브러리 버전이 같아야 함 | 원문 언급 없음. seed 재현 방식에서 나오는 추론 | §2 p.4 | 확인 불가 | 원문 명시 없음. "[구조] 추론"으로 표시. |
| 18 | **정정**: 29.8%는 캐시 생성 + 90 epoch 사전학습 비교, teacher 준비와 미세조정 제외; CutMix는 Mixup과 함께 껐음 | "We disable Mixup and Cutmix for pretraining distillation on TinyViT" (§5.1). 부록 B 범위도 같음 | p.10, p.20 | 일치 | — |
| 19 | 메모: 한 번만 학습해도 29.8% 절감, 저장 용량이 epoch 수에 비례 | "pre-store the top-100 logits … for all 90 epochs", "teacher logits per epoch are different and independent" | p.10, p.20 | 일치 | — |

---

## 3_Reuse-05 Dataset Reinforcement (Reinforce Data, Multiply Impact)
- 본 판본: arXiv v3 (22 Sep 2023)
- 대조 결과: 일치 16건 / 수정 5건 / 추가 2건 / 확인 불가 1건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 1000 epoch: MNv3-L 75.1→77.9, R50 79.6→81.7, Swin-T 80.9→83.8 | Table 4·10 같음 | Table 4 p.6; Table 10 p.9 | 일치 | — |
| 2 | 같은 반복 수에서 실제 시간이 같음 | 평균 1.12× / 1.01× / 0.99× ("The extra time for MobileNetV3 is because there is no data augmentations in our baseline") | §3 p.6; Table 19 p.25 | 수정 | 같은 반복 수에서 학습 시간은 표준 학습의 1.12배(MobileNetV3-L), 1.01배(ResNet-50), 0.99배(Swin-T) (§3, p.6). |
| 3 | 증강 난이도·모델 크기 균형 (Fig. 4, 부록 C) | 같음 | Fig. 4 p.5 | 일치 | — |
| 4 | [구조] teacher 한계가 넘어감 (p.9) | "Limitations of the teacher can potentially transfer through dataset reinforcement" | p.9 | 일치 | — |
| 5 | 생성 2205 GPU시간 (A100 64장, 2080분) | §3 "2205 GPUh … 64xA100"; 부록 F "2080 mins … 64xA100" | p.6, p.25 | 일치 | — |
| 6 | 생성은 "ResNet-50을 A100 8장에서 300 epoch 학습하는 것과 비슷한 규모" (옮겨 간 비용 줄) | 부록 F의 "similar"는 경과 시간 기준. GPU시간으로는 §3 "206 GPUh" 대비 약 10.7배 | p.6, p.25 | 수정 | 생성: ImageNet+ 2205 GPU시간 (A100 64장, 경과 2080분). ResNet-50 300 epoch 학습(A100 8장, 206 GPU시간)의 약 10.7배. 경과 시간으로만 비슷함 (§3 p.6, 부록 F p.25). |
| 7 | 저장: 400 표본, 원본 61.5 GB, 압축 37.5 GB | Table 9 Raw 61.5, GZIP 37.5. Table 2는 추가 저장 61 GB (38+8+15), 부록 F는 Joblib 압축 55 GB | Table 2 p.5; Table 9 p.9; p.25 | 일치 | — |
| 8 | 축소판 100 표본·top-5 10.6 GB, 551 GPU시간 | Table 9 Raw 10.6 / GZIP 5.6 GB, 551 GPUh. ResNet-50 80.6%, IN-OOD 48.9% | Table 9, p.9 | 일치 | — |
| 9 | CIFAR-100, Flowers-102, Food-101 생성 90·40·120분 | 같음. 저장은 약 4.8 / 1.0 / 7.3 GB | 부록 F, p.25 | 일치 | (보충 가능: 저장 4.8 / 1.0 / 7.3 GB) |
| 10 | 하드웨어 A100 64장 (생성), A100 8장 (학습) | 8×A100은 비교 기준 문장 ("training ResNet-50 for 300 epochs on 8xA100 GPUs takes 206 GPUh")에만 나옴. ImageNet+ 모델 학습의 GPU 수는 부록 G까지 검색해도 없음 | p.6, p.25–26 | 수정 | 하드웨어 = A100 64장 (생성). 학습 하드웨어는 보고 안 됨 (A100 8장은 비교 기준으로 든 ResNet-50 학습). |
| 11 | teacher IG-ResNeXt 4개 앙상블, 150~1000 epoch | "4xIG-ResNext"; 150·300·1000 epoch | p.6–9 | 일치 | — |
| 12 | 평균 외: 분포 이동 6종 (T7), ECE (Fig. 5), 검출·분할 (T6), 세부 분류 (T8) | 같음. T6은 COCO Mask-RCNN과 ADE20K DeepLabv3 | p.7–8 | 일치 | — |
| 13 | FKD 45.0% vs ImageNet+ 49.1% IN-OOD (Table 9) | 같음 (ResNet-50, 300 epoch, DR 레시피로 재학습) | Table 9, p.9 | 일치 | — |
| 14 | 위치 목록 | Fig.1·T1 p.1, T2 p.5, T7·8·Fig.5 p.8, T9·10 p.9, 부록 F p.25 | — | 일치 | — |
| 15 | (누락) 온라인 KD 대비 시간 | Fig. 1 (ResNet-50): KD 300 epoch 정확도에 ImageNet+가 2.5배 짧은 경과 시간으로 도달, ImageNet 1000 epoch 정확도에는 6.9배 짧게 도달 (그림 주석) | Fig. 1, p.1 | 추가 | 온라인 KD 대비: KD 300 epoch 정확도에 ImageNet+가 2.5배 짧은 경과 시간으로 도달 (ResNet-50, Fig. 1, p.1). 생성 비용 2205 GPU시간은 이 비교에 들어가지 않음. |
| 16 | [팀원] 학습 시간 1.12 / 1.01 / 0.99배 | §3 평균값 같음. Table 19는 epoch별 (ResNet-50 1000 epoch 0.97×) | p.6, p.25 | 일치 | — |
| 17 | [팀원] ResNet-50 IN val +1.7, V2 +3.5, IN-R +10.0%p, ECE 9.9% 감소 | 초록 문장 같음. 다만 Table 7(1000 epoch)의 V2 세 변형 차이는 +3.0 / +3.9 / +2.5로 +3.5가 어느 조건인지 원문에 없음. IN-R은 38.1→48.1 (+10.0) | 초록 p.1; Table 7 p.8 | 일치 | (보충 권장: "초록 수치, V2 +3.5의 조건은 원문에 없음") |
| 18 | [팀원] 이득의 분모가 온라인 KD가 아니라 표준 학습 | 정확도·강건성 이득(T4·T7)의 분모는 표준 학습이 맞음. 그러나 Fig. 1은 온라인 KD 대비 2.5배 경과 시간 단축을, Table 5는 FunMatch·MEALV2·DeiT와의 정확도 비교를 보고 | Fig. 1 p.1; Table 5 p.7 | 수정 | 정확도·강건성 이득의 분모는 teacher 없는 표준 학습. 온라인 KD와는 Fig. 1(경과 시간 2.5배 단축)과 Table 5(정확도)로만 비교하고, 그 비교에 생성 비용은 들어가지 않음. |
| 19 | [팀원] 10.7배 대 "비슷한 규모"는 단위 차이 | 2205 / 206 = 10.70. 206 GPU시간 / 8 = 약 25.8시간(약 1545분) 대 2080분 | p.6, p.25 | 일치 | — |
| 20 | **정정** 줄 전체 | 위 19와 같음. 저장 61.5 / 37.5 GB (표 9) 확인 | p.6, p.9, p.25 | 일치 | — |
| 21 | 메모: "생성 비용이 student 1회 학습과 비슷한 규모라고 스스로 밝힘 → 한 번만 쓰면 이득 없음" | 정정 줄과 모순됨. GPU시간으로는 ResNet-50 300 epoch 1회의 약 10.7배 | p.6 | 수정 | 생성 비용은 GPU시간으로 ResNet-50 300 epoch 학습 1회의 약 10.7배 (2205 대 206 GPU시간, §3 p.6). 경과 시간으로만 비슷함. 한 번 쓰면 손해이고, 여러 번 재사용해야 본전 → "재사용 횟수 N" 논의의 근거. |
| 22 | 메모: 목표 epoch 수의 약 1/3만큼 표본 | "approximately ×3 fewer samples than the maximum intended training epochs" | 부록 F p.25; p.5 | 일치 | — |
| 23 | 메모: 분포 이동과 보정을 함께 본 유일한 사례 | 01–07 안에서는 맞음 (ReLabel은 강건성만, MobileCLIP은 분포 이동만, 둘 다 ECE 없음). 08–14는 이 보고서 범위 밖 | — | 확인 불가 | 08–14번 대조 결과로 확인 필요. |
| 24 | (누락) 혼합 증강 저장 시 오버헤드 | "For mixing augmentations, our current implementation has approximately 30% time overhead"; ImageNet+ (RRC+RA/RE)는 0% | 부록 F p.25 | 추가 | 혼합 증강(MixUp/CutMix)을 저장하면 짝 이미지 로딩 때문에 학습 시간 약 30% 추가. 채택한 RRC+RA/RE는 추가 없음 (부록 F, p.25). |

---

## 3_Reuse-06 MobileCLIP
- 본 판본: arXiv v2 (1 Apr 2024)
- 대조 결과: 일치 12건 / 수정 2건 / 추가 4건 / 확인 불가 1건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 합성 캡션 + 증강 매개변수 + teacher 앙상블 임베딩 저장 (DataCompDR) | 같음. 캡션 5개 (CoCa), 임베딩 BFloat16 | §3.1 p.3; p.5 | 일치 | — |
| 2 | (누락) 저장 증강 수의 원문 내부 불일치 | p.5 "strong random image augmentations (10 for DataCompDR-1B and 30 for DataCompDR-12M)"와 Table 4c (12M 30, 1B 10)는 일치. p.6은 "we reinforce DataCompDR-12M and DataCompDR-1B with 10 and 30 augmentations, respectively"로 반대 | p.5, p.6 | 추가 | 저장 증강 수: DataCompDR-12M 30개, DataCompDR-1B 10개 (p.5, Table 4c). p.6 본문은 반대로 적혀 있어 원문 안에서 어긋남. |
| 3 | 일반 CLIP 학습과 epoch당 시간이 같음 (Table 4d) | Table 4d: DataComp-12M 1.3 h, DataCompDR-12M 1.3 h | Table 4d, p.6 | 일치 | — |
| 4 | (누락) 온라인 증류 대비 시간 | Table 4d: 강화 없이 캡션 생성기와 teacher를 학습 중에 돌리면 21.1 h/epoch ("16× slower"), 합성 캡션만 저장하면 4.1 h ("3× slower"). 8×A100-80GB, 1 epoch = 12.8M 표본 | Table 4d, p.6 | 추가 | 온라인 대비: 캡션 생성기와 teacher를 학습 중에 돌리면 epoch당 21.1시간으로 16배 느림. 합성 캡션만 저장하고 teacher는 돌리면 4.1시간(3배). 전부 저장하면 1.3시간 (Table 4d, A100 8장, p.6). |
| 5 | 반복 수 10배, 데이터 100배, Flickr30k 최대 1000배 | Fig. 6: IN-val 10× 반복, 100× 데이터; Flickr30k 18× 반복, 1000× 데이터. 비교 대상은 강화하지 않은 DataComp 학습 | Fig. 6, p.7 | 일치 | (보충 가능: "Flickr30k 반복 18배") |
| 6 | λ=1.0 61.7/72.0, λ=0.7 60.7/74.2 (Table 3b) | 같음 (Table 2에도 같은 값). ViT-B/16, 30k 반복 | Table 2 p.5; Table 3b p.6 | 일치 | — |
| 7 | [구조] 학습 중 강도가 바뀌는 증강은 쓸 수 없음 | 부록 C: RangeAugment는 "cannot be stored as part of the dataset reinforcement process. Hence, we only apply it to images fed to the student model". teacher·student 양쪽에 같은 증강을 쓰면 "56.6% vs 55.9% on ImageNet-val" | 부록 C, p.14 | 수정 | [실측·원논문] 학습 중 강도가 바뀌는 증강(RangeAugment)은 저장할 수 없어 student 입력에만 적용함. teacher에도 같은 증강을 줄 수 있으면 ImageNet-val 56.6%, 저장 방식은 55.9% (부록 C, p.14). |
| 8 | 저장 12M 0.9→1.9 TB, 1B 90→140 TB | Table 4c 같음 | Table 4c, p.6 | 일치 | — |
| 9 | 생성 시간 숫자 없음 | 부록까지 검색해도 생성 GPU시간 없음 ("one-time cost that is amortized"만) | p.3, p.5 | 일치 | — |
| 10 | 하드웨어 A100 80 GB 8장 (소규모) | 같음. 대규모는 "global batch size of 65,536 with 256×A100 GPUs for 200k iterations". 장기 학습은 64 노드 × A100 8장. 지연은 iPhone12 Pro Max | §5 p.5; 부록 I p.15 | 일치 | (보충 권장: "대규모 A100 256장") |
| 11 | 설정: ViT-L/14 2종 앙상블 → S0/S1/S2/B, 12M 30~45k 반복 (20~30 epoch), 1B | 같음 (datacomp_xl_s13b_b90k, openai). 1B는 200k 반복 | p.5–6 | 일치 | — |
| 12 | 평균 외: IN 분포 이동 (V2, A, O, R, ObjectNet), 검색, 38개 평균, ARO | 같음 | §5 p.5; Table 7·8 p.8 | 일치 | — |
| 13 | 위치 목록 | T2 p.5, T3·4 p.6, Fig.6 p.7, T7·8 p.8, T16·17 p.17 | — | 일치 | — |
| 14 | [팀원] 재사용만의 몫은 약 3.2배 (계산 근거 확인 필요) | 원문에 3.2배라는 값은 없음. Table 4d의 4.1 h → 1.3 h (=3.15배, 본문 "3× slower")로 재현됨. 이는 epoch당 학습 시간 단축이고, Fig. 6a의 10배(같은 오차에 필요한 반복 수)와는 다른 양 | Table 4d p.6; Fig. 6 p.7 | 수정 | teacher 임베딩 저장만의 효과는 epoch당 시간 4.1 → 1.3시간, 약 3.2배 (합성 캡션은 양쪽 다 저장, Table 4d). "10배"(Fig. 6a)는 강화 안 한 학습 대비 반복 효율이라 재사용 효과로 인용하지 않음. |
| 15 | [팀원] 합성 캡션의 오류·편향이 고정되어 재사용됨 | 원문 언급 없음. 구조상 추론 | — | 확인 불가 | 원문 명시 없음. "[구조] 추론"으로 표시. |
| 16 | [팀원] 표본당 증강 10~30개 고정, 여러 epoch에 반복 | Table 4c. "each augmentation can be reused multiple times before the added knowledge is fully learned" | p.6 | 일치 | — |
| 17 | 메모: 저장이 원본의 약 2배 (12M), 증강 5개 근처 포화 | 0.9→1.9 TB (2.1배). 1B는 90→140 TB (1.56배). Table 4a: 증강 5개, 4b: 캡션 2개 근처 포화 | Table 4, p.6 | 일치 | (보충 가능: "1B는 약 1.6배") |
| 18 | (누락) 최소 저장 권장 | "For minimal storage overhead, we recommend 5 augmentations/synthetic captions for 30 epochs on DataCompDR-12M and 2 for 10 epochs on DataCompDR-1B" | p.7 | 추가 | 저장을 줄이려면 12M은 증강·캡션 5개로 30 epoch, 1B는 2개로 10 epoch을 권장 (p.7). |
| 19 | (누락) BFloat16 손실 | Table 17: BFloat16 임베딩으로 학습해도 표준편차 안 | 부록 E, p.15·17 | 추가 | 임베딩을 BFloat16으로 저장해도 정확도가 표준편차 안에 있음 (Table 17, 부록 E). |

---

## 3_Reuse-07 CLIP-Embed-KD
- 본 판본: arXiv v1 (9 Apr 2024, 5쪽)
- 대조 결과: 일치 10건 / 수정 1건 / 추가 2건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 클래스당 100 표본 평균 임베딩, CLIP식 대조 손실 | "we compute the averaged embeddings over 100 samples per class". 손실은 0.5 CE + 0.5 Lclip (α1 = α2 = 0.5) | §2.3 p.3; §3 p.3 | 일치 | (보충 가능: "정답 CE 항과 0.5:0.5") |
| 2 | 초록: 메모리 9배, 학습 시간 8배 감소 | "outperform full scale knowledge distillation using 9× less memory and 8× less training time" | 초록, p.1 | 일치 | — |
| 3 | 표에서는 메모리만 숫자로 확인 (17배, 59배) | Table 2의 17×·59×는 맞음. 시간도 Fig. 4 막대에 "Time per epoch (hr)"와 "8×↓"로 표시 (메모리는 "Memory Allocated (GB)") | Table 2·Fig. 4, p.4 | 수정 | 표에는 메모리 배수만 있음 (Base-32 17배, Large-32 59배, Table 2). 시간은 Fig. 4 막대(epoch당 시간, 시간 단위)에 8배 감소로 표시. |
| 4 | 같은 student에서 약 2%p 낮음: 51.68→49.32, 51.95→50.33 | Table 2 같음. "roughly about ≈2% lesser accuracy". 네 행의 차이는 2.36 / 2.25 / 3.27 / 1.62%p | Table 2, p.4 | 일치 | (보충 권장: "범위 1.6~3.3%p, Base-32 52.61→49.34") |
| 5 | 사전 계산 비용 숫자 보고 안 됨 | 같음 | — | 일치 | — |
| 6 | (누락) 사전 계산 규모 | 설정에서 나오는 값: CIFAR-100 100 클래스 × 100 표본 = teacher forward 10,000장 (224 입력) | §3 p.3 | 추가 | 사전 계산은 클래스당 100장 × 100 클래스 = teacher forward 1만 장 규모 (설정에서 계산, 원문에 시간 수치 없음). |
| 7 | 단위 = 메모리 배수·시간 배수, 하드웨어 보고 안 됨 | 배수 외에 Fig. 4는 GB와 시간/epoch 축. GPU 기종 언급 0건 | Fig. 4, p.4 | 일치 | — |
| 8 | 설정: ViT teacher (Base·Large, patch 16·32) → 작은 ViT, CIFAR-100, 32·64, 200 epoch | 같음. student patch 4, batch 64 | §3 p.3; Table 1 | 일치 | — |
| 9 | (누락) teacher 체크포인트 | "ImageNet pre-trained checkpoints available on HuggingFace, with variants of google/vit-base-patch16-224-in21k". teacher 입력은 항상 224×224. CIFAR-100에 미세조정했는지는 원문에 없음 | §3, p.3 | 추가 | teacher는 HuggingFace의 ImageNet-21k 사전학습 ViT 체크포인트, 입력 224. CIFAR-100 미세조정 여부는 원문에 없음 (§3, p.3). |
| 10 | 평균 외 평가 없음 | CIFAR-100 top-1, 학습 곡선 (Fig. 2·3), α 절제 (Fig. 5)뿐 | p.3–4 | 일치 | — |
| 11 | 위치 목록 | 초록 p.1, Fig.2·3 p.3, T2·T3·Fig.4 p.4 | — | 일치 | — |
| 12 | **정정**: Fig. 4의 9배·8배는 base student 온라인 KD 대 large student 임베딩 KD | Fig. 4 막대: "CLIP-Teacher-KD: [Base][img=32]" 대 "CLIP-Embed-KD: [Large][img=32]"에 9×↓, 8×↓, 정확도 1.02×↑. teacher는 Large-32 | Fig. 4, p.4 | 일치 | — |
| 13 | 메모: 5쪽 예비 연구, 정확도 손실 약 2%p | "Our goal with this preliminary work is not to present state-of-the-art KD results" | p.2 | 일치 | — |

---

## 전체 요약

1. **가장 심각한 오류는 05 DR 메모 본문.** 정정 줄은 "GPU시간 기준 약 10.7배"로 고쳤지만, 「옮겨 간 비용」과 「메모」 줄은 여전히 "ResNet-50 1회 학습과 비슷한 규모 → 한 번만 쓰면 이득 없음"으로 남아 정정과 모순됨. 학습 하드웨어 "A100 8장"도 원문에 없는 값(비교 기준 문장을 잘못 읽음).
2. **팀원 줄에서 사실 오류 2건.** TinyViT의 대표 결과 teacher는 Florence가 아니라 CLIP-ViT-L/14임 (p.12). DR이 "온라인 KD와 비교하지 않음"이라는 줄은 Fig. 1의 경과 시간 2.5배 비교를 놓침. MobileCLIP "3.2배"는 원문에 없는 값이지만 Table 4d의 4.1→1.3시간으로 재현되며, 10배와는 다른 양임.
3. **빠진 비용 수치가 있음.** MobileCLIP의 온라인 증류 epoch당 21.1시간(16배), ReLabel의 L2 teacher KD 160 GPU일과 B7 KD 4배 이상, FKD ECCV판의 FunMatch 20일 대 FKD 2일(V100 8장). 셋 다 "teacher를 매번 돌리는 비용"을 직접 보여 주는 값임.
4. **판본·조건을 빠뜨린 패턴.** FKD는 arXiv v1과 ECCV판에서 FunMatch 비교 결론이 반대 (79.8 < 80.5 대 80.9 > 80.5). FKD ResNet-50 값은 인용처에 따라 79.8~80.5%로 다름. FKD Table 2의 200 crop은 계산 예시이고, DR Table 9의 FKD 80.1%는 DR 레시피로 다시 학습한 값.
5. **그림 값과 추론이 원문 수치처럼 적힌 패턴.** ReLabel의 k=1·10 정확도(그래프 눈대중), ReLabel이 FKD의 지적이라며 쓴 "color jitter" 불일치(FKD 원문에 없음), TinyViT·MobileCLIP 팀원 줄의 구조적 추론(라이브러리 버전, 캡션 편향 고정)은 원문 근거 없이 서술됨.
