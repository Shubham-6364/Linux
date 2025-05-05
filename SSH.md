# SSH
**SSH (Secure Shell) is like a secure tunnel between your computer and another computer (like a server)**
  - **Remotely Control Another Computer –** Run commands, manage files, or fix problems as if you were sitting in front of it.
  - **Send Data Safely –** Everything (including passwords) is encrypted, so hackers can’t spy on it.
  - **Prove Who You Are –** Uses passwords or special keys (like a digital fingerprint) to verify your identity.

# Real-Life Example
- Imagine SSH as a locked, private phone call between you and a faraway computer:
  - Only you and the computer can hear each other (encryption).
  - You must say a secret password or show a special key to start talking (authentication).
  - Once connected, you can give commands, move files, or fix things safely.

# Common Uses of SSH
  - Managing web servers (e.g., Linux servers).
  - Securely transferring files (SFTP/SCP).
  - Accessing private networks (like a company’s internal systems).

#How to Use SSH?
- Remotly login to the server
  ```
  ssh user@<ipaddress>
  ```
- Remote login with key file 
  ```
  ssh -i <path of key> user@ipaddress
  ```
- Remote login with custom port
  ```
  ssh -p <new_port> user@ipaddress
  ```
- SSH configuration file
vim /etc/ssh/sshd_config
  - By Default
  - #Port 22
  - #PermitRootLogin no
  - #PasswordAuthentication yes
  - if we change the default port address than so steps have been taken are-
- first update the new port in sshd_config file
  ```
  port 2250
  ```
- second add port to firewall
  ```
  firewall-cmd --add-port=2250/tcp
  ```
- third add port to Selinux
  ```
  semanage port -a -t ssh_port_t -p tcp 2250
  ```
- fourth restart the sshd service
  ```
  systemctl restart sshd
  ```
- now connect with host with change port
  ```
  ssh -p <newport> user@ipaddress\
  ```
# SSH Passwordless Login
- Generate a Public/Private Key
  ```
  ssh-keygen
  ```
- copy public key to the Guest Host which you want to login without password
  ```
  ssh-copy-id user@ipaddress
  ```
- Now you can login with Guest host
- for the first time you have to enter the password
  ```
  ssh user@ipaddress
  ```

# **SCP (Secure Copy) and Rsync Commands in Linux**  

Both **SCP** and **rsync** are used to transfer files securely over **SSH**, but they work differently.  

---

## **1. SCP (Secure Copy)**  
SCP copies files between hosts on a network using **SSH encryption**. It’s simple but doesn’t support partial transfers or resume.  

### **Basic Syntax**  
```sh
scp [options] source_file user@destination_host:destination_path
```  

### **Common SCP Commands**  
| Command | Description |
|---------|-------------|
| **Copy a file to a remote server** | `scp file.txt user@remote-server:/path/to/destination` |
| **Copy from remote to local** | `scp user@remote-server:/path/file.txt /local/path` |
| **Copy a directory (recursively)** | `scp -r /local/folder user@remote-server:/remote/path` |
| **Use a specific SSH port** | `scp -P 2222 file.txt user@server:/path` |
| **Limit bandwidth (KB/s)** | `scp -l 1000 file.txt user@server:/path` *(1000 KB/s = ~1MB/s)* |

---

## **2. Rsync (Remote Sync)**  
Rsync is **faster and more efficient** than SCP because it:  
✅ Only transfers **changed parts** of files (delta transfer).  
✅ Supports **resuming** interrupted transfers.  
✅ Can **delete** files at the destination if they no longer exist at the source.  

### **Basic Syntax**  
```sh
rsync [options] source_file user@destination_host:destination_path
```  

### **Common Rsync Commands**  
| Command | Description |
|---------|-------------|
| **Sync a file to remote (with progress)** | `rsync -avP file.txt user@server:/path` |
| **Sync a directory (recursively)** | `rsync -avz /local/folder/ user@server:/remote/path/` *(Note: `/` at end matters!)* |
| **Delete extra files at destination** | `rsync -av --delete /local/folder/ user@server:/remote/path/` |
| **Exclude files** | `rsync -av --exclude='*.tmp' /local/folder/ user@server:/remote/path/` |
| **Use SSH with a custom port** | `rsync -av -e 'ssh -p 2222' /local/file user@server:/path` |
| **Limit bandwidth (KB/s)** | `rsync -av --bwlimit=1000 /local/folder/ user@server:/remote/path/` |

---

### **Key Differences: SCP vs Rsync**  
| Feature | SCP | Rsync |
|---------|-----|-------|
| **Speed** | Slower (copies entire files) | Faster (only syncs changes) |
| **Resume Support** | ❌ No | ✅ Yes |
| **Bandwidth Control** | ✅ Yes (`-l`) | ✅ Yes (`--bwlimit`) |
| **Delete Extra Files** | ❌ No | ✅ Yes (`--delete`) |
| **Best For** | Simple, one-time transfers | Large, recurring syncs |

---

### **When to Use Which?**  
- **Use SCP** → Quick, simple file transfers.  
- **Use Rsync** → Large backups, frequent syncs, or partial updates.  

Both work over **SSH**, so your data stays encrypted! 🔒  



