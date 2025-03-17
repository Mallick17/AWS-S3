# AWS S3 (Simple Storage Service)

- **AWS S3 Overview**:
  - **S3** stands for **Simple Storage Service**. It's a cloud service provided by AWS for storing files.
  - It's similar to personal cloud storage services like **Google Drive**, **Dropbox**, or **iCloud**, where you can store photos, videos, and other files.
  - However, S3 is designed to store application-related data, such as text files, CSV files, JSON files, videos, and more.

- **Key Point**: 
  - S3 allows you to **store and retrieve** any type of file in the cloud.
  - It's a scalable, secure, and highly durable storage solution.

---

### **AWS S3 Key Concepts**

- **S3 Buckets**:
  - You create a bucket to store objects in S3.
  - The bucket name must be unique across all of AWS.
  
- **S3 Objects**:
  - Files are stored as **objects** (not regular files) in the bucket.
  - Each object is assigned a unique reference (ID).

- **Access Control**:
  - Permissions for objects can be set individually.
  - **Block Public Access** ensures the bucket isn’t publicly accessible.

- **Versioning**:
  - Versioning allows you to store multiple versions of an object, protecting against accidental deletion or modification.

- **Encryption**:
  - You can enable **server-side encryption** to secure objects stored in S3.

---

### **AWS S3: Object-based Storage**

1. **Object vs. File**:
   - S3 stores files as **objects** rather than regular files. 
   - Each file stored in an S3 bucket is considered an **object** with a **unique reference ID**.
   - These objects can be individual files of any type: text files, images, videos, CSVs, etc.

2. **Granular Access Control**:
   - You can control permissions for each individual object:
     - **Read**, **Write**, and **Update** permissions.
   - S3 allows **versioning**, so if an object is modified, you can keep the older version and manage revisions.

---

### **S3 Bucket Creation Process**

<details>
  <summary>S3 Bucket Creation</summary>

1. **Navigating to S3 Dashboard**:
   - After logging into AWS, use the **search bar** to search for "S3" and click the **S3 icon** to go to the S3 dashboard.

2. **Create Your First S3 Bucket**:
   - In the S3 dashboard, click on the **Create Bucket** button to start creating a new bucket.
   
   **Steps to Create the Bucket**:
   - **Region Selection**:
     - S3 is a **regional service**. You need to choose the **region** where you want your bucket to be created (e.g., **Europe/Ireland**).
     - Choose the **nearest region** to your location for better performance.
   - **Unique Bucket Name**:
     - The bucket name must be **globally unique**. AWS will not allow you to create a bucket if the name is already taken.
     - Example: If you try "test-bucket", it will show an error message saying the name already exists.

     - **Tip**: To ensure a unique name, add a suffix like a **date** or your **website name** (e.g., `test-bucket-demo-30Aug`).

   - **Bucket Configuration**:
     - **ACL (Access Control List)**: By default, this option is disabled. This means no additional granular access control is set initially.
     - **Block Public Access**: 
       - It’s crucial to block **public access** to your bucket. This ensures that no one can access your bucket unless you allow it.
       - The "Block all public access" option should be checked to keep the bucket private.
     - **Versioning**: Versioning is not enabled in this example but can be enabled later for file management.
     - **Encryption**:
       - To keep objects secure, enable **server-side encryption** (SSE).
       - Encryption can be customized using **KMS keys** (Key Management Service), but this will be covered in a later session.

3. **Finalizing the Bucket Creation**:
   - After configuring your options, click the **Create** button. AWS will confirm that your bucket has been successfully created.
   - You can now see your newly created bucket in the S3 dashboard.
  
</details>

### **Uploading Objects to S3 Bucket**
<details>
  <summary>Uploading Objects to S3 Bucket</summary>

1. **Navigating to the Bucket**:
   - In the S3 dashboard, select your bucket to view and manage its contents.
   - If the bucket is empty, you will see no objects inside it.

2. **Uploading Files**:
   - To upload files, click on the **Upload** button within the bucket's object browser.
   - You can either **drag files** into the browser or click **Add files** to choose files from your computer.

3. **Steps to Upload**:
   - **Select Files**: Choose files (e.g., images, text files) to upload.
     - Example: Choose two image files (e.g., `object1.png`, `object2.png`) from your computer.
   - **Click Open**: The selected files will appear in the upload dialog.
   - **Upload**: After selecting the files, click the **Upload** button.
     - Upload speed depends on the **file size** and your **internet bandwidth**.

4. **Post-Upload**:
   - Once the files are uploaded, they will appear in the **object browser** of your bucket.
   - You can now manage and access the objects via their unique reference IDs (URLs).
  
</details>

---


  
