# Metrics Evaluation

Motion-DeAOT introduces three evaluation metrics for assessing identity preservation and recovery capability in distractor-rich video object segmentation scenarios:

* **F1 Score**: Measures segmentation quality using the harmonic mean of precision and recall.
* **Identity Preservation Score (IPS)**: Measures identity consistency by penalizing fragmented predictions.
* **Recovery Rate (RR)**: Measures the percentage of identity failures that successfully recover.
* **Average Recovery Delay (ARD)**: Measures the average number of frames required to recover from an identity failure.

---

## Metric Definitions

### F1 Score

The segmentation F1 score is computed as:

[
F_1=\frac{2PR}{P+R}
]

where (P) and (R) denote precision and recall, respectively.

---

### Identity Preservation Score (IPS)

IPS combines segmentation quality and identity consistency.

**Definition:**

```text
IPS = 0                  if Nc = 0
IPS = F1 / Nc           otherwise
```

where:

* **F1** is the segmentation F1 score.
* **Nc** is the number of connected components in the predicted mask.

A prediction consisting of multiple disconnected regions receives a lower IPS, reflecting reduced identity consistency.

---

### Recovery Rate (RR)

A failure event is triggered whenever:

```text
IPS < 0.5
```

A recovery event is declared when IPS remains above the threshold for **two consecutive frames**.

**Definition:**

```text
RR = (Recovered Failure Events / Total Failure Events) × 100
```

Higher RR indicates stronger recovery capability following identity failures.

---

### Average Recovery Delay (ARD)

For every recovered failure event, recovery delay is measured as the number of frames between failure detection and recovery.

**Definition:**

```text
ARD = (1 / N) × Σ di
```

where:

* **N** is the number of recovered failure events.
* **di** denotes the recovery delay of the *i-th* recovered event.

Lower ARD indicates faster recovery from tracking failures.

---

## Files

```text
metrics/
├── compute_ips.py
├── compute_recovery_metrics.py
└── README.md
```

---

## Step 1: Compute IPS and F1

Generate frame-wise and video-wise IPS statistics:

```bash
python compute_ips.py \
    --gt_root Custom_distractor_dataset/Annotations \
    --pred_root pred_masks \
    --output_dir results
```

Outputs:

```text
results/
├── IPS_FRAMEWISE.csv
└── IPS_VIDEOWISE.csv
```

### IPS_FRAMEWISE.csv

Contains metrics for every frame:

```text
video,image,num_components,precision,recall,f1_score,ips
```

### IPS_VIDEOWISE.csv

Contains average metrics for every sequence:

```text
video,num_frames,avg_precision,avg_recall,avg_f1,avg_ips
```

---

## Step 2: Compute RR and ARD

Compute recovery metrics using the generated frame-wise IPS values:

```bash
python compute_recovery_metrics.py \
    --csv results/IPS_FRAMEWISE.csv \
    --output_csv results/RR_ARD_VIDEOWISE.csv
```

Output:

```text
results/
└── RR_ARD_VIDEOWISE.csv
```

The script additionally reports:

* Total Failure Events
* Recovered Failure Events
* Global Recovery Rate (RR)
* Global Average Recovery Delay (ARD)

---

## Dataset Format

Ground-truth masks and predicted masks must follow the same directory structure:

```text
Annotations/
├── sequence_1/
│   ├── 00000.png
│   ├── 00001.png
│   └── ...
├── sequence_2/
└── ...

pred_masks/
├── sequence_1/
│   ├── 00000.png
│   ├── 00001.png
│   └── ...
├── sequence_2/
└── ...
```

Each prediction mask must correspond to the same frame name as the ground-truth mask.


