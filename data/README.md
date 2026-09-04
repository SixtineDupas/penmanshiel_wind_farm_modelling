# Data

The raw Penmanshiel wind farm dataset is not included in this repository

The dataset is publicly available from Zenodo:

[Penmanshiel wind farm data – Zenodo](https://doi.org/10.5281/zenodo.16807304)

This study uses the 10-minute SCADA data for 14 turbines covering August 2016 to December 2022, together with turbine coordinates and technical information used for the wake analysis.

To reproduce the analysis:

1. Download the required Penmanshiel files from Zenodo.
2. Create a local folder called:

   `data/raw/`

3. Place the downloaded raw files inside this folder.
4. Run `01_data_cleaning.ipynb` first to generate the cleaned dataset used by the later notebooks.

The raw data are excluded from this GitHub repository to avoid duplicating the full Zenodo archive.
