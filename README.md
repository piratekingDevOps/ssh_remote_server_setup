# 🔐 SSH Remote Server Setup

**Project URL:**  
https://roadmap.sh/projects/ssh-remote-server-setup

---

## 📖 Overview

This project demonstrates how to set up a **remote Linux server** and configure secure **SSH access using multiple SSH key pairs**.

The main objectives:

- Allow SSH access to the same server using **two different SSH keys**  
- Verify authentication using each key  
- Simplify access using SSH configuration aliases  
- Optionally improve security with basic protections

The server for this project was created as a **Linux EC2 instance on AWS** ☁️.

---

## 🖥️ Server Information

- **Cloud Provider:** AWS  
- **Service:** EC2  
- **Operating System:** Linux  
- **Authentication:** SSH (Key-Based)  
- **Network Access:** Port 22 (SSH) enabled  

---

## 🔑 SSH Key Generation

Two SSH key pairs were generated locally:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/key_1
ssh-keygen -t ed25519 -f ~/.ssh/key_2
```

Files created locally:

```text
~/.ssh/key_1
~/.ssh/key_1.pub
~/.ssh/key_2
~/.ssh/key_2.pub
```

Each key can independently authenticate to the server.

---

## 🧩 Adding SSH Keys to the Server

Steps to configure the server:

1. Connect to the EC2 instance using the **default AWS key pair**  
2. Switch to the correct user:

```bash
sudo su - ec2-user
```

3. Prepare the SSH directory:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

4. Add both public keys to `authorized_keys`:

```bash
nano ~/.ssh/authorized_keys
```

Paste the contents of both public keys:

```text
# Contents of key_1.pub
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... user@local

# Contents of key_2.pub
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... user@local
```

5. Set proper permissions:

```bash
chmod 600 ~/.ssh/authorized_keys
chown -R ec2-user:ec2-user ~/.ssh
```

At this point, the server recognizes both SSH identities 🔐.

---

## 🚀 Verifying SSH Access

SSH access verified with both keys:

```bash
ssh -i ~/.ssh/key_1 ec2-user@server-ip
ssh -i ~/.ssh/key_2 ec2-user@server-ip
```

Both connections authenticated successfully.

---

## 🧭 SSH Config Alias (Optional)

To simplify SSH commands:

1. Edit local SSH config:

```bash
nano ~/.ssh/config
```

2. Add the following entry:

```text
Host aws-server
    HostName server-ip
    User ec2-user
    IdentityFile ~/.ssh/key_1
```

3. Connect easily:

```bash
ssh aws-server
```

No more typing IPs or key paths every time ✨.

---

## 🛡️ Stretch Goal: Fail2Ban

Install Fail2Ban for brute-force protection:

```bash
sudo apt update
sudo apt install fail2ban -y
```

Enable and start the service:

```bash
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

Fail2Ban monitors login attempts and blocks suspicious activity 👀.

---

## 🔐 Security Notes

- Private SSH keys were **never committed**  
- Only public keys were added to the server  
- SSH access is strictly key-based  

---

## ✅ Outcome

- Remote Linux server successfully created on AWS  
- SSH access configured using **two independent key pairs**  
- Verified login using both keys  
- SSH alias configured via `~/.ssh/config`  
- Basic brute-force protection enabled with Fail2Ban  

---

## 📌 Conclusion

This project provides hands-on experience with:

- Remote Linux server setup  
- SSH key-based authentication  
- Managing multiple SSH identities  
- Basic server security practices  

It establishes a solid foundation for future server configuration and hardening projects 🚀.
