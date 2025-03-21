## **1. Bucket Policies**
Bucket policies are JSON-based access policy documents that you attach directly to an S3 bucket. They define what actions are allowed or denied for which principals (users, accounts, roles), under what conditions.

- **Scope**: Bucket-level (not object-level)
- **Uses**: Granting cross-account access, IP/VPC restrictions, enforcing encryption, etc.
- **Structure**:
  - `Version`
  - `Statement`
    - `Sid` (optional)
    - `Effect`: Allow or Deny
    - `Principal`: Who the policy applies to
    - `Action`: What operations are allowed (e.g., `s3:GetObject`)
    - `Resource`: Bucket or object ARN
    - `Condition`: Optional filters

---

## **2. Adding a Bucket Policy**
You can add a bucket policy via:
- **AWS Management Console** (S3 > Bucket > Permissions > Bucket Policy)
- **AWS CLI**
  ```bash
  aws s3api put-bucket-policy --bucket my-bucket --policy file://policy.json
  ```
- **AWS SDKs**

---

## **3. Controlling VPC Access**
You can restrict bucket access only to requests coming from a specific **VPC endpoint** using the `aws:SourceVpce` condition key.

**Example Bucket Policy to allow access only via a specific VPC endpoint:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowVPCeAccessOnly",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": ["arn:aws:s3:::my-bucket", "arn:aws:s3:::my-bucket/*"],
      "Condition": {
        "StringNotEquals": {
          "aws:SourceVpce": "vpce-abc123xyz"
        }
      }
    }
  ]
}
```

---

## **4. Bucket Policy Examples**

- **Allow Read Access to Everyone (public):**
```json
{
  "Effect": "Allow",
  "Principal": "*",
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::my-bucket/*"
}
```

- **Grant Full Access to Another AWS Account:**
```json
{
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::111122223333:root"
  },
  "Action": "s3:*",
  "Resource": [
    "arn:aws:s3:::my-bucket",
    "arn:aws:s3:::my-bucket/*"
  ]
}
```

- **Enforce HTTPS-only Access:**
```json
{
  "Effect": "Deny",
  "Principal": "*",
  "Action": "s3:*",
  "Resource": "arn:aws:s3:::my-bucket/*",
  "Condition": {
    "Bool": {
      "aws:SecureTransport": "false"
    }
  }
}
```

---

## **5. Condition Key Examples**

- `aws:SourceIp`: Limit access to specific IPs
- `aws:SourceVpc` or `aws:SourceVpce`: Limit access via a specific VPC or VPC endpoint
- `aws:SecureTransport`: Enforce HTTPS
- `s3:x-amz-server-side-encryption`: Require encryption
- `aws:userid`: Match specific IAM users

---

## **6. Identity-Based Policies**
These are **IAM policies** attached to **users, groups, or roles**. They define what actions a principal can perform on AWS resources.

**Example IAM policy allowing full S3 access:**
```json
{
  "Effect": "Allow",
  "Action": "s3:*",
  "Resource": "*"
}
```

**Key difference**: Identity-based policies are assigned to *people*, not to the bucket.

---

## **7. Walkthroughs Using Policies**

- **Scenario 1: Allow user to upload objects only with encryption**
```json
{
  "Effect": "Deny",
  "Action": "s3:PutObject",
  "Resource": "arn:aws:s3:::my-bucket/*",
  "Condition": {
    "StringNotEqualsIfExists": {
      "s3:x-amz-server-side-encryption": "AES256"
    }
  }
}
```

- **Scenario 2: Allow a role to read objects from a bucket**
```json
{
  "Effect": "Allow",
  "Action": ["s3:GetObject"],
  "Resource": ["arn:aws:s3:::my-bucket/*"]
}
```

---

## **8. Using Service-Linked Roles for Amazon S3 Storage Lens**
Amazon S3 Storage Lens uses **service-linked roles** to collect and analyze storage metrics across your buckets.

- Created automatically when you enable Storage Lens.
- Name: `AWSServiceRoleForS3StorageLens`
- It includes the necessary permissions to read bucket metrics and deliver reports.

You can verify it in IAM > Roles.

---

## **9. Troubleshooting Amazon S3 Identity and Access**

- **Access Denied Errors?**
  - Check bucket policies
  - Check identity-based IAM policies
  - Check block public access settings
  - Validate condition keys and values
  - Check if MFA or encryption conditions are failing
  - Use **IAM Policy Simulator** or **Access Analyzer**

---

## **10. AWS Managed Policies**

These are pre-created, maintained, and updated by AWS. They provide common permissions sets for general tasks.

**Examples:**
- `AmazonS3ReadOnlyAccess`
- `AmazonS3FullAccess`
- `AmazonS3ObjectLambdaExecutionRolePolicy`
- `AmazonS3StorageLensServiceRolePolicy`

**Pros:**
- Easy to assign
- Auto-updated
- Best practice by AWS


---

##  **Hands-On Lab: Secure S3 Bucket Access**

### **Goal**:  
Create an S3 bucket with restricted access, only allowing:
- IAM user to upload files (UploaderUser)
- IAM role to read objects (DataReaderRole)
- Access only via VPC endpoint
- Encryption enforced

---

###  **Step 1: Create an S3 Bucket**
```bash
aws s3api create-bucket --bucket my-secure-bucket --region us-east-1
```

###  **Step 2: Enable Block Public Access**
```bash
aws s3api put-public-access-block \
  --bucket my-secure-bucket \
  --public-access-block-configuration BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true
```

###  **Step 3: Attach a Bucket Policy (VPC & Encryption)**
Save this in `bucket-policy.json`:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyNonEncryptedUploads",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::my-secure-bucket/*",
      "Condition": {
        "StringNotEquals": {
          "s3:x-amz-server-side-encryption": "AES256"
        }
      }
    },
    {
      "Sid": "AllowOnlyFromVPC",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": ["arn:aws:s3:::my-secure-bucket", "arn:aws:s3:::my-secure-bucket/*"],
      "Condition": {
        "StringNotEquals": {
          "aws:SourceVpce": "vpce-abc123xyz"
        }
      }
    }
  ]
}
```
Apply the policy:
```bash
aws s3api put-bucket-policy --bucket my-secure-bucket --policy file://bucket-policy.json
```

###  **Step 4: IAM Policy for UploaderUser**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::my-secure-bucket/*"
    }
  ]
}
```

###  **Step 5: IAM Policy for DataReaderRole**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-secure-bucket/*"
    }
  ]
}
```

###  **Step 6: Enable Encryption on Upload**
Uploader must set this during upload:
```bash
aws s3 cp myfile.txt s3://my-secure-bucket/ --sse AES256
```

---

##  **Real-Life Scenarios / Use Cases**

### **1. Company Internal File Sharing**
- Internal team uploads files (UploaderUser).
- Data analyst IAM role (DataReaderRole) accesses only via private VPC endpoint.
- Bucket policy enforces secure uploads and blocks public access.

### **2. Cross-Account Vendor Collaboration**
- Your team grants limited read access to a vendor’s AWS account using bucket policy.
- Uploads are monitored and must be encrypted.
- Access logs are stored for compliance.

### **3. Compliance in Regulated Environments**
- Bucket policy enforces server-side encryption.
- IAM roles and policies are used to restrict who can upload, delete, or read data.
- Data transfer is forced via HTTPS and within VPCs for better auditability.

---


**Cons:**
- Less control
- Not fine-tuned for specific security requirements

---
