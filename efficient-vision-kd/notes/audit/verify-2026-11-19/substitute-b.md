# 원문 대조 보고서: 4_Substitute 07–12

- 대상 메모: `notes/4_Substitute.md` 07–12번
- 원문: `Related Work/Efficient KD papers/4_Substitute/07_*.pdf` – `12_*.pdf` (저장소 PDF, 쪽 = 물리 쪽, 첫 장 = 1)
- 텍스트 추출: pymupdf, 쪽 표시 삽입 (`verify/tools/txt_b/NN.txt`). 표는 텍스트에서 행·열을 다시 맞춰 읽음.
- "(계산)"은 원문 수치로 내가 계산한 값. 원문에 적힌 값이 아님.

---

## Sub-07 Snapshot Distillation
- 본 판본: arXiv 1812.00123v1 (PDF p.1 하단, 1 Dec 2018)
- 대조 결과: 일치 9건 / 수정 4건 / 추가 4건 / 확인 불가 1건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 순환 학습률, 각 주기 마지막 스냅샷이 다음 주기 teacher | "The last iteration in each mini-generation serves as the teacher of all iterations in the next mini-generation. ... there are K − 1 teachers." | 초록 p.1, §3.3.4 p.5 | 일치 | — |
| 2 | 추가 학습 시간 약 1/3 | "SD requires merely 1/3 extra training time beyond the baselines" / "With around 1/3 extra training time". 측정 표·데이터셋별 값·내역 없음 | §1 p.2, 결론 p.8 | 일치 | — |
| 3 | K세대 방식은 K배 | "A training process with one teacher and K students requires K× more training time compared to a single model." | §1 p.1 | 일치 | — |
| 4 | "표에서는 손실 없음" | CIFAR-100 Table 3, DenseNet190: BL 최종 16.82%, SD(T=2) 최종 18.02%, SD(T=3) 18.65%. 최저 오류(best epoch)는 SD 16.06% vs BL 16.69%. 본문: "the final error rate goes up 18.02%" | Table 3 p.6, §4.1.2 p.7 | 수정 | 손실 없음은 ImageNet Table 4 한정. CIFAR-100 DenseNet190은 최종 epoch 기준 SD가 더 나쁨: 오류 BL 16.82% vs SD 18.02%(T=2)·18.65%(T=3). 가장 좋은 epoch 기준으로만 16.06% vs 16.69%로 앞섬. (Table 3 p.6, p.7) |
| 5 | ImageNet top-1 오류 R101 21.62→21.25, R152 21.17→20.93 | 표 값 같음 | Table 4 p.8 | 일치 | — |
| 6 | 이득 폭 0.3%p 안팎 | 본문: "top-1 ... drop by 0.37%" (R101), "0.26%" (R152). 표 값으로는 21.17−20.93 = 0.24 (계산) | p.8 | 일치 | (참고) R152 이득은 본문 0.26%p, 표 차이 0.24%p로 원문 안에서 어긋남. |
| 7 | 옮겨 간 비용: 이전 스냅샷 forward를 매 스텝 함. 이것이 1/3의 원인 | teacher는 첫 mini-generation에는 없음 ("For l ⩽ L′1 ... λT = 0"). 1/3의 내역은 원문에 없음 | §3.3.4 p.5, p.2 | 수정 | 이전 스냅샷 forward는 두 번째 mini-generation부터만 듦 (ImageNet은 뒤 45 epoch, CIFAR-100은 4개 중 뒤 3개). 1/3 추가 시간이 무엇에서 나오는지는 원문이 나누어 밝히지 않음. (p.2, p.5) |
| 8 | teacher forward 셈? Y (추가 1/3이 그것) | 위와 같음. 1/3에 무엇이 들어갔는지 정의 없음 | p.2, p.8 | 수정 | teacher forward 셈? 추정 Y (1/3이 학습 전체 시간 대비 서술이므로). 원문이 내역을 밝히지 않음. |
| 9 | 일회성 비용: 해당 없음 | 별도 teacher 학습 없음 (한 세대 안에서 끝남) | 초록 p.1 | 일치 | — |
| 10 | 하드웨어 보고 안 됨 | GPU 언급은 각주 1의 일반론뿐 ("researchers often train deep networks using a fixed number of GPUs") | 각주 p.3 | 일치 | — |
| 11 | 설정: CIFAR-100, ImageNet (R101·152, 90 epoch). 순환 주기 2개 | ImageNet: "K = 2 ... two equal sections (each has 45 epochs)", T=2. CIFAR-100: "K = 4", ResNet-20/32/56/110 164 epoch, DenseNet-100/190 300 epoch | p.6–7 | 수정 | 설정: CIFAR-100 (ResNet-20/32/56/110 164 epoch, DenseNet-100/190 300 epoch, 주기 4개), ImageNet (ResNet-101·152, 90 epoch, 45 epoch 주기 2개, T=2). (p.6–7) |
| 12 | 평균 외 평가: PascalVOC 검출·분할 전이 (Table 5) | Table 5 있음 | Table 5 p.8 | 일치 | — |
| 13 | (Table 5 수치 없음) | ResNet152 BL→SD: VOC2007 mAP 73.49→74.93, VOC2012 mIoU 77.53→77.97 | Table 5 p.8 | 추가 | 전이: ResNet152 기준 VOC2007 검출 mAP 73.49→74.93%, VOC2012 분할 mIoU 77.53→77.97%. (Table 5 p.8) |
| 14 | "그 외 없음" | Fig. 1: 학습·테스트 오류 곡선 ("SD achieves a higher training error but a lower testing error"). Table 3·4: top-5 오류, 앙상블 오류 | Fig. 1 p.8, Table 3 p.6 | 추가 | 그 밖에 학습·테스트 오류 곡선 (두 번째 주기에서 학습-테스트 격차 줄어듦, Fig. 1 p.8), top-5 오류와 스냅샷 앙상블 오류 (Table 3·4). |
| 15 | (없음) | ImageNet SE(같은 주기 분할, teacher 없음)는 BL보다 나쁨: R101 22.14 vs 21.62. "reducing the number of training epochs harms ..." | Table 4 p.8 | 추가 | 주기를 나누는 것만으로는 ImageNet에서 손해: snapshot ensemble 22.14% vs BL 21.62% (R101). SD 이득 일부는 주기 분할 손실을 메우는 몫. (Table 4 p.8) |
| 16 | (없음) | 표 1은 방법 속성 표 (SA/IN/1G) | Table 1 p.1 | 추가 | Table 1은 수치가 아니라 기존 T-S 방법의 속성 비교표 (같은 구조·ImageNet 평가·한 세대 여부). |
| 17 | 위치: Table 1 p.1, §1 p.2, Table 3 p.6, Table 4·5·Fig.1 p.8 | 모두 해당 쪽에 있음 | — | 일치 | — |
| 18 | 메모: 03–06번과 달리 추가 비용 크기를 적음 | 다른 논문 비교라 이 PDF로 확인 못 함 | — | 확인 불가 (범위 밖 논문) | — |

---

## Sub-08 Proteus (Accessing Vision Foundation Models via ImageNet-1K)
- 본 판본: arXiv 2407.10366v2 (11 Feb 2025), ICLR 2025 판
- 대조 결과: 일치 9건 / 수정 4건 / 추가 4건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | DINOv2 원 데이터(비공개 1억 4,200만) 대신 IN-1K 120만, 토큰·패치·특징 3수준 목표 | "undisclosed datasets, e.g., WIT400M ... and LVD-142M"; "three levels of training objectives, i.e., token, patch, and feature" | 초록 p.1, p.2 | 일치 | — |
| 2 | 1.42억→120만 장으로 비슷한 성능 (Fig. 1) | "Proteus-L/14 matches the performance of the Oracle method DINOv2-L/14 (142M training data) across 19 benchmarks" | 초록 p.1, Fig. 1 p.2 | 일치 | — |
| 3 | 얻는 것 (공식 DINOv2 대비) 아래에 학습 시간 30h = DeiT 30h | Table 19: DeiT ViT-S/16 (teacher RegNetY) 30h / Proteus ViT-S/14 (teacher DINOv2-B/14) 30h, 8×A100. 공식 DINOv2 학습 시간은 없음 | 부록 A.8, Table 19 p.18 | 수정 | 학습 시간 비교 대상은 공식 DINOv2가 아니라 DeiT. DeiT ViT-S/16 (RegNetY teacher) 30h vs Proteus ViT-S/14 (DINOv2-B/14 teacher) 30h, IN-1K 300 epoch, A100 8장. 공식 DINOv2의 학습 시간은 보고 안 됨. (Table 19 p.18) |
| 4 | 공식 DINOv2보다 ImageNet-A에서 뒤짐, Sketch·R 비슷, C 앞섬 | "similar results ... on ImageNet-Sketch, ImageNet-R and outperforms it obviously on ImageNet-C. However, we fall behind DINOv2 at ImageNet-A" | 부록 A.3, Table 14 p.16 | 일치 | — |
| 5 | (Table 14 수치 없음) | Im-A: ViT-S 31.1 vs 34.1, ViT-B 52.4 vs 55.5, ViT-L 68.7 vs 71.4. teacher는 Proteus-S가 DINOv2-B, Proteus-B가 DINOv2-L, 공식 DINOv2는 모두 DINOv2-g | Table 14 p.16 | 추가 | ImageNet-A 격차: ViT-S 31.1 vs 34.1%, ViT-B 52.4 vs 55.5%, ViT-L 68.7 vs 71.4%. S·B는 teacher 크기도 공식 DINOv2(g)보다 작음. (Table 14 p.16) |
| 6 | (없음) | "Proteus still lags behind DINOv2-S on FGVC-Aircraft, Stanford Cars, and SUN397"; Table 17 Country211 Proteus 19.4 vs DINOv2 21.8 (ViT-B), CLIP teacher 29.3 vs CLIP 32.3 | p.6, Table 17 p.17 | 추가 | [실측·원논문] 세부 분류 평균은 비슷하나 FGVC-Aircraft, Stanford Cars, SUN397에서 DINOv2-S보다 낮음 (p.6). Country211에서도 teacher 계열보다 낮음: 19.4 vs 21.8% (DINOv2), 29.3 vs 32.3% (CLIP). (Table 17 p.17) |
| 7 | 옮겨 간 비용: 없음 | 목표가 teacher 특징 정렬이므로 학습 매 스텝에 teacher forward 필요, 투영 헤드 있음 (A.9 "projection head ... used to align the dimension between teacher and student") | §2, A.9 p.18 | 수정 | 옮겨 간 비용: 학습 중 teacher forward와 정렬용 투영 헤드 (30h 안에 포함). teacher의 원 데이터 사전학습 비용은 측정 범위 밖. 바뀌는 것은 student가 보는 데이터 접근뿐. (p.18) |
| 8 | teacher forward 셈? Y / 일회성 N / A100 8장 | Table 19 "Training Hour"가 학습 전체, "8 A100 GPUs" | p.18 | 일치 | — |
| 9 | 설정: DINOv2-S/B/L/g → ViT-Ti/S/B/L, 300 epoch, batch 1024 | "total batch size of 1024, except the ViT-L experiment with a batch size of 256 due to GPU memory constraints" | §3.1 p.5, 부록 p.15 | 수정 | batch 1024 (ViT-L만 GPU 메모리 제약으로 256). (§3.1 p.5) |
| 10 | 평균 외 평가 Table 3, DeiT 대비 모든 항목에서 높음, ViT-B Im-A 29.6→52.4 | Table 3 값 확인. 캡션: "All DeiT results are obtained by knowledge distillation, except *" (ADE20K·NYUd). Im-C·NYUd는 ↓. Ti Im-C 67.3 vs 67.4 | Table 3 p.6 | 수정 | 보고된 DeiT 비교의 모든 항목에서 Proteus가 나음. 단 ADE20K·NYUd의 DeiT는 증류 없는 판이고, Im-C·NYUd는 낮을수록 좋음. ViT-Ti Im-C는 67.3 vs 67.4로 거의 같음. (Table 3 p.6) |
| 11 | (없음) | Table 15 다양한 proxy 데이터 (ImageNet-Merge), Table 17 미학습 개념 (GTSRB, Country211), Table 18 teacher 크기 ("the average accuracy of Proteus decreases with the increased teacher scales") | p.16–18 | 추가 | 추가 평가: proxy 데이터 다양성 (Table 15), teacher가 안 본 개념으로의 일반화 (Table 17), teacher 크기 변화 (Table 18: teacher가 클수록 평균 정확도 하락). (p.16–18) |
| 12 | 위치: Fig.1·2 p.2, Table 1·2 p.5, Table 3 p.6, Table 4–9 p.7–8, Table 14 p.16, A.8 p.18 | 모두 맞음 | — | 일치 | — |
| 13 | 메모: Table 19 검증 9h vs 1h는 프로토콜 차이 | "validation after each training epoch actually costs lots of time in DeiT. In contrast, we ... only conduct linear probing after the training" | A.8 p.18 | 일치 | — |
| 14 | 메모: DeiT 비교는 teacher 종류까지 다름 | RegNetY vs DINOv2-B/14 | Table 19 p.18 | 일치 | — |
| 15 | 메모: 데이터 100분의 1, student 학습 시간은 같음 | 1.2M / 142M ≈ 0.85% (계산). 시간 같음은 ViT-S 한 쌍 | p.18 | 일치 | — |
| 16 | (없음) | "official DINOv2-S/B/L models are obtained by distilling from DINOv2-g ... it increases the training time significantly due to a very large teacher" — 수치 없음 | §3.3.1 p.6 | 추가 | 공식 DINOv2-S/B/L은 DINOv2-g에서 증류해 teacher가 커서 학습 시간이 크게 늘어난다고 서술 (수치 없음). Proteus는 기본적으로 한 단계 큰 모델을 teacher로 씀 (Proteus-L만 g). (p.6) |
| 17 | 강건성·세부 분류·분할·깊이 항목 나열 | Table 3 열과 같음 | Table 3 p.6 | 일치 | — |

---

## Sub-09 SRe2L (Squeeze, Recover and Relabel)
- 본 판본: arXiv 2306.13092v3 (11 Feb 2024), NeurIPS 2023
- 대조 결과: 일치 8건 / 수정 4건 / 추가 4건 / 확인 불가 1건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 3단계 (압축·복원·재라벨), 합성 단계에 원본 데이터 불필요 → 메모리·시간 절감 | "decouple real and synthetic data during the training phase, thereby necessitating only minimal memory" | §2.1 p.4–5 | 일치 | — |
| 2 | Table 1: MTT 12.64 ms → 0.24 (C4)·0.75 (R18), 메모리 48.9 → 4.2·7.6 GB, RTX 4090 1장 | 값 같음. 캡션: "Tiny-ImageNet (64×64 resolution) ... Peak value of GPU memory usage is measured or converted with a batch size of 200" | Table 1 p.3 | 수정 | 조건 추가: Tiny-ImageNet 64×64, 최대 메모리는 batch 200 기준으로 재거나 환산한 값. (Table 1 p.3) |
| 3 | (없음) | 초록: "approximately 52× (ConvNet-4) and 16× (ResNet-18) faster with less memory consumption of 11.6× and 6.4×". ImageNet-1K 224: 이미지·반복당 R18 0.83 ms, R50 2.62 ms. 최고 결과는 4k 반복 | 초록 p.1, Table 3 p.7 | 추가 | ImageNet-1K (224) 합성 비용: 이미지 1장·반복 1회당 ResNet-18 0.83 ms, ResNet-50 2.62 ms (RTX 4090). ablation은 2k 반복, Table 4 최고 값은 4k 반복. (Table 3·본문 p.7) |
| 4 | 클래스당 50장: R101 student 60.8%, R18 46.8%, 전체 데이터 R18 69.76% (1절, Table 4) | Table 4: R18 46.8±0.2, R101 60.8±0.5 (복원 모델 R18). 69.76%는 "PyTorch off-the-shelf pre-trained ResNet-{18, 50} with the Top-1 accuracy of {69.76%, 76.13%}" | Table 4 p.9, p.6, 부록 p.15 | 수정 | 위치 수정: 69.76%는 Table 4가 아니라 실험 설정 (p.6)과 부록 (p.15)의 사전학습 ResNet-18 정확도. 같은 구조 기준 격차 69.76 − 46.8 = 22.96%p (계산). |
| 5 | RDED Table 2에서도 같은 값 | RDED PDF Table 2: SRe2L IN-1K IPC50 R18 46.8±0.2, R101 60.8±0.5 | RDED Table 2 p.7 | 일치 | — |
| 6 | (없음) | Table 5: ResNet-18로 압축한 데이터를 DeiT-Tiny로 학습하면 15.41%. "suboptimal performance of ViT". 부록: "on the smaller CIFAR-10 dataset, a clear gap exists" | Table 5 p.9, p.17 | 추가 | [실측·원논문] ViT student에서 약함: ResNet-18 압축 데이터로 DeiT-Tiny 15.41% (ResNet-18 46.71%) (Table 5 p.9). CIFAR-10에서는 기존 방법과 뚜렷한 격차 (부록 p.17). |
| 7 | 옮겨 간 비용: 압축 단계 모델 학습 (공개 사전학습 모델이면 0) | ImageNet-1K는 PyTorch 사전학습 모델 사용. Tiny-ImageNet은 "trained from scratch" (100 epoch, Table 7a) | p.6, Table 7 p.15 | 수정 | 압축 단계 학습은 ImageNet-1K에서만 공개 사전학습 모델로 대체 (새 학습 0). Tiny-ImageNet은 ResNet-18/50을 처음부터 100 epoch 학습. 그 시간은 보고 안 됨. (p.6, Table 7 p.15) |
| 8 | soft label 저장 용량은 숫자 없음 | "Another limitation is the extra storage for soft labels." 크기 없음 | 결론 p.10 | 일치 | — |
| 9 | 다른 논문 측정: 응축 데이터의 30배 이상 (3_Reuse 09번) | 이 PDF에 없음 | — | 확인 불가 (다른 논문 수치) | — |
| 10 | teacher forward 셈? N (재라벨링 시간 없음) | Table 1·3 합성 시간은 고정 모델(=압축 모델, 재라벨 teacher와 같은 계열)의 forward와 이미지로의 역전파를 포함. 재라벨링·student 학습 시간은 없음 | §2.1 p.4–5, Table 1 p.3 | 수정 | teacher forward 셈? 합성 단계 Y (복원 모델의 forward·backward가 합성 시간에 들어감), 재라벨링과 student 학습 N (시간 보고 안 됨). |
| 11 | 일회성: 합성 Y, 압축·재라벨 N / RTX 4090 1장 | 일치 | Table 1 p.3 | 일치 | — |
| 12 | 설정: Tiny-ImageNet, ImageNet-1K (224), 복원 R18/50, student R18/50/101 등 | 맞음. 부록에 CIFAR-100 (Table 10–11)과 저해상도 ImageNet (Table 9) 추가. student 검증 학습: AdamW, batch 1024, 300 epoch (Table 7b) | p.5–6, p.15–17 | 추가 | 설정 추가: 부록에 CIFAR-100 (Table 11)과 64·112 해상도 ImageNet (Table 9). 응축 데이터로 student를 300 epoch 학습 (Table 7b p.15). |
| 13 | 평균 외 평가: 구조 일반화 (Table 5), 지속 학습 (Fig. 6) | Fig. 6 p.10 (5·10단계 class-incremental, Tiny-ImageNet) | p.9–10 | 일치 | — |
| 14 | 위치: Fig.1 p.1, Table 1 p.3, Table 4·5 p.9, Table 7 p.15 | 맞음. Fig. 6은 p.10 | — | 일치 | (위치에 Fig. 6 p.10 추가) |
| 15 | 정정 줄: R101/R18 구분, 복구 단계 forward·역전파, 재라벨링 시간 보고 안 됨 | 모두 원문과 맞음. 복원 손실은 p.5의 BN 정합 식 | Table 4 p.9, p.4–5 | 일치 | — |
| 16 | (없음) | Fig. 1: ImageNet-1K IPC10 합성 시간 TESLA 12.89 ms vs SRe2L 0.83 ms | Fig. 1 p.1 | 추가 | Fig. 1: ImageNet-1K 클래스당 10장, 합성 시간 TESLA 12.89 ms vs SRe2L 0.83 ms (이미지·반복당). (p.1) |
| 17 | 메모: 응축이 teacher에 의존, 응축 비용과 student 학습 비용을 따로 적어야 | 해석. 사실 오류 없음 | — | 일치 | — |

---

## Sub-10 RDED
- 본 판본: arXiv 2312.03526v2 (19 Mar 2024)
- 대조 결과: 일치 9건 / 수정 3건 / 추가 4건 / 확인 불가 0건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 최적화 없이, teacher 점수 높은 패치를 골라 이어 붙이고 영역별 soft label | 부록 C.2: 이미지당 무작위 5조각 → 최고 점수 패치 → N=4개를 이어 붙임 → FKD식 영역 soft label | p.4–5, 부록 C.2 p.13 | 일치 | — |
| 2 | 클래스당 10장: 7분에 R18 42.0% vs 6시간에 21.3% | 초록: "within 7 minutes, achieving a notable 42% ... (while the SOTA only achieves 21% but requires 6 hours)"; Table 2 42.0 vs 21.3 | 초록 p.1, Table 2 p.7 | 일치 | — |
| 3 | (조건 없음) | 헤드라인은 ResNet-18로 관찰·평가. Table 3 R18: 2113.23 ms vs 39.89 ms → 10,000장이면 약 5.9시간 vs 6.6분 (계산) | Fig. 1 p.1, Table 3 p.7 | 추가 | 헤드라인은 ResNet-18 관찰·평가, ImageNet-1K. Table 3의 이미지당 시간 × 10,000장으로 계산하면 SRe2L 약 5.9시간, RDED 약 6.6분으로 초록 수치와 맞음 (계산). |
| 4 | Table 3: MobileNet-V2 3783.16 → 64.97 ms, 12.93 → 2.35 GB | 값 같음. 캡션: "consumption (ms) for each image when generating 100 images simultaneously ... measured with a batch size of 100" | Table 3 p.7 | 수정 | 조건 추가: 100장을 동시에 만들 때 이미지당 ms, 최대 메모리는 batch 100. 같은 표 ResNet-18: 2113.23 → 39.89 ms, 9.14 → 1.57 GB. (Table 3 p.7) |
| 5 | 잃는 것: 클래스당 50장 R18 56.5%, R101 61.2% | 값 같음 | Table 2 p.7 | 일치 | — |
| 6 | "전체 데이터 학습과의 차이는 여전히 큼" | RDED 원문에 전체 데이터 기준 정확도 없음 | 전체 | 수정 | 전체 데이터 기준값은 이 논문에 없음. 격차를 적으려면 다른 출처 필요 (예: 09번 SRe2L p.6의 사전학습 ResNet-18 69.76%). |
| 7 | CIFAR-10 클래스당 1장 23.5% vs MTT 46.3% | Table 2 ConvNet 열 (Conv-3으로 관찰·평가) | Table 2 p.7 | 일치 | (조건: ConvNet 열) |
| 8 | (없음) | Table 2: Tiny-ImageNet IPC50 R101 RDED 41.2 vs SRe2L 42.5. IN-1K IPC50 R101 61.2 vs 60.8. 본문: "when IPC comes to 1, our approach struggles". 부록 Table 6: CIFAR-10 MDC 47.8/62.6/74.6 vs RDED 23.5/50.2/68.4 | Table 2 p.7, Table 6 p.14 | 추가 | [실측·원논문] SRe2L보다 낮은 경우: Tiny-ImageNet 클래스당 50장 ResNet-101 41.2 vs 42.5%. ImageNet-1K 50장 ResNet-101은 61.2 vs 60.8%로 차이 0.4%p. 클래스당 1장에서는 고해상도에서도 약하다고 저자가 인정 (p.7). MDC 대비 CIFAR-10 모든 IPC에서 낮음 (Table 6 p.14). |
| 9 | 옮겨 간 비용: 패치 점수용 teacher forward, 영역별 soft label 생성·저장, 저장 용량 보고 안 됨 | "the distillation time for an image is solely dependent on the scoring cost"; C.2 "generation and storage of region-level soft labels". 용량·재라벨 시간 없음 | §5.3 p.8, p.13 | 일치 | (재라벨링 시간도 보고 안 됨을 덧붙임) |
| 10 | 어떻게 쟀나: 합성 시간(분, ms), 최대 메모리, teacher forward 합성 시간에 포함, 4090 1장 | 맞음. "분"은 초록에만 있음 | p.1, p.7–8 | 일치 | — |
| 11 | 설정: teacher와 평가 모델 ConvNet, R18, R101 등 | Table 2: ConvNet 열은 같은 ConvNet으로 관찰·평가. ResNet 열은 "SRe2L and RDED use ResNet-18 to retrieve the distilled data, and evaluate on ResNet-18 and ResNet-101" | Table 2 캡션 p.7, C.1 p.13 | 수정 | 관찰(teacher)과 평가 모델 구분: ResNet 열은 ResNet-18로 만들고 ResNet-18·101로 평가. ConvNet 열은 Conv-3~6으로 만들고 같은 모델로 평가. (Table 2 p.7) |
| 12 | 평균 외 평가: 구조 일반화 (Table 4). 그 외 없음 | Table 4 p.8. 부록 Table 5 (VGG-11, Swin-V2-Tiny 포함), Table 6 multisize, Table 7 coreset 기준선, Fig. 4·5 ablation | p.8, p.14–15 | 추가 | 부록 Table 5에서 BN 없는 구조 (VGG-11, Swin-V2-Tiny)까지 일반화 평가. 여러 IPC를 한 번에 만드는 multisize 비교 (Table 6), coreset 선택 기준선 (Table 7). (p.14) |
| 13 | 위치: 초록 p.1, Table 1 p.3, Table 2·3 p.7, Table 4 p.8, 부록 p.13 | 모두 맞음 | — | 일치 | — |
| 14 | 메모: "7분"은 제작 시간 | "distill a dataset to 10 images per class from full ImageNet-1K within 7 minutes" | 초록 p.1 | 일치 | — |
| 15 | 메모: 소수 그룹 보존 검증 안 됨 | minority/group/fairness 관련 평가 없음 (검색) | 전체 | 일치 | — |
| 16 | 메모: "teacher가 고른 부분집합"에 가까움 | C.2 "Images reconstruction ... draw N = 4 distinct patches ... and concatenate them" | p.13 | 추가 | 단, 결과물은 원본 이미지 부분집합이 아니라 패치 4개를 이어 붙인 재구성 이미지 (부록 C.2 p.13). |

---

## Sub-11 Up to 100x Faster Data-free KD (FastDFKD)
- 본 판본: arXiv 2112.06253v2 (24 Feb 2022), AAAI
- 대조 결과: 일치 10건 / 수정 1건 / 추가 4건 / 확인 불가 1건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 공통 특징을 메타 학습으로 재사용해 샘플당 최적화 스텝을 수 회로 | Alg. 1: 손실마다 k-step adaptation (z, θ); "only requires 5 steps for each batch" | Alg. 1 p.4, p.6 | 수정 | 합성 **mini-batch**당 최적화 스텝을 수 회 (2·5·10, ImageNet 50)로 줄임. (Alg. 1 p.4, p.6) |
| 2 | CIFAR-100 R34→R18: DeepInv 42.1h (61.32), CMI 19.2h (77.04), Fast10 0.27h (74.34) | 값 같음 | Table 1 p.6 | 일치 | — |
| 3 | ImageNet R50→R50: DeepInv 166h (68.00) → 6.28h (68.61) | 값 같음. 표의 Speed Up은 Generative DFD (~300h) = 1× 기준: DeepInv 1.8×, Fast50 47.8×. 합성량 140k | Table 2 p.6 | 추가 | 조건: Fast50, 합성 140k장. DeepInv 대비 166/6.28 ≈ 26.4배 (계산). 표의 47.8배는 Generative DFD (약 300h) 기준. (Table 2 p.6) |
| 4 | 스텝: 2스텝 69.76, 5스텝 72.82, 10스텝 74.34 | 값 같음 (R34→R18 열) | Table 1 p.6 | 일치 | — |
| 5 | CMI(77.04)보다 낮음 | 맞음 | Table 1 p.6 | 일치 | — |
| 6 | ImageNet 68.61 vs 75.45 | "Scratch" 1.3M, R50 75.45 | Table 2 p.6 | 일치 | — |
| 7 | WRN-40-2→WRN-16-2: CMI 68.75 vs Fast10 65.12 | 값 같음 | Table 1 p.6 | 일치 | — |
| 8 | (없음) | ImageNet R50→R18: Generative DFD 54.66 vs Fast50 53.45, →MobileNetV2 43.15 vs 43.02. 실제 데이터 scratch 68.45/70.01 | Table 2 p.6 | 추가 | [실측·원논문] ImageNet 작은 student에서는 기존 Generative DFD보다도 약간 낮음: ResNet-18 53.45 vs 54.66%, MobileNetV2 43.02 vs 43.15%. 실제 데이터 학습 68.45·70.01%와 격차 큼. (Table 2 p.6) |
| 9 | 옮겨 간 비용: 생성기와 메타 학습 연산은 보고한 GPU 시간에 포함 | 지표 정의는 "GPU hours taken by data synthesis"뿐. Alg. 1은 합성·메타 업데이트·KD 업데이트를 한 루프에 섞음. 구성 요소별 시간 없음 | p.4–5 | 확인 불가 (포함 범위 명시 없음) | 생성기 적응과 메타 업데이트는 합성 과정의 일부로 서술되나, 보고 시간에 무엇이 들어가는지 분해 없음. 같은 루프의 KD 업데이트 시간은 제외. (Alg. 1 p.4, p.5) |
| 10 | student 학습 시간 명시적 제외 (p.5) | "the time cost of student training is omitted since we only focus on the data synthesis process"; "all GPU hours are estimated on a single GPU" | p.5 | 일치 | — |
| 11 | teacher forward 셈? 합성 시간에 포함 | 합성 손실 L_cls, L_feat는 teacher, L_adv는 teacher와 student 출력 모두 사용 (식 13) | p.5 | 일치 | (추가) 합성 손실의 adversarial 항 때문에 합성 중 student forward도 듦. (식 13 p.5) |
| 12 | 하드웨어: GPU 1장 기준 추정, 종류 보고 안 됨 | GPU 종류 언급 없음 | p.5 | 일치 | — |
| 13 | 설정: CIFAR-10/100 여러 쌍, ImageNet R50→R50·R18·MBv2, NYUv2 | 맞음. NYUv2: Deeplabv3-ResNet50 → Deeplabv3-MobileNetV2 | Table 1–3 p.6–7 | 일치 | — |
| 14 | 평균 외 평가: 없음 (분할 mIoU는 다른 과제) | Table 3: Fast10 0.82h, mIoU 0.366 / DFAD 6.0h 0.364 / DAFL 3.99h 0.105 / 실제 데이터 Student 0.375, KD 0.380 | Table 3 p.7 | 추가 | NYUv2 분할: 합성 0.82시간, mIoU 0.366 (DFAD 6.0시간 0.364). 실제 데이터 KD 0.380, student 0.375보다 낮음. (Table 3 p.7) |
| 15 | 위치: Fig.1 p.1, Table 1·2 p.6, Table 3–5 p.7 | 맞음 | — | 일치 | — |
| 16 | 메모: "100배"의 분모는 가장 느린 데이터 없는 증류의 합성 시간 | CIFAR 배수는 DeepInv2k = 1.0× 기준 평균: Fast10 CIFAR-10 126.7×, CIFAR-100 124.7×, Fast2 655.0×·588.2×. ImageNet은 Generative DFD 기준 | Table 1·2 p.6 | 추가 | 배수 기준: CIFAR는 DeepInv2k 대비 평균 (Fast10 약 125배, Fast2 최대 655배이나 정확도 손실 큼), ImageNet은 Generative DFD 대비 47.8배. 제목의 "100×"는 헤드라인 값. (Table 1·2 p.6) |

---

## Sub-12 NAYER
- 본 판본: arXiv 2310.00258v2 (22 Mar 2024)
- 대조 결과: 일치 9건 / 수정 3건 / 추가 3건 / 확인 불가 1건

| # | 메모의 서술 (짧게) | 원문 | 위치 (표·쪽) | 판정 | 고칠 문구 |
|---|---|---|---|---|---|
| 1 | 무작위 잡음 대신 클래스 이름 텍스트 임베딩 입력, 잡음은 생성기 안 한 층으로 | "relocates the random source from the input to a noisy layer and utilizes the meaningful constant label-text embedding (LTE) as the input. LTE is generated by using the language model once" | 초록 p.1 | 일치 | — |
| 2 | CIFAR-100: DeepInv 31.23h (61.34), CMI 24.01h (68.75), FM 2.23h (67.15), 제안 2.15h (70.20), A100 1장, WRN-40-2→WRN-16-2 | 값 같음. FM·NAYER 모두 E=100 | Table 2 p.7 | 일치 | (조건: FM·NAYER 모두 E=100) |
| 3 | DeepInv 대비 평균 4.5~14.9배, 초록 5~15배 | Table 2 "Averaging Speed Up" NAYER E=100/200/300: 14.88×/7.93×/4.47× (CIFAR-10·100 평균). Fig. 1: "5 to 15 times compared to DeepInv". FM E=100도 14.17× | Table 2 p.7, Fig. 1 p.1 | 일치 | (추가) 같은 기준에서 FM(E=100)도 14.17배. |
| 4 | 원본 데이터 student보다 낮음: WRN-16-2 73.56 vs 71.72 (E=300) | Table 1 Student 73.56, NAYER E=300 71.72 | Table 1 p.7 | 일치 | — |
| 5 | ImageNet R50 68.92 vs teacher 75.45 | Table 1 ImageNet R50/R50: Teacher 75.45, Student 75.45, DeepInv 68.00, FM 57.37(재실험), NAYER E=300 68.92. ImageNet 학습 시간 없음 | Table 1 p.7 | 추가 | 75.45%는 teacher이자 실제 데이터 student 값. ImageNet 학습 시간은 보고 안 됨 (Table 2는 CIFAR만). (Table 1 p.7) |
| 6 | (없음) | E=100 (15배 설정)에서: CIFAR-100 R34/R18 76.29 vs CMI 77.04·SpaceshipNet 77.41, Tiny-ImageNet 61.71 vs CMI 64.01·SpaceshipNet 64.04. E=300도 CIFAR-10 R34/R18 95.21 < SpaceshipNet 95.39 (본문 인정) | Table 1 p.7, p.6 | 추가 | [실측·원논문] 가장 빠른 E=100 설정은 일부 쌍에서 기존 방법보다 낮음: CIFAR-100 ResNet-34→18 76.29 vs 77.41% (SpaceshipNet), Tiny-ImageNet 61.71 vs 64.04%. E=300도 CIFAR-10 ResNet-34→18은 95.21 vs 95.39%. (Table 1 p.7) |
| 7 | 옮겨 간 비용: 텍스트 인코더 일회성 계산, 생성기 학습. 학습 시간에 포함해 보고 | Alg. 1: LTE를 루프 전에 한 번 만들어 pool P에 저장. Table 2가 LTE 추출 시간 포함 여부를 밝히지 않음 | Alg. 1 p.5, Fig. 4 p.6, Table 2 p.7 | 확인 불가 (LTE 추출 포함 여부 명시 없음) | 생성기 학습은 학습 시간에 포함. LTE 추출(텍스트 인코더 1회)이 Table 2 시간에 들어갔는지는 명시 없음. |
| 8 | (없음) | Alg. 1 "M ← M ∪ x̂": 합성 이미지를 메모리 M에 계속 쌓음. Table 5: 버퍼 5k–200k–Full, 클수록 정확도 높음 (90.41 → 91.94). 바이트 크기 없음 | Alg. 1 p.5, Table 5 p.8 | 추가 | 옮겨 간 비용 추가: 합성 이미지 메모리 버퍼 (기본은 전부 보관). 버퍼 5k에서 90.41%, 전부 보관 91.94% (Table 5 p.8). 저장 용량은 보고 안 됨. |
| 9 | teacher forward 셈? Y / 일회성 teacher 학습 N / A100 1장 | Alg. 1 루프 안에 생성기 손실 T(x̂)와 student KD 모두 있음. Table 2 "using a single NVIDIA A100" | Alg. 1 p.5, Table 2 p.7 | 일치 | — |
| 10 | 3회 평균 (부록) | Table 14: E=300, CIFAR-10·100 각 3쌍, 6개 설정만. 본문 Table 1은 단일 값 (Table 14 값과 조금 다름: 95.24 vs 95.21 등) | Table 14 p.11 | 수정 | 3회 평균·표준편차는 부록 Table 14의 E=300 CIFAR 6개 설정에만 있음. 본문 표는 단일 실행 값. (Table 14 p.11) |
| 11 | 평균 외 평가: 다양성 지표 (Table 4). 그 외 없음 | Table 4: 수렴 시간·다양성. Table 3: 데이터 없는 양자화. Table 11: NYUv2 분할 mIoU. Table 12: 수렴 시간. t-SNE p.12 | p.7–8, p.11–12 | 수정 | 평균 외 평가: 생성기 수렴 시간과 다양성 (Table 4, 12), 데이터 없는 양자화 (Table 3 p.7), NYUv2 분할 mIoU (부록 Table 11 p.11), LTE t-SNE (p.12). |
| 12 | 위치: Fig.1 p.1, Table 1·2 p.7, Table 4–7 p.8, 부록 Table 14 p.11 | 모두 맞음 | — | 일치 | (Table 3 p.7, Table 11 p.11 추가) |
| 13 | 정정 줄: 5–15배는 DeepInv 기준, FM 대비 2.23→2.15h ≈ 1.04배, NYUv2 둘 다 0.82h | 2.23/2.15 = 1.037 (계산). Table 11 "Synthetic Time 0.82h 0.82h", mIoU FM 0.366, NAYER 0.385 | Table 2 p.7, Table 11 p.11 | 일치 | — |
| 14 | 메모: FM(E=100) 2.23h 67.15%, 시간 차이 작음 | 값 같음 | Table 2 p.7 | 일치 | — |
| 15 | 메모: 사전학습 텍스트 인코더(CLIP) 비용은 계산에 없음 | Table 7에서 Doc2Vec·SBERT·CLIP 비교 후 CLIP 선택. 사전학습 비용 언급 없음 | Table 7 p.8 | 일치 | — |
| 16 | 메모 (계속): 11번과 달리 합성 + 증류 전체 시간 보고 | 분류 Table 2는 "training times" (전체). NYUv2 Table 11은 "Synthetic Time"만 | Table 2 p.7, Table 11 p.11 | 수정 | 전체 시간 보고는 CIFAR 분류 (Table 2)에 한정. NYUv2 분할 (Table 11)은 합성 시간만 보고해 11번과 같은 경계. (p.11) |

---

## 전체 요약

1. 가장 심각한 오류: 07번 "표에서는 손실 없음". CIFAR-100 DenseNet190에서 SD의 최종 오류가 기준보다 나쁨 (18.02 vs 16.82%). 손실 없음은 ImageNet 표에만 맞음. 07번 설정의 "순환 주기 2개"도 ImageNet에만 해당 (CIFAR는 4개).
2. 비용 경계 과장: "옮겨 간 비용 없음" (08), "1/3이 teacher forward 때문" (07), "LTE·메타 학습이 시간에 포함" (11·12). 원문은 구성 요소별 시간을 밝히지 않음. 판정을 "포함 여부 명시 없음"으로 낮춰야 함.
3. 분모·조건 누락 패턴: 08번 30h 비교 대상은 공식 DINOv2가 아니라 DeiT. 11번 ImageNet 47.8배는 Generative DFD 기준 (DeepInv 기준은 26.4배). 09·10번 메모리 값의 batch 조건 (200·100) 빠짐.
4. "평균 외 평가 없음"이 틀린 곳: 12번 (양자화, NYUv2, 수렴 시간), 07번 (학습 곡선). 빠진 손실 사례: 10번 Tiny-ImageNet R101에서 SRe2L보다 낮음, 12번 E=100이 일부 쌍에서 기존 최고보다 낮음, 08번 세부 분류 3종.
5. 수치 자체(정확도·시간·메모리)의 전사 오류는 찾지 못함. 오류는 대부분 조건, 범위, 비교 대상에서 나옴.
