

#### 1. When would you choose to use Cloud Dataproc over Cloud Dataflow for a data pipeline?

Cloud Dataproc is the better option when migrating and existing Spark/Hadoop cluster to GCP. New data pipelines should prefer Cloud Dataflow.

####  2. You are migrating a database to Cloud Spanner. The current model uses auto-increment values as a primary key. You need to keep those values but don’t want to use them as a primary key.  How could you create a primary key that does not use an auto-increment value?

•Use a hash function value of the auto-increment value

•Use a binary reversed value of the value

#### 3.How would you design a time series database for an IoT application?

•2 Tables

    •Query by sensor ID and time

    •Query by time

  

•All Measurement Table

   •Row key: sensor id + time

   •Columns

        •Temperature

          •Humidity

          •Pressure

•Temperature by Time Table

      •Row key: reverse time
      •Columns
           •Temperature


#### 4. You have deployed a data loading application that processes data before loading it into BigQuery. The application runs on a Compute Engine instance. How can monitoring and logging be used to troubleshoot this performance problem?

•Install monitoring agent to get detailed performance metrics on the VM
•Create alerts to notify you when key metrics are outside expected performance range
•When there is an alert, review logs around the time of the alert
•If this does not help identify the problem, add more log messages to the application to have more data about the application execution.

