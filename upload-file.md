# Uploading a Folder to a Remote Server via SSH

This document explains how to upload a local folder or single file to a remote Ubuntu server using `scp` and an SSH config alias.

---

## Prerequisites

1. You have SSH access to the remote server.
2. Your SSH config (`~/.ssh/config`) has an entry like:

```ssh-config
Host PLATFORM
    HostName <REMOTE_SERVER_IP>
    User <USERNAME>
    IdentityFile ~/.ssh/id_ed25519
```

> Replace `<REMOTE_SERVER_IP>` and `<USERNAME>` with your server's IP and user.

---

## Upload a Folder

Use the `scp` command with the `-r` option to copy directories recursively:

```bash
scp -r ./teste1 PLATFORM:/root/srv
```

* `./teste1` → Local folder to upload
* `PLATFORM` → SSH alias from your config
* `/root/srv` → Destination path on the remote server

---

## Upload a Single File

To upload a single file, use `scp` without the `-r` option:

```bash
scp ./file.txt PLATFORM:/root/srv
```

* `./file.txt` → Local file to upload
* `PLATFORM` → SSH alias from your config
* `/root/srv` → Destination path on the remote server

You can also use `rsync`:

```bash
rsync -avz ./file.txt PLATFORM:/root/srv
```

---

### Notes

* Ensure the destination folder exists on the remote server or you have permissions to create it.
* `rsync` is faster for large files and allows resumable transfers.

---

## Downloading a Folder or File (Optional)

To copy a folder from the remote server back to your local machine:

```bash
scp -r PLATFORM:/root/srv/teste1 ./local_destination
```

To copy a single file back:

```bash
scp PLATFORM:/root/srv/file.txt ./local_destination
```

Or using `rsync`:

```bash
rsync -avz PLATFORM:/root/srv/teste1 ./local_destination
rsync -avz PLATFORM:/root/srv/file.txt ./local_destination
```

---

**Author:** Your Name
**Date:** 2025-08-18
