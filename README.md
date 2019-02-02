# ECS optimized AMI

This package will create an Optimized ECS image which includes the following components:  
1. Latest security updates  
2. SSM agent  
3. AWS inspector agent  
4. Encrypted EBS store (optional)  
5. Customize the EBS volume size (optional)  

## 1. Usage

### 1.1 Setup AWS credentials
The package relies on an aws profile from your `~/.aws/credentials` file.  
Export the desired profile:  
```bash
export AWS_PROFILE='your profile name from the ~/.aws/credentials file'
```

### 1.2 Setup AMI requirements
Update the `variables.json` file with the correct values for your environment

| Variable | Description | Default Value |
| --- | --- | --- |
| instance_type | The instance type | t2.micro |
| aws_region | Specify the AWS region | "" |
| aws_vpc_id | Specify a VPC ID | "" |
| public_subnet_id | Specify a public subnet ID | "" |
| xvda_volume_size | The root volume size | 8 |
| xvdcz_volume_size | The data volume size | 22 |
| encrypted | Encrypt the data volume | "false" |
| ami_name | The AMI name | "allcloud-amzn-ecs-{{isotime}}" |


### 1.3 Validate your template
```bash
packer validate -var-file=variables.json packer.json
```
Varify you get the output:
```bash
$ packer validate -var-file=variables.json packer.json
$ Template validated successfully.
```

### 1.4 Create AMI
```bash
packer build -var-file=variables.json packer.json
```

Login to your AWS account and locate your AMI.

### 1.5 Next steps
Update your ECS cluster with the new AMI and perform rolling update  

**IMPORTANT**: Verifty that the ECS instance role includes the SSM agent and AWS inspector policies for it to work properly.  
* SSM agent policy: `AmazonEC2RoleforSSM` 
