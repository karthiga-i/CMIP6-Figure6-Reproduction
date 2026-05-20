# CMIP6 Reproduction of Held & Soden Fig. 6

This repository contains a Jupyter Notebook that reproduces a CMIP6-based version of Fig. 6 from Held and Soden (2006), *Robust Responses of the Hydrological Cycle to Global Warming*.

The figure compares the actual zonal-mean change in precipitation minus evaporation, `P-E`, with a simple thermodynamic prediction based on Clausius-Clapeyron scaling.

## About the Paper

The reference paper is:

**Held, I. M., and B. J. Soden. 2006. Robust Responses of the Hydrological Cycle to Global Warming. _Journal of Climate_, 19, 5686-5699.**

The paper examines how the hydrological cycle responds to global warming using climate model simulations from the PCMDI/AR4 archive. Its central idea is that many robust hydrological changes are linked to the increase in lower-tropospheric water vapor as the atmosphere warms.

A key physical basis of the paper is the Clausius-Clapeyron relationship. At typical lower-tropospheric temperatures, saturation water vapor increases by about 7% per 1 K of warming. However, global mean precipitation does not increase at the same rate, because it is constrained by the atmospheric and surface energy balance. Therefore, the paper distinguishes between:

- rapid water-vapor increase, approximately 7% per K
- slower global mean precipitation increase
- enhanced moisture transport
- strengthening of existing wet and dry patterns
- changes in precipitation minus evaporation, `P-E`

Fig. 6 in the paper focuses on the zonal-mean change in `P-E`. It compares the actual model response with a simple thermodynamic estimate:

```text
Delta(P-E) ≈ 0.07 * Delta T * (P-E)
```

This means that, if circulation and relative humidity patterns remain approximately similar, regions that are already wet tend to become wetter, and regions that are already dry tend to become drier.

This repository reproduces the same idea using CMIP6 data instead of the older CMIP3/AR4 model archive used in the original paper.

## Main Notebook

```text
Figure6_Workingcode - Copy.ipynb
```

## Aim of the Analysis

The aim is to reproduce the structure of Held and Soden Fig. 6 using CMIP6 model output.

The original paper used:

- 20C3M for the historical climate
- SRES A1B for the future climate
- 2xCO2 slab-ocean runs for panel (c)

This CMIP6 version uses:

- `historical` for the past climate
- `ssp370` for the future climate
- no slab-ocean panel, because comparable slab-ocean CMIP6 runs are not included in this dataset

## Required CMIP6 Variables

The notebook requires monthly mean CMIP6 NetCDF files for these variables:

| Variable | Meaning |
|---|---|
| `tas` | Near-surface air temperature |
| `pr` | Precipitation |
| `evspsbl` | Evaporation / evapotranspiration |

The main hydrological variable is:

```text
P - E = pr - evspsbl
```

where:

- `P` is precipitation
- `E` is evaporation


## Climate Experiments and Periods

The analysis compares early and late 20-year climate means.

### Historical

```text
1900-1919 compared with 1995-2014
```

### SSP370

```text
2015-2034 compared with 2081-2100
```

The middle years are not used because the figure compares an early climate state with a late climate state, following the method used in the original paper.

## Method Summary

For each model and experiment, the notebook calculates the early-period and late-period climatological means.

The actual model response is:

```text
Delta(P-E) = (P-E)_late - (P-E)_early
```

The global mean surface temperature change is:

```text
Delta T_global = T_late - T_early
```

The response is normalized by global mean warming:

```text
Delta(P-E) / Delta T_global
```

The thermodynamic prediction is calculated using:

```text
Delta(P-E)_predicted = 0.07 * Delta T_local * (P-E)_early
```

This represents the approximate 7% increase in lower-tropospheric water vapor per 1 K of warming.

## Ensemble Mean

The notebook uses several CMIP6 climate models. Each model produces its own actual and predicted zonal-mean curves.

The ensemble mean is calculated by averaging the curves across all successfully processed models.

This reduces dependence on a single model and shows the response that is common across the selected model group.

## Output Figure

The notebook produces a two-panel figure:

| Panel | Experiment | Period Comparison |
|---|---|---|
| (a) | `historical` | 1900-1919 to 1995-2014 |
| (b) | `ssp370` | 2015-2034 to 2081-2100 |

In each panel:

- solid line = CMIP6 ensemble response
- dashed line = thermodynamic prediction

The x-axis is latitude.

The y-axis follows the original figure style:

```text
delta(P-E) (mm/day)
```

Strictly, because the response is normalized by global mean warming, the plotted quantity can also be interpreted as:

```text
mm/day per K of global warming
```

However, the axis label is kept close to the original Held and Soden figure.



The notebook will:

1. Load CMIP6 NetCDF files.
2. Select `tas`, `pr`, and `evspsbl`.
3. Calculate early and late period means.
4. Calculate `Delta(P-E)`.
5. Calculate the thermodynamic prediction.
6. Interpolate model outputs to a common latitude grid.
7. Average across models to create the ensemble mean.
8. Plot the recreated Fig. 6.
9. Save the figure.


## Notes

- The recreated figure will not be identical to the original paper because this analysis uses CMIP6 models rather than CMIP3/AR4 models.
- The original future experiment was SRES A1B, while this version uses `ssp370`.
- The selected CMIP6 models may have different grids, calendars, institutions, and ensemble members.
- The notebook interpolates zonal-mean results onto a common latitude grid before averaging.
- Some models may be skipped automatically if required files or variables are missing.

## Reference

Held, I. M., and B. J. Soden. 2006. Robust Responses of the Hydrological Cycle to Global Warming. *Journal of Climate*, 19, 5686-5699.
