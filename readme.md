# nnCPUnet

**nnCPUnet** is a controlled fork of [nnU-Net v2](https://github.com/MIC-DKFZ/nnUNet) for CPU-oriented and federated workflows (including the FedV1 application).

It keeps the nnU-Net pipeline — dataset fingerprinting, experiment planning, preprocessing, training, and inference — while giving you a separate package name, CLI, and release line under your own version control.

| | Official nnU-Net v2 | **nnCPUnet** |
|--|---------------------|--------------|
| GitHub | [MIC-DKFZ/nnUNet](https://github.com/MIC-DKFZ/nnUNet) | [Williamwhy/nnCPUNet](https://github.com/Williamwhy/nnCPUNet) |
| PyPI / pip | `pip install nnunetv2` | `pip install nnCPUnet` (or install from Git; see below) |
| Import | `import nnunetv2` | `import nncpunet` |
| CLI prefix | `nnUNetv2_*` | `nnCPUnet_*` |

Upstream project: **[nnU-Net](https://github.com/MIC-DKFZ/nnUNet)** (MIC-DKFZ / Helmholtz Imaging / DKFZ).  
This repository is **not** a drop-in replacement of the official package name; install **either** `nnunetv2` **or** `nnCPUnet` in a given environment to avoid conflicts.

---

## Relationship to nnU-Net

nnU-Net is a semantic segmentation framework that automatically adapts its pipeline to a dataset. It analyzes the training data, creates a dataset fingerprint, configures suitable U-Net variants, and provides an end-to-end workflow from preprocessing to training, model selection, and inference.

nnCPUnet starts from that codebase and is intended for:

- Full **version control** of the training/inference stack used with FedV1  
- **CPU-friendly** and clinic-oriented deployment paths  
- Custom trainers and packaging (`nnCPUnet_*` entry points) without waiting on upstream releases  

Behaviour and data layout follow upstream nnU-Net v2 unless documented otherwise in this fork.

**Environment variables (same as upstream):**

```text
nnUNet_raw
nnUNet_preprocessed
nnUNet_results
```

Keep these names so existing datasets and plans remain compatible with nnU-Net tooling and docs.

---

## Quick install

Install PyTorch for your platform first, then:

**From GitHub (recommended until PyPI publish is confirmed):**

```bash
pip install "nnCPUnet @ git+https://github.com/Williamwhy/nnCPUNet.git@v0.1.3"
```

**Editable (development):**

```bash
git clone https://github.com/Williamwhy/nnCPUNet.git
cd nnCPUNet
pip install -e .
```

**From PyPI** (once the project is published):

```bash
pip install nnCPUnet
```

Verify:

```bash
python -c "import nncpunet; print(nncpunet.__file__)"
nnCPUnet_train -h
```

Uninstall any conflicting official package if both are present:

```bash
pip uninstall nnunetv2 -y
```

For folder setup (`nnUNet_raw`, `nnUNet_preprocessed`, `nnUNet_results`), follow the upstream guide and substitute CLI names as below:  
[Installation and setup (nnU-Net)](https://github.com/MIC-DKFZ/nnUNet/blob/master/documentation/getting-started/installation-and-setup.md).

---

## CLI cheatsheet

| Task | Upstream | nnCPUnet |
|------|----------|----------|
| Plan + preprocess | `nnUNetv2_plan_and_preprocess` | `nnCPUnet_plan_and_preprocess` |
| Train | `nnUNetv2_train` | `nnCPUnet_train` |
| Predict | `nnUNetv2_predict` | `nnCPUnet_predict` |
| Find best config | `nnUNetv2_find_best_configuration` | `nnCPUnet_find_best_configuration` |

Example:

```bash
nnCPUnet_plan_and_preprocess -d DATASET_ID --verify_dataset_integrity
nnCPUnet_train DATASET_ID 3d_fullres 0
nnCPUnet_predict -i INPUT_FOLDER -o OUTPUT_FOLDER -d DATASET_ID -c 3d_fullres
```

Optional custom trainer (if provided in this repo):

```bash
nnCPUnet_train DATASET_ID 3d_fullres 0 -tr nnUNetTrainer_200epochs
```

---

## Documentation

Conceptual and how-to documentation is largely the same as upstream nnU-Net. Prefer upstream docs for methods and formats; use this README for **package name, install, and CLI** differences.

Useful upstream entry points:

- [Getting started](https://github.com/MIC-DKFZ/nnUNet/blob/master/documentation/getting-started/README.md)
- [Prepare a dataset](https://github.com/MIC-DKFZ/nnUNet/blob/master/documentation/how-to/prepare-a-dataset.md)
- [Train models](https://github.com/MIC-DKFZ/nnUNet/blob/master/documentation/how-to/train-models.md)
- [Run inference](https://github.com/MIC-DKFZ/nnUNet/blob/master/documentation/how-to/run-inference.md)
- [Residual encoder presets](https://github.com/MIC-DKFZ/nnUNet/blob/master/documentation/resenc_presets.md)

Local copies of documentation may still live under `documentation/` in this repository (inherited from upstream).

---

## FedV1

The FedV1 application is built to drive dataset creation, preprocessing, training export (e.g. Kaggle/Colab), quantization, and CPU inference. Point FedV1 at this package via:

```text
nnCPUnet @ git+https://github.com/Williamwhy/nnCPUNet.git@v0.1.3
```

or `pip install nnCPUnet` when available on PyPI. Application code should `import nncpunet` and call `nnCPUnet_*` CLIs.

---

## Citation

If you use nnCPUnet, please cite **nnU-Net** (the method and upstream software):

```text
Isensee, F., Jaeger, P. F., Kohl, S. A., Petersen, J., & Maier-Hein, K. H. (2021).
nnU-Net: a self-configuring method for deep learning-based biomedical image segmentation.
Nature Methods, 18(2), 203-211.
```

Additional upstream work:

- [nnU-Net Revisited: A Call for Rigorous Validation in 3D Medical Image Segmentation](https://arxiv.org/pdf/2404.09556.pdf)

When referring to this fork specifically, cite or link:

- https://github.com/Williamwhy/nnCPUNet

---

## Licence and acknowledgements

nnCPUnet is derived from nnU-Net and remains under the upstream licence terms (see `LICENSE` in this repository).

nnU-Net is developed and maintained by the Applied Computer Vision Lab (ACVL) of [Helmholtz Imaging](http://helmholtz-imaging.de) and the [Division of Medical Image Computing](https://www.dkfz.de/en/mic/index.php) at the [German Cancer Research Center (DKFZ)](https://www.dkfz.de/en/index.html).

This fork is maintained independently for FedV1 / nnCPUnet packaging and deployment needs.
