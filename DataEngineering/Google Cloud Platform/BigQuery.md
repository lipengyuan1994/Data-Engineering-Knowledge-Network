
50. You have migrated a data warehouse from on-premises to [[BigQuery]]. You have not modified the ETL process other than to change the target database to BigQuery.  The overall load performance is slower than expected and you have been asked to tune the process. You have determined that the most time-consuming part of the load process is the final step of the ETL process. It loads data from CSV files compressed using Snappy compression into BigQuery. The files are stored in[[ Cloud Storage]]. What change would you make to make the load process save the most time in the load process?
The correct answer is to use uncompressed [[Avro]] files are the most performant option. Changing the type of compression with CSV files will not increase performance as much as using Avro format.  Both compressed and uncompressed JSON is less performant than Avro format when loading data into BigQuery.