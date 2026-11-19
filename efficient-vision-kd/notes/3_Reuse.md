# 3_Reuse 메모

> **팀원 정리 반영**: 01, 02, 04, 05, 06, 09번에 [NGC1788/Reuse](https://github.com/NGC1788/Reuse)의 내용을 덧붙였음 (FerKD와 07, 08, 10–14번은 그 정리에 없음). 05번의 수치 충돌은 단위 차이로 정리했음 (정정 줄).
>
> **원문 대조 완료 (2026-11-19).** 논문마다 **원문 대조** 블록이 있다. Opus 5.5 서브에이전트 8개가 저장소 PDF(일부는 arXiv·학회판을 받아)와 메모의 수치·조건·판정을 한 줄씩 대조한 결과다. 블록이 위 서술과 다르면 블록이 맞다. 대조 보고서 원본: [`audit/verify-2026-11-19/`](audit/verify-2026-11-19). 사람이 다시 확인하지는 않았다.

Reuse: 한 번 만들고 재사용  
논문 목록과 PDF: [`../Related Work/Efficient KD papers/3_Reuse/`](<../Related Work/Efficient KD papers/3_Reuse>)  
작성 규칙과 항목 설명: [README](README.md)

담당: Claude 초안 (2026-11-18). 저장소 PDF 본문을 직접 읽고 채움. 사람이 다시 대조하지 않음.

## 읽기 전에

- 수치는 저장소 PDF의 표·본문에서 옮겼고, 「위치」의 쪽 번호는 그 PDF의 쪽(첫 장 = 1)임.
- 「잃는 것」 표시는 `1_Shrink.md`와 같음. **[실측·원논문]** = 논문이 보고한 관찰, **[구조]** = 방법 정의에서 나오는 제약.
- "보고 안 됨" = 논문에 해당 값이 없음. "확인 안 함" = 논문에 있을 수 있으나 이번에 읽지 않은 부분.
- 이 축은 재사용 대상이 둘로 나뉨. 01–09번은 teacher **출력**(soft label, logit, 특징, 임베딩)을 저장해 다시 씀. 10–14번은 teacher **가중치나 구조 일부**를 가져다 씀. 뒤쪽은 teacher forward가 그대로 남는 경우가 많음.

### 01. FKD (A Fast KD Framework for Visual Recognition) ([arXiv: 2112.01528](https://arxiv.org/abs/2112.01528))
- 효율화 방식: 이미지마다 여러 개의 random crop을 teacher에 통과시켜 soft label을 미리 만들고, crop 좌표·flip 여부와 함께 저장함. 학습 때는 저장된 좌표로 같은 crop을 다시 만들어 저장된 soft label로 학습하므로 teacher forward가 없음. 한 미니배치에 같은 이미지의 crop 여러 개를 넣어 데이터 로딩도 줄임.
- 얻는 것:
  - 학습 시간이 KD 없는 일반 학습과 같은 수준: ResNet-50, ImageNet, 일반 학습 대비 −0.5% (ReLabel은 +0.5%). (Table 3)
  - 온라인 증류 대비 학습 시간 비: MEAL V2 ResNet-50 0.3배, DeiT 증류 프로토콜 대비 ViT-T 0.15배, 자기지도 증류(S2-BNN) 대비 0.3~0.4배. (Table 4·5·8)
- 잃는 것:
  - [실측·원논문] 정확도는 온라인 증류와 같거나 높음: MEAL V2 80.67 → 80.70%, ViT-T 74.5 (DeiT 증류) → 75.2%. (Table 4·5)
  - [실측·원논문] 강한 증강을 쓴 온라인 증류(FunMatch 80.5%)보다는 0.7%p 낮음 (79.8%). (4.1절)
  - [실측·원논문] label 압축에 따른 손실은 작음: 처음부터 학습 시 전체 저장 79.48% vs top-5 marginal smoothing 79.51%, MEAL V2에서는 80.65 vs 80.58%. (Table 6)
  - [실측·원논문] 한 미니배치에 같은 이미지의 crop을 너무 많이 넣으면 떨어짐: 처음부터 학습한 ResNet-50에서 이미지당 8개 79.51% → 32개 74.61%. (Table 7)
  - [구조] 실험은 random crop과 flip만 사용함. RandAugment·Mixup·CutMix 같은 추가 증강은 쓰지 않았다고 명시. (4절)
- 옮겨 간 비용: soft label 생성(teacher forward를 crop 수만큼)과 저장 공간. 저장 공간은 숫자로 보고: 이미지당 200 crop 기준 전체 저장 약 0.9 TB, hard label 5.3 GB, top-5 marginal smoothing 13.3 GB, top-10 22.2 GB (Table 2). label 생성 시간은 "느림"이라고만 적고 숫자 보고 안 됨 (Table 1). 다른 논문의 추정: 200 crop 904 GPU시간, 500 crop 2260 GPU시간 (05번 논문 Table 9).
- 어떻게 쟀나: 단위 = 학습 시간 비(기준 방법 대비 배수), 저장 용량(GB) / teacher forward 셈? 학습 시간에는 없음(구조상 0). 생성 단계는 N / 일회성 비용 셈? 저장 용량 Y, 생성 시간 N / 하드웨어 = 보고 안 됨
- 설정: teacher EfficientNet-L2-ns-475 → ResNet-50/101, ViT-T, SReT-LT. ImageNet-1k, 300 epoch, 처음부터 학습. MEAL V2 설정은 사전학습 가중치에서 미세조정. 손실은 soft label 항만 사용(정답 라벨 항 없음).
- 평균 정확도 외 평가: ImageNet ReaL, ImageNetV2 (Table 12), COCO 검출·분할 전이 (Table 9), 자기지도 선형 평가 (Table 8).
- 위치: Table 1 (p. 1), Fig. 1 (p. 2), Fig. 4 (p. 5), Table 2–4 (p. 6), Table 5–7 (p. 7), Table 8·9 (p. 8), Table 10–12 (p. 11)
- 팀원 정리에서 추가 ([NGC1788/Reuse](https://github.com/NGC1788/Reuse), 사람이 원문을 읽고 작성, 2026-11-18):
  - 비슷한 성능의 온라인 증류(FunMatch) 대비 3~5배, 같은 설정의 MEAL V2 대비 2~4배 빠르다고 주장. 이 배수는 label 생성 비용을 뺀 학습 단계만의 비교.
  - ReLabel 대비 정확도: ResNet-50 78.9 → 79.8%(+0.9%p), ResNet-101 80.7 → 81.7%(+1.0%p). 학습 시간은 ReLabel(+0.5%)보다 약간 짧음. 생성 비용이 크게 다른 두 방법을 학습 시간만으로 비교한 것.
  - 논문은 label 생성 단계를 "느리다"고만 적고 GPU 시간은 밝히지 않음.
- **정정 (검수 보고서 근거)**: 79.8 / 80.1 / 80.9%의 조건이 다름. arXiv v1 표 3은 79.8%. ECCV판 표 3은 warmup·color jitter 제외 79.8%, 포함 80.1%를 둘 다 싣고, §4.1은 CutMix 적용 80.9%를 따로 보고. Dataset Reinforcement 표 9에도 이미지당 500 crop에서 80.1%. [arXiv v1 표 3](https://www.alphaxiv.org/pdf/2112.01528v1?page=6) · [ECCV 표 3](https://www.ecva.net/papers/eccv_2022/papers_ECCV/papers/136840663.pdf#page=10)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 18건 / 수정 6건 / 추가 4건 / 확인 불가 0건. 본 판본: arXiv v1 (2 Dec 2021, 11쪽). 보조로 ECCV 2022판 대조.. 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [추가] SReT-LT도 원래 KD 77.7% → FKD 78.7%, 학습 자원 0.14배 (Table 5, p.7).
  - [수정] FunMatch(80.5%) 대비: arXiv v1은 추가 증강 없이 79.8%로 0.7%p 낮다고 보고(§4.1, p.7). ECCV판은 CutMix를 넣은 80.9%로 0.4%p 높다고 보고(§4.1). 판본에 따라 결론이 반대. ← 메모: FunMatch 80.5%보다 0.7%p 낮음 (79.8%)
  - [수정] 저장 공간은 이미지당 200 crop을 **예시로 가정한 계산값**: 전체 약 0.9 TB, hard 5.3 GB, top-5 MS 13.3 GB, top-10 22.2 GB (Table 2, p.6). 실험에서 실제로 저장한 crop 수는 v1에 없음. ← 메모: 저장 0.9 TB / hard 5.3 GB / MS top-5 13.3 GB / top-10 22.2 GB (200 crop)
  - [수정] 하드웨어: arXiv v1은 보고 안 됨. ECCV판은 FunMatch와 비교하며 V100 8장 기준 FKD 2일, FunMatch 20일이라고 적음(§4.1). label 생성 시간은 이 비교에서도 빠짐. ← 메모: 하드웨어 보고 안 됨
  - [수정] teacher: EfficientNet-L2-ns-475 (MEAL V2 실험만 SENet154 + ResNet152 v1s 앙상블, §4 p.6). ← 메모: teacher EfficientNet-L2-ns-475
  - [수정] FunMatch 대비 3~5배는 v1 서론의 주장이고, 근거가 되는 측정 표는 v1에 없음(p.2). ECCV판은 V100 8장 기준 2일 대 20일(10배)로 바꿔 적음(§4.1). ← 메모: [팀원] FunMatch 대비 3~5배
  - [수정] (정정 줄 끝에 덧붙임) DR 표 9의 80.1%는 FKD가 공개한 500 crop 데이터를 DR 쪽 코드·레시피로 300 epoch 다시 학습한 값임 (DR §3.4, p.7). ← 메모: **정정**: v1 표3 79.8 / ECCV 표3 79.8·80.1 / §4.1 CutMix 80.9 / DR 표9 500 crop 80.1
  - [추가] 미니배치에 이미지당 crop을 #crop개 넣으면 데이터셋 1회 순회가 #crop epoch에 해당한다고 적음 (§3.3, p.5). 저장 crop 수와 학습 epoch 수의 관계는 v1에 없음.
  - [추가] −0.5%의 기준은 같은 프로토콜로 다시 학습한 KD 없는 ResNet-50 (Table 3의 Vanilla♦).
  - [추가] FKD ResNet-50 수치는 인용처마다 79.8~80.5%로 다름. 인용할 때 판본과 표를 같이 적음.
- 메모:
  - 학습 시간 절감은 실측 배수로 보고했지만 일회성 생성 시간은 숫자가 없음. "일회성 비용을 숫자로 보고했나" 집계에서는 저장 용량만 Y.
  - 저장한 crop 수와 epoch 수의 관계(한 crop을 몇 번 재사용하는가)는 본문에서 확인 안 함. 우리 실험의 "뷰 수" 손잡이에 해당.

### 02. ReLabel (Re-labeling ImageNet) ([arXiv: 2101.05022](https://arxiv.org/abs/2101.05022))
- 효율화 방식: 강한 분류기를 "기계 주석자"로 써서 이미지 전체에 대한 공간별 label map(15×15×1000)을 한 번 만들어 저장함. 학습 때는 random crop 영역에 해당하는 label map을 RoIAlign으로 모아 그 crop의 다중 라벨로 씀. teacher forward는 이미지당 1회뿐.
- 얻는 것:
  - 학습 시간: 일반 학습 대비 +0.5%. (3.3절)
  - KD 대비: ResNet-50, ImageNet. EfficientNet-B7 teacher KD는 79.0%에 78 GPU일, B5는 41 GPU일. ReLabel은 78.8~78.9%에 13.6 GPU일. (Fig. 6, 4.1절)
- 잃는 것:
  - [실측·원논문] 큰 teacher의 온라인 KD보다 0.1~0.2%p 낮음 (위 수치).
  - [실측·원논문] 원래 라벨과 섞으면 오히려 떨어짐: 78.9 → 78.3%. (4.1절)
  - [구조] label map을 한 번만 만들므로 color jitter 같은 증강의 효과는 label에 반영되지 않음 (01번 FKD가 지적한 불일치).
- 옮겨 간 비용: label map 생성 약 10 GPU시간 (ResNet-50 전체 학습 328 GPU시간의 3.3%), 저장 공간 전체 약 1 TB → top-5만 저장해 10 GB (원본 ImageNet의 약 10%). 둘 다 숫자로 보고. (3.3절)
- 어떻게 쟀나: 단위 = GPU시간·GPU일, 저장 용량(GB), 학습 시간 증가율 / teacher forward 셈? Y (KD 쪽 학습 시간에 포함, ReLabel 쪽은 일회성 생성으로 따로 보고) / 일회성 비용 셈? Y (생성 시간, 저장 용량). 기계 주석자 자체의 학습 비용은 N (공개 가중치 사용) / 하드웨어 = NVIDIA V100 4장
- 설정: 주석자 EfficientNet-L2 (입력 475) → ResNet-50 등, ImageNet-1k, 300 epoch. KD 비교는 EfficientNet-B1/B3/B5/B7 → ResNet-50.
- 평균 정확도 외 평가: ImageNetV2, ReaL, 다중 라벨 평가 (Table 3). 강건성: FGSM, ImageNet-A, ImageNet-C, 배경 교체 (Table 7; ResNet-50 대비 +5.6 / +2.2 / +0.2 / +8.7%p). 세부 분류 전이, COCO 검출·분할 (Table 8·9).
- 위치: 3.3절 (p. 4), Table 3 (p. 6), Fig. 6·Table 6·7 (p. 7), Table 8·9 (p. 8)
- 팀원 정리에서 추가 ([NGC1788/Reuse](https://github.com/NGC1788/Reuse), 사람이 원문을 읽고 작성, 2026-11-18):
  - annotator가 약하면 손해: EfficientNet-B1을 annotator로 쓰면 76.1%로 원래 label(77.5%)보다 낮음.
  - 저장하는 class 수 k에 따라 달라짐: k=1 78.2%, k=5 78.9%, k=10 78.7%.
  - KD 비교에서 본문 숫자(B7 KD 79.0%, ReLabel 78.8%)와 그림에서 읽히는 값이 조금 다름. 인용할 때는 본문 숫자를 씀.
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 16건 / 수정 3건 / 추가 3건 / 확인 불가 0건. 본 판본: arXiv v2 (22 Jul 2021). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [추가] B7 teacher KD는 원래 학습 시간의 4배 넘게 걸림 (§3.3, p.4). 주석자와 같은 L2 teacher로 KD하면 160 GPU일이라 실험하지 않음 (§4.1, p.6–7).
  - [수정] [구조] label map을 전체 이미지 한 장으로 한 번만 만들고 crop label은 RoIAlign으로 근사함. FKD가 지적한 불일치는 이 두 가지 (01번 FKD p.2). color jitter 등 다른 증강도 label에 반영되지 않음 (구조상 추론). ← 메모: [구조] color jitter 미반영 "(01번 FKD가 지적한 불일치)"
  - [추가] KD 비교 조건: teacher는 큰 해상도 입력, student 224, 정답 라벨 항과 온도 없음, 300 epoch, V100 4장 (부록 D.3).
  - [추가] 그 밖에 ImageNetV2 세 변형 (Table A1, p.13), COCO 다중 라벨 분류 (Table 10, p.8).
  - [수정] 저장 class 수 k: k ∈ {1, 3, 5, 10}을 실험했고 k=5(78.9%)가 가장 좋음 (Table 6, p.7). k=1·10 값은 그래프에서 읽은 근사값(약 78.2, 78.7%)이라 인용하지 않음. ← 메모: [팀원] k=1 78.2, k=5 78.9, k=10 78.7
  - [수정] 생성 시간과 학습 시간을 같은 GPU시간 단위로, 저장 용량을 GB로 함께 보고한 드문 사례. ← 메모: 메모: 생성·저장·학습을 "모두 같은 단위로" 보고
- 메모: 생성 시간, 저장 용량, 학습 시간을 모두 같은 단위로 보고한 드문 사례. 비용식 예시로 쓸 수 있음. 저장 정밀도를 16비트·8비트로 낮춘 실험(Table 6: 78.8, 78.6%)은 "저장 용량 대 정확도" 곡선의 선행 사례.

### 03. FerKD ([arXiv: 2312.17473](https://arxiv.org/abs/2312.17473))
- 효율화 방식: FKD처럼 미리 만든 crop별 soft label을 쓰되, teacher 확신도에 따라 crop을 골라 씀. 가장 쉬운 crop과 가장 어려운 crop은 버리거나 보정하고(영역 보정), 같은 이미지 안에서만 섞는 SelfMix 증강을 씀.
- 얻는 것: 학습 예산을 300 epoch에서 200 epoch으로 줄여도 정확도가 비슷하다고 보고. 수치는 그림으로만 제시 (Fig. 6, 눈대중으로 읽지 않음). 시간·연산 수치는 보고 안 됨.
- 잃는 것:
  - [실측·원논문] 보정 구간을 넓히면 떨어짐: 확신도 [0, 0.3] 보정 80.42% → [0, 0.6] 79.66%. (Table 4)
  - [실측·원논문] 가장 정확한 teacher가 가장 좋은 teacher가 아님: ViT-G14 (89.59%) teacher로는 ResNet-50 student 79.03%, RegNetY-128GF (88.24%)로는 80.34%. (Table 3)
- 옮겨 간 비용: soft label 사전 생성과 저장(FKD와 같음), 보정은 오프라인에서 미리 한다고 적음. 숫자 보고 안 됨.
- 어떻게 쟀나: 단위 = epoch (학습 예산) / teacher forward 셈? N / 일회성 비용 셈? N / 하드웨어 = A100 80 GB 클러스터 (장수 보고 안 됨)
- 설정: teacher EfficientNet-L2-475 등 → ResNet-50, ViT-S/16. ImageNet-1k, 기본 300 epoch, 온도 1.0. 3회 평균.
- 평균 정확도 외 평가: ImageNet ReaL, ImageNetV2 (Table 12), COCO 검출·분할 (Table 10), 세부 분류 전이 (Table 11).
- 위치: Fig. 6 (p. 5), Table 2–5 (p. 6), Table 6–9 (p. 7), Table 10–12 (p. 8)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 10건 / 수정 3건 / 추가 2건 / 확인 불가 0건. 본 판본: arXiv v1 (29 Dec 2023). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] teacher 확신도 [0, 0.15)와 (0.95, 1.0]의 crop은 버리고, [0.15, 0.3]의 crop은 soft label을 정답 hard label(1−ε)로 바꿈 (§4.2, p.6). SelfMix는 같은 이미지 안에서만 섞음. ← 메모: 가장 쉬운·어려운 crop은 버리거나 보정, SelfMix
  - [수정] Table 2·3은 3회 평균. 다른 표는 반복 횟수 표기 없음. ← 메모: 3회 평균
  - [수정] 분류 전이: iNaturalist 2019, Places365 (ViT-G14, EVA 대비, Table 11). ← 메모: 세부 분류 전이 (Table 11)
  - [추가] ResNet-50 300 epoch 81.2% (같은 표의 FKD 80.5%), 600 epoch 81.4% (Table 8, p.7). 앙상블 없이 비교하면 FKD 80.2 → FerKD 80.7% (Table 7).
  - [추가] 최종 설정은 teacher 4개 앙상블 (Effi L2 475, RegY 128GF, ViT L16, ViT H14; §4.5). 생성 비용이 teacher 수만큼 늘지만 숫자 보고 안 됨.
- 메모: Reuse 위에 Skip(쉬운·어려운 crop 제외)을 얹은 형태. "더 빠름"의 근거가 epoch 수와 수렴 곡선뿐이라 비용 주장으로는 약함.

### 04. TinyViT ([arXiv: 2207.10666](https://arxiv.org/abs/2207.10666))
- 효율화 방식: 큰 teacher의 logit을 상위 K개만 남겨 디스크에 미리 저장하고, 그때 쓴 증강(crop, RandAugment, CutMix 등) 정보도 부호화해 함께 저장함. 학습 때는 저장된 증강을 복원해 저장된 logit으로 사전학습 증류를 함.
- 얻는 것 (Florence teacher → TinyViT-21M, ImageNet-21k 90 epoch 사전학습):
  - 일반 증류 대비 총비용 −29.8%. logit 저장 92.4 GPU일 + student 사전학습 140.0 GPU일 = 232.4 vs 온라인 증류 330.9 GPU일. (부록 B)
  - 학습 중 시간과 GPU 메모리는 증류 없는 학습과 같음. teacher를 매번 돌리면 학습 시간이 1.7~2.4배, teacher 메모리 2.7~10.7 GB 추가. (Table 3)
- 잃는 것:
  - [실측·원논문] 저장하는 logit 수 K가 작으면 정확도가 낮고, K를 키우면 포화할 때까지 오름. 저장 용량은 K에 비례. (Fig. 5)
  - [구조] 증강의 난수까지 저장해 재현해야 하므로 증강 파이프라인이 고정됨.
- 옮겨 간 비용: logit 생성 시간과 저장 공간. 둘 다 숫자로 보고. 저장 공간은 ImageNet-1k 300 epoch에 K=10으로 16 GB, ImageNet-21k 90 epoch에 K=100으로 481 GB. (p. 11)
- 어떻게 쟀나: 단위 = GPU일·GPU시간, GPU 메모리(GB), 저장 용량(GB) / teacher forward 셈? Y (온라인 증류 쪽 시간에 포함, 제안 방법 쪽은 저장 단계로 따로 보고) / 일회성 비용 셈? Y (logit 생성 시간, 저장 용량). teacher 학습 비용은 N / 하드웨어 = NVIDIA V100
- 설정: teacher CLIP-ViT-L/14·Florence·Swin-L·BEiT-L → TinyViT-5M/11M/21M. ImageNet-21k 90 epoch 사전학습 후 ImageNet-1k 30 epoch 미세조정.
- 평균 정확도 외 평가: ImageNet-Real, ImageNet-V2 (Table 3), 선형 평가와 few-shot (Table 5), COCO 검출 (Table 6).
- 위치: Fig. 5 (p. 11), Table 3 (p. 12), Table 4 (p. 13), Table 5·6 (p. 14), 부록 B (p. 20)
- 팀원 정리에서 추가 ([NGC1788/Reuse](https://github.com/NGC1788/Reuse), 사람이 원문을 읽고 작성, 2026-11-18):
  - "여러 student에 재사용할 수 있다"를 동기로 들지만 실제로 몇 개 student에 썼는지, 그 수로 나눈 비용은 나오지 않음.
  - 최고 결과와 비용 계산에 쓴 Florence teacher는 공개되지 않음.
  - 증강을 seed로 재현하므로 저장할 때와 학습할 때 데이터 로더, 증강 구현, 라이브러리 버전이 같아야 함.
- **정정 (검수 보고서 근거)**: 29.8%는 캐시 생성 + student 90 epoch 사전학습의 비교임. teacher 준비와 student 후속 미세조정 비용은 범위 밖. 캐싱 설계의 예로 든 CutMix는 실제 사전학습에서 Mixup과 함께 껐음. [§5.1·부록 B](https://www.alphaxiv.org/pdf/2207.10666v1?page=20)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 11건 / 수정 3건 / 추가 3건 / 확인 불가 2건. 본 판본: arXiv v1 (21 Jul 2022). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [추가] 330.9 GPU일은 Table 3의 Florence 온라인 증류 7,942 GPU시간과 같은 값이고, teacher 메모리 탓에 배치가 줄어든 상태의 비용 ("due to limited batch size", 부록 B).
  - [수정] 저장 logit으로 학습하면 추가 GPU 메모리가 없고 학습 시간이 증류 없는 학습과 같다고 **주장** (p.12, 측정 행 없음). teacher를 매번 돌리면 1.7~2.4배 시간, 2.7~10.7 GB 메모리 추가 (Table 3, V100). ← 메모: 학습 중 시간·메모리는 증류 없는 학습과 같음; 온라인은 1.7~2.4배, 2.7~10.7 GB
  - [추가] CLIP-ViT-L/14와 Florence는 IN-21k에서 30 epoch 미세조정해 teacher로 씀 (§5.1, p.10). 이 비용은 29.8% 계산에 없음.
  - [추가] 효율 지표로 V100 처리량(images/s)과 MACs 보고 (Table 4, p.13).
  - [수정] 저장 logit으로 여러 student(TinyViT-5/11/21M, DeiT-Ti/S, Swin-T)를 학습했지만, student 수로 나눈 비용이나 재사용 횟수별 비용은 보고하지 않음. ← 메모: [팀원] 실제 몇 개 student에 썼는지 나오지 않음
  - [수정] 비용 계산(부록 B)과 teacher 비교(Table 3 #4)는 Florence, 대표 결과(Table 4·Fig. 6)는 CLIP-ViT-L/14 teacher. ← 메모: [팀원] 최고 결과와 비용 계산에 쓴 Florence
  - [확인 불가] 원문에 없음. 외부 근거가 필요함.
  - [확인 불가] 원문 명시 없음. "[구조] 추론"으로 표시.
- 메모: 준비 비용과 반복 비용을 같은 단위(GPU일)로 보고해 "학생을 한 번만 학습해도 본전인가"를 계산할 수 있는 사례. 여기서는 한 번만 학습해도 29.8% 절감. epoch마다 다른 증강의 logit을 저장하므로 저장 용량이 epoch 수에 비례함 (FKD의 "crop 수"와 같은 손잡이).

### 05. Dataset Reinforcement (Reinforce Data, Multiply Impact) ([arXiv: 2303.08983](https://arxiv.org/abs/2303.08983))
- 효율화 방식: 강한 teacher 앙상블의 예측을 여러 증강에 대해 미리 계산해, 증강 매개변수와 함께 데이터셋의 일부로 저장함 (ImageNet+). 사용자는 저장된 것을 읽어 일반 학습과 같은 비용으로 학습함. 한 번 만든 데이터셋을 여러 모델·여러 사용자가 재사용하는 것이 전제.
- 얻는 것: 같은 반복 수에서 일반 ImageNet 학습과 실제 시간이 같으면서 정확도가 오름 (1000 epoch 기준 MobileNetV3-Large 75.1 → 77.9%, ResNet-50 79.6 → 81.7%, Swin-Tiny 80.9 → 83.8%). (Fig. 1, Table 10)
- 잃는 것:
  - [실측·원논문] 증강 난이도와 모델 크기의 균형이 필요함. 가벼운 CNN은 쉬운 증강, Transformer는 어려운 증강에서 더 좋아 하나의 구성으로 타협함. (Fig. 4, 부록 C)
  - [구조] teacher의 한계가 student로 넘어갈 수 있다고 한계로 적음. (p. 9)
- 옮겨 간 비용: 생성 시간과 저장 공간. 둘 다 숫자로 보고.
  - 생성: ImageNet+ 2205 GPU시간 (A100 64장으로 2080분). ResNet-50을 A100 8장에서 300 epoch 학습하는 것과 비슷한 규모. (p. 6, 부록 F)
  - 저장: 이미지당 400개 표본, 원본 61.5 GB, 압축 37.5 GB. 축소판(100개 표본, top-5)은 10.6 GB, 551 GPU시간. (Table 2·9)
  - 다른 데이터셋: CIFAR-100, Flowers-102, Food-101 생성에 90분, 40분, 120분. (부록 F)
- 어떻게 쟀나: 단위 = GPU시간, 저장 용량(GB), 학습은 반복 수와 실제 시간 / teacher forward 셈? Y (생성 시간으로) / 일회성 비용 셈? Y. teacher 앙상블 자체의 학습 비용은 N / 하드웨어 = A100 64장 (생성), A100 8장 (학습)
- 설정: teacher IG-ResNeXt 4개 앙상블 → MobileNetV1/V2/V3, ResNet, EfficientNet, ViT, Swin. ImageNet, 150~1000 epoch.
- 평균 정확도 외 평가: 분포 이동 6종 (ImageNet-V2, A, R, Sketch, ObjectNet, C; Table 7), 보정 오차 ECE (Fig. 5), 검출·분할 전이 (Table 6), 세부 분류 전이 (Table 8). ReLabel·FKD와의 비교에도 분포 이동 평균을 같이 보고 (ResNet-50: FKD 45.0% vs ImageNet+ 49.1%, Table 9).
- 위치: Fig. 1·Table 1 (p. 1), Table 2 (p. 5), Table 7·8·Fig. 5 (p. 8), Table 9·10 (p. 9), 부록 F (p. 25)
- 팀원 정리에서 추가 ([NGC1788/Reuse](https://github.com/NGC1788/Reuse), 사람이 원문을 읽고 작성, 2026-11-18):
  - 표준 ImageNet 학습 대비 학습 시간: MobileNetV3-Large 1.12배, ResNet-50 1.01배, Swin-T 0.99배.
  - 표준 학습 대비 ResNet-50: IN val +1.7, V2 +3.5, IN-R +10.0%p, ECE 9.9% 감소.
  - 이득의 분모가 온라인 KD가 아니라 teacher 없는 표준 학습임. 효율화라기보다 일회성 비용으로 정확도와 강건성을 사서 배포하는 방식으로 읽는 게 맞음.
  - 생성 비용 표현(10.7배 대 "비슷한 규모")은 단위 차이로 정리됨. 아래 **정정** 줄 참고.
- **정정 (2026-11-19 직접 대조)**: 생성 비용의 두 표현은 둘 다 원문에 있고 단위가 다름. §3(p. 6): "2205 GPUh, 64×A100" vs "ResNet-50 300 epoch, 8×A100, 206 GPUh" → GPU시간 기준 약 10.7배. 부록 F(p. 25): "2080분, 64×A100 … ResNet-50 300 epoch 8×A100 학습과 비슷" → 경과 시간 기준. 위 메모의 "비슷한 규모"는 경과 시간 기준이라 GPU시간으로 인용하면 틀림. 이 값만으로 재사용 본전 횟수를 일반화하지 않음. 저장: 원본 61.5 GB, GZIP 37.5 GB (표 9). [§3](https://www.alphaxiv.org/pdf/2303.08983v3?page=6) · [부록 F](https://www.alphaxiv.org/pdf/2303.08983v3?page=25)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 16건 / 수정 5건 / 추가 2건 / 확인 불가 1건. 본 판본: arXiv v3 (22 Sep 2023). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] 같은 반복 수에서 학습 시간은 표준 학습의 1.12배(MobileNetV3-L), 1.01배(ResNet-50), 0.99배(Swin-T) (§3, p.6). ← 메모: 같은 반복 수에서 실제 시간이 같음
  - [수정] 생성: ImageNet+ 2205 GPU시간 (A100 64장, 경과 2080분). ResNet-50 300 epoch 학습(A100 8장, 206 GPU시간)의 약 10.7배. 경과 시간으로만 비슷함 (§3 p.6, 부록 F p.25). ← 메모: 생성은 "ResNet-50을 A100 8장에서 300 epoch 학습하는 것과 비슷한 규모" (옮겨 간 비용 줄)
  - [수정] 하드웨어 = A100 64장 (생성). 학습 하드웨어는 보고 안 됨 (A100 8장은 비교 기준으로 든 ResNet-50 학습). ← 메모: 하드웨어 A100 64장 (생성), A100 8장 (학습)
  - [추가] 온라인 KD 대비: KD 300 epoch 정확도에 ImageNet+가 2.5배 짧은 경과 시간으로 도달 (ResNet-50, Fig. 1, p.1). 생성 비용 2205 GPU시간은 이 비교에 들어가지 않음.
  - [수정] 정확도·강건성 이득의 분모는 teacher 없는 표준 학습. 온라인 KD와는 Fig. 1(경과 시간 2.5배 단축)과 Table 5(정확도)로만 비교하고, 그 비교에 생성 비용은 들어가지 않음. ← 메모: [팀원] 이득의 분모가 온라인 KD가 아니라 표준 학습
  - [수정] 생성 비용은 GPU시간으로 ResNet-50 300 epoch 학습 1회의 약 10.7배 (2205 대 206 GPU시간, §3 p.6). 경과 시간으로만 비슷함. 한 번 쓰면 손해이고, 여러 번 재사용해야 본전 → "재사용 횟수 N" 논의의 근거. ← 메모: 메모: "생성 비용이 student 1회 학습과 비슷한 규모라고 스스로 밝힘 → 한 번만 쓰면 이득 없음"
  - [확인 불가] 08–14번 대조 결과로 확인 필요.
  - [추가] 혼합 증강(MixUp/CutMix)을 저장하면 짝 이미지 로딩 때문에 학습 시간 약 30% 추가. 채택한 RRC+RA/RE는 추가 없음 (부록 F, p.25).
- 메모:
  - 생성 비용이 student 1회 학습과 비슷한 규모라고 스스로 밝힘. 즉 한 번만 쓰면 이득이 없고 여러 번 써야 본전 → "재사용 횟수 N" 논의의 근거.
  - 생성 비용은 "목표 epoch 수의 약 1/3만큼의 표본에 대한 teacher 추론"이라고 요약 (부록 F). 우리 실험의 "뷰 수 대 정확도" 가설과 직접 대응.
  - Reuse 논문 중 분포 이동과 보정을 함께 본 유일한 사례.

### 06. MobileCLIP ([arXiv: 2311.17049](https://arxiv.org/abs/2311.17049))
- 효율화 방식: 이미지–텍스트 데이터셋에 (1) 합성 캡션, (2) 여러 강한 증강의 매개변수, (3) CLIP teacher 앙상블의 이미지·텍스트 임베딩을 미리 저장해 둠 (DataCompDR). 학습 때는 저장된 임베딩으로 증류하므로 teacher forward가 없음. 별도로 빠른 이미지·텍스트 인코더 구조도 제안.
- 얻는 것: 저장된 데이터셋으로 학습하면 일반 CLIP 학습과 epoch당 시간이 같다고 보고 (Table 4d). 학습 효율은 일반 학습 대비 반복 수 기준 10배, 데이터 기준 100배, Flickr30k에서는 최대 1000배라고 주장 (Fig. 6).
- 잃는 것:
  - [실측·원논문] 증류 손실 가중치에 따라 ImageNet 제로샷과 Flickr30k 검색 성능이 맞바꿔짐 (λ=1.0에서 IN-val 61.7 / Flickr 72.0, λ=0.7에서 60.7 / 74.2). (Table 3b)
  - [구조] 증강 매개변수를 저장해야 하므로 학습 중에 강도가 바뀌는 증강은 쓸 수 없음. (부록 C, p. 14)
- 옮겨 간 비용: 저장 공간과 일회성 생성. 저장 공간은 숫자로 보고: DataComp-12M 0.9 TB → DataCompDR-12M 1.9 TB, DataComp-1B 90 TB → DataCompDR-1B 140 TB (Table 4c). 생성(캡션 합성, teacher 임베딩 계산)의 시간은 "일회성 비용"이라고만 적고 숫자는 본문에서 찾지 못함.
- 어떻게 쟀나: 단위 = epoch당 학습 시간, 본 표본 수, 저장 용량(TB), 추론 지연 / teacher forward 셈? 학습 시간에는 없음(구조상 0). 생성 단계는 N / 일회성 비용 셈? 저장 용량 Y, 생성 시간 N / 하드웨어 = A100 80 GB 8장 (소규모 학습)
- 설정: teacher CLIP 앙상블 (ViT-L/14 2종) → MobileCLIP-S0/S1/S2/B. DataCompDR-12M 30~45k 반복 (약 20~30 epoch), DataCompDR-1B.
- 평균 정확도 외 평가: ImageNet 분포 이동 평균 (V2, A, O, R, ObjectNet), 검색 (Flickr30k, COCO), 38개 데이터셋 평균, ARO 벤치마크 (Table 7·8).
- 위치: Table 2 (p. 5), Table 3·4 (p. 6), Fig. 6 (p. 7), Table 7·8 (p. 8), Table 16·17 (p. 17)
- 팀원 정리에서 추가 ([NGC1788/Reuse](https://github.com/NGC1788/Reuse), 사람이 원문을 읽고 작성, 2026-11-18):
  - "강화 안 한 DataComp 대비 10배"를 teacher 출력 재사용 효과로 인용하면 과장. 재사용만의 몫은 약 3.2배로 계산됨 (계산 근거 확인 필요).
  - 합성 캡션의 오류와 편향이 데이터셋에 고정되어 계속 재사용됨.
  - 샘플당 저장 증강이 10~30개로 고정이라 여러 epoch를 돌면 같은 view가 반복됨.
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 12건 / 수정 2건 / 추가 4건 / 확인 불가 1건. 본 판본: arXiv v2 (1 Apr 2024). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [추가] 저장 증강 수: DataCompDR-12M 30개, DataCompDR-1B 10개 (p.5, Table 4c). p.6 본문은 반대로 적혀 있어 원문 안에서 어긋남.
  - [추가] 온라인 대비: 캡션 생성기와 teacher를 학습 중에 돌리면 epoch당 21.1시간으로 16배 느림. 합성 캡션만 저장하고 teacher는 돌리면 4.1시간(3배). 전부 저장하면 1.3시간 (Table 4d, A100 8장, p.6).
  - [수정] [실측·원논문] 학습 중 강도가 바뀌는 증강(RangeAugment)은 저장할 수 없어 student 입력에만 적용함. teacher에도 같은 증강을 줄 수 있으면 ImageNet-val 56.6%, 저장 방식은 55.9% (부록 C, p.14). ← 메모: [구조] 학습 중 강도가 바뀌는 증강은 쓸 수 없음
  - [수정] teacher 임베딩 저장만의 효과는 epoch당 시간 4.1 → 1.3시간, 약 3.2배 (합성 캡션은 양쪽 다 저장, Table 4d). "10배"(Fig. 6a)는 강화 안 한 학습 대비 반복 효율이라 재사용 효과로 인용하지 않음. ← 메모: [팀원] 재사용만의 몫은 약 3.2배 (계산 근거 확인 필요)
  - [확인 불가] 원문 명시 없음. "[구조] 추론"으로 표시.
  - [추가] 저장을 줄이려면 12M은 증강·캡션 5개로 30 epoch, 1B는 2개로 10 epoch을 권장 (p.7).
  - [추가] 임베딩을 BFloat16으로 저장해도 정확도가 표준편차 안에 있음 (Table 17, 부록 E).
- 메모: 저장 용량이 원본 데이터의 약 2배(12M)라서 "저장 비용이 작다"고 하기 어려운 사례. 분류가 아닌 대조 학습이지만 "증강 수와 저장 임베딩 수를 늘리면 어디서 포화하는가"(Table 4a·b: 증강 5개 근처에서 포화)는 우리 Reuse 손잡이와 같은 질문.

### 07. CLIP-Embed-KD ([arXiv: 2404.06170](https://arxiv.org/abs/2404.06170))
- 효율화 방식: teacher를 매번 돌리는 대신 클래스별 teacher 임베딩을 미리 계산해 둠 (클래스당 100개 표본의 평균). student는 CLIP식 대조 손실로 이 고정 임베딩에 정렬됨.
- 얻는 것: teacher를 돌리는 증류 대비 메모리 9배, 학습 시간 8배 감소라고 초록에서 주장. 표에서는 메모리만 숫자로 확인됨: Base-32 teacher 17배, Large-32 teacher 59배 감소. (Table 2)
- 잃는 것: [실측·원논문] 같은 student에서 teacher를 돌리는 증류보다 약 2%p 낮음: Base-16 teacher 51.68 → 49.32%, Large-32 teacher 51.95 → 50.33%. 표본별 정보가 클래스 평균으로 뭉개지기 때문이라고 설명. (Table 2, 3.2절)
- 옮겨 간 비용: 클래스별 평균 임베딩의 사전 계산. 숫자 보고 안 됨 (클래스 수 × 임베딩 차원이라 저장은 작음).
- 어떻게 쟀나: 단위 = GPU 메모리 배수, 학습 시간 배수 / teacher forward 셈? Y (비교 대상 쪽) / 일회성 비용 셈? N / 하드웨어 = 보고 안 됨
- 설정: ViT teacher (Base·Large, patch 16·32) → 작은 ViT student, CIFAR-100, 이미지 32·64, 200 epoch.
- 평균 정확도 외 평가: 없음
- 위치: 초록 (p. 1), Fig. 2·3 (p. 3), Table 2·3·Fig. 4 (p. 4)
- **정정 (검수 보고서 근거)**: 9배 메모리·8배 학습 시간 비교(Fig. 4)는 base student의 온라인 KD와 large student의 임베딩 KD를 비교한 것이라 student 크기까지 다름. 같은 base student·32px의 자원 비교는 표 2 수치를 쓰고 정확도 손실도 같이 적을 것. [Fig. 4·표 2](https://www.alphaxiv.org/pdf/2404.06170v1?page=4)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 10건 / 수정 1건 / 추가 2건 / 확인 불가 0건. 본 판본: arXiv v1 (9 Apr 2024, 5쪽). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] 표에는 메모리 배수만 있음 (Base-32 17배, Large-32 59배, Table 2). 시간은 Fig. 4 막대(epoch당 시간, 시간 단위)에 8배 감소로 표시. ← 메모: 표에서는 메모리만 숫자로 확인 (17배, 59배)
  - [추가] 사전 계산은 클래스당 100장 × 100 클래스 = teacher forward 1만 장 규모 (설정에서 계산, 원문에 시간 수치 없음).
  - [추가] teacher는 HuggingFace의 ImageNet-21k 사전학습 ViT 체크포인트, 입력 224. CIFAR-100 미세조정 여부는 원문에 없음 (§3, p.3).
- 메모: 5쪽짜리 예비 연구. 표본별 출력이 아니라 클래스 평균을 저장하므로 Reuse의 극단(뷰 0개, 클래스당 1개)에 해당. 정확도 손실 2%p가 그 대가.

### 08. 지식 저장 증류 (랩실, 검출) ([DBpia](https://www.dbpia.co.kr/journal/articleDetail?nodeId=NODE12055100))
- 효율화 방식: 밀집 객체 검출 증류에서 teacher 검출기의 특징 맵 중 비최대 억제(NMS)로 고른 핵심 특징만 미리 저장하고 학습 때 재사용함. 학습 중 teacher forward가 없음.
- 얻는 것 (student 단독 학습 시간 대비 배수, COCO, 12 epoch):
  - GFL ResNet-101 → ResNet-50: 제안 1.05배, FGD 1.68배, PGD 1.51배, BCKD 1.42배.
  - FCOS ResNet-101 → ResNet-50: 제안 1.06배, FGD 1.61배. (표 1)
- 잃는 것: [실측·원논문] 검출기에 따라 우열이 바뀜. GFL에서는 43.8 mAP로 FGD(43.4)보다 높지만 FCOS에서는 39.6 mAP로 FGD(41.0)보다 1.4 낮음. (표 1)
- 옮겨 간 비용: 저장 공간. 숫자로 보고: GFL 12 GB, FCOS 6.8 GB (표 1). 저장 영역을 전경만으로 줄이면 8 GB에 43.6 mAP, 배경만이면 4 GB에 42.7 mAP (표 3). 특징 추출·저장에 드는 시간은 보고 안 됨.
- 어떻게 쟀나: 단위 = 학습 시간 배수(student 단독 = 1배), 저장 용량(GB) / teacher forward 셈? Y (비교 대상의 학습 시간에 포함) / 일회성 비용 셈? 저장 용량 Y, 저장 단계 시간 N / 하드웨어 = RTX A6000 8장
- 설정: GFL·FCOS, ResNet-101 → ResNet-50, MS COCO (학습 120k, 검증 5k), 12 epoch, batch 32.
- 평균 정확도 외 평가: 없음 (mAP, AP50, AP75)
- 위치: 표 1–3 (p. 3)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 11건 / 수정 3건 / 추가 5건 / 확인 불가 0건. 본 판본: 전자공학회논문지 Vol.62 No.1 (2025.1), pp. 95–98, doi 10.5573/ieie.2025.62.1.95 (PDF 4쪽). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] 효율화 방식: teacher 검출기의 FPN 특징 맵에서 Soft-NMS-Gaussian으로 고른 탐지 상자 위치의 특징만, 전경·배경으로 나누고 좌표·증강 정보와 함께 미리 저장. 학습 때는 저장된 증강 케이스 하나를 무작위로 불러와 student 특징을 같은 위치에서 맞춤. 학습 중 teacher forward 없음. (§II.1–2, p. 2) ← 메모: NMS로 고른 핵심 특징만 미리 저장, 학습 때 재사용
  - [추가] FCOS: 제안 1.06배, FGD 1.61배, PGD 1.52배, BCKD 1.41배. (표 1, p. 3)
  - [추가] FCOS에서 FGD(41.0)보다 1.4 낮지만 PGD(39.2)·BCKD(39.1)보다는 높음. FCOS FGD는 teacher(40.8)보다도 높음. (표 1, p. 3)
  - [수정] 저장 영역을 전경만으로 줄이면 8 GB에 43.6 mAP, 배경만이면 4 GB에 42.7 mAP (GFL, 표 3, p. 3). (검수 보고서와 같음) ← 메모: 전경만 8 GB 43.6, 배경만 4 GB 42.7
  - [추가] 선택 없이 teacher 중간 특징을 그대로 저장하면 COCO에서 4.0 TB 필요 (랜덤 플립 증강 기준, p. 1). 제안 방법의 12 GB는 그 약 1/330.
  - [추가] NMS 방법별 저장량과 mAP (GFL): Greedy 13.6 GB·43.6, Soft-linear 14.4 GB·43.7, Soft-Gaussian 12 GB·43.8, DIoU 14.4 GB·43.6. (표 2, p. 3)
  - [수정] 평균 정확도 외 평가: 정량 지표는 mAP·AP50·AP75뿐. 정성 평가로 특징 맵 시각화 (그림 2, p. 3). 강건성·보정 등은 없음. ← 메모: 평균 정확도 외 평가: 없음 (mAP, AP50, AP75)
  - [추가] 위치: 방법 §II (p. 2), 표 1–3·그림 2 (p. 3), 표 4·결과 해석 (p. 4)
- 메모: 학습 시간 배수와 저장 용량을 한 표에 같이 보고함. 분류가 아닌 검출이지만 "teacher 연산이 저장 공간으로 옮겨 간다"를 숫자로 보여 주는 국내 사례. 저장 영역을 줄이면 용량과 mAP가 함께 줄어드는 표 3은 저장 예산 곡선의 예.

### 09. Are Large-scale Soft Labels Necessary? (LPLD) ([arXiv: 2410.15919](https://arxiv.org/abs/2410.15919))
- 효율화 방식: 데이터셋 증류(합성 이미지)에서 평가용으로 저장하는 soft label의 양을 줄임. 합성 단계에서 클래스별로 배치를 구성해 클래스 내 다양성을 높이면, 저장할 soft label을 무작위로 솎아내도 성능이 유지된다는 것.
- 얻는 것 (저장 용량, 기존 방법 대비): ImageNet-1K를 클래스당 200장으로 증류한 조건에서 soft label 113 GB → 2.8 GB (40배 압축), 정확도는 +2.6%p. ImageNet-21K-P 클래스당 20장에서는 1285 GB → 32 GB. (초록, Table 7)
- 잃는 것:
  - [실측·원논문] 압축률을 높이면 떨어짐: ResNet-50, 클래스당 50장, 1배 62.2% → 10배 61.2% → 30배 56.2% → 100배 44.7%. (Table 6b)
  - [실측·원논문] 전체 데이터 학습과는 큰 차이가 남음: ResNet-50 전체 76.13% vs 클래스당 50장 62.24%. (Table 6c)
- 옮겨 간 비용: 클래스별 BN 통계 저장 (ImageNet-1K에서 +36.64 MB, 합성 단계에만 필요, Table 25). 합성 자체의 비용은 기존 방법과 같은 틀.
- 어떻게 쟀나: 단위 = 저장 용량(GB), 압축 배수 / teacher forward 셈? N / 일회성 비용 셈? N (합성·재라벨링 시간 보고 안 됨) / 하드웨어 = A100 80 GB 4장
- 설정: Tiny-ImageNet, ImageNet-1K, ImageNet-21K-P. 합성과 재라벨링에 쓰는 teacher와 평가 student는 주로 ResNet-18, 다른 구조로 교차 평가.
- 평균 정확도 외 평가: 없음 (다른 구조로의 일반화만, Table 6c)
- 위치: 초록·Fig. 1 (p. 1), Table 2·3 (p. 7), Table 6–9 (p. 9), Table 21–24 (p. 20–21), Table 25·부록 E.5 (p. 23)
- 팀원 정리에서 추가 ([NGC1788/Reuse](https://github.com/NGC1788/Reuse), 사람이 원문을 읽고 작성, 2026-11-18):
  - 압축 안 한 SRe2L(57.0%) 대비 +2.6%p는 가장 약한 기준선 대비임. 같은 압축률끼리는 IPC200 40배에서 CDA 54.4% vs LPLD 59.6% (+5.2%p)가 이 방법의 실제 강점.
  - IPC가 작으면 압축 손실이 큼: ImageNet-1K IPC10에서 1배 34.6% → 40배 20.2%.
  - squeeze 단계에서 원본 데이터 1 epoch를 더 돌아 class별 BN 통계를 계산함.
  - label을 다 만든 뒤 고르는 방식이라 생성 비용(teacher forward)은 그대로. 학습 epoch도 그대로라 학습 시간도 줄지 않음.
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 13건 / 수정 5건 / 추가 7건 / 확인 불가 0건. 본 판본: arXiv 2410.15919v2 (3 Nov 2024), NeurIPS 2024. 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] 효율화 방식: 데이터셋 증류(합성 이미지)의 사후 학습용 soft label 저장량을 줄임. 합성 단계에서 같은 클래스끼리 배치를 묶고 클래스별 BN 통계로 감독해 클래스 내 다양성을 높임. 이후 label을 batch 단위로 무작위 솎아내고, 여러 epoch의 batch를 섞은 label pool에서 무작위로 다시 씀. (§3.3, p. 5–7) ← 메모: 클래스별 배치로 클래스 내 다양성 ↑, 무작위로 솎아도 유지
  - [수정] ImageNet-1K IPC200: soft label 113.33 GB → 약 2.8 GB (40배). ResNet-18 정확도 59.6% vs 압축 안 한 SRe2L 57.0% (+2.6%p). (초록 p. 1, Table 3 p. 7, Table 24 p. 21) ← 메모: IN-1K IPC200: 113 GB → 2.8 GB (40배), +2.6%p. "(초록, Table 7)"
  - [추가] ImageNet-21K-P IPC20: 1,285 GB → 32 GB (40배), 정확도 29.4% vs 압축 안 한 SRe2L 20.5%. (Table 7, p. 9)
  - [추가] squeeze 단계에서 모델을 고정한 채 원본 학습 데이터 1 epoch(IN-1K 5,005 업데이트)를 돌려 클래스별 BN 통계를 계산. 시간은 보고 안 됨. (p. 7, Table 10 p. 16)
  - [수정] 하드웨어 = A100 80 GB, ImageNet-1K·21K-P는 4장, Tiny-ImageNet은 1장. (App. E.5, p. 23) (검수 보고서와 같음) ← 메모: 하드웨어 = A100 80 GB 4장
  - [추가] 평균 정확도 외 평가: student 쪽은 없음 (top-1과 교차 구조 Table 6c뿐). 합성 데이터의 클래스 내 다양성은 특징 코사인 유사도(Table 1, p. 4)와 MMD(Fig. 3)로 잼.
  - [추가] 압축 없는 1×에서는 CDA(63.3%)가 LPLD(62.6%)보다 높음 (IN-1K IPC200, Table 3, p. 7). 40배 압축 LPLD(59.6%)는 압축 안 한 CDA보다 3.7%p 낮음.
  - [수정] label 전체를 만든 뒤 고르는 방식이라 생성 비용(teacher forward)은 그대로. 학습 epoch도 그대로라 학습 시간이 줄지 않을 수 있다고 저자가 적음. 시간 측정은 없음. (App. E.6, p. 23) ← 메모: 팀원: 생성 비용 그대로, 학습 epoch 그대로라 학습 시간도 줄지 않음
  - [수정] label 수를 줄여도 학습 epoch이 그대로라 학습 시간이 줄지 않을 수 있다고 저자가 적음 (측정 없음, App. E.6, p. 23). ← 메모: 메모: "학습 속도가 줄지는 않는다고 스스로 적음 (p. 23)"
  - [추가] 압축 전 label 저장량은 이미지의 37–38배 (IN-1K), 21–22배 (Tiny-ImageNet), 214–257배 (21K-P). (Table 24, p. 21)
  - [추가] IPC400·30배 압축은 이미지+label 총 13 GB에 65.2%, 전체 ImageNet-1K(138 GB)는 69.8% (ResNet-18, Table 20, p. 19).
  - [추가] FKD 식 label 양자화와 비교: IN-1K IPC10에서 LPLD 10배 32.70% vs FKD 8.2배 18.10%. FKD는 logit 차원만 줄이고, 이 방법은 증강 수를 줄임. (Table 21–23, p. 20)
- 메모: 113 → 2.8 GB는 "클래스당 200장으로 응축한 ImageNet"의 label 저장량임. 일반 ImageNet의 logit 캐시(FKD의 13.3 GB 등)와 같은 줄에 놓으면 안 됨. label 수를 줄여도 학습 epoch이 그대로라 학습 속도가 줄지는 않는다고 스스로 적음 (p. 23).

### 10. TinyCLIP ([arXiv: 2309.12314](https://arxiv.org/abs/2309.12314))
- 효율화 방식: 큰 CLIP teacher의 가중치 일부를 student 초기값으로 물려받고(가중치 상속), 이미지–텍스트 유사도 분포를 모방하는 손실로 증류함. 여러 단계에 걸쳐 조금씩 줄여 나감.
- 얻는 것 (처음부터 학습하는 OpenCLIP 대비, 비슷한 정확도에 도달하는 학습 시간): OpenCLIP ViT-B/32는 LAION-400M 32 epoch에 약 287 GPU일. TinyCLIP은 매개변수 75% 모델에서 7.8배, 50% 모델에서 1.4배 빠름. (Fig. 6, 4.3절)
- 잃는 것: [실측·원논문] 크기를 줄이면 정확도가 떨어짐: ImageNet 제로샷 OpenCLIP ViT-B/32 62.9% → TinyCLIP ViT-45M/32 61.2% (Table 5). 한 번에 25%로 줄이면 47.0%, 두 단계로 줄이면 49.0% (Table 6).
- 옮겨 간 비용: teacher를 처음부터 학습하는 비용(287 GPU일 규모)은 이미 있다고 가정. 상속할 가중치를 고르는 마스크 학습(자동 상속)이 추가됨. 둘 다 절감 계산에 넣지 않음.
- 어떻게 쟀나: 단위 = GPU일 (목표 정확도 도달 시간), 추론 처리량 / teacher forward 셈? 증류 학습 시간에 포함된 것으로 보이나 명시 없음 / 일회성 비용 셈? N / 하드웨어 = V100 또는 A100 32장
- 설정: teacher OpenCLIP ViT-B/32 등 → TinyCLIP ViT (63M+31M, 45M+18M 등), LAION-400M, YFCC-15M. 2단계 6+16 epoch.
- 평균 정확도 외 평가: 제로샷 강건성 (ImageNet-V2, A, R, ObjectNet, Sketch; Table 7), 23개 데이터셋 제로샷·선형 평가 (Table 5).
- 위치: 초록 (p. 1), Table 1 (p. 6), Table 3·4 (p. 7), Table 5–7·Fig. 6 (p. 8)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 9건 / 수정 3건 / 추가 4건 / 확인 불가 0건. 본 판본: arXiv 2309.12314v1 (21 Sep 2023). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [추가] 주의: teacher는 LAION-2B로 학습한 OpenCLIP ViT-B/32(ImageNet 제로샷 65.7%)이고, 7.8배·1.4배의 비교 기준은 LAION-400M에서 처음부터 학습한 OpenCLIP ViT-B/32(62.9%)임. (§4.1 p. 5, Table 1 p. 6)
  - [추가] 같은 ViT-45M/32가 Table 1에서는 61.4%, Table 5(Elevater)에서는 61.2%.
  - [수정] 옮겨 간 비용: teacher(LAION-2B 학습) 비용은 이미 있다고 가정, 절감 계산에 없음. 자동 상속의 마스크 학습은 각 단계 첫 3,000 iteration 안에서 이루어져 학습 일정에 포함되나, 따로 분리해 보고하지 않음. (Alg. 1, p. 5) (검수 보고서와 같음) ← 메모: 마스크 학습이 추가됨. 둘 다 절감 계산에 넣지 않음
  - [수정] 설정: LAION-400M 3단계 6+16+16 epoch (100→75→50→25%), YFCC-15M 2단계 25+25 epoch. Fig. 6의 학습 시간 비교는 앞 2단계(6+16 epoch). (p. 5, p. 8) ← 메모: 설정: 2단계 6+16 epoch
  - [추가] 평균 정확도 외 평가: 제로샷 강건성 (IN-V2·A·R·ObjectNet·Sketch, Table 7), 23개 데이터셋 제로샷·선형 평가 (Table 5), 이미지–텍스트 검색 R@1 (Flickr30k·MSCOCO, Table 1).
  - [수정] 위치: 초록 (p. 1), Alg. 1·학습 설정 (p. 5), Table 1 (p. 6), Table 2–4 (p. 7), Table 5–7·Fig. 6 (p. 8) ← 메모: 위치: 초록 p.1, Table 1 p.6, Table 3·4 p.7, Table 5–7·Fig.6 p.8
  - [추가] 추론 처리량 (V100, batch 1,024): OpenCLIP ViT-B/32 2,452 pairs/s → TinyCLIP ViT-45M/32 3,682 (1.5배). (Table 1, p. 6)
- 메모: 재사용 대상이 teacher 출력이 아니라 가중치임. 학습 중 teacher forward는 그대로 있음. 절감의 분모가 "KD"가 아니라 "처음부터 학습"이라는 점에 주의.

### 11. Initializing Models with Larger Ones (Weight Selection) ([arXiv: 2311.18823](https://arxiv.org/abs/2311.18823))
- 효율화 방식: 사전학습된 큰 모델의 가중치에서 일부를 균등하게 골라 같은 계열의 작은 모델을 초기화함. 증류 손실도, teacher forward도 없음.
- 얻는 것:
  - 무작위 초기화 대비: CIFAR-100, ViT-T에서 1/3 epoch으로 같은 정확도. (Fig. 4a)
  - ImageNet-1K 사전학습 후 미세조정 대비: 같은 CIFAR-100 정확도에 도달하는 학습 시간이 6.12배 짧음 (사전학습 60 epoch에 해당). (Fig. 4b)
  - 정확도: ImageNet-1K ViT-T 73.9 → 75.5%, KD만 쓰면 74.8%, 둘을 합치면 76.0%. (Table 4)
- 잃는 것:
  - [실측·원논문] 충분히 사전학습한 뒤 미세조정한 모델보다는 낮음 (CIFAR-100: 사전학습 300 epoch 85.7% vs weight selection 81.4%, Fig. 4b).
  - [구조] 같은 모델 계열의 사전학습된 큰 모델이 있어야 함. teacher가 클수록 좋은 것은 아님 (Table 8).
- 옮겨 간 비용: 큰 모델의 사전학습 비용 (공개 가중치를 쓰면 0). 선택 자체는 추가 연산이 없다고 적음. 숫자 보고 안 됨.
- 어떻게 쟀나: 단위 = epoch, 학습 시간 배수 / teacher forward 셈? 해당 없음 (teacher를 돌리지 않음) / 일회성 비용 셈? N / 하드웨어 = 보고 안 됨
- 설정: ImageNet-21K 사전학습 ViT-S → ViT-T, ConvNeXt-T → ConvNeXt-F. ImageNet-1K와 소규모 데이터셋 8종.
- 평균 정확도 외 평가: 선형 평가 (Table 9). 그 외 없음.
- 위치: Table 2 (p. 5), Fig. 3·Table 3 (p. 6), Table 4·Fig. 4 (p. 7), Table 5–8 (p. 8)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 10건 / 수정 2건 / 추가 3건 / 확인 불가 0건. 본 판본: arXiv 2311.18823v1 (30 Nov 2023). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] 효율화 방식: 같은 계열의 사전학습된 큰 모델에서 층 선택(앞쪽 N개), 구성요소 대응, 원소 선택(균등 간격)의 세 단계로 가중치 일부를 골라 작은 모델을 초기화. 증류 손실도 teacher forward도 없음. (p. 2–4, Fig. 2 p. 3) ← 메모: 큰 모델 가중치 일부를 균등하게 골라 초기화. 증류 손실·teacher forward 없음
  - [추가] Table 4의 IN-1K 실험 teacher는 IN-1K 사전학습 ViT-S(DeiT)로, KD와 가중치 선택에 같이 씀. 같은 WS 수치가 Table 2에서는 75.6%로 적혀 있음. CIFAR-100 특징 증류: 72.3 → KD 78.4, WS 81.4, 합 83.9%. (Table 4, p. 7)
  - [수정] 설정: ImageNet-1K와 소규모 데이터셋 9종 (CIFAR-10·100, Flowers, Pets, STL-10, Food-101, DTD, SVHN, EuroSAT). 원문은 "9 datasets"로 적었으나 표 2에는 10개. (p. 5) ← 메모: ImageNet-1K와 소규모 데이터셋 8종
  - [추가] 평균 정확도 외 평가: 선형 평가 (CIFAR-100, Table 9, p. 9), self-attention 가중치 시각화 (Fig. 5, p. 10).
  - [추가] 위치에 "Table 9–11 (p. 9), Table 12·Fig. 5 (p. 10)" 추가
- 메모: KD가 아니라 초기화 방법임. "teacher 비용 0인 대안"으로서 KD와 같은 표에 놓을 수 있는 기준점. KD와 합치면 이득이 더해진다는 Table 4는 "가중치 재사용의 기여"와 "증류의 기여"를 분리한 사례.

### 12. SimKD (KD with the Reused Teacher Classifier) ([arXiv: 2203.14001](https://arxiv.org/abs/2203.14001))
- 효율화 방식: student 특징을 투영층으로 teacher 차원에 맞춘 뒤 teacher의 마지막 분류기를 그대로 가져다 씀. 학습 손실은 특징 정렬(L2) 하나뿐.
- 얻는 것: 학습 비용 절감 수치는 보고 안 됨. 보고한 것은 적은 epoch에서의 정확도 (ImageNet, ResNet-18 student): 1/4 epoch에서 KD 52.75% vs SimKD 61.73%, 1/2 epoch에서 66.69 vs 69.26%, 전체에서 71.29 vs 71.66%. (Table 3)
- 잃는 것:
  - [구조] 투영층이 추론에도 남아 student 매개변수가 늘어남. 논문은 이를 "프루닝 비율 비용"으로 분석. (Fig. 6·7)
  - [실측·원논문] teacher 분류기를 재사용하지 않고 새 분류기를 학습하면 크게 떨어지는 조합이 있음 (ResNet-8x4: 51.97 vs 78.08%). (Table 4)
- 옮겨 간 비용: 투영층의 학습과 추론 연산. teacher forward는 매 스텝 그대로 필요 (특징 정렬의 목표).
- 어떻게 쟀나: 단위 = epoch, 추가 매개변수 비율 / teacher forward 셈? N / 일회성 비용 셈? N / 하드웨어 = RTX 2080 Ti 8장 (CIFAR)
- 설정: CIFAR-100 여러 teacher–student 쌍, ImageNet ResNet-50 → ResNet-18 등.
- 평균 정확도 외 평가: 음의 로그 가능도 (Fig. 5), t-SNE 시각화. 그 외 없음.
- 위치: Table 1·2 (p. 5), Table 3·4·Fig. 5 (p. 6), Fig. 6·7 (p. 7), 부록 (p. 9–10)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 8건 / 수정 3건 / 추가 2건 / 확인 불가 0건. 본 판본: PDF에 arXiv 번호·학회 표기 없음 (본문 8쪽 + 부록, 총 15쪽). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] 적은 epoch에서의 정확도 (ImageNet, ResNet-50 → ResNet-18, 전체 120 epoch): 30 epoch(1/4) KD 52.75% vs SimKD 61.73%, 60 epoch(1/2) 66.69 vs 69.26%, 120 epoch 71.29 vs 71.66%. (Table 3, p. 6) ← 메모: 1/4 epoch KD 52.75 vs 61.73, 1/2 66.69 vs 69.26, 전체 71.29 vs 71.66
  - [추가] 투영층으로 늘어나는 매개변수는 프루닝 비율 비용으로 14쌍 중 10쌍에서 1% 미만, 기본 r=2. (Fig. 7, p. 7)
  - [수정] [실측·원논문] 정렬된 student 인코더를 고정하고 새 분류기를 처음부터 학습하면 크게 떨어지는 조합이 있음 (CIFAR-100 ResNet-32x4 → ResNet-8x4: 51.97 vs 78.08%; ShuffleNetV2 21.56 vs 78.39%). (Table 4, p. 6) ← 메모: 새 분류기 학습 시 크게 떨어짐: ResNet-8x4 51.97 vs 78.08
  - [수정] 하드웨어 = CIFAR-100은 RTX 2080 Ti 8장, ImageNet은 A40 4장. (부록 A.1, p. 9) (검수 보고서와 같음) ← 메모: 하드웨어 = RTX 2080 Ti 8장 (CIFAR)
  - [추가] 위치: Fig. 3 (p. 4), Table 1·2 (p. 5), Table 3·4·Fig. 5 (p. 6), Fig. 6·7 (p. 7), Table 5–7 (p. 8), 부록 (p. 9–15)
- 메모: 재사용 대상이 분류기라서 teacher forward 생략과 무관함. "출력 캐시 용량"을 묻는 집계에서는 해당 없음으로 처리해야 함. 적은 epoch에서 강하다는 결과는 Skip(짧은 학습)과 결합했을 때의 근거로만 쓸 수 있음.

### 13. MobileSAM ([arXiv: 2306.14289](https://arxiv.org/abs/2306.14289))
- 효율화 방식: SAM의 이미지 인코더(ViT-H)만 작은 인코더(TinyViT)로 증류하고, 프롬프트 인코더와 마스크 디코더는 원래 것을 그대로 씀 (분리 증류). teacher 이미지 임베딩을 미리 저장해 teacher forward를 한 번만 함.
- 얻는 것 (인코더와 디코더를 함께 학습하는 방식 대비): 연산 자원 1% 미만으로 mIoU 0.75 vs 0.72. 결합 방식은 GPU 128장·180k 반복·1,100만 장, 분리 방식은 GPU 2장·55k 반복·1.1만 장. (Table 2, ViT-B 기준) MobileSAM 전체 학습은 GPU 1장으로 하루 미만.
- 잃는 것: [실측·원논문] 원래 SAM의 마스크를 정답으로 본 mIoU가 학습량에 따라 0.71~0.74 (Table 4). 원래 SAM과의 차이를 표준 분할 벤치마크로 잰 수치는 본문에서 확인 안 함.
- 옮겨 간 비용: teacher 이미지 임베딩의 사전 계산과 저장. 숫자 보고 안 됨.
- 어떻게 쟀나: 단위 = GPU 수 × 반복 수 × 데이터 수 (대략적 비교), 추론 시간(ms) / teacher forward 셈? 저장 단계 1회로 처리, 시간은 N / 일회성 비용 셈? N / 하드웨어 = GPU 1장 (종류 보고 안 됨)
- 설정: SAM ViT-H (632M) → TinyViT (5.78M), SA-1B의 1% (11만 장 규모)로 8 epoch.
- 평균 정확도 외 평가: 없음 (mIoU, 정성 비교)
- 위치: 초록 (p. 1), Table 2·3 (p. 5), Table 4–7 (p. 7)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 8건 / 수정 3건 / 추가 3건 / 확인 불가 0건. 본 판본: arXiv 2306.14289v2 (1 Jul 2023; 첫 쪽 날짜 July 4, 2023). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [추가] Table 2는 ViT-B 인코더 비교: 결합 GPU 128장·batch 128·180k 반복·1,100만 장 vs 분리 GPU 2장·batch 4·55k 반복·1.1만 장(SA-1B의 0.1%), mIoU 0.72 vs 0.75 (200개 표본 평균). (p. 5)
  - [수정] 원래 SAM과의 차이를 사람 정답 기반 분할 벤치마크로 잰 수치는 보고 안 됨. 모든 mIoU는 원래 SAM 마스크를 정답으로 본 일치도. (p. 5–7) ← 메모: 표준 분할 벤치마크 수치는 본문에서 확인 안 함
  - [수정] 단위 = GPU 수·batch·반복·데이터 수를 나란히 적고 "1% 미만"으로 요약 (결합 단위 정의 없음), 추론 시간(ms). (Table 2·3, p. 5) (검수 보고서와 같음) ← 메모: 단위 = GPU 수 × 반복 수 × 데이터 수 (대략적 비교)
  - [추가] 추론 시간 (GPU 1장): 이미지 인코더 452 ms → 8 ms (Table 3, p. 5). FastSAM 비교는 Table 6(12 vs 64 ms)과 본문(10 vs 40 ms)의 수치가 서로 다름 (p. 7).
  - [추가] 평균 정확도 외 평가: 없음. 정량은 원래 SAM과의 mIoU뿐 (FastSAM과의 prompt 거리별 비교 Table 7, p. 7). 정성 비교 Fig. 4–6 (p. 6–8).
  - [수정] 위치: 초록 (p. 1), 방법·Fig. 2·3 (p. 3–4), Table 2·3 (p. 5), Table 4–7 (p. 7) ← 메모: 위치: 초록 p.1, Table 2·3 p.5, Table 4–7 p.7
- 메모: 분류가 아닌 분할 사례. 재사용이 두 겹임: teacher의 디코더(가중치)와 teacher 임베딩(출력). "1% 미만"은 설정이 다른 두 학습을 GPU 수와 반복 수로 대략 비교한 값이라 정밀한 비용 수치로 인용하기 어려움.

### 14. Tailored Coordinate System (TCS) ([arXiv: 2412.09388](https://arxiv.org/abs/2412.09388))
- 효율화 방식: teacher 특징에 PCA를 적용해 좌표계를 한 번 구하고, student 특징이 그 좌표계에 맞도록 학습함. 좌표계 계산에는 증강 없는 teacher forward 한 번이면 되고, 이후 학습에는 teacher가 필요 없음.
- 얻는 것 (CIFAR-100, ResNet-18 student, ViT-S teacher, 3090 1장):
  - epoch당 시간: 일반 KD 49.90초 → TCS 32.14초 (처음부터 학습 29.34초).
  - GPU 메모리: KD 7.5 GB → TCS 5.8 GB (처음부터 학습 3.5 GB). (Table 1)
  - ImageNet-1K, DeiT-T teacher: KD 415.33초 → TCS 328.24초 (epoch당). (Table 2)
- 잃는 것: [실측·원논문] 표에서는 정확도 손실 없음 (CIFAR-100 KD 77.26% → TCS 81.37%). 메모리는 처음부터 학습보다 2.3 GB 많음. (Table 1)
- 옮겨 간 비용: teacher forward 1회와 PCA 계산. "무시할 만하다"고 적고 숫자 보고 안 됨.
- 어떻게 쟀나: 단위 = epoch당 실제 시간(초), GPU 메모리(GB) / teacher forward 셈? Y (비교 대상의 시간에 포함) / 일회성 비용 셈? N (좌표계 계산 시간 없음) / 하드웨어 = NVIDIA RTX 3090 1장
- 설정: teacher ViT-S·ConvNeXt-T·Mixer-B/16·DeiT-T·Swin-T → ResNet-18. CIFAR-100 (224로 확대), ImageNet-1K 100 epoch. few-shot 설정은 MoCo v3·DINOv2 사전학습 모델.
- 평균 정확도 외 평가: few-shot 학습 (CUB, ImageNet-1K; Table 3). 그 외 없음.
- 위치: Fig. 1 (p. 2), Table 1·2 (p. 5), Table 3 (p. 6), Table 4·5 (p. 7)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 9건 / 수정 3건 / 추가 4건 / 확인 불가 0건. 본 판본: arXiv 2412.09388v2 (12 Feb 2025), AAAI 2025. 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [추가] 효율화 방식 보충: 좌표계 계산은 학습 데이터 전체에 대한 증강 없는 teacher forward 1회. 지도 teacher에서는 eLSH 모듈의 표본별 해시 목표도 같은 1회 forward로 미리 계산. SSL teacher(few-shot)에서는 eLSH를 쓰지 않음. (p. 2–5)
  - [추가] ImageNet-1K epoch당 시간: DeiT-T teacher KD 415.33 → TCS 328.24초, Swin-T 701.80 → 321.27초, Mixer-B 869.97 → 341.88초. 메모리 KD 7.5–7.6 → TCS 5.8 GB. (Table 2, p. 5)
  - [수정] [실측·원논문] 일반 KD 표(Table 1·2)에서는 정확도 손실 없음 (CIFAR-100 KD 77.26% → TCS 81.37%). few-shot에서는 1·2-shot에서 기준보다 낮은 경우가 있음 (IN-1K DINOv2 1-shot 34.2 vs 37.0, Table 3, p. 6). 메모리는 처음부터 학습보다 2.3 GB 많음 (Table 1). ← 메모: 정확도 손실 없음 (KD 77.26 → TCS 81.37)
  - [수정] 하드웨어 = RTX 3090 1장 (CIFAR-100 Table 1 캡션). ImageNet Table 2의 장치는 명시 없음. (p. 5) ← 메모: 하드웨어 = RTX 3090 1장
  - [추가] Table 1의 기존 방법 정확도는 LSHL2(♯)만 저자가 직접 돌렸고 나머지는 각 논문에서 가져옴. (p. 5)
  - [수정] 위치: Fig. 1 (p. 2), 방법·Fig. 2 (p. 3–4), Table 1·2 (p. 5), Table 3 (p. 6), Table 4·5 (p. 7) ← 메모: 위치: Fig.1 p.2, Table 1·2 p.5, Table 3 p.6, Table 4·5 p.7
  - [추가] 메모리 절감은 초록의 "절반"이 아니라 표 기준 약 23% (7.5 → 5.8 GB). 본문도 "roughly 25%"로 적음. (p. 1, p. 6)
- 메모: 같은 GPU에서 처음부터 학습, KD, 제안 방법의 epoch당 시간과 메모리를 한 표에 넣음. "teacher가 학습 시간의 얼마를 차지하는가"를 읽을 수 있는 드문 표: ViT-S teacher KD는 처음부터 학습의 1.7배, Mixer-B/16 teacher는 2.4배.

## 축 분류 메모

- 10번 TinyCLIP, 11번 Weight Selection, 12번 SimKD는 teacher의 가중치·분류기를 재사용함. teacher forward를 생략하지 않거나(10, 12) 아예 증류가 아님(11). "출력 캐시 용량"을 묻는 집계의 분모에서 빼거나 "해당 없음"으로 따로 표시해야 함.
- 03번 FerKD는 Reuse + Skip, 09번 LPLD는 Reuse + Substitute(합성 데이터) 성격.
- 일회성 비용 보고 현황 (출력을 저장하는 01–09번, 13·14번 기준):
  - 생성 시간과 저장 용량을 둘 다 숫자로 보고: 02 ReLabel, 04 TinyViT, 05 Dataset Reinforcement.
  - 저장 용량만 보고: 01 FKD, 06 MobileCLIP, 08 지식 저장 증류, 09 LPLD.
  - 둘 다 없음: 03 FerKD, 07 CLIP-Embed-KD, 13 MobileSAM, 14 TCS.
  - 이 구분은 이번 읽기에서 본 범위 기준임. 기존 `0/18`, `3/14` 집계와 맞는지는 기준을 정해 다시 세어야 함.
