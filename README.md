# AWS S3 (Simple Storage Service)

- **AWS S3 Overview**:
  - **S3** stands for **Simple Storage Service**. It's a cloud service provided by AWS for storing files.
  - It's similar to personal cloud storage services like **Google Drive**, **Dropbox**, or **iCloud**, where you can store photos, videos, and other files.
  - However, S3 is designed to store application-related data, such as text files, CSV files, JSON files, videos, and more.

- **Key Point**: 
  - S3 allows you to **store and retrieve** any type of file in the cloud.
  - It's a scalable, secure, and highly durable storage solution.

---

## **AWS S3 Key Concepts**

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

## **AWS S3: Object-based Storage**

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

## **Scalability and Durability of S3 Buckets**

1. **Availability of Data**:
   - **AWS S3 Promise**: AWS promises **99.99% availability**, which is achieved with **11 nines** (99.999999999%) behind that decimal.
   - This means the data you store in S3 is **highly available** when you need it.
   - **Availability**: S3 guarantees 99.99% availability by storing your data across multiple availability zones.

2. **How AWS Ensures Data Availability**:
   - **Multiple Availability Zones**:
     - AWS stores your data across **three different availability zones** (AZs) within a region.
     - In the **Frankfurt region** (as an example), your data is distributed across three separate data centers:
       - **Availability Zone 1**
       - **Availability Zone 2**
       - **Availability Zone 3**
     - **Redundancy**: This setup ensures that even if one data center or AZ goes down, your data is still accessible from the other two zones.
     - **Result**: This architecture ensures **high availability** and durability of your data.

---

## **S3 Bucket Rules and Policies**

1. **Unique Bucket Name**:
   - **Bucket Name Uniqueness**: 
     - S3 bucket names must be **globally unique**. If the name you want is already taken, you must come up with a different one.
     - Even though S3 is a regional service, the bucket name must be unique across all AWS accounts globally.

2. **Buckets are Region-Specific**:
   - **Regional Resource**: Once you create a bucket, it is **associated with a specific region** (e.g., Frankfurt region).
   - **Bucket Creation**: If you want to create a bucket in a different region, you must choose that region before creating the bucket.
   - Example: If you created a bucket in **Frankfurt**, it is tied to that specific region.

3. **Object Size Limit**:
   - **Maximum Object Size**: 
     - Each object uploaded to S3 can be up to **5 terabytes (TB)** in size.
     - This means you can store large files, such as videos or databases, as a single object in S3.
   - **Multiple Large Objects**: You can upload **multiple objects** of up to 5 TB each in your S3 bucket.
  
---
## **AWS S3 Storage Classes**  

| Storage Class | Use Case | Durability | Availability | Retrieval Time | Cost |
|--------------|----------|------------|--------------|---------------|------|
| **S3 Standard** | Frequently accessed data (e.g., dynamic web apps, databases) | 99.999999999% (11 nines) | 99.99% | Immediate | High |
| **S3 Intelligent-Tiering** | Unpredictable access patterns | 11 nines | 99.9% | Immediate | Moderate |
| **S3 Standard-IA (Infrequent Access)** | Infrequently accessed data, but requires fast retrieval | 11 nines | 99.9% | Immediate | Lower than Standard |
| **S3 One Zone-IA** | Infrequently accessed data stored in a single Availability Zone | 11 nines | 99.5% | Immediate | Lower than Standard-IA |
| **S3 Glacier Instant Retrieval** | Archive data with immediate access | 11 nines | 99.9% | Immediate | Very Low |
| **S3 Glacier Flexible Retrieval** | Long-term archiving, minutes to hours retrieval | 11 nines | 99.9% | 1 minute to 12 hours | Very Low |
| **S3 Glacier Deep Archive** | Cheapest option for archival data; retrieval takes hours | 11 nines | 99.9% | 12–48 hours | Lowest |

---

## **S3 Storage Pricing and Cost Optimization**  
- **S3 Standard is the most expensive** because it supports frequent access and high durability.  
- **S3 Glacier & Deep Archive are the cheapest** as they are designed for archival data.  
- **Graphical Cost Comparison:**  
  - **Left Side (Higher Cost):** S3 Standard, Standard-IA.  
  - **Right Side (Lower Cost):** Glacier, Glacier Flexible Retrieval, Deep Archive.

### **AWS S3 Cost Calculator**  
AWS provides an official **S3 Cost Calculator** to estimate costs based on:  
- **Storage Class Selection** (Standard, IA, Glacier, etc.).  
- **Data Volume (in GB or TB).**  
- **Operations Performed** (PUT, COPY, DELETE).  
- **Data Transfer Costs.**  

### **Example Cost Comparison:**  
- **S3 Standard (1,000GB/month):** ≈ **$23/month**  
- **S3 Glacier Instant Retrieval (1,000GB/month):** ≈ **$4/month**  

This shows a significant cost reduction when using Glacier for archival data.

### **AWS S3 Requester Pays Feature**  
- **Concept:** Normally, the bucket owner pays for data transfer costs.  
- **Requester Pays Option:** The **user accessing the data (requester) pays the retrieval cost.**  
- **Use Case:** If an external user needs access to your S3 data, they bear the cost of data transfer.
- **Benefit:** The bucket owner only pays for **storage**, while the requester bears retrieval costs.
  
---

## **AWS S3 Object Tagging**   
- Amazon S3 allows storing objects up to **5TB** in size.  
- Each object in S3 is uniquely identified by AWS, but **custom tags** can be assigned.  
- Object tagging is useful for **organizing, managing, and categorizing** objects in an S3 bucket using **key-value pairs**.
- Tags are **key-value pairs** that help in grouping and filtering objects.   

### **2. Why Use Object Tagging?**  
- **Efficient Object Management**: Helps categorize and organize objects.  
- **Facilitates Data Movement**: Move specific objects between buckets using tags.  
- **Reporting & Cost Allocation**: Track storage costs based on tags.  
- **Security & Compliance**: Apply access policies based on tags.  
- **Automation**: Helps in lifecycle policies and workflow management.  


### **3. Object Tagging Features**  
- **Maximum of 10 tags per object.**  
- **Key-Value Format:** Example: `Key: ObjectType, Value: PNG`.  
- **Editable Anytime:** Tags can be added, modified, or deleted.  
- **Supports AWS Services:** Used in AWS Lambda, S3 Lifecycle rules, Cost Allocation, etc.
- - **Supports filtering, cost tracking, automation, and access control**.    


### **4. Practical Use Case: Moving Tagged Objects Between Buckets**  
#### **Scenario**  
- **Account 1 (Source Bucket)** contains multiple objects.  
- **Account 2 (Destination Bucket)** should receive only objects tagged as `ProjectX`.  

<details>
  <summary>Steps to Move Objects Using Tags</summary>

#### **Steps to Move Objects Using Tags**  
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

</details>

### **5. Real-World Applications of Object Tagging**  
✔ **Data Categorization**: Assign tags like `JPEG`, `CSV`, `LOG` for easy identification.  
✔ **Cost Management**: AWS billing can track costs based on object tags.  
✔ **Security Policies**: Restrict access based on tags in IAM policies.  
✔ **Automated Workflows**: S3 Lifecycle rules can archive or delete objects based on tags.  
✔ **Auditing & Compliance**: Helps track and manage sensitive data. 

---

## S3 Permission and Policies
#### **AWS S3 Bucket Policies**  
S3 bucket policies are **JSON-based access control rules** attached directly to an S3 bucket. They define **who** (principals) can perform **what actions** (e.g., `s3:GetObject`, `s3:ListBucket`) on **which resources** (objects or buckets).  

**Key Components**:  
- **Principal**: The entity (user, account, service) allowed or denied access (e.g., `"Principal": "*"` for public access).  
- **Action**: The specific S3 operations permitted (e.g., `s3:ListBucket`, `s3:PutObject`).  
- **Resource**: The Amazon Resource Name (ARN) of the bucket or objects the policy applies to (e.g., `arn:aws:s3:::bucket-name/*`).  
- **Effect**: `Allow` or `Deny` to grant or restrict access.  

**How It Works**:  
- Policies enforce **least-privilege access**, ensuring only explicitly allowed actions are permitted.  
- They override **public access block settings** if configured (e.g., enabling public access despite account-level restrictions).  

**Use Cases**:  
- Grant cross-account access.  
- Restrict object access to specific IP ranges.  
- Enforce encryption requirements.  

---

#### **1. Cross-Account S3 Bucket Access**  
- Allowing users or roles from **one AWS account** to access resources (e.g., S3 buckets) in **another AWS account**.  

**Mechanism**:  
1. **Policy Creation**:  
   - The **resource owner** (root account) creates a bucket policy specifying the external account as the `Principal`.  
   - Example: Allow `s3:ListBucket` for `arn:aws:iam::ExternalAccountID:user/username`.  
2. **IAM Policy Attachment**:  
   - The **external account** attaches an IAM policy to its user/role, granting permissions to access the bucket (e.g., `s3:ListBucket`).  

**Security Considerations**:  
- Use explicit ARNs for resources (avoid `"Resource": "*"`).  
- Combine with IAM roles for temporary credentials (e.g., AWS STS).  
**Scenario**: Allow an IAM user from a secondary AWS account to access an S3 bucket in the root account.  

<details>
  <summary>Steps & Key Points</summary>

**Steps & Key Points**:  
1. **Create a Custom Policy in the Root Account**:  
   - **Purpose**: Grant limited permissions (e.g., list buckets) to the external IAM user.  
   - **Actions Taken**:  
     - Navigate to **IAM > Policies > Create Policy**.  
     - Select **S3** as the service.  
     - Specify permissions (e.g., `ListBucket`).  
     - Set resources to `*` (for simplicity, though best practice is to restrict to specific bucket ARNs).  
     - Name the policy (e.g., `S3-List-Policy-Rahul-Dev`).  

2. **Attach the Policy to the IAM User**:  
   - **Purpose**: Link the policy to the external user to enforce permissions.  
   - **Actions Taken**:  
     - Go to **IAM > Users > Select Target User**.  
     - Under **Permissions**, attach the newly created policy.  

3. **Verify Access**:  
   - **Outcome**:  
     - The IAM user can now **list the bucket** but **cannot read objects** (e.g., accessing an object URL results in "Access Denied").  
   - **Reason**: The policy only grants `ListBucket`, not `GetObject`.  

</details>

---

#### **2. Making an S3 Bucket Public**  
- An S3 bucket configured to allow **public internet access** to its objects or metadata.  

**Configuration Steps**:  
1. **Disable Public Access Block**:  
   - Uncheck **"Block all public access"** during bucket creation or via permissions.  
2. **Attach a Public Bucket Policy**:  
   - Define a policy with `"Principal": "*"` and allowed actions (e.g., `s3:GetObject`).  

**Risks**:  
- **Data Exposure**: Publicly accessible buckets risk accidental data leaks (e.g., sensitive files).  
- **Compliance Violations**: May breach regulations like GDPR or HIPAA.  

**Use Case**:  
- Host static websites or public assets (e.g., images, documents).  

**Scenario**: Allow public internet access to an S3 bucket.  

<details>
  <summary>Steps & Key Points</summary>

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

</details>

---

#### **3. Pre-Signed URLs**
Time-limited URLs that grant **temporary access** to private S3 objects. Generated using AWS credentials (e.g., IAM user/role).  

**How It Works**:  
1. **URL Generation**:  
   - Uses cryptographic signing with AWS credentials to create a URL with an expiration time (e.g., 5 minutes).  
   - Example: `https://bucket.s3.amazonaws.com/object?AWSAccessKeyId=...&Signature=...&Expires=...`.  
2. **Access Validation**:  
   - AWS validates the signature and expiration time before granting access.  

**Advantages**:  
- No need to expose the entire bucket publicly.  
- Ideal for sharing sensitive data securely (e.g., invoices, reports).  

**Limitations**:  
- URL validity is **time-bound** (e.g., 1 hour to 7 days).  
- Revocation is impossible once generated (must wait for expiration).  
  
**Scenario**: Generate temporary, time-limited access to an S3 object.  

<details>
  <summary>Steps & Key Points</summary>
  
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

</details>

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


