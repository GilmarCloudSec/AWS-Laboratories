## Title
Scaling EC2 with Simple Queue Service (SQS).


## Objectives
* Create and upload objects to an s3 bucket
* Create an SQS Queue
* Upload and run a script on a Cloud9 Instance
* Create CloudWatch alarms
* Create an Auto Scaling Group
* Verify and Monitor EC2 Instances


## Services Used
* S3
* SQS
* Cloud9
* CloudWatch
* EC2
* Auto Scaling Group


## Implementation
1. Create and upload objects to an S3 bucket:

   * I created a bucket and uploaded two files: "sendmessages" and "receivemessages".
     
     ![Creating and Uploading Objects to an S3 Bucket](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/15f03fa196157ddc78061e5b64d4de04e78a50cd/Application%20Integration/Scaling%20EC2%20with%20SQS/Create%20and%20upload%20objects%20to%20an%20S3%20bucket.png)

   * The sendmessages file contains the following code, and here is the overall flow:
     * Retrieve the URL of the SQS queue named "MyMessages".
     * Optionally, print a command to send a single message.
     * Enter a loop that iterates 2000 times.
       * Inside each iteration:

         ![Sending Messages Code](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/15f03fa196157ddc78061e5b64d4de04e78a50cd/Application%20Integration/Scaling%20EC2%20with%20SQS/sendmessages%20code.png)


  * The receivemessages file contains the following code, and here is the overall flow:
    * Retrieve the URL of the SQS queue named "MyMessages".
    * Enter a loop that iterates 2000 times (or until the queue is empty).
    * Within each iteration:
    * Retrieve the first message from the queue.
    * Get the receipt handle of the message.
    * Delete the message using the receipt handle (the commented-out line would print the deletion command for verification).
    * Wait for 1 second before proceeding to the next iteration.

      ![Receiving Messages Code](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/15f03fa196157ddc78061e5b64d4de04e78a50cd/Application%20Integration/Scaling%20EC2%20with%20SQS/receivemessages%20code.png)








