# 원문 대조 보고서: `notes/1_Shrink.md` 01–10

- 대조 대상: `$PAPER/notes/1_Shrink.md` (01–10번 전부), 이미 들어 있는 `**정정**` 줄 포함
- 원문: 저장소 PDF 8편 (`Related Work/Efficient KD papers/1_Shrink/`), 09·10번은 arXiv에서 받은 PDF
- 판본 확인: 02–05번은 저장소 PDF의 md5를 arXiv v1·v2와 비교해서 정함. 01·06·07·08·10번은 PDF 첫 쪽 arXiv 표기로 정함. 09번은 arXiv에 v1만 있음.
- 외부 근거 3건도 원본에서 확인함: Mind Your Augmentation (ICLR 2024 PDF) Table 3, Distilling OOD Robustness (arXiv 2311.01441v2) Table 3, Evowind/confident-dunce `figures/seeds_summary.csv` (main).
- 쪽 번호는 PDF 첫 장을 1로 셈.
- 작업 파일: `verify/tools/shrink/` (추출 텍스트 `txt/`, 렌더링 `img/`, 판본 비교용 PDF `ver/`, 외부 근거 `ext/`)

---

## Shrink-01 MaskedKD (The Role of Masking for Efficient Supervised KD of ViTs)
- 본 판본: arXiv 2302.10494v4 (PDF 첫 쪽 표기, 27 Sep 2024)
- 대조 결과: 일치 12건 / 수정 9건 / 추가 5건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | student 마지막 층 CLS attention이 낮은 토큰을 버림. student는 전체 이미지를 봄 | 마지막 층 class-token attention의 head 평균, top-k 유지. "we keep the student input unmasked" | §3·3.1 p.5–6, 부록 H p.25 | 일치 | |
| 2 | DeiT-S → DeiT-Ti, 50% 유지: 1770 → 866 PFLOPs (−51%), 1790 → 3702 img/s (2.1배) | 같은 값. 캡션: "total supervision cost of the teacher throughout the training" | Table 2 p.8 | 일치 | |
| 3 | GPU 메모리 teacher 6499 → 3412 MB, 합계 12925 → 9838 MB (−24%) | 값은 같음. 조건: "distilling DeiT-B to DeiT-S with batch size 128" | 부록 I p.26, Table 11 p.27 | 수정 | GPU 메모리(DeiT-B → DeiT-S, batch 128): teacher 6499 → 3412 MB, 합계 12925 → 9838 MB (−24%). 배치 크기나 오프로딩에 따라 감소율이 달라진다고 원문이 밝힘. (Table 11, p.27) |
| 4 | Table 10의 17.5 → 8.6 GFLOPs(DeiT-B). "재대조 안 함" | 값은 같음. 임베딩·잔차·LayerNorm 같은 작은 연산은 빼고 계산한 근사치 | Table 10 p.26 | 수정 | teacher forward 1회 GFLOPs: DeiT-B 17.5 → 8.6 (50% 유지). 임베딩·잔차·정규화 연산은 뺀 근사치. (Table 10, p.26) |
| 5 | logit KD 75.0 → 50% 75.2, 40% 74.9 | 같은 값 (DeiT-S → DeiT-Ti) | Table 2 p.8 | 일치 | |
| 6 | 25% 유지는 teacher에 따라 74.7~74.8 | 25% 행은 DeiT3-S(기준 75.1 → 74.8)와 DeiT3-B(기준 74.4 → 74.7)에만 있음. 75.0 기준과 비교하는 값이 아님 | Table 2 p.8 | 수정 | 25% 유지: DeiT3-S teacher 75.1 → 74.8 (−0.3), DeiT3-B teacher 74.4 → 74.7 (+0.3). teacher마다 기준값이 다름. (Table 2, p.8) |
| 7 | ESC-50 50% 유지 86.3 → 85.7 | 같은 값. 83% 유지는 86.4 | Table 3 p.9 | 일치 | |
| 8 | [구조] attention 없는 student에는 그대로 못 씀 | "MaskedKD is specialized for the supervised distillation of transformer-based models" | §6 p.14 | 일치 | |
| 9 | [구조] 논문 스스로 한계로 적음: 가짜 상관 강화 가능 (6절) | 이 서술은 한계 문단이 아니라 "Potential negative societal impact" 문단에 있음 | §6 p.14 | 수정 | [구조] 저자가 "Potential negative societal impact" 문단에서 attention 기반 선택이 데이터의 가짜 상관을 강화할 수 있다고 적음. 실험은 없음. (§6, p.14) |
| 10 | 옮겨 간 비용: attention 평균·정렬·선택이 샘플당 4.1 kFLOPs 미만 | 4.1 kFLOPs 상한은 head 평균(N·H FLOPs)만의 값: "the only additional computation is taking the mean" | §3.2 p.6 | 수정 | 옮겨 간 비용: student attention의 head 평균만 N·H FLOPs이며 ViT-G/14 기준 샘플당 4.1 kFLOPs 미만. top-k 정렬·gather 비용은 따로 세지 않음. (§3.2, p.6) |
| 11 | 병렬화에 micro-batching 필요 (부록 F) | student forward가 끝나야 teacher forward가 시작됨. 직렬로 돌려도 vanilla KD보다 빠를 수 있다고 함. 여러 GPU를 쓸 때 GPipe식 파이프라인으로 유휴 시간을 메울 수 있다고 함 | §3.2 p.6, 부록 F p.23 | 수정 | student → teacher 순서 의존이 생김. 여러 GPU를 쓸 때는 micro-batch 파이프라인으로 유휴 시간을 메울 수 있다고 함. 측정한 시간은 없고 그림(Fig. 8)만 있음. (부록 F, p.23) |
| 12 | teacher forward 셈? Y (teacher만) / 일회성 비용 셈? N | Table 2 PFLOPs는 teacher 감독 비용만 셈. teacher 학습 비용은 없음 | Table 2 p.8 | 일치 | |
| 13 | 하드웨어 RTX A6000 1장, FP32, batch 128 (부록 A) | "Throughputs reported in this paper are measured on a single NVIDIA RTX A6000 … FP32 … batch size 128" | 부록 A p.18 | 수정 | 하드웨어: 처리량(img/s) 측정만 RTX A6000 1장, FP32, batch 128. 학습에 쓴 GPU는 따로 밝히지 않음. (부록 A, p.18) |
| 14 | student는 처음부터 학습 | ViT-L·ViT-H student는 "start from the MAE checkpoint, and fine-tune for 50 epochs" | §4.1 p.7 | 수정 | student는 DeiT 레시피로 처음부터 학습. 단 ViT-L·H student는 MAE 체크포인트에서 50 epoch 미세조정. (§4.1, p.7) |
| 15 | 3 seed 평균 | "except for the large-scale experiments like CLIP and those with large/huge student models". ESC-50은 5 seed | §4.1 p.7, 부록 A p.18 | 수정 | 3 seed 평균. CLIP teacher와 large·huge student 실험은 제외(단일 실행). 오디오(ESC-50)는 5 seed 평균. (p.7, p.18) |
| 16 | Tiny는 crop + flip만 | "only use random resized crops and horizontal flips for tiny model" | 부록 A p.18 | 일치 | |
| 17 | λ=1, T=1 | (λ, τ) = (1.0, 1) | 부록 A p.18 | 일치 | |
| 18 | 224px, 196 패치, DeiT3-H·MAE-ViT-H는 14×14 패치 256개 | 같음 | §4.1 p.7 | 일치 | |
| 19 | teacher: DeiT·DeiT3·CaiT·CLIP·MAE | Table 2에는 ConViT-B teacher(표기 "ConvViT-B")와 Swin-Ti student(DeiT-B teacher)도 있음 | Table 2 p.8 | 추가 | Table 2에 ConViT-B teacher(DeiT-Ti student, 73.8 → 74.2, 8543 → 5540 PFLOPs)와 Swin-Ti student(DeiT-B teacher, 75% 유지)도 포함. (Table 2, p.8) |
| 20 | 평균 외: 선형 평가(Table 4·8), 검출 AP·분할 mIoU(Table 12). 그룹별·분포 이동·손상 평가 없음 | Table 12: YOLOS-B → Ti VOC AP 45.4 → 45.9(50% 마스킹), Segmenter-S → Ti ADE20k mIoU 38.7 → 38.8(25% 마스킹). 강건성 평가는 본문·부록 어디에도 없음 | Table 4 p.9, Table 8 p.21, Table 12 p.27 | 일치 | |
| 21 | 위치 표 목록 | Table 2 p.8, Table 3·4 p.9, Table 9 p.22, Table 10 p.26, Table 11·12 p.27, 부록 A p.18 | 해당 쪽 | 일치 | |
| 22 | Table 9: DeiT-S 약한 증강 71.3 → 79.7, 강한 증강 79.9 → 80.8 | 같은 값. 71.3·79.9는 증류 없음, 79.7·80.8은 DeiT-B logit KD. MaskedKD 50%는 80.2·81.0 | Table 9 p.22 | 일치 | |
| 23 | (없음) DINO 적용 FLOPs | DINO teacher 총 589 → 384 PFLOPs (66% 유지), 선형 평가 73.7 → 74.2 | Table 4 p.9, p.10 | 추가 | DINO 자기증류에 적용(DeiT-S, IN-1k 100 epoch): teacher 총 589 → 384 PFLOPs (66% 유지), 선형 평가 73.7 → 74.2. GPU 4장으로 줄인 설정. (Table 4, p.9–10; 부록 A p.18) |
| 24 | (없음) 오디오 연산량 | AST-S: 92.8 → 75.9 TFLOPs (83%, 86.4), → 44.0 TFLOPs (50%, 85.7) | Table 3 p.9 | 추가 | 오디오(AST-S, ESC-50): teacher 92.8 → 75.9 TFLOPs에서 정확도 유지(86.3 → 86.4), 44.0 TFLOPs(50%)에서 −0.6. (Table 3, p.9) |
| 25 | (없음) 고해상도 teacher의 분모 | "we did not count the FLOPs that are required for the interpolation procedures" | 부록 C p.20 | 추가 | 384px teacher 실험(Table 7)의 FLOPs에는 attention 점수·이미지 보간 비용이 빠짐. 같은 FLOPs라면 224px teacher + vanilla KD 쪽이 낫다고 원문이 밝힘. (부록 C, p.20) |
| 26 | (없음) 실제 학습 시간 | wall-clock 학습 시간 표는 없음. 서론은 teacher 예측 사전 계산이 "requires ample storage"라고만 적고 수치는 없음 | p.2, 부록 F p.23 | 추가 | 실제 학습 시간(wall-clock)은 보고 안 됨. 효율은 teacher FLOPs·teacher 처리량·메모리로만 제시. (Table 2, Table 11) |

---

## Shrink-02 DMAE (Masked Autoencoders Enable Efficient Knowledge Distillers)
- 본 판본: arXiv 2208.12256v2 (첫 쪽 표기 "v2 10 Nov 2022", md5가 arXiv v2와 같음)
- 대조 결과: 일치 6건 / 수정 5건 / 추가 2건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | MAE 인코더 중간 층 특징 정렬. 로짓 일치는 아님 | 기본 설정은 student 9번째 층 ↔ ViT-L teacher 18번째 층 (둘 다 깊이의 3/4) | §3.2 p.3, §4.1 p.4 | 일치 | |
| 2 | 보이는 패치만 처리하고 teacher는 정렬 층까지만 실행해 순전파 계산을 줄임 | 초록과 §3.2에 같은 주장. 다만 teacher 실행을 줄여 얻은 FLOPs나 시간은 따로 보고하지 않음 | p.1, p.4 | 일치 | |
| 3 | [구조] teacher·student가 같은 보이는 패치를 쓰는 정렬 | Fig. 1과 §3.2. AMD 논문도 DMAE를 "symmetric masking"으로 분류함 | Fig. 1 p.2 | 일치 | |
| 4 | 투영층 + 특징 정렬 손실 + 픽셀 복원 | 2층 MLP(GELU, hidden = teacher 차원), L1 손실, α=1. 디코더 깊이 8이 기본 | §4.1 p.4, Table 3 p.6 | 일치 | |
| 5 | Table 7: MAE-B 100 epoch 78 + 112 = 190h, 1600 epoch 1248 + 112 = 1360h. A5000 1장 | 같음. "tested on a single NVIDIA A5000 GPU" | §4.6, Table 7 p.8 | 일치 | |
| 6 | 195 대 208 GPU시간. "208의 설정은 Table 7에서 다시 확인할 것" | 208h = Supervised Feature Alignment (MAE 사전학습 후 미세조정한 ViT-L teacher → ViT-B, 지도 학습 100 epoch, 82.8%). DMAE-B = 83 + 112 = 195h, 84.0%. DeiT-B soft·hard 증류는 213h | §4.3 p.7, Table 7 p.8 | 수정 | 제안 방법 195 GPU시간(증류 사전학습 83h + 미세조정 112h, 84.0%) 대 Supervised Feature Alignment 208h(미세조정된 ViT-L teacher에서 지도 증류 100 epoch, 82.8%). DeiT-B soft·hard 증류는 213h. 레시피 전체끼리의 비교라서 teacher 층을 줄인 효과만 따로 볼 수는 없음. (Table 7, p.8) |
| 7 | teacher forward 셈? Y (증류 사전학습 GPU 시간에 포함) | 표에는 사전학습·미세조정·합계만 있음. teacher 실행 시간이 따로 나와 있지 않음 | Table 7 p.8 | 수정 | teacher forward 셈? Y로 보임 (증류 사전학습 83h에 teacher 실행이 들어 있을 것으로 추정, 내역 없음). |
| 8 | 일회성 비용 셈? N (teacher 사전학습 비용 없음) | teacher는 공개 MAE 체크포인트. Table 7에 teacher 학습 비용 없음 | §4.1 p.4 | 일치 | |
| 9 | 설정: ImageNet-1K. DINO·MoCo-v3 학습에도 적용 | DINO·MoCo-v3는 §4.8의 "preliminary results"이고 하이퍼파라미터 조정 없음 | §4.8, Table 9 p.9 | 수정 | 설정: ImageNet-1K. 기본 teacher는 MAE ViT-L(공개 체크포인트), student는 무작위 초기화 ViT-B. 사전학습 100 epoch, 마스킹 75%. DINO(80.9 → 82.2)·MoCo-v3(81.1 → 82.5)는 예비 실험. (§4.1 p.4, Table 9 p.9) |
| 10 | 4.7절에서 여러 seed의 평균·표준편차로 성능 향상의 안정성을 확인 | 3 seed 평균·표준편차는 DMAE의 정렬 위치 3가지에만 있음(예: 84.03 ± 0.10). MAE 기준선은 seed를 반복하지 않음 | §4.7 p.8, Table 8 p.9 | 수정 | 평균 정확도 외 평가: DMAE 정렬 위치 3가지의 3 seed 평균·표준편차(9번째 층 84.03 ± 0.10 등). 기준선은 seed를 반복하지 않아서 기준선 대비 유의성 검정은 아님. (Table 8, p.9) |
| 11 | 위치: 4.7절만 | 비용은 §4.6·Table 7, 기준선 비교는 Table 4 | p.7–9 | 수정 | 위치: Table 4 (기준선, p.7), Table 7 (GPU 시간, p.8), Table 8 (seed 표준편차, p.9), Table 9 (DINO·MoCo-v3, p.9). |
| 12 | (없음) 적은 데이터 평가 | IN-1%·10% 라벨: DMAE 50.3·73.4 대 MAE-1600 49.6·72.8. 기준선은 모두 1600 epoch MAE에서 초기화 | §4.5, Table 6 p.8 | 추가 | 적은 라벨 평가: IN-1%·10%에서 DMAE-B 50.3·73.4, MAE-B 1600 epoch 49.6·72.8. (Table 6, p.8) |
| 13 | (없음) 높은 마스킹 비율 | 마스킹 95%(보이는 패치 10개)에서 83.6, 98%(4개)에서 82.4. 이 설정의 비용은 보고하지 않음 | Fig. 2 p.6 | 추가 | 마스킹 95%에서 83.6, 98%에서 82.4 (ViT-L → ViT-B). 이 설정의 학습 시간은 보고 안 됨. (Fig. 2, p.6) |

---

## Shrink-03 Dyna-DINO
- 본 판본: arXiv 2606.19483v2 (md5가 arXiv v2와 같음)
- 대조 결과: 일치 7건 / 수정 4건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 앞쪽 층을 충분히 학습한 뒤 중간 층·마지막 층으로 목표를 옮김 | teacher 첫 블록에서 시작. online CKA ≥ τ 이거나 대기 epoch이 E_max에 도달하면 다음 블록으로 넘어감 | §3.2 p.4–5, Algorithm 1 p.5 | 수정 | 효율화 방식: teacher 첫 블록부터 목표로 삼고, student 출력과 현재 목표 층의 online CKA가 τ 이상이거나 대기 epoch이 E_max에 이르면 다음 블록으로 넘어감. 아직 목표가 아닌 뒤쪽 teacher 블록은 실행하지 않음. (Algorithm 1, p.5) |
| 2 | 얻는 것: 학습 FLOPs와 실제 학습 시간 감소 | 같음 | Table 1 p.6 | 일치 | |
| 3 | [구조] teacher 중간 특징이 필요. 최종 출력만 주는 teacher에는 못 씀 | "assumption of a white-box teacher model with accessible intermediate features" | §5 p.10 | 일치 | |
| 4 | 옮겨 간 비용: CKA 계산, 전환 판단, 투영층 | 같음. 투영층은 패치·CLS가 함께 쓰는 단일 선형층 1개, 파라미터 0.07M(S→T)·0.3M(B→S). CKA 계산 비용은 수치로 보고하지 않음 | §4.2 p.5, Table 7 p.10 | 일치 | |
| 5 | teacher forward 셈? Y (Table 1 캡션) / 일회성 비용 N | 캡션: "reduction in teacher computational overhead" | Table 1 p.6 | 일치 | |
| 6 | 하드웨어: L40S·RTX A6000·L40 등이 섞인 클러스터. 여러 종류 GPU에서 잰 시간이라 시간 비교에 주의 | 클러스터 목록에 GeForce RTX 2080 Ti도 있음. 비용은 L40S 시간으로 적음: IN-100 실험 12–14시간, IN-1K 증류 300–400시간. 어느 GPU에서 시간 절감을 쟀는지는 밝히지 않음 | §4.1 p.5 | 수정 | 하드웨어: L40S·RTX A6000·L40·RTX 2080 Ti 혼합 클러스터. 실험 1회 비용은 IN-100 12–14 L40S시간, IN-1K 300–400 L40S시간. 시간 절감률을 어느 GPU에서 쟀는지는 밝히지 않음. (§4.1, p.5) |
| 7 | ViT-G → ViT-S, IN-100 100 epoch, FLOPs −25.1%, 시간 −21% | 같은 값. Fixed-anchor 대비, CKA 임계값 0.85 | Table 1 p.6 | 일치 | |
| 8 | 분모가 teacher 오버헤드라 전체 학습 비용 절감률과 다름 | 같은 메모의 정정 줄과 충돌함. §4.2는 같은 표의 수치를 "reduction in total training time"이라고 부름 | Table 1 캡션, §4.2 p.6 | 수정 | 표 1 캡션은 teacher 오버헤드 절감으로, §4.2는 총 학습 시간 절감으로 부름. 범위를 정하지 않고 두 표현을 함께 적음. (Table 1, §4.2, p.6) |
| 9 | 정정 줄: 캡션 대 §4.2 분모 불일치 | 캡션 "Efficiency metrics … represent the reduction in teacher computational overhead". §4.2 "up to a 28.8% reduction in training FLOPs and a 22.5% reduction in total training time" | p.6 | 일치 | |
| 10 | (없음) ViT-Tiny 수치 | ViT-Tiny: FLOPs −28.82%, 시간 −22.5% | Table 1 p.6 | 추가 | ViT-G → ViT-Tiny, IN-100: FLOPs −28.82%, 시간 −22.5%. (Table 1, p.6) |
| 11 | (없음) ImageNet-1K 절감률 | IN-1K(CKA 0.8): ViT-S FLOPs −9.44%·시간 −4.83%, ViT-Tiny −12.4%·−7.5%. 저자도 "modest"라고 함 | Table 6 p.9, §4.4 p.8 | 추가 | ImageNet-1K에서는 절감 폭이 작음: ViT-S FLOPs −9.44%·시간 −4.83%, ViT-Tiny −12.4%·−7.5% (CKA 0.8). 저자도 "modest"라고 씀. (Table 6, p.9) |
| 12 | 설정: ADE20K 분할, Oxford·Paris 검색 | 증류는 ImageNet-100·1K에서 함. teacher는 DINOv2 ViT-G(registers 포함), student는 무작위 초기화 ViT-S·ViT-Tiny, LARS, batch 256 | §4.1 p.5 | 추가 | 설정: ImageNet-100(절제 실험)·ImageNet-1K에서 증류. teacher는 DINOv2 ViT-G(registers 포함), student는 무작위 초기화 ViT-S·ViT-Tiny. LARS, 전체 batch 256. 다운스트림은 ADE20K 분할, revisited Oxford·Paris 검색. (§4.1, p.5) |
| 13 | 평균 외: 분할 3가지 프로토콜, 검색 mAP | 같음 (IN-100은 Table 2·3, IN-1K는 Table 4·5) | p.6–8 | 일치 | |
| 14 | 위치: 4.3절의 분할·검색 평가 | 효율 수치는 Table 1·6에 있음 | p.6–9 | 수정 | 위치: Table 1 (IN-100 효율, p.6), Table 6 (IN-1K 효율, p.9), Table 2–5 (검색·분할, p.7–8), §5 (한계, p.10). |

---

## Shrink-04 Asymmetric Masked Distillation (AMD)
- 본 판본: arXiv 2311.03149v2 (md5가 arXiv v2와 같음)
- 대조 결과: 일치 10건 / 수정 2건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | teacher 마스킹 비율을 낮춤. 공통 패치는 직접 정렬, teacher만 보는 패치는 생성 정렬 | 같음. 두 정렬을 직렬로 연결하고 투영층을 공유함 | §3.2–3.4 p.4–5 | 일치 | |
| 2 | SSV2 200 epoch: 무마스킹 32h, AMD(teacher 45%·student 90%) 19h | No-Masking(teacher 0%·student 0%) 72.4%·32h, AMD 45/90 72.5%·19h, AMD 0/90 72.1%·26h | Table 7 p.8 | 일치 | |
| 3 | [구조] student 보이는 패치 ⊂ teacher 보이는 패치 | Eq.(2) P_stu ⊊ P_tea | §3.3 p.4 | 일치 | |
| 4 | 옮겨 간 비용: 특징 생성기·정렬 손실. 대칭 마스킹보다 teacher 처리 토큰이 많음 | 같음. "the teacher is the computational bottleneck in the whole structure" | §3.4 p.5, p.6 | 일치 | |
| 5 | 하드웨어: A100 16장에서 200 epoch 학습·시간 측정 (Table 1 캡션) | "All models are trained and timed for 200 epochs on 16 A100 GPUs" | Table 1 p.6 | 일치 | |
| 6 | 위 시간(Table 7)도 같은 하드웨어로 읽음 | Table 7 캡션에는 하드웨어가 없음. 부록 B "All ablation experiments conduct with 16 GPUs"에서 추정할 수 있을 뿐 | Table 7 p.8, 부록 B p.9 | 수정 | Table 7의 시간은 하드웨어가 캡션에 없음. 부록 B의 "절제 실험은 GPU 16장" 문장으로 추정한 것이며, Table 1만 A100 16장을 명시함. (p.6, p.9) |
| 7 | 사전학습은 A100 80 GB 32장. 보고 시간은 경과 시간으로 보임 | "32 A100-80G GPUs for pre-training on SSV2 and K400" | 부록 B p.9 | 일치 | |
| 8 | teacher forward 셈? Y / 일회성 비용 셈? N | 학습 전체 시간. teacher는 2400 epoch 사전학습된 VideoMAE ViT-L이고 그 비용은 셈하지 않음 | §4.2 p.5 | 일치 | |
| 9 | 설정: SSV2·K400 영상, ImageNet-1K 이미지, MAE 기반 | 같음. 절제 실험: teacher VideoMAE ViT-L 2400 epoch → student ViT-B 16 frame, student 마스킹 90% 고정. 이미지 실험: student 75%·teacher 50% | §4.2 p.5, §4.4 p.8 | 일치 | |
| 10 | 평균 외: AVA mAP, UCF101·HMDB51 전이, 행동 종류별 결과, 평균 어텐션 거리 | Table 6(AVA 29.9·33.5), Table 3, Fig. 5, Fig. 4 | p.6, p.8, p.11 | 일치 | |
| 11 | 위치: Table 1·7, 부록 Figure 4·5 | 같음 | p.6, p.8, p.11 | 일치 | |
| 12 | 메모: Table 7 무마스킹 기준선과 Table 1의 대칭·비대칭 마스킹 비교는 기준이 다름 | Table 1에는 대칭 마스킹 행이 없음. Table 1(c)는 student 90%를 고정하고 teacher 비율만 85·75·60·45%로 바꾼 표임 | Table 1(c) p.6 | 수정 | 메모: Table 7은 무마스킹(양쪽 0%) 기준선과의 비교이고, Table 1(c)는 student 90%를 고정하고 teacher 비율만 바꾼 비교임. 32 → 19시간은 무마스킹 대비로만 사용. |
| 13 | (없음) 기본 설정 시간 | Table 1(c): teacher 85% 12.08h(72.17), 75% 13.33h(72.38, 기본값), 60% 15.47h(72.44), 45% 18.93h(72.48) | Table 1(c) p.6 | 추가 | teacher 마스킹 비율에 따른 시간(A100 16장, 200 epoch): 85% 12.08h, 75%(기본) 13.33h, 60% 15.47h, 45% 18.93h. 정확도는 72.17 → 72.48. (Table 1c, p.6) |
| 14 | (없음) 생성기 비용 | 직접 정렬만 10.83h, 직접 + 생성(직렬) 13.33h. 생성기 깊이 1·2·4층: 12.92·13.33·15.33h | Table 1(a)(d) p.6 | 추가 | 생성 정렬의 부가 비용: 직접 정렬만 10.83h, 직접 + 생성 13.33h(+23%). 생성기 깊이 1·2·4층에서 12.92·13.33·15.33h. (Table 1a·1d, p.6) |
| 15 | (없음) teacher 0% 비교 | 같은 student 90%에서 teacher 0% → 45%로 바꾸면 26h → 19h, 72.1 → 72.5. teacher 25%는 72.3 | Table 7 p.8, 부록 F p.11 | 추가 | AMD 안에서 비교(student 90% 고정): teacher 0% 26h·72.1%, 45% 19h·72.5%. teacher 25%는 72.3%. (Table 7 p.8, 부록 F p.11) |

---

## Shrink-05 MaskAlign
- 본 판본: arXiv 2211.08887v2 (md5가 arXiv v2와 같음)
- 대조 결과: 일치 6건 / 수정 5건 / 추가 2건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | student가 보는 패치의 특징과 teacher의 전체 이미지 특징을 맞춤. DA 모듈 사용, 복원 디코더 없음 | 같음. 정렬 대상은 teacher의 뒤쪽 top K(=5) 층, 손실은 smooth L1 | §3 p.3–4, Table 3 p.6 | 일치 | |
| 2 | 얻는 것: 픽셀 복원 디코더의 계산 제거 | student 인코더에 들어가는 패치도 30%뿐(Forward Ratio 30%, 기본 마스킹 70%) | Table 1 p.5, §4.2 p.5 | 수정 | 얻는 것: 복원 디코더를 없애고 student 인코더는 패치 30%만 처리함(마스킹 70%). teacher는 전체 이미지를 처리함. (Table 1, p.5) |
| 3 | [실측·외부] COCO 물체 특징 분류에서 배경을 바꾸면 Top-1 77.4 → 53.8% | 값은 같음(OKNN → OKNN-D). 같은 표의 다른 방법도 모두 크게 떨어짐: DenseCL 72.5 → 41.5, SoCo 73.5 → 43.7, iBOT 75.2 → 45.8. 비교한 기존 방법 가운데 MaskAlign의 OKNN-D 53.8이 가장 높음 | Mind Your Augmentation Table 3 p.8 | 수정 | [실측·외부] COCO 물체 특징 k-NN에서 선택한 상자 밖 영역을 무작위 배경으로 바꾸면 Top-1 77.4 → 53.8%. 같은 표의 DenseCL(72.5 → 41.5)·iBOT(75.2 → 45.8)도 비슷하게 떨어지고, 비교한 기존 방법 중 MaskAlign이 가장 높음. MaskAlign만의 손실 근거로 쓰지 않음. (Mind Your Augmentation, Table 3, p.8) |
| 4 | [구조] 보이는 패치 위치의 teacher 특징이 필요. 로짓만 주는 teacher로는 불가 | 다층 teacher 특징 ỹ를 정규화해 목표로 씀 | §3.3 p.4 | 일치 | |
| 5 | 옮겨 간 비용: teacher 전체 이미지 특징 추출, 정규화, DA 학습·계산 | 같음. DA는 "nearly no increase in computational effort"라고 하나 수치는 없음 | §3.3 p.4 | 일치 | |
| 6 | Table 7은 상대 배수: 제안 200 epoch 1.0×, BEiT v2 300 epoch 2.8×, MILAN 400 epoch 3.2× | 같은 값. MVP·FD-CLIP도 300 epoch 2.8×. "Training speeds of these works are uniformed"라고만 적고 맞춘 방법은 설명하지 않음 | Table 7 p.7 | 일치 | |
| 7 | teacher forward 셈? Y로 보임 (내역 없음) | 본문에 적혀 있음: "the inference time of the teacher model makes our method not have a linear acceleration ratio" | p.7 | 수정 | teacher forward 셈? Y (teacher 추론 시간 때문에 가속이 선형이 아니라고 본문에 적힘, 내역은 없음). 일회성 비용 셈? N. (p.7) |
| 8 | 하드웨어 보고 안 됨 | 본문·부록에서 GPU 종류·장수·시간을 찾을 수 없음 | 전체 | 일치 | |
| 9 | 설정: COCO 탐지·인스턴스 분할 평가 | 사전학습 설정이 빠짐: ImageNet-1K, student ViT-B/16(ViT-L/16도), teacher CLIP-B/16, 200 epoch, attentive 마스킹 70%, batch 1024 | §4.1 p.4, Table 3·4 p.6 | 수정 | 설정: ImageNet-1K 사전학습, student ViT-B/16(ViT-L/16 포함), 고정 teacher CLIP-B/16(DINO-B·CLIP-L 비교), 200 epoch, attentive 마스킹 70%, batch 1024. 다운스트림은 COCO Mask R-CNN(ViTDet). (§4.1 p.4) |
| 10 | 평균 외: APbox·APmask. 부록의 선형 프로빙·분할·배경 평가도 확인 | APbox 52.1·APmask 45.7(사전학습 400 epoch), 선형 프로빙 79.9, ADE20K 52.1 mIoU, ImageNet-9 배경 평가: No-FG 51.9·Mixed-Rand 78.6(MILAN 46.8·77.9) | Table 2 p.6, Table 8–10 p.11 | 추가 | 원논문 자체 배경 강건성(ImageNet-9, 선형 프로빙): No-FG 51.9, Mixed-Rand 78.6, Mixed-Next 76.6. MILAN은 46.8·77.9·74.7. (Table 10, p.11) |
| 11 | 위치: 4.2절 COCO, 부록 A·B·C | 비용 표 위치가 빠짐 | p.5–7, p.11 | 수정 | 위치: Table 1 (Forward Ratio, p.5), Table 2 (COCO, p.6), Table 7 (사전학습 속도, p.7), 부록 A–C (Table 8–10, p.11). |
| 12 | 메모: student 토큰·디코더 연산과 teacher 전체 이미지 추출 비용을 나눠 기록 | 원문은 둘을 나눈 수치를 주지 않음 | p.7 | 일치 | |
| 13 | (없음) 마스킹 비율과 비용 | 마스킹 비율 비교는 반복 수를 "200 epochs for 70% mask ratio"에 맞춰 함. 마스킹 0%에서도 84.9 | Table 4 p.6 | 추가 | 마스킹 비율 절제(Table 4)는 반복 수를 70% 마스킹 200 epoch에 맞춤. 정확도는 20–80%에서 85.1–85.3으로 평평함. (Table 4, p.6) |

---

## Shrink-06 Decoupled KD (DKD)
- 본 판본: arXiv 2203.08679v2 (PDF 첫 쪽 표기)
- 대조 결과: 일치 7건 / 수정 3건 / 추가 3건 / 확인 불가 1건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | TCKD·NCKD 분리, NCKD 계수 (1−p_t)를 독립 가중치 β로 바꿈 | 같음 | §3 p.3–5 | 일치 | |
| 2 | 얻는 것: 특징 정렬 모듈·메모리 뱅크 없이 로짓 손실 | "almost the same computational complexity as KD, and of course no extra parameters" | §4.2 p.7 | 일치 | |
| 3 | [실측·외부] WRN-40-2 → ResNet-20, student 단독 대비 ECE 0.2258 → 0.2379 (3 seed) | CSV `corrupted` 행 값과 같음(표준편차 0.0164 대 0.0021). 같은 CSV에서 vanilla KD는 0.3321. clean ECE는 0.0686 → 0.0796 | seeds_summary.csv (main) | 일치 | |
| 4 | 위 수치가 CIFAR-100-C라는 점 | CSV의 split 이름은 `corrupted`이고, student 설정 파일(`student_resnet20_dkd.yaml`)에도 데이터셋 이름이 없음 | CSV, 설정 파일 | 확인 불가 | 확인 불가: CSV와 student 설정 파일만으로는 손상 데이터셋 이름을 확인하지 못함. |
| 5 | (외부 수치의 비교 대상) | 같은 CSV에서 DKD는 vanilla KD(0.3321)보다 ECE가 낮음 | CSV | 추가 | 같은 CSV에서 vanilla KD는 0.3321이라서 DKD는 KD보다 보정이 좋고 student 단독보다만 나쁨. 기준을 함께 적음. |
| 6 | [구조·정의] 이진 분류에서는 NCKD KL 항이 0 | 원문이 직접 쓰지는 않음. Eq.(2)의 p̂가 C−1개 원소라서 C=2이면 원소 하나(=1)가 되고 KL이 0이 됨 | Eq.(2) p.3 | 일치 | |
| 7 | 옮겨 간 비용: 두 분포·손실 계산, 가중치 설정 | 같음 | §3 | 일치 | |
| 8 | 배치당 학습 시간 = 데이터 처리 + 순전파·역전파 + 메모리 갱신. teacher 포함 여부 명시 없음 | 같음. 추가 파라미터에는 사전(dictionary) 메모리(CRD contrast memory)도 셈 | 부록 A.7 p.12–13 | 일치 | |
| 9 | 하드웨어 보고 안 됨. 단위는 ms/batch와 추가 매개변수 수 (Figure 2) | GPU 이름이 없음. 단위는 같음 | Fig. 2 p.7 | 일치 | |
| 10 | (없음) Figure 2 수치 | CIFAR-100, ResNet32×4 → ResNet8×4: KD 11 ms·0, DKD 11 ms·0(76.32%), FitNet 14 ms·16.8K, OFD 19 ms·86.9K, RKD 25 ms·0, ReviewKD 26 ms·1.8M, CRD 41 ms·12.3M | Fig. 2 표 p.7 (렌더링으로 확인) | 추가 | 학습 비용(CIFAR-100, ResNet32×4 → 8×4): DKD 11 ms/배치·추가 파라미터 0으로 KD(11 ms)와 같음. ReviewKD 26 ms·1.8M, CRD 41 ms·12.3M. (Figure 2, p.7) |
| 11 | 설정: 특징 전이 실험에서 WRN-40-2 → WRN-16-2를 특징 추출기로 씀 | 맞지만 주 실험 범위가 빠짐: CIFAR-100, ImageNet(ResNet-34 → 18, ResNet-50 → MobileNet-V1), MS-COCO(Faster R-CNN-FPN) | §4 p.5–8 | 수정 | 설정: CIFAR-100, ImageNet(ResNet-34 → ResNet-18, ResNet-50 → MobileNet-V1), MS-COCO 검출(Faster R-CNN-FPN). 특징 전이는 CIFAR-100에서 WRN-40-2 → WRN-16-2로 증류한 특징 추출기를 고정하고 마지막 GAP 특징에 선형층만 학습. (§4, p.5–8; 부록 A.7 p.13) |
| 12 | 평균 외: STL-10·Tiny-ImageNet 선형 프로빙 | Table 13: DKD 72.9·37.1, KD 70.9·33.9, 기준선 69.7·33.7. 그 밖에 COCO AP(Table 10), t-SNE(Fig. 3), teacher-student 로짓 상관행렬 차이(Fig. 4)가 있음 | Table 13 p.8, Table 10 p.7, Fig. 3·4 p.8 | 수정 | 평균 정확도 외 평가: STL-10·Tiny-ImageNet 선형 프로빙(DKD 72.9·37.1, KD 70.9·33.9), COCO 검출 AP, t-SNE, 로짓 상관행렬 차이. (Table 13 p.8, Table 10 p.7, Fig. 3·4 p.8) |
| 13 | 위치: 4.2절 특징 전이 실험 | | Fig. 2 p.7, Table 13 p.8, 부록 A.7 p.12–13 | 수정 | 위치: Figure 2 (학습 시간·파라미터, p.7), Table 10 (COCO, p.7), Table 13 (특징 전이, p.8), 부록 A.7 (측정 정의, p.12–13). |
| 14 | (없음) 원논문의 한계 | 검출에서 DKD 단독이 특징 방법보다 낮음: R-101 → R-18 AP DKD 35.05 대 ReviewKD 36.75. "logits-based methods cannot transfer knowledge about localization" | Table 10 p.7, §5 p.8 | 추가 | [실측·원논문] 검출(COCO, R-101 → R-18)에서 DKD 단독 AP 35.05로 ReviewKD 36.75보다 낮음. 저자가 로짓 방법은 위치 정보를 전달하지 못한다고 한계로 적음. (Table 10 p.7, §5 p.8) |

---

## Shrink-07 Information Theoretic Representation Distillation (ITRD)
- 본 판본: arXiv 2112.00459v3 (PDF 첫 쪽 표기). 정정 줄 확인용으로 v2도 받음
- 대조 결과: 일치 6건 / 수정 4건 / 추가 2건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 상관·상호정보량 기반 손실. 메모리 뱅크·음성 샘플 없음 | "memory bank that incurs in additional memory and computational costs, which we avoid altogether" | §2 p.3 | 일치 | |
| 2 | 얻는 것: 메모리 뱅크 기반 방법 대비 저장·샘플 관리 부담 감소 | 정성적 주장. 저장량 수치는 없음 | p.3 | 일치 | |
| 3 | [구조] teacher·student 중간 특징을 배치 단위로 받음 | 사용하는 것은 최종 FC 직전 표현이고, 중간 특징맵은 비용 때문에 피한다고 함 | §3 p.3–4 | 수정 | [구조] teacher·student의 최종 FC 직전 표현을 배치 단위로 받아 상관·Gram 행렬을 계산함. 로짓만 주는 teacher API에는 그대로 못 씀. (§3, p.3–4) |
| 4 | 옮겨 간 비용: 배치 상관·Gram 행렬, student 투영층 | "only a small overhead coming from a single linear layer … and from computing the gram and cross-correlation matrices" | p.9–10 | 일치 | |
| 5 | 하드웨어: RTX 2080 Ti 1장, 메모리 8 GB 제약에 맞게 배치 조정, 단위 img/s | 같음. ImageNet ResNet-34 → ResNet-18, 100 epoch, torchdistill | §5.1, Fig. 2 p.9 | 일치 | |
| 6 | teacher forward 셈? Y / 일회성 비용 셈? N | "total training efficiency … inversely proportional to the total training time" | p.9 | 일치 | |
| 7 | (없음) Figure 2 값 | 수치 표는 없고 그래프만 있음. 렌더링해서 읽으면 ITRD ≈530 img/s(top-1 71.68, error 28.32%), KD ≈540, CRD(k=16384) ≈130, ReviewKD ≈240, HSAKD ≈30 | Fig. 2 p.9 | 추가 | ImageNet(ResNet-34 → 18) 학습 처리량은 그래프로만 제시됨. 그래프에서 읽으면 ITRD ≈530 img/s로 KD(≈540)와 비슷하고 CRD(k=16384, ≈130)보다 큼. 근사 판독값임. (Figure 2, p.9) |
| 8 | 설정: CIFAR-100·ImageNet, 여러 CNN. 이진 신경망 증류도 | 이진 실험은 CIFAR-10 ResNet-18. NLP SQuAD 1.1(BERT → 6·3층)도 있음 | Table 3 p.10 | 수정 | 설정: CIFAR-100·ImageNet 분류, CIFAR-10 이진 신경망(ResNet-18), SQuAD 1.1 질의응답(BERT 12층 → 6·3층). (Table 1 p.8, Table 3 p.10) |
| 9 | 정정 줄: STL-10 72.7%, Tiny-ImageNet 36.0%는 v2 표 4에만 있음 | v2 p.13 Table 4에서 ITRD 72.7·36.0 확인. v3에는 Table 4가 없고 "see Supplementary"로 넘김 | v2 Table 4 p.13, v3 p.8 | 일치 | |
| 10 | 위치: 표현 전이 Table 4 | v3에는 그 표가 없음 | v3 p.8 | 수정 | 위치: 손실 정의 §3–4 (v3), 학습 효율 Figure 2 (v3 p.9), 표현 전이 Table 4는 v2 p.13에만 있음. |
| 11 | 메모: 연산 비교 시 배치 크기·특징 차원 고정 | 원문 Figure 2는 배치를 고정하지 않고 8 GB 제약에 맞춰 방법마다 조정함 | p.9 | 수정 | 메모: 원문 처리량 비교는 배치 크기를 고정하지 않고 8 GB 메모리 제약에 맞춰 방법마다 조정한 결과임. 배치를 고정한 비교는 원문에 없음. |
| 12 | (없음) ImageNet 정확도 | ITRD error 28.32%. CRCD·HSAKD 다음 | p.9 | 추가 | ImageNet ResNet-34 → ResNet-18: top-1 error 28.32%. 더 비싼 CRCD·HSAKD 다음 순위. (p.9) |

---

## Shrink-08 Understanding the Role of the Projector in KD
- 본 판본: arXiv 2303.11098v5 (PDF 첫 쪽 표기, AAAI 2024)
- 대조 결과: 일치 7건 / 수정 3건 / 추가 2건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 선형 프로젝터·BN·LogSum. 메모리 뱅크·명시적 관계 구조 없음 | Fig. 1, 기여 목록 "remove the need to explicitly construct correlation matrices or memory banks" | p.1 | 일치 | |
| 2 | 얻는 것: 작은 투영층 사용 | 같음 | p.1–2 | 일치 | |
| 3 | [실측·원논문] 프로젝터 특이값 일부가 0에 가까워짐 (Fig. 2) | ResNet-50 → ResNet-18. 성능이 좋은 정규화일수록 0으로 줄어드는 특이값이 적음 | Fig. 2 p.3, p.4 | 일치 | |
| 4 | [실측·원논문] 큰 MLP 프로젝터에서 입출력 상관이 더 낮아짐 (Fig. 3) | 모든 프로젝터가 점차 탈상관되고, 클수록 더 많이 됨 | Fig. 3 p.4 | 일치 | |
| 5 | 어떻게 쟀나: 손실 구성의 연산·메모리 규모와 배치 크기·특징 차원 | 시간·FLOPs·메모리 측정이 없음. "much more computationally efficient"(초록), "reduce the complexity and memory consumption"(결론)이라는 주장만 있음 | p.1, p.7 | 수정 | 어떻게 쟀나: 비용 측정 없음. 효율은 구조 설명과 초록·결론의 주장으로만 제시. 정확도 벤치마크만 있음. (p.1, p.7) |
| 6 | teacher forward 셈? N / 일회성 비용 셈? N | 비용을 측정하지 않았으므로 Y·N으로 판정할 대상이 없음 | — | 수정 | teacher forward 셈? 해당 없음(비용 측정 없음) / 일회성 비용 셈? 해당 없음. |
| 7 | 하드웨어 RTX A5000 1장 (p.5) | "All experiments were performed on a single NVIDIA RTX A5000" | p.5 | 일치 | |
| 8 | 설정: CIFAR-100·ImageNet, CNN → ViT, COCO | 같음. 정규화 절제는 ImageNet-1K 20% 부분집합에서 함 | Table 1 p.4, Table 4–8 | 일치 | |
| 9 | 평균 외: 특이값, 입출력 상관, 탐지 AP | 같음. 그 밖에 Table 7 평행이동 등변성(DeiT-S 1.52 ± 0.15, CivT-S 0.13, 제안 0.04)이 있음 | Table 7 p.7 | 추가 | 평균 외 평가: 평행이동 등변성 μT (DeiT-S 1.52, CivT-S 0.13, 제안 0.04, 낮을수록 등변). (Table 7, p.7) |
| 10 | (없음) COCO 수치 | Faster R-CNN R50 → MV2: 29.47 → 32.92 mAP (ReviewKD 33.71). YOLOv5m → s: 56.8 → 57.3 (첫 열) | Table 8 p.7 | 추가 | COCO 검출: Faster R-CNN R50 → MobileNetV2 mAP 29.47 → 32.92(ReviewKD 33.71). YOLOv5m → s 56.8 → 57.3. 표의 열 이름(mAP 50-95·mAP 50)과 값의 순서가 맞지 않아 보이므로 인용 시 주의. (Table 8, p.7) |
| 11 | 위치: Figure 2·3, 다운스트림 평가 | | Fig. 2 p.3, Fig. 3 p.4, Table 7·8 p.7 | 수정 | 위치: Figure 2 (p.3), Figure 3 (p.4), Table 1 (정규화 절제, p.4), Table 7·8 (등변성·COCO, p.7), 하드웨어 p.5. |
| 12 | 메모: 특이값·상관을 특정 능력 손실로 바꾸어 적지 않음 | 원문도 증류 효율 저하의 설명으로만 씀 | p.4 | 일치 | |

---

## Shrink-09 Parameter-Efficient and Student-Friendly Knowledge Distillation (PESF-KD)
- 본 판본: arXiv 2205.15308v1 (arXiv에 v1만 있음. `tools/shrink/arxiv/pesf_v1.pdf`)
- 대조 결과: 일치 7건 / 수정 2건 / 추가 4건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 고정된 teacher 본체 대신 작은 어댑터만 갱신 | 같음 (Fig. 1의 세 방식 비교) | 초록 p.1, Fig. 1 p.2 | 일치 | |
| 2 | ImageNet Table 3 batch time: FT-KD 1.44, PESF-KD 0.47, vanilla KD 0.46 (단위는 표 그대로) | 같은 값. 표에 단위 없음. ResNet-50 → ResNet-18, 3 seed 평균 | Table 3 p.8 | 일치 | |
| 3 | (없음) 같은 표의 파라미터·정확도 | params 47M / 149M / 54M, top-1 69.20 / 70.00 / 69.96 (vanilla / FT-KD / PESF) | Table 3 p.8 | 추가 | 같은 표: 갱신 파라미터 vanilla KD 47M, FT-KD 149M, PESF-KD 54M. top-1 69.20 / 70.00 / 69.96. batch time 단위는 표에 없음. (Table 3, p.8) |
| 4 | [구조] teacher에 어댑터를 넣고 학습해야 함 | 같음 | §3.2 p.2–3 | 일치 | |
| 5 | 옮겨 간 비용: 어댑터 학습·teacher 순전파 | 같음 | §3.2 | 일치 | |
| 6 | teacher forward 셈? · 일회성 · 하드웨어 = 확인 안 함 | 부록 Infrastructure: "GPU: 8x GeForce RTX 3090". Table 3 측정에 몇 장을 썼는지, batch time의 정의는 없음 | 부록 p.14 | 수정 | teacher forward 셈? 명시 없음(학습 batch time이라 포함으로 보임) / 일회성 비용 셈? N (시작 teacher의 학습 비용 없음) / 하드웨어 = 부록에 8× RTX 3090이라고만 적음. Table 3 측정에 쓴 장수와 batch time 단위는 명시 없음. (부록 p.14) |
| 7 | 설정: CIFAR-100·ImageNet R50 → R18, GLUE | 같음 | §5 p.7–10 | 일치 | |
| 8 | 평균 외: KL, 예측 일치율(Agree), PL·Gap | v1에는 Agree·PL이 없음. 쓰는 지표는 KL, CKA, sharpness GAP | §4 p.6, §5.2 p.8 | 수정 | 평균 정확도 외 평가: teacher-student 예측 KL, CKA, sharpness GAP. Table 3 PESF-KD는 KL 8.9, GAP 18.6이며 본문 §5.3.2는 19.2로 적어 서로 다름. (p.6, Table 3 p.8, p.9) |
| 9 | 정정 줄: 이 판은 prediction KL, CKA, sharpness GAP를 분석 | 같음 | §4 p.6 | 일치 | |
| 10 | 위치: Figure 1, Table 3, 일관성 분석 | 같음 | p.2, p.6, p.8 | 일치 | |
| 11 | (없음) CIFAR 갱신 파라미터 | vgg13-8: 갱신 파라미터 ∼54M → ∼16M | §5.3.1 p.9 | 추가 | CIFAR-100 vgg13 → vgg8: 갱신 파라미터 ∼54M → ∼16M. (§5.3.1, p.9) |
| 12 | (없음) 어댑터 종류별 비용 | VGG CIFAR-100: 제안 어댑터 batch time 0.137, LoRA 0.166, Scaled PA 0.165 (파라미터 모두 15.95M) | Table 5 p.10 | 추가 | 어댑터 구조 비교(VGG, CIFAR-100): batch time 어댑터 0.137, LoRA 0.166, Scaled PA 0.165. (Table 5, p.10) |
| 13 | 메모: vanilla KD 0.46과 PESF-KD 0.47로 vanilla보다 빠르다고 쓰지 않음 | 맞음. GLUE Table 4의 Time도 "lowest … except for vanilla KD" | p.10 | 추가 | GLUE(Table 4)도 학습 비용이 vanilla KD 다음으로 낮다고 적음. vanilla KD보다 싸다는 근거는 없음. (§5.3.3, p.10) |

---

## Shrink-10 Knowledge Distillation from A Stronger Teacher (DIST)
- 본 판본: arXiv 2205.10536v3 (PDF 첫 쪽 표기. `tools/shrink/arxiv/dist_v3.pdf`)
- 대조 결과: 일치 8건 / 수정 2건 / 추가 3건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 정확한 확률 일치 대신 클래스 간·샘플 간 Pearson 상관 | inter-class(샘플마다 클래스 축), intra-class(클래스마다 배치 축) | p.2, §3 p.5 | 일치 | |
| 2 | 논문은 vanilla KD와 거의 같은 학습 비용이라고 설명 | "has almost the same training cost as the vanilla KD" | p.2 | 일치 | |
| 3 | (없음) 다른 방법의 추가 비용 | CRD는 ImageNet 전체 128-d 특징 메모리 뱅크와 추가 260M FLOPs, SRRL·Review는 추가 conv | p.7 | 추가 | 비교 대상 비용: CRD는 ImageNet 전체 128-d 특징 메모리 뱅크와 추가 260M FLOPs가 필요하다고 원문이 적음. (§4.2, p.7) |
| 4 | [실측·외부] CLIP ViT-L/14 → ViT-B/16, KD 대비 ImageNet-R 51.5 → 40.8, Sketch 34.7 → 26.9 | 같은 값 | DAD Table 3 p.8 | 일치 | |
| 5 | [실측·외부] ImageNet-C mCE 61.5 → 65.8, Stylized 16.2 → 12.7 | 같은 값 | DAD Table 3 p.8 | 일치 | |
| 6 | (외부 표의 분포 내 정확도) | 같은 표에서 분포 내 정확도도 떨어짐: ImageNet 78.6 → 76.6, V2 67.2 → 63.9, A 16.0 → 11.2. KD와 DIST 모두 DAD 증강 없이 학습 | DAD Table 3 p.8 | 추가 | 같은 실험에서 분포 내 ImageNet도 78.6 → 76.6, V2 67.2 → 63.9, ImageNet-A 16.0 → 11.2로 떨어짐. 분포 밖 하락이 분포 내 하락과 함께 나타나므로 강건성만의 손실로 읽지 않음. (Distilling OOD Robustness, Table 3, p.8) |
| 7 | 옮겨 간 비용: 클래스·배치 방향 상관 계산 | 같음 (부록 A.1의 몇 줄짜리 코드) | p.13 | 일치 | |
| 8 | teacher forward 셈? · 일회성 · 하드웨어 = 확인 안 함 | Table 11: 8× V100, ResNet-34 → 18, ImageNet B1 전략, batches/s. teacher 실행 분해 없음 | 부록 A.4, Table 11 p.14 | 수정 | teacher forward 셈? Y로 보임(학습 속도 전체, 내역 없음) / 일회성 비용 셈? N / 하드웨어 = NVIDIA V100 8장. 단위 batches/s. (Table 11, p.14) |
| 9 | 정정 줄: DIST 14.19, vanilla KD 14.28 batches/s | 같은 값. RKD 11.11, SRRL 12.98, CRD 8.33 | Table 11 p.14 | 일치 | |
| 10 | 설정: CIFAR-100·ImageNet, 강한 teacher, 탐지·분할 | 같음 | §4 | 일치 | |
| 11 | 평균 외: 탐지 AP, Cityscapes mIoU | COCO Faster R-CNN R50: DIST 40.4(KD 39.7, CWD 41.7), DIST + mimic 41.8. Cityscapes DeepLabV3-R18 77.10, PSPNet-R18 76.31 | Table 6·7 p.8 | 추가 | 탐지(COCO, Faster R-CNN-R50): DIST 40.4 AP로 KD 39.7보다 높고 CWD 41.7보다 낮음. 41.8은 FPN 특징 MSE를 더한 DIST + mimic의 값. 분할(Cityscapes): DeepLabV3-R18 77.10, PSPNet-R18 76.31 mIoU. (Table 6·7, p.8) |
| 12 | 위치: §1·3, 탐지·분할 평가, 외부 Table 3 | 표 번호·쪽이 없음 | p.8, p.14 | 수정 | 위치: §1 (비용 주장, p.2), §3 (손실, p.5), Table 6·7 (탐지·분할, p.8), Table 11 (학습 속도, p.14), 외부 Table 3 (2311.01441v2, p.8). |
| 13 | 메모: 강한 teacher 비용과 손실 비용 분리, 외부 수치는 해당 설정에 한정 | 원문 Table 11도 teacher를 고정한 채 손실만 바꾼 비교임 | p.14 | 일치 | |

---

## 전체 요약

1. 가장 심각한 오류는 외부 결과의 비교 기준이 빠진 것. 05번 MaskAlign의 배경 교체 77.4 → 53.8%는 같은 표의 모든 방법(DenseCL 72.5 → 41.5, iBOT 75.2 → 45.8)이 함께 떨어진 결과이고, MaskAlign이 그중 가장 높음. 10번 DIST의 분포 밖 하락에는 분포 내 ImageNet 하락(78.6 → 76.6)도 함께 있음. 06번 DKD의 ECE 악화도 같은 CSV에서는 vanilla KD(0.3321)보다 나음. 세 건 모두 지금 쓰인 대로 「잃는 것」 근거로 옮기면 과장이 됨.
2. 분모와 범위가 흔들리는 서술이 남아 있음. 03번은 「어떻게 쟀나」가 "teacher 오버헤드라 전체와 다름"이라고 단정해서 같은 메모의 정정 줄과 충돌하고, 절감 폭이 작아지는 ImageNet-1K 수치(시간 −4.83%·−7.5%)가 빠짐. 01번 4.1 kFLOPs는 head 평균만의 상한인데 정렬·선택까지 포함한 것처럼 적힘.
3. 조건 누락이 가장 흔함. 01번 메모리 수치의 모델 쌍(DeiT-B → DeiT-S)·배치, seed 예외, ViT-L/H 초기화, A6000이 처리량 측정용이라는 범위가 빠짐. 04번 Table 7의 하드웨어는 캡션이 아니라 부록에서 추정한 것.
4. 09·10번의 "확인 안 함" 칸은 원문으로 채울 수 있음(PESF-KD 8× RTX 3090·단위 없는 batch time, DIST 8× V100·batches/s). 02번의 "208 설정 다시 확인"도 Supervised Feature Alignment로 확정됨.
5. 「평균 정확도 외 평가」와 비용 수치는 대체로 덜 적혀 있음. DKD Figure 2의 ms/배치(KD·DKD 11, CRD 41), AMD 생성기 비용(+23%), MaskAlign 자체 ImageNet-9 배경 평가, Projector 등변성 Table 7이 빠짐. 08번 Projector는 비용을 측정하지 않았는데 측정한 것처럼 적힘.
