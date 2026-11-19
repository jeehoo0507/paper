# 4절: Shrink

담당: Shrink 담당. 할 일은 [`../4_taxonomy.md`](../4_taxonomy.md).

> **AI 초안 (2026-11-19).** [`../../notes/`](../../notes)의 읽기 메모에서 기계적으로 옮긴 값이다. 원문과 대조하지 않았다. 칸 값을 원문으로 확인해 고치고 확인 열에 `이름 날짜`를 적는다. "메모에 없음"은 메모에 적혀 있지 않다는 뜻이지 논문이 보고하지 않았다는 뜻이 아니다. 고칠 곳은 [`../../notes/audit/`](../../notes/audit/README.md)에도 있다.

## 방법 표

| 번호 | 방법 | 갈래 (초안) | 줄이는 비용 | 새로 드는 비용 | 보고한 절감률 | 절감률의 분모 | 설정 | 위치 | 확인 |
|---|---|---|---|---|---|---|---|---|---|
| S-01 | MaskedKD | teacher 입력 토큰 | teacher 입력 토큰 일부를 버려 teacher forward 연산을 줄임. 버릴 토큰은 student 마지막 층의 CLS attention 점수가 낮은 순으로 고름. student는 전체 이미지를 그대로 봄. | student attention 점수 평균·정렬·선택 (ViT-G/14 기준 샘플당 4.1 kFLOPs 미만이라고 보고, 3.2절). student forward가 teacher forward보다 먼저 끝나야 해서… | DeiT-S → DeiT-Ti, 토큰 50% 유지: teacher FLOPs 1770 → 866 PFLOPs (−51%), teacher 처리량 1790 → 3702 img/s (2.1배). (Table 2) GPU 메모리: teacher 6499 → 3412 MB, student 포함… | teacher 연산 대비, 전체 학습 대비 아님 | ImageNet-1k, DeiT 레시피. student는 처음부터 학습 (Tiny는 crop + flip만 사용). teacher는 DeiT·DeiT3·CaiT·CLIP·MAE.… | Table 2 (정확도·FLOPs·처리량), Table 3·4 (오디오, DINO), Table 9 (증강), Table 10 (FLOPs 분석… | |
| S-02 | DMAE | 토큰 + teacher 깊이 | MAE 인코더 중간 층의 특징 출력을 정렬해 증류함. 로짓을 일치시키는 방식은 아님. | student 특징을 teacher 차원에 맞추는 투영층과 특징 정렬 손실. student MAE의 픽셀 복원 학습도 함께 수행. | 전체 이미지·teacher 전체 깊이를 사용하는 증류와 비교해, 보이는 패치만 처리하고 teacher도 정렬 층까지만 실행해 순전파 계산을 줄임. | 메모에 없음 | ImageNet-1K. MAE 기반 특징 증류 외에 DINO·MoCo-v3 학습에도 적용한 실험이 있음. | 4.7절 Standard Deviation Analysis. | |
| S-03 | Dyna-DINO | teacher 깊이 (조기 종료) | teacher의 최종 층을 처음부터 목표로 삼지 않고, 앞쪽 층을 충분히 학습한 뒤 중간 층과 마지막 층으로 증류 목표를 이동함. | CKA 유사도 계산, 목표 teacher 층 전환 판단, 특징 차원을 맞추는 투영층. | 학습 FLOPs와 실제 학습시간 감소. | 메모에 없음 | 의미 영역 분할은 ADE20K, 인스턴스 검색은 Oxford·Paris 데이터셋에서 평가. | 4.3절의 의미 영역 분할·인스턴스 검색 평가. | |
| S-04 | Asymmetric Masked Distillation (AMD) | 마스킹 비율 | student보다 teacher의 마스킹 비율을 낮춰 더 많은 문맥을 보게 함. 공통 패치는 직접 정렬하고 teacher만 보는 패치는 생성 정렬. | 특징 생성기·정렬 손실. 대칭 마스킹보다 teacher 처리 토큰은 많음. | 무마스킹 특징 증류 대비 teacher 토큰 계산 감소. SSV2 200-epoch 비교에서 무마스킹 32시간, teacher 45%·student 90% 마스킹 AMD 19시간(Table 7). | 메모에 없음 | SSV2·Kinetics-400 영상, ImageNet-1K 이미지. MAE 기반 teacher·student. | Table 1·7, 부록 Figure 4·5. [원문](https://arxiv.org/html/2311.03149v2) | |
| S-05 | MaskAlign | 마스킹 (student 쪽) | student이 보는 패치의 특징과 teacher가 전체 이미지에서 추출한 대응 특징 사이의 일관성을 학습함. 동적 정렬 모듈(DA)을 사용하며 픽셀 복원 디코더를 제거함. | 전체 이미지의 teacher 특징 추출, 특징 정규화, 동적 정렬 모듈(DA)의 학습·계산. | 픽셀 복원 디코더의 계산 제거. | 메모에 없음 | COCO에서 객체 탐지와 인스턴스 분할을 평가. | 4.2절 COCO 평가, 부록 A 선형 프로빙, 부록 B 의미 영역 분할, 부록 C 배경 관련 평가. | |
| S-06 | Decoupled KD (DKD) | 손실 설계 | 정답 클래스 지식 증류(TCKD)와 비정답 클래스 지식 증류(NCKD)를 분리하고, 각 손실의 학습 기여도를 따로 조절함. 기존 KD에서 teacher의 정답 확신도에 따라 작아지던 NCKD의 계수를 독립적인 가중치로 바꿈. | TCKD·NCKD의 별도 확률 분포와 손실 계산, 두 손실의 가중치 설정. | 특징 정렬 모듈·메모리 뱅크를 사용하는 증류보다 단순한 로짓 손실로 학습. teacher 입력이나 실행 깊이를 줄이는 방식은 아님. | 메모에 없음 | 특징 전이 실험에서는 CIFAR-100에서 WRN-40-2 teacher로 증류한 WRN-16-2 student을 특징 추출기로 사용. | 4.2절 추가 분석의 특징 전이 실험. | |
| S-07 | Information Theoretic Representation Distillation | 손실 설계 | 특징의 상관·상호정보량 기반 손실 사용. 대조 증류의 별도 메모리 뱅크·음성 샘플 저장 없이 계산. | 배치 내 상관·Gram 행렬 계산과 student 특징 투영층. | 메모리 뱅크 기반 표현 증류 대비 저장·샘플 관리 부담 감소. | 메모에 없음 | CIFAR-100·ImageNet, 여러 CNN 조합. 이진 신경망 증류도 실험. | 손실 정의, 표현 전이 Table 4. [원문](https://arxiv.org/abs/2112.00459) | |
| S-08 | Understanding the Role of the Projector in KD | 부가 모듈 | 선형 프로젝터·배치 정규화·LogSum 거리로 특징 증류. 별도 메모리 뱅크나 명시적 관계 구조를 만들지 않음. | 프로젝터 학습·배치 정규화·특징 손실 계산. | 복잡한 관계 기반 증류 모듈 대신 작은 투영층 사용. | 메모에 없음 | CIFAR-100·ImageNet 분류, CNN→ViT 증류, COCO 탐지. | Figure 2·3, 다운스트림 평가. [원문](https://arxiv.org/html/2303.11098v5) | |
| S-09* | Parameter-Efficient and Student-Friendly Knowledge Distillat… | 미정 | 고정된 teacher 본체 대신 작은 어댑터만 갱신해 teacher를 student에 맞게 적응시킴. | 어댑터 학습·teacher 순전파. frozen-teacher KD에 없던 teacher 적응 비용이 추가됨. | teacher 전체를 갱신하는 FT-KD 대비 적은 갱신 파라미터. ImageNet Table 3 batch time: FT-KD 1.44, PESF-KD 0.47, vanilla KD 0.46(표의 단위 그대로). | 메모에 없음 | CIFAR-100·ImageNet. ImageNet ResNet50→ResNet18. NLP GLUE도 포함. | Figure 1, Table 3, 일관성 분석. [저자 코드](https://github.com/1170300319/PESF-KD) | |
| S-10* | Knowledge Distillation from A Stronger Teacher | 미정 | 예측 확률의 정확한 일치 대신 클래스 간·샘플 간 Pearson 상관을 정렬. | 클래스·배치 방향의 상관 계산. 강한 teacher를 쓰는 경우 teacher 순전파 비용도 별도 고려. | 특징 정렬 모듈·메모리 뱅크 없이 로짓에서 계산. 논문은 vanilla KD와 거의 같은 학습 비용이라고 설명. | 메모에 없음 | CIFAR-100·ImageNet, 강한 teacher와 다양한 student. 객체 탐지·분할에도 적용. | §1·3, 탐지·분할 평가. 외부 강건성 Table 3. [공식 코드](https://github.com/hunto/DIST_KD) | |

## 치른 것에 대한 문헌 근거

| 번호 | 논문 | 평균 정확도 외에 본 지표 | 어떻게 변했나 | 논문이 밝힌 한계·실패 조건 (메모의 "잃는 것") | 위치 | 확인 |
|---|---|---|---|---|---|---|
| S-01 | MaskedKD | 선형 평가 정확도 (DINO 적용 Table 4, 선형 분류기만 증류 Table 8), 검출 AP·분할 mIoU (Table 12). 그룹별·분포 이동·손상 강건성 평가는 없음. | 메모 참고 | [실측·원논문] 정확도 손실 거의 없음: logit KD 75.0 → 50% 유지 75.2, 40% 유지 74.9. 25% 유지는 teacher에 따라 74.7~74.8. (Table 2) [실측·원논문] 영상 밖에서는 여유가 작음: 오디오(ESC-50)는 50% 유지에서 86.3 →… | Table 2 (정확도·FLOPs·처리량), Table 3·4 (오디오, DINO), Table 9 (증강)… | |
| S-02 | DMAE | 4.7절에서 여러 seed의 평균과 표준편차를 제시해 성능 향상의 안정성을 확인함. | 메모 참고 | [구조] teacher의 중간 특징과 패치 위치 대응이 필요함. teacher·student이 같은 보이는 패치를 사용하는 정렬 방식. [원문](https://arxiv.org/abs/2208.12256) | 4.7절 Standard Deviation Analysis. | |
| S-03 | Dyna-DINO | 의미 영역 분할: 선형 영역 분할, 인코더 전용 마스크 트랜스포머(EOMT), 다중 스케일(MS) 추론의 세 가지 프로토콜 사용. 인스턴스 검색: 백본에서 이미지별 전역 임베딩을 추출하고, 질의 이미지와 데이터베이스… | 메모 참고 | [구조] CKA와 목표 층 전환에 teacher의 중간 특징이 필요함. 최종 출력만 제공하는 teacher에는 그대로 적용할 수 없음. [공식 코드](https://github.com/KevinZ0217/Dyna-DINO) | 4.3절의 의미 영역 분할·인스턴스 검색 평가. | |
| S-04 | Asymmetric Masked Distillation (AMD) | AVA 행동 탐지 mAP, UCF101·HMDB51 전이, 행동 종류별 결과, 평균 어텐션 거리. | 메모 참고 | [구조] student이 보는 패치는 teacher가 보는 패치의 부분집합이어야 함. teacher·student 마스크를 서로 독립적으로 정하는 방식은 아님. [§3.3](https://arxiv.org/html/2311.03149v2#S3.SS3) | Table 1·7, 부록 Figure 4·5. [원문](https://arxiv.org/html/2311.0… | |
| S-05 | MaskAlign | 객체 탐지 APbox, 인스턴스 분할 APmask. 부록의 선형 프로빙·의미 영역 분할·배경 관련 평가도 확인. | 메모 참고 | [실측·외부] COCO 물체 특징 분류에서 배경을 교체하면 Top-1 77.4→53.8%. 마스킹 자체가 원인인지를 분리한 실험은 아님. [Mind Your Augmentation, Table 3](https://proceedings.iclr.cc/paper_files/paper/202… | 4.2절 COCO 평가, 부록 A 선형 프로빙, 부록 B 의미 영역 분할, 부록 C 배경 관련 평가. | |
| S-06 | Decoupled KD (DKD) | 특징 추출기를 고정하고 STL-10·Tiny-ImageNet에서 선형 프로빙을 수행해 특징의 전이 가능성을 평가. | 메모 참고 | [실측·외부] WRN-40-2→ResNet-20, CIFAR-100-C에서 student 단독 대비 ECE 0.2258→0.2379(3-seed 평균, 낮을수록 좋음). 통계적 유의성은 확인되지 않음. [CSV](https://github.com/Evowind/confident-dunc… | 4.2절 추가 분석의 특징 전이 실험. | |
| S-07 | Information Theoretic Representation Distillation | CIFAR-100 특징을 고정한 STL-10·Tiny-ImageNet 선형 프로빙. | 메모 참고 | [구조] teacher·student의 중간 특징을 배치 단위로 받아 상관·Gram 행렬을 계산함. 로짓만 제공하는 teacher API에는 그대로 적용할 수 없음. [공식 코드](https://github.com/roymiles/ITRD) | 손실 정의, 표현 전이 Table 4. [원문](https://arxiv.org/abs/2112.00459) | |
| S-08 | Understanding the Role of the Projector in KD | 특이값 변화, 프로젝터 입력·출력 상관, 객체 탐지 AP. | 메모 참고 | [실측·원논문] 정규화 비교에서 프로젝터 가중치의 일부 특이값이 학습 중 0에 가까워지는 현상을 측정함. 특정 인식 능력 손실을 별도로 측정한 결과는 아님. [Figure 2](https://arxiv.org/html/2303.11098v5) [실측·원논문] 큰 MLP 프로젝터에서 입력… | Figure 2·3, 다운스트림 평가. [원문](https://arxiv.org/html/2303.11098… | |
| S-09* | Parameter-Efficient and Student-Friendly Knowledge Distillat… | teacher·student KL divergence, 예측 일치율(Agree), 논문 정의의 PL·Gap. | 메모 참고 | [구조] teacher에 어댑터를 넣고 학습해야 함. 수정할 수 없는 고정 teacher API만으로는 해당 teacher 적응을 수행할 수 없음. [저자 코드](https://github.com/1170300319/PESF-KD) | Figure 1, Table 3, 일관성 분석. [저자 코드](https://github.com/117030… | |
| S-10* | Knowledge Distillation from A Stronger Teacher | 탐지 AP, Cityscapes mIoU. 외부 연구는 ImageNet-C·R·Sketch·A·Stylized-ImageNet 평가. | 메모 참고 | [실측·외부] CLIP ViT-L/14→ViT-B/16에서 일반 KD 대비 ImageNet-R 51.5→40.8%, Sketch 34.7→26.9%. [외부 Table 3](https://arxiv.org/html/2311.01441v2#S4) [실측·외부] 같은 실험에서 ImageNe… | §1·3, 탐지·분할 평가. 외부 강건성 Table 3. [공식 코드](https://github.com/h… | |

## 하위 갈래

(갈래 이름과 한 줄 정의)

## 축 문단

(5–7문장: 무엇을 빼는가 / 무엇이 새로 드는가 / 공통으로 적지 않는 것)

## 경계 논문과 뺀 논문

| 논문 | 결정 | 이유 |
|---|---|---|
| | | |
