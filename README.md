# EC2 Monitoring and Alerting using CloudWatch and SNS

## Project Overview
This project demonstrates how to stress test an Amazon EC2 instance and configure monitoring and alerting using Amazon CloudWatch and Amazon SNS.

## Architecture
EC2 Instance → CloudWatch Metrics → CloudWatch Alarm → SNS → Email Notification

## AWS Services Used
- Amazon EC2
- Amazon CloudWatch
- Amazon SNS
- Amazon Linux 2

## Implementation Steps
1. Launched an EC2 instance (t2.micro).
2. Installed stress tool to simulate high CPU usage.
3. Monitored CPU utilization using CloudWatch metrics.
4. Created CloudWatch alarm for CPU utilization > 70%.
5. Integrated SNS topic with email subscription.
6. Verified alert notification by triggering high CPU usage.

## Commands Used
```bash
sudo yum update -y
sudo amazon-linux-extras install epel -y
sudo yum install stress -y
stress --cpu 2 --timeout 600
