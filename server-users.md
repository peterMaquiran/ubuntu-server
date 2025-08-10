
# SSH Key-Based Authentication Best Practices

This document outlines best practices for setting up secure SSH key-based authentication on a Linux server with a non-root admin user.

---

## 1. Create a Non-Root Admin User with Sudo Privileges

```bash
adduser peter
usermod -aG sudo peter
```

---

## 2. Copy SSH Authorized Keys

Copy the root user's authorized keys to the new user:

```bash
mkdir -p /home/peter/.ssh
cp /root/.ssh/authorized_keys /home/peter/.ssh/
chown -R peter:peter /home/peter/.ssh
chmod 700 /home/peter/.ssh
chmod 600 /home/peter/.ssh/authorized_keys
```
when generating ssh key run
```bash
# chmod changes file permissions.
chmod 600 ~/.ssh/id_ed25519 # so the owner can read and write
```

## 3. Disable Root Login Over SSH

Edit the SSH daemon configuration file `/etc/ssh/sshd_config` and set:

```
PermitRootLogin no
```

Restart the SSH service:

```bash
sudo systemctl restart ssh
```

---

## 4. Disable Password Authentication (Allow Only Key-Based Auth)

In `/etc/ssh/sshd_config`, set:

```
PasswordAuthentication no
```

---

## 5. Restrict SSH Access to Specific Users

In `/etc/ssh/sshd_config`, add:

```
AllowUsers peter
```

---

## 6. Use Root Privileges via `sudo`

When elevated privileges are needed, switch to root using:

```bash
sudo -i
```

---

## Summary

- Use a non-root user (`peter`) with sudo privileges.
- Use SSH keys for authentication only.
- Disable root SSH login.
- Disable password authentication.
- Restrict SSH access to allowed users.

---

## Notes

- Always test SSH login in a separate session before closing your current session to avoid lockout.
- Keep backups of your SSH keys and configuration.

---

*End of Document*
