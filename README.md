<div align="center">
🔐 SSH Remote Server Setup

Secure access to a remote Linux server using multiple SSH keys

📍 Project URL
👉 https://roadmap.sh/projects/ssh-remote-server-setup

</div>
✨ Overview

This project focuses on setting up a remote Linux server and configuring it to allow secure SSH access using multiple SSH key pairs.

The goal is simple and precise:

✅ Be able to SSH into the same server using two different SSH keys

The server for this project was created as a Linux EC2 instance on AWS using the AWS Console ☁️.

🖥️ Server Information
Item	Details
☁️ Cloud Provider	AWS
🧱 Service	EC2
🐧 Operating System	Linux
🔑 Authentication	SSH (Key-Based)
🌐 Network Access	Port 22 (SSH) enabled
🗝️ SSH Key Generation

Two SSH key pairs were generated locally to represent multiple trusted identities.

ssh-keygen -t ed25519 -f ~/.ssh/key_1
ssh-keygen -t ed25519 -f ~/.ssh/key_2


This produced:

🔑 key_1 / key_1.pub

🔑 key_2 / key_2.pub

Each key can independently authenticate to the server.

🧩 Adding SSH Keys to the Server

Connected to the EC2 instance using the default AWS key pair

Switched to the correct user:

sudo su - ec2-user


Prepared the SSH directory:

mkdir -p ~/.ssh
chmod 700 ~/.ssh


Added both public keys:

nano ~/.ssh/authorized_keys

<contents of key_1.pub>
<contents of key_2.pub>


Applied strict permissions:

chmod 600 ~/.ssh/authorized_keys
chown -R ec2-user:ec2-user ~/.ssh


🔒 At this point, the server recognized both SSH identities.

🚀 Verifying SSH Access

Successfully connected using both SSH keys:

ssh -i ~/.ssh/key_1 ec2-user@server-ip
ssh -i ~/.ssh/key_2 ec2-user@server-ip


Both commands authenticated correctly, meeting the core project requirement.

🧭 SSH Config Alias (Optional Enhancement)

To simplify access, an SSH alias was configured.

Edited the local SSH config:

nano ~/.ssh/config

Host aws-server
    HostName server-ip
    User ec2-user
    IdentityFile ~/.ssh/key_1


Now the server can be accessed with:

ssh aws-server


✨ Cleaner commands, fewer flags.

🛡️ Stretch Goal: Fail2Ban

To protect against brute-force login attempts, Fail2Ban was installed:

sudo apt update
sudo apt install fail2ban -y


Enabled and started the service:

sudo systemctl enable fail2ban
sudo systemctl start fail2ban


Fail2Ban now actively monitors and blocks suspicious SSH activity.

🔐 Security Notes

❌ Private SSH keys were never committed to the repository

✅ Only public keys were added to the server

🔑 SSH access is fully key-based

🎯 Final Outcome

✔ Remote Linux server successfully created on AWS
✔ SSH configured with two independent key pairs
✔ Verified access using both keys
✔ SSH alias configured via ~/.ssh/config
✔ Basic brute-force protection enabled

📌 Conclusion

This project establishes a strong foundation in:

Remote Linux server setup

SSH key-based authentication

Managing multiple SSH identities

Basic server security practices

Future projects will build on this foundation with deeper server configuration and hardening 🚀.
