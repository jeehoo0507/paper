# Efficient Vision KD Survey (진행 중)

AIM Lab 서베이 작업 공간. 처음 온 사람이 **뭘 하는지, 어디부터 보면 되는지** 빠르게 파악하기 위한 안내.

## 한 줄 요약

비전 지식 증류(KD)를 **학습시키는 데 드는 비용**(teacher 실행, 데이터, 학습 길이)을 줄이는 방법들을 모아 분류하고, 같은 기준으로 비용을 비교하고, 평균 정확도 말고 무엇을 잃는지까지 보는 서베이.

## 왜 하나

- KD는 학습할 때 teacher를 매 스텝 돌려서 비쌈 (TinyViT 기준 학습 시간 1.7–2.4배).
- 기존 KD 서베이는 "어떤 지식을 옮기나"로 분류하고 **학습 비용 축이 없음**.
- 효율 KD 논문들은 비용을 세는 범위가 제각각이라 **숫자를 직접 비교할 수 없음**.
- 효율화하면 평균 정확도는 비슷해도 소수 그룹·특정 클래스에서 손해를 볼 수 있는데 **잘 안 봄**.

## 무엇을 내놓나 (기여 3개)

| # | 질문 | 내용 | 상태 |
|---|---|---|---|
| 1 | 어떤 방법들이 있나? | 비용을 줄이는 방식 기준 4축 분류: **Shrink**(teacher 입력 축소) / **Skip**(데이터·구간 생략) / **Reuse**(teacher 출력 저장·재사용) / **Substitute**(teacher·데이터 대체) | 초안 |
| 2 | 비용을 어떻게 비교하나? | 일회성 / 반복 / 저장 비용으로 나눠 세는 공통 기준 | 후보만, 미확정 |
| 3 | 효율화하면 무엇을 잃나? | 평균 정확도 + WGA·최악 클래스·분포 이동 등 지표 세트 | 후보만, 실험 예정 |

## 어디부터 보면 되나

처음이면 위에서부터 순서대로:

1. [`PROGRESS.md`](efficient-vision-kd/PROGRESS.md): 지금까지 어떻게 여기까지 왔나 (5분)
2. [`presentation/index.html`](efficient-vision-kd/presentation/index.html): 발표 자료 12장. 내려받아 브라우저로 열고 ←/→
3. [`README.md`](efficient-vision-kd/README.md): 주제·주장·Q1~Q3 상세
4. [`our paper.md`](<efficient-vision-kd/our paper.md>): 논문 목차 초안
5. [`experiments/PLAN.md`](efficient-vision-kd/experiments/PLAN.md): 실험 계획 초안

논문 읽으러 왔으면:

- [`Related Work/Efficient KD paper.md`](<efficient-vision-kd/Related Work/Efficient KD paper.md>): 읽을 목록. **맨 위 17편이 우선**
- [`Related Work/Efficient KD papers/`](<efficient-vision-kd/Related Work/Efficient KD papers>): 축별 PDF 62편 (폴더마다 README에 목록)
- [`notes/`](efficient-vision-kd/notes): 읽으면서 남길 메모. `notes/README.md`에 뭘 적을지 가이드

## 폴더 구조

```
efficient-vision-kd/
├── README.md            주제·주장·Q1~Q3
├── PROGRESS.md          진행 과정
├── our paper.md         논문 목차 초안
├── experiments/PLAN.md  실험 계획 (미실행)
├── presentation/        발표 자료
├── notes/               논문 읽기 메모 (축별 파일)
└── Related Work/
    ├── Efficient KD paper.md      읽을 논문 목록
    ├── Efficient KD papers/       PDF 62편 (1_Shrink ~ 6_Evaluation)
    └── survey paper/              참고한 기존 서베이 (en: PART 1~3, ko: PART 4)
```

## 지금 상태

| 항목 | 상태 |
|---|---|
| 주제·분류 축 | 거의 정해짐 (논문 읽으며 조정 가능) |
| 비용 계산 방식 | 후보만 |
| 논문 읽기 | 17편 우선 읽는 중 |
| 실험 | 계획만, 서버에서 곧 시작 |
| 논문 초안 | 목차만 |

## 참여할 때 규칙

- 분류·주장·수치는 **초안**. 확정된 것처럼 쓰지 않기.
- 다른 논문의 숫자를 그대로 합쳐서 비교하지 않기 (논문마다 세는 범위가 다름).
- 논문을 읽으면 `notes/`의 해당 축 파일에 메모 추가.
