# Set Up an App Dev Environment on Google Cloud: Challenge Lab ☁️💻

**Lab Type:** Challenge Lab  
**Duration:** 1 hour ⏱️  
**Credit:** 1 💰  
**Level:** Introductory  

> This lab may incorporate AI tools to support your learning.  

---

## Introduction 📝

In a challenge lab, you’re given a scenario and a set of tasks. Instead of following step-by-step instructions, you will use the skills learned from the labs in the course to figure out how to complete the tasks on your own! An automated scoring system (shown on this page) will provide feedback on whether you have completed your tasks correctly.

When you take a challenge lab, you will not be taught new Google Cloud concepts. You are expected to extend your learned skills, like changing default values and reading and researching error messages to fix your own mistakes.

To score 100% you must successfully complete all tasks within the time period!

This lab is recommended for students who have enrolled in the **Set Up an App Dev Environment on Google Cloud** skill badge. Are you ready for the challenge?

---

## Setup ⚙️

Before you click the **Start Lab** button:

- Read these instructions. Labs are timed and you cannot pause them. The timer, which starts when you click **Start Lab**, shows how long Google Cloud resources are made available to you.
- This hands-on lab lets you do the lab activities in a **real cloud environment**, not a simulation or demo. It gives you new, temporary credentials to sign in and access Google Cloud for the lab duration.
- To complete this lab, you need:
  - Access to a standard internet browser (Chrome recommended).  
  - Use an Incognito or private browser window to prevent conflicts between your personal account and the student account.  
  - Time to complete the lab—remember, once you start, you cannot pause a lab.  
  - Use **only the student account** for this lab. Using a different Google Cloud account may incur charges.

---

## Challenge Scenario 🎯

You are just starting your junior cloud engineer role with **Jooli Inc.**. So far, you have been helping teams create and manage Google Cloud resources.

You are expected to have the skills and knowledge for these tasks, so don’t expect step-by-step guides.

Your challenge:  

You are asked to help a newly formed development team with some of their initial work on a new project called **Memories**, which focuses on storing and organizing photographs. You have been asked to assist the Memories team with **initial configuration for their application development environment**.

You receive the following request to complete the tasks:

- Create a bucket for storing the photographs.  
- Create a Pub/Sub topic that will be used by a Cloud Run Function you create.  
- Create a Cloud Run Function.  
- Remove the previous cloud engineer’s access from the memories project.

### Jooli Inc. Standards:

- Create all resources in the `us-east4` region and `us-east4-c` zone, unless otherwise directed.  
- Use the project VPCs.  
- Naming is normally `team-resource`, e.g., an instance could be named `kraken-webserver1`.  
- Allocate **cost-effective resource sizes**. Projects are monitored; excessive resource use may result in project termination. Guidance: use `e2-micro` for small Linux VMs and `e2-medium` for Windows or Kubernetes nodes.

---

## Task 1: Create a Bucket 🪣

You need to create a bucket called `qwiklabs-gcp-03-c8adde435dba-bucket` for storing photographs. Ensure the resource is created in the `REGION` region and `ZONE` zone.

- Click **Check my progress** to verify the objective.  
- Create a bucket called `qwiklabs-gcp-03-c8adde435dba-bucket`.

### ✅ Solution

```bash
gsutil mb -l us-east4 -p qwiklabs-gcp-03-c8adde435dba gs://qwiklabs-gcp-03-c8adde435dba-bucket/
```

where 
- qwiklabs-gcp-03-c8adde435dba = Project ID
- qwiklabs-gcp-03-c8adde435dba-bucket = Bucket Name
- us-east4 = Region

then verify if successfully created:
```bash
gsutil ls -p qwiklabs-gcp-03-c8adde435dba
```

> Successfully created.

---

## Task 2: Create a Pub/Sub Topic 📢

Create a Pub/Sub topic called `topic-memories-548` for the Cloud Run Function to send messages.

```bash
gcloud pubsub topics create topic-memories-548 --project=qwiklabs-gcp-03-c8adde435dba
```

where 
- qwiklabs-gcp-03-c8adde435dba = Project ID
- topic-memories-548 = Topic Name

then verify:
```bash
gcloud pubsub topics list --project=qwiklabs-gcp-03-c8adde435dba
```

> Successfully created.

---

## Task 3: Create the Thumbnail Cloud Run Function 🖼️🚀

Create the function
Create a Cloud Run Function `memories-thumbnail-maker` that will to create a thumbnail from an image added 
to the `qwiklabs-gcp-03-c8adde435dba-bucket` bucket.

Ensure the Cloud Run Function is using the Cloud Run function environment (which is 2nd generation). 
Ensure the resource is created in the `us-east4` region and `us-east4-c` zone.

  1. Create a Cloud Run Function (2nd generation) called `memories-thumbnail-maker` using `Node.js 22`.
     Note: The Cloud Run Function is required to execute every time an object is created in the bucket created in Task 1.
           During the process, Cloud Run Function may request permission to enable APIs or request permission to grant roles to
           service accounts. Please enable each of the required APIs and grant roles as requested.
  2. Make sure you set the Entry point (Function to execute) to `memories-thumbnail-maker` and Trigger to `Cloud Storage`.
  3. Add the following code to the index.js:
```javascript
const functions = require('@google-cloud/functions-framework');
const { Storage } = require('@google-cloud/storage');
const { PubSub } = require('@google-cloud/pubsub');
const sharp = require('sharp');

functions.cloudEvent('memories-thumbnail-maker', async cloudEvent => {
  const event = cloudEvent.data;

  console.log(`Event: ${JSON.stringify(event)}`);
  console.log(`Hello ${event.bucket}`);

  const fileName = event.name;
  const bucketName = event.bucket;
  const size = "64x64";
  const bucket = new Storage().bucket(bucketName);
  const topicName = "topic-memories-548";
  const pubsub = new PubSub();

  if (fileName.search("64x64_thumbnail") === -1) {
    // doesn't have a thumbnail, get the filename extension
    const filename_split = fileName.split('.');
    const filename_ext = filename_split[filename_split.length - 1].toLowerCase();
    const filename_without_ext = fileName.substring(0, fileName.length - filename_ext.length - 1); // fix sub string to remove the dot

    if (filename_ext === 'png' || filename_ext === 'jpg' || filename_ext === 'jpeg') {
      // only support png and jpg at this point
      console.log(`Processing Original: gs://${bucketName}/${fileName}`);
      const gcsObject = bucket.file(fileName);
      const newFilename = `${filename_without_ext}_64x64_thumbnail.${filename_ext}`;
      const gcsNewObject = bucket.file(newFilename);

      try {
        const [buffer] = await gcsObject.download();
        const resizedBuffer = await sharp(buffer)
          .resize(64, 64, {
            fit: 'inside',
            withoutEnlargement: true,
          })
          .toFormat(filename_ext)
          .toBuffer();

        await gcsNewObject.save(resizedBuffer, {
          metadata: {
            contentType: `image/${filename_ext}`,
          },
        });

        console.log(`Success: ${fileName} → ${newFilename}`);

        await pubsub
          .topic(topicName)
          .publishMessage({ data: Buffer.from(newFilename) });

        console.log(`Message published to ${topicName}`);
      } catch (err) {
        console.error(`Error: ${err}`);
      }
    } else {
      console.log(`gs://${bucketName}/${fileName} is not an image I can handle`);
    }
  } else {
    console.log(`gs://${bucketName}/${fileName} already has a thumbnail`);
  }
});
```

  4. Add the following code to the package.json:
```json
{
 "name": "thumbnails",
 "version": "1.0.0",
 "description": "Create Thumbnail of uploaded image",
 "scripts": {
   "start": "node index.js"
 },
 "dependencies": {
   "@google-cloud/functions-framework": "^3.0.0",
   "@google-cloud/pubsub": "^2.0.0",
   "@google-cloud/storage": "^6.11.0",
   "sharp": "^0.32.1"
 },
 "devDependencies": {},
 "engines": {
   "node": ">=4.3.2"
 }
}
```

Note: If you get a permission denied error stating it may take a few minutes before all necessary
permissions are propagated to the Service Agent, wait a few minutes and try again.
Ensure you have the appropriate roles (Eventarc Service Agent, Eventarc Event Receiver,
Service Account Token Creator, and Pub/Sub Publisher) assigned to the correct service accounts.


First Grant roles/pubsub.publisher to the GCS service account
```bash
gcloud projects add-iam-policy-binding $(gcloud config get-value project) \
  --member="serviceAccount:service-978812217160@gs-project-accounts.iam.gserviceaccount.com" \
  --role="roles/pubsub.publisher"
```

Then Deploy
```bash
gcloud functions deploy memories-thumbnail-maker \
  --gen2 \
  --runtime nodejs22 \
  --region us-east4 \
  --trigger-resource qwiklabs-gcp-03-c8adde435dba-bucket \
  --trigger-event google.cloud.storage.object.v1.finalized \
  --entry-point memories-thumbnail-maker
```

then verify
```bash
gcloud functions list --regions=us-east4
```

for function details:
```bash
gcloud functions describe memories-thumbnail-maker --region=us-east4
```

### Test the Function
- Upload a PNG or JPG image to the bucket (qwiklabs-gcp-03-c8adde435dba-bucket)
- Alternatively, download this image: [map.jpg](https://storage.googleapis.com/cloud-training/gsp315/map.jpg) and upload it
- Check the bucket to see the thumbnail appear
> Optional: If trigger does not work, check Triggers tab and recreate the trigger.

Download the example image
```bash
curl -O https://storage.googleapis.com/cloud-training/gsp315/map.jpg
```

Then upload it to your bucket:
```bash
gsutil cp map.jpg gs://qwiklabs-gcp-03-c8adde435dba-bucket/
```

Verify:
```bash
gsutil ls gs://qwiklabs-gcp-03-c8adde435dba-bucket/
```

> image map.jpg successfully uploaded

---

## Task 4: Remove the Previous Cloud Engineer 👤❌
Two users in the project:
- Your account: student-01-830d3af6b9b0@qwiklabs.net (Owner)
- Previous engineer: student-01-8b7195c8d23b@qwiklabs.net (Viewer)
Remove the previous engineer's access.

```bash
gcloud projects remove-iam-policy-binding qwiklabs-gcp-03-c8adde435dba \
  --member="user:student-01-8b7195c8d23b@qwiklabs.net" \
  --role="roles/viewer"
```

Where:
- qwiklabs-gcp-03-c8adde435dba = Project ID


---

## 🎉 Task Completed
- Set Up an App Dev Environment on Google Cloud challenge lab.
