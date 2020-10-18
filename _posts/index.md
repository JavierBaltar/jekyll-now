---
title: AWS Config
date: "2019-10-26T15:46:37.121Z"
description: Continuously monitors and records your AWS resource configurations, automating the evaluation of recorded configurations against desired configurations. 

--- 

[AWS Config](https://aws.amazon.com/config/) provides a detailed view of the AWS resources configuration in your AWS account. This way, you track how the resources are related and how they were configured, evaluating how the configurations change over time. 

- Track state of all resources(incuding EC2 OS level). 
- Meet compliance standards (i.e. PCI compliance). 
- Validate configuration against AWS Config Rules. 

## Pricing

With AWS Config, you are charged based on the number of configuration items recorded and the number of active AWS Config rule evaluations in your account. Take a glance of the AWS Config pricing site for further details at https://aws.amazon.com/config/pricing/

## Getting Started

Go to AWS console, find the AWS Config service and click on Get started.
  
![](./aws-config-dashboard.png)
#
NOTE: AWS Config is not a global service. You need to enable it on a per region basis. 

Firstly, you have to enable the audit and recording for each of the resource you are planning to track. Thus, click on Settings and select the type or resources. There is an option to enable all of them or select specific ones. 

![](./aws-config-resource-types-to-record.png)

In my case, I am tracking IAM roles, S3 buckets and Lambda functions resources only. 

Select a destination S3 bucket, which stores the configuration history related to the selected resources. 
Optionally, you can stream configuration changes and notifications to an AWS SNS topic. 

![](./aws-config-s3-bucket.png)

AWS Config creates a role, which is granted to audit the resources recorded. Finally, click on Confirm. 
#
![](./aws-config-review.png)

After few seconds, the recording in on as shown below. 
#
![](./aws-config-recorder.png)

The AWS Config dashboard displays the resources and the compliance status. Up to this moment, the resources are being tracked and the configuration history is stored in S3 but there are no rules set.  

![](./aws-config-compliance-status.png)

The next step is to create Rules for validating a given configuration. Click on Rules, add rule. 
#
![](./aws-config-rules.png)

There are two types of rules:
- AWS managed rules preconfigured by AWS with an extensive suite of options. 
- Custom rules for anyone in need of conditions on a more granular basis. 


![](./aws-config-specify-rule-type.png)

Select the rules which fit for your environment and Click on Add rule. 

![](./aws-config-review-rule.png)

In order to maximise the potential of this powerful service once a rule is configured,is attaching remeditation actions, which will be automatically triggered when the rule is not met. 
Select a rule (i.e. s3-bucket-public-read-prohibited), click on Actions and Manage remediation. 
# 
![](./aws-config-manage-remediation.png)

In this case, I have selected an automatic remediation. 

![](./aws-config-edit-remediation.png)

There are several actions available. For demo purposes, I am just selecting "AWS-ConfigureS3BucketLogging". This remediation consist of enabling logging for buckets that are public readable. 

![](./aws-config-remediation-actions.png)

The moment a S3 bucket is created or configured with public readable, the rule is not met and the remediation will be triggered enabling logging on that bucket. 

Before concluding this post, I wish to suggest you to take a glance of the AWS Config concepts and give a try on this AWS service https://docs.aws.amazon.com/config/latest/developerguide/config-concepts.html






