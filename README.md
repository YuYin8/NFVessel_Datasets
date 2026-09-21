# NFVessel: A Nearshore Vessel Detection Dataset for Complex Backgrounds and Occlusion

<div align="center">

[![Download](https://img.shields.io/badge/Download-NFVessel-blue)](https://pan.quark.cn/s/a221f5b69d0b)
![Task](https://img.shields.io/badge/Task-Vessel%20Detection-green)
![Annotation](https://img.shields.io/badge/Annotation-Bounding%20Boxes-orange)
![Status](https://img.shields.io/badge/Status-Public-brightgreen)

</div>

---

## Introduction

**NFVessel** is a task-specific benchmark dataset for vessel detection in complex nearshore environments.
The dataset was constructed to support research on vessel detection under challenging nearshore conditions, including normal navigation, berthing, vessel encounters, adjacent navigation, partial occlusion, distant small targets, low-contrast imaging, and structured background interference such as coastlines, buildings, water-surface reflections, waves, and vessel wakes.
NFVessel contains **10,326 images** and **19,035 annotated vessel instances**. All vessels are annotated as a single object category using rectangular bounding boxes.
The training, validation, and test sets are separated according to acquisition scene, acquisition time, and video clip to reduce information leakage caused by highly correlated adjacent video frames.

---

## Associated Manuscript

NFVessel is introduced and evaluated in the following work:

> **FRA-YOLO: Frequency-guided receptive-field adaptation for occluded ship detection in complex nearshore backgrounds**

Yu Yin, Qing Hu*, Yue Zhou, Yu Zhang, Shuaiheng Huai

School of Information Science and Technology, Dalian Maritime University  
National Engineering Research Center for Ship Navigation Systems

NFVessel is introduced in this work as a task-specific benchmark for evaluating vessel detection in complex nearshore environments, particularly under small-target, vessel-interaction, occlusion-related, and structured-background conditions.
The paper link and complete bibliographic information will be updated after publication.

---

## News

- **2026-09-21**: NFVessel v1.0 is publicly released.
---

## Dataset Overview

| Item | Description |
|---|---:|
| Dataset | NFVessel |
| Task | Vessel detection |
| Number of classes | 1 |
| Class name | `ship` |
| Total images | **10,326** |
| Total vessel instances | **19,035** |
| Training images | **6,243** |
| Validation images | **1,780** |
| Test images | **2,303** |
| Average instances per image | **1.84** |
| Original video resolution | **2560 × 1440** |
| Annotation type | Rectangular bounding boxes |
| Annotation tool | CVAT |

The official dataset split is:

```text
Train:       6,243 images
Validation:  1,780 images
Test:        2,303 images
-----------------------
Total:      10,326 images
```

The image split is consistent with that used in the associated manuscript.

---

## Data Collection

NFVessel was collected from shore-based optical observations of nearshore waters in Qingdao, China.

### Acquisition Information

| Item | Description |
|---|---|
| Location | Qingren Dam, Olympic Sailing Center, Qingdao, Shandong Province, China |
| Camera | Hikvision DS-2DC4423IW-DE |
| Original resolution | 2560 × 1440 |
| Acquisition period | 29 March 2026 – 1 April 2026 |
| Imaging modality | RGB optical imagery |
| Viewpoint | Shore-based camera |

The dataset contains representative nearshore conditions including:

- normal vessel navigation;
- vessel berthing;
- vessel encounters;
- adjacent vessel navigation;
- partially occluded vessels;
- distant small-scale vessels;
- low-contrast imaging;
- water-surface reflections;
- waves and vessel wakes;
- coastline structures;
- buildings and other nearshore background structures.

The current NFVessel release was collected over a four-day acquisition period and therefore does not represent full seasonal or long-term environmental variability.

---

## Dataset Split

To reduce information leakage caused by adjacent frames from the same video sequence, NFVessel is **not randomly divided at the individual-frame level**.

Instead, the training, validation, and test sets are separated according to:

- acquisition scene;
- acquisition time;
- video clip.

The official split is:

| Split | Images |
|---|---:|
| Train | 6,243 |
| Validation | 1,780 |
| Test | 2,303 |
| **Total** | **10,326** |

Users who wish to reproduce the experiments reported in the associated manuscript are encouraged to retain this official split.

---

## Annotation Protocol

NFVessel is formulated as a **single-class vessel detection dataset**.

### Object Class

```text
0: vessel
```

### Bounding-Box Annotation

Each visible vessel is annotated using a rectangular bounding box.

For partially occluded vessels, the bounding box is drawn to cover the **visible hull region as completely as possible**, rather than estimating the invisible full-object extent.

The dataset does not provide:

- pixel-level vessel segmentation masks;
- paired full-extent and visible-region bounding boxes;
- manually assigned instance-level occlusion-severity labels.

Therefore, NFVessel should be interpreted as a vessel detection dataset based on visible-region bounding-box annotations.

---

## Dataset Statistics

NFVessel currently contains:

```text
10,326 images
19,035 vessel instances
1.84 instances per image
```

Additional task-related statistics used in the associated study include:

| Statistic | Value |
|---|---:|
| Small-target proportion | TBD |
| Multi-vessel image proportion | TBD |
| Overlap-related image proportion | TBD |

These three statistics will be updated after recalculation using the final public annotation release.

### Definition of Small Targets

A vessel instance is defined as a **small target** when its bounding-box area occupies less than:

```text
0.1% of the full image area
```

### Definition of Multi-Vessel Images

An image is defined as a **multi-vessel image** when it contains at least:

```text
2 annotated vessels
```

### Definition of Overlap-Related Images

An image is defined as overlap-related when at least one vessel bounding box has at least 10% coverage by a putatively foreground vessel box according to the geometric rule used in the associated manuscript.

---

## Dataset Structure

The released dataset is organized using the following structure:

```text
NFVessel/
│
├── images/
│   ├── train/
│   ├── val/
│   └── test/
│
├── labels/
│   ├── train/
│   ├── val/
│   └── test/
└── NFVessel.yaml
```

Each image has a corresponding annotation file with the same file stem.

## File Naming Convention

NFVessel image files retain information about the acquisition date, scene background, scene type, and source frame index in their filenames.

A typical filename is:

```text
0329_backB_normal_frame_000240.png
<date>_<background>_<scene_type>_frame_<frame_id>.png
---

## YOLO Annotation Format

The YOLO-format annotations follow:

```text
<class_id> <x_center> <y_center> <width> <height>
```

All bounding-box coordinates are normalized to the range:

```text
[0, 1]
```

Example:

```text
0 0.512430 0.463210 0.083540 0.041270
```

Since NFVessel contains only one category:

```text
class_id = 0
class_name = ship
```

---

## Dataset Configuration

An example Ultralytics/YOLO configuration file is:

```yaml
path: /path/to/NFVessel

train: images/train
val: images/val
test: images/test

names:
  0: ship
```

Users should replace `/path/to/NFVessel` with the local dataset path.

---

## Complex Occlusion Candidate Subset

The associated study additionally defines a **complex occlusion candidate subset** from the NFVessel test set for diagnostic evaluation under close-range vessel interactions and potential occlusion.

For two vessel bounding boxes, the screening procedure considers:

- vertical overlap;
- normalized horizontal proximity.

A frame is included when at least one vessel pair satisfies:

```text
Vertical overlap ratio >= 0.5
Normalized horizontal gap <= 1.0
```

The corresponding official image list will be provided in:

```text
metadata/complex_occlusion_subset.txt
```

This subset is intended only for diagnostic evaluation under complex vessel-interaction conditions and **does not replace the full NFVessel test set**.

---

## NFVessel and NFVessel_Extreme

Please note that **NFVessel** and **NFVessel_Extreme** are different evaluation sets.

### NFVessel

NFVessel is the main dataset released in this repository:

```text
Images:           10,326
Vessel instances: 19,035
Train:             6,243
Validation:        1,780
Test:              2,303
```

### NFVessel_Extreme

NFVessel_Extreme is a separate **737-frame stress-test set** collected under substantially different acquisition conditions for evaluating direct transfer to extreme nearshore scenarios.

The acquisition conditions differ from the original NFVessel dataset in:

- acquisition site;
- acquisition date;
- season;
- imaging device;
- camera viewpoint.
---

## Examples

Representative NFVessel images can be placed in the `examples/` directory.

Suggested examples include:

- normal vessel navigation;
- partially occluded vessels;
- distant small vessels;
- dense multi-vessel scenes;
- coastline interference;
- water-surface reflections;
- vessel-wake interference.

Example display format:

```html
<p align="center">
  <img src="examples/sample_01.jpg" width="45%">
  <img src="examples/sample_02.jpg" width="45%">
</p>
```

---

## Download

The NFVessel dataset is publicly available through **Quark Cloud Drive**.

| Dataset | Download Link | Access Code |
|---|---|---|
| **NFVessel v1.0** | [Quark Cloud Drive](https://pan.quark.cn/s/a221f5b69d0b) | `R6nK` |
| **NFVessel_Extreme** | Independent 737-frame stress-test set | [Quark Cloud Drive](https://pan.quark.cn/s/4bdf7a571dd6) | `cVEj` |

The released data contain the training, validation, and test images together with their corresponding vessel detection annotations.

Official split:

| Split | Images |
|---|---:|
| Train | 6,243 |
| Validation | 1,780 |
| Test | 2,303 |
| **Total** | **10,326** |

Please retain this split when reproducing the experimental results reported in the associated manuscript.

If the download link becomes unavailable, please open an issue in this repository or contact the authors.

---

## Reproducibility

For experiments intended to reproduce the associated study, please use the official NFVessel split:

```text
Train: 6243
Val:   1780
Test:  2303
```

Please do not randomly reshuffle the dataset when directly comparing experimental results with those reported in the associated manuscript.

The official complex-occlusion subset should also use the provided metadata file:

```text
metadata/complex_occlusion_subset.txt
```

---

## Annotation Quality

Annotation consistency was additionally examined in the associated study using independent reannotation and discrepancy review.

A second annotator independently reannotated a randomly sampled subset using the same target-inclusion and visible-region annotation rules.

Detailed consistency metrics and the complete evaluation procedure are reported in the associated manuscript.

Because the public dataset statistics should correspond exactly to the final annotation release, related annotation-quality metadata will be kept synchronized with the released version.

---

## License

NFVessel is released for academic and research use subject to the terms specified in the repository license.

Please refer to:

```text
DATASET_LICENSE
```

for detailed information regarding:

- permitted use;
- redistribution;
- modification;
- commercial use;
- citation requirements.

Users should not assume unrestricted commercial redistribution rights unless explicitly permitted by the dataset license.

---

## Citation

If you use NFVessel in your research, please cite the associated work.

### Associated Manuscript

> **FRA-YOLO: Frequency-guided receptive-field adaptation for occluded ship detection in complex nearshore backgrounds**  
> Yu Yin, Qing Hu, Yue Zhou, Yu Zhang, Shuaiheng Huai

Complete bibliographic information and the official BibTeX entry will be updated after publication.

Before publication, users may refer to the manuscript title together with this repository.

A BibTeX entry will be provided after the manuscript is formally published.

---

## Acknowledgements

NFVessel was collected as part of research on intelligent maritime perception and vessel detection conducted at:

- School of Information Science and Technology, Dalian Maritime University;
- National Engineering Research Center for Ship Navigation Systems.

The associated research was supported in part by the National Key R&D Program of China and the Xingliao Yingcai Program of Liaoning Province.

---

## Contact

For questions regarding NFVessel, please contact:

```text
Yu Yin
Dalian Maritime University
Email: [rainup@dlmu.edu.cn]
```

## Future Updates

Future versions of NFVessel may expand the coverage of:

- acquisition sites;
- seasons;
- illumination conditions;
- weather conditions;
- sea states;
- vessel-density distributions;
- occlusion-related annotations;
- additional benchmark protocols.

All major dataset updates will be documented in the **News** and **Releases** sections of this repository.

---

## Disclaimer

NFVessel is intended for academic research and benchmark evaluation in vessel detection and maritime visual perception.

Users are responsible for complying with the dataset license and applicable laws and regulations when using, modifying, or redistributing the dataset.
