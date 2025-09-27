# Fail2ban: Enable and Disable Guide

This guide explains how to enable and disable Fail2ban globally, 
or just for specific actions such as SSH (sshd).

---

## 1. Global Control of Fail2ban

- **Stop Fail2ban temporarily:**
  ```bash
  sudo systemctl stop fail2ban
  ```

- **Disable Fail2ban at boot:**
  ```bash
  sudo systemctl disable fail2ban
  ```

- **Mask Fail2ban completely (cannot be started):**
  ```bash
  sudo systemctl mask fail2ban
  ```

- **Start Fail2ban again:**
  ```bash
  sudo systemctl start fail2ban
  ```

- **Enable Fail2ban at boot:**
  ```bash
  sudo systemctl enable fail2ban
  ```

- **Unmask Fail2ban:**
  ```bash
  sudo systemctl unmask fail2ban
  ```

---

## 2. Enable/Disable Fail2ban for SSH (sshd)

- **Stop the sshd jail only:**
  ```bash
  sudo fail2ban-client stop sshd
  ```

- **Disable sshd jail permanently:**
  Edit `/etc/fail2ban/jail.local` and set:
  ```ini
  [sshd]
  enabled = false
  ```
  Then restart Fail2ban:
  ```bash
  sudo systemctl restart fail2ban
  ```

- **Re-enable sshd jail:**
  Edit `/etc/fail2ban/jail.local` and set:
  ```ini
  [sshd]
  enabled = true
  ```
  Then restart Fail2ban:
  ```bash
  sudo systemctl restart fail2ban
  ```

- **Check current Fail2ban status:**
  ```bash
  sudo fail2ban-client status
  ```
