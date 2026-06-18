## Installation

### Requirements

- Python 3.8 or higher
- PyTorch >= 1.7.0
- torchvision
- opencv-python
- Pillow
- NumPy
- SciPy
- FilterPy
- scikit-image
- tqdm
- matplotlib

### Install Dependencies

```bash
pip install torch torchvision
pip install opencv-python Pillow numpy scipy filterpy scikit-image tqdm matplotlib
```

### Install Pytorch Correlation Extension

Motion-DeAOT requires the Pytorch Correlation extension. We recommend installing it from source:

```bash
git clone https://github.com/ClementPinard/Pytorch-Correlation-extension.git
cd Pytorch-Correlation-extension
pip install .
cd ..
```

## Dataset Preparation

The repository supports both DAVIS datasets and the proposed distractor-rich benchmark.

Extract the dataset archive before evaluation.

### Dataset Structure

```text
Custom_distractor_dataset/
├── Annotations/
│   ├── sequence_1/
│   │   ├── 00000.png
│   │   ├── 00001.png
│   │   └── ...
│   ├── sequence_2/
│   └── ...
│
├── JPEGImages/
│   ├── sequence_1/
│   │   ├── 00000.jpg
│   │   ├── 00001.jpg
│   │   └── ...
│   ├── sequence_2/
│   └── ...
│
└── ImageSets/
    └── test.txt
```

### Directory Description

- **JPEGImages/** contains RGB video frames for each sequence.
- **Annotations/** contains ground-truth segmentation masks.
- **ImageSets/test.txt** contains the list of evaluation sequences, one sequence name per line.

Example:

```text
drone_still
eagle_attack_drone
multi_fighterjets
```

### Metric Evaluation

Compute IPS and F1:
```text
python metrics/compute_ips.py \
    --gt_root Custom_distractor_dataset/Annotations \
    --pred_root pred_masks \
    --output_dir results
```

Compute RR and ARD:
```text
python metrics/compute_recovery_metrics.py \
    --csv results/IPS_FRAMEWISE.csv \
    --output_csv results/RR_ARD_VIDEOWISE.csv
```

Generated files:
```text
results/
├── IPS_FRAMEWISE.csv
├── IPS_VIDEOWISE.csv
└── RR_ARD_VIDEOWISE.csv
```

### Semi-Supervised VOS Setting

Motion-DeAOT follows the semi-supervised VOS protocol:

- The first-frame ground-truth mask is provided.
- The model propagates the target mask to all subsequent frames.
- Predicted masks are generated for the entire sequence.
