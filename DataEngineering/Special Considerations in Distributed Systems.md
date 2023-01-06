1. distributed systems are built based on multiple services and we coordinate workflows between those services.

2. services may have different scaling characteristics
3. For example:
	1. stateless web interfaces can saily scale up
	2. stateful services more difficult to scale

### [[Synchronous Processing]]

1. Multiple services
2. One service calls another
3. calling service waits for called service to finish 
4. Throughput of both services determined by the slower of the two services

### [[Asynchronous messaging]]

1. Calling service does not waits for called service to finish 
2. Calling service continues to process without waiting 
3. Input to called service is buffed 
4. Called service processed input as it can. 
5. instead of calling another service directly, write results to a queue
6. Another service reads data from the queue, processes it 
7. Possibly write to another queue for another service to consume
8. Called servce processes input as it can
9. use [[Cloud PubSub]] to implement 

