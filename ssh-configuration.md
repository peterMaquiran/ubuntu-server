# SSH Configuration Documentation

This document explains the SSH configuration for connecting to different servers used in a sample project.

---

## 1. Sample VPS (Personal Access)

**Host Alias:** `SAMPLE-VPS`
**Purpose:** Personal VPS server access

**Configuration:**

```ssh
Host SAMPLE-VPS
  HostName 192.168.100.50
  User alice
  IdentityFile ~/.ssh/id_ed25519_sample
```

**Details:**

* **HostName:** The actual IP address of the VPS (`192.168.100.50`).
* **User:** The SSH username on the remote server (`alice`).
* **IdentityFile:** The private key used for authentication (`~/.ssh/id_ed25519_sample`).

**Usage Example:**

```bash
ssh SAMPLE-VPS
```

This will automatically use the `alice` user and the specified identity file.

---

## 2. Git Repositories on Example Host

**Host Alias:** `example-git-server.com`
**Purpose:** Access Git repositories via SSH on a custom port

**Configuration:**

```ssh
Host example-git-server.com
    User git
    Port 2200
    IdentityFile ~/.ssh/id_ed25519_sample
```

**Details:**

* **HostName:** The full host address (`example-git-server.com`).
* **User:** `git` (common user for Git SSH access).
* **Port:** `2200` (custom SSH port for this server).
* **IdentityFile:** The private key used for authentication (`~/.ssh/id_ed25519_sample`).

**Usage Example:**

```bash
git clone ssh://git@example-git-server.com:2200/sample-project.git
```

---

## Notes

1. **IdentityFile** points to the private key you want SSH to use. Ensure permissions are set correctly:

```bash
chmod 600 ~/.ssh/id_ed25519_sample
```

2. You can simplify Git commands by relying on this SSH configuration instead of specifying the key and port each time.

3. Using **Host aliases** allows shorter commands and easier management of multiple servers.

---

## Summary

| Host Alias             | Remote User | Port | Identity File               | Purpose                          |
| ---------------------- | ----------- | ---- | --------------------------- | -------------------------------- |
| SAMPLE-VPS             | alice       | 22   | \~/.ssh/id\_ed25519\_sample | Personal VPS access              |
| example-git-server.com | git         | 2200 | \~/.ssh/id\_ed25519\_sample | Git repositories on example host |
