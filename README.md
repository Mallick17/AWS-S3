# Bucket
# Objects
Objects are the fundamental entities stored in Amazon S3. You can use [Amazon S3 inventory](https://docs.aws.amazon.com/console/s3/inventory)  to get a list of all objects in your bucket. For others to access your objects, you’ll need to explicitly grant them permissions. [Learn more](https://docs.aws.amazon.com/console/s3/object-properties) 

# Properties
### Bucket overview
- This is a general section that gives you an overview of the settings for your Amazon S3 bucket. It shows where your bucket is located (AWS Region) and its name.

**1. **AWS Region****
   - **Asia Pacific (Mumbai) ap-south-1**: This indicates that the S3 bucket is hosted in the Asia Pacific region, specifically in Mumbai, India. It helps determine where your data is physically stored.

**2. **Amazon Resource Name (ARN)****
   - **arn:aws:s3:::mallick-static-site**: This is a unique identifier for your bucket. It's used by AWS to refer to this bucket in various operations (like permissions, logging, etc.).

**4. **Creation Date****
   - **March 17, 2025, 09:52:44 (UTC+05:30)**: This is the timestamp when the bucket was created.

### Bucket Versioning
Versioning is a means of keeping multiple variants of an object in the same bucket. You can use versioning to preserve, retrieve, and restore every version of every object stored in your Amazon S3 bucket. With versioning, you can easily recover from both unintended user actions and application failures. [Learn more](https://docs.aws.amazon.com/console/s3/enable-bucket-versioning)
**1. **Bucket Versioning****
   - **Disabled**: Versioning is a feature that helps keep multiple versions of an object in a bucket. When it's enabled, you can recover deleted or overwritten files. In this case, versioning is turned off, so older versions of objects won’t be retained.

**2. **Multi-factor Authentication (MFA) Delete****
   - An additional layer of security that requires multi-factor authentication for changing Bucket Versioning settings and permanently deleting object versions. To modify MFA delete settings, use the AWS CLI, AWS SDK, or the Amazon S3 REST API. [Learn more](https://docs.aws.amazon.com/console/s3/bucket-versioning-mfa-delete)
   - **Disabled**: This is an additional security measure. If enabled, it requires multi-factor authentication to make changes to the bucket’s versioning settings or delete object versions. Since it’s disabled, no additional authentication is needed for these actions.

### Tags
You can use bucket tags to track storage costs and organize buckets. [Learn more](https://docs.aws.amazon.com/console/s3/cost-allocation-tagging)
   - **No tags associated with this resource**: Tags are labels that help organize and track resources in AWS. For example, you might tag your bucket with a name like "Project-1" or "Cost-Center-123" for easier management. This bucket doesn’t have any tags.

### Default encryption
Server-side encryption is automatically applied to new objects stored in this bucket.

<details>
   <summary>Info</summary>

- Amazon S3 applies server-side encryption with S3 managed keys (SSE-S3) as the base level of encryption for all S3 buckets. With server-side encryption, Amazon S3 encrypts an object before saving it to disk and decrypts it when you download the object. Encryption doesn't change the way that you access data as an authorized user. It only further protects your data. Objects stored in directory buckets can only be encrypted with Amazon S3 managed keys (SSE-S3). For general purpose buckets, you have the option to configure your objects to use other encryption types such as server-side encryption with AWS Key Management Service (AWS KMS) keys (SSE-KMS), or dual-layer server-side encryption with AWS KMS keys (DSSE-KMS).

</details>

**1. Encryption Type**

<details>
   <summary>Info</summary>

   When you configure default encryption for an Amazon S3 bucket, you can use any of the following types of encryption:
- Server-side encryption with Amazon S3 managed keys (SSE-S3) (the default)
- Server-side encryption with AWS Key Management Service (AWS KMS) keys (SSE-KMS)
- Dual-layer server-side encryption with AWS KMS keys (DSSE-KMS)

With the default option (SSE-S3), Amazon S3 uses one of the strongest block ciphers—256-bit Advanced Encryption Standard (AES-256) to encrypt each object uploaded to the bucket. With SSE-KMS, you have more control over your key. If you use SSE-KMS, you can choose an AWS KMS customer managed key or use the default AWS managed key (aws/s3). SSE-KMS also provides you with an audit trail that shows when your KMS key was used and by whom. With DSSE-KMS, Amazon S3 applies two individual layers of object-level encryption to satisfy compliance requirements for highly regulated customers.

</details>

**Server-side encryption with Amazon S3 managed keys (SSE-S3)**:**
   - This encryption method automatically encrypts any object you upload to this bucket. SSE-S3 uses AWS-managed keys to secure your data.

**2. **Bucket Key****
When KMS encryption is used to encrypt new objects in this bucket, the bucket key reduces encryption costs by lowering calls to AWS KMS. [Learn more](https://docs.aws.amazon.com/console/s3/bucket-key) 
   - **Enabled**: This is used to lower encryption costs when using AWS Key Management Service (KMS) encryption. It helps reduce the number of calls to KMS, which can save money.

### Intelligent-Tiering Archive Configurations
Enable objects stored in the Intelligent-Tiering storage class to tier-down to the Archive Access tier or the Deep Archive Access tier which are optimized for objects that will be rarely accessed for long periods of time. [Learn more](https://docs.aws.amazon.com/console/s3/intelligent-tiering) 
   - **No archive configurations**: Intelligent-Tiering is a storage class in S3 that automatically moves objects to cheaper storage tiers based on their access patterns. Here, no objects are set to move to cheaper archive storage.

### Server access logging
Log requests for access to your bucket. Use CloudWatch to check the health of your server access logging. [Learn more](https://docs.aws.amazon.com/console/s3/cloudtrail-logging) 
   - **Disabled**: Server access logging logs requests made to your bucket (who accessed the data, when, and from where). This can be useful for auditing and security. Since it’s disabled, access logs are not being recorded.

### AWS CloudTrail data events
Configure CloudTrail data events to log Amazon S3 object-level API operations in the CloudTrail console.

<details>
   <summary>Info</summary>
   
- CloudTrail supports logging Amazon S3 object-level API operations, such as `GetObject`, `DeleteObject`, and `PutObject`. These events are called data events. CloudTrail data events provide information about object-level requests.
- You can use the CloudTrail console to configure CloudTrail data events for objects in an S3 general purpose bucket or for objects in an S3 directory bucket. In the CloudTrail console, you can enable data events for all of the buckets of each bucket type in your account. For step-by-step instructions, see [Creating a trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-a-trail-using-the-console-first-time.html?icmpid=docs_console_unmapped) in the AWS CloudTrail User Guide.
- For general purpose buckets or directory buckets with high workloads, you can quickly generate thousands of logs in a short amount of time. We recommend that you create a lifecycle configuration for your AWS CloudTrail data event general purpose buckets. Configure the lifecycle configuration to periodically remove log files after the period of time that you need to audit them.
  
</details>

   - **No data events**: CloudTrail is a service that logs AWS API calls. For S3, it can log object-level API operations (like accessing, uploading, or deleting objects). There are no data events configured for this bucket.

### Event notifications
Send a notification when specific events occur in your bucket. [Learn more](https://docs.aws.amazon.com/console/s3/enable-event-notifications)
   - **No event notifications**: Event notifications allow you to receive alerts when specific actions occur in your bucket, like an object being uploaded or deleted. Currently, no notifications are set up.

### Amazon EventBridge
For additional capabilities, use Amazon EventBridge to build event-driven applications at scale using S3 event notifications. [Learn more](https://docs.aws.amazon.com/console/s3/eventbridge-what-is)  or [see EventBridge pricing](https://aws.amazon.com/eventbridge/pricing) 
   - **Off**: EventBridge is a service that enables you to set up event-driven applications based on S3 events. Currently, it is not set up to send notifications for this bucket.

### Transfer acceleration
Use an accelerated endpoint for faster data transfers. [Learn more](https://docs.aws.amazon.com/console/s3/transfer-acceleration) 
   - **Disabled**: Transfer acceleration speeds up uploads to S3 by using a global network of edge locations. Since it’s disabled, transfers won’t be accelerated.

### Object Lock
Store objects using a write-once-read-many (WORM) model to help you prevent objects from being deleted or overwritten for a fixed amount of time or indefinitely. Object Lock works only in versioned buckets. [Learn more](https://docs.aws.amazon.com/console/s3/object-lock) 
   - **Disabled**: Object Lock prevents an object from being overwritten or deleted for a specified period of time or indefinitely. This is useful for data retention requirements. Since it’s disabled, objects in the bucket can be deleted or changed.

### Requester pays
When enabled, the requester pays for requests and data transfer costs, and anonymous access to this bucket is disabled. [Learn more](https://docs.aws.amazon.com/console/s3/requesterpaysbucket) 
   - **Disabled**: Normally, the bucket owner pays for requests and data transfer costs. With "Requester Pays" enabled, the person making the request (not the bucket owner) would pay the costs. Since it’s disabled, the owner is responsible for the costs.

### Static website hosting
Use this bucket to host a website or redirect requests. [Learn more](https://docs.aws.amazon.com/console/s3/hostingstaticwebsite) 
   - **Enabled**: This bucket is set up to host a static website. A static website is one where the content (like HTML, CSS, images) doesn't change dynamically. 
     - **Bucket Website Endpoint**: The website hosted on this bucket is accessible at: [http://mallick-static-site.s3-website.ap-south-1.amazonaws.com](http://mallick-static-site.s3-website.ap-south-1.amazonaws.com)

# Permissions
### **Permissions Overview:**
- **Access findings**: These are results provided by IAM (Identity and Access Management) external access analyzers that detect if your S3 resources are exposed to public access or to accounts other than yours. This helps you monitor potential security risks by identifying unwanted access.

### **Block Public Access (Bucket Settings):**
These settings help you manage public access to your S3 buckets and objects. Public access is typically granted via **Access Control Lists (ACLs)**, **bucket policies**, or **access point policies**. AWS provides various options to block public access to safeguard your data. These settings can be customized to allow certain levels of public access if required.

- **Block all public access**: When turned on, this will block all public access to your S3 bucket and its objects, preventing any unauthenticated users from accessing them. AWS recommends enabling this setting unless you need some level of public access.
  
  You can adjust the individual public access block settings:
  - **Block public access to buckets and objects granted through new ACLs**: Prevents the creation of new public ACLs that could grant access to your bucket or its objects.
  - **Block public access to buckets and objects granted through any ACLs**: Ignores any existing ACLs that would grant public access, essentially disabling public access through ACLs.
  - **Block public access through new bucket or access point policies**: Prevents the creation of new bucket or access point policies that grant public access.
  - **Block public and cross-account access through any public policies**: Blocks any public or cross-account access granted by bucket or access point policies.

### **Bucket Policy:**
A **bucket policy** is a JSON-based policy document that defines who has access to the bucket and its contents. It is often used for more granular access control and applies to all objects within the bucket.

- Example bucket policy provided:
<details>
   <summary>Example</summary>

```json
{
    "Version": "2008-10-17",
    "Id": "PolicyForCloudFrontPrivateContent",
    "Statement": [
        {
            "Sid": "AllowCloudFrontServicePrincipal",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudfront.amazonaws.com"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::cloudfronts-mallick-bucket-12345/*",
            "Condition": {
                "StringEquals": {
                    "AWS:SourceArn": "arn:aws:cloudfront::339713104321:distribution/E1ZMLEKL2QVV8A"
                }
            }
        }
    ]
}
```

</details>
  - It allows **CloudFront** (a content delivery service) to **GetObject** from the bucket, but only if the request comes from a specific CloudFront distribution. This ensures that only requests originating from the specified distribution can access the bucket contents, preventing public access.

### **Object Ownership:**
Object ownership determines who controls the access permissions for objects within the bucket. It’s important for managing objects uploaded by external AWS accounts.

- **Bucket owner enforced**: When enabled, the bucket owner has full control over all objects, even if they were uploaded by other AWS accounts. This setting disables the use of ACLs and ensures that only policies define who can access the objects.

### **Access Control List (ACL):**
ACLs allow you to grant permissions to specific AWS accounts or groups. They define who can read or write objects in the bucket.

- **Bucket ACL**: This is the ACL for the bucket itself.
  - **Grantee**: The entity (user or group) receiving the permissions.
  - The **Bucket owner** is the account that owns the bucket, and you can assign **List** and **Write** permissions for the objects within the bucket.
  
- **Groups**:
  - **Everyone (public access)**: The permissions are applied to everyone, i.e., the general public.
  - **Authenticated users**: Any authenticated AWS user can access the objects.
  - **S3 log delivery group**: Used for S3 logging, granting access for logging services to write logs into your bucket.

In the **Bucket ACL**, you typically assign read and write permissions. For example:
  - The bucket owner has full permissions to list and write to the bucket.
  - "Everyone" (the public) has no permissions (since public access is blocked).
  - **Authenticated users** group and **S3 log delivery group** might have restricted permissions depending on the configuration.

### **Cross-Origin Resource Sharing (CORS):**
CORS allows a web application running in one domain to access resources in a different domain. It’s used when client-side web applications interact with S3 from a different origin (e.g., a website hosted on `example.com` accessing assets stored in an S3 bucket).
   - In your provided settings, no CORS configurations are listed, meaning the bucket doesn't allow cross-origin requests by default. If you want to enable this, you can define CORS rules in JSON format that specify what types of requests are allowed from which domains.

