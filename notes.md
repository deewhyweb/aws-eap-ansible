## Deploy app through cli
scp -i "~/Downloads/eap-demo.pem" ./target/helloworld.war  ec2-user@ec2-75-101-250-189.compute-1.amazonaws.com:/home/ec2-user

sudo su -
cd /opt/rh/eap7/root/usr/share/wildfly/bin
[root@ip-172-31-9-156 bin]# ./jboss-cli.sh --connect
[standalone@localhost:9990 /] deploy /home/ec2-user/helloworld.war





ssh -i "~/Downloads/eap-demo.pem" ec2-user@ec2-75-101-250-189.compute-1.amazonaws.com

sudo su -

echo "JAVA_OPTS=\"$JAVA_OPTS -Djboss.bind.address=0.0.0.0 -Djboss.bind.address.private=0.0.0.0 -Djboss.bind.address.management=0.0.0.0\"" >> /opt/rh/eap7/root/usr/share/wildfly/bin/standalone.conf


systemctl start  eap7-standalone

## Add admin user


/opt/rh/eap7/root/usr/share/wildfly/bin/add-user.sh



## setup db

```
yum install wget

cd /opt
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-8.0.17.tar.gz

tar -xvzf mysql-connector-java-8.0.17.tar.gz

export WILDFLY_HOME=/opt/rh/eap7/root/usr/share/wildfly 

mkdir -p $WILDFLY_HOME/modules/system/layers/base/com/mysql/main
cp /opt/mysql-connector-java-8.0.17/mysql-connector-java-8.0.17.jar $WILDFLY_HOME/modules/system/layers/base/com/mysql/main/

vi $WILDFLY_HOME/modules/system/layers/base/com/mysql/main/module.xml

```

```
<module xmlns="urn:jboss:module:1.5" name="com.mysql">
    <resources>
        <resource-root path="mysql-connector-java-8.0.17.jar" />
    </resources>
    <dependencies>
        <module name="javax.api"/>
        <module name="javax.transaction.api"/>
    </dependencies>
</module>
```

```

systemctl restart  eap7-standalone

yum install mysql

mysql -h  database-1.cvaumqh0xefq.us-east-1.rds.amazonaws.com -u admin -p

create database eap;

$WILDFLY_HOME/bin/jboss-cli.sh --connect

/subsystem=datasources/jdbc-driver=mysql:add(driver-name=mysql,driver-module-name=com.mysql)

data-source add --name=mysql --jndi-name=java:/jdbc/mysql --driver-name=mysql --connection-url=jdbc:mysql://database-1.cvaumqh0xefq.us-east-1.rds.amazonaws.com:3306/eap --user-name=admin --password=password



```

```

scp -i /home/philip/Downloads/eap-demo.pem ./target/kitchensink.war  ec2-user@ec2-34-205-39-136.compute-1.amazonaws.com:/home/ec2-user



sudo su -
cd /opt/rh/eap7/root/usr/share/wildfly/bin
[root@ip-172-31-9-156 bin]# ./jboss-cli.sh --connect
[standalone@localhost:9990 /] deploy /home/ec2-user/kitchensink.war

```

pass: BH676!!h88



## Add admin user
/opt/rh/eap7/root/usr/share/wildfly/bin/add-user.sh

## Spin up with cli
aws configure import --csv file://~/Downloads/philip_accessKeys.csv 
https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-config

t2-medium
us-east-1

AMI: RHEL-7-JBEAP-7.4.0_HVM_GA-20220804-x86_64-0-Marketplace-GP2-98e76783-8c82-4afd-827e-cbc8e27edf0d
ami-0e3314c4e353d52d5

security group id sg-0aa1d5adc2deca94d


aws configure

aws ec2 create-security-group --group-name eap-security --description "EAP security group"

aws ec2 authorize-security-group-ingress --group-id sg-0245222eefcf8e9c3 --protocol tcp --port 22 --cidr 64.222.231.57/32
aws ec2 authorize-security-group-ingress --group-id sg-0245222eefcf8e9c3 --protocol tcp --port 8080 --cidr 64.222.231.57/32
aws ec2 authorize-security-group-ingress --group-id sg-0245222eefcf8e9c3 --protocol tcp --port 9990 --cidr 64.222.231.57/32

aws ec2 authorize-security-group-ingress --group-id $GROUPID --protocol tcp --port 22 --cidr $IP/32
aws ec2 authorize-security-group-ingress --group-id $GROUPID --protocol tcp --port 8080 --cidr $IP/32
aws ec2 authorize-security-group-ingress --group-id $GROUPID --protocol tcp --port 9990 --cidr $IP/32


aws ec2 run-instances --image-id ami-0e3314c4e353d52d5 --count 1 --instance-type t2.medium --key-name eap --security-group-ids sg-0aa1d5adc2deca94d

aws ec2 describe-instances

aws ec2 describe-instances --instance-ids i-08e30a66a3492d3fa --query 'Reservations[*].Instances[*].PublicIpAddress' --output text
aws ec2 describe-instances --instance-ids i-08e30a66a3492d3fa --query 'Reservations[*].Instances[*].PublicDnsName' --output text
aws ec2 create-key-pair --region us-east-1 --key-name "eap-key" | jq -r ".KeyMaterial" > ~/.ssh/eap-key.pem

export PUBLICDNS=$(aws ec2 describe-instances \
--query "Reservations[*].Instances[*].PublicDnsName"  \
--filters "Name=instance-state-name,Values=running" "Name=image-id,Values='ami-0e3314c4e353d52d5'" \
--output text)

echo "JAVA_OPTS=\"$JAVA_OPTS -Djboss.bind.address=0.0.0.0 -Djboss.bind.address.private=0.0.0.0 -Djboss.bind.address.management=0.0.0.0\"" >> /opt/rh/eap7/root/usr/share/wildfly/bin/standalone.conf


aws ec2 create-subnet \
    --vpc-id vpc-062d51ef4bf5679b4 \
    --cidr-block 10.0.0.0/24 



aws ec2 describe-images --filters 'Name=name,Values=RHEL-7-JBEAP*' 'Name=description,Values="Provided by Red Hat, Inc."' --query "Images[*].ImageId" --output text

aws ec2 describe-images --filters 'Name=name,Values=RHEL-7-JBEAP*'  --query "Images[*].ImageId" --output text