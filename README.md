# Correlation Measurements Code Repository
This repository contains the measurement plans, Jupyter notebooks, and data analysis pipelines for single-photon counting and correlation measurements.

Hardware connections and data acquisition use the Swabian Time Tagger API. In particular, the measurement logic utilizes the `Correlation` (for cross-correlation functions), `Countrates` and `Counter` classes. More information about these functions is found on the [Swabian Instruments documentation](https://www.swabianinstruments.com/static/documentation/TimeTagger/).

## Dependencies & Installation

To run the data analysis sections of these notebooks, you will need a standard Python scientific stack. 

1. **Standard Packages:** Install the required Python libraries using the provided requirements file:
   ```bash
   pip install -r requirements.txt

2. **Hardware API (Swabian Time Tagger):** The live measurement code requires the proprietary Time Tagger library, which must be downloaded directly from the [Swabian Instruments website](https://www.swabianinstruments.com/static/documentation/TimeTagger/). To set it up, install the core software and USB drivers, run `pip install Swabian-TimeTagger` in your environment, and connect the hardware to automatically fetch the license and begin taking measurements.


## Directory Structure
To maintain a clean and scalable environment, the repository separates executable logic from datasets. 
The logic that carries out the measurements and data analysis are in a series of jupyter notebooks. 
Each notebook is mapped to a dedicated dataset directory within `data/raw/` sharing the same name.

```text
photon-measurements/
├── .gitignore             # Excludes the data/raw/ directory to prevent Git bloat
├── README.md              # Project mapping and documentation
├── data/
│   └── raw/               # Local storage for all acquired .npy and .npz datasets
└── notebooks/             # Jupyter notebooks for measurements and analysis
```

## Workflow & Usage
Each Jupyter notebook is structured into two distinct operational phases:

Measurement Code (First Sections): This block contains the live code used to configure the time tagger, interface with the hardware, and acquire the raw data.

Data Analysis (Bottom Sections): This is a dedicated, offline block where the acquired data is called, processed, and visualized. You do not need to be connected to the hardware to run this section. Users can manually modify the I/O paths here to import, visualize, and analyze any previously recorded dataset of their choosing.

Jupyter notebooks and their respective datasets are mapped bellow: 

1. **`deadtime_afterpulsing.ipynb`**: A notebook to measure deadtime and afterpulsing based on correlation measurements.  
    - `autocorr_spad1_counts_note.npy (data)`: Autocorrelation (``Correlation.getData``) data. `note` refers to the SPAD detector filters or covers used. 
    - `autocorr_time_ps.npy` saved in the corresponding lag time axis in picoseconds (`.getIndex`).

2. **`photocount_statistics .ipynb`** : A notebook to measure the poissonian statistics of coherent light. 
    - `photon_counts_spad#_laser###nm_Power#_Opticalfilter.npy (data)`: Count histogram in a specified time window (``Counter.getData``). `Power#` refers to the power setting of the laser S1FC660 (e.g.05_00 corresponds to 5 mW). 

3. **`correlation_2ch_zerodelay.ipynb`**: A notebook to measure 2 channel coincidences and calculates the g^2(0) function.
    - `pulsed###nm_#MHz_#Power (data)`: Contains calibration, g^2(0) and g^2(t) measurement data for pulsed sources with  specified frequency.
        + `calib_Ch#_Ch#_pulsed###nm_##Hz_res=##ps_run# (data)`: ``Correlation.getData`` and  ``.getIndex`` arrays together with a lag value of 0 where the peak is located. 
        + `uncalib_Ch#_Ch#_pulsed###nm_##Hz_res=##ps_run# (data)`: ``Correlation.getData`` and  ``.getIndex`` arrays together with a lag value where the peak is located. 
        + `g2_meas_pulsed###nm_##Hz_jitter=###ps_run# (data)`: Contains the value of the g^2(0) measurement together with raw_counts, raw_coincidences, events_a, events_b, total_pulses, coincidences, p_a, p_b, p_ab, g2_0, coinc_window_ps. This forms the results of measurement plan 1. 

4. **`correlation_2ch.ipynb`**: A notebook to measure 2 channel coincidences and calculates the g^2(t) function. 
    - `g2_tau_measurement_Pulsed_##MHz_###cps_bin=####ps.npz`: ``Correlation.getData`` and  ``.getIndex`` arrays. 
    - `data_coincidence_meas_run1_ThermalSources_g2`: g^2(t) measurement data for thermal sources: Lab Lamp and Thermal Lamp
        + `g2_tau_measurement_Source_##MHz_###cps_bin=####ps`: ``Correlation.getData`` and  ``.getIndex`` arrays. 

5. **`decorrelation_11ch.ipynb`**: A notebook to measure decorrelation times of a speckle field with one time tagger. 
    - `spad_line_data_run#`
        + `g2_ch#_ch#_filter-wavelen-source-velocity`: Series of correlation measurements. Counts (.getData) and lags (.getIndex) are bundled inside the zip file (e.g. results[pair]['time']).

6. **`decorrelation_24ch.ipynb`**: A notebook to measure decorrelation times of a speckle field with two time taggers.
    -  `spad_line_data_run#`
        + `g2_ch#_ch#_countrate-specklesize-wavelength-velocity`: Series of correlation measurements. Counts (.getData) and lags (.getIndex) are bundled inside the zip file (['histogram'] ['time']).

7. **`decorrelation.ipynb`**: A notebook that replicates the results of this [paper]{https://opg.optica.org/boe/fulltext.cfm?uri=BOE-14-2-703}.
    - `raw_autocorr_20260807_200133`: 3D array of periods, channels and bins. The ['tau_axis'] label contains the one-dimensional array of time delay values. The ['raw_hists'] label contains the three-dimensional array of unnormalized photon counts for each measurement period, pixel channel, and time bin.

7. `live_cps_counter.ipynb`
    - No data saved.

8. `jitter.ipynb`
    - No data saved.

## Related Repositories
Related Repositories
The theoretical background, detailed methodology, and comprehensive discussion of these experimental results are maintained in a separate repository.

LaTeX Report: [optics-spad-measurements](https://github.com/Barroso01/optics-spad-measurements)