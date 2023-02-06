**1.1 Selecting the appropriate storage technologies**

-   Mapping storage systems to business requirements
-   Data modeling
-   Tradeoffs involving latency, throughput, transactions
-   Distributed systems
-   Schema design


Business requirements are the **starting** point for choosing a data storage system. Data engineers will use different types of storage systems for different purposes. The specific storage system you should choose is determined, in large part, by the stage of the data lifecycle for which the storage system is used.

The data lifecycle consists of four stages:

-   Ingest
	- _Ingestion_ is the first stage in the data lifecycle, and it entails acquiring data and bringing data into the Google Cloud Platform (GCP). The _storage stage_ is about persisting data to a storage system from which it can be accessed for later stages of the data lifecycle. The _process and analyze stage_ begins with transforming data into a usable format for analysis applications. _Explore and visualize_ is the final stage, in which insights are derived from analysis and presented in tables, charts, and other visualizations for use by others.
-   Store
	- 
-   Process and analyze
-   Explore and visualize


#### Access Controls

Security and access control in particular also influence how data is stored.

[[Relational databases]], like [[Cloud SQL]] and [[Cloud Spanner]], provide mechanisms to restrict access to tables and views. Some users can be granted permission to update data, whereas others can only view data, and still others are not allowed any direct access to data in the database. **Fine-grained security** can be implemented at the application level or by creating views that limit the data available to some users.

Some access controls are coarse grained. For example, [[Cloud Storage]] can limit access based on bucket permissions and access control lists on objects stored in a bucket. If a user has access to a file in the bucket, then they will have access to all the data in that file. Cloud Storage treats files as atomic objects; there is no concept of a row of data, for example, in Cloud Storage as there is in a relational database.

In some cases, you may be able to use other security features of a service along with access controls. [[BigQuery]], for example, is an analytical database used for data warehousing, data analytics, and machine learning. Data is organized into datasets, which are groups of tables and views. At the current time, BigQuery supports dataset-level access controls but not access controls on tables or views directly. One way to work around these limitations is to create authorized views in one dataset that reference tables in another dataset. The dataset with the authorized views can have one set of access controls whereas the dataset with the source tables can have more restrictive access controls.

When choosing a data store, it is important to consider access control requirements and how well a storage system supports those requirements.


#### Time to Store

Consider how long data will be stored when choosing a data store. Some data is transient. For example, data that is needed only temporarily by an application running on a [[Compute Engine ]]instance could be stored on a local [[solid-state drive]] (SSD) on the instance. As long as the data can be lost when the instance shuts down, this could be a reasonable option.

Data is often needed longer than the lifetime of a virtual machine instance, so other options are better fits for those cases. Cloud Storage is a good option for long-term storage, especially if you can make use of storage lifecycle policies to migrate older data to Nearline or Coldline storage. For long-lived analytics data, Cloud Storage or BigQuery are good options, since the costs are similar.

##### Note

[[Nearline storage]] is used for data that is accessed less than once per 30 days. [[Coldline storage]] is used to store data accesses less than once per year.

Data that is frequently accessed is often well suited for either relational or NoSQL databases. As data ages, it may not be as likely to be accessed. In those cases, data can be deleted or exported and archived. If the data is not likely to be used for other purposes, such as machine learning, and there are no regulations that require you to keep the older data, then deleting it may be the best option. In cases where the data can be useful for other purposes or you are required to retain data, **then exporting and storing it in [[Cloud Storage]] is an option**. Then, if the data needs to be accessed, **it can be imported to the database and queried there.**


