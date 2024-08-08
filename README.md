# memory_utilization
memory_utilization
AWS EC2 Monitoring with Cloudwatch | Monitor Memory Utilization using CloudWatch | AWS CloudWatch Demo
Steps:
Step 1: Create an IAM and Attach CloudWatch and SSM Full Access - EC2-CloudWatch-Role
Step 2: Create a parameter in Systems Manger with the name "/alarm/AWS-CWAgentLinConfig" and store the value.
Step 3: Create an EC2 Instance, Attach the role created in Step 1 and Add the commands in the Userdata Section.
Commands that needs to be added in Userdata Section:
Code:
#!/bin/bash
wget https://s3.amazonaws.com/amazoncloudwatch-agent/linux/amd64/latest/AmazonCloudWatchAgent.zip
unzip AmazonCloudWatchAgent.zip
sudo ./install.sh
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c ssm:/alarm/AWS-CWAgentLinConfig -s
Check if EC2 Instance has CWAgent Installed or not:
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status
Value for the SSM Parameter (/alarm/AWS-CWAgentLinConfig):
{
	"metrics": {
		"append_dimensions": {
			"InstanceId": "${aws:InstanceId}"
		},
		"metrics_collected": {
			"mem": {
				"measurement": [
					"mem_used_percent"
				],
				"metrics_collection_interval": 60
			}
		}
	}
}
