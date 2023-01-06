* Unstructured data
	* Images
	* Video
	* Text
* Archived data
* Temporary storage between services
* Global Access, Web-accessible


* Create buckets to store data
* Buckets are globally unique - Name (globally unique)
* Location
	* Storage Class
		* High-Performance Object Storage
			* Frequent access
			* optimized for performance
		* Backup and Archival 
			* Nearline
			* Coldline
			* archive

### Redundancy 
  • Regional
	  • Data stored in single region
	  • Replicated across zones
  • Dual-region
	  • Data replicated across specific pair of regions
	  • Auto-failover
  • Multi-region
	   • Data duplicated across US, EU or Asia
	   • Auto-failover 


  

## Cloud Storage Benefits for [[Cloud Dataproc]] storage
* Direct data acccess
* HDFS compatibility
* Interoperability
* Data accessibility
* High data availablity ( reduncy policy)
* No storage maangement overhead
* quick startup (no need to copy data)
* another reason that we are able to use [[ephemeral cluster]] so effectively in google cloud.

### Object Lifecycle Management
• Automatically transition to lower-cost storage class
	• Specified by lifecycle policy    
	• Useful for archiving
• Retention policies specifies minimum retention period
• Object holds prevent deletion



### Security
• Automatic encryption
	• Google managed
	• Customer managed - using [[Cloud Key Management Service]]
	• Customer supplied - using key created and
		• managed by customer
• Bucket level access using object access control list
	(ACLs)
• Activity logs and data access logs





# Loading Data

## Cloud Storage Transfer
* Move data from on-premise or other cloud providers
* Between cloud storage buckets
* One-time or recurring transfers
* On-premises transfer
	* < 1 TB - use gsutil
	* 1 TB - use Cloud Storage Transfer Service

## Transfer Appliance
* High-capacity storage server
* Shipped to your data center
* Physically attach to your network to copy data
* ship back to google cloud
* Data copied to cloud storage
* capacity
	* 100TB
	* 480TB


# Access Controls for Cloud Storage 

## Primary Options
* Uniform bucket-level access
	* Recommended
	* Uses [[Cloud Identity]] and [[Access Management]] 
	* Applies permissions to objects in bucket or group of objects with common prefix
* Fine-grained
	* Legacy access control method
	* uses access control lists
	* apply permissions at both bucket and object level 

## Other options

* Signed URLs
	* Time-limited read or write access to an object
	* Generated URL
	* Allows access to those without IAM authorizations
* Signed policy documents
	* Specify what can be uploaded to a bucket 
		* control
			* size
			* type 
			* other uploaded characteristics

