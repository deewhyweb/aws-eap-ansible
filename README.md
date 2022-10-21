ansible-galaxy collection install -r ./requirements.yml
ansible-playbook eap-instance.yml -i ./hosts.yml --tags=ec2-create