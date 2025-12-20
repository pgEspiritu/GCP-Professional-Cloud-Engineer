# 🛠️ Migrate a MySQL Database to Google Cloud SQL — Challenge Lab

**Lab ID:** GSP306  
**Level:** Advanced  
**Duration:** 20 minutes  
**Credits:** 7  
**Format:** Challenge Lab  
**Note:** This lab may incorporate AI tools to support learning.

---

## 📘 Overview

This is a **challenge lab**, meaning you are given a scenario and required tasks **without step-by-step instructions**. You are expected to apply and extend the skills learned from prior labs.

An **automated scoring system** will validate whether each task is completed correctly.

### Key Expectations
- No new Google Cloud concepts are taught.
- You must troubleshoot issues independently (e.g., error messages, configuration changes).
- To score **100%**, all tasks must be completed within the time limit.

This lab is recommended for learners preparing for the **Google Cloud Certified Professional Cloud Architect** exam.

---

## ⚙️ Setup Instructions

### Before Starting the Lab
- Labs are **timed** and **cannot be paused**.
- The timer starts when you click **Start Lab**.
- Temporary Google Cloud credentials are provided for the duration of the lab.

### Requirements
- A standard web browser (**Chrome recommended**)
- Use an **Incognito or private window** to avoid account conflicts
- Enough uninterrupted time to finish the lab
- **Use only the provided student account** to avoid unexpected charges

---

## 🧩 Challenge Scenario

Your **WordPress blog** is currently running on a server that is no longer suitable. As part of a broader migration effort, you must migrate the blog’s **locally hosted MySQL database** to **:contentReference[oaicite:0]{index=0}**.

### Existing Environment
- **Compute Engine instance:** `blog` (already running)
- **WordPress directory:**  
  `/var/www/html/wordpress`
- **Access:**  
  Via the external IP address of the `blog` instance
- **Current database:**
  - Database name: `wordpress`
  - User: `blogadmin`
  - Password: `Password1*`
  - Database engine: MySQL (local to the instance)

---

## 🎯 Your Challenge

You are required to:

1. Create a new **Cloud SQL** instance to host the migrated database.
2. Create a **database dump** from the existing local MySQL database.
3. Import the dump into the Cloud SQL database.
4. Reconfigure the **:contentReference[oaicite:1]{index=1}** application to use the new Cloud SQL database instead of the local MySQL server.

### WordPress Configuration File
- Location:  
  `/var/www/html/wordpress/wp-config.php`

---

## ✅ Success Criteria

- After migration, the WordPress blog must continue running correctly.
- The application must rely **only on Cloud SQL**, not the local MySQL database.

### Scoring Notes
- You start with **20 points** because the blog is initially running.
- If the blog continues to run correctly using Cloud SQL, those points remain.
- If the database migration is incorrect, the blog test fails and **20 points are deducted**.

---

## 🌍 Location Requirements

Use the following values **where applicable**:
- **Zone:** `ZONE`
- **Region:** `REGION`

---

## 💡 Tips & References

- **Google Cloud SQL How-To Guides**  
  Official documentation for creating instances, databases, and connecting applications.

- **WordPress Installation and Migration**  
  WordPress Codex documentation on installing, configuring, and migrating WordPress sites, including database preparation.

---

## ✅ Task 1: Create a New Cloud SQL Instance (CLI)

### 🎯 Goal
Create a **MySQL Cloud SQL** instance using the **Google Cloud CLI**, deployed in the required **Region: `REGION`**, suitable for hosting a WordPress database.

---

### 📌 Assumptions
- You are logged in using the **student account**
- The correct project is already set
- Required APIs are enabled (Cloud SQL Admin API)

---

### 🔹 Step 1: Set Environment Variables
Use variables to avoid mistakes and keep commands reusable.

```bash
export REGION=REGION
export ZONE=ZONE
export SQL_INSTANCE=wordpress-sql
export ROOT_PASSWORD=Password1*
```

---

### 🔹 Step 2: Create the Cloud SQL MySQL Instance

Create a MySQL instance compatible with WordPress.

```bash
gcloud sql instances create $SQL_INSTANCE \
  --database-version=MYSQL_5_7 \
  --region=$REGION \
  --tier=db-f1-micro \
  --root-password=$ROOT_PASSWORD \
  --storage-type=SSD \
  --storage-size=10GB
```

---

🔹 Step 3: Verify the Instance Creation

Confirm that the instance is running and located in the correct region.

```bash
gcloud sql instances describe $SQL_INSTANCE
```
Verify:

- State: RUNNABLE
- Region: REGION
- Database Version: MySQL 5.7

---

### 📝 Notes

- MySQL 5.7 is fully compatible with WordPress
- The db-f1-micro tier is sufficient for lab workloads
- Cloud SQL instances are regional, not zonal—ZONE will be used later for Compute Engine resources

---

## ✅ Solution — Task 2: Configure the New Database (CLI)

### 🎯 Goal
Configure the Cloud SQL instance by creating the WordPress database and user so it is ready to receive the migrated data.

---

### 🔹 Step 1: Define Task-Specific Variables
(Only variables **not** defined in Task 1)

```bash
export DB_NAME=wordpress
export DB_USER=blogadmin
export DB_PASSWORD=Password1*
```

---

### 🔹 Step 2: Create the WordPress Database

```bash
gcloud sql databases create $DB_NAME \
  --instance=$SQL_INSTANCE
```

---

### 🔹 Step 3: Create the WordPress Database User

```bash
gcloud sql users create $DB_USER \
  --instance=$SQL_INSTANCE \
  --password=$DB_PASSWORD
```

---

### 🔹 Step 4: Verify Database and User

```bash
gcloud sql databases list --instance=$SQL_INSTANCE
```

```bash
gcloud sql users list --instance=$SQL_INSTANCE
```

---

## ✅ Solution — Task 3: Perform Database Dump and Import (CLI)

### 🎯 Goal
Export the existing local **WordPress** MySQL database and import it into the newly created **Cloud SQL** database.

---

### 🔹 Step 1: Define Task-Specific Variables
(Only variables **not** defined in Tasks 1–2)

```bash
export DUMP_FILE=wordpress.sql
export CLOUDSQL_CONNECTION=$(gcloud sql instances describe $SQL_INSTANCE --format="value(connectionName)")
```

---

### 🔹 Step 2: Create a Database Dump from the Local MySQL Server

Run this command on the blog VM.

```bash
mysqldump -u blogadmin -p wordpress > $DUMP_FILE
```

When prompted, enter:
```bash
Password1*
```

---

### 🔹 Step 3: Import the Dump into Cloud SQL

Use the Cloud SQL import command.

```bash
gcloud sql import sql $SQL_INSTANCE $DUMP_FILE \
  --database=$DB_NAME
```

---

### 🔹 Step 4: Verify the Import
(Optional but recommended)

```bash
gcloud sql databases describe $DB_NAME \
  --instance=$SQL_INSTANCE
```

---

### 📝 Notes
- The dump includes all tables, data, and schema required by WordPress
- Do not modify the SQL dump file before importing
- A successful import is required before reconfiguring the application in the next task

---

### ## ✅ Task 4: Reconfigure the WordPress Installation (CLI)

### 🎯 Goal
Update the WordPress configuration so the application uses the **Cloud SQL** database instead of the local MySQL server.

---

### 🔹 Step 1: Define Task-Specific Variables
(Only variables **not** defined in Tasks 1–3)

```bash
export WP_CONFIG=/var/www/html/wordpress/wp-config.php
export CLOUDSQL_IP=$(gcloud sql instances describe $SQL_INSTANCE --format="value(ipAddresses[0].ipAddress)")
```

---

### 🔹 Step 2: Update Database Connection Settings

Edit the WordPress configuration file to point to Cloud SQL.

```bash
sudo sed -i "s/define('DB_NAME'.*/define('DB_NAME', '$DB_NAME');/" $WP_CONFIG
sudo sed -i "s/define('DB_USER'.*/define('DB_USER', '$DB_USER');/" $WP_CONFIG
sudo sed -i "s/define('DB_PASSWORD'.*/define('DB_PASSWORD', '$DB_PASSWORD');/" $WP_CONFIG
sudo sed -i "s/define('DB_HOST'.*/define('DB_HOST', '$CLOUDSQL_IP');/" $WP_CONFIG
```

---

### 🔹 Step 3: Restart the Web Server

Apply the configuration changes.

```bash
sudo systemctl restart apache2
```

---

### 🔹 Step 4: Verify the Site

- Open a browser
- Navigate to the external IP of the blog instance
- Confirm the WordPress site loads correctly

---

### 📝 Notes

- WordPress now connects directly to Cloud SQL using the instance IP
- If the database connection is misconfigured, the site will fail to load
- A working site confirms a successful migration and reconfiguration

---

## ✅ Task 5: Validate and Troubleshoot (CLI)

### 🎯 Goal
Ensure that the WordPress blog works correctly with the Cloud SQL database and fix any issues that arise.

---

### 🔹 Step 1: Verify Database Connection
Check connectivity from the VM to the Cloud SQL instance.

```bash
mysql -h $CLOUDSQL_IP -u $DB_USER -p $DB_NAME
```
- Enter Password1* when prompted.
- Successful login confirms Cloud SQL is reachable and credentials are correct.

---

### 🔹 Step 2: Test the WordPress Site

Open the WordPress site in a browser using the external IP of the blog instance.
Check for:
- Pages loading correctly
- Admin dashboard access
- Existing content (posts, pages, media) is intact

---

### 🔹 Step 3: Check Web Server Logs for Errors

Inspect logs for database or PHP errors.

```bash
sudo tail -f /var/log/apache2/error.log
```
- Look for lines mentioning DB_HOST, mysqli_connect, or WordPress.
- Errors usually indicate incorrect credentials, IP, or database name.

---

### 🔹 Step 4: Troubleshoot Common Issues

| Issue                           | CLI Check / Fix                                                                                                            |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| Cannot connect to Cloud SQL     | Confirm `CLOUDSQL_IP` is correct, check user/password, ensure Cloud SQL **public IP** is enabled and authorized for the VM |
| WordPress shows database errors | Check `wp-config.php` for correct DB_NAME, DB_USER, DB_PASSWORD, DB_HOST                                                   |
| Missing content                 | Verify the import completed successfully using `mysql -h $CLOUDSQL_IP -u $DB_USER -p $DB_NAME -e "SHOW TABLES;"`           |

---

### 🔹 Step 5: Optional — Test with wp-cli

If wp-cli is installed, you can run:

```bash
wp db check --path=/var/www/html/wordpress
```
- Ensures database tables are accessible and intact.

---

# 🎉 Task Completed

Here’s what I accomplished:

- Migrated the **WordPress MySQL database** to **Google Cloud SQL**
- Configured the Cloud SQL instance with the appropriate **database and user**
- Performed a **database dump** from the local MySQL server and imported it into Cloud SQL
- **Reconfigured WordPress** to connect to the new Cloud SQL database
- **Validated** the site functionality and **troubleshot** any issues

WordPress blog is now fully operational using a managed Cloud SQL backend. ✅

---

💡 **Key Takeaways:**
- Cloud SQL provides a **reliable, managed database** for web applications
- Proper configuration of users, database names, and permissions is crucial
- Verification and troubleshooting are essential to ensure a successful migration
