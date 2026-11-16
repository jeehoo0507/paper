# Efficient KD: 읽을 논문

축은 초안 기준. 분류는 초록 기준이라 읽으면서 옮길 수 있음.

## 내일 읽을 것 (최종 17편)

축별로 뽑되, **그 논문으로 우리가 주장할 것**이 있는 것만.

| 파트 | 파일 | 이 논문으로 주장할 것 |
|---|---|---|
| Shrink | `1_Shrink/01_MaskedKD` | 보고 범위가 teacher FLOPs뿐. 사례 실험 대상 |
| Shrink | `1_Shrink/02_DMAE` | 같은 Shrink라도 줄이는 항이 다름 (teacher 깊이 + student 입력) |
| Skip | `2_Skip/01_Distill on a Diet` | 학습된 데이터 선택. 선택 비용을 셌나 |
| Skip | `2_Skip/02_Distilling the Knowledge in Data Pruning` | 반대 주장: KD 쓰면 무작위 프루닝으로 충분 |
| Skip | `2_Skip/09_Weak-to-Strong KD` | "빨리 끝난다". Patient and Consistent와 정반대 |
| Reuse | `3_Reuse/01_FKD` | 저장 0.9TB, 증강 고정, 라벨 생성 비용 미보고 |
| Reuse | `3_Reuse/04_TinyViT` | 온라인 teacher가 학습 시간 1.7~2.4배. 일회성 비용까지 센 유일한 사례 |
| Reuse | `3_Reuse/09_Are Large-scale Soft Labels Necessary` | 비용이 저장으로 옮겨 감 (soft label 113GB) |
| Substitute | `4_Substitute/01_KD from Model Checkpoints` | teacher 학습 비용 대체. 실제로 셌나 |
| Substitute | `4_Substitute/08_Proteus` | 데이터 대체. 절감을 무엇으로 보고했나 |
| Q2 측정 | `5_Measurement (Q2)/01_Patient and Consistent` | KD 레시피는 길다 (300 → 9600 에폭) |
| Q2 측정 | `5_Measurement (Q2)/04_Distillation Scaling Laws` | teacher 비용을 넣으면 KD가 언제 이득인가 |
| Q2 측정 | `5_Measurement (Q2)/06_Are Coreset Methods Worth Their Cost` | 선택 비용까지 넣으면 이득이 사라짐 |
| Q3 평가 | `6_Evaluation (Q3)/01_Teacher's Pet` | KD 이득이 그룹마다 다름 |
| Q3 평가 | `6_Evaluation (Q3)/05_When Masking Helps or Hurts` | 마스킹이 WGA를 데이터셋마다 다르게 바꿈 |
| 선행 서베이 | `survey paper/papers/PART 2/06_Efficient On-Policy Distillation` | 비용 계산 틀은 있지만 언어 모델 한정, 실험 없음 |
| 선행 서베이 | `survey paper/papers/PART 3/06_A Survey on Efficient Vision Transformers` | 효율 지표가 student 추론 비용만 셈 |

경로는 `Related Work/Efficient KD papers/` 기준 (`survey paper/`로 시작하는 것만 `Related Work/` 기준).

## 정독 목록 (전체 27편)
1순위 14편 포함. 축 안의 하위 메커니즘마다 1편, 반대 주장 쌍은 둘 다.

| 구분 | 폴더 번호 | 논문 | 정독 이유 |
|---|---|---|---|
| Shrink | 1 #1 | MaskedKD | 입력 토큰 축소. 사례 실험 대상 |
| Shrink | 1 #2 | DMAE | teacher 깊이 축소. MaskedKD와 메커니즘 다름 |
| Shrink | 1 #6 | Decoupled KD | 손실 오버헤드 쪽 대표 |
| Skip | 2 #1 | Distill on a Diet | 학습된 데이터 선택 |
| Skip | 2 #2 | Distilling the Knowledge in Data Pruning | 반대 주장: KD 쓰면 무작위 프루닝으로 충분 |
| Skip | 2 #3 | When Less Data Favors Smaller Teachers | 데이터 예산과 teacher 크기의 긴장 |
| Skip | 2 #6 | UNIX | teacher 호출 건너뛰기의 비전 대표 |
| Skip | 2 #9 | Weak-to-Strong KD | 학습 구간 축소 |
| Reuse | 3 #1 | FKD | 출력 캐싱 대표 |
| Reuse | 3 #4 | TinyViT | 일회성 비용 계산 사례 |
| Reuse | 3 #5 | Dataset Reinforcement | 저장물 배포로 커뮤니티 전체가 회수 |
| Reuse | 3 #8 | 지식 저장 증류 (랩실) | 검출 특징 저장 |
| Reuse | 3 #9 | Are Large-scale Soft Labels Necessary? | 저장물 압축 |
| Reuse | 3 #10 | TinyCLIP | 가중치 재사용 |
| Substitute | 4 #1 | KD from Model Checkpoints | teacher 학습 비용 대체 |
| Substitute | 4 #4 | Tf-KD | teacher 없음 |
| Substitute | 4 #8 | Proteus | 데이터 대체 |
| Substitute | 4 #9 | SRe2L | dataset distillation과의 경계 |
| Q2 측정 | 5 #1 | Patient and Consistent | 반대 주장: 길게 해야 한다 |
| Q2 측정 | 5 #2 | VanillaKD | 긴 스케줄 + 단순 KD |
| Q2 측정 | 5 #4 | Distillation Scaling Laws | teacher/student compute 배분 |
| Q2 측정 | 5 #6 | Are Coreset Methods Worth Their Cost? | 선택 비용까지 넣은 계산 |
| Q3 평가 | 6 #1 | Teacher's Pet | KD의 그룹별 효과 |
| Q3 평가 | 6 #3 | DeTT | 편향 데이터로 증류 |
| Q3 평가 | 6 #5 | When Masking Helps or Hurts | 마스킹과 강건성 |
| Q3 평가 | 6 #7 | DRoP | 데이터 축소와 그룹 강건성 |
| 선행 서베이 | PART 2 #6 | Efficient On-Policy Distillation | 가장 가까운 비용 서베이 |

나머지는 훑기 (초록 + 비용 표 하나).

## 1. Shrink: 한 스텝 비용을 줄임

| # | 논문 | 한 줄 | 링크 |
|---|---|---|---|
| 1 | MaskedKD (The Role of Masking for Efficient Supervised KD of ViTs) | teacher 입력 토큰을 student attention으로 골라 줄임 | [2302.10494](https://arxiv.org/abs/2302.10494) |
| 2 | DMAE (Masked Autoencoders Enable Efficient Knowledge Distillers) | 보이는 패치만 쓰고 teacher는 앞쪽 층만 돌림 | [2208.12256](https://arxiv.org/abs/2208.12256) |
| 3 | Dyna-DINO | 학습 초반에 teacher 추론을 중간 층에서 멈춤 | [2606.19483](https://arxiv.org/abs/2606.19483) |
| 4 | Asymmetric Masked Distillation (AMD) | student는 높은 마스킹, teacher는 낮은 마스킹 | [2311.03149](https://arxiv.org/abs/2311.03149) |
| 5 | MaskAlign | student가 보이는 패치만 인코딩 | [2211.08887](https://arxiv.org/abs/2211.08887) |
| 6 | Decoupled KD (DKD) | 싼 logit 손실로 feature KD 수준 | [2203.08679](https://arxiv.org/abs/2203.08679) |
| 7 | Information Theoretic Representation Distillation (ITRD) | 학습 오버헤드가 작은 손실 | [2112.00459](https://arxiv.org/abs/2112.00459) |
| 8 | Understanding the Role of the Projector in KD | projector 설계로 싼 레시피 | [2303.11098](https://arxiv.org/abs/2303.11098) |

배경 기법 (KD 아님): [EViT](https://arxiv.org/abs/2202.07800), [ToMe](https://arxiv.org/abs/2210.09461), [DynamicViT](https://arxiv.org/abs/2106.02034)

## 2. Skip: 샘플, teacher 호출, 학습 구간을 건너뜀

| # | 논문 | 한 줄 | 링크 |
|---|---|---|---|
| 1 | Distill on a Diet | KD용 학습 가능한 데이터 프루닝 | [2606.25488](https://arxiv.org/abs/2606.25488) |
| 2 | Distilling the Knowledge in Data Pruning | KD를 쓰면 무작위 프루닝으로도 충분 | [2403.07854](https://arxiv.org/abs/2403.07854) |
| 3 | When Less Data Favors Smaller Teachers | 데이터가 적으면 작은 teacher가 유리 | [2609.34489](https://arxiv.org/abs/2609.34489) |
| 4 | Few Sample KD (FSKD) | 1% 데이터로 블록 단위 맞춤 | [1812.01839](https://arxiv.org/abs/1812.01839) |
| 5 | PRACTISE | 적은 샘플로 블록 통째 제거 | [2202.07861](https://arxiv.org/abs/2202.07861) |
| 6 | UNIX (Computation-Efficient KD via Uncertainty-Aware Mixup) | 불확실한 샘플만 teacher에 질의 | [2012.09413](https://arxiv.org/abs/2012.09413) |
| 7 | Active Mixup | teacher에 보낼 이미지를 능동 선택 | [2003.13960](https://arxiv.org/abs/2003.13960) |
| 8 | Comparative KD | teacher 추가 호출 없이 신호 늘림 | [2311.02253](https://arxiv.org/abs/2311.02253) |
| 9 | Weak-to-Strong KD Accelerates Visual Learning | 약한 teacher로 초반만 증류하고 끔 | [2604.15451](https://arxiv.org/abs/2604.15451) |
| 10 | DearKD | 초반에만 증류, 이후엔 증류 없이 학습 | [2204.12997](https://arxiv.org/abs/2204.12997) |

## 3. Reuse: 한 번 만들고 재사용

| # | 논문 | 한 줄 | 링크 |
|---|---|---|---|
| 1 | FKD (A Fast KD Framework for Visual Recognition) | crop별 soft label을 미리 저장 | [2112.01528](https://arxiv.org/abs/2112.01528) |
| 2 | ReLabel (Re-labeling ImageNet) | 라벨 맵을 1회 생성해 저장 | [2101.05022](https://arxiv.org/abs/2101.05022) |
| 3 | FerKD | FKD 후속, 저장 라벨 보정 | [2312.17473](https://arxiv.org/abs/2312.17473) |
| 4 | TinyViT | teacher 로짓을 희소화해 디스크 저장 | [2207.10666](https://arxiv.org/abs/2207.10666) |
| 5 | Dataset Reinforcement (Reinforce Data, Multiply Impact) | teacher 출력을 데이터셋에 넣어 배포 | [2303.08983](https://arxiv.org/abs/2303.08983) |
| 6 | MobileCLIP | CLIP 앙상블 임베딩과 캡션을 저장 | [2311.17049](https://arxiv.org/abs/2311.17049) |
| 7 | CLIP-Embed-KD | teacher forward 대신 저장 임베딩 사용 | [2404.06170](https://arxiv.org/abs/2404.06170) |
| 8 | 지식 저장 증류 (랩실, 검출) | NMS로 고른 teacher 특징만 저장 | [DBpia](https://www.dbpia.co.kr/journal/articleDetail?nodeId=NODE12055100) |
| 9 | Are Large-scale Soft Labels Necessary? (LPLD) | 저장한 soft label을 프루닝 | [2410.15919](https://arxiv.org/abs/2410.15919) |
| 10 | TinyCLIP | teacher 가중치를 물려받아 시작 | [2309.12314](https://arxiv.org/abs/2309.12314) |
| 11 | Initializing Models with Larger Ones (Weight Selection) | 큰 모델 가중치 일부로 초기화 | [2311.18823](https://arxiv.org/abs/2311.18823) |
| 12 | SimKD (KD with the Reused Teacher Classifier) | teacher 분류기를 그대로 재사용 | [2203.14001](https://arxiv.org/abs/2203.14001) |
| 13 | MobileSAM | 인코더만 증류, 디코더는 재사용 | [2306.14289](https://arxiv.org/abs/2306.14289) |
| 14 | Tailored Coordinate System (TCS) | teacher를 한 번만 통과시켜 부분공간 추출 | [2412.09388](https://arxiv.org/abs/2412.09388) |

## 4. Substitute: teacher나 데이터를 싼 대역으로

| # | 논문 | 한 줄 | 링크 |
|---|---|---|---|
| 1 | Efficient KD from Model Checkpoints | 중간 체크포인트를 teacher로 | [2210.06458](https://arxiv.org/abs/2210.06458) |
| 2 | On the Efficacy of KD | 조기 종료한 teacher가 더 나음 | [1910.01348](https://arxiv.org/abs/1910.01348) |
| 3 | Learning from a Lightweight Teacher for Efficient KD | 가벼운 teacher로 학습 시간 단축 | [2005.09163](https://arxiv.org/abs/2005.09163) |
| 4 | Tf-KD (Revisiting KD via Label Smoothing Regularization) | teacher 없이 자기 자신·수제 분포로 | [1909.11723](https://arxiv.org/abs/1909.11723) |
| 5 | Be Your Own Teacher | 깊은 층이 얕은 층을 가르침 | [1905.08094](https://arxiv.org/abs/1905.08094) |
| 6 | Deep Mutual Learning | 사전학습 teacher 없이 서로 가르침 | [1706.00384](https://arxiv.org/abs/1706.00384) |
| 7 | Snapshot Distillation | 이전 주기 스냅샷이 teacher | [1812.00123](https://arxiv.org/abs/1812.00123) |
| 8 | Proteus (Accessing Vision Foundation Models via ImageNet-1K) | 1.42억 장 대신 ImageNet 120만 장으로 증류 | [2407.10366](https://arxiv.org/abs/2407.10366) |
| 9 | SRe2L (Squeeze, Recover and Relabel) | 합성 데이터 + teacher 재라벨 | [2306.13092](https://arxiv.org/abs/2306.13092) |
| 10 | RDED | 합성 최적화 없이 빠른 데이터 증류 | [2312.03526](https://arxiv.org/abs/2312.03526) |
| 11 | Up to 100x Faster Data-free KD (FastDFKD) | 합성기를 메타 학습해 빠르게 | [2112.06253](https://arxiv.org/abs/2112.06253) |
| 12 | NAYER | data-free 합성 5~15배 가속 | [2310.00258](https://arxiv.org/abs/2310.00258) |

## 5. 측정 방식 (Q2) 참고

| #   | 논문                                              | 한 줄                            | 링크                                             |
| --- | ----------------------------------------------- | ------------------------------ | ---------------------------------------------- |
| 1   | A Good Teacher is Patient and Consistent      | 긴 스케줄이 필요하다는 반례                | [2106.05237](https://arxiv.org/abs/2106.05237) |
| 2   | VanillaKD                                       | 강한 레시피면 단순 KD로 충분              | [2305.15781](https://arxiv.org/abs/2305.15781) |
| 3   | DeiT                                            | 비전 KD 기준 설정                    | [2012.12877](https://arxiv.org/abs/2012.12877) |
| 4   | Distillation Scaling Laws                       | teacher와 student 사이 compute 배분 | [2502.08606](https://arxiv.org/abs/2502.08606) |
| 5   | Is KD Actually Greener?                         | teacher 포함 전 과정 에너지 계산         | [2602.09691](https://arxiv.org/abs/2602.09691) |
| 6   | Are Coreset Selection Methods Worth Their Cost? | 선택 비용까지 넣으면 이득이 사라짐            | [2609.22894](https://arxiv.org/abs/2609.22894) |
| 7   | Canonical KD for Semantic Segmentation          | 같은 실제 시간에서는 단순 KD가 이김          | [2604.25530](https://arxiv.org/abs/2604.25530) |

## 6. 평가 방식 (Q3) 참고

| # | 논문 | 한 줄 | 링크 |
|---|---|---|---|
| 1 | Teacher's Pet | KD 이득이 그룹마다 다르고 일부는 손해 | [2106.10494](https://arxiv.org/abs/2106.10494) |
| 2 | DeDiER (Using Early Readouts to Mediate Featural Bias) | student가 spurious 특징에 더 의존 | [2310.18590](https://arxiv.org/abs/2310.18590) |
| 3 | Debiased Distillation by Transplanting the Last Layer (DeTT) | 편향 데이터로 증류하면 student도 편향 | [2302.11187](https://arxiv.org/abs/2302.11187) |
| 4 | What is Left After Distillation? | KD가 클래스별 성능을 바꿈 | [2410.08407](https://arxiv.org/abs/2410.08407) |
| 5 | When Masking Helps or Hurts Robustness in Compressed CLIP | 토큰 마스킹의 WGA 효과가 데이터셋마다 다름 | [2609.39704](https://arxiv.org/abs/2609.39704) |
| 6 | Later Is Better: Token Reduction under Distribution Shift | 토큰 제거율이 높을수록 분포 이동에 취약 | [2610.07758](https://arxiv.org/abs/2610.07758) |
| 7 | DRoP: Distributionally Robust Data Pruning | 일반 프루닝은 클래스 편향을 키움 | [2404.05579](https://arxiv.org/abs/2404.05579) |
| 8 | Impact of Coreset Selection on Spurious Correlations | coreset 선택과 그룹 강건성 | [2507.11690](https://arxiv.org/abs/2507.11690) |
| 9 | Group DRO (Waterbirds) | WGA 평가의 기준 논문 | [1911.08731](https://arxiv.org/abs/1911.08731) |
| 10 | Last Layer Re-Training is Sufficient (DFR) | 마지막 층 재학습으로 진단 | [2204.02937](https://arxiv.org/abs/2204.02937) |
| 11 | Group Robustness is in the Details | WGA가 학습 설정에 민감 | [2407.13957](https://arxiv.org/abs/2407.13957) |
