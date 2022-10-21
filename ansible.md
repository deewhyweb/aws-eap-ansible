pip3 install boto
pip3 install boto3

AKIAZ4DZGOJM35MU73UL

vYVXIWcu9cKwpBRkks+kQmd9fsBCZJIrlazo+0rd

ansible-galaxy collection install community.aws
ansible-galaxy collection install amazon.aws:==3.3.1 --force

 ansible-playbook eap-instance.yml --connection=local --tags=ec2-create