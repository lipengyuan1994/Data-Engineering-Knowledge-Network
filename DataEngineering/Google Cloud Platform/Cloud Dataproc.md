managed Spark/Hadoop service.


## Feature

-   Dataproc is managed Spark and  [[Hadoop]] cluster on GCP (i.e. managed Hadoop, [[Pig]], [[Hive]], [[Spark]] programs)
- Used for large-scale [[batch processing]] and machine learning, [[stream processing]] based on Spark. 
- Often used for ETL and ELT
- [[Ephemeral cluster]]s

-   automated cluster management. resizing 
-   Code/Query only
-   Job management screen in the console.
-   
-   Used **if migrating existing on-premise Hadoop or Spark infrastructure** to Google Cloud Platform without redevelopment effort.
- Dataproc **does not support specifying snapshots when creating a cluster.**


## Create cluster

### Cluster mode
  
* A [[single node]] will basically provide you the environment, you need to test something out but it's really not a production environment.
* The [[standard node]] has one master or controlling server, and then a certain number of workers that you can specify.
* High availability, 3 masters, N workers.

### Machine type
### CPU platform and GPU
[[SSD]] persistent disk for high performance IO

with cloud Dataproc, we can use [[preemptible worker node]]s.

**Storage** 
-   Can store data on disk (**HDFS**) or can use **GCS** [[Google Cloud Sorage]]
-   **GCS** allows for the use of preemptible machines that can reduce costs significantly
-   - store in HDFS (split up on the cluster, but requires cluster to be up) or in GCS (separate cluster and storage) 


## Monitoring a Cloud Dataproc Cluster
* [[Job Driver output]]
* [[Cloud Monitoring]]
* [[Cloud Logging]]



**Customize the software**

-   Set initialization actions
-   Modify configuration files using cluster properties
-   Log into the master node and make changes from there
-   NO Cloud Deployment Manager

  
**Tech:**
-   creating a new Cloud Dataproc cluster with the projects.regions.clusters.create operation, these four values are required: **project, region, name, and zone**. 
-   you can access the YARN web interface by configuring a browser to connect through a **SOCKS** proxy
-   You can **SSH directly** to the cluster nodes 
-   you can use a **SOCKS** proxy to connect your browser through an **SSH tunnel**.
-   **YARN** ResourceManager and the **HDFS** NameNode interfaces are available on **master node**

  

**Billing:**

-   is billed by the second. All Cloud Dataproc clusters are billed in **one-second clock-time increments**, subject to a 1-minute minimum billing