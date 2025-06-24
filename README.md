# DF-GAN

> **해당 Repository는 **[**DF-GAN**](https://github.com/tobran/DF-GAN)** 실행 가이드를 보강하여 구성한 프로젝트입니다.**

---

## 📋 요구 사항

- **운영체제**: Ubuntu
- **Python**: 3.8
- **PyTorch**: 1.9 (CUDA 버전에 맞춰 설치)
- **GPU**: 12GB 이상 (예: RTX 3090)

---

## 🚀 설치

```bash
# 저장소 클론
git clone https://github.com/tobran/DF-GAN.git
cd DF-GAN

# 가상환경 생성 (conda 예시)
conda create -n DFGAN python=3.8 -y
conda activate DFGAN

# 의존성 설치
pip install -r requirements.txt
```

---

## 📂 데이터 준비

1. **메타데이터 다운로드**

   - Birds: [preprocessed metadata](https://drive.google.com/file/d/1I6ybkR7L64K8hZOraEZDuHh0cCJw5OUj/view?usp=sharing)
   - coco : [preprocessed metadata](https://drive.google.com/file/d/15Fw-gErCEArOFykW3YTnLKpRcPgI_3AB/view)
   - 압축을 풀어 `data/` 디렉터리에 위치시킵니다.

2. **이미지 데이터 다운로드**

   - CUB-200-2011 (Birds): [다운로드 링크](https://www.vision.caltech.edu/datasets/cub_200_2011/)
   - `data/birds/` 폴더에 압축을 풀어주세요.
   - coco2014: [다운로드 링크](https://cocodataset.org/#download)
   - `data/coco/images` 폴더에 압축을 풀어주세요.
   

---

## 🔥 학습

```bash
cd DF-GAN/code/
bash scripts/train.sh ./cfg/bird.yml   # Birds dataset
bash scripts/train.sh ./cfg/coco.yml   # COCO dataset
```

**학습 재개**\
`scripts/train.sh` 내 파라미터 수정으로 중단된 지점부터 이어서 학습 가능:

```bash
# 재개할 epoch
resume_epoch=10
# 불러올 체크포인트 경로
resume_model_path=./models/checkpoint_epoch_10.pth
```

---

## 📊 평가 (FID)

### Pretrained 모델

- **Bird**: [다운로드](https://drive.google.com/file/d/1rzfcCvGwU8vLCrn5reWxmrAMms6WQGA6/view?usp=sharing) 후 `code/saved_models/bird/`에 압축 해제
- **coco**: [다운로드](https://drive.google.com/file/d/1e_AwWxbClxipEnasfz_QrhmLlv2-Vpyq/view) 후 `code/saved_models/coco/`에 압축 해제

### FID 계산

```bash
cd DF-GAN/code/
bash scripts/calc_FID.sh ./cfg/bird.yml   # Birds dataset
bash scripts/calc_FID.sh ./cfg/coco.yml   # COCO dataset
```

- 합성 이미지(약 3만 장)를 저장하려면, `*.yml` 설정에서 `save_image: True`로 변경하세요.

---

## 🎨 샘플링

```bash
cd DF-GAN/code/
```

### 1. 예제 캡션으로 이미지 합성

```bash
bash scripts/sample.sh ./cfg/bird.yml    # Birds
bash scripts/sample.sh ./cfg/coco.yml    # COCO
```

### 2. 직접 캡션 작성 후 합성

1. `./code/example_captions/<dataset_name>.txt` 파일 수정
2. 동일 스크립트 실행:

```bash
bash scripts/sample.sh ./cfg/bird.yml    # Birds
bash scripts/sample.sh ./cfg/coco.yml    # COCO
```

- 생성된 이미지는 `./code/samples/` 폴더에 저장됩니다.

#### Prompt 예시

```text
this bird has wings that are black and has a red belly
```
<img src="https://github.com/user-attachments/assets/fa2c7c8d-3c98-4bf4-898f-65be747b653b" width="300" alt="bird example"/>

---

## 📑 인용

```bibtex
@inproceedings{tao2022df,
  title        = {DF-GAN: A Simple and Effective Baseline for Text-to-Image Synthesis},
  author       = {Tao, Ming and Tang, Hao and Wu, Fei and Jing, Xiao-Yuan and Bao, Bing-Kun and Xu, Changsheng},
  booktitle    = {Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition},
  pages        = {16515--16525},
  year         = {2022}
}
```



