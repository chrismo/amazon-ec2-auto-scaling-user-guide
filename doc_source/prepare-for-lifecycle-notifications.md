# Prepare to add a lifecycle hook to your Auto Scaling group<a name="prepare-for-lifecycle-notifications"></a>

Before you add a lifecycle hook to your Auto Scaling group, be sure that your user data script or notification target is set up correctly\.
+ To use a user data script to perform custom actions on your instances as they are launching, you do not need to configure a notification target\. However, you must have already created the launch template or launch configuration that specifies your user data script and associated it with your Auto Scaling group\. For more information about user data scripts, see [Run commands on your Linux instance at launch](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html) in the *Amazon EC2 User Guide for Linux Instances*\. 
+ To signal Amazon EC2 Auto Scaling when the lifecycle action is complete, you must add the [CompleteLifecycleAction](https://docs.aws.amazon.com/autoscaling/ec2/APIReference/API_CompleteLifecycleAction.html) API call to the script, and you must manually create an IAM role with a policy that allows Auto Scaling instances to call this API\. Your launch template or launch configuration must specify this role using an IAM instance profile that gets attached to your Amazon EC2 instances at launch\. For more information, see [Complete a lifecycle action](completing-lifecycle-hooks.md) and [IAM role for applications that run on Amazon EC2 instances](us-iam-role.md)\.
+ To use a service such as Lambda to perform a custom action, you must have already created an EventBridge rule and specified a Lambda function as its target\. For more information, see [Configure a notification target for lifecycle notifications](#lifecycle-hook-notification-target)\.
+ To allow Lambda to signal Amazon EC2 Auto Scaling when the lifecycle action is complete, you must add the [CompleteLifecycleAction](https://docs.aws.amazon.com/autoscaling/ec2/APIReference/API_CompleteLifecycleAction.html) API call to the function code\. You must also have attached an IAM policy to the function's execution role that gives Lambda permission to complete lifecycle actions\. For more information, see [Tutorial: Configure a lifecycle hook that invokes a Lambda function](tutorial-lifecycle-hook-lambda.md)\.
+ To use a service such as a Amazon SNS or Amazon SQS to perform a custom action, you must have already created the SNS topic or SQS queue and have ready its Amazon Resource Name \(ARN\)\. You must also have already created the IAM role that gives Amazon EC2 Auto Scaling access to your SNS topic or SQS target and have ready its ARN\. For more information, see [Configure a notification target for lifecycle notifications](#lifecycle-hook-notification-target)\. 
**Note**  
By default, when you add a lifecycle hook in the console, Amazon EC2 Auto Scaling sends lifecycle event notifications to Amazon EventBridge\. Using EventBridge or a user data script is a recommended best practice\. To create a lifecycle hook that sends notifications directly to Amazon SNS or Amazon SQS, use the AWS CLI, AWS CloudFormation, or an SDK to add the lifecycle hook\.

## Configure a notification target for lifecycle notifications<a name="lifecycle-hook-notification-target"></a>

You can add lifecycle hooks to an Auto Scaling group to perform custom actions when an instance enters a wait state\. You can choose a target service to perform these actions depending on your preferred development approach\.

The first approach uses Amazon EventBridge to invoke a Lambda function that performs the action you want\. The second approach involves creating an Amazon Simple Notification Service \(Amazon SNS\) topic to which notifications are published\. Clients can subscribe to the SNS topic and receive published messages using a supported protocol\. The last approach involves using Amazon Simple Queue Service \(Amazon SQS\), a messaging system used by distributed applications to exchange messages through a polling model\.

As a best practice, we recommend that you use EventBridge\. The notifications sent to Amazon SNS and Amazon SQS contain the same information as the notifications that Amazon EC2 Auto Scaling sends to EventBridge\. Before EventBridge, the standard practice was to send a notification to SNS or SQS and integrate another service with SNS or SQS to perform programmatic actions\. Today, EventBridge gives you more options for which services you can target and makes it easier to handle events using serverless architecture\. 

The following procedures cover how to set up your notification target\.

Remember, if you have a user data script in your launch template or launch configuration that configures your instances when they launch, you do not need to receive notifications to perform custom actions on your instances\.

**Topics**
+ [Route notifications to Lambda using EventBridge](#cloudwatch-events-notification)
+ [Receive notifications using Amazon SNS](#sns-notifications)
+ [Receive notifications using Amazon SQS](#sqs-notifications)
+ [Notification message example](#notification-message-example)

**Important**  
The EventBridge rule, Lambda function, Amazon SNS topic, and Amazon SQS queue that you use with lifecycle hooks must always be in the same Region where you created your Auto Scaling group\.

### Route notifications to Lambda using EventBridge<a name="cloudwatch-events-notification"></a>

You can configure an EventBridge rule to invoke a Lambda function when an instance enters a wait state\. Amazon EC2 Auto Scaling emits a lifecycle event notification to EventBridge about the instance that is launching or terminating and a token that you can use to control the lifecycle action\. For examples of these events, see [Amazon EC2 Auto Scaling event reference](ec2-auto-scaling-event-reference.md)\.

**Note**  
When you use the AWS Management Console to create an event rule, the console automatically adds the IAM permissions necessary to grant EventBridge permission to call your Lambda function\. If you are creating an event rule using the AWS CLI, you need to grant this permission explicitly\.   
For information about how to create event rules in the EventBridge console, see [Creating Amazon EventBridge rules that react to events](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-create-rule.html) in the *Amazon EventBridge User Guide*\.  
– or –   
For an introductory tutorial that is directed towards console users, see [Tutorial: Configure a lifecycle hook that invokes a Lambda function](tutorial-lifecycle-hook-lambda.md)\. This tutorial shows you how to create a simple Lambda function that listens for launch events and writes them out to a CloudWatch Logs log\.

**To create an EventBridge rule that invokes a Lambda function**

1. Create a Lambda function by using the [Lambda console](https://console.aws.amazon.com/lambda/home#/functions) and note its Amazon Resource Name \(ARN\)\. For example, `arn:aws:lambda:region:123456789012:function:my-function`\. You need the ARN to create an EventBridge target\. For more information, see [Getting started with Lambda](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html) in the *AWS Lambda Developer Guide*\.

1. To create a rule that matches events for instance launch, use the following [put\-rule](https://docs.aws.amazon.com/cli/latest/reference/events/put-rule.html) command\.

   ```
   aws events put-rule --name my-rule --event-pattern file://pattern.json --state ENABLED
   ```

   The following example shows the `pattern.json` for an instance launch lifecycle action\. Replace the text in **italics** with the name of your Auto Scaling group\.

   ```
   {
     "source": [ "aws.autoscaling" ],
     "detail-type": [ "EC2 Instance-launch Lifecycle Action" ],
     "detail": {
         "AutoScalingGroupName": [ "my-asg" ]
      }
   }
   ```

   If the command runs successfully, EventBridge responds with the ARN of the rule\. Note this ARN\. You'll need to enter it in step 4\.

   To create a rule that matches for other events, modify the event pattern\. For more information, see [Use EventBridge to handle Auto Scaling events](automating-ec2-auto-scaling-with-eventbridge.md)\.

1. To specify the Lambda function to use as a target for the rule, use the following [put\-targets](https://docs.aws.amazon.com/cli/latest/reference/events/put-targets.html) command\.

   ```
   aws events put-targets --rule my-rule --targets Id=1,Arn=arn:aws:lambda:region:123456789012:function:my-function
   ```

   In the preceding command, *my\-rule* is the name that you specified for the rule in step 2, and the value for the `Arn` parameter is the ARN of the function that you created in step 1\.

1. To add permissions that allow the rule to invoke your Lambda function, use the following Lambda [add\-permission](https://docs.aws.amazon.com/cli/latest/reference/lambda/add-permission.html) command\. This command trusts the EventBridge service principal \(`events.amazonaws.com`\) and scopes permissions to the specified rule\.

   ```
   aws lambda add-permission --function-name my-function --statement-id my-unique-id \
     --action 'lambda:InvokeFunction' --principal events.amazonaws.com --source-arn arn:aws:events:region:123456789012:rule/my-rule
   ```

   In the preceding command:
   + *my\-function* is the name of the Lambda function that you want the rule to use as a target\.
   + *my\-unique\-id* is a unique identifier that you define to describe the statement in the Lambda function policy\.
   + `source-arn` is the ARN of the EventBridge rule\.

   If the command runs successfully, you receive output similar to the following\.

   ```
   {
     "Statement": "{\"Sid\":\"my-unique-id\",
       \"Effect\":\"Allow\",
       \"Principal\":{\"Service\":\"events.amazonaws.com\"},
       \"Action\":\"lambda:InvokeFunction\",
       \"Resource\":\"arn:aws:lambda:us-west-2:123456789012:function:my-function\",
       \"Condition\":
         {\"ArnLike\":
           {\"AWS:SourceArn\":
            \"arn:aws:events:us-west-2:123456789012:rule/my-rule\"}}}"
   }
   ```

   The `Statement` value is a JSON string version of the statement that was added to the Lambda function policy\.

1. After you have followed these instructions, continue on to [Add lifecycle hooks](adding-lifecycle-hooks.md) as a next step\.

### Receive notifications using Amazon SNS<a name="sns-notifications"></a>

You can use Amazon SNS to set up a notification target \(an SNS topic\) to receive notifications when a lifecycle action occurs\. Amazon SNS then sends the notifications to the subscribed recipients\. Until the subscription is confirmed, no notifications published to the topic are sent to the recipients\. 

**To set up notifications using Amazon SNS**

1. Create an Amazon SNS topic by using either the [Amazon SNS console](https://console.aws.amazon.com/sns/) or the following [create\-topic](https://docs.aws.amazon.com/cli/latest/reference/sns/create-topic.html) command\. Ensure that the topic is in the same Region as the Auto Scaling group that you're using\. For more information, see [Getting started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/sns-getting-started.html) in the *Amazon Simple Notification Service Developer Guide*\. 

   ```
   aws sns create-topic --name my-sns-topic
   ```

1. Note the topic Amazon Resource Name \(ARN\), for example, `arn:aws:sns:region:123456789012:my-sns-topic`\. You need it to create the lifecycle hook\.

1. Create an IAM service role to give Amazon EC2 Auto Scaling access to your Amazon SNS notification target\.

    **To give Amazon EC2 Auto Scaling access to your SNS topic** 

   1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

   1. In the navigation pane on the left, choose **Roles**\.

   1. Choose **Create role**\.

   1. For **Select trusted entity**, choose **AWS service**\.

   1. For your use case, under **Use cases for other AWS services**, choose **EC2 Auto Scaling** and then **EC2 Auto Scaling Notification Access**\.

   1. Choose **Next** twice to go to the **Name, review, and create** page\.

   1. For **Role name**, enter a name for the role \(for example, **my\-notification\-role**\) and choose **Create role**\.

   1. On the **Roles** page, choose the role that you just created to open the **Summary** page\. Make a note of the role **ARN**\. For example, `arn:aws:iam::123456789012:role/my-notification-role`\. You need it to create the lifecycle hook\.

1. After you have followed these instructions, continue on to [Add lifecycle hooks \(AWS CLI\)](adding-lifecycle-hooks.md#adding-lifecycle-hooks-aws-cli) as a next step\.

### Receive notifications using Amazon SQS<a name="sqs-notifications"></a>

You can use Amazon SQS to set up a notification target to receive messages when a lifecycle action occurs\. A queue consumer must then poll an SQS queue to act on these notifications\.

**Important**  
FIFO queues are not compatible with lifecycle hooks\.

**To set up notifications using Amazon SQS**

1. Create an Amazon SQS queue by using the [Amazon SQS console](https://console.aws.amazon.com/sqs/)\. Ensure that the queue is in the same Region as the Auto Scaling group that you're using\. For more information, see [Getting started with Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-getting-started.html) in the *Amazon Simple Queue Service Developer Guide*\. 

1. Note the queue ARN, for example, `arn:aws:sqs:us-west-2:123456789012:my-sqs-queue`\. You need it to create the lifecycle hook\.

1. Create an IAM service role to give Amazon EC2 Auto Scaling access to your Amazon SQS notification target\.

    **To give Amazon EC2 Auto Scaling access to your SQS queue** 

   1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

   1. In the navigation pane on the left, choose **Roles**\.

   1. Choose **Create role**\.

   1. For **Select trusted entity**, choose **AWS service**\.

   1. For your use case, under **Use cases for other AWS services**, choose **EC2 Auto Scaling** and then **EC2 Auto Scaling Notification Access**\.

   1. Choose **Next** twice to go to the **Name, review, and create** page\.

   1. For **Role name**, enter a name for the role \(for example, **my\-notification\-role**\) and choose **Create role**\.

   1. On the **Roles** page, choose the role that you just created to open the **Summary** page\. Make a note of the role **ARN**\. For example, `arn:aws:iam::123456789012:role/my-notification-role`\. You need it to create the lifecycle hook\.

1. After you have followed these instructions, continue on to [Add lifecycle hooks \(AWS CLI\)](adding-lifecycle-hooks.md#adding-lifecycle-hooks-aws-cli) as a next step\.

### Notification message example for Amazon SNS and Amazon SQS<a name="notification-message-example"></a>

While the instance is in a wait state, a message is published to the Amazon SNS or Amazon SQS notification target\. The message includes the following information:
+ `LifecycleActionToken` — The lifecycle action token\.
+ `AccountId` — The AWS account ID\.
+ `AutoScalingGroupName` — The name of the Auto Scaling group\.
+ `LifecycleHookName` — The name of the lifecycle hook\.
+ `EC2InstanceId` — The ID of the EC2 instance\.
+ `LifecycleTransition` — The lifecycle hook type\.
+ `NotificationMetadata` — The notification metadata\.

The following is a notification message example\.

```
Service: AWS Auto Scaling
Time: 2021-01-19T00:36:26.533Z
RequestId: 18b2ec17-3e9b-4c15-8024-ff2e8ce8786a
LifecycleActionToken: 71514b9d-6a40-4b26-8523-05e7ee35fa40
AccountId: 123456789012
AutoScalingGroupName: my-asg
LifecycleHookName: my-hook
EC2InstanceId: i-0598c7d356eba48d7
LifecycleTransition: autoscaling:EC2_INSTANCE_LAUNCHING
NotificationMetadata: hook message metadata
```

#### Test notification message example<a name="test-notification-message-example"></a>

When you first add a lifecycle hook, a test notification message is published to the notification target\. The following is a test notification message example\.

```
Service: AWS Auto Scaling
Time: 2021-01-19T00:35:52.359Z
RequestId: 18b2ec17-3e9b-4c15-8024-ff2e8ce8786a
Event: autoscaling:TEST_NOTIFICATION
AccountId: 123456789012
AutoScalingGroupName: my-asg
AutoScalingGroupARN: arn:aws:autoscaling:us-west-2:123456789012:autoScalingGroup:042cba90-ad2f-431c-9b4d-6d9055bcc9fb:autoScalingGroupName/my-asg
```

**Note**  
For examples of the events delivered from Amazon EC2 Auto Scaling to EventBridge, see [Amazon EC2 Auto Scaling event reference](ec2-auto-scaling-event-reference.md)\.