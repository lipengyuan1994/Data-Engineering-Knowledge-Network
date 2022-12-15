**Feature**:

-   Stored on Google’s internal store **Colossus**
-   **no transactional** support (so can handle **petabytes** of data) 
-   **not relational** (No SQL or joins), ACID only at row level.

-   Avoid schema designs that require atomicity across rows

-   **high throughput**: Throughput has linear growth with node count if correctly balanced.
-   work with it using **Hbase API**
-   **no-ops**, auto-balanced, replicated, compacted,

  

**Query**:

-   **Single key lookup**. No property search.
-   Stored **lexicographically** in big endian format so keys can be anything. 
-   **quick range lookup**

  

**Performance**:

-   Fast to **petabyte** scale, **not a good** solution for storing **less than 1 TB of data**.That's because Bigtable needs to do a set of smart optimizations related to sharding and distributed storage, and you just won't be able to do that if your dataset is too small.
-   **Low-latency** read/write access 
-   High-throughput analytics 
-   **Native time series** support
-   for large **analytical and operational** workloads
-   designed for **sparse tables** 

  

**Key Design**:

-   Design your keys **how you intend to query**. 
-   If your most common query is the **most recent data**, use a **reverse date stamp** at the end of the key.
-   Ensure your **keys are evenly distributed** to void **hot spotting**. This is why date stamps as a key or starting a key is bad practise as all of the most recent data is being written at the same time. 

-   For **historical data analytic, hotspot issue** may not a biggest concern.

-   For **time-series** data, use **tall/narrow** tables. Denormalize- prefer multiple tall and narrow tables
-   **avoid hotspotting**

-   **Field promotion (preferred):** Move fields from the column data into the row key to make writes non- contiguous.
-   **Salting**: (only where field promotion does not resolve) Add an additional calculated element to the row key to artificially make writes non- contiguous.

  

**Performance Test**

-   learns about your **access patterns** and will adjust the metadata stored in nodes in order to try balance your workloads. 
-   This takes minutes to hours and requires to use at least **300GB of data**
-   use a **production** instance
-   Stay **below the recommended storage utilization** per node.
-   Before you test, run **a heavy pre-test** for several minutes
-   Run your test for at least **10 minutes**

  

**Data update**

-   When querying BigTable selects **the most recent value that matches the key**. 

-   This means that when **deleting/updating we actually write a new row with the desired data** and compaction can remove the deleted row later. This means that deleting data will temporarily increase the disk usage

-   append only (cannot update a single field like in CSQL/CDS) 
-   tables should be **tall and narrow** (store changes by appending new rows- tall, collapse flags into a single column - narrow)

  

  

**Group columns**

-   **Group columns** of data you are likely to query together (for example address fields, first/last name and contact details).
-   use **short column names**, organise into column families (groups e.g. MD:symbol, MD:price) 

  

**Periodically compacts**

-   BigTable periodically compacts the data for you. This means reorganising and removing deleted records once they’re no longer needed.

  

**Sorted String Tables**

-   BigTable relies on **Sorted String Tables** to organize the data. 
-   are immutable pre sorted **key,value pairs** of strings or **protobufs**. 

  

  

**Architecture**

  ![[Pasted image 20221214105553.png]]

  

**Size limits**

-   A single row key: 4 KB (soft limit?)
-   A single value in a table cell: 100 MB
-   All values in a single row: 256 MB

  

**Access Control**

-   you can configure access control at the **project** level and the **instance** level(lowest IAM resources control)

-   A Cloud Bigtable **instance** is mostly just a container for your clusters and nodes, which do all of the real work.
-   **Tables belong to instances**, not to clusters or nodes. So if you have an instance with up to 2 clusters, you can't assign tables to individual clusters  
    

  

**Production & Development**

-   **Production**: A standard instance with **either 1 or 2 clusters**, as well as **3 or more nodes** in each cluster.

-   use **replication** to provide high availability

-   **Development**: A low-cost instance for development and testing, with performance limited to the equivalent of a 1-node cluster.
-   It is recommended to create your **Compute Engine instance** in the **same zone** as your **Cloud Bigtable** instance for the best possible performance (when use Cloud Bigtable with a Compute Engine-based application)

  

**TOOLS**

-   **cbt** is a tool for doing basic interactions with Cloud Bigtable.
-   **HBase shell** is a command-line tool that performs administrative tasks, such as creating and deleting tables. 
-   you can update any of the following settings without any downtime:

-   number of clusters  / replication settings
-   upgrade a development instance to a production (permanent)

-   Impossible to Switching between SSD and HDD

-   export the data from the existing instance and import the data into a new instance. 
-   OR write a Cloud Dataflow or Hadoop MapReduce job that copies the data from one instance to another.