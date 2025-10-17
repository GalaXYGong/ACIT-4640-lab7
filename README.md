# Lab 7 - Xinyu Gong, John Yin, Chris Poon

This document outlines the steps and commands required to set up the AWS infrastructure using Terraform, configure the web servers using Ansible, and ensure proper cleanup.

-----

## Create new Key

This command generates the ED25519 SSH key pair needed to securely connect to the AWS EC2 instances.

```sh
# create new key pair for AWS access
ssh-keygen -t ed25519 -f ~/.ssh/aws
```

-----

## Import new Key to AWS and Delete new Key

These commands use the provided scripts to manage the public key on the AWS platform.

```sh
# import key using script to add public key to AWS account
./scripts/import_lab_key ~/.ssh/aws.pub
# delete key using script to remove key from AWS account (Cleanup step)
./scripts/delete_lab_key aws
```

-----

## Terraform Commands

These commands are run inside the `terraform` directory to provision the EC2 instances.

```sh
# go to terraform directory
cd terraform 
# initialize the terraform working directory
terraform init
# format terraform files for consistency
terraform fmt
# validate terraform files for syntax and structure
terraform validate
# plan terraform changes and save to 'plan' file
terraform plan --out plan
# apply terraform changes to create 2 EC2 instances
terraform apply plan
```

-----

## Ansible Commands

The public IP/DNS names are in `ansible/inventory/hosts.yml` before these commands are run.

```sh
# go to ansible directory
cd ../ansible
# validate YAML syntax and formatting (best practice)
yamllint playbook.yml inventory/hosts.yml
# check ansible playbook for syntax errors
ansible-playbook --syntax-check "playbook.yml"
# run the playbook to configure NGINX on the servers
ansible-playbook "playbook.yml"
```

-----

## Screenshot of Rendered HTML Page

The following screenshot confirms that NGINX is installed and the custom index page deployed via the Ansible template is successfully rendered.

![Nginx Site Running](image.png)

-----

## Clean up Terraform Created Resources (To Prevent Charges)

```sh
# go to terraform directory
cd ../terraform 
# destroy all AWS resources created by Terraform
terraform destroy 
```
