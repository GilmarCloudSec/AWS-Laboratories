## Title
Integrating S3 (Simple Storage)  + with SNS (Simple Notification Service) + SQS (Simple Queue Service).


## Objectives
* Create a workflow where an S3 event triggers notifications to SQS via SNS whenever an object is uploaded to an S3 bucket. Both managers and developers should be notified.


## Services Used
* S3
* SNS
* SQS


## Implementation
1. Creating an S3 bucket:

    ![S3 Bucket](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/dfdbf3cebeb0aca66e6861991a470c02684653a3/Application%20Integration/S3%20%2B%20SNS%20%2B%20SQS/S3%20bucket.png)



2. Creating a SNS topic:
   Creating an SNS topic that will handle notifications from S3 and forward them to the SQS queues.

    ![S3 + SNS + SQS Topic](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/dfdbf3cebeb0aca66e6861991a470c02684653a3/Application%20Integration/S3%20%2B%20SNS%20%2B%20SQS/topic.png)




3. Creating two SQS queues:
   Both SQS queues will receive notifications from SNS.

    ![SQS Queues](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/dfdbf3cebeb0aca66e6861991a470c02684653a3/Application%20Integration/S3%20%2B%20SNS%20%2B%20SQS/SQS%20queues.png)



4. Subscribing SQS Queues to SNS Topic:
   Subscribing the SQS queues to the SNS topic so that messages published to the SNS topic are delivered to both SQS queues.

    ![Subscribing SQS Queues to SNS Topic](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/dfdbf3cebeb0aca66e6861991a470c02684653a3/Application%20Integration/S3%20%2B%20SNS%20%2B%20SQS/Subscribing%20SQS%20Queues%20to%20SNS%20Topic.png)




5. Configure S3 Event notification:
   Configuring the S3 bucket to send events to the SNS topic whenever objects are uploaded.
   * Events: All object create events.
   * Send to: "SNS Topic".
   * SNS Topic: topic.
  
     ![Configure S3 Event Notification](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/dfdbf3cebeb0aca66e6861991a470c02684653a3/Application%20Integration/S3%20%2B%20SNS%20%2B%20SQS/Configure%20S3%20Event%20notification.png)




6. Testing the integration:
   Uploading a file to the S3 bucket triggered S3 to publish an event notification to the SNS topic, which in turn delivers the message to both SQS queues.



Conclusion
This fan-out architecture ensures that multiple consumers can react to the same event, enabling scalable and decoupled systems for efficient data processing and workflow automation.




