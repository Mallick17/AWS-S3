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
An additional layer of security that requires multi-factor authentication for changing Bucket Versioning settings and permanently deleting object versions. To modify MFA delete settings, use the AWS CLI, AWS SDK, or the Amazon S3 REST API. [Learn more](https://docs.aws.amazon.com/console/s3/bucket-versioning-mfa-delete)
   - **Disabled**: This is an additional security measure. If enabled, it requires multi-factor authentication to make changes to the bucket’s versioning settings or delete object versions. Since it’s disabled, no additional authentication is needed for these actions.

### Tags
You can use bucket tags to track storage costs and organize buckets. [Learn more](https://docs.aws.amazon.com/console/s3/cost-allocation-tagging)
   - **No tags associated with this resource**: Tags are labels that help organize and track resources in AWS. For example, you might tag your bucket with a name like "Project-1" or "Cost-Center-123" for easier management. This bucket doesn’t have any tags.

### Default encryption
Server-side encryption is automatically applied to new objects stored in this bucket.
**1. **Server-side encryption with Amazon S3 managed keys (SSE-S3)**:**
   - This encryption method automatically encrypts any object you upload to this bucket. SSE-S3 uses AWS-managed keys to secure your data.

**2. **Bucket Key****
   - **Enabled**: This is used to lower encryption costs when using AWS Key Management Service (KMS) encryption. It helps reduce the number of calls to KMS, which can save money.
