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
Create and upload objects to an S3 bucket:

I created a bucket and uploaded two files: "sendmessages" and "receivemessages".




The sendmessages file contains the following code, and here is the overall flow:
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
queueurl=$(aws sqs get-queue-url --queue-name "MyMessages"|jq ".QueueUrl"|sed "s/sqs.us-east-1.amazonaws.com/queue.amazonaws.com/g"|sed 's/"//g')
echo "queueurl:" $queueurl


for i in {1..2000};
do
    # receiptHandle is required to delete the message from SQS after the message is retreived
    receiptHandle=$(aws sqs receive-message --queue-url $queueurl|jq .Messages[0].ReceiptHandle|sed 's/"//g')
    #echo "receiptHandle:" $receiptHandle


    #echo -e "aws sqs delete-message --queue-url $queueurl --receipt-handle $receiptHandle --region us-east-1"
    aws sqs delete-message --queue-url $queueurl --receipt-handle $receiptHandle --region us-east-1;


    echo "Sleep for 1 second..."
    sleep 1
done;



Create an SQS Queue:
I created a queue named MyMessages with a standard type.



Upload and run a script on a Cloud9 Instance:
I selected an EC2 instance running on a Linux platform for the environment type.


After uploading the `sendMessages.sh` file to the environment, I executed `sudo yum install jq -y` in the terminal to install dependencies, followed by `chmod +x sendMessages.sh` to grant execute permissions to the script. Finally, I ran the script to send 2000 messages to the MyMessages queue.

The script sends 2000 messages to the MyMessages queue.



Messages are being received.


Create CloudWatch alarms:
The CloudWatch metric is SQS > Queue metrics > ApproximateNumberOfMessagesVisible.

ScaleOut alarm:
Metric: ApproximateNumberOfMessagesVisible
Condition: > 500
























ScaleIn alarm:
Metric: ApproximateNumberOfMessagesVisible
Condition: < 300








Create an Auto Scaling Group:
I configured an Auto Scaling group for ScaleIn and ScaleOut based on the CloudWatch alarms. The desired capacity is set to 1, with a minimum capacity of 1 and a maximum capacity of 4.











Verify and Monitor EC2 Instances:

The SQS queue contains 2000 messages.



The "ScaleOut" alarm was triggered as the ApproximateNumberOfMessagesVisible metric exceeded 500.



The Auto Scaling group has been triggered, launching four instances.




The SQS queue contains fewer than 300 messages.



The "ScaleIn" alarm triggered because the ApproximateNumberOfMessagesVisible metric fell below 300.



Conclusion
In this implementation, I utilized SQS, CloudWatch, and Auto Scaling Groups to construct a dynamic and scalable architecture. The system adeptly manages load by adjusting the number of EC2 instances according to the message volume in the SQS queue, ensuring efficient processing and optimal resource utilization.




