# Surakshith-Rail: Risk-Driven Railway Maintenance

**Surakshith-Rail** is a predictive analytics platform designed to shift India's railway maintenance from a traditional schedule-driven model to a proactive, **risk-driven** approach. By integrating historical operational data, weather patterns, and accident records, the system identifies hazardous track sections and assigns a risk score to ensure critical areas are inspected before accidents occur.

---

##  Elevator Pitch
India manages over 68,000 kilometers of railway track where maintenance is currently schedule-driven rather than risk-driven. Approximately one-quarter of derailments in India are attributed to inadequate track maintenance. **Surakshith-Rail** synthesizes existing delay records, accident history, and weather patterns to identify sections becoming hazardous, ensuring inspections happen before the next monsoon rather than after the next accident.

---

##  Architecture & Data Stack
The project is built on **Databricks** using **PySpark** and **Delta Lake** for robust data handling and versioning.

### Data Sources
| Table | Source | Description |
| :--- | :--- | :--- |
| `running_history` | Railways CSV | Historical train running data including `average_delay_minutes` and `pct_significant_delay`. |
| `stations_cleaned` | DataMeet GitHub | Flattened JSON data mapping station codes to names, states, and Zonal Railways. |
| `weather_history` | Open-Meteo API | Daily precipitation and max temperature data (July–Sept 2025) for 10 major stations. |
| `incidents_by_zone` | Official CSVs | Historical records of consequential train collisions and accidents on account of Signal Passing at Danger (SPAD). |

---

##  Risk Scoring Model
The system employs a **Weighted Scoring Model** to quantify the risk level of each Zonal Railway.

### The Formula
The **Risk Score** is calculated using three normalized features:
$$\text{Risk Score} = (0.4 \times \text{Delay Spike}) + (0.3 \times \text{Weather Correlation}) + (0.3 \times \text{Incident Proximity})$$


* **Delay Spike Frequency (40%):** Calculated from the average percentage of significant delays per zone.
* **Weather Correlation (30%):** Derived from average precipitation levels per zone, normalized to a 0–1 scale.
* **Incident Proximity (30%):** Based on the normalized count of real historical consequential accidents per zone.

### Risk Classification
* 🔴 **High Risk:** Score >= 0.65
* 🟡 **Medium Risk:** Score in between 0.45 – 0.64
* 🟢 **Low Risk:** Score < 0.45

---

##  Model Performance & Validation
The model's reliability was validated through **back-testing** against real-world historical data:
* **High-Risk Zones Identified:** Eastern Railway and South East Central Railway.
* **Confirmation:** Both identified zones were confirmed to have the highest actual incident counts (4 real incidents each).
* **Back-Test Accuracy:** **100%** in identifying zones where accidents have historically occurred.

---

##  Technical Features
* **Delta Lake Time Travel:** Uses `DESCRIBE HISTORY` to track how risk scores and data versions evolve over time.
* **MLflow Integration:** Tracks model parameters (weights), metrics (accuracy), and logs final scored runs for reproducibility.
* **Data Cleaning:** Standardizes station names to uppercase and strips hidden characters to ensure successful joins between disparate datasets.
* **API Ingestion:** Automated fetching of historical weather data using geographic coordinates (latitude/longitude) for specific railway hubs.

---

##  Setup & Run
1.  **Data Ingestion:** Run `01-data-ingestion.ipynb` to process the stations JSON and delay CSVs into Delta tables.
2.  **Weather Fetching:** Execute `ingest_weather_history.ipynb` to pull historical monsoon data from the Open-Meteo API.
3.  **Risk Modeling:** Run `mlflow-real.ipynb` to compute normalized features, apply the weighted scoring model, and log results to MLflow.