# Clash Royale Analytics System
 
**Date:** December 2025  
**Status:** Completed (Phases 1-4)

## Project Overview
An end-to-end data mining pipeline designed to extract, process, and analyze Clash Royale game data. The system ingests data from the official API, normalizes it into a relational database, performs in-database SQL preprocessing, and applies machine learning to predict battle outcomes.

## Project Phases

### Phase 1: API Data Extraction
- **Source:** Official Clash Royale API.
- **Targets:** Players, Battle Logs, Cards, Clans.

### Phase 2: Database Ingestion
- **Tech Stack:** Python, `requests`, `SQLModel`, PostgreSQL/SQLite.
- **Outcome:** Raw JSON data is upserted into a 3NF normalized database schema.

### Phase 3: Data Preprocessing (SQL-Based)
- **Method:** All cleaning (NULL handling, text standardization) is performed *inside* the database using SQL.
- **Automation:** Stored procedures (`clean_staging_data`) transform raw data into analytics-ready views.

### Phase 4: Analysis & Machine Learning
- **EDA:** Correlation heatmaps and distribution plots to understand game meta.
- **ML Model:** Random Forest Classifier to predict `Win`/`Loss` based on deck elixir cost and card rarity.
- **Results:** Achieved ~72% accuracy on test data.

## Repository Structure
- `phase4_analysis.ipynb`: Jupyter Notebook containing the full Analysis and ML code.
- `Phase4_Narrative_Report.md`: Detailed report of the findings and methodology.
- `Presentation_Outline.md`: Outline and speaker notes for the final project presentation.
- `requirements.txt`: Python dependencies.

## How to Run
1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Launch Jupyter Notebook:
   ```bash
   jupyter notebook phase4_analysis.ipynb
   ```
