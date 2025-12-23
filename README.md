# 🔧 Metal Defect Synthesis PoC

> **LlamaGen VQGAN + Halton-MaskGIT 기반 금속 표면 결함 이미지 합성**

[![Hugging Face Spaces](https://img.shields.io/badge/🤗%20Hugging%20Face-Spaces-blue)](https://huggingface.co/spaces/Yumi-Park996/metal-defect-synthesis)
[![Hugging Face Models](https://img.shields.io/badge/🤗%20Hugging%20Face-Models-yellow)](https://huggingface.co/Yumi-Park996/metal-defect-checkpoints)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1U0dUqou_aCxwloasepwBF5jtioOjeMju?usp=sharing)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

⚠️ **PoC 단계** - 생성 품질 개선 작업 진행 중

---

## 📋 프로젝트 개요

제조업 품질 관리에서 **희귀 결함 데이터 부족** 문제를 해결하기 위한 생성 모델 PoC입니다.

**핵심 기술:**
- **LlamaGen VQGAN**: 8차원 codebook 기반 효율적인 이미지 토큰화
- **Halton-MaskGIT**: 저불일치 시퀀스 기반 마스크 스케줄링 (ICLR 2025)
- **Inpainting**: 정상 금속 표면에 특정 결함 합성

---

## 🎯 현재 상태

| 항목 | 상태 | 비고 |
|------|------|------|
| VQGAN Fine-tuning | ✅ 완료 | Edge IoU +10.6% 개선 |
| MaskGIT 학습 | ⚠️ 수렴 중 | Loss 6.77 (목표: ~4.0) |
| Gradio 데모 | ✅ 동작 | 생성 품질 개선 필요 |
| 생성 품질 | 🔄 개선 중 | 데이터 부족으로 인한 한계 |

### 알려진 한계점
- MaskGIT 학습 데이터 부족 (21K 샘플 vs 권장 1M+)
- 클래스 간 차이가 미미한 생성 결과
- 텍스처 일관성 개선 필요

---

## 📁 프로젝트 구조

```
metal-defect-synthesis/
├── V0/                                              # 버전 0 (현재)
│   ├── Metal_Defect_Synthesis_PRD_v2_0.pdf          # 📄 상세 PRD 문서
│   ├── metal_defect_synthesis(PoCFinal).ipynb       # 1️⃣ VQGAN Fine-tuning
│   ├── metal_defect_HaltonMaskGIT(PoCFinal).ipynb   # 2️⃣ MaskGIT 학습
│   └── metal_defect_gradio_demo_LlamaGen_Halton(PoCFinal).ipynb  # 3️⃣ Gradio 데모
└── README.md
```

---

## 🚀 실행 방법

### Google Colab에서 바로 실행 (권장)

> 💡 **GitHub에서 .ipynb 파일이 안 열릴 때** Colab 링크를 사용하세요!

| 단계 | 노트북 | Colab 링크 | 소요 시간 |
|------|--------|------------|----------|
| 1️⃣ | VQGAN Fine-tuning | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1U0dUqou_aCxwloasepwBF5jtioOjeMju?usp=sharing) | ~2시간 |
| 2️⃣ | MaskGIT 학습 | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1utaTLpAMD-OXp56mapU7EfrxusC67JUN?usp=sharing) | ~2시간 |
| 3️⃣ | Gradio 데모 | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1sbftaG4L7rvDh2ZVA7U3EWxtThpxzGZU?usp=sharing) | ~10분 |

### 사전 요구사항
- Google Colab (GPU 런타임 권장: **A100**)
- Google Drive 연동 (체크포인트 저장용)

### 실행 순서

```bash
# 1️⃣ VQGAN Fine-tuning (약 2시간)
# - LlamaGen VQGAN을 금속 결함 도메인에 맞게 fine-tuning
# - 결과: Edge IoU +10.6% 개선

# 2️⃣ MaskGIT 학습 (약 2시간)  
# - Halton-MaskGIT Transformer 학습
# - 결과: 조건부 이미지 생성 모델

# 3️⃣ Gradio 데모 실행
# - 인터랙티브 결함 합성 데모
```

---

## 📊 데이터셋

| 데이터셋 | 이미지 수 | 출처 |
|----------|-----------|------|
| NEU-DET | 1,440장 | [Kaggle](https://www.kaggle.com/datasets/kaustubhdikshit/neu-surface-defect-database) |
| SD-saliency-900 | 900장 | [Kaggle](https://www.kaggle.com/datasets/alex000kim/sdsaliency900) |
| X-SDD | 319장 | [Kaggle](https://www.kaggle.com/datasets/sayelabualigah/x-sdd) |
| **합계** | **2,659장** | 8배 증강 → 21,272 샘플 |

### 결함 클래스 (6종)

| 클래스 | 한글명 | 이미지 수 |
|--------|--------|-----------|
| `scratches` | 스크래치 | 674장 |
| `patches` | 패치 | 662장 |
| `inclusion` | 개재물 | 540장 |
| `rolled-in_scale` | 압연 스케일 | 303장 |
| `pitted_surface` | 피팅 | 240장 |
| `crazing` | 균열 | 240장 |

---

## 🏗️ 모델 아키텍처

### LlamaGen VQGAN
| 구성요소 | 스펙 |
|----------|------|
| Codebook 크기 | 16,384 토큰 |
| Codebook 차원 | 8 (taming 대비 32배 압축) |
| 다운샘플링 | 16x (256×256 → 16×16 = 256 토큰) |

### Halton-MaskGIT Transformer
| 구성요소 | 스펙 |
|----------|------|
| 파라미터 | ~69M (Small 설정) |
| Layers | 12 |
| Attention Heads | 8 |
| Hidden Dimension | 512 |
| 특징 | AdaLayerNorm, SwiGLU FFN, QK Normalization |

---

## 📈 개선 로드맵

### 단기 (현재 아키텍처 유지)
- [ ] 학습 epochs 증가 (100 → 500+)
- [ ] Learning rate 조정 (Warmup 적용)
- [ ] Weighted sampling으로 클래스 밸런싱

### 중장기
- [ ] 추가 데이터셋 확보 (MVTec AD, GC10-DET 등)
- [ ] Two-stage 학습 (unconditional → conditional)
- [ ] Stable Diffusion Inpainting 기반 접근법 검토

---

## 📚 참고 자료

| 자료 | 링크 |
|------|------|
| LlamaGen | [GitHub](https://github.com/FoundationVision/LlamaGen) |
| Halton-MaskGIT | [GitHub](https://github.com/valeoai/Halton-MaskGIT) |
| MaskGIT 논문 | [arXiv](https://arxiv.org/abs/2202.04200) |
| Surface Defect Detection | [GitHub](https://github.com/Charmve/Surface-Defect-Detection) |

---

## 📄 문서

- [PRD v2.0](./V0/Metal_Defect_Synthesis_PRD_v2_0.pdf) - 상세 기술 명세 및 한계점 분석

---

## 📝 버전 이력

| 버전 | 일자 | 변경 내용 |
|------|------|----------|
| v2.0 | 2024-12-12 | LlamaGen VQGAN + Halton-MaskGIT 전환 |
| v1.1 | 2024-12-11 | 피드백 반영 (증강 8배, MaskGIT 설정 수정) |
| v1.0 | 2024-12-11 | 초안 작성 (taming VQGAN + 직접 구현 MaskGIT) |

---

**Note**: 이 프로젝트는 PoC 단계이며, 생성 품질 개선을 위해 지속적으로 업데이트됩니다.

## 📧 Contact

문의사항이 있으시면 이슈를 등록해주세요.
