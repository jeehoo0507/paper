# 논문 목록 (분모의 기준)

이 파일이 "N편 중 M편"의 N이다. 논문을 넣거나 빼면 여기부터 고친다.
PDF: [`../Related Work/Efficient KD papers/`](<../Related Work/Efficient KD papers>) · 읽기 메모: [`../notes/`](../notes)

- **갈래**는 AI 초안이다. 담당자가 확정하고 확인 칸에 이름을 적는다.
- **표시**가 있는 논문은 축이나 범위가 애매하다. 담당자가 결정하고 `decisions.md`에 이유를 적는다.
- 할 일: 4절 = 방법 표 한 줄, 5절 = 체크표 한 줄. 측정·평가 논문은 방법이 아니므로 5절 체크표만 채운다.

## Shrink (8편) · Shrink 담당

**축의 기준**: teacher 순전파 1회의 연산이 줄어드는가. logit 증류인지 특징 증류인지는 따지지 않는다. 폴더 README의 "한 스텝 비용을 줄임"보다 좁은 정의다. 이 기준이면 메모상 01–04번만 해당하고 05–08번은 해당하지 않는다. 담당자가 원문으로 확정한다.

| 번호 | 논문 | 한 줄 | 링크 | 갈래 (초안) | 표시 | 4절 | 5절 | 확인 |
|---|---|---|---|---|---|---|---|---|
| S-01 | MaskedKD (The Role of Masking for Efficient Supervised KD of ViTs) | teacher 입력 토큰을 student attention으로 골라 줄임 | [arXiv: 2302.10494](https://arxiv.org/abs/2302.10494) | teacher 입력 토큰 |  | ☐ | ☐ | |
| S-02 | DMAE (Masked Autoencoders Enable Efficient Knowledge Distillers) | 보이는 패치만 쓰고 teacher는 앞쪽 층만 돌림 | [arXiv: 2208.12256](https://arxiv.org/abs/2208.12256) | 토큰 + teacher 깊이 | 메모 기준 teacher가 보이는 패치만, 정렬 층까지만 돎. 특징 증류(사전학습)라는 점을 표에 적을 것 | ☐ | ☐ | |
| S-03 | Dyna-DINO | 학습 초반에 teacher 추론을 중간 층에서 멈춤 | [arXiv: 2606.19483](https://arxiv.org/abs/2606.19483) | teacher 깊이 (조기 종료) |  | ☐ | ☐ | |
| S-04 | Asymmetric Masked Distillation (AMD) | student는 높은 마스킹, teacher는 낮은 마스킹 | [arXiv: 2311.03149](https://arxiv.org/abs/2311.03149) | 마스킹 비율 |  | ☐ | ☐ | |
| S-05 | MaskAlign | student가 보이는 패치만 인코딩 | [arXiv: 2211.08887](https://arxiv.org/abs/2211.08887) | 마스킹 (student 쪽) | 메모 기준 teacher는 전체 이미지를 봄. teacher 연산을 줄이지 않음. 축에 둘지 미정 | ☐ | ☐ | |
| S-06 | Decoupled KD (DKD) | 싼 logit 손실로 feature KD 수준 | [arXiv: 2203.08679](https://arxiv.org/abs/2203.08679) | 손실 설계 | teacher 연산을 줄이지 않음. 축에 둘지 미정 | ☐ | ☐ | |
| S-07 | Information Theoretic Representation Distillation (ITRD) | 학습 오버헤드가 작은 손실 | [arXiv: 2112.00459](https://arxiv.org/abs/2112.00459) | 손실 설계 | teacher 연산을 줄이지 않음. 축에 둘지 미정 | ☐ | ☐ | |
| S-08 | Understanding the Role of the Projector in KD | projector 설계로 싼 레시피 | [arXiv: 2303.11098](https://arxiv.org/abs/2303.11098) | 부가 모듈 | teacher 연산을 줄이지 않음. 축에 둘지 미정 | ☐ | ☐ | |

## Skip (10편) · Skip 담당

| 번호 | 논문 | 한 줄 | 링크 | 갈래 (초안) | 표시 | 4절 | 5절 | 확인 |
|---|---|---|---|---|---|---|---|---|
| K-01 | Distill on a Diet | KD용 학습 가능한 데이터 프루닝 | [arXiv: 2606.25488](https://arxiv.org/abs/2606.25488) | 데이터 |  | ☐ | ☐ | |
| K-02 | Distilling the Knowledge in Data Pruning | KD를 쓰면 무작위 프루닝으로도 충분 | [arXiv: 2403.07854](https://arxiv.org/abs/2403.07854) | 데이터 |  | ☐ | ☐ | |
| K-03 | When Less Data Favors Smaller Teachers | 데이터가 적으면 작은 teacher가 유리 | [arXiv: 2609.34489](https://arxiv.org/abs/2609.34489) | 데이터 | Substitute(작은 teacher)와 걸침 | ☐ | ☐ | |
| K-04 | Few Sample KD (FSKD) | 1% 데이터로 블록 단위 맞춤 | [arXiv: 1812.01839](https://arxiv.org/abs/1812.01839) | 데이터 (소량 샘플) | 모델 압축용. 범위 확인 | ☐ | ☐ | |
| K-05 | PRACTISE | 적은 샘플로 블록 통째 제거 | [arXiv: 2202.07861](https://arxiv.org/abs/2202.07861) | 데이터 (소량 샘플) | 모델 압축용. 범위 확인 | ☐ | ☐ | |
| K-06 | UNIX (Computation-Efficient KD via Uncertainty-Aware Mixup) | 불확실한 샘플만 teacher에 질의 | [arXiv: 2012.09413](https://arxiv.org/abs/2012.09413) | 샘플 (teacher 질의) |  | ☐ | ☐ | |
| K-07 | Active Mixup | teacher에 보낼 이미지를 능동 선택 | [arXiv: 2003.13960](https://arxiv.org/abs/2003.13960) | 샘플 (teacher 질의) |  | ☐ | ☐ | |
| K-08 | Comparative KD | teacher 추가 호출 없이 신호 늘림 | [arXiv: 2311.02253](https://arxiv.org/abs/2311.02253) | 샘플 (teacher 질의) | teacher 호출을 줄이는지 확인 | ☐ | ☐ | |
| K-09 | Weak-to-Strong KD Accelerates Visual Learning | 약한 teacher로 초반만 증류하고 끔 | [arXiv: 2604.15451](https://arxiv.org/abs/2604.15451) | 학습 구간 |  | ☐ | ☐ | |
| K-10 | DearKD | 초반에만 증류, 이후엔 증류 없이 학습 | [arXiv: 2204.12997](https://arxiv.org/abs/2204.12997) | 학습 구간 |  | ☐ | ☐ | |

## Reuse (14편) · Reuse·Substitute 담당

| 번호 | 논문 | 한 줄 | 링크 | 갈래 (초안) | 표시 | 4절 | 5절 | 확인 |
|---|---|---|---|---|---|---|---|---|
| R-01 | FKD (A Fast KD Framework for Visual Recognition) | crop별 soft label을 미리 저장 | [arXiv: 2112.01528](https://arxiv.org/abs/2112.01528) | 출력 저장 |  | ☐ | ☐ | |
| R-02 | ReLabel (Re-labeling ImageNet) | 라벨 맵을 1회 생성해 저장 | [arXiv: 2101.05022](https://arxiv.org/abs/2101.05022) | 출력 저장 |  | ☐ | ☐ | |
| R-03 | FerKD | FKD 후속, 저장 라벨 보정 | [arXiv: 2312.17473](https://arxiv.org/abs/2312.17473) | 출력 저장 |  | ☐ | ☐ | |
| R-04 | TinyViT | teacher 로짓을 희소화해 디스크 저장 | [arXiv: 2207.10666](https://arxiv.org/abs/2207.10666) | 출력 저장 |  | ☐ | ☐ | |
| R-05 | Dataset Reinforcement (Reinforce Data, Multiply Impact) | teacher 출력을 데이터셋에 넣어 배포 | [arXiv: 2303.08983](https://arxiv.org/abs/2303.08983) | 출력 저장 |  | ☐ | ☐ | |
| R-06 | MobileCLIP | CLIP 앙상블 임베딩과 캡션을 저장 | [arXiv: 2311.17049](https://arxiv.org/abs/2311.17049) | 출력 저장 | 분류 밖 (CLIP) | ☐ | ☐ | |
| R-07 | CLIP-Embed-KD | teacher forward 대신 저장 임베딩 사용 | [arXiv: 2404.06170](https://arxiv.org/abs/2404.06170) | 출력 저장 | 분류 밖 (CLIP) | ☐ | ☐ | |
| R-08 | 지식 저장 증류 (랩실, 검출) | NMS로 고른 teacher 특징만 저장 | [DBpia: DBpia](https://www.dbpia.co.kr/journal/articleDetail?nodeId=NODE12055100) | 출력 저장 | 랩실 국문 논문, 검출 | ☐ | ☐ | |
| R-09 | Are Large-scale Soft Labels Necessary? (LPLD) | 저장한 soft label을 프루닝 | [arXiv: 2410.15919](https://arxiv.org/abs/2410.15919) | 출력 저장 |  | ☐ | ☐ | |
| R-10 | TinyCLIP | teacher 가중치를 물려받아 시작 | [arXiv: 2309.12314](https://arxiv.org/abs/2309.12314) | 가중치·모듈 재사용 | 출력이 아니라 가중치 재사용 | ☐ | ☐ | |
| R-11 | Initializing Models with Larger Ones (Weight Selection) | 큰 모델 가중치 일부로 초기화 | [arXiv: 2311.18823](https://arxiv.org/abs/2311.18823) | 가중치·모듈 재사용 | 출력이 아니라 가중치 재사용 | ☐ | ☐ | |
| R-12 | SimKD (KD with the Reused Teacher Classifier) | teacher 분류기를 그대로 재사용 | [arXiv: 2203.14001](https://arxiv.org/abs/2203.14001) | 가중치·모듈 재사용 | 출력이 아니라 분류기 재사용 | ☐ | ☐ | |
| R-13 | MobileSAM | 인코더만 증류, 디코더는 재사용 | [arXiv: 2306.14289](https://arxiv.org/abs/2306.14289) | 가중치·모듈 재사용 | 분류 밖 (SAM) | ☐ | ☐ | |
| R-14 | Tailored Coordinate System (TCS) | teacher를 한 번만 통과시켜 부분공간 추출 | [arXiv: 2412.09388](https://arxiv.org/abs/2412.09388) | teacher 1회 통과 |  | ☐ | ☐ | |

## Substitute (12편) · Reuse·Substitute 담당

| 번호 | 논문 | 한 줄 | 링크 | 갈래 (초안) | 표시 | 4절 | 5절 | 확인 |
|---|---|---|---|---|---|---|---|---|
| U-01 | Efficient KD from Model Checkpoints | 중간 체크포인트를 teacher로 | [arXiv: 2210.06458](https://arxiv.org/abs/2210.06458) | 약한 teacher |  | ☐ | ☐ | |
| U-02 | On the Efficacy of KD | 조기 종료한 teacher가 더 나음 | [arXiv: 1910.01348](https://arxiv.org/abs/1910.01348) | 약한 teacher | teacher 용량·조기 종료 실험. 우리 Skip(구간) 방법의 출처이기도 함 | ☐ | ☐ | |
| U-03 | Learning from a Lightweight Teacher for Efficient KD | 가벼운 teacher로 학습 시간 단축 | [arXiv: 2005.09163](https://arxiv.org/abs/2005.09163) | 약한 teacher |  | ☐ | ☐ | |
| U-04 | Tf-KD (Revisiting KD via Label Smoothing Regularization) | teacher 없이 자기 자신·수제 분포로 | [arXiv: 1909.11723](https://arxiv.org/abs/1909.11723) | teacher 없음 |  | ☐ | ☐ | |
| U-05 | Be Your Own Teacher | 깊은 층이 얕은 층을 가르침 | [arXiv: 1905.08094](https://arxiv.org/abs/1905.08094) | teacher 없음 |  | ☐ | ☐ | |
| U-06 | Deep Mutual Learning | 사전학습 teacher 없이 서로 가르침 | [arXiv: 1706.00384](https://arxiv.org/abs/1706.00384) | teacher 없음 |  | ☐ | ☐ | |
| U-07 | Snapshot Distillation | 이전 주기 스냅샷이 teacher | [arXiv: 1812.00123](https://arxiv.org/abs/1812.00123) | teacher 없음 |  | ☐ | ☐ | |
| U-08 | Proteus (Accessing Vision Foundation Models via ImageNet-1K) | 1.42억 장 대신 ImageNet 120만 장으로 증류 | [arXiv: 2407.10366](https://arxiv.org/abs/2407.10366) | 데이터 대체 | 분류 밖 과제 평가 포함 | ☐ | ☐ | |
| U-09 | SRe2L (Squeeze, Recover and Relabel) | 합성 데이터 + teacher 재라벨 | [arXiv: 2306.13092](https://arxiv.org/abs/2306.13092) | 데이터 대체 |  | ☐ | ☐ | |
| U-10 | RDED | 합성 최적화 없이 빠른 데이터 증류 | [arXiv: 2312.03526](https://arxiv.org/abs/2312.03526) | 데이터 대체 |  | ☐ | ☐ | |
| U-11 | Up to 100x Faster Data-free KD (FastDFKD) | 합성기를 메타 학습해 빠르게 | [arXiv: 2112.06253](https://arxiv.org/abs/2112.06253) | 데이터 대체 |  | ☐ | ☐ | |
| U-12 | NAYER | data-free 합성 5~15배 가속 | [arXiv: 2310.00258](https://arxiv.org/abs/2310.00258) | 데이터 대체 |  | ☐ | ☐ | |

## 측정 (7편) · 2절 담당

| 번호 | 논문 | 한 줄 | 링크 | 표시 | 5절 | 확인 |
|---|---|---|---|---|---|---|
| M-01 | A Good Teacher is Patient and Consistent | 긴 스케줄이 필요하다는 반례 | [arXiv: 2106.05237](https://arxiv.org/abs/2106.05237) |  | ☐ | |
| M-02 | VanillaKD | 강한 레시피면 단순 KD로 충분 | [arXiv: 2305.15781](https://arxiv.org/abs/2305.15781) |  | ☐ | |
| M-03 | DeiT | 비전 KD 기준 설정 | [arXiv: 2012.12877](https://arxiv.org/abs/2012.12877) |  | ☐ | |
| M-04 | Distillation Scaling Laws | teacher와 student 사이 compute 배분 | [arXiv: 2502.08606](https://arxiv.org/abs/2502.08606) |  | ☐ | |
| M-05 | Is KD Actually Greener? | teacher 포함 전 과정 에너지 계산 | [arXiv: 2602.09691](https://arxiv.org/abs/2602.09691) |  | ☐ | |
| M-06 | Are Coreset Selection Methods Worth Their Cost? | 선택 비용까지 넣으면 이득이 사라짐 | [arXiv: 2609.22894](https://arxiv.org/abs/2609.22894) |  | ☐ | |
| M-07 | Canonical KD for Semantic Segmentation | 같은 실제 시간에서는 단순 KD가 이김 | [arXiv: 2604.25530](https://arxiv.org/abs/2604.25530) |  | ☐ | |

## 평가 (11편) · 2절 담당

| 번호 | 논문 | 한 줄 | 링크 | 표시 | 5절 | 확인 |
|---|---|---|---|---|---|---|
| E-01 | Teacher's Pet | KD 이득이 그룹마다 다르고 일부는 손해 | [arXiv: 2106.10494](https://arxiv.org/abs/2106.10494) |  | ☐ | |
| E-02 | DeDiER (Using Early Readouts to Mediate Featural Bias) | student가 spurious 특징에 더 의존 | [arXiv: 2310.18590](https://arxiv.org/abs/2310.18590) |  | ☐ | |
| E-03 | Debiased Distillation by Transplanting the Last Layer (DeTT) | 편향 데이터로 증류하면 student도 편향 | [arXiv: 2302.11187](https://arxiv.org/abs/2302.11187) |  | ☐ | |
| E-04 | What is Left After Distillation? | KD가 클래스별 성능을 바꿈 | [arXiv: 2410.08407](https://arxiv.org/abs/2410.08407) |  | ☐ | |
| E-05 | When Masking Helps or Hurts Robustness in Compressed CLIP | 토큰 마스킹의 WGA 효과가 데이터셋마다 다름 | [arXiv: 2609.39704](https://arxiv.org/abs/2609.39704) | 추론 시 토큰 줄이기와 최저 그룹. 증류 아님. 주장 A와 가장 가까움 | ☐ | |
| E-06 | Later Is Better: Token Reduction under Distribution Shift | 토큰 제거율이 높을수록 분포 이동에 취약 | [arXiv: 2610.07758](https://arxiv.org/abs/2610.07758) | 토큰 줄이기와 분포 이동. 주장 A와 가까움 | ☐ | |
| E-07 | DRoP: Distributionally Robust Data Pruning | 일반 프루닝은 클래스 편향을 키움 | [arXiv: 2404.05579](https://arxiv.org/abs/2404.05579) | 데이터 프루닝과 클래스 편향. 주장 A와 가까움 | ☐ | |
| E-08 | Impact of Coreset Selection on Spurious Correlations | coreset 선택과 그룹 강건성 | [arXiv: 2507.11690](https://arxiv.org/abs/2507.11690) | coreset과 그룹 강건성. 주장 A와 가까움 | ☐ | |
| E-09 | Group DRO (Waterbirds) | WGA 평가의 기준 논문 | [arXiv: 1911.08731](https://arxiv.org/abs/1911.08731) |  | ☐ | |
| E-10 | Last Layer Re-Training is Sufficient (DFR) | 마지막 층 재학습으로 진단 | [arXiv: 2204.02937](https://arxiv.org/abs/2204.02937) |  | ☐ | |
| E-11 | Group Robustness is in the Details | WGA가 학습 설정에 민감 | [arXiv: 2407.13957](https://arxiv.org/abs/2407.13957) |  | ☐ | |

합계 62편.

## 넣을지 정해야 하는 논문

| 논문 | 링크 | 어디에 | 왜 미정인가 |
|---|---|---|---|
| PESF-KD | 읽기 메모 1_Shrink 09번 참고 | Shrink | 메모에는 있고 폴더와 목록에는 없음. teacher 연산을 줄이지 않음 |
| DIST | 읽기 메모 1_Shrink 10번 참고 | Shrink | 같음 |
| ShiftKD | [arXiv: 2312.16242](https://arxiv.org/abs/2312.16242) | 평가, 2절 | KD 방법 30여 개를 분포 이동에서 평가. 최저 그룹과 ECE를 봄 |
| Knowledge Distillation Must Account for What It Loses | [arXiv: 2604.25110](https://arxiv.org/abs/2604.25110) | 2절 | 입장 논문. 제목과 논지가 우리와 가까움 |
| End-to-End Energy Accounting of Distillation Pipelines | [arXiv: 2605.13981](https://arxiv.org/abs/2605.13981) | 측정, 2절 | 언어 모델의 단계별 비용 회계와 재사용 본전 지점 |
