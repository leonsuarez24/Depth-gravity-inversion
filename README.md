# Depth-Aware Implicit Neural Representation Priors for 3D Gravity Inversion

This directory reproduces the paper's Cerro Machín synthetic scenario
(`modelA`): an irregular density model designed to resemble the Cerro Machín
volcanic system. The experiment evaluates the proposed depth-aware
compositional implicit neural representation in the model domain using PSNR,
RMSE, and SSIM, and separately measures the receiver-domain gravity RMSE. The
analysis focuses on recovering the dominant negative, vertically elongated
body and the smaller positive anomaly to the east while preserving their signs,
relative locations, spatial organization, and vertical extent.

Paper: [arXiv:2608.08959](https://arxiv.org/abs/2608.08959)

## Abstract

Gravimetry images subsurface density contrasts associated with geological
structures, geothermal systems, and intrusive bodies. Recovering a
three-dimensional density model from gravity observations is highly ill-posed
because of its non-uniqueness, limited data coverage, and the attenuation of
the gravity field with depth. Classical inversion methods rely on explicit
regularization and parameter tuning, whereas supervised deep-learning
approaches require representative gravity–density pairs that are rarely
available. This paper proposes an unsupervised depth-aware implicit neural
representation for 3D gravity inversion. The density volume is represented by
multiple coordinate-based neural networks assigned to overlapping depth slabs
and optimized directly from the observed gravity measurements through the
sensitivity matrix. Slab-specific Fourier features, physics-based depth gains,
and scheduled regularization provide structural priors without requiring
labeled density models. Experiments on four synthetic scenarios show that the
proposed method provides better overall performance in terms of RMSE, PSNR, and
SSIM than the evaluated conventional and neural baselines. It also recovers more
compact and spatially coherent density bodies, improves the separation of
nearby anomalies, preserves internal structures, and reconstructs their
vertical extent better. These results indicate that the proposed depth-aware
formulation helps to mitigate the depth ambiguity inherent in gravity
inversion. In the field experiment, where no ground-truth density model was
available, the method produced compact, separated, and vertically coherent
anomalies consistent with the observed gravity pattern.

## Citation

```bibtex
@misc{suarezrodriguez2026depthawareimplicitneuralrepresentation,
      title={Depth-Aware Implicit Neural Representation Priors for 3D Gravity Inversion}, 
      author={León Suarez-Rodriguez and Paul Goyes-Peñafiel and Javier Torres-Quintero and Henry Arguello},
      year={2026},
      eprint={2608.08959},
      archivePrefix={arXiv},
      primaryClass={cs.AI},
      url={https://arxiv.org/abs/2608.08959}, 
}
```

## Authors

The authors are affiliated with the Department of Systems Engineering and
Informatics at the Universidad Industrial de Santander (UIS), Bucaramanga,
Colombia, and are associated with the
High-Dimensional Signal Processing (HDSP) Research Group.

### León Suárez-Rodríguez

León Suárez-Rodríguez received a B.S.E. degree in Civil Engineering from the
Universidad Industrial de Santander, Bucaramanga, Colombia, in 2023, and a
Master's degree in Systems Engineering from the same institution in 2025. He is
currently pursuing a Ph.D. in Computer Science at the Universidad Industrial de
Santander. His research interests include inverse problems and deep-learning
applications in geosciences and computational imaging.

- [Google Scholar](https://scholar.google.com/citations?hl=es&user=FAT4XIUAAAAJ)
- [LinkedIn](https://www.linkedin.com/in/leon-suarez24/)
- Email: [leonsuarez24@gmail.com](mailto:leonsuarez24@gmail.com)

### Paul Goyes-Peñafiel

Paul Goyes-Peñafiel received a B.Sc. in Geology from the Universidad Industrial
de Santander, Bucaramanga, Colombia, in 2009, and an M.Sc. in Geophysics from
Perm State University, Perm, Russia, in 2018. He is currently a Ph.D. candidate
in Computer Science at the Universidad Industrial de Santander. He has worked
as a researcher in the hydrocarbon industry, focusing on applied geophysics for
shallow and deep exploration. His research interests include inverse theory and
applications in geophysics, seismic acquisition and processing, potential and
electromagnetic methods, and deep-learning applications in geoscience.

- [Google Scholar](https://scholar.google.com/citations?hl=es&oi=ao&user=xrnAAhwAAAAJ)
- [LinkedIn](https://www.linkedin.com/in/paul-goyes-0212b810/)
- Email: [ypgoype@correo.uis.edu.co](mailto:ypgoype@correo.uis.edu.co)

### Javier Torres-Quintero

Javier Torres-Quintero received a B.Sc. degree in Systems Engineering in 2024.
He is currently pursuing an M.Sc. in Systems Engineering and Informatics and a
Ph.D. in Computer Science at the Universidad Industrial de Santander,
Bucaramanga, Colombia. His research interests include deep learning,
computational imaging, seismic processing, hyperspectral-image analysis, and
signal-processing applications in geoscience.

- [Google Scholar](https://scholar.google.com/citations?hl=es&oi=ao&user=wVkX0BAAAAAJ)
- [LinkedIn](https://www.linkedin.com/in/javier-torres-b2244a222/)
- Email: [torresjaqui32@gmail.com](mailto:torresjaqui32@gmail.com)

### Henry Arguello

Henry Arguello received a Ph.D. from the Department of Electrical and Computer
Engineering at the University of Delaware in 2013. He is currently a titular
professor in the Department of Systems Engineering at the Universidad
Industrial de Santander, Bucaramanga, Colombia, and director of the HDSP
Research Group. He was a Fulbright-funded visiting professor at Stanford
University. He is an Associate Editor of *IEEE Transactions on Computational
Imaging* and has served as Co-Chair and Technical Co-Chair of several
international conferences and workshops. His research interests include
computational-imaging techniques, high-dimensional signal coding and
processing, and optical design. He is a Senior Member of IEEE.

- [Google Scholar](https://scholar.google.com/citations?hl=en&user=R7gjbGIAAAAJ)
- [LinkedIn](https://www.linkedin.com/in/henry-arguello-2905929/)
- Email: [henarfu@uis.edu.co](mailto:henarfu@uis.edu.co)

## Synthetic model and observations

The subsurface is represented by a regular `50 × 50 × 50` grid containing
125,000 cells. Cells above the topography are marked as inactive, leaving 89,413
active subsurface cells. The cell dimensions are:

- 274 m along X;
- 255 m along Y;
- 180 m along Z.

The true model contains positive and negative density contrasts. Density values
are loaded from `grav_modelA.npz`, converted from g/cm³ to kg/m³, and processed
with the experiment's thresholds:

- positive contrasts below 200 kg/m³ are set to zero;
- negative contrasts above −250 kg/m³ are set to zero.

After thresholding, the density range is approximately −290 to 350 kg/m³.

The survey contains 676 gravity receivers. The synthetic observations are
calculated with

$$
\mathbf d = \mathbf K\mathbf m,
$$

where $\mathbf K \in \mathbb{R}^{676 \times 89413}$ is the gravity sensitivity
matrix, $\mathbf m$ contains the active-cell density contrasts in kg/m³, and
$\mathbf d$ contains the gravity response in mGal. The stored kernel is converted
to mGal once when it is loaded and is denoted by $\mathbf K$ everywhere else.
The true model is used to generate the observations and evaluate the
reconstruction, but it is not supplied to the inversion algorithm.

Download the Cerro Machín synthetic experiment data from the
[Google Drive data folder](https://drive.google.com/drive/folders/1o8Kn3wEKhzi9uDe9FQVDmZ_8v2PzjvgU?usp=sharing).
After downloading, place the required input files in
`data_obs_exps/modelA/`:

| File | Contents |
| --- | --- |
| `grav_modelA.npz` | Cell centers, cell dimensions, and true density model. |
| `Kernel_G.npy` | Gravity sensitivity matrix for the active cells. |
| `receivers_modelA.npy` | Gravity receiver coordinates. |

## Depth-aware implicit neural representation

The proposed reconstruction divides the volume into overlapping slabs along the
vertical grid direction. Each slab is generated by an independent, untrained
Fourier-feature multilayer perceptron. The complete density model is

$$
\hat m = \rho_{\max}\tanh\!\left(
    \sum_{i=1}^{P} g_i W_i \odot u_i
\right),
$$

where:

- $P$ is the number of slabs;
- $u_i$ is the density field generated by slab network $i$;
- $W_i$ is a Gaussian depth window;
- $g_i$ is a fixed Li–Oldenburg inverse-sensitivity depth gain;
- $\rho_{\max}=400$ kg/m³ bounds the reconstructed density.

The windows overlap and form a partition of unity. The depth gains increase the
ability of deeper slabs to represent density, counteracting the decay of gravity
sensitivity with depth. Fourier bandwidths are assigned geometrically from deep
to shallow slabs: deeper slabs use lower bandwidths for smooth structure, while
shallower slabs use higher bandwidths for finer detail.

The networks are optimized for this single observation vector. No pretrained
weights or collection of training models is used. The objective combines:

- gravity-data mean squared error;
- anisotropic total variation for piecewise-smooth bodies;
- an $\ell_1$ penalty for a sparse background;
- neighboring-slab consensus for a coherent reconstruction.

TV and $\ell_1$ weights decay during optimization, while the consensus weight
increases from zero to its final value.

## Reproduction configuration

The full Cerro Machín experiment uses:

| Parameter | Value |
| --- | ---: |
| Grid | `50 × 50 × 50` |
| Active cells | 89,413 |
| Observations | 676 |
| Density bound | ±400 kg/m³ |
| Depth slabs | 14 |
| Fourier bandwidths | Geometric spacing from 1 to 48 |
| Fourier features per slab | 64 |
| Hidden width | 128 |
| Hidden layers | 4 |
| Optimizer | AdamW |
| Learning rate | `1e-3` |
| Optimization steps | 3500 |
| TV weight | 0.1 |
| $\ell_1$ weight | 0.1 |
| Final consensus weight | 24 |
| Depth offset $z_0$ | 3 cells |
| Depth exponent $\beta$ | 3 |
| Random seed | 0 |

A CUDA GPU is strongly recommended for the full configuration.

## Notebook

Run the complete, self-explained experiment with
[`cerro_machin_synthetic_experiment.ipynb`](cerro_machin_synthetic_experiment.ipynb).
The notebook performs the following operations in order:

1. loads and validates the model, kernel, and receivers;
2. generates the synthetic gravity observations;
3. visualizes the true model and survey response;
4. defines every component of the proposed depth-aware INR directly in the notebook;
5. constructs and optimizes the proposed depth-aware INR;
6. calculates reconstruction and depth metrics;
7. plots orthogonal sections, depth profiles, and predicted gravity.

The notebook is self-contained apart from its three input data files. It does
not import implementation code from the repository and runs only the proposed
depth-aware INR.

The notebook defaults to the full reproduction configuration. For a short
execution test, launch Jupyter with:

```bash
CERRO_MACHIN_MODE=quick jupyter lab
```

Quick mode uses three slabs, a smaller network, and three optimization steps. It
only verifies that the workflow executes; its reconstruction and metrics are not
scientific results.

Notebook-generated models and figures are written to
`synthetic/notebook_results/`. Input data are never modified.

## Evaluation

Reconstructions are evaluated only on active subsurface cells using:

- model RMSE in kg/m³;
- PSNR with an 800 kg/m³ data range;
- three-dimensional SSIM;
- receiver-domain RMSE in mGal.

A small gravity-data misfit is not sufficient evidence of a correct model,
because many subsurface density distributions can produce nearly identical
surface gravity. The model, structural, and depth metrics must therefore be
considered together. In the paper's Cerro Machín result, the proposed method
recovers the main negative body's location and elongated geometry while
retaining the smaller positive anomaly, although the reconstructed boundaries
remain smoother than the ground truth. The reported metrics are 27.15 dB PSNR,
35.12 kg/m³ model RMSE, 0.873 SSIM, and 0.071 mGal receiver RMSE.
