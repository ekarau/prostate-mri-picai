# Prostate MRI csPCa Detection — PI-CAI Re-implementation

Re-implementation and evaluation of the official PI-CAI Challenge baseline
algorithms for clinically significant prostate cancer (csPCa) detection on
multi-parametric MRI, together with anatomical zonal segmentation using the
MONAI Model Zoo bundle by Adams & Bressem (2022).

This repository tracks an interview project: download the PI-CAI dataset,
run the baseline detection/segmentation models, and showcase the data,
ground-truth annotations, and model performance.

---

## Goals

The deliverables targeted by this project:

1. Download and inspect the PI-CAI Challenge dataset (Public Training and
   Development).
2. Run / re-use the baseline detection models (U-Net, nnU-Net) released by
   the PI-CAI organizers.
3. Visualize the imaging data and ground-truth segmentations.
4. Report segmentation performance for prostate anatomical zones
   (transitional zone, peripheral zone).
5. Report lesion segmentation / detection performance for csPCa.

---

## External References

This project consumes — but does not re-host — the following third-party
artifacts. Reproduce locally by cloning / downloading them into the
project root:

| Component | Source |
|-----------|--------|
| PI-CAI baseline models | <https://github.com/DIAGNijmegen/picai_baseline> |
| PI-CAI labels & clinical info | <https://github.com/DIAGNijmegen/picai_labels> |
| PI-CAI imaging data (Public Training) | <https://zenodo.org/records/6624726> |
| Prostate anatomy MONAI bundle | <https://github.com/Project-MONAI/model-zoo> (`prostate_mri_anatomy`) |
| prostate158 dataset (anatomy model training) | <https://doi.org/10.5281/zenodo.6481141> |

---

## Project Structure

```
Prostate_MRI_Project/
├── README.md                  # this file
├── COLAB_SETUP.md             # Colab + Drive setup guide
├── step1_setup.ipynb          # environment bootstrap (Colab)
├── step2_download_data.ipynb  # (planned) Zenodo download
├── step3_anatomy_inference.ipynb   # (planned) zonal segmentation
├── step4_lesion_inference.ipynb    # (planned) csPCa detection
├── step5_visualizations.ipynb      # (planned) figures for slides
│
├── picai_baseline-main/   # (gitignored) clone of DIAGNijmegen/picai_baseline
├── picai_labels-main/     # (gitignored) clone of DIAGNijmegen/picai_labels
├── prostate_mri_anatomy/  # (gitignored) MONAI bundle (pretrained model.pt)
├── input/                 # (gitignored) PI-CAI imaging data
├── workdir/               # (gitignored) preprocessing outputs
└── output/                # (gitignored) predictions, metrics, figures
```

---

## Environment

- **Compute:** Google Colab (T4 GPU, 16 GB VRAM) backed by Google Drive
- **Local:** Windows 11, PowerShell, Python 3.11, PyTorch 2.x, MONAI 1.3.x
- **Key libraries:** MONAI, SimpleITK, nibabel, `picai_baseline`,
  `picai_eval`, `picai_prep`

Detailed setup steps are in [`COLAB_SETUP.md`](COLAB_SETUP.md).

---

## Roadmap

| Phase | Notebook | Status |
|-------|----------|--------|
| 1 — Environment setup | `step1_setup.ipynb` | in progress |
| 2 — Data download | `step2_download_data.ipynb` | planned |
| 3 — Anatomical segmentation | `step3_anatomy_inference.ipynb` | planned |
| 4 — Lesion detection | `step4_lesion_inference.ipynb` | planned |
| 5 — Visualizations / slides | `step5_visualizations.ipynb` | planned |

---

## License

The notebooks and documentation in this repository are released under the
MIT License (see `LICENSE`). The third-party repositories and datasets
referenced above retain their own licenses; consult the upstream sources.

---

## Citation

If you build on the PI-CAI baseline or labels, please cite the original
organizers (see the upstream repositories for the canonical citation).

For the anatomy model:

> Adams, L. C., & Bressem, K. K. (2022). *Prostate158 — An expert-annotated
> 3T MRI dataset and algorithm for prostate cancer detection.* Computers in
> Biology and Medicine, 148, 105817. <https://doi.org/10.1016/j.compbiomed.2022.105817>
