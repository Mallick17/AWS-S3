# Amazon S3 Storage Classes
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

| **Storage Class** | **Durability & Availability** | **Best For** | **Cost Considerations** | **Retrieval Time** | **Minimum Storage Duration** | **Use Case Example** |
|-------------------|-----------------------------|--------------|------------------------|------------------|-----------------------|------------------|
| **S3 Standard (STANDARD)** _(Default Storage Class)_ | 99.999999999% durability, 99.99% availability | Frequently accessed data, general-purpose storage | Higher storage cost, no retrieval fee | Milliseconds | None | Frequently accessed business-critical data, transactional workloads |
| **S3 Express One Zone (EXPRESS_ONEZONE)** | 99.999999999% durability, 99.9% availability (Single AZ) | Latency-sensitive workloads, real-time applications | 50% lower than Standard, but stored in a single AZ | Milliseconds (10x faster than Standard) | None | Low-latency workloads like machine learning inference |
| **S3 Intelligent-Tiering (INTELLIGENT_TIERING)** | 99.999999999% durability, 99.9% availability | Data with unpredictable access patterns | Automatically moves data to cost-effective tiers, no retrieval fee | Milliseconds | None | Dynamic storage needs, such as logs or analytics data |
| **S3 Standard-IA (STANDARD_IA)** | 99.999999999% durability, 99.9% availability | Infrequently accessed data but still needs fast retrieval | Lower storage cost, per GB retrieval fee | Milliseconds | 30 days | Backups, disaster recovery, infrequent data retrieval |
| **S3 One Zone-IA (ONEZONE_IA)** | 99.999999999% durability, 99.5% availability (Single AZ) | Infrequent data that does not require multi-AZ resiliency | Lower cost than Standard-IA, stored in a single AZ | Milliseconds | 30 days | Secondary backups, non-critical logs |
| **S3 Glacier Instant Retrieval (GLACIER_IR)** | 99.999999999% durability, 99.9% availability | Archive data that requires immediate access | Lower than IA, per GB retrieval fee | Milliseconds | 90 days | Legal documents, archived medical records with quick access |
| **S3 Glacier Flexible Retrieval (GLACIER)** | 99.999999999% durability, 99.99% availability | Cold data storage, infrequently accessed | Low storage cost, retrieval fees apply | Minutes to hours | 90 days | Long-term backups, historical data |
| **S3 Glacier Deep Archive (DEEP_ARCHIVE)** | 99.999999999% durability, 99.99% availability | Long-term archival storage, rarely accessed | Lowest storage cost, retrieval fees apply | Hours (12-48 hours) | 180 days | Compliance archives, regulatory storage, large media libraries |
| **S3 Outposts (OUTPOSTS)** | 99.999999999% durability, depends on Outposts' availability | On-premises AWS S3 storage for compliance | Varies based on configuration | Milliseconds | None | Local data storage for hybrid cloud applications |

---

### **Key Takeaways**
1. **Frequent Access Needs →** **S3 Standard** (default)  
2. **Low Latency →** **S3 Express One Zone**  
3. **Cost Optimization for Dynamic Workloads →** **S3 Intelligent-Tiering**  
4. **Infrequent Access with Quick Retrieval →** **S3 Standard-IA / One Zone-IA**  
5. **Archival Storage →** **Glacier & Deep Archive (cheapest, but slowest retrieval)**  
6. **On-Premises Hybrid Storage →** **S3 Outposts**  

---

## **Summary & Recommendations**
1. **Use S3 Standard** for frequently accessed data.
2. **Use S3 Express One Zone** for high-performance, low-latency workloads.
3. **Use S3 Intelligent-Tiering** if data access patterns are unknown or dynamic.
4. **Use S3 Standard-IA** for infrequently accessed but mission-critical data.
5. **Use S3 One Zone-IA** for infrequently accessed, easily reproducible data.
6. **Use S3 Glacier classes** for long-term storage and archiving.
7. **Use S3 Outposts** for on-premises object storage.
