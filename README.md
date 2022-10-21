## demonstration of deploying instances of PARG JBoss EAP to AWS EC2

This sample playbook will create two instances of EAP on AWS EC2 and deploy a simple helloworld.war file.

## Pre-requisites

* An AWS account
* a key-pair created on aws with the pem file referenced in hosts.yml
* an AWS security group allowing inbound access on ports 22, 8080, and 9990
* a vpc-subnet, configured to automatically assign ip address and publis dns name.


## Steps

Install ansible dependencies

```ansible-galaxy collection install -r ./requirements.yml```

Run the playbook

```ansible-playbook eap-instance.yml -i ./hosts.yml --tags=ec2-create```

Once the playbook is created the helloworld application should be available on:

http://ec2-instance-public-dns:8080/helloworld