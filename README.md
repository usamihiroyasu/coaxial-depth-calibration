# Sharp Coaxial Illumination Thresholds for Monotone Near-Field Depth Calibration

Hiroyasu Usami · Ayato Tsuboi · Keisuke Hara

Design coaxial illumination for a unique intensity-to-depth response.

The paper gives sharp illumination thresholds, a tilt-independent guarantee for weighted balanced arrays, and a finite-band inverse-stability rule. It covers isotropic sources and axially aligned angular emission.

For isotropic illumination and a 45° normal cone:

| Layout class | Sharp coaxial-to-off-axis coefficient ratio |
|---|---:|
| Unrestricted layouts | 0.1768 |
| Weighted balanced common-radius layouts | 0.01789 |

The two class-wide guarantees differ by a factor of **9.88**.

The model uses camera-axis calibration points, camera-plane sources, known fixed normals and gain, calibrated source strengths, and local Lambertian clamping. The numerical controls evaluate this ideal model.

![Coaxial thresholds and depth-inversion stability](coaxial_thresholds.gif)

## Reproduce

Use POSIX Python 3 and its standard library. From a fresh repository checkout, unpack the source archive and run:

```sh
python3 -m zipfile -e code-and-results.zip .
mkdir reproduced
mkdir reproduced/isotropic reproduced/angular
cp code/radiometry.py code/run_validation.py code/balanced_controls.py reproduced/isotropic/
(cd reproduced/isotropic && python3 run_validation.py && python3 balanced_controls.py)
cp code/angular_controls/angular_controls.py code/angular_controls/EFFECTIVE_SPEC.json reproduced/angular/
(cd reproduced/angular && python3 angular_controls.py)
python3 code/design_case/design_case.py --output-dir reproduced/design_case
```

The commands use fresh output directories. The design-case CLI reads its adjacent `SPEC.json`.

Reference results are grouped in:

- `code/RESULTS.json`: isotropic irradiance, derivative, sharpness, and inversion controls.
- `code/BALANCED_RESULTS.json`: 40 balanced-array controls.
- `code/angular_controls/RESULTS.json`: exact rational root counts, 80-digit numerical coefficient evaluation, and 36 controls at exponent 1.
- `code/design_case/ROWS.json` and `code/design_case/SUMMARY.json`: 20 threshold-versus-margin observations.

## Figures and animation

Vector figures and CSV inputs are in `code/figure_sources/`. With LaTeX, `standalone`, TikZ, and PGFplots installed, run `pdflatex -halt-on-error figure1.tex` there; repeat for `figure2.tex` and `figure3.tex`.

To rebuild the GIF and silent MP4 on macOS, install Swift, LaTeX, FFmpeg, and Python 3, then run `(cd code/animation && zsh reproduce.sh)`. The script prints the output directory.

For citation, use [CITATION.bib](CITATION.bib).

Code license: to be specified.
