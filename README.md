# A spectral theory for turbulent burning velocity across scales: data and figures

Data and code for

> M. D. Walker, G. V. Nivarti, M. E. Mueller, R. S. Cant and S. Hochgreb,
> *A spectral theory for turbulent burning velocity across scales*, submitted to *Journal of Fluid Mechanics* (Rapids).

The notebook `make_figures.ipynb` reproduces every figure of the paper and of its supplementary material, the
symbols of table 1, and the agreement statistics quoted in §4, from the datasets in `data/`.

## Contents

```
make_figures.ipynb      notebook: model, figures, statistics
data/                   experimental and digitized data (described below)
  source/               spreadsheet from which Damkohler_1940.csv is taken
figures/                output of the notebook
```

## Running

Requires Python 3 with NumPy, SciPy, pandas, Matplotlib and Jupyter (tested with Python 3.13.9, NumPy 2.3.1,
SciPy 1.18.0, pandas 2.3.3 and Matplotlib 3.10.6). Open the notebook from this folder and run all cells, or run

```bash
jupyter nbconvert --to notebook --execute --inplace make_figures.ipynb
```

The notebook writes to `figures/`:

- `fig1a_regime` … `fig4b_landscape`: one PDF and one 1200 dpi PNG per panel of figures 1–4 of the paper;
- `supp_beta_pdf`, `supp_scaling_fan`: figures S1 and S2 of the supplementary material;
- `sym_*.pdf`: the plot symbols used in table 1;
- `predictions.csv`: for each of the 80 measurements, the inputs, $`Re`$, $`Ka`$, $`\mathcal{F}(Re)`$, the measured and
  predicted $`s_T/s_L`$, and $`A_T/A`$ where available.

## Model

The turbulent burning velocity is

```math
\frac{s_T}{s_L} = \left[1 + \mathcal{C}\,\mathcal{F}(Re)\,\frac{u'}{s_L}\right]^{1/2}, \qquad
\mathcal{F}(Re) = \int_1^{Re^{3/4}} \kappa^{*\,-4/3} \left(1 + \frac{c_L}{\kappa^{*2}}\right)^{-(5/3 + p_0)/4}
\exp\left(-\frac{\beta \kappa^*}{2 Re^{3/4}}\right) \mathrm{d}\kappa^*,
```

with $`\mathcal{C} = D_e^+ C^{1/2} (3/2)^{1/3} = 2.64`$, $`Re = (u'/s_L)(\ell_0/\delta_L)`$ and
$`Ka = (u'/s_L)^{3/2} (\ell_0/\delta_L)^{-1/2}`$. The constants are $`C = 1.5`$, $`\beta = 5.2`$, $`c_L = 6.78`$ and
$`p_0 = 0`$ for the model spectrum (Pope 2000), and $`\nu_e^+ = 0.868`$, $`Sc_T = 0.462`$ and $`D_e^+ = \nu_e^+/Sc_T = 1.88`$
at the cusp of the spectral eddy viscosity and diffusivity, from fits to Chollet (1984).

## Data

| File | Label in the paper | Source | Rows | Used in the paper |
|---|---|---|---|---|
| `Wabel_2017_Michigan.csv` | Michigan (2017) | Wabel, Skiba & Driscoll (2017) | 18 | 18 points; $`A_T/A`$ from `At/Al_shepherd` |
| `Yuen_2013_Toronto.csv` | Toronto (2013) | Yuen & Gülder (2013) | 31 | 8 methane points; $`A_T/A`$ from `At/Al_grad_c` |
| `Kobayashi_1996_Tohoku.csv` | Tohoku (1996) | Kobayashi *et al.* (1996) | 31 | 4 points at 0.1 MPa |
| `Plessing_2000_Aachen.csv` | Aachen (2000) | Plessing *et al.* (2000) | 6 | 6 points |
| `Smallwood_1995_Ottawa.csv` | Ottawa (1995) | Smallwood *et al.* (1995) | 6 | 6 points; $`s_T`$ at $`\bar c = 0.05`$ |
| `Tamadonfar_2014_Toronto.csv` | Toronto (2014) | Tamadonfar & Gülder (2014) | 16 | 16 points |
| `Filatyev_2005_Michigan.csv` | Michigan (2005) | Filatyev *et al.* (2005) | 12 | 7 points; $`A_T/A = (2/\pi)(P_T/\bar P)^2`$ |
| `Damkohler_1940.csv` | Damköhler (1940) | Damköhler (1940) | 22 | 15 points; $`s_T/s_L`$ = `wf_max/wf_min` |
| `Chollet_1984_spectral_eddy_viscosity.csv` | | Chollet (1984) | 1634 | fits in figure 2(*b*) |
| `Charlette_2002b_beta_pdf.csv` | | Charlette, Meneveau & Veynante (2002*b*) | 61 | supplementary figure S1 |

Values were transcribed from tables or digitized from figures of the original publications; please cite those
publications when using the data. Missing values are `NA`. Unless stated in the table, $`s_T/s_L`$ is taken on the
$`\bar c = 0.5`$ isosurface (`st/sl_c=0.5`), and only points with $`u'/s_L`$, $`\ell_0/\delta_L`$ and $`s_T/s_L`$ all
available are used. For Michigan (2005), the area ratio is the squared ratio of wrinkled to mean flame perimeter,
multiplied by $`2/\pi`$ as a consistency factor for comparing the slot burner with the round burners.

### Burner datasets

The seven burner files share the same columns.

| Column | Meaning |
|---|---|
| `Name`, `Experiment` | short code and identifier of the dataset |
| `Phi`, `P (MPa)`, `T(K)`, `Fuel`, `Le` | equivalence ratio, pressure, unburned temperature, fuel, Lewis number |
| `u'/sl` | turbulence intensity over laminar burning velocity, $`u'/s_L`$ |
| `l0/dl` | integral length scale over laminar flame thickness, $`\ell_0/\delta_L`$ |
| `st/sl_c=X` | turbulent burning velocity on the mean progress-variable isosurface $`\bar c = X`$, over $`s_L`$ |
| `At/Al_grad_c`, `At/Al_shepherd` | flame-surface-area ratio $`A_T/A`$ reported by the source, from the progress-variable gradient and from Shepherd's flame-surface-density method |
| `At/Al_fractal`, `At/Al_modifiedfractal` | $`A_T/A`$ from fractal analysis (Smallwood *et al.* 1995) |
| `(P_T/P)^2` | squared ratio of wrinkled to mean flame perimeter in the laser sheet (Filatyev *et al.* 2005) |
| `Burner Diameter (mm)` | burner exit diameter (slot width for Filatyev *et al.* 2005) |
| `Taylor (mm)`, `eta (mm)`, `l0 (mm)` | Taylor microscale, Kolmogorov scale, integral length scale |
| `Re_l0 (provided)`, `Ka (provided)`, `Da (provided)` | Reynolds, Karlovitz and Damköhler numbers as given by the source |
| `D (cm2/s)`, `nu (cm2/s)`, `alpha (cm2/s)`, `Pr` | mass diffusivity, kinematic viscosity, thermal diffusivity, Prandtl number |
| `Pilot (Y/N)`, `Pilot Fuel` | whether the flame is piloted, and the pilot fuel |
| `U_mean (m/s)`, `u' (m/s)`, `sl (m/s)`, `st (m/s)` | mean velocity, r.m.s. velocity fluctuation, laminar and turbulent burning velocities |
| `dl (mm)`, `dl (mm) thesis` | laminar flame thickness; for Toronto (2013), also the value given by Yuen (2009) |
| `Ma`, `Markstein_L (mm)` | Markstein number, Markstein length |

### Damköhler (1940)

`Damkohler_1940.csv` is the sheet `Full` of `source/Damkohler_1940_tables.xlsx`, which also holds the tables and
figures of Damköhler (1940) from which it was assembled. The flames are propane–butane–oxygen flames on tubes.
Columns not listed above are `Phi_O2` (equivalence ratio with oxygen), `Re_d` (tube Reynolds number),
`Re_T` ($`Re_d - 2300`$), `wf_max (cm/s)` and `wf_min (cm/s)` (flame speeds, whose ratio `wf_max/wf_min` is used as
$`s_T/s_L`$), and `r/R` and `l/R` (radial position and mixing length over tube radius).

### Chollet (1984)

`Chollet_1984_spectral_eddy_viscosity.csv` holds the curves of the spectral eddy viscosity $`\nu_e^+`$, eddy
diffusivity $`D_e^+`$ and turbulent Schmidt number $`Sc_T`$ against $`\kappa/\kappa_c`$, digitized from Chollet (1984), in
long format: `quantity` (`nu_e+`, `D_e+` or `Sc_T`), `k/kc` and `value`.

### Charlette *et al.* (2002*b*)

`Charlette_2002b_beta_pdf.csv` holds the probability density function of the local wrinkling exponent $`\beta_w`$
returned by the dynamic procedure of Charlette, Meneveau & Veynante (2002*b*), digitized: `beta_w` and `pdf`.

## References

- Charlette, F., Meneveau, C. & Veynante, D. 2002*b* A power-law flame wrinkling model for LES of premixed turbulent combustion Part II: dynamic formulation. *Combust. Flame* **131**, 181–197. [doi:10.1016/S0010-2180(02)00401-7](https://doi.org/10.1016/S0010-2180(02)00401-7)
- Chollet, J.-P. 1984 Two-point closure used for a sub-grid scale model in large eddy simulations. In *Turbulent Shear Flows 4*, pp. 62–72. Springer. [doi:10.1007/978-3-642-69996-2_5](https://doi.org/10.1007/978-3-642-69996-2_5)
- Damköhler, G. 1940 Der Einfluss der Turbulenz auf die Flammengeschwindigkeit in Gasgemischen. *Z. Elektrochem. Angew. Phys. Chem.* **46**, 601–652 (English translation: NACA TM 1112, 1947). [doi:10.1002/bbpc.19400461102](https://doi.org/10.1002/bbpc.19400461102)
- Filatyev, S. A., Driscoll, J. F., Carter, C. D. & Donbar, J. M. 2005 Measured properties of turbulent premixed flames for model assessment, including burning velocities, stretch rates, and surface densities. *Combust. Flame* **141**, 1–21. [doi:10.1016/j.combustflame.2004.07.010](https://doi.org/10.1016/j.combustflame.2004.07.010)
- Kobayashi, H., Tamura, T., Maruta, K., Niioka, T. & Williams, F. A. 1996 Burning velocity of turbulent premixed flames in a high-pressure environment. *Symp. (Int.) Combust.* **26**, 389–396. [doi:10.1016/S0082-0784(96)80240-2](https://doi.org/10.1016/S0082-0784(96)80240-2)
- Plessing, T., Kortschik, C., Peters, N., Mansour, M. S. & Cheng, R. K. 2000 Measurements of the turbulent burning velocity and the structure of premixed flames on a low-swirl burner. *Proc. Combust. Inst.* **28**, 359–366. [doi:10.1016/S0082-0784(00)80231-3](https://doi.org/10.1016/S0082-0784(00)80231-3)
- Pope, S. B. 2000 *Turbulent Flows*. Cambridge University Press.
- Smallwood, G. J., Gülder, Ö. L., Snelling, D. R., Deschamps, B. M. & Gökalp, I. 1995 Characterization of flame front surfaces in turbulent premixed methane/air combustion. *Combust. Flame* **101**, 461–470. [doi:10.1016/0010-2180(94)00226-I](https://doi.org/10.1016/0010-2180(94)00226-I)
- Tamadonfar, P. & Gülder, Ö. L. 2014 Flame brush characteristics and burning velocities of premixed turbulent methane/air Bunsen flames. *Combust. Flame* **161**, 3154–3165. [doi:10.1016/j.combustflame.2014.06.014](https://doi.org/10.1016/j.combustflame.2014.06.014)
- Wabel, T. M., Skiba, A. W. & Driscoll, J. F. 2017 Turbulent burning velocity measurements: extended to extreme levels of turbulence. *Proc. Combust. Inst.* **36**, 1801–1808. [doi:10.1016/j.proci.2016.08.013](https://doi.org/10.1016/j.proci.2016.08.013)
- Yuen, F. T. C. 2009 Experimental investigation of the dynamics and structure of lean-premixed turbulent combustion. PhD thesis, University of Toronto.
- Yuen, F. T. C. & Gülder, Ö. L. 2013 Turbulent premixed flame front dynamics and implications for limits of flamelet hypothesis. *Proc. Combust. Inst.* **34**, 1393–1400. [doi:10.1016/j.proci.2012.06.167](https://doi.org/10.1016/j.proci.2012.06.167)
