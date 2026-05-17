# Apache Spark — Beginner Notes 🚀

## What is Spark?

Apache Spark is a **distributed unified analytics engine**.

In simple words:

> Spark splits huge data work across many computers and combines the final result.

---

# Why is Spark called “Unified”?

Because Spark can do:

- SQL Queries
- Streaming
- Machine Learning
- Batch Processing
- Graph Processing

…all inside ONE engine.

```text
           Apache Spark
        ┌───────────────┐
        │ SQL           │
        │ Streaming     │
        │ Machine Learn │
        │ Graphs        │
        │ Big Data ETL  │
        └───────────────┘
```

---

# Why Spark Exists

Imagine processing:

```text
1 BILLION rows of data
```

One computer becomes slow.

Spark solves this by distributing work across many machines.

---

# Spark Architecture

```text
                You
                 │
                 ▼
          Write Spark Code
                 │
                 ▼
          ┌─────────────┐
          │ Spark Driver│
          └─────────────┘
                 │
     ┌───────────┼───────────┐
     ▼           ▼           ▼
 ┌────────┐ ┌────────┐ ┌────────┐
 │Worker 1│ │Worker 2│ │Worker 3│
 └────────┘ └────────┘ └────────┘
     │           │           │
   Task         Task        Task
```

---

# Example

```python
df.groupBy("city").avg("fare")
```

Spark internally:

```text
Read Data
   ↓
Group By City
   ↓
Average Fare
```

Then:

```text
Split work across machines
        ↓
Parallel processing
        ↓
Combine final result
```

---

# Most Important Concept — Lazy Evaluation

Spark is **lazy**.

```text
You write operations
        ↓
Spark builds DAG
        ↓
Waits
        ↓
Action called
        ↓
Now executes everything
```

---

# Transformations vs Actions

## Transformations

```python
filter()
groupBy()
select()
map()
```

## Actions

```python
show()
count()
collect()
write()
```

---

# Final Mental Model

```text
Your Code
   ↓
Spark builds DAG
   ↓
Splits work into tasks
   ↓
Distributes tasks across machines
   ↓
Parallel execution
   ↓
Final result returned
```

---

# One-Line Definition

> Apache Spark is a distributed engine that processes massive datasets by dividing computations across multiple machines in parallel.

---

# Spark Ecosystem

## The Unified Stack Architecture

The image below illustrates Apache Spark as a layered, unified stack where higher-level libraries rely on the core engine, allowing developers to use different languages and data sources seamlessly.

![Spark Ecosystem](spark-ecosystem.jpg)

> Image Source: Apache Spark Ecosystem Architecture Diagram

---

## 1. Language APIs (Top Layer)

Spark supports multiple programming languages:

- Python
- Scala
- Java
- R

This allows developers to work with Spark using their preferred language.

---

## 2. Domain-Specific Libraries

Spark includes built-in libraries for different workloads:

- **Spark SQL** → Structured data processing
- **Spark Streaming** → Real-time data processing
- **Spark MLlib** → Machine learning
- **Spark GraphX** → Graph and network analysis

---

## 3. Core Engine

### Spark Core
Handles:
- Task scheduling
- Memory management
- Fault tolerance

### RDD API
Spark’s distributed data structure used for parallel processing.

---

## 4. Deployment Environment

Spark can run on:

- Docker
- Kubernetes
- Amazon EMR
- Databricks

---

## 5. Data Sources

Spark can connect to:

- CSV
- JSON
- Parquet
- Hadoop
- Hive
- SQL Databases
- AWS S3

using connectors.

---

# Spark Shuffle Cheat Sheet

This table summarizes when shuffles happen in Spark and whether you need to adjust `spark.sql.shuffle.partitions`.

| Data Type                  | Operation                              | Shuffle Happens? | Adjust `spark.sql.shuffle.partitions`? | Notes                                                                 |
|-----------------------------|----------------------------------------|-----------------|----------------------------------------|-----------------------------------------------------------------------|
| CSV / Parquet / DataFrame   | `select`, `filter`, `withColumn`       | ❌ No            | ❌ Not needed                           | Row-wise operations, each partition can work independently           |
|                             | `groupBy`, `join`, `distinct`, `aggregate` | ✅ Yes           | ✅ Yes, especially on local machine or large dataset | Spark moves rows so related keys end up in the same partition        |
| Spark SQL Queries           | `SELECT ... WHERE ...`                  | ❌ No            | ❌ Not needed                           | Simple filtering / selecting columns → no shuffle                    |
|                             | `SELECT key, COUNT(*) FROM table GROUP BY key` | ✅ Yes           | ✅ Yes                                   | `GROUP BY` triggers shuffle                                           |
|                             | `JOIN table1 t1 ON t1.id = t2.id`      | ✅ Yes           | ✅ Yes                                   | Data from different partitions must come together                    |
| Streaming DataFrames        | `select`, `filter`, `withColumn`       | ❌ No            | ❌ Not needed                           | Row-wise operation in micro-batch                                     |
|                             | `groupBy(window, key).count(), aggregations` | ✅ Yes           | ✅ Yes                                   | Windowed aggregations / streaming joins → shuffle occurs             |
|                             | `join` with another stream or batch    | ✅ Yes           | ✅ Yes                                   | Shuffle needed to match keys across streams                           |

## Quick Tips for `spark.sql.shuffle.partitions`

- **Local / Small Datasets**
  - Reduce partitions to ~2 × number of cores.
  - Avoid creating hundreds of tiny tasks.

- **Production / Big Data**
  - Default is `200`, but this may be too low for TB-scale datasets.
  - Increase if each partition is huge → helps avoid OOM (Out Of Memory) errors.

- **Rule of Thumb**
  - **No shuffle** → ignore `spark.sql.shuffle.partitions`.
  - **Shuffle** → adjust `spark.sql.shuffle.partitions` accordingly.
