
# Illumination Beams for a Light-Sheet Microscope: Fourier-Optics Simulation

Numerical study of the illumination beams used in light-sheet fluorescence microscopy (LSFM). A Python/NumPy simulator computes the electric field produced by a microscope objective from the field in its pupil, and is used to compare Gaussian, Bessel and Airy beams.

Team project carried out at Télécom Physique Strasbourg (April–May 2026). The starting formula (Fraunhofer propagation through the objective) was given in the assignment; the choice of beams, the simulations and the analysis were left open.

## Method

The field in the focal region is obtained from the Fourier transform of the pupil field:

```
E(x, y, z) = -(i f / (λ0 kt²)) · F[ Et(θ, φ) / cos(θ) ] · exp(i k z)
```

- The pupil field is a 250 × 250 grid (disk of radius 4 mm, 32 µm per pixel); the Fourier transform is computed with `numpy.fft`.
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

## Usage

```bash
pip install numpy matplotlib
python src/[script_name].py
```

Requirements: Python 3, NumPy, Matplotlib. [Add any other library actually used.]

## Author

Nolan Le Tyrant, Télécom Physique Strasbourg. I wrote all the simulation code and the results and analysis chapters of the project report, which is available on request.

## License

[MIT, see `LICENSE`]
