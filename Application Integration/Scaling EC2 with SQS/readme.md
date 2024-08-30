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


2. Create an SQS Queue:

   I created a queue named MyMessages with a standard type.

   ![Creating an SQS Queue](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/525736e1033c9e19ae6b0699e6f7df4b4e18ac1b/Application%20Integration/Scaling%20EC2%20with%20SQS/Create%20an%20SQS%20Queue.png)



3. Upload and run a script on a Cloud9 Instance:
   * I selected an EC2 instance running on a Linux platform for the environment type.
  
     ![Cloud9 Instance](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/525736e1033c9e19ae6b0699e6f7df4b4e18ac1b/Application%20Integration/Scaling%20EC2%20with%20SQS/Cloud9%20Instance.png)

  
   
   * After uploading the `sendMessages.sh` file to the environment, I executed `sudo yum install jq -y` in the terminal to install dependencies, followed by `chmod +x sendMessages.sh` to grant execute permissions to the script. Finally, I ran the script to send 2000 messages to the MyMessages queue.
  
     ![Sending Messages](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/abdd4ea13bf61ef81dc621712199087acb4045c2/Application%20Integration/Scaling%20EC2%20with%20SQS/sending%20messages.png)
  

     The image below shows that the messages are being received.
     
     ![Messages Received](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/8bc4072baaa8601079c7005400f883cca5c3ac0a/Application%20Integration/Scaling%20EC2%20with%20SQS/messages%20received.png)



4. Create CloudWatch alarms:
   The CloudWatch metric is SQS > Queue metrics > ApproximateNumberOfMessagesVisible.
    * ScaleOut alarm:
      Metric: ApproximateNumberOfMessagesVisible
      Condition: > 500

      ![Scale Out Alarm](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/075b8ff5d56ceb0776298ee66f88761bf10c1a4d/Application%20Integration/Scaling%20EC2%20with%20SQS/scale%20out%20alarm.png)



    * ScaleIn alarm:
      Metric: ApproximateNumberOfMessagesVisible
      Condition: < 300

      ![Scale In Alarm](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/bd5b217944cd9106de75a837c99a9ec162c14bd2/Application%20Integration/Scaling%20EC2%20with%20SQS/scale%20in%20alarm.png)


5. Create an Auto Scaling Group:
   I configured an Auto Scaling group for ScaleIn and ScaleOut based on the CloudWatch alarms. The desired capacity is set to 1, with a minimum capacity of 1 and a maximum capacity of 4.

   ![Scaling Group](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/ba9f6ce91b3e2e04ca5a65478b2ad1356fbfff2a/Application%20Integration/Scaling%20EC2%20with%20SQS/scaling%20group.png)


6. Verify and Monitor EC2 Instances:

   The SQS queue contains 2000 messages.
   
   ![SQS Queue](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/9fb4b8d6db37cdd286d609d33afe5f70f8bad00c/Application%20Integration/Scaling%20EC2%20with%20SQS/SQS%20queue.png)
   

   The "ScaleOut" alarm was triggered as the ApproximateNumberOfMessagesVisible metric exceeded 500.

   ![Alarm Scale Out](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/9fb4b8d6db37cdd286d609d33afe5f70f8bad00c/Application%20Integration/Scaling%20EC2%20with%20SQS/alarm%20scale%20OUT.png)

   
   The Auto Scaling group has been triggered, launching four instances.

   ![EC2 Instances](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/9fb4b8d6db37cdd286d609d33afe5f70f8bad00c/Application%20Integration/Scaling%20EC2%20with%20SQS/instances.png)


   The SQS queue contains fewer than 300 messages.

   ![Updated SQS Queue](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/9fb4b8d6db37cdd286d609d33afe5f70f8bad00c/Application%20Integration/Scaling%20EC2%20with%20SQS/SQS%20queue%20updated.png)



   The "ScaleIn" alarm triggered because the ApproximateNumberOfMessagesVisible metric fell below 300.

   ![Scale-In Alarm](https://raw.githubusercontent.com/GilmarCloudSec/AWS-Laboratories/9fb4b8d6db37cdd286d609d33afe5f70f8bad00c/Application%20Integration/Scaling%20EC2%20with%20SQS/alarm%20scale%20IN.png)


## Conclusion
In this implementation, I utilized SQS, CloudWatch, and Auto Scaling Groups to construct a dynamic and scalable architecture. The system adeptly manages load by adjusting the number of EC2 instances according to the message volume in the SQS queue, ensuring efficient processing and optimal resource utilization.










