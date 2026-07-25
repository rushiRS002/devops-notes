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
   - Update the CloudWatch Agent configuration to include Apache logs:
     ```bash
     sudo nano /opt/aws/amazon-cloudwatch-agent/bin/config.json
     ```
   - Example configuration:
  ```json
     {
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/var/log/syslog",
            "log_group_name": "access-logs",
            "log_stream_name": "{instance_id}"
          },
          {
            "file_path": "/var/log/syslog",
            "log_group_name": "error-logs",
            "log_stream_name": "{instance_id}"
          }
        ]
      }
    },
    "log_stream_name": "default-log-stream"
  }
}
```

3. **Restart CloudWatch Agent**
   - Apply the updated configuration and restart the agent:
     ```bash
     sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
         -a fetch-config \
         -m ec2 \
         -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json \
         -s
     ```
Verify the agent's status:
```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status
```
4. **View Logs in CloudWatch**
   - Go to **CloudWatch Console** > **Log Groups**.
   - Select the log group (e.g., `httpd-access-logs`) to view and analyze the logs.
