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

### **1. SSE-S3 (Server-Side Encryption with Amazon S3-Managed Keys)**  
- **Overview:**  
S3 automatically encrypts data using **AES-256** encryption before saving it to the disk and decrypts it when accessed. The encryption keys are managed by AWS, and users do not need to handle them.

- **How It Works:**  
  - When an object is uploaded, S3 automatically encrypts it using an encryption key generated and managed by Amazon S3.
  - When the object is retrieved, S3 decrypts it automatically.

- **Key Management:**  
  - Amazon S3 manages the encryption keys internally.
  - No additional configuration is required.

- **Use Case:**  
  - Suitable for users who need basic encryption without managing keys.

- **Limitations:**  
  - Users do not have control over encryption keys.
  - No fine-grained access control over key usage.
  
<details>
  <summary>Steps to Create</summary>
  
**Steps**:  
1. **For Existing Buckets**:  
   - Go to **S3 > Bucket > Properties > Default Encryption > Edit**.  
   - Select **SSE-S3** (Amazon S3-managed keys).  
2. **For New Buckets**:  
   - During bucket creation, under **Default Encryption**, select **SSE-S3**.  

**Example**:  
- The user navigates to the bucket properties, clicks **Edit** under **Default Encryption**, and selects **SSE-S3** to enable encryption for an existing bucket.  

</details>

**Key Points**:  
- No need to create or manage keys.  
- AWS handles key rotation and security.  
- Suitable for general use cases where key management is not a concern.  


### **2. SSE-KMS (Server-Side Encryption with AWS Key Management Service)**   
- **Overview:**  
S3 encrypts objects using **AWS KMS (Key Management Service)**, allowing more control over encryption keys, including key rotation and access management.

- **How It Works:**  
  - When an object is uploaded, S3 encrypts it using a customer-managed KMS key.
  - The key is stored in AWS KMS and requires permissions to use.
  - AWS KMS logs all key usage in **AWS CloudTrail** for auditing.

- **Key Management:**  
  - Users can choose an **AWS-managed KMS key (default)** or a **customer-managed KMS key (CMK)**.
  - Allows fine-grained access control with **IAM policies** and **key policies**.
  - Supports key rotation.

- **Use Case:**  
  - Suitable for organizations that need better security control and audit logging.
  - Required for compliance with security policies.

- **Limitations:**  
  - Additional cost for AWS KMS key usage.
  - More complex than SSE-S3 due to key permissions.  

<details>
  <summary>Steps to Create</summary>

**Steps**:  
1. **Create a KMS Key**:  
   - Go to **AWS KMS > Create Key**.  
   - Define key type (symmetric/asymmetric), usage (encrypt/decrypt), and permissions.  
2. **Enable SSE-KMS for Existing Buckets**:  
   - Go to **S3 > Bucket > Properties > Default Encryption > Edit**.  
   - Select **SSE-KMS** and choose your KMS key from the dropdown.  
3. **Enable SSE-KMS for New Buckets**:  
   - During bucket creation, under **Default Encryption**, select **SSE-KMS** and specify your KMS key.  

**Example**:  
- The user selects **SSE-KMS** in the bucket properties and chooses an existing KMS key. If no key exists, they can create a new one directly from the S3 console.  

</details>

**Key Points**:  
- You control key creation, rotation, and access.  
- Provides audit logs via CloudTrail for compliance.  
- Suitable for sensitive data and regulatory requirements.  

---

#### **Comparison: SSE-S3 vs. SSE-KMS**  
| Feature                | SSE-S3                          | SSE-KMS                          |  
|------------------------|---------------------------------|----------------------------------|  
| **Key Management**     | AWS manages keys automatically. | You create and manage keys.      |  
| **Control**            | Limited control over keys.      | Full control over keys.          |  
| **Audit Logs**         | No audit logs for key usage.    | Detailed logs via CloudTrail.    |  
| **Use Case**           | General-purpose encryption.     | Sensitive data and compliance.   |  
| **Ease of Use**        | Simple, no setup required.      | Requires key creation and setup. |  

---

#### **Humanized Analogy**  
- **SSE-S3**: Like a hotel safe where the hotel staff holds the master key. You trust them to keep your valuables secure.  
- **SSE-KMS**: Like a personal safe in your home. You hold the key and decide who else can access it.  

### **3.DSSE-KMS (Dual-Layer Server-Side Encryption with AWS KMS)**
- **Overview:**  
AWS provides an additional security layer with **dual-layer encryption**, where each object is encrypted **twice using two different KMS keys**.

- **How It Works:**  
  - The first layer of encryption is applied using an AWS KMS key.
  - A second layer of encryption is applied using a separate KMS key.
  - The decryption process reverses these two encryption layers.

- **Key Management:**  
  - Both encryption layers use AWS KMS.
  - Users can define their own KMS keys for each layer.

- **Use Case:**  
  - Required for compliance with regulatory frameworks like **US government security standards**.
  - Suitable for high-security environments needing multiple layers of encryption.

- **Limitations:**  
  - Higher cost due to the use of two KMS keys.
  - Slightly increased processing overhead.

<details>
  <summary>Steps to Create</summary>
  
### **How to Enable DSSE-KMS:**  
**Example using AWS CLI:**
```sh
aws s3 cp file.txt s3://my-bucket/ --sse aws:kms --sse-kms-key-id <key-id> --sse-dsse
```

---

</details>

### **Summary Table:**

| Encryption Type | Key Management | Security Level | Cost | Use Case |
|---------------|---------------|--------------|------|---------|
| **SSE-S3** | Managed by S3 | Basic | No additional cost | General encryption without key management |
| **SSE-KMS** | Managed by AWS KMS | High | AWS KMS cost applies | Compliance, logging, fine-grained access control |
| **DSSE-KMS** | Two AWS KMS keys | Highest | Higher cost due to dual encryption | High-security environments, government compliance |

### **4. In-Transit Encryption (Enforce HTTPS)**  
- **What**: Ensures data is encrypted **during transfer** between users and S3.  
- **Why**: Prevents eavesdropping (like sending a sealed letter instead of a postcard).  

<details>
  <summary>Steps to Create</summary>

**Steps**:  
1. **Edit Bucket Policy**:  
   - Go to **S3 > Bucket > Permissions > Bucket Policy > Edit**.  
   - Add a policy to **block HTTP** and **allow HTTPS**:  
     ```json
     {
       "Version": "2012-10-17",
       "Id": "EnforceHTTPS",
       "Statement": [{
         "Effect": "Deny",
         "Principal": "*",
         "Action": "s3:*",
         "Resource": "arn:aws:s3:::your-bucket-name/*",
         "Condition": { "Bool": { "aws:SecureTransport": "false" }}
       }]
     }
     ```  
2. **Test**:  
   - Try accessing an object via HTTP (e.g., `http://bucket.s3.amazonaws.com/object`). You’ll see **Access Denied**.  

**Example**:  
- Without HTTPS, hackers could intercept data. This policy acts as a "bouncer" rejecting insecure requests.  

</details>


### **5. Bucket Versioning**   
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


### **6. Cross-Bucket Replication**  
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

---

#### **Key Takeaways**  
1. **Encryption**:  
   - **SSE-S3**: Quick & easy (AWS handles keys).  
   - **SSE-KMS**: More control (audit key usage via CloudTrail).  
2. **HTTPS**: Non-negotiable for secure data transfer.  
3. **Versioning**: Your safety net against mistakes.  
4. **Replication**: Automate backups for peace of mind.  


#### **Common Pitfalls & Fixes**  
- **"Access Denied" After Enforcing HTTPS**:  
  - Ensure your bucket policy uses the correct ARN and `"aws:SecureTransport": "false"`.  
- **Replication Fails**:  
  - Check IAM role permissions and enable versioning on both buckets.  
- **KMS Key Errors**:  
  - Grant the S3 service permission to use the KMS key (via KMS key policy).
  

#### **Humanized Analogy**  
Imagine your S3 bucket is a **bank vault**:  
- **Encryption (SSE-S3/KMS)**: Locking valuables in safe deposit boxes.  
- **HTTPS**: Armored trucks transporting cash securely.  
- **Versioning**: Security cameras storing footage of every vault entry.  
- **Replication**: A duplicate vault in another city for emergencies.  

---
