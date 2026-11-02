# Illumination Balance and Sharp Thresholds for Near-Field Depth Calibration

Hiroyasu Usami · Ayato Tsuboi · Keisuke Hara

This repository contains the numerical calculations, figure sources, and animation for the paper. The results relate illumination balance and coaxial source strength to uniqueness and sensitivity of scalar depth calibration.

For isotropic illumination and a 45° normal cone:

| Layout class | Sharp coaxial-to-off-axis coefficient ratio |
|---|---:|
| Unrestricted layouts | 0.1768 |
| Common radius, weighted-centroid residual at most 0.1 | 0.02406 |
| Weighted balanced common-radius layouts | 0.01789 |

The unrestricted and balanced class-wide thresholds differ by a factor of **9.88** in calibrated source coefficients. A normalized weighted-centroid residual of 0.1 gives the intermediate threshold 0.02406. This residual is the norm of the strength-weighted source centroid divided by the common radius.

The model uses a camera-axis point, camera-plane sources, a known fixed normal and gain, and local Lambertian clamping. For example, a reference patch could be translated along the camera axis with its orientation fixed, allowing distance to be recovered from one intensity measurement. The numerical calculations use this radiometric model.

![Coaxial thresholds and depth-inversion stability](coaxial_thresholds.gif)

## Reproduce

Use Python 3 and its standard library. From a fresh repository checkout, unpack the source archive and run:

```sh
python3 -m zipfile -e code-and-results.zip .
mkdir reproduced
mkdir reproduced/isotropic reproduced/angular
cp code/radiometry.py code/run_validation.py code/balanced_controls.py reproduced/isotropic/
(cd reproduced/isotropic && python3 run_validation.py && python3 balanced_controls.py)
cp code/angular_controls/angular_controls.py code/angular_controls/EFFECTIVE_SPEC.json reproduced/angular/
(cd reproduced/angular && python3 angular_controls.py)
python3 code/design_case/design_case.py --output-dir reproduced/design_case
python3 code/residual_balance/residual_controls.py --spec code/residual_balance/SPEC.json --threshold code/figure_sources/threshold.csv --output reproduced/residual_balance
```

The commands use fresh output directories. The design-case CLI reads its adjacent `SPEC.json`.

Reference results are grouped in:

- `code/RESULTS.json`: isotropic irradiance, derivative, sharpness, and inversion controls.
- `code/BALANCED_RESULTS.json`: 40 balanced-array controls.
- `code/angular_controls/RESULTS.json`: exact rational root counts, 80-digit numerical coefficient evaluation, and 36 controls at exponent 1.
- `code/design_case/ROWS.json` and `code/design_case/SUMMARY.json`: 20 threshold-versus-margin observations.

- `code/residual_balance/ROWS.json`, `SUMMARY.json`, and `tolerance.csv`: 21 imperfect-balance examples and the common-radius residual curve. The geometry is fixed in the script and described in `SPEC.json`.

## Figures and animation

Vector figures and CSV inputs are in `code/figure_sources/`. With LaTeX, `standalone`, TikZ, and PGFplots installed, run `pdflatex -halt-on-error figure1.tex` there; repeat for `figure2.tex` and `figure3.tex`.

To rebuild the GIF and silent MP4 on macOS, install Swift, LaTeX, FFmpeg, and Python 3, then run `(cd code/animation && zsh reproduce.sh)`. The script prints the output directory.

For citation, use [CITATION.bib](CITATION.bib).

Code license: to be specified.
