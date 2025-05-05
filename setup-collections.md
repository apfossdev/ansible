# Setup EC2 Collection and Authentication

## Install boto3

```
pip install boto3
```

## Install Azure/AWS Collection

https://galaxy.ansible.com/ui/repo/published/azure/azcollection/docs/
https://galaxy.ansible.com/ui/repo/published/azure/azcollection/

```
ansible-galaxy collection install azure.azcollection
ansible-galaxy collection install amazon.aws
```

## Setup Vault 

1. Create a password for vault

```
openssl rand -base64 2048 > vault.pass
```

2. Add your AWS credentials using the below vault command

```
ansible-vault create group_vars/all/pass.yml --vault-password-file vault.pass
```



--- 
- hosts: localhost
  connection: local
  tasks:
  - name: start an instance with a public IP address
    amazon.aws.ec2_instance:
      name: "ansible-instance"
      # key_name: "prod-ssh-key"
      # vpc_subnet_id: subnet-013744e41e8088axx
      instance_type: t2.micro
      security_group: default
      region: us-east-1
      aws_access_key: "{{ec2_access_key}}"  # From vault as defined
      aws_secret_key: "{{ec2_secret_key}}"  # From vault as defined      
      network:
        assign_public_ip: true
      image_id: ami-04b70fa74e45c3917
      tags:
        Environment: Testing

https://learn.microsoft.com/en-us/azure/developer/ansible/vm-configure?tabs=ansible




