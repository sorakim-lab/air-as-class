# Air as Class

A data essay on spatial inequality, pollution, and health in Los Angeles County.

> The conventional story says poor neighborhoods breathe dirtier air.
> The data from LA tells a stranger one — about who gets pushed where,
> and what they inherit when they get there.

---

## Background

Environmental justice scholarship since the 1990s — beginning with Robert Bullard's foundational work on toxic-waste siting — has consistently documented a positive association between neighborhood poverty, racial composition, and exposure to environmental hazards. The framework predicts that low-income and minority communities bear a disproportionate burden of air pollution, and that this exposure drives a measurable share of the health gap between rich and poor.

This study tests that prediction within Los Angeles County, using the most comprehensive environmental-justice dataset available for California — CalEnviroScreen 4.0 — and finds that the relationship between poverty and air quality is weaker than the framework anticipates, while the relationship between poverty and asthma is strong and operates largely outside the air-pollution pathway. The implication is not that environmental justice is wrong, but that it captures only one axis of a two-axis system. The second axis is displacement.

## Research question

Within Los Angeles County, does poverty predict (a) air pollution exposure and (b) respiratory health outcomes, and what is the relative magnitude of each pathway?

## Data

**Source.** [CalEnviroScreen 4.0](https://oehha.ca.gov/calenviroscreen/report/calenviroscreen-40), California Office of Environmental Health Hazard Assessment (OEHHA), released 2021. Census-tract-level environmental and demographic indicators covering 8,035 tracts statewide.

**Filter.** Los Angeles County only (`California County == "Los Angeles"`). Initial subset: 2,343 tracts. After removing rows with missing values in any of the four analytical variables (`PM2.5`, `Poverty`, `Asthma`, `Approximate Location`): **n = 2,295**.

**Key variables used:**

| Variable | CalEnviroScreen field | Unit | Description |
|---|---|---|---|
| Air pollution | `PM2.5` | μg/m³ | Annual mean concentration of fine particulate matter, 2015–2017 |
| Poverty | `Poverty` | % | Share of population below 2× federal poverty level, ACS 2015–2019 |
| Asthma | `Asthma` | rate | Age-adjusted ER visits for asthma per 10,000, 2015–2017 |
| Location | `Approximate Location` | string | OEHHA-assigned nearest city name |

## Methods

### Spatial classification

Tracts were classified as **Outer LA** if their `Approximate Location` matched one of the following keywords: `Lancaster`, `Palmdale`, `Santa Clarita`, `Unincorporated`, `Acton`, `Agua Dulce`, `Castaic`, `Quartz Hill`. These correspond to high-desert exurbs in the Antelope Valley and unincorporated areas beyond the San Gabriel Mountains. All other LA County tracts were classified as **Inner LA**. Final split: Inner = 2,138, Outer = 157.

This is a binary geographic classification, not a sociological one. It is intended to separate the urbanized LA basin (south of the mountains) from the high-desert exurbs (north of the mountains) where displacement of low-income residents has been documented since the 2010s ("suburbanization of poverty").

### Statistical analysis

**Pairwise correlations** were computed between Poverty, PM2.5, and Asthma using Pearson's r, separately for Inner and Outer subsets and pooled.

**Multiple linear regression** of Asthma on Poverty and PM2.5:

```
Asthma_std = β₀ + β₁ · PM2.5_std + β₂ · Poverty_std + ε
```

All variables standardized (mean = 0, SD = 1) so coefficients are directly comparable in standard-deviation units. Estimated via OLS using `statsmodels.api.OLS`. n = 2,305 (rows with non-missing values in all three variables).

### Visualization

Scatter plots, boxplots, and a three-group bar comparison constructed in `matplotlib`. Geographic distribution mapped using `folium` on OpenStreetMap base tiles. All figures exported as PNG (raster) and SVG (vector).

## Findings

### 1. Air does not track poverty cleanly

| Correlation | Pooled | Inner LA | Outer LA |
|---|---|---|---|
| Poverty ↔ PM2.5 | 0.07 | 0.16 | −0.36 |

Across LA County as a whole, the relationship is essentially flat. PM2.5 ranges narrowly between 11 and 13 μg/m³ for the vast majority of urban tracts regardless of poverty rate. The Outer LA tracts cluster around PM2.5 = 7.8 μg/m³ — substantially cleaner than the urban basin — despite having poverty rates equal to or higher than inner-city poor neighborhoods.

### 2. Asthma does

| Correlation | Pooled | Inner LA | Outer LA |
|---|---|---|---|
| Poverty ↔ Asthma | 0.53 | 0.52 | 0.74 |
| PM2.5 ↔ Asthma | −0.10 | 0.20 | −0.63 |

Poverty is a strong positive predictor of asthma everywhere. PM2.5 is nearly uncorrelated with asthma pooled, weakly positive within Inner LA, and *strongly negative* within Outer LA — driven by the same cluster of cleaner-air, high-poverty tracts in the Antelope Valley that show the highest asthma rates in the dataset.

### 3. Three populations, three different geographies of harm

| Group | n | Poverty | PM2.5 | Asthma ER | Education (no HS) | Unemployment |
|---|---|---|---|---|---|---|
| Rich Inner | 300 | 10.4% | 11.6 μg/m³ | 29.9 | 4.4% | 4.5% |
| Poor Inner | 525 | 60.8% | 12.0 μg/m³ | 70.4 | 41.7% | 8.1% |
| **Poor Outer** | **30** | **60.2%** | **7.8 μg/m³** | **121.7** | 29.7% | 10.2% |

Two populations are equally poor by income. Their air is not equal. Their lungs are less equal still. Poor Outer tracts breathe air roughly 35% cleaner than Poor Inner — and visit ERs for asthma 73% more often.

### 4. Regression decomposes the pathway

Standardized OLS coefficients (n = 2,305; R² = 0.298; both p < 0.001):

| Predictor | β (std) | Direction |
|---|---|---|
| PM2.5 | −0.168 | negative |
| Poverty | +0.542 | positive |

Poverty predicts asthma **3.2× more strongly than PM2.5 does**, holding the other constant. The negative sign on PM2.5 is not a causal claim that cleaner air worsens asthma — it is a structural artifact: the tracts with the cleanest air in LA are disproportionately tracts where displaced low-income residents now live, and those residents bring with them the determinants of asthma that operate outside the air pathway (housing quality, occupational exposures, healthcare access, chronic stress).

## Interpretation

The 30 tracts in Lancaster, Palmdale, and the unincorporated outer county are not an outlier to be dismissed. They are the second axis of LA's geography of harm — a population pushed by rising urban rents into exurbs where economic activity is thin, healthcare is two counties away, and the air, incidentally, is clean. Reducing PM2.5 further in the urban basin will improve some metrics but will not close the asthma gap, because the asthma gap is now anchored in the geography of displacement, not the geography of emissions.

The framework that captures this needs both axes:

- **Pollution exposure** (the inherited environmental-justice axis)
- **Displacement geography** (the emerging axis)

Tracts at the intersection of high poverty and either axis carry the burden. Tracts at the intersection of high poverty and *both* axes — the 30 in Outer LA, plus their counterparts in Inner LA's worst pollution corridors — carry the most.

## Limitations

1. **Ecological inference.** Census-tract-level data does not capture within-tract heterogeneity. Individual exposure histories can diverge sharply from tract means.
2. **Asthma proxy.** ER-visit rates conflate underlying disease prevalence with healthcare access patterns. Areas with worse access may show suppressed counts.
3. **Cross-sectional.** This is a snapshot, not a longitudinal model. The displacement dynamics it points to are temporal and would require panel data to test directly.
4. **Single county.** Patterns may differ in other US metros with different geographic constraints (no mountain barriers, different exurb dynamics).
5. **PM2.5 only.** Air pollution is multidimensional. The conventional pathway may operate through ozone, diesel particulates, or near-roadway exposures not captured by a single PM2.5 mean.

## Repository structure

```
air-as-class/
├── README.md
├── air_as_class.ipynb           # full analysis notebook
├── data/
│   └── calenviroscreen40_F_2021.xlsx
├── figures/
│   ├── air_as_class_poster.svg  # gallery poster, vector
│   ├── air_as_class_poster.png  # gallery poster, raster
│   ├── 01_scatter_pm.png        # individual figures
│   ├── 02_scatter_asthma.png
│   ├── 03_three_worlds.png
│   └── 04_regression.png
└── outputs/
    └── la_data.csv              # cleaned LA subset
```

## Reproducibility

```bash
git clone https://github.com/sora-urban/air-as-class.git
cd air-as-class
pip install pandas matplotlib statsmodels folium openpyxl
jupyter lab air_as_class.ipynb
```

All cells run end-to-end from the included Excel file. No API keys required.

## References

- Bullard, R. D. (1990). *Dumping in Dixie: Race, Class, and Environmental Quality.* Westview Press.
- D'Ignazio, C. & Klein, L. (2020). *Data Feminism.* MIT Press.
- OEHHA (2021). *CalEnviroScreen 4.0.* California Office of Environmental Health Hazard Assessment.
- Williams, S. (2020). *Data Action: Using Data for Public Good.* MIT Press.

## Author

**Sora Kim** — independent researcher, Seoul.
Part of an ongoing research program on intersectional spatial inequality across cities.
[sorakim-lab.github.io](https://sorakim-lab.github.io) · [ORCID 0009-0003-0856-4193](https://orcid.org/0009-0003-0856-4193)

## License

Code: MIT License.
Data: California public records, redistributed under OEHHA terms.
