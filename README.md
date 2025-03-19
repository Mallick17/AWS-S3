# Bucket
## Objects
Objects are the fundamental entities stored in Amazon S3. You can use [Amazon S3 inventory](https://docs.aws.amazon.com/console/s3/inventory)  to get a list of all objects in your bucket. For others to access your objects, you’ll need to explicitly grant them permissions. [Learn more](https://docs.aws.amazon.com/console/s3/object-properties) 

## Properties
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
Enable objects stored in the Intelligent-Tiering storage class to tier-down to the Archive Access tier or the Deep Archive Access tier which are optimized for objects that will be rarely accessed for long periods of time. Learn more 
   - **No archive configurations**: Intelligent-Tiering is a storage class in S3 that automatically moves objects to cheaper storage tiers based on their access patterns. Here, no objects are set to move to cheaper archive storage.

### Server access logging
