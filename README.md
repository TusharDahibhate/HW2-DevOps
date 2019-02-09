# CSC 519- DevOps Homework 2 - Configuration Management

### Student Name: Tushar Himmat Dahibhate
### Unity Id: tdahibh

This file will describe the deliverables for Homework 2.

This repository contains ansible playbooks for configuring Mattermost server.

## Tools used:
Baker - Used for VM provisioning

- ansible-server/baker.yml contains the IP address and configuration details of the ansible server

- mattermost-server/baker.yml contains the IP address and configuration details of the mattermost server

## Setting variables:
Before running the ansible playbook, the following variables need to be set first:(Default values have been provided for reference)

- Location: vars/main.yml - https://github.ncsu.edu/tdahibh/HW2-Devops/blob/master/ansible-server/vars/main.yml

```bash
mysql_databases:
  name: mattermost  # Database name
  
mysql_users:
  name: mmuser # Mattermost sql user name
  host: "%"
  priv: "mattermost.*:ALL"

NotificationFromAddress: gmail.com
SMTPServerUserName: test.mattermost.user@gmail.com  # Username of the SMTP server
SMTPServer: mail.gmail.com
SMTPServerPort: 465
ConnectionSecurity: TLS
SiteURL: https://mattermost.example.com

mattermost_user:
  firstname: Test   # Mattermost user first name
  lastname: User    # Mattermost user last name
  email: test.mattermost.user@gmail.com   # Mattermost user email
  username: Test    # Mattermost user username

mattermost_team:    
  name: testteam1   # Mattermost team name
  display_name: "Test_team"  
  email: "test.mattermost.user@gmail.com" # Mattermost user email
  
```

Once these variable have been set, we will need to set the secrets. They include mysql root passwords and passwords of the mattermost user and server. (This repo contains an encrypted secrets.yml. The user will have to create another secrets.yml)

- Location: vars/secrets.yml - https://github.ncsu.edu/tdahibh/HW2-Devops/blob/master/ansible-server/vars/secrets.yml

```bash

mysql_root_password: 
mysql_user_password: 
SMTPServerUserPassword: 
mattermost_userpassword: 

```

# Roles

Following roles have been defined in this repository

1. init - This role will simply execute the apt update command
2. mysql - This role handles the installation and configuration of mysql server
3. mattermost - This role handles the installation, configuration, user and team creation for mattermost

# Steps to execute Ansible Playbook
1. Go to the mattermost-server folder and execute:

```bash
cd /mattermost-server/
$ baker bake
```

This will spawn the mattermost server

2. Go to the ansible-server folder and execute:

```bash
$ cd /ansible-server/
$ baker bake
```

This will spawn the ansible server

3. After setting the variables and secrets, we will encrypt the secrets.yml file
```bash
$ ansible-vault encrypt vars/secrets.yml
```
It will then prompt you for the password.
After setting the password, the secrets.yml file will be encrypted

4. To execute the playbooks

```bash
$ cd /ansible-server/
$ ansible-playbook --vault-id @prompt -i inventory main.yml
```
It will prompt for the password in order to use the secrets file.

After entering the password, the playbook will start its execution.


4. To access the mattermost UI, go to the browser and type  192.168.33.100:8065. 
This is the IP of the mattermost server



# Screencast:
https://youtu.be/Gu8WXWaNJbA

