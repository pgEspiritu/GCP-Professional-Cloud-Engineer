# Introduction to SQL for BigQuery and Cloud SQL 📊🗄️  
**Lab · 1 hour 15 minutes · 1 Credit · Introductory**  
GSP281

---

## Overview 🌐
SQL (Structured Query Language) is a standard language for data operations that allows you to ask questions and gain insights from structured datasets. It is widely used in database management for tasks ranging from writing transaction records to performing petabyte-scale data analysis.

This lab is divided into two parts:

### **Part 1 — BigQuery**
You will learn fundamental SQL querying keywords and run queries on a public BigQuery dataset containing London bikeshare information.

### **Part 2 — Cloud SQL**
You will export subsets of the bikeshare dataset into CSV files, upload them to Cloud Storage, and then:
- Create a new Cloud SQL instance  
- Load your CSV data into a Cloud SQL table  
- Practice additional SQL statements to manipulate and edit data  

---

## What you'll learn 🎯
In this lab, you will learn how to:

- Read data into **BigQuery**
- Execute simple queries in BigQuery to explore data
- Export a dataset subset into a **CSV file** and store it in a new **Cloud Storage** bucket
- Create a **Cloud SQL** instance and load your exported CSV as a new table

---

## Prerequisites 📝
> ⚠️ **Very Important:** Before starting this lab, **log out** of your personal or corporate Gmail account.

This is an introductory SQL lab. No prior SQL experience is required. Familiarity with Cloud Storage and Cloud Shell is helpful but not necessary.

If you want more advanced SQL practice, consider:

- **Weather Data in BigQuery**
- **Analyzing Natality Data Using Vertex AI and BigQuery**

Once ready, scroll down and prepare your environment.

---

## Setup and requirements ⚙️

### Before clicking **Start Lab**
- Read all instructions. Labs are timed and **cannot be paused**.
- The timer starts when you click **Start Lab**.
- You will use temporary credentials provided for the duration of the lab.

To complete this lab, you need:

- A standard internet browser (Chrome recommended)
- **Incognito mode** (recommended) to avoid conflicts with your personal account  
- Sufficient time to complete the lab (no pausing allowed)

> 💡 **Important:** Use *only the student account provided*.  
Using your personal Google Cloud account may incur charges.

---

## How to start your lab and sign in to the Google Cloud console 🚀

1. Click **Start Lab**.  
   If required, choose your payment method.
2. The **Lab Details** pane will show:
   - **Open Google Cloud console** button  
   - **Time remaining**  
   - **Temporary Username and Password**  
   - Additional instructions  
3. Click **Open Google Cloud console**  
   (Right-click > *Open in Incognito Window* recommended for Chrome users)

A new tab opens with the Google Sign-In page.

> 💡 Tip: Arrange the Cloud console and lab instructions side-by-side.

4. If you see **Choose an account**, click **Use another account**.
5. Copy the **Username** from the Lab Details pane → paste → **Next**.
6. Copy the **Password** → paste → **Next**.

> ⚠️ Do **not** use your personal Google Cloud credentials.

7. Proceed through the initial account setup:
   - Accept terms and conditions  
   - Do **not** add recovery options  
   - Do **not** enable 2-factor authentication  
   - Do **not** sign up for free trials  

After a few seconds, the **Google Cloud console** opens.

> 🔍 You can access services using the **Navigation menu** or by typing service names in the **Search** field.

---

# Task 1. The basics of SQL 🧠📊

## Databases and tables 📁
SQL works with **structured datasets**—data organized in a predictable format, typically using **tables** consisting of **rows and columns**.

Example of a structured table:

| User  | Price | Shipped |
|-------|--------|----------|
| Sean  | $35   | Yes      |
| Rocky | $50   | No       |

If you're familiar with Google Sheets, this should look similar.

### Unstructured vs Structured Data
- **Unstructured data** (e.g., images) **cannot** be used directly with SQL or stored natively in BigQuery tables.  
  For such data, you’d use services like **Cloud Vision API**.
- **Structured data**, like the table above, is ideal for SQL.

A **Database** is simply a collection of tables.  
In most cases (including this lab), you run SQL queries on **individual tables** or **joined tables**, not entire databases.

---

## SELECT and FROM 🔍
Before running a query, it's useful to think about **what question you want answered**.

SQL uses predefined keywords to translate your question into a readable command. Two essential keywords are:

- **SELECT** → specifies *which columns* you want
- **FROM** → specifies *which table* the data comes from

### Example table: `example_table`
Columns: `USER`, `PRICE`, `SHIPPED`

---

### Get all values from the USER column:
```sql
SELECT USER FROM example_table
```

Select multiple columns (USER and SHIPPED):
```sql
SELECT USER, SHIPPED FROM example_table
```

These commands retrieve the respective columns from memory.

---

### WHERE 🔎

The WHERE keyword filters rows based on a condition.

Example: Get names of users whose packages were shipped:
```sql
SELECT USER FROM example_table WHERE SHIPPED='YES'
```

This returns only users with `SHIPPED` = `'YES'.`

---

# 🧪 Task 2. Exploring the BigQuery Console

## 🏗️ The BigQuery Paradigm

BigQuery is a fully managed, petabyte-scale data warehouse on Google Cloud.
It allows data analysts and data scientists to run fast SQL queries on massive datasets without managing servers.

You can use BigQuery via:
- 💻 Cloud Shell CLI (bq)
- 🌐 Web console (used in this lab)

---

### 🚀 Open the BigQuery Console

1. Go to Navigation menu → BigQuery.
2. Click Done on the “Welcome to BigQuery” popup.

🖥️ BigQuery Console Layout
- Right pane → Query Editor + Query history
- Left pane → Navigation menu with:
  - Explorer
  - Saved queries
  - Job history
Your project currently contains no datasets or tables, so nothing appears when expanding it.

---

### 📥 Uploading Queryable Data

You'll load public data into the BigQuery console (not into your project).
1. In the Explorer, click Add data.
2. Select Star a project by name.
3. Enter:
```kotlin
bigquery-public-data
```
4. Click STAR.
You're still inside your lab project; you only starred a public project.

📂 Newly Accessible Data
- Project → bigquery-public-data
- Dataset → london_bicycles
  - Tables:
    - cycle_hire
    - cycle_stations
Open cycle_hire → click Preview to view sample rows.
This table contains 83,434,866 rows of London bike-share trips (2015–2017). 🚴‍♂️

---

### 🧮 Running SQL Queries in BigQuery

🔍 1. Simple SELECT Query
Extract one column: end_station_name.
```sql
SELECT end_station_name 
FROM `bigquery-public-data.london_bicycles.cycle_hire`;
```
After ~20 seconds, BigQuery returns 83M+ rows — one per bike trip.

🕒 2. Using WHERE to Filter Rows
Find trips lasting 20 minutes or longer.
Duration is in seconds → 20 minutes = 20 × 60 = 1200 seconds.
```sql
SELECT * 
FROM `bigquery-public-data.london_bicycles.cycle_hire` 
WHERE duration >= 1200;
```
This query takes about a minute.

📊 Result
Rows returned: 26,441,016

Fraction of total:
```math
26,441,016 / 83,434,866 ≈ 0.30
```
➡️ ~30% of all London bikeshare rides lasted 20 minutes or more! 🚴‍♀️⏱️

---

# Task 3. More SQL Keywords: GROUP BY, COUNT, AS, and ORDER BY 🧩📊

## GROUP BY 🔗
The **GROUP BY** keyword aggregates rows that share a common value and returns only **unique entries** for that value.

Example: Get all unique starting stations:

```sql
SELECT start_station_name 
FROM `bigquery-public-data.london_bicycles.cycle_hire` 
GROUP BY start_station_name;
```
- Click Run
- Results: 954 rows, representing 954 distinct London bikeshare starting points.
> Without GROUP BY, all 83,434,866 rows would be returned.

---

### COUNT() 🔢

The COUNT() function returns the number of rows that match a certain criterion.
Combine with GROUP BY to see counts per category.

Example: Count rides per starting station:
```sql
SELECT start_station_name, COUNT(*) 
FROM `bigquery-public-data.london_bicycles.cycle_hire` 
GROUP BY start_station_name;
```
- Output shows how many rides start at each station.

---

## AS 🏷️

The AS keyword creates an alias for a column or table.

Example: Rename the COUNT column to num_starts:
```sql
SELECT start_station_name, COUNT(*) AS num_starts 
FROM `bigquery-public-data.london_bicycles.cycle_hire` 
GROUP BY start_station_name;
```
- Click Run
- The right column now shows num_starts instead of COUNT(*).
> Aliases make results easier to read, especially with large datasets.

---

## ORDER BY ⬆️⬇️

The ORDER BY keyword sorts results based on a column in ascending (ASC) or descending (DESC) order.

Examples:
1. Alphabetically by station name:
```sql
SELECT start_station_name, COUNT(*) AS num 
FROM `bigquery-public-data.london_bicycles.cycle_hire` 
GROUP BY start_station_name 
ORDER BY start_station_name;
```

2. Numerically from lowest to highest count:
```sql
SELECT start_station_name, COUNT(*) AS num 
FROM `bigquery-public-data.london_bicycles.cycle_hire` 
GROUP BY start_station_name 
ORDER BY num;
```

3. Numerically from highest to lowest count:
```sql
SELECT start_station_name, COUNT(*) AS num 
FROM `bigquery-public-data.london_bicycles.cycle_hire` 
GROUP BY start_station_name 
ORDER BY num DESC;
```
- Click Run for each query.
- The last query shows "Hyde Park Corner, Hyde Park" as the station with the highest number of starts.
  - 671,688 of 83,434,866 rides (~<1%) start here.


---

# Task 4. Working with Cloud SQL 🗄️☁️

## Exporting Queries as CSV Files 📤
Cloud SQL is a fully-managed service for **PostgreSQL and MySQL databases** in Google Cloud.  
It accepts data in two formats: **dump files (.sql)** or **CSV files (.csv)**.  

In this task, you will **export subsets of the `cycle_hire` table as CSV** and upload them to Cloud Storage as an intermediate step.

---

### 1️⃣ Export start station data
1. In the BigQuery console, ensure your last query was:

```sql
SELECT start_station_name, COUNT(*) AS num 
FROM `bigquery-public-data.london_bicycles.cycle_hire` 
GROUP BY start_station_name 
ORDER BY num DESC;
```

2. In the Query Results section, click:
Save results → CSV (Local download)

3. Note the location and filename of the downloaded CSV — you will need it soon.

### 2️⃣ Export end station data

1. Clear the query editor, then run:
```sql
SELECT end_station_name, COUNT(*) AS num 
FROM `bigquery-public-data.london_bicycles.cycle_hire` 
GROUP BY end_station_name 
ORDER BY num DESC;
```

2. In the Query Results section, click:
Save results → CSV (Local download)

3. Note the location and filename of this CSV.

---

Upload CSV Files to Cloud Storage ☁️📦
1. In the Cloud Console, go to Navigation menu → Cloud Storage → Buckets.
2. Click CREATE BUCKET.
  - Enter a unique bucket name
  - Keep all other settings as default
  - Click Create
  - Click Confirm if prompted for Public access will be prevented
3. Open your newly created bucket.
4. Click UPLOAD → Upload files, and select your start_station_name CSV.
Repeat for the end_station_name CSV.
5. Rename the files:
- Click the three dots next to the start station file → Rename → start_station_data.csv
- Click the three dots next to the end station file → Rename → end_station_data.csv\

You should now see both:
- `start_station_data.csv`
- `end_station_data.csv`
in the Objects list of your bucket.

---

# Task 5. Create a Cloud SQL Instance 🗄️⚡

1. In the Cloud Console, go to **Navigation menu → Cloud SQL**.
2. Click **CREATE INSTANCE → Choose MySQL**.
3. Configure the instance:
   - **Cloud SQL edition:** Enterprise  
   - **Instance ID:** `my-demo`  
   - **Password:** `[a secure password]` (remember this!)  
   - **Database version:** MySQL 8  
   - **Edition preset:** Development (4 vCPU, 16 GB RAM, 100 GB Storage, Single zone)  

> ⚠️ Warning: Choosing a preset larger than Development will flag your project and terminate the lab.

4. Set **Region:** `<Lab Region>`  
5. Set **Multi zones (Highly available) → Primary Zone:** `<Lab Zone>`  
6. Click **CREATE INSTANCE**

> ⏱️ Note: Instance creation may take a few minutes.  
> A green checkmark appears next to the instance name once ready.

7. Click on the **Cloud SQL instance** to open the **SQL Overview** page.

---

# Task 6. New Queries in Cloud SQL 🧩🗄️

## CREATE Keyword (Databases and Tables) 🏗️
With your Cloud SQL instance ready, you can create a **database** using Cloud Shell.

---

### 1️⃣ Open Cloud Shell
Click the **Cloud Shell** icon in the top right corner of the console.

---

### 2️⃣ Set Project ID
Run the following commands to set your project ID as an environment variable:

```bash
export PROJECT_ID=$(gcloud config get-value project)
gcloud config set project $PROJECT_ID
```

---

### 3️⃣ Authenticate in Cloud Shell

Run:
```bash
gcloud auth login --no-launch-browser
```
- If prompted [Y/n], type Y and press ENTER.
- Open the provided link in the same browser as your Qwiklabs account.
- Copy the verification code and paste it in Cloud Shell.

---

### 4️⃣ Connect to your Cloud SQL instance

Replace my-demo if you used a different instance name:
```sql
gcloud sql connect my-demo --user=root --quiet
```
- Enter the root password when prompted (cursor will not move).
- Wait if you see: "Operation failed because another operation was already in progress".

You should see: 
```vbnet
Welcome to the MySQL monitor.  Commands end with ; or \g.
Server version: 8.0.31-google (Google)
...
mysql>
```

### 5️⃣ Create a Database

At the MySQL prompt, create a database called bike:
```sql
CREATE DATABASE bike;
```

Expected output:
```graphsql
Query OK, 1 row affected (0.05 sec)
```
> Your Cloud SQL instance now has a custom database ready to store London bikeshare data.

--- 

## Create Tables in the `bike` Database 🏗️

### 1️⃣ Create `london1` table
```sql
USE bike;
CREATE TABLE london1 (
    start_station_name VARCHAR(255), 
    num INT
);
```
- USE bike; selects the database.
- london1 has two columns:
  - start_station_name → string, up to 255 characters
  - num → integer

---

### 2️⃣ Create london2 table
```sql
USE bike;
CREATE TABLE london2 (
    end_station_name VARCHAR(255), 
    num INT
);
```

---

### 3️⃣ Verify tables
```sql
SELECT * FROM london1;
SELECT * FROM london2;
```
- Output: Empty set (0.04 sec) — tables are empty.

---

## Upload CSV Files to Tables 📤

### 1️⃣ Upload start_station_data.csv → london1
1. In Cloud SQL console, click IMPORT.
2. Cloud Storage file field → Browse → start_station_data.csv
3. File format: CSV
4. Database: bike
5. Table: london1
6. Click Import

### 2️⃣ Upload end_station_data.csv → london2
1. Repeat the same process for the other CSV file.
2. Table: london2

### 3️⃣ Verify data in tables
```sql
SELECT * FROM london1;  -- 955 rows
SELECT * FROM london2;  -- 959 rows
```

---

## DELETE Keyword 🗑️

Remove rows with num = 0 (column headers from CSV):
```sql
DELETE FROM london1 WHERE num=0;
DELETE FROM london2 WHERE num=0;
```
- Output: Query OK, 1 row affected (0.04 sec)
- These rows are now deleted from the tables.

---

## INSERT INTO Keyword ➕

Add a new row to london1:
```sql
INSERT INTO london1 (start_station_name, num) 
VALUES ("test destination", 1);
```
- Output: Query OK, 1 row affected (0.05 sec)
- Row appears at the bottom of london1.

---

## UNION Keyword 🔀

Combine data from both tables:
```sql
SELECT start_station_name AS top_stations, num 
FROM london1 
WHERE num > 100000
UNION
SELECT end_station_name, num 
FROM london2 
WHERE num > 100000
ORDER BY top_stations DESC;
```
- Explanation:
  - AS top_stations → alias for column
  - WHERE num > 100000 → filters high-volume stations
  - UNION → merges results from london1 and london2
  - ORDER BY top_stations DESC → sorts alphabetically in descending order

- Example Output: Shows top stations for rideshare start and end points (13–14 stations).
> With these basic SQL commands, you successfully queried a large dataset and extracted meaningful insights. 🚴‍♂️📊

---

# Task Completed 🎉✅

In this lab, you successfully learned and practiced the fundamentals of SQL and applied them in both **BigQuery** and **Cloud SQL**.

## Key Takeaways:

- **SQL Basics:**  
  - Core keywords: `SELECT`, `FROM`, `WHERE`, `GROUP BY`, `COUNT`, `AS`, `ORDER BY`, `INSERT INTO`, `DELETE`, `UNION`.
  - Understanding **databases**, **tables**, and structured datasets.

- **BigQuery:**  
  - Loaded and queried public datasets.
  - Explored `cycle_hire` table to analyze London bikeshare data.
  - Exported query results as **CSV files**.

- **Cloud SQL:**  
  - Created a **MySQL instance**.
  - Created databases (`bike`) and tables (`london1`, `london2`).
  - Imported CSV files into tables.
  - Manipulated data using SQL commands.
  - Combined and analyzed data using the `UNION` keyword.

You practiced **reading, manipulating, and analyzing data**, learned how to move data between **BigQuery** and **Cloud SQL**, and drew insights about London bikesharing patterns.
