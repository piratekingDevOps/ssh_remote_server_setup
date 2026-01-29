🔐 #SSH Remote Server Setup

Project URL:
https://roadmap.sh/projects/ssh-remote-server-setup

📖 Overview

This project demonstrates how to set up a remote Linux server and configure secure SSH access using multiple SSH key pairs.

The main objective is simple:

Allow SSH access to the same server using two different SSH keys

Verify authentication using each key

Optionally simplify access using SSH configuration

Improve security with basic protections

The remote server for this project was created as a Linux EC2 instance on AWS using the AWS Console ☁️.

🖥️ Server Information

Cloud Provider: AWS

Service: EC2

Operating System: Linux

Authentication: SSH (Key-Based)

Network Access: Port 22 (SSH) enabled

🔑 SSH Key Generation

Two SSH key pairs were generated locally to represent multiple trusted identities.

ssh-keygen -t ed25519 -f ~/.ssh/key_1
ssh-keygen -t ed25519 -f ~/.ssh/key_2


This created the following files:

key_1 and key_1.pub

key_2 and key_2.pub

Each key can independently authenticate to the server.

🧩 Adding SSH Keys to the Server

The public keys were added to the remote server as follows:

Connected to the EC2 instance using the default AWS key pair

Switched to the correct user:

sudo su - ec2-user


Created the SSH directory and set permissions:

mkdir -p ~/.ssh
chmod 700 ~/.ssh


Added both public keys to the authorized keys file:

nano ~/.ssh/authorized_keys

<contents of key_1.pub>
<contents of key_2.pub>


Applied strict permissions:

chmod 600 ~/.ssh/authorized_keys
chown -R ec2-user:ec2-user ~/.ssh


At this point, the server recognized both SSH identities 🔐.

🚀 Verifying SSH Access

SSH access was verified using both keys:

ssh -i ~/.ssh/key_1 ec2-user@server-ip
ssh -i ~/.ssh/key_2 ec2-user@server-ip


Both connections authenticated successfully.

🧭 SSH Config Alias (Optional)

To simplify SSH access, an alias was configured.

Edited the local SSH configuration file:

nano ~/.ssh/config


Added the following entry:

Host aws-server
    HostName server-ip
    User ec2-user
    IdentityFile ~/.ssh/key_1


Now the server can be accessed using:

ssh aws-server


This removes the need to specify the key and IP every time ✨.

🛡️ Stretch Goal: Fail2Ban

As an additional security measure, Fail2Ban was installed to help prevent brute-force SSH attacks.

sudo apt update
sudo apt install fail2ban -y


The service was enabled and started:

sudo systemctl enable fail2ban
sudo systemctl start fail2ban


Fail2Ban now monitors login attempts and blocks suspicious behavior.

🔐 Security Notes

Private SSH keys were not pushed to the repository

Only public keys were added to the server

SSH authentication is fully key-based

✅ Outcome

Remote Linux server successfully created on AWS

SSH access configured using two SSH key pairs

Verified access using both keys

SSH alias configured using ~/.ssh/config

Basic brute-force protection enabled with Fail2Ban

📌 Conclusion

This project provides hands-on experience with:

Remote Linux server setup

SSH key-based authentication

Managing multiple SSH identities

Basic server security practices

It lays a strong foundation for future projects involving server configuration and hardening 🚀.
