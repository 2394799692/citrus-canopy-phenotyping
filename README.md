# Citrus canopy phenotyping

Reproducibility companion for **Orthogonal crown silhouettes for citrus canopy volume and reconstructed surface area estimation: Predictive gains and transfer reliability**.

This repository provides frozen tree-level predictions, operational reference values, analysis tables, and Python scripts for recalculating the principal statistics in the citrus canopy study. The inputs needed for the statistical workflow are included locally. Model training and raw point-cloud reconstruction are outside this minimal package.

The companion tools below provide access to the previously released data/reconstruction archive and the field-oriented Android application. A related patent application is disclosed with its filing date in [PATENTS.md](PATENTS.md).

## Contents

| Location | Purpose |
|---|---|
| `reproduce_paper_statistics.py` | Recalculate performance, held-form controls, external evaluations, and conditional paired comparisons from frozen predictions |
| `data/` | Inputs used by the executable statistical workflow, including saved predictions, reference traits, split records, and original comparison tables |
| `expected_results/` | Previously executed reference outputs for comparison |
| `SourceData/` | Extended manuscript evidence, current summaries, and explicitly labelled historical analyses |
| `plot_core_results.py` | Generate a compact statistical audit plot from recalculated metrics |
| `docs/REPRODUCIBILITY.md` | Metric definitions, repeated-prediction aggregation, result scope, and version boundaries |
| `docs/COMPANION_TOOLS.md` | Zenodo reconstruction archive and Zhonggan Huiju Android application |
| `PATENTS.md` | Verified patent application disclosure and filing date |
| `CITATION.cff` | Citation information based on the supplied manuscript |
| `SHA256.json`, `verify_release.py` | Integrity manifest and verification script |

## Quick start

The statistical entry point was tested on Windows with Python 3.10.18, NumPy 1.26.4, and pandas 2.3.3. It does not require MATLAB, a GPU, an Android account, model weights, or access to the authors' local drives.

Download and extract this repository, open a terminal in its root, and run:

```powershell
py -3.10 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe verify_release.py
.\.venv\Scripts\python.exe reproduce_paper_statistics.py --data-dir data --out-dir reproduced
```

On a system where the environment is already activated, the last two commands are simply:

```bash
python verify_release.py
python reproduce_paper_statistics.py --data-dir data --out-dir reproduced
```

The main script checks recalculated scores against frozen comparison tables and exits with an assertion error if the checked metrics differ by more than `1e-10`. It writes `reproduced/reproduction_audit.json` along with CSV result tables. Running the script fits **zero regression models**.

Optional audit plot:

```bash
python -m pip install -r requirements_plotting.txt
python plot_core_results.py --source reproduced/reproduced_principal_performance.csv --output reproduced/core_result_overview
```

The optional plot is a statistical overview; it does not regenerate every final manuscript layout. Verification should be run before modifying tracked files. New outputs are written to `reproduced/`, which is excluded by `.gitignore`.

## Dataset and analysis populations

| Cohort | Observations | Registered tree identities | Valid DR volume | Valid AS0.05 area |
|---|---:|---:|---:|---:|
| Historical development archive | 316 | 316 | 315 | 316 |
| Historical evaluation cohort | 35 | 35 | 34 | 35 |
| Original Dagou trees observed over three periods | 168 | 56 | 151 | 168 |
| Additional September cohort | 108 | 108 | 41 | 108 |
| **Total** | **627** | **515** | **541** | **627** |

The principal development analysis uses 315 records with both references. The expanded training branch uses 466 records representing 371 registered tree identities. The final identity-level manual crown-form counts are 337 round-headed, 150 open-centre, and 28 hedge-trained; the principal 315-tree subset contains 235, 61, and 19 trees, respectively.

The 56 Dagou trees contribute repeated observations, not 168 independent trees. Registration identities are retained as originally documented. Known source-point overlap between two development/evaluation pairs is handled in separate sensitivity outputs; it has not been silently merged into a revised registration count. Site assignments remain unknown for 299 historical development records without reliable orchard linkage keys.

DR volume (m³) and AS0.05 reconstructed surface area (m²) are point-cloud algorithm-defined operational reference traits. AS0.05 is not total leaf area.

## What can be reproduced

- Forty rows of principal-model performance: R², RMSE, MAE, Bias, and logRMSE.
- The expanded 315-tree leave-one-crown-form-out evaluation and training-count-matched controls.
- Historical and September external evaluations, including the expanded-training branch.
- Paired conditional comparisons generated with the explicitly recorded analysis seed.
- Historical source-overlap sensitivity summaries, alongside the original frozen evaluation.

Repeated validation predictions are aggregated as losses within observations and then within tree identities. They are not averaged into an ensemble prediction before scoring. The original bootstrap intervals remain in `data/original_frozen_conditional_intervals.csv`; newly recalculated intervals use a separately declared seed and need not reproduce older endpoints exactly.

Details and output filenames are in [docs/REPRODUCIBILITY.md](docs/REPRODUCIBILITY.md). No new model selection, fitting, manual relabelling, or reference reconstruction is performed by these scripts.

## Companion data and reconstruction software

A previously released public Zenodo archive remains available at **[https://doi.org/10.5281/zenodo.16562849](https://doi.org/10.5281/zenodo.16562849)** and provides LiDAR point clouds, RGB canopy images, and a Windows desktop application implementing DR canopy-volume reconstruction and AS outer-canopy surface-area reconstruction. The reconstruction application accepts individual-tree point-cloud files, supports both single-tree and batch processing, and does not require MATLAB.

The Zenodo archive is a separate release. Its data coverage and reconstruction settings should be checked against its own documentation when matching individual files to the final analysis population used here. This repository does not claim that every current observation is contained in that earlier release.

## Field-oriented Android application

The **Zhonggan Huiju (中柑慧橘)** Android application used for the field-oriented inference demonstration is available at **[https://down.scxys.com/aims-v2.0/farming-apps-down.html](https://down.scxys.com/aims-v2.0/farming-apps-down.html)**. A trial account can be requested from the authors to access its main functions.

The application links crown dimensions, canopy images, structural estimates, and individual-tree records. The application and desktop reconstruction tool are companion software; their source code and installers are not embedded in this statistical repository. The Android application is not needed to run the reproducibility workflow. No phone-side accuracy, inference latency, or independent RGB-to-LiDAR equivalence result is asserted by this package.

Further scope information is provided in [docs/COMPANION_TOOLS.md](docs/COMPANION_TOOLS.md).

## Related patent application

The related Chinese invention patent application **202611532440.0**, entitled **一种果树冠层体积和表面积估计方法** (*A method for estimating fruit-tree canopy volume and surface area*, unofficial translation), was filed on **30 September 2026**. The supplied CNIPA acceptance notice was also issued on **30 September 2026** and confirms acceptance for processing. It does not establish a patent grant. Applicants and inventor information are provided in [PATENTS.md](PATENTS.md).

## Data provenance and historical evidence

`SourceData/archive_E48/` retains historical evidence such as earlier 637-observation/118-tree external summaries, the original 76-tree crown-form analysis, and technical sensitivity experiments. Those tables retain their original populations and are not relabelled as final 627-observation results. Current retained-cohort summaries are listed in [docs/source_data_inventory.csv](docs/source_data_inventory.csv).

For the public companion, machine-specific absolute paths in source-data metadata have been replaced with stable `source-ref:` identifiers. All other CSV cell values, row order, and column order are preserved. The executable inputs under `data/`, the statistical scripts, and the expected results remain byte-identical to the supplied package. These `source-ref:` identifiers document external provenance and are not downloadable files or paths needed to execute the analysis.

Historical source hashes and figure/table crosswalks describe the versions in which they were generated. The current public-file checksum authority is `SHA256.json`. Author-edited manuscript figure numbering may differ from older source crosswalks; use the scientific content and [result map](docs/PAPER_RESULT_MAP.md) to locate evidence.

## Citation

The companion manuscript is **Orthogonal crown silhouettes for citrus canopy volume and reconstructed surface area estimation: Predictive gains and transfer reliability**. The author list in `CITATION.cff` follows the supplied manuscript. No publication DOI, accepted-journal status, or GitHub repository URL is invented. Update the citation metadata after publication or repository DOI registration.

Please cite the manuscript and the separate Zenodo archive when using their respective materials.

## Reuse 

An open-source or data-reuse licence has not been selected for this repository preparation. The related patent application is disclosed separately; the earlier Zenodo record and Android application have their own distribution terms. See [LICENSE_STATUS.md](LICENSE_STATUS.md).

