# 🔧 Metal Defect Synthesis PoC

> **LlamaGen VQGAN + Halton-MaskGIT 기반 금속 표면 결함 이미지 합성**

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/metal-defect-synthesis/blob/main/notebooks/metal_defect_gradio_demo_LlamaGen_Halton_PoCFinal_.ipynb)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

⚠️ **PoC 단계** - 생성 품질 개선 작업 진행 중

---

## 📋 프로젝트 개요

제조업 품질 관리에서 **희귀 결함 데이터 부족** 문제를 해결하기 위한 생성 모델 PoC입니다.

**핵심 기술:**
- **LlamaGen VQGAN**: 8차원 codebook 기반 효율적인 이미지 토큰화
- **Halton-MaskGIT**: 저불일치 시퀀스 기반 마스크 스케줄링 (ICLR 2025)
- **Inpainting**: 정상 금속 표면에 특정 결함 합성

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

## 📁 프로젝트 구조

```
metal-defect-synthesis/
├── notebooks/
│   ├── metal_defect_synthesis_llamagen_PoCFinal_.ipynb  # 1️⃣ VQGAN Fine-tuning
│   ├── metal_defect_HaltonMaskGIT_PoCFinal_.ipynb       # 2️⃣ MaskGIT 학습
│   └── metal_defect_gradio_demo_LlamaGen_Halton_PoCFinal_.ipynb  # 3️⃣ Gradio 데모
├── docs/
│   └── Metal_Defect_Synthesis_PRD_v2_0.pdf              # 상세 PRD
└── README.md
```

## 🚀 실행 방법

### 사전 요구사항
- Google Colab (GPU 런타임 권장: A100)
- Google Drive 연동 (체크포인트 저장용)

### 순서

1. **VQGAN Fine-tuning** (약 2시간)
   ```
   notebooks/metal_defect_synthesis_llamagen_PoCFinal_.ipynb
   ```

2. **MaskGIT 학습** (약 2시간)
   ```
   notebooks/metal_defect_HaltonMaskGIT_PoCFinal_.ipynb
   ```

3. **Gradio 데모 실행**
   ```
   notebooks/metal_defect_gradio_demo_LlamaGen_Halton_PoCFinal_.ipynb
   ```

## 📊 데이터셋

| 데이터셋 | 이미지 수 | 출처 |
|----------|-----------|------|
| NEU-DET | 1,440장 | [Link](http://faculty.neu.edu.cn/yunhyan/NEU_surface_defect_database.html) |
| SD-saliency-900 | 900장 | [Link](https://github.com/prsn670/SD-saliency-900) |
| X-SDD | 319장 | [Link](https://github.com/SDC-CVLAB/X-SDD) |
| **합계** | **2,659장** | 8배 증강 → 21,272 샘플 |

### 결함 클래스 (6종)
- `inclusion` (개재물) - 540장
- `scratches` (스크래치) - 674장  
- `patches` (패치) - 662장
- `pitted_surface` (피팅) - 240장
- `rolled-in_scale` (압연 스케일) - 303장
- `crazing` (균열) - 240장

## 🏗️ 모델 아키텍처

### LlamaGen VQGAN
- **Codebook**: 16,384 토큰, 8차원
- **다운샘플링**: 16x (256×256 → 16×16 = 256 토큰)
- **특징**: taming-transformers 대비 32배 압축된 codebook 차원

### Halton-MaskGIT Transformer
- **파라미터**: ~69M (Small 설정)
- **구조**: 12 layers, 8 heads, hidden 512
- **특징**: AdaLayerNorm, SwiGLU FFN, QK Normalization

## 📈 개선 로드맵

### 단기 (현재 아키텍처 유지)
- [ ] 학습 epochs 증가 (100 → 500+)
- [ ] Learning rate 조정 (Warmup 적용)
- [ ] Weighted sampling으로 클래스 밸런싱

### 중장기
- [ ] 추가 데이터셋 확보 (MVTec AD, GC10-DET 등)
- [ ] Two-stage 학습 (unconditional → conditional)
- [ ] 모델 사이즈 조정 (Tiny 23M 시도)

## 📚 참고 자료

- [LlamaGen](https://github.com/FoundationVision/LlamaGen) - Autoregressive Image Generation
- [Halton-MaskGIT](https://github.com/valeoai/Halton-MaskGIT) - ICLR 2025
- [MaskGIT](https://arxiv.org/abs/2202.04200) - Masked Generative Image Transformer

---

**Note**: 이 프로젝트는 PoC 단계이며, 생성 품질 개선을 위해 지속적으로 업데이트됩니다.
