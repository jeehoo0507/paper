# 실험 계획

> **2026-11-19 알림**: 이 계획은 이전 판이다. ImageNet-100은 뺐고, 지금은 Waterbirds, CelebA, CUB-LT에서 기준선, Shrink, Skip(샘플)을 먼저 돌리고 있다. 새 계획이 정리되면 이 파일을 바꾼다.

**상태 (2026-11-18)**: 설계는 확정. Shrink 축은 기존 결과가 있고, 나머지는 미실행. 온도·epoch 같은 세부 값은 1일 차 측정 후 정함.

논문의 방향과 목차: [`../our paper.md`](<../our paper.md>)

## 목표

효율 KD 방법이 아끼는 비용과 잃는 성능을 같은 조건에서 재고, 예산을 줄일 때 각 지표가 꺾이는 지점을 찾는다.

## 원칙

1. **격자 하나**: 방법 × 예산 단계 × 데이터셋 × seed. 실행 목록, 학습 코드, 로그 형식을 하나로 둔다.
2. **KD > CE 확인 먼저**: 데이터셋마다 CE와 Full KD를 먼저 돌린다. Full KD가 CE보다 낮으면 그 데이터셋은 쓰지 않는다.
3. **잡음 기준**: seed 표준편차의 2배보다 작은 차이는 주장하지 않는다.
4. **같은 조건**: 모든 방법이 같은 epoch과 같은 튜닝 규칙을 쓴다. 비용은 계산값이 아니라 측정값으로 적는다.
5. **결론을 미리 정하지 않는다**: 차이가 안 나오면 그것을 결과로 쓴다.

## 두 가지 상황

| | A. 사전학습 student 미세조정 | B. 처음부터 학습 |
|---|---|---|
| 데이터셋 | Waterbirds, CUB-200. 확인 통과 시 Flowers-102, Oxford Pets, Food101 | ImageNet-100, 224px |
| teacher → student | DeiT-B (데이터셋에 미세조정) → DeiT-Ti (IN-1k 사전학습) | DeiT-S (IN-1k 공개 가중치, logit 100개만 사용) → DeiT-Ti (처음부터) |
| 손실 | (1−α)·CE + α·KD, α=0.5, T=1 | CE + KD. 온도는 확인 단계에서 T=1과 T=4 비교 후 고정 |
| 증강 | DeiT 레시피 (mixup, cutmix, RandAugment) | random resized crop + flip, teacher와 student 같은 뷰 |
| 학습 | AdamW, weight decay 0.05, batch 128, 100 epoch, lr은 3개 값 중 선택 | AdamW, lr 5e-4, weight decay 0.05, cosine. epoch은 처리량 측정 후 확정 (기본 100) |
| seed | 3 | 1 |
| 코드 | 기존 harness ([Loop-Research-for-MaskedKD](https://github.com/jeehoo0507/Loop-Research-for-MaskedKD)) | 같은 레포의 `quick/` 스크립트를 바탕으로 추가 |
| 상태 | Shrink 축 결과 있음 | 미실행 |

**소규모 데이터셋을 처음부터 학습하지 않는 이유**: 지난 실험에서 DeiT-Ti를 CIFAR-100에서 처음부터 학습하면 약 24%에 그쳤다 (harness 설정 파일의 기록). 문헌의 큰 KD 이득(Flowers 66 → 93 등)은 ResNet-50을 수천 epoch 학습한 조건이다. 그래서 소규모는 상황 A 방식으로 돌리고, 확인을 통과한 것만 쓴다.

**평가 전용** (학습 없음): ImageNet-V2 (100 클래스), ImageNet-100 손상 버전, ImageNet-100 student의 Flowers/Pets 선형 평가.

## 방법과 예산 손잡이

상황 A에서는 손잡이를 단계별로 돌려 곡선을 만든다. 상황 B에서는 방법당 기준점 하나만 돌린다.

| 축 | 방법 | 손잡이 (A) | 기준점 (B) |
|---|---|---|---|
| 기준 | CE / Full KD / Full KD 절반 epoch | - | 같음 |
| Shrink | MaskedKD: student attention으로 teacher 토큰 선택 | 유지 토큰 50 / 25 / 15 / 6% (결과 있음) | 50% |
| Skip (데이터) | 무작위 부분집합으로 KD | 데이터 75 / 50 / 25% | 50% |
| Skip (구간) | 앞 구간만 KD, 이후 CE | KD epoch 50 / 30 / 10% | 30% |
| Reuse | teacher logit을 저장해 재사용 | 저장하는 증강 뷰 수 1 / 4 / 16 | 1 |
| Substitute | 약한 teacher | teacher 미세조정 10 / 30 / 100% 체크포인트 | 작은 teacher (DeiT-Ti 공개 가중치) |

## 측정

**비용 (모든 실행)**: 학습 1스텝 GFLOPs/img (teacher, student, 선택 연산 따로), 처리량, 실제 학습 시간, 최대 GPU 메모리, 일회성 시간 (teacher 미세조정, logit 저장), 저장 용량.

**성능**

| # | 지표 | 데이터셋 | 재는 법 |
|---|---|---|---|
| 1 | 평균 정확도 | 전부 | |
| 2 | WGA, 그룹별 정확도 | Waterbirds | 4개 그룹 중 최저. 체크포인트 선택에 그룹 라벨 안 씀 |
| 3 | 최악 클래스 | ImageNet-100, CUB | 하위 10개 클래스 평균 |
| 4 | 분포 이동 | ImageNet-V2 (100 클래스) | 원래 정확도와의 차이 |
| 5 | 손상 강건성 | ImageNet-100 손상 버전 | 손상 15종 × 강도 5단계, 한 번 만들어 고정 |
| 6 | 전이 | Flowers, Pets | student 특징 고정, 선형 분류기만 학습 |
| 7 | teacher 일치율, ECE | 전부 | top-1 일치 비율, 확신도 15구간 |
| 8 | 학습 곡선 | 전부 | Full KD 최종 정확도의 95%에 닿는 시간 |

**증거로 낼 것**: 방법 × 지표 표 (Full KD 대비 변화), 순위 상관 (Kendall τ), 축별 예산–지표 곡선.

## 기존 결과 (재사용)

출처: 위 레포의 `RESULTS_TABLE.md`. DeiT-B → DeiT-Ti, 100 epoch, 3 seed, val split.

**Waterbirds**

| 방법 | teacher 토큰 | 평균 정확도 | WGA | 1스텝 GFLOPs/img |
|---|---|---|---|---|
| CE | - | 89.80 ± 0.56 | 68.42 ± 0.75 | 7.64 |
| Full KD | 196 (100%) | 91.16 ± 0.52 | 67.42 ± 0.43 | 42.77 |
| MaskedKD | 98 (50%) | 90.46 ± 0.39 | 64.16 ± 2.42 | 25.04 |
| MaskedKD | 29 (15%) | 87.74 ± 0.55 | 38.59 ± 1.74 | 12.99 |
| MaskedKD | 12 (6%) | 84.21 ± 0.05 | 22.31 ± 1.15 | 10.07 |
| Rollout으로 선택 | 29 (15%) | 89.57 ± 0.65 | 62.16 ± 2.42 | 12.99 |
| Rollout으로 선택 | 12 (6%) | 88.10 ± 1.17 | 50.88 ± 1.74 | 10.08 |

- KD > CE 확인: 평균 정확도 +1.36%p로 통과. WGA는 Full KD가 CE보다 높지 않음.
- 잡음 기준: 평균 정확도 σ 0.44 (2σ = 0.88), WGA σ 2.78 (2σ = 5.56).

**CUB-200**

| 방법 | teacher 토큰 | 평균 정확도 | 1스텝 GFLOPs/img |
|---|---|---|---|
| CE | - | 81.22 ± 0.54 | 7.64 |
| Full KD | 196 (100%) | 81.72 ± 1.64 | 42.77 |
| MaskedKD | 49 (25%) | 81.67 ± 1.17 | 16.44 |
| MaskedKD | 29 (15%) | 80.22 ± 1.08 | 12.99 |
| MaskedKD | 12 (6%) | 78.28 ± 0.48 | 10.07 |

- 5 seed 확인 실행에서는 Full KD 82.17 대 CE 80.80으로 통과했지만, 위 3 seed에서는 차이가 +0.50%p로 잡음(σ 0.96) 안이다. CUB는 보조로만 쓴다.

## 실행 순서

GPU는 A5000 1장을 가정한다. 시간은 기존 harness에 기록된 처리량(실행당 초당 200~650장)으로 어림한 값이다.

| 순서 | 내용 | 실행 수 | 어림 시간 |
|---|---|---|---|
| 1 | B 확인: ImageNet-100에서 CE와 Full KD (T=1, T=4)를 20~30 epoch, 처리량 측정 | 3 | 반나절 |
| 2 | A 신규: Waterbirds, CUB × Skip 6단계, Reuse 3단계, Substitute 2단계 × 3 seed | 약 66 | 약 35시간 |
| 3 | B 기준점 8개 | 8 | 100 epoch 기준 45~145시간 |
| 4 | 평가 전용 (V2, 손상, 선형 평가)과 집계 | - | 수 시간 |
| 5 | 선택: Flowers, Pets, Food101을 A 방식으로 확인 후 추가 | 미정 | 시간이 남으면 |

- ImageNet-100이 일정의 대부분을 차지한다. 처리량이 낮으면 epoch을 모든 방법 동일하게 줄이고, 그래도 모자라면 기준점을 줄인다 (Full KD 절반 epoch → Skip 구간 → Substitute 순).
- 300 epoch이면 기준점 8개만으로 가장 빠른 경우에도 약 135시간이라, 다른 실행과 함께 일주일에 넣기 어렵다.

## 관리 방식

- 학습은 작업 큐로 돌리고 접속이 끊겨도 계속된다. epoch마다 체크포인트를 저장해 이어서 학습한다.
- Claude는 관리만 한다: 제출, 상태 확인, 죽은 실행 재시작, 결과 집계. 학습 코드와 실행 목록은 해시로 잠그고 바꾸지 않는다.
- 같은 실행이 두 번 실패하면 재시도를 멈추고 기록을 남긴다.
- 확인 단계에서 Full KD가 CE보다 낮으면 그 데이터셋의 나머지 실행을 멈춘다.

## 데이터셋 선정 근거 (KD > CE를 보고한 논문)

| 데이터셋 | 논문 | 보고 (CE → KD) | 조건 |
|---|---|---|---|
| ImageNet-100 | [MaskedKD](https://arxiv.org/abs/2302.10494) | 72.0 → 75.0 | ImageNet-1k, DeiT-S → DeiT-Ti. 100 클래스 직접 보고는 없음 |
| Waterbirds | [DEDIER](https://arxiv.org/abs/2310.18590) | WGA 33.8 → 84.9 | GroupDRO teacher, R50 → R18 |
| Waterbirds | [AGRE-KD](https://arxiv.org/abs/2411.14984) | WGA 54.1 → 39.6~55.0 | 일반 teacher면 KD가 CE보다 낮을 수 있음 |
| CUB-200 | [Good Practices](https://arxiv.org/abs/2402.11305) | 87.3 → 89.7 | DINOv2 ViT-L → ViT-S (사전학습) |
| Flowers-102, Oxford Pets, Food101 | [Patient & Consistent](https://arxiv.org/abs/2106.05237) | 66.4 → 92.8, 74.2 → 91.0, 74.6 → 87.2 | R50 처음부터, 수천 epoch |
| 소규모 전반 | [Hidden Role of Dataset Size](https://arxiv.org/abs/2510.15516) | 데이터가 적을수록 KD 이득이 큼 | CIFAR-100 10%에서 +25.5%p |
