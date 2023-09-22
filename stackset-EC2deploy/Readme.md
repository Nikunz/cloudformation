# AWS CloudFormation Template for EC2 Instance with Security Group #

This CloudFormation template allows you to create an Amazon EC2 instance with an associated security group. The EC2 instance runs the Amazon Linux AMI, and the security group is configured to allow SSH access, HTTP traffic, and ICMP traffic. Please note that running this template will result in AWS resource charges.

## Template Details

- **AWSTemplateFormatVersion**: 2010-09-09
- **Description**: AWS CloudFormation Sample Template EC2InstanceWithSecurityGroupSample: Create an Amazon EC2 instance running the Amazon Linux AMI. The AMI is chosen based on the region in which the stack is run. This example creates an EC2 security group for the instance to give you SSH access. **WARNING** This template creates an Amazon EC2 instance. You will be billed for the AWS resources used if you create a stack from this template.

## Parameters
```
1. **KeyName**:
   - *Description*: Name of an existing EC2 KeyPair to enable SSH access to the instance.
   - *Type*: AWS::EC2::KeyPair::KeyName
   - *Default*: devops
   - *ConstraintDescription*: Must be the name of an existing EC2 KeyPair.

2. **InstanceType**:
   - *Description*: WebServer EC2 instance type.
   - *Type*: String
   - *Default*: t2.micro
   - *AllowedValues*: [ "t2.micro"]
   - *ConstraintDescription*: Must be a valid EC2 instance type.

3. **SSHLocation**:
   - *Description*: The IP address range that can be used to SSH to the EC2 instances.
   - *Type*: String
   - *MinLength*: 9
   - *MaxLength*: 18
   - *Default*: 0.0.0.0/0
   - *AllowedPattern*: Must be a valid IP CIDR range of the form x.x.x.x/x.
   - *ConstraintDescription*: Must be a valid IP CIDR range.

4. **HTTPLocation**:
   - *Description*: The IP address range that can be used for HTTP Traffic to the EC2 instances.
   - *Type*: String
   - *MinLength*: 9
   - *MaxLength*: 18
   - *Default*: 0.0.0.0/0
   - *AllowedPattern*: Must be a valid IP CIDR range of the form x.x.x.x/x.
   - *ConstraintDescription*: Must be a valid IP CIDR range.

5. **ICMPLocation**:
   - *Description*: The IP address range that can be used for ICMP Traffic to the EC2 instances.
   - *Type*: String
   - *MinLength*: 9
   - *MaxLength*: 18
   - *Default*: 0.0.0.0/0
   - *AllowedPattern*: Must be a valid IP CIDR range of the form x.x.x.x/x.
   - *ConstraintDescription*: Must be a valid IP CIDR range.

## Mappings

- **AWSInstanceType2Arch**: Mapping for EC2 instance types to their architecture.
- **AWSInstanceType2NATArch**: Mapping for EC2 instance types to their NAT architecture.
- **AWSRegionArch2AMI**: Mapping for region-specific AMIs based on architecture.

## Resources

1. **EC2Instance**:
   - *Type*: AWS::EC2::Instance
   - *Properties*: Configuration for the EC2 instance, including instance type, security group, and image ID.

2. **InstanceSecurityGroup**:
   - *Type*: AWS::EC2::SecurityGroup
   - *Properties*: Configuration for the security group, allowing SSH access (port 22), HTTP access (port 80), and ICMP traffic.

## Outputs

1. **InstanceId**:
   - *Description*: InstanceId of the newly created EC2 instance.
   - *Value*: [Ref](Ref) : "EC2Instance"

2. **AZ**:
   - *Description*: Availability Zone of the newly created EC2 instance.
   - *Value*: [Fn::GetAtt](Fn::GetAtt) : [ "EC2Instance", "AvailabilityZone" ]

3. **PublicDNS**:
   - *Description*: Public DNS Name of the newly created EC2 instance.
   - *Value*: [Fn::GetAtt](Fn::GetAtt) : [ "EC2Instance", "PublicDnsName" ]

4. **PublicIP**:
   - *Description*: Public IP address of the newly created EC2 instance.
   - *Value*: [Fn::GetAtt](Fn::GetAtt) : [ "EC2Instance", "PublicIp" ]
```

## How to Use

1. Provide values for the parameters:
   - **KeyName**: The name of an existing EC2 KeyPair.
   - **InstanceType**: Choose the EC2 instance type (default is t2.micro).
   - **SSHLocation**: Specify the IP address range for SSH access.
   - **HTTPLocation**: Specify the IP address range for HTTP traffic.
   - **ICMPLocation**: Specify the IP address range for ICMP traffic.

2. Deploy this CloudFormation template in your AWS account either via uploading the json file or from an s3 bucket, and it will create an EC2 instance with the specified configuration and security group.

3. Access the EC2 instance using the provided Public DNS or Public IP address.

4. Remember to delete the stack when you are done to avoid ongoing charges for the resources created by this template.
