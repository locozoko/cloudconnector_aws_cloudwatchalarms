# cloudconnector-monitoring
Basic AWS Monitoring and Alerting for ☁️Zscaler Cloud Connectors☁️

=========================================================================

⚠️  **Attention:** This cloudformation template is not an official Zscaler template

Overview
------------

This CloudFormation template will deploy an SNS Topic with Email subscription, CloudWatch Alarms, and EventBridge Rules.
The default template assumes there is a Gateway Load Balancer with 2 Cloud Connectors.

An email notification will be sent if any of following alarms are triggered:

- Cloud Connector EC2 instance is stopped
- AWS GWLB to Cloud Connector Health Probe fails for 15 minutes
- Cloud Connector network throughput exceeds 500Mbps for 15 minutes
- Cloud Connector CPU utilization exceeds 70% for 15 minutes
- Cloud Connector EC2 Instance Check fails
- Cloud Connector EC2 System Check fails

How to Deploy
---------------------
Deploy the stack is simple. Deploy this stack for every GWLB Service you have, typically at a Region level.



1. Create a CloudFormation Stack with new resources
1. Provide a stack name that is logical such as zsccalarms-useast1
1. Select the zscc_aws_alarms.yaml CFT
1. Input the EC2 Instance ID of the first Cloud Connector
1. Input the EC2 Instance ID of the second Cloud Connector
1. Input the ARN of the Gateway Load Balancer Service
1. Input the ARN of the Load Balancer Target Group
1. Input the valid email address to receive the alerts
1. Input a topic name that is logical, such as ZSCC_Alarms_useast1
1. Optionally add tags as required by your organization
1. Create the stack
1. Confirm the subscription via the automated email that is sent from AWS

Additional Information
---------------------
If you need to modify the stack to add more Cloud Connectors per group:

- Copy the "Cloud Connector 2 Health Alerts" section and change all the references from 2 to 3
- Repeat for additional Cloud Connectors
- Copy the "CCInstance2" Parameter section and change the reference from 2 to 3
- Repeat for additional Cloud Connectors

If you do not wish to receive alerts if a Cloud Connector EC2 is stopped:

- Remove the "ZSCC1EC2EventRule" section
- Remove the "ZSCC2EC2EventRule section

If you wish to adjust the alarm threshold from 15 minutes (5 minute periods with 3 consecutives meeting threshold),
simply adjust the periods in all the relevent sections. If you adjust the throughput periods please be aware you will
also need to change the math formula associated otherwise you will have an incorrect measurement in Mbps.

Versioning
---------------------
1.0 - September 23 2022 - Initial version made available