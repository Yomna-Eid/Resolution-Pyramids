# Estimating Spatial Means from Earth Observation products can be done accurately from Downsampled Images

This repository contains the code, and evaluation results for the publication.

Building on previous work presented at [EGU25](https://meetingorganizer.copernicus.org/EGU25/EGU25-19140.html) titled "**When is a finer spatial resolution justified in remote sensing analysis?**"
<details>
<summary>
  Abstract
</summary>

Remote sensing analysis is often used to provide supporting information for evidence-informed policy-making. Typically, such analysis presents results as classification maps, such as a land cover classification used to estimate deforestation areas in a region. For such analyses, where aggregated areal values of specific classes are the primary targets, a critical question arises: do the results significantly degrade when lower spatial resolution Earth Observation (EO) products are used instead of higher-resolution ones?

EO products like Dynamic World land use and land cover maps, produced at a high temporal and spatial resolution (5 days and 10m, respectively), are built on the widely held belief that higher resolutions inherently yield better results. However, with the exponential growth in data volumes and the computational demands of high-resolution workflows, it becomes increasingly important to determine where these resource-intensive approaches provide meaningful advantages — and where they do not — to balance computational efficiency with the need for accuracy in remote sensing workflows.

To address this question, we examine two case studies: deforestation in the Cerrado Biome of Brazil, and the imperviousness of sealed surfaces in Germany. Classification maps from each study are systematically downsampled from their native resolutions in steps up to 10 km spatial resolution. Using Ripley’s Equation[^1], numerically approximated with a Gaussian-Quadrature approach, we compute standard errors to assess the impact of spatial resolution on classification accuracy.

We report our findings on how the aggregated target values derived from lower-resolution data compare to those from higher-resolution inputs. We also try to identify the resolution thresholds beyond which the quality of the final product loses acceptable representation of the phenomena in the selected use cases.

[^1]: See Eq. 3.4, page 23 in Ripley, B.D. (1981), “Spatial Sampling” in Spatial Statistics.

**How to cite:** 
```md
Eid, Y. and Pebesma, E.: When is a finer spatial resolution justified in remote sensing analysis?, 
EGU General Assembly 2025, Vienna, Austria, 
27 Apr–2 May 2025, EGU25-19140, 
https://doi.org/10.5194/egusphere-egu25-19140, 2025.
```
</details>


## Overview
This repository contains the code used to quantify the standard error of spatial means _(uncertainty)_ of target spatially aggregated classes derived from Earth Observation (EO) classification products. The study evaluates how downsampling spatial resolution affects the accuracy of aggregated class estimates using a model-based variance estimator following Ripley (1981)[^1].

Two numerical integration approaches are implemented to approximate the covariance terms required for the variance calculation:
- Monte Carlo integration (stochastic sampling)
- Gauss Quadrature (deterministic sampling)

The methodology is demonstrated using two case studies:
1. Impervious surface density mapping (Copernicus HRL Imperviousness Density)
2. Deforestation area monitoring (PRODES dataset, INPE - Brazil)

The repository provides the full workflow for:
- preprocessing the EO datasets
- downsampling schemes
- fitting variogram models
- computing covariance-based standard errors
- reproducing figures and results presented in the paper

The code is written in R and relies on standard geospatial and numerical libraries.

## Repository Structure
```
├── Data/                      # (empty) Placeholder for input datasets
├── Results/                   # Generated downsampled raster files (outputs of scripts 1a. & 2a.)
│   ├── Imperviousness/        # Downsampled raster files for the HRL Imperviousness binary 
│   │   ├── 2006/                classification maps (output of script 1a.)
│   │   ├── 2009/
│   │   ├── 2012/
│   │   ├── 2015/
│   │   └── 2019/
│   ├── Deforestation/         # Downsampled raster files for the PRODES Yearly Deforesation
│   ├── ├── 2002/                binary classification maps (outout of script 2a.)
│   ├── ├── 2004/
│   ├── ├── 2006/
│   ├── ├── 2008/
│   ├── ├── 2010/
│   ├── ├── 2012/
│   ├── ├── 2014/
│   ├── ├── 2016/
│   ├── ├── 2018/
│   ├── ├── 2020/
│   ├── ├── 2022/
│   └── └── 2024/
├── 1a. CaseStudy_Imperviousness.Rmd            # Pre-processing of the data to create 
│                                                 downsampled raster HRL-IMP maps
├── 1b. MonteCarlo_GaussQuad.Rmd                # Calculation of the SE of mean using the 
│                                                 two numerical methods for the IMP case study
├── 1c. CaseStudy_Imperviousness_Results.Rmd    # Producing the results + figures used in the paper
├── 2a. CaseStudy_Deforestation.Rmd             # Pre-processing of the data to create 
│                                                 downsampled raster PRODES deforestation maps
├── 2b. PRODES_MonteCarlo_GaussQuad.Rmd         # Calculation of the SE of mean using the 
│                                                 two numerical methods for PRODES case study
├── 2c. CaseStudy_Deforestation_Results.RmD     # Producing the results + figures used in the paper
├── Data.md                    # Details about the data and citations
├── LICENSE.txt                
└── README.md                 
```

## Data
The datasets used in this study are publicly available Earth Observation products but are not included in this repository due to their size (~GB scale) and licensing restrictions.

The datasets used in this study are publicly available but are not included in this repository due to their size and licensing restrictions. They were acquired from the following official services:

- Copernicus Land Monitoring Service, HRL Imperviousness.
- INPE PRODES, Yearly Deforestation.

For further information regarding the portal, downloading the data from their sources, and the dataset citation, please refer to the [Data.md](Data.md).

After downloading, place the data in the [`./Data`](Data) directory.

Alternatively, the resulting cropped and downsampled raster files are also provided in the [`./Results`](Results) directory of the repository.

## License
This repository is released under the MIT License (see [LICENSE](LICENSE.txt)).

Note that the datasets used in this study are subject to their respective licenses and terms of use. Users are responsible for complying with the licensing conditions of the original data providers.

## Citation
If you use this code or methodology in your work, please cite the repository:

```md
@misc{YomnaEid2026Downsampling,
  author        = {Yomna Eid}
  title         = {Project Name: Resolution Pyramids}
  year          = {2026}
  howpublished  = {\url{https://github.com/Yomna-Eid/Resolution-Pyramids}}
  note          = {Version 1.0}
  } 
```

A corresponding paper is currently under submission and will be added here once available.
