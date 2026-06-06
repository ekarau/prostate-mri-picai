# Prostate MRI — PI-CAI Baseline Evaluation

A small study around the [PI-CAI Challenge](https://pi-cai.grand-challenge.org/):
take the published baseline models for prostate MRI, run them on a 20-patient
subset, and measure how well they do against the reference annotations. The
results feed a short presentation (slides under `presentation/`).

The project covers two separate tasks:

- **Anatomical zonal segmentation** — the pretrained MONAI `prostate_mri_anatomy`
  bundle (Adams & Bressem, 2022) splitting the gland into the transitional zone
  (TZ) and peripheral zone (PZ).
- **csPCa lesion detection** — the PI-CAI U-Net baseline (Bosma et al., 2022)
  flagging clinically significant prostate cancer on multi-parametric MRI
  (T2W + ADC + high b-value DWI).

Everything runs on Google Colab against data kept in Google Drive. I did not
re-train anything — the point was to evaluate the released models, not to beat
them.

## Results (20-patient PI-CAI subset)

| Task | Metric | This subset | Reference |
|------|--------|-------------|-----------|
| Anatomy — TZ | mean Dice | see `step3` output | 0.88 (prostate158) |
| Anatomy — PZ | mean Dice | see `step3` output | 0.75 (prostate158) |
| Lesion | PI-CAI score (AP+AUROC)/2 | 0.90 | 0.61 (full test set) |
| Lesion | case-level Sens / Spec | 1.00 / 1.00 | — |

The lesion numbers sit above the published reference, but that is expected: the
cohort is tiny (10 positive + 10 negative) and hand-balanced, and the baseline
output is binary, so the patient-level AUROC is measured at a single operating
point. The presentation discusses this caveat.

## Repository layout

```
.
├── notebooks/                 # the actual pipeline, run in order on Colab
│   ├── step1_setup.ipynb          environment + Drive bootstrap
│   ├── step2_download_data.ipynb   PI-CAI fold 0 download, 20-case subset
│   ├── step3_anatomy_inference.ipynb   zonal segmentation + Dice
│   ├── step4_lesion_inference.ipynb    csPCa detection + PI-CAI metrics
│   └── step5_visualizations.ipynb      failure cases + summary figures
├── docs/
│   └── COLAB_SETUP.md         Colab + Drive setup walkthrough
├── presentation/
│   └── *.pptx                 the slide deck
├── requirements.txt
└── external/                  third-party repos/data (not tracked — see below)
```

## Getting the external data

`external/` is gitignored — it holds large third-party repositories and the
imaging data, which are not re-hosted here. Download them into `external/`:

| What | Where from |
|------|------------|
| PI-CAI baseline code | https://github.com/DIAGNijmegen/picai_baseline |
| PI-CAI labels + clinical info | https://github.com/DIAGNijmegen/picai_labels |
| PI-CAI imaging (fold 0) | https://zenodo.org/records/6624726 |
| `prostate_mri_anatomy` MONAI bundle | https://github.com/Project-MONAI/model-zoo |
| prostate158 (anatomy model source) | https://doi.org/10.5281/zenodo.6481141 |

Then follow [`docs/COLAB_SETUP.md`](docs/COLAB_SETUP.md) to put everything in
the right place on Drive and run the notebooks `step1` → `step5`.

## Environment

Colab T4 GPU + Google Drive. Key libraries pinned in `requirements.txt`
(notably `numpy<2` and `monai==1.3.2`, which have to match — newer NumPy breaks
the MONAI transforms).

## Credits

This project is built entirely on third-party models and data — full credit
goes to the original authors. I only wrote the evaluation notebooks and the
slides. For the anatomy model:

> Adams, L. C., & Bressem, K. K. (2022). Prostate158 — An expert-annotated 3T
> MRI dataset and algorithm for prostate cancer detection. *Computers in Biology
> and Medicine*, 148, 105817.

For the PI-CAI baseline and labels, cite the original organizers (see the
upstream repositories listed above).
