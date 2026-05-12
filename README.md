# Air as Class

A data essay on spatial inequality, pollution, and health in Los Angeles County.

> The conventional story says poor neighborhoods breathe dirtier air.
> The data from LA tells a stranger one — about who gets pushed where,
> and what they inherit when they get there.

---

## Summary

Environmental justice frameworks predict a positive correlation between poverty and air pollution: polluting infrastructure is sited near poor and minority neighborhoods, so the people who live there breathe worse air. This study analyzes 2,295 census tracts in Los Angeles County, drawing on CalEnviroScreen 4.0 data, and finds that the relationship does not hold cleanly within LA. Poverty correlates with asthma (r = 0.53) but barely with PM2.5 (r = 0.07). The mechanism is not air. It is displacement: low-income residents pushed beyond urban boundaries inherit cleaner air and worse lungs.

## Key finding

A multiple regression of asthma on poverty and PM2.5, both standardized:

| Predictor | Coefficient | Direction |
|-----------|-------------|-----------|
| PM2.5     | −0.168      | (negative) |
| Poverty   | +0.542      | (positive) |

**Poverty predicts asthma 3.2× more strongly than PM2.5 does.** PM2.5 carries a small negative coefficient because the tracts with the cleanest air in LA — Lancaster, Palmdale, and other outer-county areas — also carry the heaviest asthma burdens.

## Three populations

| | Tracts | Poverty | PM2.5 | Asthma ER |
|---|---|---|---|---|
| Rich Inner | 300 | 10.4% | 11.6 μg/m³ | 29.9 |
| Poor Inner | 525 | 60.8% | 12.0 μg/m³ | 70.4 |
| **Poor Outer** | **30** | **60.2%** | **7.8 μg/m³** | **121.7** |

The cleanest air in LA is breathed by some of its sickest residents.

## Data

[CalEnviroScreen 4.0](https://oehha.ca.gov/calenviroscreen/report/calenviroscreen-40) — California Office of Environmental Health Hazard Assessment. Census-tract-level environmental and demographic indicators for the entire state. This analysis uses LA County (n = 2,295 after dropping rows with missing values).

## Method

Python: `pandas`, `matplotlib`, `folium`, `statsmodels`. Notebook reproducible end-to-end from the public data file.

- `air_as_class.ipynb` — full analysis
- `data/` — input data (CalEnviroScreen Excel)
- `figures/` — output figures (PNG + SVG)

## Author

Sora Kim — independent researcher, Seoul.
Part of an ongoing research program on intersectional spatial inequality in cities.
[sorakim-lab.github.io](https://sorakim-lab.github.io)

## License

MIT. Data licensed under California public records law.
