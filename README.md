## Demonstration of deploying instances of PAYG JBoss EAP to AWS EC2 with Ansible

This sample playbook will create two instances of EAP on AWS EC2 and deploy a simple helloworld.war file.

## Prerequisites

* An AWS account
* You must subscribe to the Red Hat JBoss EAP marketplace offer in the aws marketplace, and accept the terms and conditions: https://aws.amazon.com/marketplace/search/results?searchTerms=jboss
* a key-pair with the pem file referenced in hosts.yml https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#KeyPairs:



## Steps
Create a user and access keys at: https://us-east-1.console.aws.amazon.com/iamv2/home#/users 

For the purpose of this demonstration we created a user with admin permissions, more fine grained permissions should be used and this repo will be updated with instructions on how to create these.

Copy the access keys from the AWS console and set them as local environment variables:

```
 export AWS_ACCESS_KEY=aws_key_from_aws
 export AWS_SECRET_ACCESS_KEY=aws_secret_access_key_from_aws
```

The playbook will create the required vpc, internet gateway, subnet, and security groups.  You can provide these as variables at runtime e.g.:

* aws_vpc_id
* aws_security_group
* vpc_subnet_id

Install ansible dependencies

```ansible-galaxy collection install -r ./requirements.yml```

Run the playbook

```ansible-playbook eap-instance.yml -i ./hosts.yml```

Once the playbook is created the helloworld application should be available on:

http://ec2-instance-public-dns:8080/helloworld