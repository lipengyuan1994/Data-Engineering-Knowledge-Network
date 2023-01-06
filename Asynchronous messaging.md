1. Calling service does not waits for called service to finish 
2. Calling service continues to process without waiting 
3. Input to called service is buffed 
4. Called service processed input as it can. 
5. instead of calling another service directly, write results to a queue
6. Another service reads data from the queue, processes it 
7. Possibly write to another queue for another service to consume
8. Called servce processes input as it can
9. use [[Cloud PubSub]] to implement 
