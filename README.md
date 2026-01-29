🔐 SSH Remote Server Setup

📍 Project URL
https://roadmap.sh/projects/ssh-remote-server-setup

🌍 Project Overview

This project is about bringing a remote Linux server to life and teaching it how to recognize trusted visitors. Using SSH key-based authentication, the server is configured to allow secure access using multiple SSH keys, with optional defenses against unwanted guests.

The server for this project was launched as a Linux EC2 instance on AWS using the AWS Console ☁️.

🖥️ Server Details

☁️ Cloud Provider: AWS

🧱 Service: EC2

🐧 Operating System: Linux

🔑 Authentication: SSH (Key-Based)

The EC2 security group allows inbound SSH traffic on port 22, enabling secure remote access.

🗝️ Creating SSH Key Pairs

Two separate SSH key pairs were generated locally to allow multiple trusted identities:

ssh-keygen -t ed25519 -f ~/.ssh/key_1
ssh-keygen -t ed25519 -f ~/.ssh/key_2


This resulted in:

key_1 / key_1.pub

key_2 / key_2.pub

Each key represents a different trusted way into the server 🚪.

🧩 Adding SSH Keys to the Server

Connected to the EC2 instance using the default AWS key pair.

Switched to the correct user:

sudo su - ec2-user


Prepared the SSH directory with secure permissions:

mkdir -p ~/.ssh
chmod 700 ~/.ssh


Added both public keys to the server’s trust list:

nano ~/.ssh/authorized_keys

<contents of key_1.pub>
<contents of key_2.pub>


Locked everything down properly:

chmod 600 ~/.ssh/authorized_keys
chown -R ec2-user:ec2-user ~/.ssh


At this point, the server knew exactly who was allowed in 🔐.

🚀 Connecting Using Both SSH Keys

SSH access was verified using both keys:

ssh -i ~/.ssh/key_1 ec2-user@server-ip
ssh -i ~/.ssh/key_2 ec2-user@server-ip


Both connections were successful, confirming that the server accepts multiple identities without confusion.

🧭 SSH Config Alias (Quality of Life Upgrade)

To make connecting easier, an SSH alias was added.

Edited the local SSH config:

nano ~/.ssh/config

Host aws-server
    HostName server-ip
    User ec2-user
    IdentityFile ~/.ssh/key_1


Now the server can be accessed with a single command:

ssh aws-server


Fast. Clean. Civilized ✨.

🛡️ Stretch Goal: Fail2Ban Protection

To discourage brute-force attempts, Fail2Ban was installed and enabled:

sudo apt update
sudo apt install fail2ban -y

sudo systemctl enable fail2ban
sudo systemctl start fail2ban


Fail2Ban now quietly watches the door and blocks suspicious behavior 👀.

🔒 Security Notes

❌ Private SSH keys were never pushed to the repository

✅ Only public keys were added to the server

🔐 SSH access is strictly key-based

🎯 Final Outcome

✅ Remote Linux server successfully created on AWS

✅ SSH access configured using two SSH key pairs

✅ Verified login using both keys

✅ SSH alias configured via ~/.ssh/config

✅ Basic brute-force protection enabled with Fail2Ban

This project provides a solid foundation in remote server access and secure authentication, setting the stage for deeper server configuration in future projects 🚀.
