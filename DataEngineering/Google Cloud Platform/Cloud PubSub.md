**Feature**

	-   **server-less** messaging 
	-   **decouples** producers and consumers of data in large organizations / complex systems
	-   the glue that connects all the components. 
	-   **order not guaranteed**
- Cloud Pub/Sub will scale as needed and will not lose data

  
  

**Asynchronous processing** 

- **availability** (buffer during outages) 

- **change management** 

- **throughput** (balance load among workers) 

- unification (cross-organisational) 

- **latency** (accept requests at edge of network) 

-   consistency

  

**basic concepts**

	-   topics
	-   subscriptions
	-   push and pull 
	-   **pull is a more efficient message deliver/consumption mechanism**

  

**Message Flow** 

-   Publisher **creates a topic** in the Cloud Pub/Sub service and sends messages to the topic. 
-   Messages are **persisted** in a message store until they are **delivered and acknowledged** by subscribers. 
-   The Pub/Sub service forwards messages from a topic to all of its subscriptions, individually. Each subscription receives messages either by pushing/pulling. 
-   The subscriber receives pending messages from its subscription and acknowledges message.
-   When a message is acknowledged by the subscriber, it is removed from the subscription’s message queue. 

  

**Deduplicate**

-   Maintain a database table to store the hash value and other metadata for each data entry.
-   Cloud Pub/Sub assigns a **unique** `message_id` to each message, which can be used to **detect duplicate messages** received by the subscriber. 
-   Lots of duplicate messages may happen when:  endpoint is not acknowledging messages within the **acknowledgement deadline**