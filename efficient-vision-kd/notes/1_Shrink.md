# 1_Shrink 메모

> **팀원 최신본 대조 (2026-11-19)**: [yohan0825/paper의 1_Shrink.md](https://github.com/yohan0825/paper/blob/main/efficient-vision-kd/notes/1_Shrink.md) 최신본(011e5b2)과 비교함. 이 파일이 그 초안에 수치와 측정 항목을 더한 판이라, 최신본에만 있는 사실은 없음.
>
> **원문 대조 완료 (2026-11-19).** 논문마다 **원문 대조** 블록이 있다. Opus 5.5 서브에이전트 8개가 저장소 PDF(일부는 arXiv·학회판을 받아)와 메모의 수치·조건·판정을 한 줄씩 대조한 결과다. 블록이 위 서술과 다르면 블록이 맞다. 대조 보고서 원본: [`audit/verify-2026-11-19/`](audit/verify-2026-11-19). 사람이 다시 확인하지는 않았다.

Shrink: 한 스텝 비용을 줄임  
논문 목록과 PDF: [`../Related Work/Efficient KD papers/1_Shrink/`](<../Related Work/Efficient KD papers/1_Shrink>)  
작성 규칙과 항목 설명: [README](README.md)

담당: yohan0825 (초안), 01번은 본문 대조 후 수치 보강

## 읽기 전에

- 내용은 원문, 공식 코드, 공개 결과에서 가져온 것이고 직접 재실험한 결과가 아님.
- 「잃는 것」의 표시:
  - **[실측]**: 조건이 명시된 관찰 결과. `원논문` 또는 `외부`(다른 논문·자료)로 출처를 구분.
  - **[구조]**: 방법 정의나 공식 코드에서 나오는 적용 제약. 성능 저하의 실험 증거가 아님.
- 02~10번의 수치는 아직 원문과 다시 대조하지 않음. 인용 전에 「위치」의 표·절을 확인할 것.
- 02~08번의 「어떻게 쟀나」 뒷부분(teacher forward 셈?, 일회성 비용 셈?, 하드웨어)은 2026-11-18에 Claude가 저장소 PDF를 읽고 덧붙였음. 앞부분 서술은 원 작성자의 것 그대로임. 09·10번은 PDF가 저장소에 없어 채우지 않음.

### 01. MaskedKD (The Role of Masking for Efficient Supervised KD of ViTs) ([arXiv: 2302.10494](https://arxiv.org/abs/2302.10494))
- 효율화 방식: teacher 입력 토큰 일부를 버려 teacher forward 연산을 줄임. 버릴 토큰은 student 마지막 층의 CLS attention 점수가 낮은 순으로 고름. student는 전체 이미지를 그대로 봄.
- 얻는 것 (teacher 연산 대비, 전체 학습 대비 아님):
  - DeiT-S → DeiT-Ti, 토큰 50% 유지: teacher FLOPs 1770 → 866 PFLOPs (−51%), teacher 처리량 1790 → 3702 img/s (2.1배). (Table 2)
  - GPU 메모리: teacher 6499 → 3412 MB, student 포함 합계 12925 → 9838 MB (−24%). (Table 11)
  - teacher forward 1회 GFLOPs 수치는 Table 10 (17.5 → 8.6, DeiT-B). 이 값은 재대조 안 함.
- 잃는 것:
  - [실측·원논문] 정확도 손실 거의 없음: logit KD 75.0 → 50% 유지 75.2, 40% 유지 74.9. 25% 유지는 teacher에 따라 74.7~74.8. (Table 2)
  - [실측·원논문] 영상 밖에서는 여유가 작음: 오디오(ESC-50)는 50% 유지에서 86.3 → 85.7. (Table 3)
  - [구조] 토큰 선택에 student 마지막 층 attention이 필요함. attention이 없는 student(CNN 등)에는 그대로 못 씀. [공식 코드](https://github.com/effl-lab/MaskedKD)
  - [구조] 논문 스스로 한계로 적음: attention 기반 선택이 데이터의 가짜 상관을 강화할 수 있음 (6절). 실험은 없음.
- 옮겨 간 비용: student attention 점수 평균·정렬·선택 (ViT-G/14 기준 샘플당 4.1 kFLOPs 미만이라고 보고, 3.2절). student forward가 teacher forward보다 먼저 끝나야 해서 병렬화에 micro-batching 필요 (부록 F).
- 어떻게 쟀나: 단위 = teacher FLOPs(학습 전체 합, PFLOPs), 처리량(img/s), GPU 메모리 / teacher forward 셈? Y (teacher만 셈, student 학습 비용은 분모에 없음) / 일회성 비용 셈? N (teacher 학습 비용 없음) / 하드웨어 = RTX A6000 1장, FP32, batch 128 (부록 A)
- 설정: ImageNet-1k, DeiT 레시피. student는 처음부터 학습 (Tiny는 crop + flip만 사용). teacher는 DeiT·DeiT3·CaiT·CLIP·MAE. 손실 CE + λ·KD, λ=1, T=1. 3 seed 평균. 224px, 16×16 패치 196개 (DeiT3-H, MAE-ViT-H는 14×14 패치 256개).
- 평균 정확도 외 평가: 선형 평가 정확도 (DINO 적용 Table 4, 선형 분류기만 증류 Table 8), 검출 AP·분할 mIoU (Table 12). 그룹별·분포 이동·손상 강건성 평가는 없음.
- 위치: Table 2 (정확도·FLOPs·처리량), Table 3·4 (오디오, DINO), Table 9 (증강), Table 10 (FLOPs 분석), Table 11 (메모리), Table 12 (검출·분할), 부록 A (설정)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 12건 / 수정 9건 / 추가 5건 / 확인 불가 0건. 본 판본: arXiv 2302.10494v4 (PDF 첫 쪽 표기, 27 Sep 2024). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] GPU 메모리(DeiT-B → DeiT-S, batch 128): teacher 6499 → 3412 MB, 합계 12925 → 9838 MB (−24%). 배치 크기나 오프로딩에 따라 감소율이 달라진다고 원문이 밝힘. (Table 11, p.27) ← 메모: GPU 메모리 teacher 6499 → 3412 MB, 합계 12925 → 9838 MB (−24%)
  - [수정] teacher forward 1회 GFLOPs: DeiT-B 17.5 → 8.6 (50% 유지). 임베딩·잔차·정규화 연산은 뺀 근사치. (Table 10, p.26) ← 메모: Table 10의 17.5 → 8.6 GFLOPs(DeiT-B). "재대조 안 함"
  - [수정] 25% 유지: DeiT3-S teacher 75.1 → 74.8 (−0.3), DeiT3-B teacher 74.4 → 74.7 (+0.3). teacher마다 기준값이 다름. (Table 2, p.8) ← 메모: 25% 유지는 teacher에 따라 74.7~74.8
  - [수정] [구조] 저자가 "Potential negative societal impact" 문단에서 attention 기반 선택이 데이터의 가짜 상관을 강화할 수 있다고 적음. 실험은 없음. (§6, p.14) ← 메모: [구조] 논문 스스로 한계로 적음: 가짜 상관 강화 가능 (6절)
  - [수정] 옮겨 간 비용: student attention의 head 평균만 N·H FLOPs이며 ViT-G/14 기준 샘플당 4.1 kFLOPs 미만. top-k 정렬·gather 비용은 따로 세지 않음. (§3.2, p.6) ← 메모: 옮겨 간 비용: attention 평균·정렬·선택이 샘플당 4.1 kFLOPs 미만
  - [수정] student → teacher 순서 의존이 생김. 여러 GPU를 쓸 때는 micro-batch 파이프라인으로 유휴 시간을 메울 수 있다고 함. 측정한 시간은 없고 그림(Fig. 8)만 있음. (부록 F, p.23) ← 메모: 병렬화에 micro-batching 필요 (부록 F)
  - [수정] 하드웨어: 처리량(img/s) 측정만 RTX A6000 1장, FP32, batch 128. 학습에 쓴 GPU는 따로 밝히지 않음. (부록 A, p.18) ← 메모: 하드웨어 RTX A6000 1장, FP32, batch 128 (부록 A)
  - [수정] student는 DeiT 레시피로 처음부터 학습. 단 ViT-L·H student는 MAE 체크포인트에서 50 epoch 미세조정. (§4.1, p.7) ← 메모: student는 처음부터 학습
  - [수정] 3 seed 평균. CLIP teacher와 large·huge student 실험은 제외(단일 실행). 오디오(ESC-50)는 5 seed 평균. (p.7, p.18) ← 메모: 3 seed 평균
  - [추가] Table 2에 ConViT-B teacher(DeiT-Ti student, 73.8 → 74.2, 8543 → 5540 PFLOPs)와 Swin-Ti student(DeiT-B teacher, 75% 유지)도 포함. (Table 2, p.8)
  - [추가] DINO 자기증류에 적용(DeiT-S, IN-1k 100 epoch): teacher 총 589 → 384 PFLOPs (66% 유지), 선형 평가 73.7 → 74.2. GPU 4장으로 줄인 설정. (Table 4, p.9–10; 부록 A p.18)
  - [추가] 오디오(AST-S, ESC-50): teacher 92.8 → 75.9 TFLOPs에서 정확도 유지(86.3 → 86.4), 44.0 TFLOPs(50%)에서 −0.6. (Table 3, p.9)
  - [추가] 384px teacher 실험(Table 7)의 FLOPs에는 attention 점수·이미지 보간 비용이 빠짐. 같은 FLOPs라면 224px teacher + vanilla KD 쪽이 낫다고 원문이 밝힘. (부록 C, p.20)
  - [추가] 실제 학습 시간(wall-clock)은 보고 안 됨. 효율은 teacher FLOPs·teacher 처리량·메모리로만 제시. (Table 2, Table 11)
- 메모: 효율을 FLOPs, 처리량, 메모리 세 가지로 확인한 점은 좋음. 다만 −51%는 teacher 연산만의 감소율이라 전체 학습 비용 감소율과 다름. 증강이 약할수록 KD 이득이 큼 (Table 9: DeiT-S 약한 증강 71.3 → 79.7, 강한 증강 79.9 → 80.8).

### 02. DMAE (Masked Autoencoders Enable Efficient Knowledge Distillers) ([arXiv: 2208.12256](https://arxiv.org/abs/2208.12256))
- 효율화 방식: MAE 인코더 중간 층의 특징 출력을 정렬해 증류함. 로짓을 일치시키는 방식은 아님.
- 얻는 것: 전체 이미지·teacher 전체 깊이를 사용하는 증류와 비교해, 보이는 패치만 처리하고 teacher도 정렬 층까지만 실행해 순전파 계산을 줄임.
- 잃는 것:
  - [구조] teacher의 중간 특징과 패치 위치 대응이 필요함. teacher·student이 같은 보이는 패치를 사용하는 정렬 방식. [원문](https://arxiv.org/abs/2208.12256)
- 옮겨 간 비용: student 특징을 teacher 차원에 맞추는 투영층과 특징 정렬 손실. student MAE의 픽셀 복원 학습도 함께 수행.
- 어떻게 쟀나: Training Cost(GPU-hours). teacher의 사전학습 비용은 별도이며, 증류 학습 비용과 구분해 읽기. / teacher forward 셈? Y (증류 사전학습의 GPU 시간에 포함) / 일회성 비용 셈? N (teacher 사전학습 비용 없음) / 하드웨어 = NVIDIA A5000 1장. Table 7은 사전학습·미세조정·합계 GPU 시간을 따로 적음 (MAE-B 100 epoch 78 + 112 = 190시간, 1600 epoch 1248 + 112 = 1360시간). 본문은 제안 방법을 195 GPU시간(대 208)이라고 적는데, 208의 설정은 Table 7에서 다시 확인할 것. (p. 2, p. 8)
- 설정: ImageNet-1K. MAE 기반 특징 증류 외에 DINO·MoCo-v3 학습에도 적용한 실험이 있음.
- 평균 정확도 외 평가: 4.7절에서 여러 seed의 평균과 표준편차를 제시해 성능 향상의 안정성을 확인함.
- 위치: 4.7절 Standard Deviation Analysis.
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 6건 / 수정 5건 / 추가 2건 / 확인 불가 0건. 본 판본: arXiv 2208.12256v2 (첫 쪽 표기 "v2 10 Nov 2022", md5가 arXiv v2와 같음). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] 제안 방법 195 GPU시간(증류 사전학습 83h + 미세조정 112h, 84.0%) 대 Supervised Feature Alignment 208h(미세조정된 ViT-L teacher에서 지도 증류 100 epoch, 82.8%). DeiT-B soft·hard 증류는 213h. 레시피 전체끼리의 비교라서 teacher 층을 줄인 효과만 따로 볼 수는 없음. (Table 7, p.8) ← 메모: 195 대 208 GPU시간. "208의 설정은 Table 7에서 다시 확인할 것"
  - [수정] teacher forward 셈? Y로 보임 (증류 사전학습 83h에 teacher 실행이 들어 있을 것으로 추정, 내역 없음). ← 메모: teacher forward 셈? Y (증류 사전학습 GPU 시간에 포함)
  - [수정] 설정: ImageNet-1K. 기본 teacher는 MAE ViT-L(공개 체크포인트), student는 무작위 초기화 ViT-B. 사전학습 100 epoch, 마스킹 75%. DINO(80.9 → 82.2)·MoCo-v3(81.1 → 82.5)는 예비 실험. (§4.1 p.4, Table 9 p.9) ← 메모: 설정: ImageNet-1K. DINO·MoCo-v3 학습에도 적용
  - [수정] 평균 정확도 외 평가: DMAE 정렬 위치 3가지의 3 seed 평균·표준편차(9번째 층 84.03 ± 0.10 등). 기준선은 seed를 반복하지 않아서 기준선 대비 유의성 검정은 아님. (Table 8, p.9) ← 메모: 4.7절에서 여러 seed의 평균·표준편차로 성능 향상의 안정성을 확인
  - [수정] 위치: Table 4 (기준선, p.7), Table 7 (GPU 시간, p.8), Table 8 (seed 표준편차, p.9), Table 9 (DINO·MoCo-v3, p.9). ← 메모: 위치: 4.7절만
  - [추가] 적은 라벨 평가: IN-1%·10%에서 DMAE-B 50.3·73.4, MAE-B 1600 epoch 49.6·72.8. (Table 6, p.8)
  - [추가] 마스킹 95%에서 83.6, 98%에서 82.4 (ViT-L → ViT-B). 이 설정의 학습 시간은 보고 안 됨. (Fig. 2, p.6)
- 메모: 단일 실행 결과뿐 아니라 표준편차도 제시함. 평가 아이디어: 잡음·흐림 등 손상 강건성 평가를 붙여볼 만함 (이 논문이 강건성 손실을 보였다는 뜻은 아님).

### 03. Dyna-DINO ([arXiv: 2606.19483](https://arxiv.org/abs/2606.19483))
- 효율화 방식: teacher의 최종 층을 처음부터 목표로 삼지 않고, 앞쪽 층을 충분히 학습한 뒤 중간 층과 마지막 층으로 증류 목표를 이동함.
- 얻는 것: 학습 FLOPs와 실제 학습시간 감소.
- 잃는 것:
  - [구조] CKA와 목표 층 전환에 teacher의 중간 특징이 필요함. 최종 출력만 제공하는 teacher에는 그대로 적용할 수 없음. [공식 코드](https://github.com/KevinZ0217/Dyna-DINO)
- 옮겨 간 비용: CKA 유사도 계산, 목표 teacher 층 전환 판단, 특징 차원을 맞추는 투영층.
- 어떻게 쟀나: FLOPs, 실제 학습시간, 이미지 분류 정확도. / teacher forward 셈? Y (보고한 절감률 자체가 teacher 연산 오버헤드의 감소분, Table 1 캡션) / 일회성 비용 셈? N / 하드웨어 = NVIDIA L40S·RTX A6000·L40 등이 섞인 클러스터 (p. 5). 수치: ViT-G → ViT-S, ImageNet-100 100 epoch에서 FLOPs −25.1%, 학습 시간 −21% (Table 1, p. 6). 분모가 teacher 오버헤드라 전체 학습 비용 절감률과 다름. 여러 종류의 GPU에서 잰 시간이라 시간 비교에 주의.
- 설정: 의미 영역 분할은 ADE20K, 인스턴스 검색은 Oxford·Paris 데이터셋에서 평가.
- 평균 정확도 외 평가: 의미 영역 분할: 선형 영역 분할, 인코더 전용 마스크 트랜스포머(EOMT), 다중 스케일(MS) 추론의 세 가지 프로토콜 사용. 인스턴스 검색: 백본에서 이미지별 전역 임베딩을 추출하고, 질의 이미지와 데이터베이스 이미지의 유사도순으로 검색 결과를 정렬해 평균 정밀도(mAP)를 계산.
- 위치: 4.3절의 의미 영역 분할·인스턴스 검색 평가.
- **정정 (검수 보고서 근거)**: 시간 절감의 분모가 원문 안에서도 다름. 표 1 캡션은 FLOPs와 시간을 teacher 오버헤드 절감으로, §4.2는 ViT-Tiny의 22.5%를 총 학습 시간 절감으로 부름. 어느 범위인지 확정해 인용하지 않는다. [표 1·§4.2](https://arxiv.org/pdf/2606.19483v2#page=6)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 7건 / 수정 4건 / 추가 3건 / 확인 불가 0건. 본 판본: arXiv 2606.19483v2 (md5가 arXiv v2와 같음). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] 효율화 방식: teacher 첫 블록부터 목표로 삼고, student 출력과 현재 목표 층의 online CKA가 τ 이상이거나 대기 epoch이 E_max에 이르면 다음 블록으로 넘어감. 아직 목표가 아닌 뒤쪽 teacher 블록은 실행하지 않음. (Algorithm 1, p.5) ← 메모: 앞쪽 층을 충분히 학습한 뒤 중간 층·마지막 층으로 목표를 옮김
  - [수정] 하드웨어: L40S·RTX A6000·L40·RTX 2080 Ti 혼합 클러스터. 실험 1회 비용은 IN-100 12–14 L40S시간, IN-1K 300–400 L40S시간. 시간 절감률을 어느 GPU에서 쟀는지는 밝히지 않음. (§4.1, p.5) ← 메모: 하드웨어: L40S·RTX A6000·L40 등이 섞인 클러스터. 여러 종류 GPU에서 잰 시간이라 시간 비교에 주의
  - [수정] 표 1 캡션은 teacher 오버헤드 절감으로, §4.2는 총 학습 시간 절감으로 부름. 범위를 정하지 않고 두 표현을 함께 적음. (Table 1, §4.2, p.6) ← 메모: 분모가 teacher 오버헤드라 전체 학습 비용 절감률과 다름
  - [추가] ViT-G → ViT-Tiny, IN-100: FLOPs −28.82%, 시간 −22.5%. (Table 1, p.6)
  - [추가] ImageNet-1K에서는 절감 폭이 작음: ViT-S FLOPs −9.44%·시간 −4.83%, ViT-Tiny −12.4%·−7.5% (CKA 0.8). 저자도 "modest"라고 씀. (Table 6, p.9)
  - [추가] 설정: ImageNet-100(절제 실험)·ImageNet-1K에서 증류. teacher는 DINOv2 ViT-G(registers 포함), student는 무작위 초기화 ViT-S·ViT-Tiny. LARS, 전체 batch 256. 다운스트림은 ADE20K 분할, revisited Oxford·Paris 검색. (§4.1, p.5)
  - [수정] 위치: Table 1 (IN-100 효율, p.6), Table 6 (IN-1K 효율, p.9), Table 2–5 (검색·분할, p.7–8), §5 (한계, p.10). ← 메모: 위치: 4.3절의 분할·검색 평가
- 메모: 효율 감소분은 현재 목표 층 뒤의 teacher 연산 생략에서 발생. CKA·투영층을 포함한 전체 시간과 teacher 순전파 FLOPs를 구분해 비교.

### 04. Asymmetric Masked Distillation (AMD) ([arXiv: 2311.03149](https://arxiv.org/abs/2311.03149))
- 효율화 방식: student보다 teacher의 마스킹 비율을 낮춰 더 많은 문맥을 보게 함. 공통 패치는 직접 정렬하고 teacher만 보는 패치는 생성 정렬.
- 얻는 것: 무마스킹 특징 증류 대비 teacher 토큰 계산 감소. SSV2 200-epoch 비교에서 무마스킹 32시간, teacher 45%·student 90% 마스킹 AMD 19시간(Table 7).
- 잃는 것:
  - [구조] student이 보는 패치는 teacher가 보는 패치의 부분집합이어야 함. teacher·student 마스크를 서로 독립적으로 정하는 방식은 아님. [§3.3](https://arxiv.org/html/2311.03149v2#S3.SS3)
- 옮겨 간 비용: 특징 생성기·정렬 손실. 대칭 마스킹보다 teacher 처리 토큰은 많음.
- 어떻게 쟀나: 실제 학습시간, epoch, teacher·student 마스킹 비율. 위 시간은 Table 7 안의 비교. / teacher forward 셈? Y (학습 전체의 실제 시간) / 일회성 비용 셈? N (teacher 사전학습 비용 없음) / 하드웨어 = A100 16장에서 200 epoch을 학습하며 시간 측정 (Table 1 캡션, p. 6). 사전학습 실험은 A100 80 GB 32장 (부록 B, p. 9). 보고한 시간은 16장에서의 경과 시간으로 보이며 GPU 시간(장수 × 시간)이 아님.
- 설정: SSV2·Kinetics-400 영상, ImageNet-1K 이미지. MAE 기반 teacher·student.
- 평균 정확도 외 평가: AVA 행동 탐지 mAP, UCF101·HMDB51 전이, 행동 종류별 결과, 평균 어텐션 거리.
- 위치: Table 1·7, 부록 Figure 4·5. [원문](https://arxiv.org/html/2311.03149v2)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 10건 / 수정 2건 / 추가 3건 / 확인 불가 0건. 본 판본: arXiv 2311.03149v2 (md5가 arXiv v2와 같음). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] Table 7의 시간은 하드웨어가 캡션에 없음. 부록 B의 "절제 실험은 GPU 16장" 문장으로 추정한 것이며, Table 1만 A100 16장을 명시함. (p.6, p.9) ← 메모: 위 시간(Table 7)도 같은 하드웨어로 읽음
  - [수정] 메모: Table 7은 무마스킹(양쪽 0%) 기준선과의 비교이고, Table 1(c)는 student 90%를 고정하고 teacher 비율만 바꾼 비교임. 32 → 19시간은 무마스킹 대비로만 사용. ← 메모: 메모: Table 7 무마스킹 기준선과 Table 1의 대칭·비대칭 마스킹 비교는 기준이 다름
  - [추가] teacher 마스킹 비율에 따른 시간(A100 16장, 200 epoch): 85% 12.08h, 75%(기본) 13.33h, 60% 15.47h, 45% 18.93h. 정확도는 72.17 → 72.48. (Table 1c, p.6)
  - [추가] 생성 정렬의 부가 비용: 직접 정렬만 10.83h, 직접 + 생성 13.33h(+23%). 생성기 깊이 1·2·4층에서 12.92·13.33·15.33h. (Table 1a·1d, p.6)
  - [추가] AMD 안에서 비교(student 90% 고정): teacher 0% 26h·72.1%, 45% 19h·72.5%. teacher 25%는 72.3%. (Table 7 p.8, 부록 F p.11)
- 메모: Table 7의 무마스킹 기준선과 Table 1의 대칭·비대칭 마스킹 비교는 기준이 다름. 32→19시간은 무마스킹 대비 수치로만 사용.

### 05. MaskAlign ([arXiv: 2211.08887](https://arxiv.org/abs/2211.08887))
- 효율화 방식: student이 보는 패치의 특징과 teacher가 전체 이미지에서 추출한 대응 특징 사이의 일관성을 학습함. 동적 정렬 모듈(DA)을 사용하며 픽셀 복원 디코더를 제거함.
- 얻는 것: 픽셀 복원 디코더의 계산 제거.
- 잃는 것:
  - [실측·외부] COCO 물체 특징 분류에서 배경을 교체하면 Top-1 77.4→53.8%. 마스킹 자체가 원인인지를 분리한 실험은 아님. [Mind Your Augmentation, Table 3](https://proceedings.iclr.cc/paper_files/paper/2024/file/040321891311e31134a1b3844efb6a9a-Paper-Conference.pdf)
  - [구조] 보이는 패치 위치에 해당하는 teacher 특징을 받아야 함. 최종 분류 로짓만 반환하는 teacher로는 같은 특징 정렬을 수행할 수 없음. [공식 코드](https://github.com/OpenDriveLab/maskalign)
- 옮겨 간 비용: 전체 이미지의 teacher 특징 추출, 특징 정규화, 동적 정렬 모듈(DA)의 학습·계산.
- 어떻게 쟀나: 학습 epoch, 시간(Time), 미세조정 정확도(FT Acc. %), 탐지·분할 성능(APbox·APmask). / teacher forward 셈? Y로 보임 (사전학습 속도 비교, 내역 없음) / 일회성 비용 셈? N (CLIP teacher 사전학습 비용 없음) / 하드웨어 = 보고 안 됨. Table 7의 시간은 절대값이 아니라 상대 배수임: 제안 200 epoch 1.0배, BEiT v2 300 epoch 2.8배, MILAN 400 epoch 3.2배 (CLIP-ViT-B/16 teacher 기준, p. 7).
- 설정: COCO에서 객체 탐지와 인스턴스 분할을 평가.
- 평균 정확도 외 평가: 객체 탐지 APbox, 인스턴스 분할 APmask. 부록의 선형 프로빙·의미 영역 분할·배경 관련 평가도 확인.
- 위치: 4.2절 COCO 평가, 부록 A 선형 프로빙, 부록 B 의미 영역 분할, 부록 C 배경 관련 평가.
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 6건 / 수정 5건 / 추가 2건 / 확인 불가 0건. 본 판본: arXiv 2211.08887v2 (md5가 arXiv v2와 같음). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] 얻는 것: 복원 디코더를 없애고 student 인코더는 패치 30%만 처리함(마스킹 70%). teacher는 전체 이미지를 처리함. (Table 1, p.5) ← 메모: 얻는 것: 픽셀 복원 디코더의 계산 제거
  - [수정] [실측·외부] COCO 물체 특징 k-NN에서 선택한 상자 밖 영역을 무작위 배경으로 바꾸면 Top-1 77.4 → 53.8%. 같은 표의 DenseCL(72.5 → 41.5)·iBOT(75.2 → 45.8)도 비슷하게 떨어지고, 비교한 기존 방법 중 MaskAlign이 가장 높음. MaskAlign만의 손실 근거로 쓰지 않음. (Mind Your Augmentation, Table 3, p.8) ← 메모: [실측·외부] COCO 물체 특징 분류에서 배경을 바꾸면 Top-1 77.4 → 53.8%
  - [수정] teacher forward 셈? Y (teacher 추론 시간 때문에 가속이 선형이 아니라고 본문에 적힘, 내역은 없음). 일회성 비용 셈? N. (p.7) ← 메모: teacher forward 셈? Y로 보임 (내역 없음)
  - [수정] 설정: ImageNet-1K 사전학습, student ViT-B/16(ViT-L/16 포함), 고정 teacher CLIP-B/16(DINO-B·CLIP-L 비교), 200 epoch, attentive 마스킹 70%, batch 1024. 다운스트림은 COCO Mask R-CNN(ViTDet). (§4.1 p.4) ← 메모: 설정: COCO 탐지·인스턴스 분할 평가
  - [추가] 원논문 자체 배경 강건성(ImageNet-9, 선형 프로빙): No-FG 51.9, Mixed-Rand 78.6, Mixed-Next 76.6. MILAN은 46.8·77.9·74.7. (Table 10, p.11)
  - [수정] 위치: Table 1 (Forward Ratio, p.5), Table 2 (COCO, p.6), Table 7 (사전학습 속도, p.7), 부록 A–C (Table 8–10, p.11). ← 메모: 위치: 4.2절 COCO, 부록 A·B·C
  - [추가] 마스킹 비율 절제(Table 4)는 반복 수를 70% 마스킹 200 epoch에 맞춤. 정확도는 20–80%에서 85.1–85.3으로 평평함. (Table 4, p.6)
- 메모: student이 처리하는 토큰·복원 디코더 연산과 teacher의 전체 이미지 특징 추출 비용을 나눠 기록. 외부 배경 교체 결과는 MaskAlign 원논문의 실험과 구분.

### 06. Decoupled KD (DKD) ([arXiv: 2203.08679](https://arxiv.org/abs/2203.08679))
- 효율화 방식: 정답 클래스 지식 증류(TCKD)와 비정답 클래스 지식 증류(NCKD)를 분리하고, 각 손실의 학습 기여도를 따로 조절함. 기존 KD에서 teacher의 정답 확신도에 따라 작아지던 NCKD의 계수를 독립적인 가중치로 바꿈.
- 얻는 것: 특징 정렬 모듈·메모리 뱅크를 사용하는 증류보다 단순한 로짓 손실로 학습. teacher 입력이나 실행 깊이를 줄이는 방식은 아님.
- 잃는 것:
  - [실측·외부] WRN-40-2→ResNet-20, CIFAR-100-C에서 student 단독 대비 ECE 0.2258→0.2379(3-seed 평균, 낮을수록 좋음). 통계적 유의성은 확인되지 않음. [CSV](https://github.com/Evowind/confident-dunce/blob/main/figures/seeds_summary.csv)
  - [구조·정의] 이진 분류에서는 정답을 제외하면 클래스가 하나뿐이므로 NCKD의 KL 항은 0이 됨. 다중 비정답 클래스 사이의 관계를 전달할 수 없음. [NCKD 정의](https://arxiv.org/abs/2203.08679)
- 옮겨 간 비용: TCKD·NCKD의 별도 확률 분포와 손실 계산, 두 손실의 가중치 설정.
- 어떻게 쟀나: 분류 정확도와 학습시간·추가 파라미터 비교. teacher·student 전체 실행을 줄이는 방법과 KD 손실 오버헤드 비교를 구분. / teacher forward 셈? 배치당 학습 시간을 "데이터 처리 + 순전파·역전파 + 메모리 갱신"의 합으로 정의하나 teacher 포함 여부는 명시 없음 (부록 A.7, p. 12–13) / 일회성 비용 셈? N / 하드웨어 = 보고 안 됨. 단위는 배치당 학습 시간(ms)과 추가 매개변수 수 (Figure 2, p. 7).
- 설정: 특징 전이 실험에서는 CIFAR-100에서 WRN-40-2 teacher로 증류한 WRN-16-2 student을 특징 추출기로 사용.
- 평균 정확도 외 평가: 특징 추출기를 고정하고 STL-10·Tiny-ImageNet에서 선형 프로빙을 수행해 특징의 전이 가능성을 평가.
- 위치: 4.2절 추가 분석의 특징 전이 실험.
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 7건 / 수정 3건 / 추가 3건 / 확인 불가 1건. 본 판본: arXiv 2203.08679v2 (PDF 첫 쪽 표기). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [확인 불가] 확인 불가: CSV와 student 설정 파일만으로는 손상 데이터셋 이름을 확인하지 못함.
  - [추가] 같은 CSV에서 vanilla KD는 0.3321이라서 DKD는 KD보다 보정이 좋고 student 단독보다만 나쁨. 기준을 함께 적음.
  - [추가] 학습 비용(CIFAR-100, ResNet32×4 → 8×4): DKD 11 ms/배치·추가 파라미터 0으로 KD(11 ms)와 같음. ReviewKD 26 ms·1.8M, CRD 41 ms·12.3M. (Figure 2, p.7)
  - [수정] 설정: CIFAR-100, ImageNet(ResNet-34 → ResNet-18, ResNet-50 → MobileNet-V1), MS-COCO 검출(Faster R-CNN-FPN). 특징 전이는 CIFAR-100에서 WRN-40-2 → WRN-16-2로 증류한 특징 추출기를 고정하고 마지막 GAP 특징에 선형층만 학습. (§4, p.5–8; 부록 A.7 p.13) ← 메모: 설정: 특징 전이 실험에서 WRN-40-2 → WRN-16-2를 특징 추출기로 씀
  - [수정] 평균 정확도 외 평가: STL-10·Tiny-ImageNet 선형 프로빙(DKD 72.9·37.1, KD 70.9·33.9), COCO 검출 AP, t-SNE, 로짓 상관행렬 차이. (Table 13 p.8, Table 10 p.7, Fig. 3·4 p.8) ← 메모: 평균 외: STL-10·Tiny-ImageNet 선형 프로빙
  - [수정] 위치: Figure 2 (학습 시간·파라미터, p.7), Table 10 (COCO, p.7), Table 13 (특징 전이, p.8), 부록 A.7 (측정 정의, p.12–13). ← 메모: 위치: 4.2절 특징 전이 실험
  - [추가] [실측·원논문] 검출(COCO, R-101 → R-18)에서 DKD 단독 AP 35.05로 ReviewKD 36.75보다 낮음. 저자가 로짓 방법은 위치 정보를 전달하지 못한다고 한계로 적음. (Table 10 p.7, §5 p.8)
- 메모: NCKD는 정답 클래스를 제외한 클래스들의 예측 분포에 담긴 지식을 다룸. 기존 KD에서는 teacher의 정답 확신도가 높을수록 이 부분의 기여가 작아지는 점을 분리해 다룸.

### 07. Information Theoretic Representation Distillation (ITRD) ([arXiv: 2112.00459](https://arxiv.org/abs/2112.00459))
- 효율화 방식: 특징의 상관·상호정보량 기반 손실 사용. 대조 증류의 별도 메모리 뱅크·음성 샘플 저장 없이 계산.
- 얻는 것: 메모리 뱅크 기반 표현 증류 대비 저장·샘플 관리 부담 감소.
- 잃는 것:
  - [구조] teacher·student의 중간 특징을 배치 단위로 받아 상관·Gram 행렬을 계산함. 로짓만 제공하는 teacher API에는 그대로 적용할 수 없음. [공식 코드](https://github.com/roymiles/ITRD)
- 옮겨 간 비용: 배치 내 상관·Gram 행렬 계산과 student 특징 투영층.
- 어떻게 쟀나: 추가 연산·메모리 부담. 배치 크기와 특징 차원에 따른 행렬 크기를 구분. / teacher forward 셈? Y (학습 전체의 처리량으로 측정) / 일회성 비용 셈? N / 하드웨어 = RTX 2080 Ti 1장. 메모리 8 GB 제약에 맞게 방법마다 배치 크기를 조정함 (p. 9). 단위는 학습 처리량(img/s)이고 총 학습 시간에 반비례한다고 설명.
- 설정: CIFAR-100·ImageNet, 여러 CNN 조합. 이진 신경망 증류도 실험.
- 평균 정확도 외 평가: CIFAR-100 특징을 고정한 STL-10·Tiny-ImageNet 선형 프로빙.
- 위치: 손실 정의, 표현 전이 Table 4. [원문](https://arxiv.org/abs/2112.00459)
- **정정 (검수 보고서 근거)**: STL-10 72.7%, Tiny-ImageNet 36.0% 선형 평가는 v2 표 4에만 있음. 저장소 PDF(v3)에는 그 표가 없으므로 인용 시 v2를 명시. [ITRD v2 표 4](https://arxiv.org/pdf/2112.00459v2#page=13)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 6건 / 수정 4건 / 추가 2건 / 확인 불가 0건. 본 판본: arXiv 2112.00459v3 (PDF 첫 쪽 표기). 정정 줄 확인용으로 v2도 받음. 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] [구조] teacher·student의 최종 FC 직전 표현을 배치 단위로 받아 상관·Gram 행렬을 계산함. 로짓만 주는 teacher API에는 그대로 못 씀. (§3, p.3–4) ← 메모: [구조] teacher·student 중간 특징을 배치 단위로 받음
  - [추가] ImageNet(ResNet-34 → 18) 학습 처리량은 그래프로만 제시됨. 그래프에서 읽으면 ITRD ≈530 img/s로 KD(≈540)와 비슷하고 CRD(k=16384, ≈130)보다 큼. 근사 판독값임. (Figure 2, p.9)
  - [수정] 설정: CIFAR-100·ImageNet 분류, CIFAR-10 이진 신경망(ResNet-18), SQuAD 1.1 질의응답(BERT 12층 → 6·3층). (Table 1 p.8, Table 3 p.10) ← 메모: 설정: CIFAR-100·ImageNet, 여러 CNN. 이진 신경망 증류도
  - [수정] 위치: 손실 정의 §3–4 (v3), 학습 효율 Figure 2 (v3 p.9), 표현 전이 Table 4는 v2 p.13에만 있음. ← 메모: 위치: 표현 전이 Table 4
  - [수정] 메모: 원문 처리량 비교는 배치 크기를 고정하지 않고 8 GB 메모리 제약에 맞춰 방법마다 조정한 결과임. 배치를 고정한 비교는 원문에 없음. ← 메모: 메모: 연산 비교 시 배치 크기·특징 차원 고정
  - [추가] ImageNet ResNet-34 → ResNet-18: top-1 error 28.32%. 더 비싼 CRCD·HSAKD 다음 순위. (p.9)
- 메모: 연산을 비교할 때 배치 크기와 특징 차원을 고정해야 함. 상관행렬·Gram 행렬 계산과 특징 투영층도 KD 오버헤드에 포함.

### 08. Understanding the Role of the Projector in KD ([arXiv: 2303.11098](https://arxiv.org/abs/2303.11098))
- 효율화 방식: 선형 프로젝터·배치 정규화·LogSum 거리로 특징 증류. 별도 메모리 뱅크나 명시적 관계 구조를 만들지 않음.
- 얻는 것: 복잡한 관계 기반 증류 모듈 대신 작은 투영층 사용.
- 잃는 것:
  - [실측·원논문] 정규화 비교에서 프로젝터 가중치의 일부 특이값이 학습 중 0에 가까워지는 현상을 측정함. 특정 인식 능력 손실을 별도로 측정한 결과는 아님. [Figure 2](https://arxiv.org/html/2303.11098v5)
  - [실측·원논문] 큰 MLP 프로젝터에서 입력·출력 특징의 상관이 더 낮아지는 현상을 측정함. [Figure 3](https://arxiv.org/html/2303.11098v5)
- 옮겨 간 비용: 프로젝터 학습·배치 정규화·특징 손실 계산.
- 어떻게 쟀나: 손실 구성의 연산·메모리 규모와 배치 크기·특징 차원. / teacher forward 셈? N / 일회성 비용 셈? N / 하드웨어 = NVIDIA RTX A5000 1장 (p. 5). 학습 시간이나 연산량의 실측 수치는 본문에서 찾지 못함.
- 설정: CIFAR-100·ImageNet 분류, CNN→ViT 증류, COCO 탐지.
- 평균 정확도 외 평가: 특이값 변화, 프로젝터 입력·출력 상관, 객체 탐지 AP.
- 위치: Figure 2·3, 다운스트림 평가. [원문](https://arxiv.org/html/2303.11098v5)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 7건 / 수정 3건 / 추가 2건 / 확인 불가 0건. 본 판본: arXiv 2303.11098v5 (PDF 첫 쪽 표기, AAAI 2024). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [수정] 어떻게 쟀나: 비용 측정 없음. 효율은 구조 설명과 초록·결론의 주장으로만 제시. 정확도 벤치마크만 있음. (p.1, p.7) ← 메모: 어떻게 쟀나: 손실 구성의 연산·메모리 규모와 배치 크기·특징 차원
  - [수정] teacher forward 셈? 해당 없음(비용 측정 없음) / 일회성 비용 셈? 해당 없음. ← 메모: teacher forward 셈? N / 일회성 비용 셈? N
  - [추가] 평균 외 평가: 평행이동 등변성 μT (DeiT-S 1.52, CivT-S 0.13, 제안 0.04, 낮을수록 등변). (Table 7, p.7)
  - [추가] COCO 검출: Faster R-CNN R50 → MobileNetV2 mAP 29.47 → 32.92(ReviewKD 33.71). YOLOv5m → s 56.8 → 57.3. 표의 열 이름(mAP 50-95·mAP 50)과 값의 순서가 맞지 않아 보이므로 인용 시 주의. (Table 8, p.7)
  - [수정] 위치: Figure 2 (p.3), Figure 3 (p.4), Table 1 (정규화 절제, p.4), Table 7·8 (등변성·COCO, p.7), 하드웨어 p.5. ← 메모: 위치: Figure 2·3, 다운스트림 평가
- 메모: 특이값·입출력 상관은 프로젝터 내부 분석 지표. 이를 작은 물체 인식·강건성 등 특정 능력의 손실로 바꾸어 적지 않음.

### 09. Parameter-Efficient and Student-Friendly Knowledge Distillation (PESF-KD) ([원문](https://arxiv.org/abs/2205.15308))
- 효율화 방식: 고정된 teacher 본체 대신 작은 어댑터만 갱신해 teacher를 student에 맞게 적응시킴.
- 얻는 것: teacher 전체를 갱신하는 FT-KD 대비 적은 갱신 파라미터. ImageNet Table 3 batch time: FT-KD 1.44, PESF-KD 0.47, vanilla KD 0.46(표의 단위 그대로).
- 잃는 것:
  - [구조] teacher에 어댑터를 넣고 학습해야 함. 수정할 수 없는 고정 teacher API만으로는 해당 teacher 적응을 수행할 수 없음. [저자 코드](https://github.com/1170300319/PESF-KD)
- 옮겨 간 비용: 어댑터 학습·teacher 순전파. frozen-teacher KD에 없던 teacher 적응 비용이 추가됨.
- 어떻게 쟀나: 갱신 파라미터 수, batch time. FT-KD 대비 절감과 vanilla KD 대비 비용을 구분. / teacher forward 셈? · 일회성 비용 셈? · 하드웨어 = 확인 안 함 (PDF가 저장소에 없어 이번 보강에서 제외).
- 설정: CIFAR-100·ImageNet. ImageNet ResNet50→ResNet18. NLP GLUE도 포함.
- 평균 정확도 외 평가: teacher·student KL divergence, 예측 일치율(Agree), 논문 정의의 PL·Gap.
- 위치: Figure 1, Table 3, 일관성 분석. [저자 코드](https://github.com/1170300319/PESF-KD)
- **정정 (검수 보고서 근거)**: 링크된 arXiv 판은 prediction KL, CKA, sharpness GAP를 분석함. 비용·평가 칸을 이 판 기준으로 다시 볼 것. [PESF-KD §4](https://arxiv.org/pdf/2205.15308v1#page=6)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 7건 / 수정 2건 / 추가 4건 / 확인 불가 0건. 본 판본: arXiv 2205.15308v1 (arXiv에 v1만 있음. `tools/shrink/arxiv/pesf_v1.pdf`). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [추가] 같은 표: 갱신 파라미터 vanilla KD 47M, FT-KD 149M, PESF-KD 54M. top-1 69.20 / 70.00 / 69.96. batch time 단위는 표에 없음. (Table 3, p.8)
  - [수정] teacher forward 셈? 명시 없음(학습 batch time이라 포함으로 보임) / 일회성 비용 셈? N (시작 teacher의 학습 비용 없음) / 하드웨어 = 부록에 8× RTX 3090이라고만 적음. Table 3 측정에 쓴 장수와 batch time 단위는 명시 없음. (부록 p.14) ← 메모: teacher forward 셈? · 일회성 · 하드웨어 = 확인 안 함
  - [수정] 평균 정확도 외 평가: teacher-student 예측 KL, CKA, sharpness GAP. Table 3 PESF-KD는 KL 8.9, GAP 18.6이며 본문 §5.3.2는 19.2로 적어 서로 다름. (p.6, Table 3 p.8, p.9) ← 메모: 평균 외: KL, 예측 일치율(Agree), PL·Gap
  - [추가] CIFAR-100 vgg13 → vgg8: 갱신 파라미터 ∼54M → ∼16M. (§5.3.1, p.9)
  - [추가] 어댑터 구조 비교(VGG, CIFAR-100): batch time 어댑터 0.137, LoRA 0.166, Scaled PA 0.165. (Table 5, p.10)
  - [추가] GLUE(Table 4)도 학습 비용이 vanilla KD 다음으로 낮다고 적음. vanilla KD보다 싸다는 근거는 없음. (§5.3.3, p.10)
- 메모: FT-KD 대비 비용 절감 사례. 같은 표의 vanilla KD 0.46과 PESF-KD 0.47을 근거로 vanilla KD보다 빠르다고 쓰지 않음.

### 10. Knowledge Distillation from A Stronger Teacher (DIST) ([원문](https://arxiv.org/abs/2205.10536))
- 효율화 방식: 예측 확률의 정확한 일치 대신 클래스 간·샘플 간 Pearson 상관을 정렬.
- 얻는 것: 특징 정렬 모듈·메모리 뱅크 없이 로짓에서 계산. 논문은 vanilla KD와 거의 같은 학습 비용이라고 설명.
- 잃는 것:
  - [실측·외부] CLIP ViT-L/14→ViT-B/16에서 일반 KD 대비 ImageNet-R 51.5→40.8%, Sketch 34.7→26.9%. [외부 Table 3](https://arxiv.org/html/2311.01441v2#S4)
  - [실측·외부] 같은 실험에서 ImageNet-C mCE 61.5→65.8(낮을수록 좋음), Stylized-ImageNet 정확도 16.2→12.7%. [같은 표](https://arxiv.org/html/2311.01441v2#S4)
- 옮겨 간 비용: 클래스·배치 방향의 상관 계산. 강한 teacher를 쓰는 경우 teacher 순전파 비용도 별도 고려.
- 어떻게 쟀나: 원논문의 비용 설명과 다운스트림 결과. vanilla KD 대비 정량 가속률을 임의로 부여하지 않음. / teacher forward 셈? · 일회성 비용 셈? · 하드웨어 = 확인 안 함 (PDF가 저장소에 없어 이번 보강에서 제외).
- 설정: CIFAR-100·ImageNet, 강한 teacher와 다양한 student. 객체 탐지·분할에도 적용.
- 평균 정확도 외 평가: 탐지 AP, Cityscapes mIoU. 외부 연구는 ImageNet-C·R·Sketch·A·Stylized-ImageNet 평가.
- 위치: §1·3, 탐지·분할 평가. 외부 강건성 Table 3. [공식 코드](https://github.com/hunto/DIST_KD)
- **정정 (검수 보고서 근거)**: 학습 처리량 표가 있음: 8×V100, ResNet-34 → 18에서 DIST 14.19, vanilla KD 14.28 batches/s. "비용 보고 없음"이 아님. [DIST 표 11](https://arxiv.org/pdf/2205.10536v3#page=14)
- **원문 대조 (2026-11-19, 서브에이전트가 PDF와 한 줄씩 대조)**: 일치 8건 / 수정 2건 / 추가 3건 / 확인 불가 0건. 본 판본: arXiv 2205.10536v3 (PDF 첫 쪽 표기. `tools/shrink/arxiv/dist_v3.pdf`). 아래 항목이 위 서술과 다르면 **아래가 맞다**.
  - [추가] 비교 대상 비용: CRD는 ImageNet 전체 128-d 특징 메모리 뱅크와 추가 260M FLOPs가 필요하다고 원문이 적음. (§4.2, p.7)
  - [추가] 같은 실험에서 분포 내 ImageNet도 78.6 → 76.6, V2 67.2 → 63.9, ImageNet-A 16.0 → 11.2로 떨어짐. 분포 밖 하락이 분포 내 하락과 함께 나타나므로 강건성만의 손실로 읽지 않음. (Distilling OOD Robustness, Table 3, p.8)
  - [수정] teacher forward 셈? Y로 보임(학습 속도 전체, 내역 없음) / 일회성 비용 셈? N / 하드웨어 = NVIDIA V100 8장. 단위 batches/s. (Table 11, p.14) ← 메모: teacher forward 셈? · 일회성 · 하드웨어 = 확인 안 함
  - [추가] 탐지(COCO, Faster R-CNN-R50): DIST 40.4 AP로 KD 39.7보다 높고 CWD 41.7보다 낮음. 41.8은 FPN 특징 MSE를 더한 DIST + mimic의 값. 분할(Cityscapes): DeepLabV3-R18 77.10, PSPNet-R18 76.31 mIoU. (Table 6·7, p.8)
  - [수정] 위치: §1 (비용 주장, p.2), §3 (손실, p.5), Table 6·7 (탐지·분할, p.8), Table 11 (학습 속도, p.14), 외부 Table 3 (2311.01441v2, p.8). ← 메모: 위치: §1·3, 탐지·분할 평가, 외부 Table 3
- 메모: 강한 teacher를 사용하는 비용과 DIST 손실 자체의 비용을 분리. 외부 강건성 수치는 CLIP ViT-L/14→ViT-B/16이라는 해당 설정에 한정.

## 축 분류 메모

- 06~10번(DKD, ITRD, Projector, PESF-KD, DIST)은 teacher forward를 줄이지 않고 KD 손실·부가 모듈의 오버헤드를 줄이거나 비교하는 논문임. Shrink("teacher 입력·연산 축소")에 두는 게 맞는지 다시 정해야 함.
- 09번 PESF-KD, 10번 DIST는 `1_Shrink` 폴더와 읽기 목록에 아직 없음. 넣을지 정하고, 넣으면 PDF와 폴더 README도 추가.
- 06번 DKD의 [실측·외부] ECE 수치는 논문이 아닌 개인 GitHub 저장소의 CSV에서 온 값임. 논문에 인용하기에는 근거가 약함.
