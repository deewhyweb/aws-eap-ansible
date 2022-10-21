## Demonstration of deploying instances of PAYG JBoss EAP to AWS EC2 with Ansible

This sample playbook will create two instances of EAP on AWS EC2 and deploy a simple helloworld.war file.

## Prerequisites

* An AWS account
* a key-pair with the pem file referenced in hosts.yml https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#KeyPairs:
* an AWS security group allowing inbound access from allowed ip addresses (e.g. your own ip) on ports 22, 8080 https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#SecurityGroups
* a vpc-subnet, configured to automatically assign ip address and publis dns name. https://us-east-1.console.aws.amazon.com/vpc/home?region=us-east-1#Home:


## Steps
Create a user and access keys at: https://us-east-1.console.aws.amazon.com/iamv2/home#/users

Set the keys as local environment variables:

```
 export AWS_ACCESS_KEY=aws_key_from_aws
 export AWS_SECRET_ACCESS_KEY=aws_secret_access_key_from_aws
```

update the variables in eap-instance.yml with your own aws security group, key pair name, and vpc-subnet id - see prerequisites

Install ansible dependencies

```ansible-galaxy collection install -r ./requirements.yml```

Run the playbook

```ansible-playbook eap-instance.yml -i ./hosts.yml```

Once the playbook is created the helloworld application should be available on:

http://ec2-instance-public-dns:8080/helloworld