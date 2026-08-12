# COSMOS-Web GSMF–MS data products

Data products associated with the paper:

**Koprowski, M. P., Sawant, P., & Lisiecki, K., _Stellar mass growth in COSMOS-Web: a mass-complete main sequence to z ~ 8 and its consistency with GSMF evolution_.**

This repository contains the machine-readable measurements and posterior samples used in the analysis of the star-forming main sequence (MS), quiescent-galaxy fraction, and their connection to the evolution of the galaxy stellar mass function (GSMF).

The analysis is based on the COSMOS-Web v1.1 galaxy catalog combined with stacked Herschel and SCUBA-2 far-infrared data. Full methodological details are given in the associated paper.

## Files

### `FIR_fluxes_corrected.ecsv`

Final de-blended stacked far-infrared flux densities for the mass-complete star-forming sample.

Each row corresponds to one redshift and stellar-mass bin.

Columns:

- `z_min`, `z_max` — redshift-bin limits
- `z_mean` — mean redshift of galaxies contributing to the stack
- `logM_min`, `logM_max` — stellar-mass-bin limits
- `logM_mean` — mean logarithmic stellar mass, log10(M*/M_sun)
- `F100`, `F160`, `F250`, `F350`, `F500`, `F850` — final de-blended stacked flux densities in mJy
- `e_F100`, `e_F160`, `e_F250`, `e_F350`, `e_F500`, `e_F850` — corresponding 1σ uncertainties in mJy

Negative flux densities are valid measurements and should not be interpreted as missing data.

### `SFR_components.ecsv`

Infrared, ultraviolet, and total star-formation-rate measurements used to determine the star-forming MS.

Each row corresponds to one mass-complete redshift and stellar-mass bin.

Columns:

- `z_min`, `z_max`, `z_mean` — redshift-bin limits and mean redshift
- `logM_min`, `logM_max`, `logM_mean` — stellar-mass-bin limits and mean logarithmic stellar mass
- `log_L_IR`, `e_log_L_IR` — log10(L_IR/L_sun) and its 1σ uncertainty
- `log_L_NUV`, `e_log_L_NUV` — log10(L_NUV/L_sun) and its 1σ uncertainty
- `log_SFR`, `e_log_SFR` — log10[SFR/(M_sun yr^-1)] and its 1σ uncertainty

`nan` values of `log_L_IR` indicate bins for which no valid infrared luminosity could be derived; the corresponding SFR is therefore based on the unobscured UV component alone. Bins below the adopted stellar-mass-completeness limits are omitted.

### `MS_posterior_chains.ecsv`

Flattened posterior samples from the MCMC fit to the redshift-dependent star-forming MS.

The adopted relation is

```math
\psi(x,z)
=
s_0(z)
-
\log_{10}
\left[
1+
10^{-\gamma\left[x-x_0(z)\right]}
\right],
```

where

```math
x \equiv \log_{10}(M_\ast/M_\odot),
\qquad
\psi \equiv \log_{10}\!\left[\mathrm{SFR}/(M_\odot\,\mathrm{yr}^{-1})\right],
```

and

```math
s_0(z)=a_{s_0}\log_{10}(z)+b_{s_0},
\qquad
x_0(z)=a_{M_0}\log_{10}(z)+b_{M_0}.
```

Columns:

- `sample` — posterior-sample index
- `a_s0`, `b_s0` — parameters describing the redshift evolution of the MS normalization
- `a_M0`, `b_M0` — parameters describing the redshift evolution of the turnover mass
- `gamma` — low-mass logarithmic slope

Each row represents one retained posterior sample.

### `f_Q_binned_measurements.ecsv`

Binned COSMOS-Web measurements of the quiescent-galaxy fraction.

Columns:

- `z_min`, `z_max`, `z_center` — redshift-bin limits and center
- `logM_min`, `logM_max`, `logM_center` — stellar-mass-bin limits and center
- `f_Q` — measured quiescent fraction
- `f_Q_err` — 1σ uncertainty
- `N_all` — total number of galaxies represented in the bin
- `N_QG` — number of quiescent galaxies represented in the bin
- `used_in_global_fit` — boolean flag indicating whether the measurement was included in the global quiescent-fraction fit

The global fit used measurements with log10(M*/M_sun) >= 9.0.

### `f_Q_posterior_samples.ecsv`

Flattened post-burn-in MCMC posterior samples for the global quiescent-fraction model,

```math
f_{\rm Q}(x,z)
=
A(z)
\exp
\left[
-\frac{1}{2}
\left(
\frac{x-\mu(z)}{\sigma(z)}
\right)^2
\right],
```

with

```math
p(z)=a_p\exp(b_p z)+c_p,
\qquad
p\in\{A,\mu,\sigma\}.
```

Columns:

- `sample_id` — posterior-sample index
- `a_A`, `b_A`, `c_A` — redshift-evolution parameters for A(z)
- `a_mu`, `b_mu`, `c_mu` — redshift-evolution parameters for μ(z)
- `a_sigma`, `b_sigma`, `c_sigma` — redshift-evolution parameters for σ(z)

## File format

All tables use the Astropy Enhanced Character-Separated Values (`ECSV`) format. The files contain machine-readable column names, units where applicable, and metadata.

For example, they can be read in Python using Astropy:

```python
from astropy.table import Table

table = Table.read("SFR_components.ecsv", format="ascii.ecsv")
```

## Supplementary material

Supplementary figures and diagnostic plots associated with the analysis are archived separately on Zenodo:

**DOI: 10.5281/zenodo.21648421**

The Zenodo material includes diagnostic products that are not required as machine-readable tables in this repository.

## Citation

If you use these data products, please cite the associated paper:

Koprowski, M. P., Sawant, P., & Lisiecki, K., _Stellar mass growth in COSMOS-Web: a mass-complete main sequence to z ~ 8 and its consistency with GSMF evolution_, submitted to Astronomy & Astrophysics.

The citation information should be updated after the paper receives an arXiv identifier and again after publication.

## License

Please use the license specified for this repository and the corresponding Zenodo record.
