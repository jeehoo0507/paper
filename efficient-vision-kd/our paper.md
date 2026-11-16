# Our Paper — 목차 초안

**제목 후보**: *What Efficient Distillation Leaves Unreported: Cost and Group Robustness in Vision KD*

**한 줄**: 효율 KD는 비용을 줄인 게 아니라 옮긴 경우가 많고, 그 대가를 평균 정확도로는 못 봄.

---

| 절                            | 내용                                                                   |
| ---------------------------- | -------------------------------------------------------------------- |
| **1. Intro**                 | KD 학습 비용 커짐 (큰 teacher × 긴 스케줄) → 논문끼리 비교 불가 → 손해 보는 그룹 안 봄          |
| **2. Scope & Prior Surveys** | 기존 KD 서베이: 비용 축 없음. Efficient ViT 서베이: student 추론만. LM 비용 서베이: 언어 한정 |
| **3. Cost Accounting** ★     | 비용식, 비교 기준 (README 핵심 1)                                             |
| **4. Four Strategies** ★     | Shrink / Skip / Reuse / Substitute × 비용 항 지도. 빈칸 = 미해결               |
| **5. 전략별 상세**                | 전략마다: 대표 2~3편 → 절감 숫자 → 옮겨 간 비용 → 강건성 근거 → 언제 쓰나                     |
| **6. 긴장 관계**                 | 긴 스케줄 vs 가속 / 캐싱 vs 증강 / 선택 비용 vs 절감                                 |
| **7. 보고 실태 조사**              | 논문 40~60편: 비용 단위·범위, 평균 외 평가 여부 집계                                   |
| **8. 사례: MaskedKD**          | 토큰 유지율 75/50/25/15%, CLS attn vs Rollout, Waterbirds WGA. 전체 비용 측정   |
| **9. Open Problems & 체크리스트** | 저해상도 teacher, 토큰 병합 teacher, 검출 저장 비용 등                              |
| **10. Conclusion**           |                                                                      |

★ = 핵심 기여

---

## 5절 대표 논문

| 전략 | 논문 | 강건성 근거 |
|---|---|---|
| Shrink | MaskedKD, DMAE, Dyna-DINO, DKD | 토큰 축소 → WGA 방향 엇갈림 |
| Skip | Distill on a Diet, Data Pruning KD, UNIX, Weak-to-Strong, DearKD | 프루닝 → 소수 그룹 불리 |
| Reuse | FKD, TinyViT, Dataset Reinforcement, MobileCLIP, 랩실 지식 저장 증류 | 근거 없음 (빈칸) |
| Substitute | 체크포인트 teacher, self-KD, Proteus, SRe2L | KD가 편향 전이·증폭 |

## 남은 일
- 3절 계산 예시 (MaskedKD·FKD 실제 숫자) — 논문 본문 확인 중
- 분류는 초록 기준 → 본문 확인 후 조정
- 8절 실험: 신규 결과 없음
