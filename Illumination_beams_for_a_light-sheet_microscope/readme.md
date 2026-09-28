# Illumination Beams for a Light-Sheet Microscope: Fourier-Optics Simulation

Numerical study of the illumination beams used in light-sheet fluorescence microscopy (LSFM). A Python/NumPy simulator computes the electric field produced by a microscope objective from the field in its pupil, and is used to compare Gaussian, Bessel and Airy beams.

Team project carried out at Télécom Physique Strasbourg (April–May 2026). The starting formula (Fraunhofer propagation through the objective) was given in the assignment; the choice of beams, the simulations and the analysis were left open.

## Method

The field in the focal region is obtained from the Fourier transform of the pupil field:

```
E(x, y, z) = -(i f / (λ0 kt²)) · F[ Et(θ, φ) / cos(θ) ] · exp(i k z)
```

- The pupil field is sampled on a grid of about 250 × 250 points (disk of radius 4 mm, about 32 µm per pixel); the Fourier transform is computed with `numpy.fft`.
- The field is evaluated on a list of z values (500 points over [-100, 100] µm) and visualised through the XY, XZ and YZ cross-sections.
- For the obstacle study, the beam is propagated with an angular-spectrum propagator: `E(x, y, z) = F⁻¹{ F{E(x, y, 0)} · H(fx, fy, z) }`.

| Parameter | Value |
|---|---|
| Wavelength λ | 600 nm |
| Objective focal length f | 5 mm |
| Pupil radius | 4 mm |
| Numerical aperture NA | 0.8 |
| Refractive index | 1 |

## What is simulated

- **Gaussian beam** (reference): waist w0 ≈ 0.24 µm and Rayleigh length zR ≈ 0.30 µm at NA = 0.8. The FWHM follows the expected 1/NA law for NA > 0.3; deviations at NA < 0.1 come from grid resolution and FFT truncation/aliasing.
- **Gaussian beam with quadratic phase**: the focal plane is shifted along z while the waist and Rayleigh length are unchanged. The shift versus the phase coefficient α is fitted by a power law `y(α) = a·α^b + c` (gradient descent), giving an exponent b ≈ 1.02, i.e. an almost linear dependence.
- **Bessel beam** (thin ring in the pupil): the FWHM stays at about 10 µm over roughly 3 mm, about four orders of magnitude longer than the Gaussian Rayleigh length, at the cost of energy carried by the secondary rings (lower contrast).
- **Airy beam** (cubic phase in the pupil): parabolic trajectory (about 8 µm of deviation over 200 µm of propagation) and square-shaped main lobe.
- **Self-healing of the Airy beam**: an obstacle is placed on the main lobe and the beam is propagated with the angular-spectrum method; the intensity distribution is recovered after a certain distance, while the energy blocked by the obstacle stays lost.

## Repository structure

The code is provided as Jupyter notebooks. Each notebook redefines the few shared functions (field computation, plotting, beam-edge detection), so they can be run independently.

```
├── README.md
└── notebooks/
    ├── PMI.ipynb                  # simulator + compact runs of all beams (Gaussian, quadratic phase, Bessel, Airy)
    ├── Boucle_R.ipynb             # sweep of the pupil radius (NA): FWHM versus NA, compared with theory
    ├── phase_quadratique.ipynb    # sweep of the quadratic-phase coefficient, focal shift, power-law fit
    │                              # (gradient descent written from scratch)
    ├── Bessel.ipynb               # ring parameters scan, length of the constant-FWHM zone
    └── Airy.ipynb                 # Airy beam, trajectory, self-healing with an obstacle, (angular-spectrum propagator), Poynting vector
└── Figures                               
```

## Usage

```bash
pip install numpy matplotlib jupyter
jupyter notebook
```

Open a notebook from the `notebooks/` folder and run it from top to bottom. Requirements: Python 3, NumPy, Matplotlib, Jupyter. `Boucle_R.ipynb` is the slowest one (a 3D field is computed for each of the 30 pupil radii).

## Notes

- Comments and plot labels in the notebooks are in French.
- Some notebooks contain parameter scans used to find values that make the beam features most visible, so a few cells use values that differ from the final ones quoted in the report.

## Author

Nolan Le Tyrant, Télécom Physique Strasbourg. I wrote all the simulation code and the results and analysis chapters of the project report, which is available on request.

## License

[MIT, see `LICENSE`]
