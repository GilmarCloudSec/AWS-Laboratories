## Title
Simple Queue Service (SQS): Visibility Timeout


## Objective
Explain the SQS: Visibility Timeout.


## Implementation
The visibility timeout determines how long a message received from a queue will be invisible to other consumers after it has been received by one consumer.

![Visibility Timeout](./AWS-Labs/Application%20Integration/SQS%3A%20Visibility%20Timeout/visibility%20timeout.png)

Imagine you have a queue named MyQueue in Amazon SQS that contains several messages. Each message represents a task that needs to be processed by consumers.

1. **Visibility Timeout**: Let's say the visibility timeout for MyQueue is set to 60 seconds. This means that when a consumer retrieves (or "receives") a message from MyQueue, that message will become invisible to other consumers for the next 60 seconds.

2. **Consumer Action**: A consumer application polls MyQueue for messages. It receives a message Message A from the queue.

3. **Invisibility Period**: From the moment Message A is received by the consumer, it becomes invisible to other consumers for the next 60 seconds. During this time, the consumer application processes Message A.

4. **Processing Time**: The consumer has up to 60 seconds to process Message A. If the consumer successfully processes the message within this time frame, it can delete (DeleteMessage API) the message from the queue once processing is complete.

5. **Visibility Timeout Expiry**: If the consumer fails to delete Message A within the visibility timeout period (60 seconds), SQS will make Message A visible in the queue again. This allows another consumer to retrieve and process the message.

6. **Handling Failures**: If the consumer application encounters an issue or crashes while processing Message A, SQS will make Message A visible again after the visibility timeout expires. This ensures that the message can be processed by another consumer.


## Conclusion
The visibility timeout feature of SQS ensures efficient message processing and fault tolerance in distributed systems. It allows messages to be processed without the risk of duplicate handling and provides a mechanism to manage message visibility based on the processing time needed by consumers.




