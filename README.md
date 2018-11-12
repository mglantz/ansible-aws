# ansible-aws
Simple playbook to deploy VMs on Amazon

## Install Ansible

This setup was tested at the time of writing this README with Ansible version 2.6.
To install Ansible follow [Ansible install guidance](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) for your favourite OS, Linux :) .

## Ansible dynamic inventory

Before running the installer, you need to install boto Python modules on your Ansible machine using [the Ansible AWS documentation](http://docs.ansible.com/ansible/latest/scenario_guides/guide_aws.html).

## Set parameters to Ansible variables file

Playbooks expect file ```content/vars/vars.yml``` to contain settings your personal AWS credentials, machine AMI image and some other parameters. Copy ```content/vars/vars-example.yml``` and fill it with your settings. It is recommended to use Ansible Vault to encrypt your credentials.

## Encrypting credentials with Ansible Vault

Put password of your choice into a text file. In this example ```content/vault-password.txt```. Then you can use command ```ansible-vault --vault-password-file content/vault-password.txt encrypt_string``` to encrypt your credentials. The output can be used in the ```content/vars/vars.yml``` file, see example in ```content/vars/vars-example.yml```-file.

You can also put the credentials in plain text, but you should make sure that you don't commit them into any git repository! Files ```content/vars/vars.yml``` and ```content/vault-password.txt``` are ignored by git in this repository for your safety.

# Provision the lab environment

## Ansible run

Playbook ```provision-all.yml``` includes other playbooks to create all necessary resources into AWS, and configure each of them. It will use dynamic inventory provided by ```content/inventory/ec2.py```. That's why you need boto Python modules installed in addition to AWS credentials in ```content/vars/vars.yml```.

```
ansible-playbook --vault-password-file vault-password.txt -i inventory/ec2.py deploy-server.yml
```
