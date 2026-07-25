# CloudWatch Logging

## Overview:
CloudWatch Logs allow you to monitor, store, and access log files from EC2 instances, applications, or other AWS services.

### Practical: Storing HTTPD Server Logs in Log Groups

1. **Install and Configure Apache HTTPD Server**
   - Install Apache:
     ```bash
     sudo yum install httpd -y  # For Amazon Linux/Red Hat
     ```
     ```
     sudo apt-get install apache2 -y  # For Ubuntu/Debian
     ```
   - Start the service:
     ```bash
     sudo systemctl start httpd
     sudo systemctl enable httpd
     ```

2. **Configure CloudWatch Agent for HTTPD Logs**
## cloudwatch agent 
Step 1: Download and install the CloudWatch agent 
```bash
wget https://amazoncloudwatch-agent.s3.amazonaws.com/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb
```
step 2: install the package with dpkg:
```bash
sudo dpkg -i -E ./amazon-cloudwatch-agent.deb
```
step 3: manual configuration 
first install collectd to colletc logs from aws to cloudwatch 
```
sudo apt update
sudo apt install collectd -y
```
step 4 : manual configuration 
```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
```

3. **Restart CloudWatch Agent**
   - Apply the updated configuration and restart the agent:
```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
-a fetch-config \
-m ec2 \
-s \
-c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json
```

Verify the agent's status:
```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status
```
4. **View Logs in CloudWatch**
   - Go to **CloudWatch Console** > **Log Groups**.
   - Select the log group (e.g., `httpd-access-logs`) to view and analyze the logs.

--- 

## cloudwatch --> monitoring and observability service provided by AWS.
components
- log group. --> log event
- metric. ---> metric data / numerical data point
- alarm. ---> alarm event driven by metric data point
- dashboard. ---> grafical representation of metrics
---

metrics -->  event driven by log data.

types of metrics
- standard metrics. --> provided by aws services. no cost.
- custom metrics. --> created by users.
    |--> cloudwatch agent. --> installed on ec2 instances to collect system level metrics and log data.

---
alarm --> event driven by metric data point.
70 % cpu utilization --> alarm triggered.
SNS service --> used to send notifications when an alarm is triggered.
 states 
- ok. --> metric data point is within the defined threshold.
- in alarm. --> metric data point is outside the defined threshold.
- insufficient data. --> not enough data to determine the state of the alarm.







dashboard --> grafical representation of metrics.
 multipal dashboard 
15 servers ---> dashboard

sale --> realtime services --> traffic --> dashboard

[cloudwatch_agent](./cloudwatch)







