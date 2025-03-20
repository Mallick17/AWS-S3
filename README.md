# **Amazon S3 Storage Classes

## **1. Storage Classes for Frequently Accessed Objects**
These storage classes are designed for performance-sensitive applications that require millisecond access time.

### **A. S3 Standard (STANDARD)**
- **Default storage class** – used when no other class is specified.
- **High durability and availability** (99.99% availability, 99.999999999% durability).
- Best for **frequently accessed** data.
- Used for **general-purpose storage**, dynamic websites, content distribution, and mobile applications.
- **No retrieval fee**, but storage cost is higher than infrequent-access classes.
- **Use case:** Frequently accessed business-critical data, transactional workloads.

---

### **B. S3 Express One Zone (EXPRESS_ONEZONE)**
- **High-performance single-zone storage** – lowest latency among all S3 storage classes.
- Data stored redundantly within a **single Availability Zone (AZ)**.
- Up to **10x faster access** and **50% lower request costs** compared to S3 Standard.
- Suitable for latency-sensitive applications that can tolerate **single AZ failure**.
- **Use case:** AI/ML workloads, real-time analytics, financial transactions.

---

### **C. Reduced Redundancy Storage (RRS) – (Deprecated)**
- Designed for **noncritical, reproducible data** with less redundancy.
- **Lower durability** compared to S3 Standard (0.01% annual object loss risk).
- **Not recommended** – S3 Standard is more cost-effective.
- **Use case:** Temporary or reproducible data (e.g., logs, thumbnails).

---

## **2. Storage Class for Data with Changing or Unknown Access Patterns**
This storage class optimizes costs by automatically moving objects to lower-cost tiers.

### **A. S3 Intelligent-Tiering (INTELLIGENT_TIERING)**
- **Automatically moves data** to cost-effective access tiers based on access frequency.
- **No retrieval fees** – unlike Standard-IA or One Zone-IA.
- **99.9% availability, 99.999999999% durability**.
- **Small monthly monitoring fee** (objects <128 KB are not monitored).
- **Tiers:**  
  1. **Frequent Access Tier** – Default for new objects.
  2. **Infrequent Access Tier** – Moves objects not accessed for **30 days**.
  3. **Archive Instant Access Tier** – Moves objects not accessed for **90 days**.
  4. **Optional Archive Access Tier** – Moves objects not accessed for **90 days** (requires restore for access).
  5. **Optional Deep Archive Access Tier** – Moves objects not accessed for **180 days** (requires restore for access).
  
- **Use case:** Data with unpredictable access patterns, logs, machine learning datasets.

---

## **3. Storage Classes for Infrequently Accessed Objects**
These classes offer **millisecond access** at lower storage costs but with **retrieval fees**.

### **A. S3 Standard-IA (STANDARD_IA)**
- **Multi-AZ storage**, higher durability than One Zone-IA.
- Cheaper than S3 Standard but **retrieval fee applies**.
- **Minimum storage duration:** 30 days.
- Best for **long-lived but infrequently accessed data**.
- **Use case:** Backups, disaster recovery, long-term archives.

### **B. S3 One Zone-IA (ONEZONE_IA)**
- **Single AZ storage** – lower cost than Standard-IA.
- **No resilience** to Availability Zone failures.
- **Minimum storage duration:** 30 days.
- **Use case:** Secondary backups, easily reproducible data.

---

## **4. Storage Classes for Rarely Accessed Objects (Cold Storage)**
These classes provide the lowest-cost storage for long-term data retention.

### **A. S3 Glacier Instant Retrieval (GLACIER_IR)**
- **Lowest-cost storage for archival data with millisecond retrieval**.
- Suitable for **rarely accessed but business-critical data**.
- **Minimum storage duration:** 90 days.
- **Use case:** Medical records, compliance data, financial records.

### **B. S3 Glacier Flexible Retrieval (GLACIER)**
- Used for **data that doesn’t require immediate access**.
- Retrieval times: **Minutes to hours** (Expedited, Standard, Bulk retrieval options).
- **Minimum storage duration:** 90 days.
- **Use case:** Large-scale backups, disaster recovery.

### **C. S3 Glacier Deep Archive (DEEP_ARCHIVE)**
- **Lowest-cost storage** – for data that’s **almost never accessed**.
- Retrieval times: **Hours** (Standard: 12 hours, Bulk: 48 hours).
- **Minimum storage duration:** 180 days.
- **Use case:** Regulatory compliance, digital preservation.

---

## **5. Storage Class for On-Premises (Outposts)**
### **A. S3 Outposts (OUTPOSTS)**
- **S3 storage on AWS Outposts hardware** (on-premises).
- **Supports encryption** (SSE-S3, SSE-C).
- Used for applications requiring **local data residency**.

---

## **Key Differences: Amazon S3 Storage Classes**
| Storage Class        | Availability | Durability | Retrieval Time | Retrieval Fee | Use Case |
|----------------------|-------------|------------|----------------|---------------|----------|
| **S3 Standard** | 99.99% | 99.999999999% | Milliseconds | No | Frequently accessed data |
| **S3 Express One Zone** | Single AZ | 99.999999999% | Milliseconds | No | Low-latency applications |
| **S3 Intelligent-Tiering** | 99.9% | 99.999999999% | Milliseconds | No | Unknown access patterns |
| **S3 Standard-IA** | 99.9% | 99.999999999% | Milliseconds | Yes | Infrequent but long-lived data |
| **S3 One Zone-IA** | 99.5% | 99.999999999% | Milliseconds | Yes | Secondary backups, easily reproducible data |
| **S3 Glacier Instant Retrieval** | 99.9% | 99.999999999% | Milliseconds | Yes | Archival data with real-time access |
| **S3 Glacier Flexible Retrieval** | 99.9% | 99.999999999% | Minutes to hours | Yes | Backup and disaster recovery |
| **S3 Glacier Deep Archive** | 99.9% | 99.999999999% | 12–48 hours | Yes | Long-term cold storage |
| **S3 Outposts** | On-premises | 99.999999999% | Milliseconds | No | Data residency and local processing |

---

## **Summary & Recommendations**
1. **Use S3 Standard** for frequently accessed data.
2. **Use S3 Express One Zone** for high-performance, low-latency workloads.
3. **Use S3 Intelligent-Tiering** if data access patterns are unknown or dynamic.
4. **Use S3 Standard-IA** for infrequently accessed but mission-critical data.
5. **Use S3 One Zone-IA** for infrequently accessed, easily reproducible data.
6. **Use S3 Glacier classes** for long-term storage and archiving.
7. **Use S3 Outposts** for on-premises object storage.
