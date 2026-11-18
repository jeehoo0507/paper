# Efficient Vision KD Survey (진행 중)

**논문 제목**: *Efficient Knowledge Distillation for Vision: A Survey and Controlled Study of What Is Saved and What Is Lost*

AIM Lab 서베이 작업 공간. 처음 온 사람이 **뭘 하는지, 어디부터 보면 되는지** 빠르게 파악하기 위한 안내.

## 한 줄 요약

비전 지식 증류(KD)를 **학습시키는 데 드는 비용**(teacher 실행, 데이터, 학습 길이)을 줄이는 방법들을 모아 분류하고, 같은 기준으로 비용을 비교하고, 평균 정확도 말고 무엇을 잃는지까지 보는 서베이.

## 왜 하나

- KD는 학습할 때 teacher를 매 스텝 돌려서 비쌈 (TinyViT 기준 학습 시간 1.7–2.4배).
- 기존 KD 서베이는 "어떤 지식을 옮기나"로 분류하고 **학습 비용 축이 없음**.
- 효율 KD 논문들은 비용을 세는 범위가 제각각이라 **숫자를 직접 비교할 수 없음**.
- 효율화하면 평균 정확도는 비슷해도 소수 그룹·특정 클래스에서 손해를 볼 수 있는데 **잘 안 봄**.

## 무엇을 내놓나

형태는 서베이 + 보고 실태 집계 + 통제 실험. 논문을 나열하지 않고, 효율 KD에서 생기는 현상을 숫자로 보이는 것이 목표.

| # | 기여 | 내용 | 상태 |
|---|---|---|---|
| 1 | 분류 | 비용을 줄이는 방식 기준 4축: **Shrink**(teacher 입력 축소) / **Skip**(데이터·구간 생략) / **Reuse**(teacher 출력 저장·재사용) / **Substitute**(teacher·데이터 대체) | 초안 |
| 2 | 비용 계산 틀과 보고 실태 | 일회성 / 반복 / 저장 비용으로 나눠 세는 기준, 논문들이 무엇을 세고 안 세는지 집계 | 집계 수치 있음, 비용식은 후보 |
| 3 | 통제 실험 | 축마다 예산을 단계별로 바꿔 평균 밖 지표(그룹, 최악 클래스, 분포 이동 등)가 꺾이는 지점을 잼 | Shrink 축은 기존 결과 있음, 나머지 예정 |
| 4 | 권장 사항 | 상황 × 축별로 같이 봐야 할 지표 | 실험 후 |

## 어디부터 보면 되나

처음이면 위에서부터 순서대로:

1. [`our paper.md`](<efficient-vision-kd/our paper.md>): **방향, 제목, 목차 확정본**
2. [`experiments/PLAN.md`](efficient-vision-kd/experiments/PLAN.md): **실험 계획** (기존 결과 포함)
3. [`PROGRESS.md`](efficient-vision-kd/PROGRESS.md): 지금까지 어떻게 여기까지 왔나
4. [`README.md`](efficient-vision-kd/README.md): 초기 정리 (주제, Q1~Q3)
5. [`presentation/index.html`](efficient-vision-kd/presentation/index.html): 발표 자료. 방향 확정 전 버전이라 제목과 실험 장이 예전 내용

논문 읽으러 왔으면:

- [`Related Work/Efficient KD paper.md`](<efficient-vision-kd/Related Work/Efficient KD paper.md>): 읽을 목록. **맨 위 17편이 우선**
- [`Related Work/Efficient KD papers/`](<efficient-vision-kd/Related Work/Efficient KD papers>): 축별 PDF 62편 (폴더마다 README에 목록)
- [`notes/`](efficient-vision-kd/notes): 읽으면서 남길 메모. `notes/README.md`에 뭘 적을지 가이드

## 폴더 구조

```
efficient-vision-kd/
├── README.md            주제·주장·Q1~Q3
├── PROGRESS.md          진행 과정
├── our paper.md         방향, 제목, 목차 (확정)
├── experiments/PLAN.md  실험 계획
├── presentation/        발표 자료
├── notes/               논문 읽기 메모 (축별 파일)
└── Related Work/
    ├── Efficient KD paper.md      읽을 논문 목록
    ├── Efficient KD papers/       PDF 62편 (1_Shrink ~ 6_Evaluation)
    └── survey paper/              참고한 기존 서베이 (en: PART 1~3, ko: PART 4)
```

## 지금 상태 (2026-11-18)

| 항목 | 상태 |
|---|---|
| 방향, 제목, 목차 | 확정 |
| 분류 축 | 초안 (논문 읽으며 조정 가능) |
| 비용 계산 방식 | 후보만 |
| 논문 읽기 | 17편 우선. `notes/1_Shrink.md`에 초안 있음 |
| 실험 | Shrink 축은 기존 결과 있음. 나머지는 설계 확정, 미실행 |
| 논문 초안 | 목차만 |

## 참여할 때 규칙

- 분류·주장·수치는 **초안**. 확정된 것처럼 쓰지 않기.
- 다른 논문의 숫자를 그대로 합쳐서 비교하지 않기 (논문마다 세는 범위가 다름).
- 논문을 읽으면 `notes/`의 해당 축 파일에 메모 추가.
