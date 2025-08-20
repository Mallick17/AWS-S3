# AWS S3 CLI
### S3 CLI Sync Commands
- 1st Create a folder inside a bucket and
- 2nd Create a folder and add some files
- 3rd Copied 2 files `dump.py , user.env` files from the local folder to s3 bucket folder

<img width="1204" height="321" alt="image" src="https://github.com/user-attachments/assets/04bb6965-ea7a-4352-af84-58c9e36eb437" />

- 4th modified the `dump.py` file and didnt change the `uesr.env` file and gonna try the sync command and there are few other files in the local folder which isnt synced.
```shell
pwd
/Users/gyanaranjan.mallick/Downloads/testing/test

aws s3 sync . s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/
upload: ./dump.py to s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/dump.py
upload: ./dump1.py to s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/dump1.py
upload: ./user-laravel.env to s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/user-laravel.env
```
<img width="1189" height="392" alt="image" src="https://github.com/user-attachments/assets/f15b490c-ba8f-49e2-8caa-6c8bac014251" />

- `aws s3 sync . s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/` it overwrites the modified files and adds the misisng files which is not present in s3 bucket after comparing the local folder.

Absolutely! Let’s clarify **why `aws s3 sync` cannot avoid overwriting modified files**, and then integrate that explanation into a clean, cohesive documentation.

---

## Why `aws s3 sync` Can’t Avoid Overwriting Modified Local Files

* **Designed as a one-way sync**, `aws s3 sync` prioritizes the source (local) version. If a local file is newer or different, it *always* overwrites the existing object in S3 ([Stack Overflow][1]).

* It determines whether to upload primarily based on three criteria:

  1. The local file doesn’t exist in S3.
  2. The file size differs.
  3. The local file is newer than the S3 version (based on timestamps) ([Zenduty Community][2], [awsfundamentals.com][3]).

* **There is no option** in `aws s3 sync` to say “skip overwriting if the file exists, even if it’s different.” The CLI offers no flag like `--ignore-existing` or `--no-overwrite` for `sync` ([Stack Overflow][1]).

In short—**if `sync` sees a difference, it will overwrite**, with *no built-in method to change that behavior*.

---

## Complete Documentation

### Scenario Steps

1. **Create a folder inside the S3 bucket**

   ```bash
   aws s3api put-object --bucket elasticbeanstalk-ap-south-1-508351649560 --key sync-commands-test/
   ```

2. **Create a local folder and add files**

   ```bash
   mkdir -p ~/Downloads/testing/test
   cd ~/Downloads/testing/test

   echo "print('hello world')" > dump.py
   echo "APP_ENV=local" > user.env
   echo "print('second file')" > dump1.py
   echo "APP_ENV=laravel" > user-laravel.env
   ```

3. **Copy two files to S3 using `cp`**

   ```bash
   aws s3 cp dump.py s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/
   aws s3 cp user.env s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/
   ```

4. **Modify one file locally**

   ```bash
   echo "print('modified content')" > dump.py
   ```

5. **Run `sync`**

   ```bash
   aws s3 sync . s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/
   ```

   Output:

   ```
   upload: ./dump.py to s3://…/dump.py
   upload: ./dump1.py to s3://…/dump1.py
   upload: ./user-laravel.env to s3://…/user-laravel.env
   ```

6. **What Happened**

   * `dump.py`: Overwritten in S3 (because it's changed locally).
   * `user.env`: Left untouched (unchanged locally).
   * `dump1.py` & `user-laravel.env`: Newly uploaded (missing in S3).

---

### Feature Comparison Table

| Desired Behavior                   | `aws s3 sync` Result   | Alternative (`aws s3 cp --ignore-existing`) |
| ---------------------------------- | ---------------------- | ------------------------------------------- |
| Upload missing files only          | Yes                    | Yes                                         |
| Skip existing but modified files   | No (overwrites)        | Yes (skips)                                 |
| Full sync (missing + changed)      | Yes (default behavior) | No (doesn't overwrite)                      |
| Built-in flag to prevent overwrite | No                     | Yes (`--ignore-existing`)                   |

---

### Why `sync` Overwrites Modified Local Files

* `aws s3 sync` is built to ensure the destination mirrors the source; if a local file is different (by size or timestamp), it assumes the local copy is authoritative and deliberately overwrites ([Zenduty Community][2], [awsfundamentals.com][3]).

* The CLI offers no switch to reverse that logic (e.g., “never overwrite”), meaning **you cannot use `sync` if you want to only upload missing files and leave modified ones untouched** ([Stack Overflow][1]).

---

### Final Recommendation

Use `aws s3 sync` when you want the bucket to fully reflect your local folder—missing files are uploaded, changed files are updated.

But if your goal is *only to upload missing files and leave any modified local files alone*, you should use:

```bash
aws s3 cp . s3://elasticbeanstalk-ap-south-1-508351649560/sync-commands-test/ --recursive --ignore-existing
```

---






# AWS S3 (Simple Storage Service)

- **AWS S3 Overview**:
  - **S3** stands for **Simple Storage Service**. It's a cloud service provided by AWS for storing files.
  - It's similar to personal cloud storage services like **Google Drive**, **Dropbox**, or **iCloud**, where you can store photos, videos, and other files.
  - However, S3 is designed to store application-related data, such as text files, CSV files, JSON files, videos, and more.

- **Key Point**: 
  - S3 allows you to **store and retrieve** any type of file in the cloud.
  - It's a scalable, secure, and highly durable storage solution.

---

## Features of S3
<details>
    <summary>Storage Management and Monitoring</summary>
    
Amazon S3 provides a flat, non-hierarchical structure where all objects are stored in buckets. To organize data, you can use shared names called prefixes and assign up to 10 key-value pairs known as object tags to each object. These tags can be created, updated, or deleted throughout an object's lifecycle. For tracking purposes, S3 Inventory reports list stored objects within a bucket or specific prefix, along with their metadata and encryption status. These reports can be generated daily or weekly. 
    
</details>

<details>
    <summary>Storage Classes</summary>

Amazon S3 offers various storage classes tailored to different access patterns and cost requirements:

- **S3 Standard**: Ideal for frequently accessed data, offering low latency and high throughput.

- **S3 Intelligent-Tiering**: Automatically moves data between two access tiers (frequent and infrequent) based on changing access patterns, optimizing costs.

- **S3 Standard-Infrequent Access (Standard-IA)**: Suitable for data accessed less frequently but requiring rapid access when needed.

- **S3 One Zone-Infrequent Access (One Zone-IA)**: Similar to Standard-IA but stores data in a single availability zone, offering a lower-cost option for infrequently accessed data.

- **S3 Glacier Instant Retrieval**: Designed for rarely accessed data that requires immediate retrieval.

- **S3 Glacier Flexible Retrieval**: Offers low-cost storage for archival data with retrieval times ranging from minutes to hours.

- **S3 Glacier Deep Archive**: The lowest-cost storage class for data that is rarely accessed and requires retrieval times of up to 12 hours.

Each storage class is designed to provide different levels of durability, availability, and performance to help optimize costs based on data access needs. 

</details>

<details>
    <summary>Access Management and Security</summary>

Amazon S3 offers robust security features to protect data:

- **Access Control**: Utilize AWS Identity and Access Management (IAM) policies, bucket policies, and Access Control Lists (ACLs) to manage access to buckets and objects.

- **Encryption**: Supports both server-side encryption (SSE) and client-side encryption to protect data at rest.

- **Bucket Versioning**: Enables tracking of changes to objects over time, allowing for recovery from unintended actions or failures.

- **Object Lock**: Prevents objects from being deleted or overwritten for a specified retention period, supporting regulatory compliance and data protection.

These features ensure that data stored in S3 is secure and meets various compliance requirements. 

</details>


<details>
    <summary>Data Processing</summary>

Amazon S3 integrates with other AWS services to facilitate data processing:

- **S3 Object Lambda**: Allows you to add your own code to process data retrieved from S3 before returning it to an application, enabling on-the-fly transformation of data without creating additional copies.

- **S3 Batch Operations**: Enables the execution of bulk operations like copying or tagging across billions of objects with a single request, simplifying large-scale data management tasks.

These capabilities streamline data processing workflows and reduce the need for additional infrastructure. 

</details>

<details>
    <summary>Query in Place</summary>

Amazon S3 provides features that allow you to run queries directly on data stored in S3 without moving it to a separate analytics platform:

- **S3 Select**: Retrieves subsets of data from within an object using simple SQL expressions, reducing the amount of data transferred and accelerating application performance.

- **Amazon Athena**: An interactive query service that allows you to analyze data in S3 using standard SQL, with no need for complex ETL processes.

These features enable efficient data analysis and reduce the time and cost associated with data movement. 

</details>


<details>
    <summary>Data Transfer</summary>

Amazon S3 offers multiple options to facilitate data transfer:

- **AWS DataSync**: Automates moving large amounts of data between on-premises storage and S3, simplifying data migration and replication.

- **AWS Snow Family**: Provides physical devices to transfer large datasets into and out of AWS when network bandwidth is limited.

- **S3 Transfer Acceleration**: Speeds up content uploads to S3 by using Amazon CloudFront's globally distributed edge locations.

These options cater to various data transfer requirements, ensuring efficient and secure data movement. 

</details>


<details>
    <summary>Performance</summary>

Amazon S3 is designed for high performance, supporting parallel requests and offering features like multipart upload to optimize the upload of large objects. It also integrates with Amazon CloudFront to deliver content globally with low latency.

</details>

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

</details>

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

## Securing Your S3 Bucket 
Amazon S3 (Simple Storage Service) provides various encryption options to protect data at rest. The keys used for encryption in S3 determine how the encryption and decryption processes are managed. Below are the encryption types in detail:

### **1. Bucket Versioning**   
- **What**: Saves **multiple versions** of an object (like a "time machine" for files).  
- **Why**: Recovers accidentally deleted/overwritten files (e.g., restoring a previous report draft).  

<details>
  <summary>Steps to Create</summary>

**Steps**:  
1. **Enable Versioning**:  
   - Go to **S3 > Bucket > Properties > Bucket Versioning > Edit > Enable**.  
2. **Upload New Versions**:  
   - Upload a file with the same name as an existing object.  
3. **View Versions**:  
   - Click the object > **Versions** tab to see all historical copies.  

**Example**:  
- If you overwrite `report.pdf`, the old version stays hidden but recoverable.  

</details>


### **2. Cross-Bucket Replication**  
- **What**: Automatically copies objects from a **source bucket** to a **destination bucket**.  
- **Why**: Backup, compliance, or low-latency access in different regions (like having a photocopy in another location).  

<details>
  <summary>Steps to Create</summary>

**Steps**:  
1. **Prerequisites**:  
   - Enable **versioning** on *both* source and destination buckets.  
2. **Create Replication Rule**:  
   - Go to **Source Bucket > Management > Replication Rules > Create Rule**.  
   - Configure:  
     - **Source/Destination Buckets**: Select buckets.  
     - **IAM Role**: Let AWS create a new role for replication permissions.  
     - **Sync Existing Objects**: Enable to replicate all current files.  
3. **Verify**:  
   - Upload a file to the source bucket. It will auto-copy to the destination.  

**Example**:  
- Replicating `customer-data/` to a backup bucket ensures disaster recovery.  

</details>



