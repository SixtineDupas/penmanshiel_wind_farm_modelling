# Notebooks

The notebooks in this folder contain the complete analysis workflow 

## Workflow

### `01_data_cleaning.ipynb` - Data preparation

Loads the raw Penmanshiel SCADA and turbine static-data files and prepares the datasets used throughout the analysis.

Main steps include:

- loading the raw turbine SCADA files for the dissertation study period;
- retaining the variables required for the analysis;
- identifying and summarising data-quality issues;
- removing invalid turbine observations;
- retaining only timestamps with valid observations from all 14 turbines;
- preparing the turbine coordinates and static turbine information;
- saving cleaned and processed datasets for use by subsequent notebooks.

Key outputs include:

- `penmanshiel_scada_clean.parquet`
- `penmanshiel_scada_complete.parquet`
- `penmanshiel_static.parquet`
- `penmanshiel_data_quality_summary.csv`

These processed files form the main input to the remaining analysis.

### `02_eda.ipynb` - Exploratory data analysis

Performs the exploratory analysis

The notebook examines both farm-level and turbine-level behaviour, including:

- turbine layout;
- wind-direction distribution;
- nacelle wind-speed distribution;
- farm power as a function of wind speed;
- directional variation in farm power;
- mean turbine contributions to total farm power;
- variation in turbine power contributions through time;
- turbine power-curve behaviour;
- directional variation in relative turbine wind speed.

The notebook generates Figures 3.1–3.9 and several supporting summary tables.

### `03_curtailment_detection.ipynb` - Curtailment-like operation

Identifies observations displaying behaviour consistent with power limitation or curtailment.

The notebook:

- applies the curtailment-detection procedure to the cleaned turbine-level SCADA data;
- creates turbine-level curtailment flags;
- identifies farm-level timestamps affected by flagged operation;
- saves the timestamps required for the later curtailment sensitivity analysis;
- generates the example curtailment-detection figure used in the dissertation.

Key outputs include:

- `penmanshiel_curtailment_flags.parquet`
- `penmanshiel_excluded_curtailment_timestamps.parquet`
- `penmanshiel_scada_complete_with_curtailment_flags.parquet`

### `04_models_ABC.ipynb` - Model definitions

Defines the common modelling framework and the three neural-network approaches compared in the study.

The notebook:

- constructs the common farm-level modelling dataset;
- applies the chronological training, validation and test split;
- calculates standardisation parameters from the training period only;
- defines the feedforward neural-network architecture;
- implements training with early stopping;
- constructs the training-derived empirical turbine power curves used by Model B;
- defines the common performance metrics.

The three models are:

- **Model A:** direct prediction of total wind-farm power;
- **Model B:** prediction of individual turbine wind speeds followed by turbine-specific empirical power-curve conversion and aggregation;
- **Model C:** direct prediction of individual turbine powers followed by aggregation.

The notebook also contains the function used to train and evaluate all three models for an individual random seed.


### `05_run_final_models.ipynb` - Final model experiment

Runs the final comparison of Models A, B and C.

The models are evaluated across five random seeds:

`1, 21, 42, 84, 123`

The notebook:

- loads the common model definitions from `04_models_ABC.ipynb`;
- loads the cleaned modelling dataset;
- verifies the chronological train/validation/test split;
- trains Models A, B and C for each seed;
- evaluates performance on the independent 2022 test period;
- calculates the regression baseline and measured-speed reference;
- saves farm-level and turbine-level predictions;
- saves the training and validation histories.

Important outputs include:

- `seed_summary.csv`
- `reference_metrics.csv`
- `predictions_seed_<seed>.parquet`
- `loss_history_seed_<seed>.csv`
- `mean_std_summary.csv`

### `06_capacity_testing.ipynb` - Network-capacity and dropout testing

The notebook:

- evaluates alternative hidden-layer sizes for Models A, B and C;
- compares training and validation errors across architectures;
- tests dropout on the selected full-dataset architecture;
- performs an additional positive-control experiment using a much smaller training sample to verify that dropout reduces validation error when substantial overfitting is deliberately introduced.

Key outputs include:

- `capacity_results.csv`
- `dropout_full_dataset.csv`
- `positive_control_dropout.csv`

### `07_curtailment_sensitivity.ipynb` - Curtailment sensitivity analysis

The notebook compares two training conditions:

- flagged observations retained;
- flagged observations excluded.

Both conditions are evaluated across the same five random seeds using the common modelling framework.

The analysis is performed using validation performance so that the independent test period remains untouched during model-development decisions.

Key outputs include:

- `curtailment_sensitivity_seed_results.csv`
- `curtailment_sensitivity_summary.csv`
- `curtailment_sensitivity_comparison.csv`

### `08_wake_analysis.ipynb` - Wake-related analysis

The notebook combines turbine geometry, observed SCADA measurements and final model predictions.

Main stages include:

- constructing direction-dependent pairwise turbine geometry;
- identifying Jensen-defined upstream/downstream wake interactions;
- assigning turbine observations to low, medium and high wake-exposure classes;
- comparing Model B wind-speed error and Model C turbine-power error across wake classes and wind-speed bands;
- constructing a Jensen-defined non-waked wind-speed reference;
- calculating directional relative farm power;
- comparing observed and predicted directional farm-power behaviour;
- calculating directional farm-power residuals;
- defining farm-level wake severity;
- comparing errors between high-wake and non-high-wake operating conditions.

Key outputs include:

- `jensen_wake_pairs.parquet`
- `test_turbine_wake_classes.parquet`
- `model_B_wake_error_summary.csv`
- `model_C_wake_error_summary.csv`
- `directional_relative_power_summary.csv`
- `directional_residuals_summary.csv`
- `directional_jensen_severity.csv`
- `high_vs_non_high_wake_summary.csv`

### `09_results_figures.ipynb` — Final results and figures

Uses the saved outputs from the preceding notebooks to generate the final results tables and figures presented in Chapter 5.

This includes:

- farm-level model-performance metrics;
- observed and predicted farm-power distributions;
- predicted versus observed farm-power variability;
- network-capacity results;
- training and validation histories;
- per-turbine predictive performance;
- Model B error decomposition;
- power-curve sensitivity to wind-speed error;
- turbine-to-farm error cancellation;
- wake-exposure error;
- directional relative farm power;
- directional model residuals;
- high-wake versus non-high-wake model comparisons.

The notebook generates Figures 5.1–5.9 and the main results tables used in the dissertation.

Final table outputs are saved to:

`results/final_tables/`

## Recommended running order

Run the notebooks in numerical order
