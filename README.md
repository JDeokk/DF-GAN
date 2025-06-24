# DF-GAN

해당 레포지토리는 https://github.com/tobran/DF-GAN 기반으로 실행 가이드를 보강하여 구성한 GitHub 저장소입니다.

## 요구 사항
- Ubuntu
- python 3.8
- Pytorch 1.9(자신의 CUDA 버전에 맞게 설치)
- 1x12GB NVIDIA GPU 이상(필자는 RTX 3090 환경 사용)



해당 레포 clone
```
git clone https://github.com/JDeokk/DF-GAN
pip install -r requirements.txt
cd DF-GAN/code/
```


## 준비
### 데이터셋
1. 아래 링크에서 전처리된 메타데이터를 내려받아 data/ 폴더에 압축을 품
Birds 메타데이터: https://drive.google.com/file/d/1I6ybkR7L64K8hZOraEZDuHh0cCJw5OUj/view?usp=sharing

3. CUB-200-2011(Birds) 이미지 데이터를 다운로드한 뒤 data/birds/ 폴더에 압축을 품
다운로드 링크: http://www.vision.caltech.edu/visipedia/CUB-200-2011.html


## 학습  
  ```bash
  bash scripts/train.sh ./cfg/bird.yml
  ```

학습이 예기치 않게 중단된 경우, scripts/train.sh 내부의 다음 두 변수를 수정하여 이어서 학습할 수 있음

재개할 에폭 번호 (예: 10)
resume_epoch=10

로드할 모델 파일 경로
resume_model_path=./models/checkpoint_epoch_10.pth


## 평가

### Pretrained Model
- [DF-GAN for bird](https://drive.google.com/file/d/1rzfcCvGwU8vLCrn5reWxmrAMms6WQGA6/view?usp=sharing) 다운로드 후 /code/saved_models/bird/에 압축해제

### FID(Frechet Inception Distance) 계산

테스트 캡션으로부터 약 3만 장의 이미지를 합성한 뒤, 각 데이터셋의 **합성 이미지**와 **실제 테스트 이미지** 간 FID를 계산

```bash
cd DF-GAN/code/
bash scripts/calc_FID.sh ./cfg/bird.yml
```

### 추가 팁

- 평가 과정은 합성된 이미지(약 3만 장)를 기본적으로 저장하지 않음  
  이미지를 저장하려면, 해당 YAML 설정 파일에서 아래 항목을 `True`로 변경:

  ```yaml
  save_image: True
  ```

## 샘플링
```bash
cd DF-GAN/code/
```
### 예시 텍스트 설명을 이용하여 이미지 합성
  `bash scripts/sample.sh ./cfg/bird.yml`

### 직접 텍스트 설명을 작성하여 이미지 합성
  -  ./code/example_captions/dataset_name.txt 파일 내 텍스트 설명 수정 

        `bash scripts/sample.sh ./cfg/bird.yml`

  - 합성된 이미지는  ./code/samples에 저장됨

### Prompt (this bird has wings that are black and has a red belly) 예시:

<img src="https://github.com/user-attachments/assets/fa2c7c8d-3c98-4bf4-898f-65be747b653b" width="300" alt="bird example"/>


### DF-GAN 인용

```
@inproceedings{tao2022df,
  title={DF-GAN: A Simple and Effective Baseline for Text-to-Image Synthesis},
  author={Tao, Ming and Tang, Hao and Wu, Fei and Jing, Xiao-Yuan and Bao, Bing-Kun and Xu, Changsheng},
  booktitle={Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition},
  pages={16515--16525},
  year={2022}
}
```
