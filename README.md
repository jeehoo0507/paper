# paper

## Efficient Vision KD Survey (진행 중)

비전 지식 증류(KD)에서 **증류 학습 자체의 비용**을 줄이는 방법을 정리하는 서베이 작업 노트.

### 무엇을 하나

| 질문 | 답할 것 | 현재 방향 (미확정) |
|---|---|---|
| Q1. 어떤 efficient KD 방식이 있나? | 나누는 축 | Shrink / Skip / Reuse / Substitute |
| Q2. 효율을 어떻게 나타내나? | 측정 방식 | 하나의 비용식으로 모든 방법을 같은 기준에 놓음 |
| Q3. 효율화하면 무엇을 잃나? | 평가 방식 | 평균 정확도 말고 그룹별 성능도 봄 |

### 먼저 볼 파일

| 파일 | 내용 |
|---|---|
| [`efficient-vision-kd/README.md`](efficient-vision-kd/README.md) | 주제, 주장, Q1~Q3 상세 |
| [`efficient-vision-kd/PROGRESS.md`](efficient-vision-kd/PROGRESS.md) | **지금까지 진행 과정** |
| [`efficient-vision-kd/experiments/PLAN.md`](efficient-vision-kd/experiments/PLAN.md) | **실험 계획 (초안)** |
| [`efficient-vision-kd/presentation/index.html`](efficient-vision-kd/presentation/index.html) | 발표 자료 (다운로드 후 브라우저로 열기) |
| [`efficient-vision-kd/our paper.md`](<efficient-vision-kd/our paper.md>) | 우리 논문 목차 초안 |
| [`efficient-vision-kd/Related Work/Efficient KD paper.md`](<efficient-vision-kd/Related Work/Efficient KD paper.md>) | **읽을 논문 목록** (내일 읽을 17편이 맨 위) |
| [`efficient-vision-kd/Related Work/survey paper/survey - en.md`](<efficient-vision-kd/Related Work/survey paper/survey - en.md>) | 참고한 기존 서베이 (PART 1~3) |
| [`efficient-vision-kd/notes/`](efficient-vision-kd/notes) | **논문 읽기 메모** (가이드 + 파트별 파일) |
| [`efficient-vision-kd/Related Work/Efficient KD papers/`](<efficient-vision-kd/Related Work/Efficient KD papers>) | 축별 논문 PDF 62편 (1_Shrink ~ 6_Evaluation) |
| [`efficient-vision-kd/Related Work/survey paper/survey- ko.md`](<efficient-vision-kd/Related Work/survey paper/survey- ko.md>) | 랩실 추천 국문 논문 (PART 4) |

### 읽는 순서 (추천)

16 (On-Policy 비용 서베이) → 01~05 → 11 (Patient and Consistent) → 06~10 → 12~13 → 14~15 → 17

번호는 `Efficient KD paper.md` 맨 위 17편 표 기준:

| 번호 | 파트 | 논문 |
|---|---|---|
| 01, 02 | Shrink | MaskedKD, DMAE |
| 03, 04, 05 | Skip | Distill on a Diet, Distilling the Knowledge in Data Pruning, Weak-to-Strong KD |
| 06, 07, 08 | Reuse | FKD, TinyViT, Are Large-scale Soft Labels Necessary? |
| 09, 10 | Substitute | Efficient KD from Model Checkpoints, Proteus |
| 11, 12, 13 | Q2 측정 | Patient and Consistent, Distillation Scaling Laws, Are Coreset Selection Methods Worth Their Cost? |
| 14, 15 | Q3 평가 | Teacher's Pet, When Masking Helps or Hurts Robustness |
| 16, 17 | 선행 서베이 | Efficient On-Policy Distillation (비용 서베이), Survey on Efficient ViTs |

### 참고

- 축별 논문 PDF와 목록: [`efficient-vision-kd/Related Work/Efficient KD papers/`](<efficient-vision-kd/Related Work/Efficient KD papers>). 폴더마다 README에 논문 이름·링크·파일명 표가 있음.
- 분류와 주장은 초안. 논문을 읽으면서 바뀔 수 있음.
