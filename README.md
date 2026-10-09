# Crash Simulation Post-Processing

Jupyter notebook for evaluating LS-DYNA frontal-impact simulations of four human body models
(AF5, AF50, AM50, AM95). It reads `binout` and `d3plot` data, computes lower-extremity and pelvis
quantities, applies a CFC filter where required, and saves comparison plots (all models in one
diagram, left vs. right side). Methodology and results are described in the accompanying thesis.

## What it does

- Contact forces: knee ↔ dashboard, heel ↔ carpet (peak, peak time, impulse)
- Femur and tibia cross-section forces and moments
- Calcaneus acceleration and displacement, pelvis translation, femur–tibia relative displacement
- Joint angles (knee, ankle, hip)
- Femur and tibia stresses (principal, shear, von Mises, 99th percentile) and principal stress direction
- CFC filtering via `lowpass_filter` (default CFC 60), with automatic interpolation if the
  sampling rate of the data is too low or the time steps are not uniform
- Identical Y-axis for left/right plots via `set_common_ylim`

## Requirements

```bash
pip install numpy pandas matplotlib scipy lasso-python jupyterlab
```

## Usage

1. Set the simulation folders (`DIR`) and `OUTPUT_DIR` in the setup cell.
2. Each simulation folder needs `binout*`, `d3plot/d3plot` and a `config.txt`
   (Python dict with the part, node, section and contact IDs of the model).
3. Run all cells from top to bottom.

Plots (PNG) and a log file (`simulation_output.txt`) are written to `OUTPUT_DIR`.

## Notes

- Units: mm, s, N, MPa (`G_FACTOR = 9810` mm/s²).
- Quantities taken from `d3plot` (acceleration, displacement, angles, stresses) have a coarse time
  resolution; CFC filtering of these data is an approximation and not a standard-conforming CFC evaluation.
