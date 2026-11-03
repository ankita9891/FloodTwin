# FloodTwin

**Forecast-informed extreme-value relief-inventory allocation under flood risk**

This repository contains the code and outputs for the paper

> Gupta, A., Dutta Choudhury, K. *A forecast-informed extreme-value model for stochastic relief-inventory allocation under flood risk, with special reference to a real flood event.* Submitted to the *International Journal of Disaster Risk Reduction*, 2026.
> https://doi.org/10.5281/zenodo.23921953

The model decides how many household relief kits to pre-position in each warehouse for each district before a flood. Flood occurrence is forecast by a calibrated machine-learning classifier, flood severity is described by a common-shape Generalized Pareto distribution (GPD), and the two are combined in a convex bi-objective programme that balances preparation cost against expected shortage cost within a budget and warehouse capacities. The programme is solved exactly through a power-cone reformulation (CVXPY + Clarabel).

The case study is the June 2022 flood in the Barak Valley (Cachar, Karimganj and Hailakandi districts, Assam, India), with a proactive decision on 18 June and a reactive decision on 19 June 2022.

---

## Repository structure

```
FloodTwin/
├── FloodTwin_Decision_Model.ipynb   # the complete pipeline (data -> forecasts -> GPD -> optimisation -> dashboard)
├── requirements.txt                  # Python packages
├── data/
│   └── README.md                     # how to obtain the IMD input data (not redistributed here)
├── outputs/
│   ├── results/                      # final_results.json and the CSV tables of the paper
│   ├── figures/                      # figures of the paper (PNG)
│   ├── district_data/                # district daily rainfall and temperature series derived from the IMD grid
│   ├── solved_plans/                 # solved plans (lead times 1-5 and reactive), read by the dashboard
│   └── dashboard_exports/            # plans exported from the decision-support dashboard
├── CITATION.cff
├── LICENSE
└── README.md
```


---

## Data

The model uses the India Meteorological Department (IMD) daily gridded data for 1951-2025:

* rainfall, 0.25° × 0.25° (Pai et al., 2014);
* maximum and minimum temperature, 1° × 1° (Srivastava et al., 2009).

These data are distributed by IMD and are **not** included in this repository. See [`data/README.md`](data/README.md) for how to download them and where to place them.

All other inputs (census population, flood hazard atlas areas, warehouse capacities, road distances, kit composition and budget) are written in the `REGION` settings block of the notebook (Step 2), with their sources.

---

## How to run

### Google Colab (recommended)

1. Upload `FloodTwin_Decision_Model.ipynb` to Colab.
2. Place the IMD files in Google Drive in the folders `Rainfall/`, `Max_temp/` and `Min_temp/` inside `BASE_DIR` (default `MyDrive/ISRO_Digital_Twin`; edit `BASE_DIR` in Step 2 if needed). See `data/README.md`.
3. *Runtime → Run all.* Step 1 installs the packages, and Step 2 checks that all 225 raw files are present before anything else runs.

### Local Jupyter

```bash
pip install -r requirements.txt
jupyter notebook FloodTwin_Decision_Model.ipynb
```

Place the folders `Rainfall/`, `Max_temp/` and `Min_temp/` next to the notebook. Step 1 contains `!pip install` lines for Colab; they can be skipped if the packages are already installed.

### Run time

A full run (75 years, lead times 1-5 days, all sensitivity analyses) takes a few hours on Colab, mainly for classifier tuning (Step 6) and the bootstrap and sensitivity experiments. Setting `QUICK_TEST = True` in Step 2 runs the pipeline on a short period to check that everything works.

---

## Outputs

The notebook writes everything to `FloodTwin_outputs/` inside `BASE_DIR`. Step 10j copies the files kept in this repository into `FloodTwin_outputs/final_output/`. The `outputs/` folder of this repository contains the files of the run used for the paper:

* `outputs/results/final_results.json`: all numbers reported in the paper;
* `outputs/results/*.csv`: tables of the case study and of the sensitivity analyses (for example `retrospective_2022.csv` for the comparison with the June 2022 flood);
* `outputs/figures/`: reliability diagrams, GPD diagnostics, Pareto frontier, allocation by lead time, regime map and the tail-shape experiment;
* `outputs/district_data/`: district-average daily rainfall and temperature series computed from the IMD grid;
* `outputs/solved_plans/decision_results_*.pkl`: solved plans for each lead time and for the reactive decision, read by the dashboard.

`outputs/dashboard_exports/` contains the plans of the paper as exported from the dashboard:

| File | Content |
|---|---|
| `district_plan_L1.csv` … `district_plan_L5.csv` | Proactive plan of 18 June 2022 for lead times 1-5 days: probability, demand, certain hazard demand, stock and coverage per district |
| `district_plan_reactive.csv` | Reactive plan of 19 June 2022 |
| `allocation_by_warehouse_*.csv` | The same plans split by warehouse |
| `scenario_comparison.csv` | Preparation cost, expected shortage cost and total stock of every scenario |

The mapping between notebook steps and the sections, tables and figures of the paper is given in the first cell of the notebook.

---

## Applying the model to another region

All region-specific values are collected in the `REGION` block of Step 2. To apply the model elsewhere, edit only that block:

1. district names and bounding boxes on the rainfall grid;
2. population, household size and flood hazard map areas (exposure);
3. warehouses, capacities, road distances, unusable routes and freight rate;
4. relief-kit composition and prices;
5. annual relief fund and the population it serves (budget);
6. hazard threshold and decision dates.

Any gridded daily rainfall product with a long record (for example CHIRPS or ERA5) can replace the IMD grid by changing the reader in Step 3.

---

## Decision-support dashboard

Steps 11-13 build and launch a Streamlit dashboard (maps, Pareto frontier, what-if scenarios). To open it from Colab, an [ngrok](https://ngrok.com) authentication token is needed. **Never write the token in the notebook.** Store it as a Colab secret named `NGROK_AUTH_TOKEN`, or type it when prompted.

---

## Citation

If you use this code, please cite the paper above and this repository (see `CITATION.cff`).

## License

The code is released under the MIT License (see `LICENSE`). The IMD data are subject to the terms of the India Meteorological Department.

## Contact

Ankita Gupta, Department of Mathematics, Assam University, Silchar, India.
