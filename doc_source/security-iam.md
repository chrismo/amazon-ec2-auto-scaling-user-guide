# Identity and Access Management for Amazon EC2 Auto Scaling<a name="security-iam"></a>



AWS Identity and Access Management \(IAM\) is an AWS service that helps an administrator securely control access to AWS resources\. IAM administrators control who can be *authenticated* \(signed in\) and *authorized* \(have permissions\) to use Amazon EC2 Auto Scaling resources\. IAM is an AWS service that you can use with no additional charge\.

To use Amazon EC2 Auto Scaling, you need an AWS account and your specific user credentials for signing into your account\. For more information, see the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)\.

After your account is set up, you will need to obtain your AWS access keys if you want to access Amazon EC2 Auto Scaling through its API, whether by the Query \(HTTPS\) interface directly or indirectly through an [SDK](https://aws.amazon.com/tools/), the [AWS Command Line Interface](https://aws.amazon.com/cli/), or the [AWS Tools for Windows PowerShell](https://aws.amazon.com/powershell/)\. AWS access keys consist of an access key ID and a secret access key\. For more information about getting your AWS access keys, see [AWS security credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in the *AWS General Reference*\.



## Access control<a name="access-control"></a>

You can have valid credentials to authenticate your requests, but unless you have permissions you cannot create or access Amazon EC2 Auto Scaling resources\. For example, you must have permissions to create Auto Scaling groups, create launch configurations, and so on\. 

The following sections provide details on how an IAM administrator can use IAM to help secure your Amazon EC2 Auto Scaling resources, by controlling who can perform Amazon EC2 Auto Scaling actions\. 

We recommend that you read the Amazon EC2 topics first\. See [Identity and access management for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/security-iam.html) in the *Amazon EC2 User Guide for Linux Instances*\. After reading the topics in this section, you should have a good idea what access control permissions Amazon EC2 offers and how they can fit in with your Amazon EC2 Auto Scaling resource permissions\.

**Topics**
+ [Access control](#access-control)
+ [How Amazon EC2 Auto Scaling works with IAM](control-access-using-iam.md)
+ [AWS managed policies for Amazon EC2 Auto Scaling](security-iam-awsmanpol.md)
+ [Service\-linked roles for Amazon EC2 Auto Scaling](autoscaling-service-linked-role.md)
+ [Amazon EC2 Auto Scaling identity\-based policy examples](security_iam_id-based-policy-examples.md)
+ [Launch template support](ec2-auto-scaling-launch-template-permissions.md)
+ [IAM role for applications that run on Amazon EC2 instances](us-iam-role.md)
+ [Required AWS KMS key policy for use with encrypted volumes](key-policy-requirements-EBS-encryption.md)