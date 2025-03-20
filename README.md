# AWS S3 (Simple Storage Service)
## **S3 Bucket Creation Process**

1. **Navigating to S3 Dashboard**:
   - After logging into AWS, use the **search bar** to search for "S3" and click the **S3 icon** to go to the S3 dashboard.

2. **Create Your First S3 Bucket**:
   - In the S3 dashboard, click on the **Create Bucket** button to start creating a new bucket.
   
   **Steps to Create the Bucket**:
   - **Region Selection**:
     - S3 is a **regional service**. You need to choose the **region** where you want your bucket to be created (e.g., **Mumbai/Hyderabad**).
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

### **How to Add Object Tags in AWS Console**  
#### **Step-by-Step Guide**  
1. **Access the AWS S3 Console**  
   - Search for **S3** in the AWS Management Console.  
   - Open the **S3 Dashboard** and select your **bucket**.  
2. **Choose an Object to Tag**  
   - Click on the **object** to open its properties.  
3. **Add Tags**  
   - Scroll to the **Tagging** section.  
   - Click **Edit > Add Tag**.  
   - Define a **Key-Value pair** (e.g., `Key: ObjectType, Value: PNG`).  
4. **Save & Verify**  
   - Click **Save Changes** and refresh the page.  
   - The tag will now appear under the **Tagging** section.

---

### S3 Permission and Policies
#### **1. Cross-Account S3 Bucket Access**  
**Scenario**: Allow an IAM user from a secondary AWS account to access an S3 bucket in the root account.  

**Steps & Key Points**:  
1. **Create a Custom Policy in the Root Account**:  
   - **Purpose**: Grant limited permissions (e.g., list buckets) to the external IAM user.  
   - **Actions Taken**:  
     - Navigate to **IAM > Policies > Create Policy**.  
     - Select **S3** as the service.  
     - Specify permissions (e.g., `ListBucket`).  
     - Set resources to `*` (for simplicity, though best practice is to restrict to specific bucket ARNs).  
     - Name the policy (e.g., `S3-List-Policy-mallick-Dev`).  

2. **Attach the Policy to the IAM User**:  
   - **Purpose**: Link the policy to the external user to enforce permissions.  
   - **Actions Taken**:  
     - Go to **IAM > Users > Select Target User**.  
     - Under **Permissions**, attach the newly created policy.  

3. **Verify Access**:  
   - **Outcome**:  
     - The IAM user can now **list the bucket** but **cannot read objects** (e.g., accessing an object URL results in "Access Denied").  
   - **Reason**: The policy only grants `ListBucket`, not `GetObject`.  

---

#### **2. Making an S3 Bucket Public**  
**Scenario**: Allow public internet access to an S3 bucket.  

**Steps & Key Points**:  
1. **Create a New Bucket with Public Access Enabled**:  
   - **Actions Taken**:  
     - Uncheck **"Block all public access"** during bucket creation.  
     - Acknowledge the security warning.  

2. **Configure Bucket Policy for Public Access**:  
   - **Purpose**: Define which actions (e.g., listing, reading) are allowed publicly.  
   - **Actions Taken**:  
     - Use the **Policy Generator** to create a JSON policy.  
     - Set **Principal** to `*` (public).  
     - Specify actions (e.g., `ListBucket`, `GetObject`).  
     - Attach the policy to the bucket.  

3. **Test Public Access**:  
   - **Outcome**:  
     - Users can **list bucket contents** via the bucket URL.  
     - Direct object access is **denied** unless explicitly allowed in the policy.  
   - **Security Note**: Public buckets pose risks; only enable this if necessary.  

---

#### **3. Pre-Signed URLs**  
**Scenario**: Generate temporary, time-limited access to an S3 object.  

**Steps & Key Points**:  
1. **Generate a Pre-Signed URL**:  
   - **Purpose**: Share secure, time-bound access without making the bucket public.  
   - **Actions Taken**:  
     - Select an object in the S3 bucket.  
     - Click **"Share" > "Create pre-signed URL"**.  
     - Set expiration time (e.g., 5 minutes).  
     - Copy the generated URL.  

2. **Test the URL**:  
   - **Outcome**:  
     - The URL works **until the expiration time**.  
     - After expiration, access is denied.  
   - **Use Case**: Ideal for sharing sensitive files temporarily (e.g., invoices, reports).  

---

### **Key Takeaways**  
- **Cross-Account Access**:  
  - Policies define granular permissions (e.g., list vs. read).  
  - Attach policies to IAM users/roles to enforce access controls.  

- **Public Buckets**:  
  - Disabling "Block public access" is required but risky.  
  - Use bucket policies to limit actions (e.g., read-only for static websites).  

- **Pre-Signed URLs**:  
  - Time-bound access enhances security.  
  - No need to expose the entire bucket publicly.  

---

### **Common Issues & Fixes**  
1. **"Access Denied" for Objects**:  
   - **Cause**: Missing `GetObject` permission in the policy.  
   - **Fix**: Update the policy to include `s3:GetObject`.  

2. **Policy Generation Errors**:  
   - **Tip**: Use the **Policy Generator** for syntax help.  
   - Validate JSON with AWS tools to avoid typos.  

3. **Pre-Signed URL Expiry**:  
   - **Reminder**: Set appropriate expiration times based on use case (e.g., 15 minutes for temporary downloads).  
     
---

## S3 Bucket Versioning

### **Concept**
Bucket versioning maintains multiple versions of an object, enabling data recovery if an object is deleted or modified.

### **Enabling Versioning on an Existing Bucket**
1. Go to **AWS S3 Dashboard**.
2. Select your bucket and navigate to **Properties**.
3. Click **Edit** under **Bucket versioning**.
4. Toggle **Enable**.
5. Click **Save Changes**.

### **Verifying Object Versioning**
1. Upload an object to the bucket.
2. Modify and upload the object again with the same name.
3. Click on the object and check the **Versions** tab.
4. You will see multiple versions of the object.

---

## S3 Bucket Replication

### **Concept**
Bucket replication copies objects from a source bucket to a destination bucket in another AWS region.

### **Steps to Enable Replication**
1. Create a **destination bucket** in another region.
2. Go to **AWS S3 Dashboard** and select the **source bucket**.
3. Navigate to **Management** and click **Create replication rule**.
4. Provide a **Rule name** and select the **Destination bucket**.
5. Choose an existing **IAM role** or create a new one for replication.
6. Click **Save rule**.
7. Now, all new objects uploaded to the source bucket will be replicated to the destination bucket.

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
     - **Region**: Specify the region you are working in (e.g., `ap-south-1` for Mumbai).

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

### **Steps to Move Objects Using Tags in AWS CLI**  
#### **Scenario**  
- **Account 1 (Source Bucket)** contains multiple objects.  
- **Account 2 (Destination Bucket)** should receive only objects tagged as `ProjectX`. 
1. **Apply Tags to Objects in the Source Bucket**  
   - Assign a tag like `ProjectX` to the required objects.  
2. **List Objects with Specific Tags Using AWS CLI**  
   ```sh
   aws s3api list-objects --bucket SourceBucket --query "Contents[?Tagging=='ProjectX']"
   ```  
3. **Copy Tagged Objects to Destination Bucket**  
   ```sh
   aws s3 cp s3://SourceBucket s3://DestinationBucket --recursive --exclude "*" --include "Tagging=='ProjectX'"
   ```  
4. **Verify the Object Transfer in the Destination Bucket**.

---

### **Summary of Key Operations**

- **AWS CLI Setup**: Install, configure, and authenticate AWS CLI.
- **Basic S3 Commands**: List buckets (`aws s3 ls`) and view contents (`aws s3 ls s3://bucket-name`).
- **Advanced Operations**: Move (`aws s3 mv`) or copy objects between buckets.
- **Recursive Listing**: Use `--recursive` to list all objects in a bucket, including those in subfolders.


