#Surakshith-Rail

Elevator pitch
India has 68,000 kilometres of railway track. Maintenance is schedule driven, not risk-driven. To solve this problem, we developed Surakshith-Rail. 

What it does:
It reviews years of delay records, accident history, and weather patterns and identifies the sections that are gradually becoming hazardous. It assigns a risk score to each route segment, ensuring that the most critical areas are inspected before the next monsoon. Not following the next accident. A quarter of derailments in India can be attributed to inadequate track maintenance. The data needed to prevent them already exists. SafeTrack finally reads it.

##Architecture
### Weather_history (Delta table)
- **Source:** Open-Meteo Historical Weather API (free, no API key)
- **Endpoint:** `https://archive-api.open-meteo.com/v1/archive`
- **Coverage:** 10 stations, 2023-07-01 to 2023-09-30, daily granularity
- **Stations:** NEW DELHI, CHENNAI CENTRAL, HOWRAH JN, VIJAYAWADA JN, 
  KALYAN JN, PT DEEN DAYAL UPADHYAY JN, SALEM JN, AGRA CANTT, 
  ARAKKONAM, KATPADI JN
- **Schema:**
  - `station_name` — string, all caps
  - `latitude` — double
  - `longitude` — double
  - `date` — DateType (YYYY-MM-DD)
  - `precipitation_mm` — double
  - `temp_max_c` — double
- **Nulls:** [filled with 0.0]
- **Used for:** Joined to running_history on station_name + date 
  to build the weather_correlation feature

### Data Sources
| Table              | Source                        | 
|--------------------|-------------------------------|
| running_history    | Railways Running History CSV  | 
| stations           | datameet/railways GitHub      | 
| incidents          | Consequential Accidents CSVs  | 
| weather_history    | Open-Meteo Historical API     | 

##Run steps:
##Demo steps :
