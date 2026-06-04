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
├── venv/                     # Virtual environment (not tracked)
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
| 4 | PostgreSQL + SQLAlchemy — load data into database | ✅ Done |
| 5 | SQL queries + analytics via SQLAlchemy ORM | 🔄 In Progress |
| 6 | API ingestion — fetch live data | ⏳ Upcoming |
| 7 | Apache Spark — big data processing | ⏳ Upcoming |
| 8 | AWS S3 + RDS — cloud storage and warehouse | ⏳ Upcoming |
| 9 | Airflow — automate the pipeline | ⏳ Upcoming |
| 10 | Kafka — real-time data streaming | ⏳ Upcoming |

---

## 🗄️ Database — PostgreSQL

All three datasets are loaded into a local PostgreSQL database called `mumbai_rides`.

**Tables created:**

| Table | Rows | Description |
|---|---|---|
| `driver` | 10,000 | Driver profiles with vehicle and rating info |
| `customer` | 50,000 | Customer profiles with registration date |
| `rides` | 100,000 | Ride records linked to drivers and customers |

**How data flows:**

```
CSV files (data/)
      ↓
Python (pandas)
      ↓
SQLAlchemy ORM
      ↓
PostgreSQL (mumbai_rides database)
```

**Database PostgreSQL — rides table in pgAdmin:**

> 📸  ![Rides table - sample data](assets/pgadmin_rides_sample.png) 
— `SELECT * FROM public.rides ORDER BY ride_id ASC`  
> 100,000 ride records successfully loaded into PostgreSQL via SQLAlchemy.

---

## 📊 Datasets

### 1. `drivers.csv` — 10,000 rows
| Column | Description |
|---|---|
| `driver_id` | Unique ID e.g. `DRV00001` |
| `driver_name` | Faker-generated Indian name |
| `driver_phone` | Indian format phone number |
| `license_plate` | Unique MH-format plate e.g. `MH12 AB 1234` |
| `driver_vehicle_model` | e.g. Honda City, Tata Nexon |
| `car_type` | Hatchback / Sedan / SUV |
| `driver_rating` | Float between 3.5 – 5.0 |
| `driver_experience_years` | Integer between 1 – 20 |

### 2. `customer.csv` — 50,000 rows
| Column | Description |
|---|---|
| `customer_id` | Unique ID e.g. `CUS00001` |
| `customer_name` | Faker-generated Indian name |
| `customer_phone` | Indian format phone number |
| `customer_email` | Faker-generated email |
| `registration_date` | Random date between 2011 – present |

### 3. `rides_data.csv` — 100,000 rows
| Column | Description |
|---|---|
| `ride_id` | Unique ID e.g. `RID000001` |
| `driver_id` | Foreign key → driver table |
| `customer_id` | Foreign key → customer table |
| `car_type` | Pulled from driver's record |
| `pick_up_location` | Random Mumbai area |
| `drop_location` | Always different from pickup |
| `pickup_time` | Random datetime in past 1 year |
| `drop_time` | pickup + 20–90 mins |
| `total_duration` | Duration in minutes |
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
| Functions | `ride_generator()`, `data_generator()` |
| `while` loop | Ensuring unique pickup ≠ drop location |
| `timedelta` | Calculating drop time and duration |
| `os.path.exists` | Idempotency — avoiding duplicate data generation |
| Pandas `DataFrame` | Structuring and saving to CSV |
| Data cleaning | `.dropna()`, `.fillna()`, IQR outlier removal, `.drop_duplicates()` |
| Table relationships | `driver_id`, `customer_id` as foreign keys |
| SQLAlchemy ORM | Defining table classes, creating tables, inserting and querying data |
| PostgreSQL | Storing and querying 100,000+ rows in a relational database |
| Session & Engine | Connecting Python to PostgreSQL via SQLAlchemy |

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
| SQLAlchemy | ORM — Python to database connection |
| PostgreSQL | Relational database — stores all 3 tables |
| Apache Spark | Big data processing *(upcoming)* |
| AWS S3 + RDS | Cloud storage and database *(upcoming)* |
| Airflow | Pipeline automation *(upcoming)* |
| Kafka | Real-time streaming *(upcoming)* |
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

**PostgreSQL setup required:**  
Create a database called `mumbai_rides` and update the connection string in the notebook:
```python
engine = create_engine("postgresql://postgres:yourpassword@localhost:5432/mumbai_rides")
```

---

## 📅 Progress Log

| Week | What Was Built |
|---|---|
| Week 1 | Environment setup, project structure, 10-row dataset, basic analytics, data cleaning |
| Week 2 | Scaled to 1 lakh rows — drivers, customers, rides datasets with fare logic, foreign keys, unique locations |
| Week 3 | PostgreSQL setup, SQLAlchemy ORM — defined table models, loaded all 3 CSVs into database, ran first queries |

---

> Built as a learning project to understand Data Engineering end to end —  
> from raw CSV files to automated cloud pipelines.