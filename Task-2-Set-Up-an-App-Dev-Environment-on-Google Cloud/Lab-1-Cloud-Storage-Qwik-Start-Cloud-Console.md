# 🌩️ Cloud Storage: Qwik Start – Cloud Console  

## 📝 Overview
Cloud Storage allows world-wide storage and retrieval of any amount of data at any time. You can use Cloud Storage for a range of scenarios including serving website content, storing data for archival and disaster recovery, or distributing large data objects to users via direct download.

---

## 🎯 What You'll Do
In this hands-on lab you will learn how to use the Cloud Console to:

- 🪣 Create a storage bucket  
- 📤 Upload objects to the bucket  
- 📁 Create folders and subfolders in the bucket  
- 🌍 Make objects in a storage bucket publicly accessible  

---

## 🧰 Setup and Requirements

### Before you click **Start Lab**
Labs are timed ⏳ and cannot be paused. The timer starts once you click **Start Lab**, and that's how long Google Cloud resources remain available.

This hands-on lab provides temporary credentials to access Google Cloud.

You need:

- 💻 A standard internet browser (Chrome recommended)  
- 🔒 Use **Incognito** or **Private** mode to avoid conflicts  
- ⏰ Enough uninterrupted time to complete the lab  
- 🚫 Use **only the student account** provided to avoid charges  

---

## 🚀 How to Start Your Lab and Sign In to Google Cloud Console

1. Click **Start Lab**. If payment is required, choose your payment method.  
2. Look at the **Lab Details** pane, which contains:  
   - ▶️ **Open Google Cloud Console**  
   - ⏳ Time remaining  
   - 👤 Temporary **Username** and **Password**  
3. Click **Open Google Cloud Console** (Incognito recommended).  
4. The Google sign-in page appears.  
5. If you see **Choose an account**, click **Use another account**.  
6. Copy/paste the **Username** → Next.  
7. Copy/paste the **Password** → Next.  
8. Accept the terms.  
9. Do **not** add recovery options or 2FA.  
10. Do **not** sign up for free trials.  
11. The Google Cloud Console loads.  

To access Cloud services, open the **Navigation menu** or use the **Search bar**.

---

# 🧪 Task 1: Create a Bucket

Buckets are the basic containers that hold your data in Cloud Storage.

### To create a bucket:

1. In the Cloud Console, navigate to  
   **Navigation menu → Cloud Storage → Buckets**  
2. Click **+ Create**  
3. Enter your bucket details:  
   - **Name your bucket:** Use a unique name (Project ID is allowed).  
   - Bucket naming rules:  
     - Only lowercase letters, numbers, dashes (-), underscores (_), dots (.)  
     - Must start and end with a letter or number  
     - 3–63 characters (up to 222 with dots)  
     - Cannot look like an IP address  
     - Cannot contain “goog”, “google”, or close misspellings  
     - Avoid underscores for DNS compatibility  
4. Choose:  
   - **Location type:** Region  
   - **Location:** (value provided at lab start)  
   - **Default storage class:** Standard  
   - **Access control:** Uniform  
   - Uncheck **Enforce public access prevention**  
5. Leave other fields as default → **Create**

If prompted about public access, uncheck the setting and confirm.

### ✅ Test Completed Task
Click **Check my progress** to verify.

### ❓ Test Your Understanding

**Every bucket must have a unique name across the entire Cloud Storage namespace.**  
- True  
- False  

---

# 🧪 Task 2: Upload an Object into the Bucket

1. Right-click the kitten image above and download it as **kitten.png**.  
2. Go to your bucket in the Cloud Console.  
3. In **Objects**, click **Upload → Upload Files**.  
4. Select the downloaded file.  
5. Ensure it is named **kitten.png**.  
   - If not, click the **three-dot menu → Rename** → kitten.png  
6. After upload, the object appears with details like size and type.

### ✅ Test Completed Task
Click **Check my progress** to verify.

### ❓ Test Your Understanding

**Object names must be unique only within a given bucket.**  
- True  
- False  

---

# 🧪 Task 3: Share a Bucket Publicly

1. Click the **Permissions** tab.  
2. Ensure view is set to **Principals**.  
3. Click **Grant Access**.  
4. In **New principals**: enter `allUsers`.  
5. In **Select a role**: choose  
   **Cloud Storage → Storage Object Viewer**  
6. Click **Save**.  
7. In the warning popup, click **Allow public access**.

### ✅ Test Completed Task
Click **Check my progress** to verify.

### To verify:
1. Open the **Objects** tab.  
2. Public access should show **Public to Internet**.  
3. Click **Copy URL** next to kitten.png.  
4. Open the URL in a new browser tab.

The URL format:  
```text
https://storage.googleapis.com/YOUR_BUCKET_NAME/kitten.png
```


---

# 🧪 Task 4: Create Folders

1. In **Objects**, click **Create folder**.  
2. Name it **folder1** → Create.  
3. Click **folder1** → **Create folder** → name it **folder2** → Create.  
4. Go to **folder2 → Upload → Upload Files**.  
5. Upload the screenshot you downloaded.  

---

# 🗑️ Task 5: Delete a Folder

1. Click the arrow beside **Bucket details** to return to bucket list.  
2. Select the bucket.  
3. Click **Delete**.  
4. Type **DELETE** to confirm.  
5. Click **Delete** to permanently delete the folder and all contents.

---

Task Completed

- Create a Cloud Storage bucket 🪣  
- Upload objects 📤  
- Organize data with folders 📁  
- Make objects publicly accessible 🌍  
