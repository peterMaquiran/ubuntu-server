# Fail2Ban SSH Protection

This guide explains how to enable **Fail2Ban** for SSH using the common approach with a local override file.  
Fail2Ban will ban IPs after **3 failed login attempts** within 10 minutes.

---

## 📂 Configuration

Fail2Ban loads settings from `/etc/fail2ban/jail.conf`, but the recommended way is to **override settings in `/etc/fail2ban/jail.local`** (or inside `/etc/fail2ban/jail.d/*.local`).  
This way, your changes survive package updates.

Edit the jail.local file:

```bash
sudo nano /etc/fail2ban/jail.local
```

```bash
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
