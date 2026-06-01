# 🚖 Mumbai Smart Transport Data Platform

A real-world style Data Engineering project built on Mumbai ride data.
Imagine you work at a company like Ola or Uber — your job is to collect ride data, clean it, analyze it, and make it useful for the business. That is exactly what this project does — starting simple and growing into a full production-style data pipeline.

---

## 📁 Project Structure

```
Data_Engineering/
├── data/
│   ├── drivers.csv           # 10,000 drivers
│   ├── customer.csv          # 50,000 customers
│   └── rides_data.csv        # 100,000 rides
├── notebooks/                # Jupyter notebooks
├── scripts/                  # ETL pipeline scripts
├── output/                   # Processed and cleaned data
├── logs/                     # Pipeline logs
├── venv/                     # Virtual environment
├── requirements.txt
└── README.md
```

---

## 🗺️ Project Phases

| Phase | Topic | Status |
|---|---|---|
| 1 | Environment setup, project structure, load dataset | ✅ Done |
| 2 | Basic analytics + Data cleaning with Pandas | ✅ Done |
| 3 | Scale to 1 lakh rows — generate drivers, customers, rides datasets | ✅ Done |
| 4 | PostgreSQL + SQLAlchemy — load data into database | 🔄 Up Next |
| 5 | API ingestion — fetch live data | ⏳ Upcoming |
| 6 | Apache Spark — big data processing | ⏳ Upcoming |
| 7 | AWS S3 + RDS — cloud storage and warehouse | ⏳ Upcoming |
| 8 | Airflow — automate the pipeline | ⏳ Upcoming |
| 9 | Kafka — real-time data streaming | ⏳ Upcoming |

---

## 📊 Datasets

### 1. `drivers.csv` — 10,000 rows
| Column | Description |
|---|---|
| `driver_id` | Unique ID e.g. `DRV00001` |
| `name` | Faker-generated Indian name |
| `phone` | Indian format phone number |
| `car_type` | Hatchback / Sedan / SUV |
| `rating` | Float between 3.0 – 5.0 |
| `city` | Mumbai |

### 2. `customer.csv` — 50,000 rows
| Column | Description |
|---|---|
| `customer_id` | Unique ID e.g. `CUS00001` |
| `name` | Faker-generated Indian name |
| `phone` | Indian format phone number |
| `email` | Faker-generated email |
| `joined_date` | Random date in past 3 years |

### 3. `rides_data.csv` — 100,000 rows
| Column | Description |
|---|---|
| `driver_id` | Foreign key → drivers |
| `customer_id` | Foreign key → customers |
| `car_type` | Pulled from driver's record |
| `pick_up_location` | Random Mumbai area |
| `drop_location` | Always different from pickup |
| `pickup_time` | Random datetime in past 1 year |
| `drop_time` | pickup + 20–90 mins |
| `total_duration` | Duration in minutes (integer) |
| `distance` | Random 1–30 km |
| `rate_per_km` | Based on car type |
| `fare` | `base_fare + rate_per_km × distance` |
| `ride_status` | completed (80%) / cancelled (20%) |

---

## 💰 Fare Logic

```python
fare_rates = {
    'Hatchback': 11,
    'Sedan': 15,
    'SUV': 19
}
base_fare = 50

total_fare = round(base_fare + fare_rates[car_type] * distance_km, 2)
```

---

## 🧠 Concepts Practiced

| Concept | Where Used |
|---|---|
| Variables & Data Types | All datasets |
| Lists & Dictionaries | `fare_rates`, `mumbai_areas`, driver/customer records |
| `random` module | IDs, locations, fares, status |
| `faker` library | Names, phones, emails, datetimes |
| Functions | `ride_generator()` |
| `while` loop | Ensuring unique pickup ≠ drop location |
| `timedelta` | Calculating drop time and duration |
| `os.path.exists` | Avoiding duplicate data generation |
| Pandas `DataFrame` | Structuring and saving to CSV |
| Data cleaning | `.dropna()`, `.fillna()`, IQR outlier removal, `.drop_duplicates()` |
| Table relationships | `driver_id`, `customer_id` as foreign keys |

---

## 🗺️ Mumbai Areas Used

```python
mumbai_areas = [
    'Andheri', 'Bandra', 'Borivali', 'Chembur', 'Dadar',
    'Ghatkopar', 'Juhu', 'Kurla', 'Malad', 'Mulund',
    'Powai', 'Santacruz', 'Vile Parle', 'Worli'
]
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| Python 3.11 | Core language |
| Pandas | Data processing and cleaning |
| NumPy | Numerical operations |
| Faker (`en_IN`) | Realistic Indian data generation |
| Matplotlib / Seaborn | Visualization |
| SQLAlchemy | Database connection |
| PostgreSQL | Data warehouse |
| Apache Spark | Big data processing |
| AWS S3 + RDS | Cloud storage and database |
| Airflow | Pipeline automation |
| Kafka | Real-time streaming |
| Jupyter Notebook | Analysis and exploration |

---

## ⚙️ Setup

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

## 📅 Progress Log

| Week | What Was Built |
|---|---|
| Week 1 | Environment setup, project structure, 10-row dataset, basic analytics, data cleaning |
| Week 2 | Scaled to 1 lakh rows — drivers, customers, rides datasets with fare logic, foreign keys, unique locations |

---

> Built as a learning project to understand Data Engineering end to end —
> from raw CSV files to automated cloud pipelines.