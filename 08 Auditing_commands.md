# **Linux Command Auditing: Tracking User Activity**

Auditing in Linux helps monitor system activities, track user commands, and investigate security incidents. Here are the key methods for auditing commands in Linux:

---

## **1. Using `history` Command**
The simplest way to view executed commands for the current user:

```bash
history          # Show all commands
history 20       # Show last 20 commands
!45              # Re-run command #45 from history
```

**Permanent History Storage:**  
By default, history is saved in `~/.bash_history`. Configure size in `~/.bashrc`:
```bash
HISTFILESIZE=2000   # Max lines in history file
HISTSIZE=1000       # Max lines in memory
HISTTIMEFORMAT="%F %T "  # Add timestamps
```

---

## **2. System-Wide Command Auditing with `auditd`**
The **Linux Audit Daemon (`auditd`)** provides enterprise-grade auditing.

### **Install & Enable `auditd`**
```bash
sudo apt install auditd    # Debian/Ubuntu
sudo yum install audit    # RHEL/CentOS
sudo systemctl enable --now auditd
```

### **Track All Commands by Users**
```bash
sudo auditctl -a exit,always -F arch=b64 -S execve -k COMMAND_LOGGING
```

**Where:**
- `-a exit,always` → Log on command exit
- `-F arch=b64` → For 64-bit systems
- `-S execve` → Monitor command execution
- `-k COMMAND_LOGGING` → Tag for filtering

### **View Audit Logs**
```bash
sudo ausearch -k COMMAND_LOGGING | aureport -f -i
```

### **Make Rules Permanent**
Add to `/etc/audit/rules.d/audit.rules`:
```bash
-a always,exit -F arch=b64 -S execve -k COMMAND_LOGGING
```

---

## **3. `sudo` Command Logging**
All `sudo` commands are logged in `/var/log/auth.log` (Debian) or `/var/log/secure` (RHEL).

**View sudo commands:**
```bash
sudo grep sudo /var/log/auth.log
```

**Example output:**
```
May 10 14:30:01 server sudo: alice : TTY=pts/0 ; USER=root ; COMMAND=/usr/bin/apt update
```

---

## **4. `script` Command (Session Recording)**
Record an entire terminal session:
```bash
script -a /var/log/user_sessions/alice_$(date +%F).log
```
- Records **all input/output** until `exit`.
- Useful for **admin training** or **forensics**.

---

## **5. Centralized Logging with `rsyslog`**
Forward logs to a central server for auditing.

**Configure `/etc/rsyslog.conf`:**
```bash
*.* @192.168.1.100:514   # Send logs to remote server
```

---

## **6. Real-World Auditing Examples**
### **Find Who Ran a Dangerous Command**
```bash
sudo ausearch -k COMMAND_LOGGING -x rm -rf | aureport -f -i
```

### **Monitor Specific User**
```bash
sudo auditctl -w /home/alice/.bash_history -p wa -k ALICE_CMDS
```

### **Alert on Privileged Commands**
```bash
sudo auditctl -a always,exit -F path=/usr/bin/su -F success=1 -k SU_ATTEMPTS
```

---

## **7. Best Practices**
1. **Enable `auditd`** for critical systems.
2. **Regularly review logs** (`aureport`).
3. **Secure audit logs** (prevent tampering):
   ```bash
   sudo chmod 750 /var/log/audit
   sudo chown root:adm /var/log/audit
   ```
4. **Use centralized logging** for multi-server environments.

---

## **Summary Table**
| Method | Scope | Log Location | Best For |
|--------|-------|-------------|----------|
| `history` | Single user | `~/.bash_history` | Basic command tracking |
| `auditd` | System-wide | `/var/log/audit/audit.log` | Enterprise auditing |
| `sudo` logs | Privileged commands | `/var/log/auth.log` | Tracking root access |
| `script` | Session recording | Custom file | Training/forensics |
| `rsyslog` | Centralized logs | Remote server | Multi-server environments |

---

## **8. Other commands**
### **Show the list of users who currently access the server**
```bash
w
```

### **Show the last login user details**
```bash
last
```

### **Shoe the deatail of user failed login attempts**
```bash
lastb
```

---

**Auditing is crucial for:**  
🔒 **Security compliance** (PCI DSS, HIPAA)  
🔍 **Incident investigation**  
👁️ **User accountability**  
