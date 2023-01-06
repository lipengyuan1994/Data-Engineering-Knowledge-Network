## Feature

-   Executes **[[Apache Beam]]** Pipelines(no-ops, could use **[[Spark]], [[Flink]]**, [[Java]]) - Apache Beam Runner
-   Can be used for **batch or stream** data
-   **scaleable**, **fault-tolerant**, multi-step processing of data 
-   Often used for data preparation/[[ETL]] for data sets
-   filter, group, transform
-   Pipelines and how it works for ETL
-  not a messaging service
-  implements sliding window functions.
- **You cannot assign a role to a [[Cloud Dataflow]] job**.
- Horizontally scalable, managed service
- Supports windowing operations



Example:
read from cloud storage, run a world count template and save the result back in cloud storage.  use google managed keys for encryption. 

## Job Status:
* Running
* Not started
* Queued
* Canceling/canceled
* Draining/Drained
* Updating/Updated
* Succeeded
* Failed


## Troubleshooting a Workflow
* Check error messages
	* job logs panel
	* job error reporting panel
* Review steps of job
* Look for
	* Pipeline construction errors
	* Errors in job validation
	* Exceptions in workers


## Invalid Dataflow
