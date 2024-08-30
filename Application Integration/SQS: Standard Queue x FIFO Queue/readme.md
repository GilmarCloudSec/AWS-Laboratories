## Title
Simple Queue Service (SQS): Standard Queue x FIFO Queue

## Objective
Create a Standard Queue and FIFO Queue to observe and understand the differences in message delivery behavior.

## Implementation

#### Standard Queue
* Messages might be delivered in a different order than they were sent.
* A message is delivered at least once, but sometimes multiple copies are delivered.

  Sending Messages:

   ![Sending Messages Standard](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Labs/e11b24572792ac1a0d02879ecd28572dd654c114/Application%20Integration/SQS%3A%20Standard%20Queue%20x%20FIFO%20Queue/Sending%20Messages%20Standard.png)


  Receiving Messages:

  ![Receiving Messages Standard](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Labs/e11b24572792ac1a0d02879ecd28572dd654c114/Application%20Integration/SQS%3A%20Standard%20Queue%20x%20FIFO%20Queue/Receiving%20Messages%20Standard.png)




#### FIFO Queue
* The order in which messages are sent and received are preserved
* Duplicates are not introduced into the queue

  Sending Messages:

  ![Sending Messages FIFO](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Labs/e11b24572792ac1a0d02879ecd28572dd654c114/Application%20Integration/SQS%3A%20Standard%20Queue%20x%20FIFO%20Queue/Sending%20Messages%20FIFO.png)


  Receiving Messages:
  
  ![Receiving Messages FIFO](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Labs/e11b24572792ac1a0d02879ecd28572dd654c114/Application%20Integration/SQS%3A%20Standard%20Queue%20x%20FIFO%20Queue/Receiving%20Messages%20FIFO.png)



## Conclusion
By setting up both a Standard Queue and a FIFO Queue, we observed the key differences between them:
* Standard Queue: Provides at-least-once delivery with no guarantee of message order, which may result in multiple copies of a message and out-of-order delivery.
* FIFO Queue: Ensures exactly-once delivery while preserving the order of messages, making it ideal for use cases where the order of operations is critical.





