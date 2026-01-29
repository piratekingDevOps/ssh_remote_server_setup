SSH Remote Server Setup

Project URL:
https://roadmap.sh/projects/ssh-remote-server-setup

Overview

This project demonstrates how to set up a remote Linux server and configure it to allow SSH access using multiple SSH key pairs. The goal is to securely connect to the server using either key and simplify access using an SSH config alias.

The remote server for this project was created as a Linux EC2 instance on AWS using the AWS Console.

Server Setup

Cloud provider: AWS

Service used: EC2

OS: Linux (Amazon Linux / Ubuntu)

Authentication method: SSH key-based authentication

An EC2 instance was launched via the AWS Console with SSH (port 22) allowed in the security group.

SSH Key Pair Creation

Two separate SSH key pairs were created locally:

ssh-keygen -t ed25519 -f ~/.ssh/key_one
ssh-keygen -t ed25519 -f ~/.ssh/key_two


This generated the following files:

key_one and key_one.pub

key_two and key_two.pub

Adding SSH Keys to the Server

Connected to the EC2 instance using the initial AWS-provided key.

Created (or updated) the authorized_keys file:

mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys


Added the contents of both public keys:

key_one.pub
key_two.pub


Set correct permissions:

chmod 600 ~/.ssh/authorized_keys

Connecting to the Server Using Both Keys

Verified SSH access using both private keys:

ssh -i ~/.ssh/key_one user@server-ip
ssh -i ~/.ssh/key_two user@server-ip


Both commands successfully authenticated and logged into the server.

SSH Config Setup

To simplify connections, an SSH config alias was added.

Edited the SSH config file:

nano ~/.ssh/config


Added the following configuration:

Host aws-server
    HostName server-ip
    User user
    IdentityFile ~/.ssh/key_one


Now the server can be accessed using:

ssh aws-server


The identity file can be switched to key_two if required.

Stretch Goal: Fail2Ban Installation

Fail2Ban was installed to protect against brute force SSH attacks.

sudo apt update
sudo apt install fail2ban -y


Enabled and started the service:

sudo systemctl enable fail2ban
sudo systemctl start fail2ban


Verified status:

sudo systemctl status fail2ban
