# Mumbai Smart Transport Data Platform

A real-world style Data Engineering project built on Mumbai ride data.
The goal is to learn Data Engineering by actually building something — not doing random exercises.

---

## What is this project?

Imagine you work at a company like Ola or Uber.
Your job is to collect ride data, clean it, analyze it, and make it useful for the business.

That is exactly what this project does — starting simple and growing into a full production-style data pipeline.

---

## What are we building?

A system that:
- Collects raw ride data (driver, source, destination, fare, status)
- Cleans and processes it
- Calculates business metrics like revenue, cancellations, average fare
- Stores it in a database
- Automates it and streams it in real time

---

## Project Phases

| Phase | Topic | Status |
|-------|-------|--------|
| 1 | Environment setup, project structure, load dataset | ✅ Done |
| 2 | Basic analytics + Data cleaning with Pandas | ✅ Done |
| 3 | Scale to 1 lakh rows + PostgreSQL + SQLAlchemy | 🔄 In Progress |
| 4 | API ingestion — fetch live data | ⏳ Upcoming |
| 5 | Apache Spark — big data processing | ⏳ Upcoming |
| 6 | AWS S3 + RDS — cloud storage and warehouse | ⏳ Upcoming |
| 7 | Airflow — automate the pipeline | ⏳ Upcoming |
| 8 | Kafka — real-time data streaming | ⏳ Upcoming |

---

## What we have built so far

**Phase 1 — Setup**
- Python 3.11 virtual environment configured
- All dependencies installed and saved to requirements.txt
- Project folder structure created
- Dataset loaded and verified

**Phase 2 — Analytics + Data Cleaning**
- Loaded `rides.csv` using Pandas
- Calculated total rides, completed rides, cancelled rides
- Calculated total revenue, average fare, platform profit
- Simulated real world data problems — missing values, duplicates, outliers, inconsistent values
- Cleaned data using:
  - `.str.title().str.strip()` — fixed inconsistent ride_status
  - `.dropna()` and `.fillna(median)` — handled missing values
  - IQR method — detected and replaced outliers
  - `.drop_duplicates()` — removed duplicate rows
- Saved cleaned data to `output/cleaned_rides.csv`
- Analytics output:
```
=== Mumbai Ride Analytics ===
Total rides: 10
Completed rides: 8
Cancelled rides: 2
Total revenue: ₹4430
Average fare: ₹525.0
Platform profit: ₹443.0
```

---

## Dataset

**rides.csv** — 10 Mumbai ride records

| Column | Description |
|--------|-------------|
| ride_id | Unique ride number |
| driver_name | Driver's name |
| source | Pickup location |
| destination | Drop location |
| distance_km | Distance in kilometers |
| fare | Ride fare in rupees |
| ride_status | Completed or Cancelled |

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| Python 3.11 | Core language |
| Pandas | Data processing and cleaning |
| NumPy | Numerical operations |
| Matplotlib / Seaborn | Visualization |
| SQLAlchemy | Database connection |
| PostgreSQL | Data warehouse |
| Apache Spark | Big data processing |
| AWS S3 + RDS | Cloud storage and database |
| Airflow | Pipeline automation |
| Kafka | Real-time streaming |
| Jupyter Notebook | Analysis and exploration |

---

## Project Structure

```
Data_Engineering/
│
├── data/               # Raw input files
├── scripts/            # ETL pipeline scripts
├── notebooks/          # Jupyter notebooks
├── output/             # Processed and cleaned data
├── logs/               # Pipeline logs
├── venv/               # Virtual environment
├── requirements.txt    # Project dependencies
└── README.md
```

---

## Setup

```bash
# Step 1 - Create virtual environment
C:\Users\Admin\AppData\Local\Programs\Python\Python311\python.exe -m venv venv

# Step 2 - Activate
.\venv\Scripts\Activate

# Step 3 - Install dependencies
pip install -r requirements.txt

# Step 4 - Register Jupyter kernel
python -m ipykernel install --user --name=dataengineering --display-name "Python (DataEngineering)"
```

---

> Built as a learning project to understand Data Engineering end to end —
> from raw CSV files to automated cloud pipelines.