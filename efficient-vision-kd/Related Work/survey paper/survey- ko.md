## Summary: How to Use PART 4

**Context.** Korean papers recommended by AIM Lab, all published in the Journal of the Institute of Electronics and Information Engineers (IEIE, 전자공학회논문지), 2024–2026. Only #4 and #5 are on-topic for our survey (cost of the distillation process itself).

| # | Priority | What to take from it |
|---|---|---|
| 4 | **Essential** | Teacher-side caching for dense object detection. Its stated problem is ours: existing KD frameworks ignore the cost of running the teacher at every iteration. Storing raw teacher FPN features for COCO would need 4.0 TB; selecting key features with Soft-NMS cuts this to 12 GB. Table 1 reports **training time relative to the student** (FGD 1.68x, PGD 1.51x, BCKD 1.42x, ours 1.05x) and **storage space**. Use it as (a) the detection example of output caching, (b) evidence that **storage is a separate cost term** traded against teacher compute, and (c) an example of incomplete cost reporting: the one-time pass that runs the teacher over M augmented images to build the store is not reported separately from the 1.05x. Read its results critically: it beats FGD on GFL (43.8 vs 43.4 mAP) but not on FCOS (39.6 vs 41.0), and the abstracts overstate this as "superior". |
| 5 | Supporting | Image super-resolution (not classification); main contribution is pruning plus a distillation loss, not KD cost. Cite only Table 6: per-step training time differs up to 4x across KD methods at similar PSNR (BasicKD 0.410 s, AugKD 0.811 s, DCKD 1.661 s, ours 0.413 s). Supports the claim that KD methods differ greatly in training cost and rarely report it. |
| 1–3 | Not used | Not topic-relevant (time-series LLMs, spike encoding, UAV distributed learning). Do not use them as content sources for this survey. |

## PART 4. AIM Lab

| # | Title | Year | Link |
|---|---|---|---|
| 1 | LLM 모델의 시계열 데이터 단기 예측 성능 비교 연구 | 2025 | [DBpia](https://www.dbpia.co.kr/journal/articleDetail?nodeId=NODE12318964) |
| 2 | 시계열 데이터를 위한 스파이크 인코딩 최신 연구 동향 | 2024 | [DBpia](https://www.dbpia.co.kr/journal/articleDetail?nodeId=NODE11946242) |
| 3 | 무인 항공기 기반 분산 머신러닝 기술 동향 | 2024 | [DBpia](https://www.dbpia.co.kr/journal/articleDetail?nodeId=NODE11764556) |
| 4 | 비최대 억제를 통한 밀집 객체 검출을 위한 지식 저장 증류 | 2025 | [DBpia](https://www.dbpia.co.kr/journal/articleDetail?nodeId=NODE12055100) |
| 5 | 점진적 필터 가지치기와 적응형 응답 증류 기반 경량 이미지 초해상도 | 2026 | [DBpia](https://www.dbpia.co.kr/pdf/pdfView.do?nodeId=NODE12558170&width=1395) |
