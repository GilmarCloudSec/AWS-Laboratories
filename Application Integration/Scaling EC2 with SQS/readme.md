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
   * The sendmessages file contains the following code, and here is the overall flow:
     * Retrieve the URL of the SQS queue named "MyMessages".
     * Optionally, print a command to send a single message.
     * Enter a loop that iterates 2000 times.
       * Inside each iteration:

  






