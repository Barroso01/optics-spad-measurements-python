# Correlation Measurements Code Repository
This repository contains the measurement plans, Jupyter notebooks, and data analysis pipelines for single-photon counting and correlation measurements.

Hardware interfacing and data acquisition rely on the Swabian Time Tagger API. The measurement logic heavily utilizes the Correlation (for auto- and cross-correlation functions) and Counter (for count rates and histograms) classes. For detailed implementation mechanics regarding these functions, please refer to the official [Swabian Instruments documentation](https://www.swabianinstruments.com/static/documentation/TimeTagger/).

## Directory Structure
To maintain a clean and scalable environment, the repository separates executable logic from heavy datasets:

```text
photon-measurements/
├── .gitignore             # Excludes the data/raw/ directory to prevent Git bloat
├── README.md              # Project mapping and documentation
├── data/
│   └── raw/               # Local storage for all acquired .npy and .npz datasets
└── notebooks/             # Jupyter notebooks for measurement plans and analysis
```

## Workflow & Usage
Each Jupyter notebook is structured into two distinct operational phases:

Measurement Code (First Sections): This block contains the live code used to configure the time tagger, interface with the hardware, and acquire the raw data.

Data Analysis (Bottom Sections): This is a dedicated, offline block where the acquired data is called, processed, and visualized. You do not need to be connected to the hardware to run this section. Users can manually modify the I/O paths here to import, visualize, and analyze any previously recorded dataset of their choosing.

Jupyter notebooks and their respective datasets are mapped bellow: 

1. **`deadtime_afterpulsing (.ipynb and data)`**
    - `autocorr_spad1_counts_note.npy`: Autocorrelation (``Correlation.getData``) data. `note` refers to the SPAD detector filters or covers used. 
    - `autocorr_time_ps.npy` saved in the corresponding lag time axis in picoseconds (`.getIndex`).

2. **`photocount_statistics (.ipynb and data)`** 
    - `photon_counts_spad#_laser###nm_Power#_Opticalfilter.npy`: Count histogram in a specified time window (``Counter.getData``). `Power#` refers to the power setting of the laser S1FC660 (e.g.05_00 corresponds to 5 mW). 

3. **`correlation_2ch_zerodelay (.ipynb and data)`**
    - `pulsed###nm_#MHz_#Power`: Contains calibration, g^2(0) and g^2(t) measurement data for pulsed sources with  specified frequency.
        + `calib_Ch#_Ch#_pulsed###nm_##Hz_res=##ps_run#`: ``Correlation.getData`` and  ``.getIndex`` arrays together with a lag value of 0 where the peak is located. 
        + `uncalib_Ch#_Ch#_pulsed###nm_##Hz_res=##ps_run#`: ``Correlation.getData`` and  ``.getIndex`` arrays together with a lag value where the peak is located. 
        + `g2_meas_pulsed###nm_##Hz_jitter=###ps_run#`: Contains the value of the g^2(0) measurement together with raw_counts, raw_coincidences, events_a, events_b, total_pulses, coincidences, p_a, p_b, p_ab, g2_0, coinc_window_ps. This forms the results of measurement plan 1. 

4. **`correlation_2ch (.ipynb and data)`**
    - `g2_tau_measurement_Pulsed_##MHz_###cps_bin=####ps.npz`: ``Correlation.getData`` and  ``.getIndex`` arrays. 
    - `data_coincidence_meas_run1_ThermalSources_g2`: g^2(t) measurement data for thermal sources: Lab Lamp and Thermal Lamp
        + `g2_tau_measurement_Source_##MHz_###cps_bin=####ps`: ``Correlation.getData`` and  ``.getIndex`` arrays. 

5. **`decorrelation_11ch (.ipynb and data)`**
    - `spad_line_data_run#`
        + `g2_ch#_ch#_filter-wavelen-source-velocity`: Series of correlation measurements. Counts (.getData) and lags (.getIndex) are bundled inside the zip file (e.g. results[pair]['time']).

6. **`correlation_15ch (.ipynb and data)`**  (check)
    -  `data_cw_correlations_run#`
        + `autocorr_raw_ch#.npy`: Autocorrelation (``Correlation.getData``) data for channels 1-15. **`autocorr_lags.npy`** is the corresponding lag time axis in picoseconds (`.getIndex`).
        + `g2_raw_ch#_ch#`: Correlation between channels (``Correlation.getData``). **`g2_lags`** its lag time axis in picoseconds. 
        + `photon_counts_ch#`: Histogram of the number of detections that arrive in a time window.


7. `live_cps_counter.ipynb`
    - No data saved.

8. `jitter.ipynb`
    - No data saved.

## Related Repositories
Related Repositories
The theoretical background, detailed methodology, and comprehensive discussion of these experimental results are maintained in a separate repository.

LaTeX Report: [optics-spad-measurements](https://github.com/Barroso01/optics-spad-measurements)