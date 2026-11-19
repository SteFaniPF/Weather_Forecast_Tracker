# # Weather Forecast Tracker 🌦️

This project is an automated weather data integration pipeline. It reads user-submitted geographical coordinates (Place Name, Latitude, and Longitude) collected via a custom **GitHub Pages** frontend, retrieves real-time temperature data from the **Open-Meteo API**, and outputs localized weather summaries.

## 🚀 Project Architecture

1. **Data Collection (Frontend):** 
   - Hosted on **GitHub Pages**, users interact with an intuitive interface to input a location's name, latitude, and longitude.
   - Submissions are securely stored in a Google Sheet backend.
   - **Github Pages Link:** https://stefanipf.github.io/Weather_Forecast_Tracker/
   
2. **Data Processing (Backend Pipeline):**
   - This Python environment programmatically fetches the latest spreadsheet entries as a CSV dataset.
   - The script iterates through each registered location, triggering targeted API requests to the Open-Meteo endpoint to fetch current temperatures.
   - Generates real-time localized reports for each entry.

## 🛠️ Tech Stack & API Endpoints

- **Frontend Hosting:** GitHub Pages
- **Data Storage:** Google Sheets (accessed via CSV export endpoint)
- **Weather Provider:** [Open-Meteo API](https://open-meteo.com/)
- **Libraries Used:** 
  - `pandas` (Data manipulation and ingestion)
  - `requests` (API communication)
  - `time` (Rate-limiting and delay handling)

## 📖 How to Run

1. Ensure your Google Sheet sharing link is active and properly structured in `endpoint_planilha`.
2. Run the initialization cells to load dependencies and verify connectivity.
3. Run the iterator cell to fetch, process, and display live temperature metrics for all submitted locations.
