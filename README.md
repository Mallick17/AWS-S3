# **Amazon S3 Object Lifecycle Management**  

Amazon S3 provides **Lifecycle Policies** that help automate the transition and expiration of objects based on predefined rules. These lifecycle policies reduce storage costs and optimize data management by automatically moving objects between storage classes or deleting them when they are no longer needed.  

#### **Key Features of S3 Lifecycle Management**  

1. **Transition Rules**  
   - Move objects between different **S3 Storage Classes** based on their access patterns.  
   - Examples:  
     - Move from **S3 Standard** to **S3 Intelligent-Tiering** after 30 days.  
     - Move from **S3 Standard** to **S3 Glacier Deep Archive** after 180 days.  

2. **Expiration Rules**  
   - Automatically delete objects that are no longer needed.  
   - Example:  
     - Delete objects older than **365 days** to save storage costs.  

3. **Multipart Upload Expiration**  
   - Automatically clean up incomplete **multipart uploads** to free up storage.  
   - Example:  
     - Delete incomplete uploads older than **7 days**.  

4. **Noncurrent Version Expiration**  
   - For **versioned buckets**, delete old object versions after a specific period.  
   - Example:  
     - Keep the latest **3 versions** and delete older ones.  

#### **How Lifecycle Policies Work?**  

- **Define a Lifecycle Rule:**  
  - Specify conditions such as object age, storage class transitions, and expiration.  
- **Apply the Rule to a Bucket or Specific Objects:**  
  - Use filters like **prefixes** or **tags** to apply policies selectively.  
- **Automatic Execution:**  
  - S3 will apply lifecycle actions automatically based on your defined rules.  

#### **Example of an S3 Lifecycle Rule (JSON Format)**  

```json
{
  "Rules": [
    {
      "ID": "Move to Glacier",
      "Status": "Enabled",
      "Filter": { "Prefix": "logs/" },
      "Transitions": [
        {
          "Days": 90,
          "StorageClass": "GLACIER"
        }
      ],
      "Expiration": {
        "Days": 365
      }
    }
  ]
}
```

- Moves **objects in the `logs/` folder** to **Glacier** after 90 days.  
- Deletes **objects** after **365 days**.  

### **Benefits of S3 Lifecycle Management**  

- **Cost Optimization** – Moves rarely accessed data to lower-cost storage.  
- **Automated Data Retention** – Ensures compliance with retention policies.  
- **Reduces Manual Work** – Automatically manages data without admin intervention.  

This feature is especially useful for **log storage, backups, and data archiving**, ensuring efficient and cost-effective storage management.

---
