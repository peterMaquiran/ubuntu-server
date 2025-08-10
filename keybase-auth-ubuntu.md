# SSH Key Authentication Setup on Ubuntu Server

---
## Dependency
- Windows
    -  choco install ssh-copy-id
- Linux 
## 1. Generate SSH Key Pair (Local Machine)

Run this on your **local computer** (Linux, macOS, or Windows with WSL/Git Bash):

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

## 2. Copy Public Key to Server (Local Machine)

Run this on your **local computer** (Linux, macOS, or Windows with WSL/Git Bash):

```bash
ssh-copy-id username@your-server-ip
```

## 3. Connect to your server using SSH:

Run this on your **local computer** (Linux, macOS, or Windows with WSL/Git Bash):

```bash
ssh username@your-server-ip
```
## 4. Add shorcat

add ~/.ssh/config
```bash
Host SIH
  HostName your-server-ip
  User root
  IdentityFile ~/.ssh/id_personal
```

connect
```bash
ssh SIH
```


## 5. Disable Password Authentication on Server:

Edit SSH configuration:

```bash
sudo nano /etc/ssh/sshd_config
```

Set the following options:
```bash
PasswordAuthentication no
ChallengeResponseAuthentication no
PubkeyAuthentication yes
AuthenticationMethods publickey
```

Restart SSH service
```bash
sudo systemctl restart ssh
```
