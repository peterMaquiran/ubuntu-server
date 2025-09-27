# Fail2Ban SSH Protection

This guide explains how to enable **Fail2Ban** for SSH to protect against brute-force attacks.  
Fail2Ban will ban IPs after **3 failed login attempts** within 10 minutes.

---

## 📦 Installation

### Ubuntu/Debian
```bash
sudo apt update && sudo apt install fail2ban -y
```

### CentOS/RHEL/AlmaLinux
```bash
sudo yum install fail2ban -y
```

---

## ⚙️ Enable and Start Service

```bash
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

---

## 📂 Configuration

Fail2Ban loads settings from `/etc/fail2ban/jail.conf`, but the recommended way is to **override settings in `/etc/fail2ban/jail.local`** (or inside `/etc/fail2ban/jail.d/*.local`).  
This way, your changes survive package updates.

Edit the jail.local file:

```bash
sudo nano /etc/fail2ban/jail.local
```

Paste this configuration:

```ini
[sshd]
enabled  = true
port     = ssh
logpath  = /var/log/auth.log     ; Ubuntu/Debian
# logpath = /var/log/secure      ; CentOS/RHEL/AlmaLinux
backend  = systemd

maxretry = 3        ; number of failed attempts before ban
findtime = 600      ; time window in seconds (10 minutes)
bantime  = 3600     ; ban time in seconds (1 hour)
```

---

## 🔄 Restart Fail2Ban

```bash
sudo systemctl restart fail2ban
```

---

## 📊 Check Status

Check if the SSH jail is active:

```bash
sudo fail2ban-client status sshd
```

Example output:

```
Status for the jail: sshd
|- Filter
|  |- Currently failed: 0
|  `- Total failed: 5
`- Actions
   |- Currently banned: 1
   `- Total banned: 3
```

---

## 🚫 Unban an IP (Optional)

```bash
sudo fail2ban-client set sshd unbanip <IP_ADDRESS>
```

---

✅ You now have **Fail2Ban SSH protection** enabled and working!
