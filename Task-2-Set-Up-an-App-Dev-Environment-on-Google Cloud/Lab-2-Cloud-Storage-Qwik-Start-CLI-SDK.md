# ☁️ Cloud Storage: Qwik Start – CLI/SDK  
**Lab • 30 minutes ⏱️ • 1 Credit 💳 • Introductory 📘**  
GSP074

---

## 📝 Overview
Cloud Storage allows world-wide storage and retrieval of any amount of data at any time. You can use Cloud Storage for a range of scenarios including:

- 🌐 Serving website content  
- 📦 Storing data for archival and disaster recovery  
- ⬇️ Distributing large data objects via direct download  

In this hands-on lab, you will learn how to create a storage bucket, upload objects, create folders and subfolders, and make objects publicly accessible **using the Google Cloud command line**.

You can verify your work anytime via:  
➡️ **Navigation menu → Cloud Storage** (refresh the browser after each command)

---

## 🎯 What You'll Do
Using the Google Cloud command line, you will:

- 🪣 Create a storage bucket  
- 📤 Upload objects to a bucket  
- 📁 Create folders and subfolders  
- 🌍 Make bucket objects publicly accessible  

---

## 🧰 Setup and Requirements

### Before clicking **Start Lab**
- Labs are timed ⏳ and **cannot be paused**  
- Temporary Google Cloud credentials are provided  
- Use **Incognito Mode** to avoid conflicts  
- Use **only the student account** to avoid charges  

### You need:
- 💻 A standard internet browser (Chrome recommended)  
- ⏰ Enough time to complete the lab  

---

## 🚀 How to Start the Lab and Sign In

1. Click **Start Lab**  
2. See the Lab Details pane with:  
   - ▶️ Open Google Cloud Console  
   - ⏳ Time remaining  
   - 👤 Temporary Username & Password  
3. Click **Open Google Cloud Console**  
4. On the sign-in page:  
   - Click **Use another account**  
   - Copy/paste **Username** → Next  
   - Copy/paste **Password** → Next  
5. Accept terms → Do **not** add recovery/2FA → Do **not** start free trials  
6. Google Cloud Console will open  

Use the **Navigation menu** or **Search bar** to access services.

---

# 💻 Activate Cloud Shell

1. Click **Activate Cloud Shell** (top-right terminal icon)  
2. Continue through prompts  
3. Authorize Cloud Shell  
4. You’ll see:  
```text
Your Cloud Platform project in this session is set to "PROJECT_ID"
```

---

### Optional useful commands

List active account:
```bash
gcloud auth list
```


List active project:
```bash
gcloud config list project
```

---

### Set the region:
```bash
gcloud config set compute/region "REGION"
```


---

# 🧪 Task 1: Create a Bucket

### Bucket Naming Rules
- Only lowercase letters, numbers, dashes (-), underscores (_), and dots (.)  
- Must start/end with a letter or number  
- 3–63 characters (up to 222 with dots)  
- No IP-address-looking names  
- Cannot contain **goog** or **google** or misspellings  
- Avoid underscores and adjacent punctuation (DNS compliance)

### Create your bucket:
```bash
gcloud storage buckets create gs://<YOUR-BUCKET-NAME>
```

If the name is taken, you will see:
```nginx
ServiceException: 409 Bucket <name> already exists.
```

Choose a new name.

### ✔️ Test Completed Task
Click **Check my progress**.

### ❓ Test Your Understanding
**Each bucket has a default storage class that you can specify during creation.**  
- True  
- False  

---

# 🧪 Task 2: Upload an Object into Your Bucket

Download the ADA image:
```bash
curl https://upload.wikimedia.org/wikipedia/commons/thumb/a/a4/Ada_Lovelace_portrait.jpg/800px-Ada_Lovelace_portrait.jpg
 --output ada.jpg
```

Upload it:
```bash
gcloud storage cp ada.jpg gs://YOUR-BUCKET-NAME
```


Remove local copy:
```bash
rm ada.jpg
```

---

# 🧪 Task 3: Download an Object from Your Bucket
```bash
gcloud storage cp -r gs://YOUR-BUCKET-NAME/ada.jpg .
```


Expected output:
```nginx
Operation completed over 1 objects/360.1 KiB.
```

---

# 🧪 Task 4: Copy an Object to a Folder in the Bucket
```bash
gcloud storage cp gs://YOUR-BUCKET-NAME/ada.jpg gs://YOUR-BUCKET-NAME/image-folder/
```


Expected output:
```nginx
Operation completed over 1 objects/360.1 KiB
```


### ✔️ Test Completed Task
Copy an object into a folder.

---

# 🧪 Task 5: List Contents of a Bucket or Folder

```bash
gcloud storage ls gs://YOUR-BUCKET-NAME
```


Example output:
```nginx
gs://YOUR-BUCKET-NAME/ada.jpg
gs://YOUR-BUCKET-NAME/image-folder/
```

---

# 🧪 Task 6: List Details for an Object
```bash
gcloud storage ls -l gs://YOUR-BUCKET-NAME/ada.jpg
```


Example:
```nginx
TOTAL: 1 objects, 360.1 KiB
```

---

# 🧪 Task 7: Make Your Object Public

Give read access to everyone:
```bash
gsutil acl ch -u AllUsers:R gs://YOUR-BUCKET-NAME/ada.jpg
```


Expected:
```nginx
Updated ACL on gs://YOUR-BUCKET-NAME/ada.jpg
```


### ✔️ Test Completed Task
Confirm your progress.

### Verify:
Open **Cloud Storage → your bucket → ada.jpg → Copy URL**

This is **Ada Lovelace**, widely considered the first computer programmer.

---

### ❓ Test Your Understanding
**An ACL (access control list) defines who has access to your buckets and objects.**  
- True  
- False  

---

# 🧪 Task 8: Remove Public Access

Remove public read permission:
```bash
gsutil acl ch -d AllUsers gs://YOUR-BUCKET-NAME/ada.jpg
```


Check in console → Refresh → Public icon removed.

### ❓ Test Your Understanding
To stop publicly sharing an object, remove the permission entry that has:  
- By updating storage class  
- By removing project owner  
- **allUsers** ✔️

---

# 🗑️ Delete Objects

Delete the image:
```bash
gcloud storage rm gs://YOUR-BUCKET-NAME/ada.jpg
```


Expected:
```nginx
Removing gs://YOUR-BUCKET-NAME/ada.jpg...
```

---

# Task Completed

- Created a storage bucket 🪣  
- Organized it with folders 📁  
- Uploaded objects 📤  
- Managed public access 🌍  
- Used Cloud Shell for all operations 💻  






