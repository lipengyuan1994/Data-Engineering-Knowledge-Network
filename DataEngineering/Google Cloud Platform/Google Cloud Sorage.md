-   Blob storage. Upload any bytes to a location. The content **isn’t indexed** at all just stored. (Amazon S3)
-   Virtually **unlimited** storage
-   Nearline and Coldline: for ~1 sec lookup for access, charged for volume of data accessed

-   **Nearline** for once per month
-   **Coldline**  for once per year 

-   buckets to segregate storage items 
-   geographical separation: 

-   persistant, durable, replicated
-   spread data across zones to minimise impact of service disruptions 
-   spread data across regions to provide global access to data 

-   Ideal for storing but **not for high volume of read/write** (e.g. sensor data)
-   A way to store the data that can be **commonly used by Dataproc and BigQuery**

  

**Encryption**:

-   Google Cloud Platform encrypts customer data stored **at rest** by default
-   Encryption Options:

-   Server-side encryption:

-   [**Customer-supplied encryption keys**](https://cloud.google.com/storage/docs/encryption/customer-supplied-keys): You can create and manage your own encryption keys for server-side encryption
-   [**Customer-managed encryption keys**](https://cloud.google.com/storage/docs/encryption/customer-managed-keys): You can generate and manage your encryption keys using **Cloud Key Management Service**.

-   [Client-side encryption](https://cloud.google.com/storage/docs/encryption/client-side-keys): encryption that occurs before data is sent to Cloud Storage.