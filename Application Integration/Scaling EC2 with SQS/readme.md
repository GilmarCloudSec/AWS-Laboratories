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

  








Retrieve the URL of the SQS queue named "MyMessages".
Optionally, print a command to send a single message.
Enter a loop that iterates 2000 times.
Inside each iteration:
Send a message with the body "Message-$i" (where $i is the current loop iteration) to the queue.

queueurl=$(aws sqs get-queue-url --queue-name "MyMessages"|jq ".QueueUrl"|sed "s/sqs.us-east-1.amazonaws.com/queue.amazonaws.com/g"|sed 's/"//g')
echo "queueurl:" $queueurl


echo "aws sqs send-message --queue-url $queueurl --message-body Message --region us-east-1"
for i in {1..2000} ;
    do
        aws sqs send-message --queue-url $queueurl --message-body "Message-$i" --region us-east-1 ;
done;






The receivemessages file contains the following code, and here is the overall flow:
Retrieve the URL of the SQS queue named "MyMessages".
Enter a loop that iterates 2000 times (or until the queue is empty).
Within each iteration:
Retrieve the first message from the queue.
Get the receipt handle of the message.
Delete the message using the receipt handle (the commented-out line would print the deletion command for verification).
Wait for 1 second before proceeding to the next iteration.
