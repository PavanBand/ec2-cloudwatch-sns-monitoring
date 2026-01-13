# ec2-cloudwatch-sns-monitoring
Project: Stress Testing an EC2 Instance using CloudWatch & SNS
Architecture Flow (Simple)

EC2 → CloudWatch Metrics → CloudWatch Alarm → SNS → Email Notification

STEP 1: Launch an EC2 Instance

Go to AWS Console → EC2

Click Launch Instance

Configure:

Name: cpu-stress-test-ec2

AMI: Amazon Linux 2

Instance type: t2.micro (Free Tier)

Key pair: Create or select existing

Security Group:

Allow SSH (22) from your IP

Click Launch Instance

STEP 2: Connect to EC2 Instance

From your local terminal (Git Bash / PowerShell / Linux):

ssh -i your-key.pem ec2-user@<EC2-Public-IP>


Once connected, update packages:

sudo yum update -y

STEP 3: Install Stress Tool (CPU Load Generator)

Install stress package:

sudo amazon-linux-extras install epel -y
sudo yum install stress -y


Verify installation:

stress --help

STEP 4: Generate High CPU Load

Run this command to stress CPU:

stress --cpu 2 --timeout 300


Explanation:

--cpu 2 → uses 2 CPU workers

--timeout 300 → runs for 5 minutes

This will push CPU usage above 80–90%.

STEP 5: Monitor CPU in CloudWatch

Go to AWS Console → CloudWatch

Click Metrics

Select:

EC2

Per-Instance Metrics

Choose:

CPUUtilization

Select your EC2 instance

You’ll see the CPU spike graph 📈

STEP 6: Create SNS Topic (Email Alerts)

Go to SNS → Topics

Click Create topic

Choose:

Type: Standard

Name: ec2-cpu-alerts

Create the topic

Add Email Subscription

Open the topic

Click Create subscription

Protocol: Email

Endpoint: your-email@gmail.com

Click Create subscription

Confirm the email from your inbox (IMPORTANT)

STEP 7: Create CloudWatch Alarm (CPU Threshold)

Go to CloudWatch → Alarms

Click Create alarm

Select metric:

EC2 → Per-Instance Metrics

CPUUtilization

Select your EC2 instance

Alarm Configuration

Statistic: Average

Period: 5 minutes

Threshold:

Greater than 70%

Notification

Select existing SNS topic: ec2-cpu-alerts

Finalize

Alarm name: High-CPU-Utilization-Alarm

Click Create alarm

STEP 8: Test the Alarm 🚨

Run stress command again:

stress --cpu 2 --timeout 300


Wait 5–10 minutes

Check:

CloudWatch alarm status → ALARM

Your email inbox → Alert received

✅ Alarm triggered successfully

STEP 9: Stop Stress Test

If running manually, stop using:

CTRL + C


Alarm will return to OK state, and you may get a recovery email.

STEP 10: Clean Up (Important)

To avoid charges:

Terminate EC2 instance

Delete:

CloudWatch alarm

SNS topic
