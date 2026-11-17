# Shrink: 한 스텝 비용을 줄임

| # | 논문 | 한 줄 | 링크 | 파일 |
|---|---|---|---|---|
| 1 | MaskedKD (The Role of Masking for Efficient Supervised KD of ViTs) | teacher 입력 토큰을 student attention으로 골라 줄임 | [arXiv: 2302.10494](https://arxiv.org/abs/2302.10494) | `01_MaskedKD (The Role of Masking for Efficient Supervised KD of ViTs).pdf` |
| 2 | DMAE (Masked Autoencoders Enable Efficient Knowledge Distillers) | 보이는 패치만 쓰고 teacher는 앞쪽 층만 돌림 | [arXiv: 2208.12256](https://arxiv.org/abs/2208.12256) | `02_DMAE (Masked Autoencoders Enable Efficient Knowledge Distillers).pdf` |
| 3 | Dyna-DINO | 학습 초반에 teacher 추론을 중간 층에서 멈춤 | [arXiv: 2606.19483](https://arxiv.org/abs/2606.19483) | `03_Dyna-DINO.pdf` |
| 4 | Asymmetric Masked Distillation (AMD) | student는 높은 마스킹, teacher는 낮은 마스킹 | [arXiv: 2311.03149](https://arxiv.org/abs/2311.03149) | `04_Asymmetric Masked Distillation (AMD).pdf` |
| 5 | MaskAlign | student가 보이는 패치만 인코딩 | [arXiv: 2211.08887](https://arxiv.org/abs/2211.08887) | `05_MaskAlign.pdf` |
| 6 | Decoupled KD (DKD) | 싼 logit 손실로 feature KD 수준 | [arXiv: 2203.08679](https://arxiv.org/abs/2203.08679) | `06_Decoupled KD (DKD).pdf` |
| 7 | Information Theoretic Representation Distillation (ITRD) | 학습 오버헤드가 작은 손실 | [arXiv: 2112.00459](https://arxiv.org/abs/2112.00459) | `07_Information Theoretic Representation Distillation (ITRD).pdf` |
| 8 | Understanding the Role of the Projector in KD | projector 설계로 싼 레시피 | [arXiv: 2303.11098](https://arxiv.org/abs/2303.11098) | `08_Understanding the Role of the Projector in KD.pdf` |
