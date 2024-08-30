## Title
SNS (Simple Notification Service) and SQS (Simple Queue Service) = FAN-OUT


## Objectives
* Explain how SNS and SQS can be integrated to achieve a fan-out messaging pattern.
* Demonstrate how this integration ensures no data loss and fully decoupled services.


## Implementation
SNS and SQS can be combined to create a robust and scalable fan-out messaging pattern. This pattern allows a single message to be distributed to multiple consumers, each of which can process the message independently.
* The message sent to the SNS topic "OrderNotifications" is automatically distributed to both subscribed SQS queues ("InventoryUpdates" and "ShippingUpdates").
* Each SQS queue receives and stores the message independently, ensuring no data loss.
* Consumers (e.g., EC2 instances, Lambda functions) can then poll these SQS queues to process the messages asynchronously.

1. Create a SNS Topic:
I created a topic named OrderNotifications and chose "Standard" as the type.


2. Create SQS Queues:
2.1 Create the First SQS Queue:
I created a queue named InventoryUpdatesand and chose "Standard" as the type.


2.2 Create the Second SQS Queue:
I created a queue named ShippingUpdates and chose "Standard" as the type.


3. Subscribe both SQS Queues to the SNS Topic:
The queues InventoryUpdatesand and ShippingUpdates were subscribed to the OrderNotifications SNS topic.




4. Send Messages to the SNS Topic:
Here is an example of the message that will be sent to both SQS queues:



5. Receiving the messages on the SQS queues:





## Conclusion
Integrating Amazon SNS with Amazon SQS using a fan-out pattern provides a scalable and reliable way to distribute messages to multiple consumers. This setup ensures no data loss and allows services to remain fully decoupled, enhancing the overall robustness and flexibility of the system. By utilizing SNS for message distribution and SQS for message storage and processing, you can build efficient, event-driven architectures that handle various use cases, from order processing to event notifications.



