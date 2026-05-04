# LDL-SR: Locally Discriminative Learning for Remote-Sensing Super-Resolution

## Overview
This project implements Locally Discriminative Learning (LDL) to suppress hallucinated artifacts in GAN-based super-resolution for remote-sensing imagery. The approach integrates LDL into an ESRGAN framework to improve reconstruction fidelity while preserving structural consistency.

This work is based on the study:
Evaluating Locally Discriminative Learning for Hallucination Suppression in Remote-Sensing Super-Resolution.

## Motivation
GAN-based super-resolution methods can generate visually sharp images but often introduce artificial high-frequency details. These artifacts are problematic in remote sensing, where geometric accuracy is critical. LDL addresses this by applying targeted supervision to unstable regions instead of uniform penalties.

## Method

### Baseline
- ESRGAN with RRDB architecture

### LDL Components
1. Residual computation:
   R = I_HR - I_SR

2. Local variance estimation to detect unstable regions

3. EMA-based refinement:
   Compare predictions from current model and EMA model

4. Loss function:
   L_total = L_GAN + beta * L_artifact

## Dataset
UC Merced Land Use Dataset:
- 2,100 RGB images
- 21 classes
- Image size: 256x256

Preprocessing:
- Downsampling x4 (64x64 input)
- Train/Validation/Test split: 70/15/15
- Random crop: 128x128
- Data augmentation: flips, rotations, HSV jitter

## Training Setup
- Optimizer: Adam (beta1=0.9, beta2=0.999)
- Learning rate: 1e-4 with cosine decay
- Batch size: 16
- Training iterations: ~50,000
- Hardware: NVIDIA T4 GPU (16GB)

## Results

| Model        | PSNR | SSIM | LPIPS |
|-------------|------|------|------|
| ESRGAN      | 21.95 | 0.565 | 0.360 |
| NoGAN       | 23.39 | 0.623 | 0.372 |
| SwinIR      | 21.15 | 0.591 | 0.368 |
| LDL-SR      | 26.18 | 0.741 | 0.212 |

LDL improves both reconstruction fidelity and perceptual quality compared to baseline methods.

## Key Observations
- ESRGAN introduces hallucinated textures in structured regions
- Removing adversarial loss reduces artifacts but weakens detail
- LDL balances artifact suppression and detail preservation
- Best improvements observed in structured regions such as urban layouts and agricultural patterns

## Project Structure
LDL_UC-Merced/
- LDL_SR_Training.ipynb
- Result&Experiments.ipynb
- models/
- data/
- utils/
- README.md

## How to Run

1. Clone repository:
   git clone https://github.com/Sarah-Wai/LDL_UC-Merced.git
   cd LDL_UC-Merced

2. Install dependencies:
   pip install -r requirements.txt

3. Run training:
   jupyter notebook LDL_SR_Training.ipynb

4. View results:
   jupyter notebook Result&Experiments.ipynb

## Dependencies
- Python 3.x
- PyTorch
- OpenCV
- NumPy
- Matplotlib
- scikit-image

## Evaluation Metrics
- PSNR: pixel-level accuracy
- SSIM: structural similarity
- LPIPS: perceptual similarity

## Future Work
- Extend LDL to diffusion-based super-resolution
- Apply to larger remote-sensing datasets
- Explore self-supervised approaches
- Evaluate downstream tasks such as classification and detection

## Author
Wai Phu Paing 
MSc Computer Science  
University of Regina

## License
MIT License

## Citation
@article{wai2026ldl,
  title={Evaluating Locally Discriminative Learning for Hallucination Suppression in Remote-Sensing Super-Resolution},
  author={Wai, Paing and Yao, JingTao},
  year={2026}
}
