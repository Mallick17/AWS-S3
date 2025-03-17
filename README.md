# AWS S3 (Simple Storage Service)
## **S3 Bucket Creation Process**

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

### **Uploading Objects to S3 Bucket**

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
---

## **AWS S3 Requester Pays Feature**  
- **Concept:** Normally, the bucket owner pays for data transfer costs.  
- **Requester Pays Option:** The **user accessing the data (requester) pays the retrieval cost.**  
- **Use Case:** If an external user needs access to your S3 data, they bear the cost of data transfer.  

#### **How to Enable Requester Pays in AWS Console:**  
1. Go to **AWS S3 Console** → Select the **Bucket**.  
2. Navigate to **Properties** → Scroll to **Requester Pays**.  
3. Click **Edit**, select **Enable**, and save changes. 
  
---

# Accessing S3 using AWS CLI

### **Setting Up AWS CLI for Accessing S3 Buckets**

1. **Install AWS CLI**:
   - Go to the [AWS CLI official website](https://aws.amazon.com/cli/) to download the appropriate version for your operating system (Windows, MacOS, Linux).
   - Once installed, verify the installation by running the command:
     ```
     aws --version
     ```

2. **Configure AWS CLI**:
   - To authenticate with your AWS account, use the following command:
     ```
     aws configure
     ```
   - Enter the following details:
     - **Access Key**: Obtain this from your AWS account under "Security Credentials."
     - **Secret Key**: This is generated alongside the Access Key.
     - **Region**: Specify the region you are working in (e.g., `eu-central-1` for Frankfurt).

3. **Verify Configuration**:
   - To ensure the AWS CLI is configured correctly, use the following command to check the current user and account:
     ```
     aws sts get-caller-identity
     ```

---

### **Basic AWS CLI Commands for S3**

1. **List All S3 Buckets**:
   - Use the `aws s3 ls` command to list all the S3 buckets in your AWS account:
     ```
     aws s3 ls
     ```

2. **List the Contents of a Specific Bucket**:
   - To view the contents inside a specific S3 bucket, use the following command:
     ```
     aws s3 ls s3://<bucket-name>
     ```
   - Example:
     ```
     aws s3 ls s3://test-bucket-demo-jook-30-august
     ```

---

### **Advanced AWS CLI Commands for S3 Operations**

1. **Move or Copy an Object Between Buckets**:
   - **Move an Object**: The command `aws s3 mv` is used to move an object from one bucket to another.
     - Syntax:
       ```
       aws s3 mv s3://<source-bucket>/<object-name> s3://<destination-bucket>/<object-name>
       ```
     - Example:
       ```
       aws s3 mv s3://test-bucket-demo-jook-30-august/object2 s3://destination-bucket/object2
       ```

2. **Verify Object Movement**:
   - After performing the move, check the **destination bucket** to ensure the object has been transferred.
   - You can refresh the AWS S3 GUI to see the moved object listed in the new bucket.

---

### **Listing All Objects Recursively Within a Bucket**

1. **List All Content Recursively**:
   - To list all objects, including those in subfolders within the bucket, use the `--recursive` option.
   - Syntax:
     ```
     aws s3 ls s3://<bucket-name> --recursive
     ```
   - Example:
     ```
     aws s3 ls s3://my-s3-bucket --recursive
     ```
   - This command will list not only the direct objects but also those inside subfolders.

---

### **Summary of Key Operations**

- **AWS CLI Setup**: Install, configure, and authenticate AWS CLI.
- **Basic S3 Commands**: List buckets (`aws s3 ls`) and view contents (`aws s3 ls s3://bucket-name`).
- **Advanced Operations**: Move (`aws s3 mv`) or copy objects between buckets.
- **Recursive Listing**: Use `--recursive` to list all objects in a bucket, including those in subfolders.


