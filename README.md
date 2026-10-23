# ElectroCatalyst AI

**Analyse LSV, EIS and stability data from raw instrument files in seconds.**

ElectroCatalyst AI is a Streamlit web app for electrocatalyst research. You upload raw measurement files; the app detects the technique, fits the parameters, shows how good each fit is, draws publication-quality figures and explains the result in plain language.

Built by **Team CatalystIQ** for the Open Scientific Intelligence (OSI) Hackathon 2026 (AI for Science).
Team: Rabia Noor · Farrukh Raza · Sachin Nenaniya

## Why

Water-splitting catalysts are judged by three measurements: LSV (activity), EIS (charge-transfer kinetics) and chronoamperometry (stability). Turning those files into numbers is usually manual. Fitting ranges, units and potential scales differ between labs, and fit quality is rarely shown, so the same file can give different answers. ElectroCatalyst AI makes the analysis fast, consistent and checkable.

## What it does

| Technique | What you get |
|---|---|
| **LSV** (HER / OER) | Onset potential, E and overpotential at 10 and 100 mA/cm², Tafel slope with R², points used and fit window, plus a Tafel Fit Inspector (points, fitted line, residuals) |
| **EIS** | Rs, Rct and capacitance from an RC circuit fit with RMSE; CPE fit, Bode plots and residuals; RC vs CPE comparison |
| **Chronoamperometry** | Initial and final current, retention, average current, duration (hours) |
| **Chronopotentiometry** | Applied current, potential drift (mV/h) |
| **Photoelectrochemistry** | Photocurrent (light minus dark), onset shift, ABPE, transient photocurrent, IPCE, dark vs light Rct |
| **Comparison dashboard** | Ranks samples only when their experimental conditions match |

Also included: automatic technique detection, a plain-language interpretation under every result, PNG/TIF figure export (150 to 600 DPI, custom colours) and results as CSV.

## Built not to give confident wrong numbers

- If the potential reference scale is unknown, **no overpotential is calculated**. Choose RHE or enter a verified offset. A header that says "vs RHE" is recognised automatically.
- A current-density column is **never divided by area a second time**. Area is used only when the current itself is in A or mA.
- If a fit fails, the app reports **nothing** instead of a substitute value.
- Weak Tafel fits are flagged, with R², number of points and the fitted current window. You can set the window yourself.
- EIS values are labelled as model dependent. The app prefers the simpler circuit when it describes the data equally well.
- The comparison dashboard refuses duplicate entries and does not rank samples measured under different conditions.

## Quick start

Requires Python 3.10 or newer.

**Mac / Linux**
```bash
git clone https://github.com/farrukh6785-source/Electrocatalyst_AI.git
cd Electrocatalyst_AI
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python -m streamlit run electrocatalyst_real_app.py
```

**Windows**
```bat
git clone https://github.com/farrukh6785-source/Electrocatalyst_AI.git
cd Electrocatalyst_AI
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
python -m streamlit run electrocatalyst_real_app.py
```
(or double-click `Run Real App.bat` after installing the requirements)

Then open http://localhost:8501.

## How to use

1. Upload one file per sample (CSV, TXT or XLSX; Excel files are read sheet by sheet). You can upload several techniques at once.
2. The app lists the detected technique for each file. You do not choose it.
3. Use the sidebar for the current unit, electrode area, potential reference scale and Tafel window.
4. Read the results table, plots and interpretation. Download figures and CSV tables.

## Accepted input files

Column names are detected by keyword. Plain CSV files, tab-separated instrument exports and Excel files are supported.

- **LSV:** potential + current (density, A or mA)
- **EIS:** frequency, Z′ (Zreal), Z″ (Zimag)
- **Chronoamperometry:** time + current. A time column labelled in seconds is shown in hours.
- **Chronopotentiometry:** time + potential + (constant) current
- **IPCE:** wavelength, photocurrent, incident power (see `ipce_example.csv`)

## Repository contents

| File | Purpose |
|---|---|
| `electrocatalyst_real_app.py` | The Streamlit application and all analysis code |
| `unit_conversions.py` | Current-density unit conversion helper |
| `test_units.py` | Quick check of the unit conversions |
| `ipce_example.csv` | Example input for the IPCE tool |
| `requirements.txt` | Python dependencies |
| `Run Real App.bat` | Windows launcher |
| `train_ml_models.py` | Optional: trains the ML prediction models from your own table |

## Machine-learning prediction (optional)

The app has an optional prediction tab. It needs a trained model file (`ml_bundle.pkl`) created with `train_ml_models.py`. **No model file and no training data are included in this repository.** Without the file, every other part of the app works as normal and the tab reports that prediction is not available.

## Testing and limitations

- The analysis was checked on synthetic data with known answers and on our own measurements. Checking against known answers exposed biases that clean data had hidden, such as Tafel slopes overestimated when a background current is present.
- The automatic Tafel window can be too wide for some curves. The app warns when R² is low. Narrow the window using the Tafel Fit Inspector.
- Fitted EIS parameters depend on the circuit model and are not direct measurements.
- Photocurrent from a three-electrode measurement is never reported as solar-to-hydrogen efficiency.

## Roadmap

iR correction, direct instrument-file upload (.DTA), more equivalent circuits (Warburg), and a next-experiment recommender.

## Acknowledgements

Built for the OSI Hackathon 2026, organised by the Open Scientific Intelligence community.
