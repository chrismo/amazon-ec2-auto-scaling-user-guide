# Amazon EC2 Auto Scaling User Guide

-----
*****Copyright &copy; Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in
connection with any product or service that is not Amazon's,
in any manner that is likely to cause confusion among customers,
or in any manner that disparages or discredits Amazon. All other
trademarks not owned by Amazon are the property of their respective
owners, who may or may not be affiliated with, connected to, or
sponsored by Amazon.

-----
## Contents
+ [What is Amazon EC2 Auto Scaling?](what-is-amazon-ec2-auto-scaling.md)
   + [Amazon EC2 Auto Scaling benefits](auto-scaling-benefits.md)
   + [Amazon EC2 Auto Scaling instance lifecycle](ec2-auto-scaling-lifecycle.md)
   + [Quotas for Amazon EC2 Auto Scaling](ec2-auto-scaling-quotas.md)
+ [Set up Amazon EC2 Auto Scaling](setting-up.md)
+ [Get started with Amazon EC2 Auto Scaling](get-started-with-ec2-auto-scaling.md)
+ [Launch templates](launch-templates.md)
   + [Create a launch template for an Auto Scaling group](create-launch-template.md)
   + [Copy launch configurations to launch templates](copy-launch-config.md)
   + [Replace a launch configuration with a launch template](replace-launch-config.md)
   + [Request Spot Instances for fault-tolerant and flexible applications](launch-template-spot-instances.md)
   + [Examples for creating and managing launch templates with the AWS Command Line Interface (AWS CLI)](examples-launch-templates-aws-cli.md)
+ [Launch configurations](launch-configurations.md)
   + [Create a launch configuration](create-launch-config.md)
   + [Create a launch configuration using an EC2 instance](create-lc-with-instanceID.md)
   + [Change the launch configuration for an Auto Scaling group](change-launch-config.md)
   + [Configure instance tenancy with a launch configuration](auto-scaling-dedicated-instances.md)
+ [Auto Scaling groups](auto-scaling-groups.md)
   + [Auto Scaling groups with multiple instance types and purchase options](ec2-auto-scaling-mixed-instances-groups.md)
      + [Configure overrides](ec2-auto-scaling-configuring-overrides.md)
         + [Specify a different launch template for an instance type](ec2-auto-scaling-mixed-instances-groups-launch-template-overrides.md)
         + [Configure instance weighting for Amazon EC2 Auto Scaling](ec2-auto-scaling-mixed-instances-groups-instance-weighting.md)
   + [Create an Auto Scaling group using attribute-based instance type selection](create-asg-instance-type-requirements.md)
   + [Create an Auto Scaling group using a launch template](create-asg-launch-template.md)
   + [Create an Auto Scaling group using a launch configuration](create-asg-launch-configuration.md)
   + [Create an Auto Scaling group using parameters from an existing instance](create-asg-from-instance.md)
   + [Create an Auto Scaling group using the Amazon EC2 launch wizard](create-asg-ec2-wizard.md)
   + [Tag Auto Scaling groups and instances](ec2-auto-scaling-tagging.md)
   + [Replace Auto Scaling instances](ec2-auto-scaling-group-replacing-instances.md)
      + [Replace Auto Scaling instances based on an instance refresh](asg-instance-refresh.md)
      + [Check the status of an instance refresh](check-status-instance-refresh.md)
      + [Instance refresh examples that enable skip matching with the AWS Command Line Interface (AWS CLI)](asg-instance-refresh-skip-matching.md)
      + [Add checkpoints to an instance refresh](asg-adding-checkpoints-instance-refresh.md)
      + [Replace Auto Scaling instances based on maximum instance lifetime](asg-max-instance-lifetime.md)
   + [Delete your Auto Scaling infrastructure](as-process-shutdown.md)
+ [Scale the size of your Auto Scaling group](scale-your-group.md)
   + [Set capacity limits on your Auto Scaling group](asg-capacity-limits.md)
   + [Maintain a fixed number of instances in your Auto Scaling group](as-maintain-instance-levels.md)
   + [Manual scaling for Amazon EC2 Auto Scaling](as-manual-scaling.md)
      + [Attach EC2 instances to your Auto Scaling group](attach-instance-asg.md)
      + [Detach EC2 instances from your Auto Scaling group](detach-instance-asg.md)
   + [Dynamic scaling for Amazon EC2 Auto Scaling](as-scale-based-on-demand.md)
      + [Target tracking scaling policies for Amazon EC2 Auto Scaling](as-scaling-target-tracking.md)
      + [Step and simple scaling policies for Amazon EC2 Auto Scaling](as-scaling-simple-step.md)
      + [Set default values for instance warmup or scaling cooldown](set-default-values-for-instance-warmup-or-scaling-cooldown.md)
         + [Set the default instance warmup for an Auto Scaling group](ec2-auto-scaling-default-instance-warmup.md)
         + [Scaling cooldowns for Amazon EC2 Auto Scaling](ec2-auto-scaling-scaling-cooldowns.md)
         + [Available warm-up and cooldown settings](consolidated-view-of-warm-up-and-cooldown-settings.md)
      + [Scaling based on Amazon SQS](as-using-sqs-queue.md)
      + [Verify a scaling activity for an Auto Scaling group](as-verify-scaling-activity.md)
      + [Disable a scaling policy for an Auto Scaling group](as-enable-disable-scaling-policy.md)
      + [Delete a scaling policy](deleting-scaling-policy.md)
      + [Example scaling policies for the AWS Command Line Interface (AWS CLI)](examples-scaling-policies.md)
   + [Predictive scaling for Amazon EC2 Auto Scaling](ec2-auto-scaling-predictive-scaling.md)
      + [Explore your data and forecast](predictive-scaling-graphs.md)
      + [Override forecast values using scheduled actions](predictive-scaling-overriding-forecast-capacity.md)
      + [Advanced predictive scaling policy configurations using custom metrics](predictive-scaling-customized-metric-specification.md)
   + [Scheduled scaling for Amazon EC2 Auto Scaling](ec2-auto-scaling-scheduled-scaling.md)
   + [Amazon EC2 Auto Scaling lifecycle hooks](lifecycle-hooks.md)
      + [How lifecycle hooks work](lifecycle-hooks-overview.md)
      + [Prepare to add a lifecycle hook to your Auto Scaling group](prepare-for-lifecycle-notifications.md)
         + [Retrieve the target lifecycle state through instance metadata](retrieving-target-lifecycle-state-through-imds.md)
      + [Add lifecycle hooks](adding-lifecycle-hooks.md)
      + [Complete a lifecycle action](completing-lifecycle-hooks.md)
      + [Tutorial: Configure user data to retrieve the target lifecycle state through instance metadata](tutorial-lifecycle-hook-instance-metadata.md)
      + [Tutorial: Configure a lifecycle hook that invokes a Lambda function](tutorial-lifecycle-hook-lambda.md)
   + [Warm pools for Amazon EC2 Auto Scaling](ec2-auto-scaling-warm-pools.md)
      + [Use lifecycle hooks with a warm pool](warm-pool-instance-lifecycle.md)
      + [View health check status and the reason for health check failures](warm-pools-health-checks-monitor-view-status.md)
      + [Examples for creating and managing warm pools with the AWS CLI](examples-warm-pools-aws-cli.md)
   + [Control which Auto Scaling instances terminate during scale in](as-instance-termination.md)
      + [Work with Amazon EC2 Auto Scaling termination policies](ec2-auto-scaling-termination-policies.md)
      + [Create a custom termination policy with Lambda](lambda-custom-termination-policy.md)
      + [Use instance scale-in protection](ec2-auto-scaling-instance-protection.md)
   + [Temporarily remove instances from your Auto Scaling group](as-enter-exit-standby.md)
   + [Suspend and resume a process for an Auto Scaling group](as-suspend-resume-processes.md)
+ [Monitor your Auto Scaling instances and groups](as-monitoring-features.md)
   + [Health checks for Auto Scaling instances](ec2-auto-scaling-health-checks.md)
   + [AWS Health Dashboard notifications for Amazon EC2 Auto Scaling](monitoring-personal-health-dashboard.md)
   + [Monitor CloudWatch metrics for your Auto Scaling groups and instances](ec2-auto-scaling-cloudwatch-monitoring.md)
      + [Configure monitoring for Auto Scaling instances](enable-as-instance-metrics.md)
      + [View monitoring graphs in the Amazon EC2 Auto Scaling console](viewing-monitoring-graphs.md)
   + [Log Amazon EC2 Auto Scaling API calls with AWS CloudTrail](logging-using-cloudtrail.md)
   + [Get Amazon SNS notifications when your Auto Scaling group scales](ec2-auto-scaling-sns-notifications.md)
+ [AWS services integrated with Amazon EC2 Auto Scaling](ec2-auto-scaling-integrations.md)
   + [Use Capacity Rebalancing to handle Amazon EC2 Spot Interruptions](ec2-auto-scaling-capacity-rebalancing.md)
   + [Create Auto Scaling groups with AWS CloudFormation](creating-auto-scaling-groups-with-cloudformation.md)
   + [Create Auto Scaling groups from the command line using AWS CloudShell](create-auto-scaling-groups-with-cloudshell.md)
   + [Use AWS Compute Optimizer to get recommendations for the instance type for an Auto Scaling group](asg-getting-recommendations.md)
   + [Use Elastic Load Balancing to distribute traffic across the instances in your Auto Scaling group](autoscaling-load-balancer.md)
      + [Prerequisites for getting started with Elastic Load Balancing](getting-started-elastic-load-balancing.md)
      + [Attach a load balancer to your Auto Scaling group](attach-load-balancer-asg.md)
      + [Understand the attachment status of your load balancer](load-balancer-status.md)
      + [Configure an Application Load Balancer or Network Load Balancer from the Amazon EC2 Auto Scaling console](as-create-load-balancer-console.md)
      + [Add Elastic Load Balancing health checks to an Auto Scaling group](as-add-elb-healthcheck.md)
      + [Add and remove Availability Zones](as-add-availability-zone.md)
      + [Examples for working with Elastic Load Balancing with the AWS Command Line Interface (AWS CLI)](examples-elastic-load-balancing-aws-cli.md)
      + [Tutorial: Set up a scaled and load-balanced application](tutorial-ec2-auto-scaling-load-balancer.md)
   + [Use EventBridge to handle Auto Scaling events](automating-ec2-auto-scaling-with-eventbridge.md)
      + [Amazon EC2 Auto Scaling event reference](ec2-auto-scaling-event-reference.md)
      + [Warm pool event types and patterns](warm-pools-eventbridge-events.md)
      + [Create EventBridge rules](create-eventbridge-rules.md)
         + [Create EventBridge rules for instance refresh events](monitor-events-eventbridge-sns.md)
         + [Create EventBridge rules for warm pool events](warm-pool-events-eventbridge-rules.md)
   + [Provide network connectivity for your Auto Scaling instances using Amazon VPC](asg-in-vpc.md)
+ [Security in Amazon EC2 Auto Scaling](security.md)
   + [Amazon EC2 Auto Scaling and data protection](ec2-auto-scaling-data-protection.md)
   + [Identity and Access Management for Amazon EC2 Auto Scaling](security-iam.md)
      + [How Amazon EC2 Auto Scaling works with IAM](control-access-using-iam.md)
      + [AWS managed policies for Amazon EC2 Auto Scaling](security-iam-awsmanpol.md)
      + [Service-linked roles for Amazon EC2 Auto Scaling](autoscaling-service-linked-role.md)
      + [Amazon EC2 Auto Scaling identity-based policy examples](security_iam_id-based-policy-examples.md)
      + [Launch template support](ec2-auto-scaling-launch-template-permissions.md)
      + [IAM role for applications that run on Amazon EC2 instances](us-iam-role.md)
      + [Required AWS KMS key policy for use with encrypted volumes](key-policy-requirements-EBS-encryption.md)
   + [Compliance validation for Amazon EC2 Auto Scaling](ec2-auto-scaling-compliance.md)
   + [Resilience in Amazon EC2 Auto Scaling](disaster-recovery-resiliency.md)
   + [Infrastructure security in Amazon EC2 Auto Scaling](infrastructure-security.md)
   + [Amazon EC2 Auto Scaling and interface VPC endpoints](ec2-auto-scaling-vpc-endpoints.md)
+ [Troubleshoot Amazon EC2 Auto Scaling](CHAP_Troubleshooting.md)
   + [Troubleshoot Amazon EC2 Auto Scaling: EC2 instance launch failures](ts-as-instancelaunchfailure.md)
   + [Troubleshoot Amazon EC2 Auto Scaling: AMI issues](ts-as-ami.md)
   + [Troubleshoot Amazon EC2 Auto Scaling: Load balancer issues](ts-as-loadbalancer.md)
   + [Troubleshoot Amazon EC2 Auto Scaling: Launch templates](ts-as-launch-template.md)
   + [Troubleshoot Amazon EC2 Auto Scaling: Health checks](ts-as-healthchecks.md)
+ [Amazon EC2 Auto Scaling resources](as-resources.md)
+ [Document history](DocumentHistory.md)