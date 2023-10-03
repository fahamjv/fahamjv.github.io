+++
author = "fahamjv"
title = "Enabling Root SSH Access on AWS Instances"
date = "2023-09-26"
description = "Learn how to resolve the common SSH error message 'Please login as the user 'ubuntu' rather than the user 'root'' on AWS instances and grant root access."
aliases = [
    "SSH error 'Please login as the user 'ubuntu' rather than the user 'root''",
    "Resolve 'Please login as the user 'ubuntu' rather than the user 'root'' SSH error",
    "Fix SSH 'root' user error on AWS",
    "AWS root access SSH error",
    "Troubleshoot 'root' user SSH issue",
    "Granting root access on AWS SSH",
    "AWS server 'root' SSH login",
    "Login as 'root' instead of 'ubuntu' on AWS",
]
+++

If you've encountered the SSH error message 'Please login as the user 'ubuntu' rather than the user 'root'?' and you need root user access, follow these steps:

1. **Log in as "ubuntu"**
   ```bash
   ssh ubuntu@your-server-ip
2. **Edit the SSH Configuration**
   ```bash
   sudo vim /etc/ssh/sshd_config
3. **Modify SSH Configuration**
   ```bash
   PermitRootLogin prohibit-password
   ```

    Replace it with:
   ```bash
   PermitRootLogin yes
4. **Edit the authorized_keys file**
   ```bash
   sudo vim /root/.ssh/authorized_keys
5. **Remove or comment out the restrictive line**
   ```bash
    no-port-forwarding,no-agent-forwarding,no-X11-forwarding,command="echo 'Please login as the user \"ubuntu\" rather than the user \"root\".';echo;sleep 10"
6. **Log in as root**
   ```bash
    ssh root@your-server-ip
