# A spectral theory for turbulent burning velocity across scales: data and figures

Data and code for

> M. D. Walker, G. V. Nivarti, M. E. Mueller, R. S. Cant and S. Hochgreb,
> *A spectral theory for turbulent burning velocity across scales*, submitted to *Journal of Fluid Mechanics* (Rapids).

The notebook `make_figures.ipynb` reproduces every figure of the paper and of its supplementary material, the
symbols of table 1, and the agreement statistics quoted in §4, from the datasets in `data/`. A second notebook,
`data/source/Damkohler_laminar_flames.ipynb`, computes laminar flames at the conditions of Damköhler (1940), to
compare with his flame speeds and with the flame thickness used for his data (§S2.5 of the supplementary material).

## Contents

```
make_figures.ipynb                    notebook: model, figures, statistics
data/                                 experimental and digitized data (described below)
  source/
    Damkohler_1940_tables.xlsx        tables and figures of Damköhler (1940), from which Damkohler_1940.csv is taken
    Damkohler_laminar_flames.ipynb    notebook: laminar flames at Damköhler's conditions
    mechanisms/                       chemical mechanisms for these flames
    Damkohler_laminar_flames/         output of Damkohler_laminar_flames.ipynb
figures/                              output of make_figures.ipynb
```

## Running

Requires Python 3 with NumPy, SciPy, pandas, Matplotlib and Jupyter (tested with Python 3.13.9, NumPy 2.3.1,
SciPy 1.18.0, pandas 2.3.3 and Matplotlib 3.10.6). Open the notebook from this folder and run all cells, or run

```bash
jupyter nbconvert --to notebook --execute --inplace make_figures.ipynb
```

The notebook writes to `figures/`:

- `fig1a_regime` … `fig4b_landscape`: one PDF and one 1200 dpi PNG per panel of figures 1–4 of the paper;
- `supp_beta_pdf`, `supp_regime_skiba`, `supp_regime_bray`, `supp_scaling_fan`: figures S1, S2(*a*), S2(*b*) and S3
  of the supplementary material;
- `graphical_abstract`: the graphical abstract, a single panel based on figure 4(*b*) on a 6 cm × 5 cm canvas, as a
  1200 dpi JPEG and a PDF;
- `sym_*.pdf`: the plot symbols used in table 1;
- `predictions.csv`: for each of the 80 measurements, the inputs, $`Re`$, $`Ka`$, $`\mathcal{F}(Re)`$, the measured and
  predicted $`s_T/s_L`$, and $`A_T/A`$ where available.

`data/source/Damkohler_laminar_flames.ipynb` also needs Cantera and openpyxl; it is described in the last section.

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

`Damkohler_1940.csv` is the sheet `Full` of `source/Damkohler_1940_tables.xlsx` without its empty columns; the other
sheets transcribe tables 1–3, 8 and 9 and figures 5 and 6(*a*) of Damköhler (1940), from which it was assembled. The
flames are propane–butane–oxygen flames on tubes of 1.385, 2.18 and 2.718 mm diameter. The fuel, Damköhler's technical
propane, is taken as 85 % propane and 15 % butane by volume (column `Fuel`), as he inferred from its mean molar mass of
46.1 g/mol. Columns not listed above are `Phi_O2` (equivalence ratio with oxygen), `Re_d` (tube Reynolds number),
`Re_T` ($`Re_d - 2300`$), `wf_max (cm/s)` and `wf_min (cm/s)` (flame speeds from the inner and outer boundaries of the
luminous zone, whose ratio `wf_max/wf_min` is used as $`s_T/s_L`$), and `r/R` and `l/R` (radial position and mixing
length over tube radius). As described in §S2.5 of the supplementary material, `l0 (mm)` is the largest mixing length
in each tube, `dl (mm)` is 0.01 mm, a tenth of the 0.1 mm thickness Damköhler gives for the luminous zone of his
laminar flames, and `u'/sl` is $`Re_T^{1/2}/(\ell_0/\delta_L)`$, or zero for $`Re_d < 2300`$.

### Chollet (1984)

`Chollet_1984_spectral_eddy_viscosity.csv` holds the curves of the spectral eddy viscosity $`\nu_e^+`$, eddy
diffusivity $`D_e^+`$ and turbulent Schmidt number $`Sc_T`$ against $`\kappa/\kappa_c`$, digitized from Chollet (1984), in
long format: `quantity` (`nu_e+`, `D_e+` or `Sc_T`), `k/kc` and `value`.

### Charlette *et al.* (2002*b*)

`Charlette_2002b_beta_pdf.csv` holds the probability density function of the local wrinkling exponent $`\beta_w`$
returned by the dynamic procedure of Charlette, Meneveau & Veynante (2002*b*), digitized: `beta_w` and `pdf`.

## Laminar flames at Damköhler's conditions

`data/source/Damkohler_laminar_flames.ipynb` computes freely propagating, adiabatic, laminar flames with Cantera
(Goodwin *et al.* 2025) for the 22 mixtures of Damköhler's table 3 (sheet `Table 3` of the spreadsheet): his
technical propane, 85 % propane and 15 % butane by volume, burning in pure oxygen at 18 °C and 1 atm. It compares
the laminar burning velocity $`s_L`$ with his flame speeds, and the flame thickness by several definitions with the
$`\delta_L = 0.01`$ mm used for his data in the paper. `MECH_FILE` selects the mechanism, from `mechanisms/`:

- `UCSD_mech.yaml`: UCSD mech, the C<sub>0</sub>–C<sub>4</sub> mechanism of the University of California at San Diego,
  57 species and 268 reactions ([*Chemical-kinetic mechanisms for combustion applications*](https://web.eng.ucsd.edu/mae/groups/combustion/mechanism.html),
  Mechanical and Aerospace Engineering, Combustion Research, UCSD);
- `USC_mech_II.yaml`: USC mech II (Wang *et al.* 2007), 111 species and 784 reactions, with the thermodynamic data of
  every species from the mechanism's own `thermdat.txt`; the header of the file lists the species whose data were
  corrected.

Besides the packages above, the notebook needs Cantera 3.1 or later and openpyxl; the results here are from Cantera
3.2.0. Run it once for each mechanism, which takes about 10 minutes with UCSD mech and 20 with USC mech II. It
writes to `data/source/Damkohler_laminar_flames/`:

- `sL_Damkohler`: $`s_L`$ from both mechanisms against Damköhler's flame speeds $`w_{f,\min}`$ and $`w_{f,\max}`$, as
  a PDF and a 1200 dpi PNG;
- `<mechanism>/dL_definitions`: the flame thickness by each definition;
- `<mechanism>/flame_speed_technical_propane_<mechanism>_P_1.00.csv`: one row per mixture, with $`s_L`$, the peak
  temperature, the flame thickness by each definition, the Prandtl, Schmidt and Lewis numbers, properties of the
  burned gas, and the activation temperature, Zel'dovich number and Markstein number;
- `<mechanism>/Array_tech_propane_<phi>_P_1.01.csv`: the structure of each flame.

With UCSD mech, $`s_L`$ is 0.53–3.0 m/s; USC mech II gives 7–12 % less. For the laminar tests, the median
$`w_{f,\min}/s_L`$ is 1.01 with UCSD mech and 1.11 with USC mech II. The thermal thickness
$`(T_b - T_u)/\max(\mathrm{d}T/\mathrm{d}x)`$, 0.07–0.29 mm, is comparable to the 0.1 mm Damköhler gives for the
luminous zone, whereas $`\nu_u/s_L`$, with the kinematic viscosity of the reactants, is 0.004–0.019 mm, so that the
$`\delta_L`$ = 0.01 mm of the paper corresponds to $`\nu/s_L`$. The activation temperature, Zel'dovich number and
Markstein number in the summary files are not meaningful for these flames (see the notes at the end of the notebook).

## References

- Charlette, F., Meneveau, C. & Veynante, D. 2002*b* A power-law flame wrinkling model for LES of premixed turbulent combustion Part II: dynamic formulation. *Combust. Flame* **131**, 181–197. [doi:10.1016/S0010-2180(02)00401-7](https://doi.org/10.1016/S0010-2180(02)00401-7)
- Chollet, J.-P. 1984 Two-point closure used for a sub-grid scale model in large eddy simulations. In *Turbulent Shear Flows 4*, pp. 62–72. Springer. [doi:10.1007/978-3-642-69996-2_5](https://doi.org/10.1007/978-3-642-69996-2_5)
- Damköhler, G. 1940 Der Einfluss der Turbulenz auf die Flammengeschwindigkeit in Gasgemischen. *Z. Elektrochem. Angew. Phys. Chem.* **46**, 601–652 (English translation: NACA TM 1112, 1947). [doi:10.1002/bbpc.19400461102](https://doi.org/10.1002/bbpc.19400461102)
- Filatyev, S. A., Driscoll, J. F., Carter, C. D. & Donbar, J. M. 2005 Measured properties of turbulent premixed flames for model assessment, including burning velocities, stretch rates, and surface densities. *Combust. Flame* **141**, 1–21. [doi:10.1016/j.combustflame.2004.07.010](https://doi.org/10.1016/j.combustflame.2004.07.010)
- Goodwin, D. G., Moffat, H. K., Schoegl, I., Speth, R. L. & Weber, B. W. 2025 Cantera: an object-oriented software toolkit for chemical kinetics, thermodynamics, and transport processes, version 3.2.0. [doi:10.5281/zenodo.17620923](https://doi.org/10.5281/zenodo.17620923)
- Kobayashi, H., Tamura, T., Maruta, K., Niioka, T. & Williams, F. A. 1996 Burning velocity of turbulent premixed flames in a high-pressure environment. *Symp. (Int.) Combust.* **26**, 389–396. [doi:10.1016/S0082-0784(96)80240-2](https://doi.org/10.1016/S0082-0784(96)80240-2)
- Plessing, T., Kortschik, C., Peters, N., Mansour, M. S. & Cheng, R. K. 2000 Measurements of the turbulent burning velocity and the structure of premixed flames on a low-swirl burner. *Proc. Combust. Inst.* **28**, 359–366. [doi:10.1016/S0082-0784(00)80231-3](https://doi.org/10.1016/S0082-0784(00)80231-3)
- Pope, S. B. 2000 *Turbulent Flows*. Cambridge University Press.
- Smallwood, G. J., Gülder, Ö. L., Snelling, D. R., Deschamps, B. M. & Gökalp, I. 1995 Characterization of flame front surfaces in turbulent premixed methane/air combustion. *Combust. Flame* **101**, 461–470. [doi:10.1016/0010-2180(94)00226-I](https://doi.org/10.1016/0010-2180(94)00226-I)
- Tamadonfar, P. & Gülder, Ö. L. 2014 Flame brush characteristics and burning velocities of premixed turbulent methane/air Bunsen flames. *Combust. Flame* **161**, 3154–3165. [doi:10.1016/j.combustflame.2014.06.014](https://doi.org/10.1016/j.combustflame.2014.06.014)
- Wabel, T. M., Skiba, A. W. & Driscoll, J. F. 2017 Turbulent burning velocity measurements: extended to extreme levels of turbulence. *Proc. Combust. Inst.* **36**, 1801–1808. [doi:10.1016/j.proci.2016.08.013](https://doi.org/10.1016/j.proci.2016.08.013)
- Wang, H., You, X., Joshi, A. V., Davis, S. G., Laskin, A., Egolfopoulos, F. & Law, C. K. 2007 USC Mech Version II. High-temperature combustion reaction model of H<sub>2</sub>/CO/C<sub>1</sub>–C<sub>4</sub> compounds. [ignis.usc.edu/USC_Mech_II.htm](http://ignis.usc.edu/USC_Mech_II.htm)
- Yuen, F. T. C. 2009 Experimental investigation of the dynamics and structure of lean-premixed turbulent combustion. PhD thesis, University of Toronto.
- Yuen, F. T. C. & Gülder, Ö. L. 2013 Turbulent premixed flame front dynamics and implications for limits of flamelet hypothesis. *Proc. Combust. Inst.* **34**, 1393–1400. [doi:10.1016/j.proci.2012.06.167](https://doi.org/10.1016/j.proci.2012.06.167)
