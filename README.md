# Air as Class

A data essay on spatial inequality, pollution, and health in Los Angeles County.

> The conventional story says poor neighborhoods breathe dirtier air.
> The data from LA tells a stranger one — about who gets pushed where,
> and what they inherit when they get there.

---

## Background

Environmental justice frameworks predict a positive correlation between neighborhood poverty and exposure to air pollution. This study tests that prediction within Los Angeles County, using CalEnviroScreen 4.0 data on 2,295 census tracts. The relationship between poverty and PM2.5 turns out to be weak. The relationship between poverty and asthma is strong but operates largely outside the air-pollution pathway. The framework that captures this needs two axes: pollution exposure (the inherited environmental-justice axis) and displacement (the geography of who can afford to stay in the urban basin and who is pushed out).

## Research question

Within Los Angeles County, does poverty predict (a) air pollution exposure and (b) respiratory health outcomes, and what is the relative magnitude of each pathway?

## Data

[CalEnviroScreen 4.0](https://oehha.ca.gov/calenviroscreen/report/calenviroscreen-40), California Office of Environmental Health Hazard Assessment (OEHHA), 2021. Census-tract-level indicators for 8,035 California tracts.

Filtered to Los Angeles County. After dropping rows with missing values in the analytical variables: **n = 2,295**.

Variables used:

| Variable | Field | Unit |
|---|---|---|
| Air pollution | `PM2.5` | μg/m³, annual mean |
| Poverty | `Poverty` | % below 2× federal poverty level |
| Asthma | `Asthma` | age-adjusted ER visits per 10,000 |
| Location | `Approximate Location` | nearest city (string) |

## Methods

**Spatial classification.** Tracts whose `Approximate Location` matches `Lancaster`, `Palmdale`, `Santa Clarita`, `Unincorporated`, `Acton`, `Agua Dulce`, `Castaic`, or `Quartz Hill` were tagged as **Outer LA** (high-desert exurbs north of the San Gabriel Mountains). All other LA County tracts: **Inner LA**. Split: Inner = 2,138, Outer = 157.

**Correlations.** Pearson's r for Poverty × PM2.5, Poverty × Asthma, PM2.5 × Asthma; computed pooled and within each subset.

**Regression.** OLS of asthma on PM2.5 and poverty, all standardized:

```
Asthma_std = β₀ + β₁ · PM2.5_std + β₂ · Poverty_std + ε
```

Standardization makes coefficients directly comparable in SD units. n = 2,305.

**Visualization.** `matplotlib` scatter plots, boxplots, and bar comparisons. `folium` for the geographic map. Static figures exported as PNG; gallery poster also as SVG.

## Findings

### 1. Air does not track poverty

| Correlation | Pooled | Inner LA | Outer LA |
|---|---|---|---|
| Poverty × PM2.5 | 0.07 | 0.16 | −0.36 |

PM2.5 in LA hovers around 11–13 μg/m³ across most poverty levels. Outer LA tracts sit around 7.8 μg/m³ — substantially cleaner — even though their poverty rates equal or exceed inner-city poor neighborhoods.

### 2. Asthma does

| Correlation | Pooled | Inner LA | Outer LA |
|---|---|---|---|
| Poverty × Asthma | 0.53 | 0.52 | 0.74 |
| PM2.5 × Asthma | −0.10 | 0.20 | −0.63 |

Poverty predicts asthma strongly. PM2.5 does not — and within Outer LA the sign reverses: cleaner-air tracts there carry the heaviest asthma burden.

### 3. Three populations

| Group | n | Poverty | PM2.5 | Asthma ER | No HS | Unemployment |
|---|---|---|---|---|---|---|
| Rich Inner | 300 | 10.4% | 11.6 | 29.9 | 4.4% | 4.5% |
| Poor Inner | 525 | 60.8% | 12.0 | 70.4 | 41.7% | 8.1% |
| **Poor Outer** | **30** | **60.2%** | **7.8** | **121.7** | 29.7% | 10.2% |

Two populations are equally poor by income. Poor Outer breathes air 35% cleaner than Poor Inner and visits ERs for asthma 73% more often.

### 4. Regression

Standardized OLS, n = 2,305, R² = 0.298, both p < 0.001:

| Predictor | β (std) |
|---|---|
| PM2.5 | −0.168 |
| Poverty | +0.542 |

Poverty predicts asthma **3.2× more strongly than PM2.5 does**, holding the other constant. The negative sign on PM2.5 is structural, not causal: tracts with the cleanest air in LA are disproportionately tracts where displaced low-income residents now live.

## Interpretation

The 30 Outer LA tracts are not an anomaly. They are the second axis of LA's geography of harm — a population pushed by urban rent into exurbs where economic activity is thin and healthcare is far. Reducing PM2.5 in the urban basin will not close the asthma gap because the gap is anchored in displacement, not emissions.

## Limitations

1. Tract-level data does not capture within-tract variation.
2. Asthma is measured as ER visits, which conflates prevalence with healthcare access.
3. The analysis is cross-sectional; displacement is temporal and would require panel data to model directly.
4. LA only — patterns may differ in other metros.
5. PM2.5 is one air-quality dimension among several.

## Files

```
air-as-class/
├── README.md
├── air_as_class.ipynb                          # full analysis notebook
├── calenviroscreen40resultsdatadictionary_F_2021.xlsx
└── figures/
    ├── air_as_class_scatter.png
    ├── air_as_class_boxplot.png
    ├── air_as_class_two_kinds.png
    ├── air_as_class_asthma.png
    └── air_as_class_summary.png
```

## Reproducibility

```bash
pip install pandas matplotlib statsmodels folium openpyxl
jupyter lab air_as_class.ipynb
```

## Author

**Sora Kim** — independent researcher, Seoul.
[sorakim-lab.github.io](https://sorakim-lab.github.io) · [ORCID 0009-0003-0856-4193](https://orcid.org/0009-0003-0856-4193)

## License

Code: MIT. Data: OEHHA public records.
