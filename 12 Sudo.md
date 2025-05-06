# **`sudo` in Linux**

## **1. What is `sudo`?**
`sudo` (SuperUser DO) allows permitted users to execute commands as the **root user** or another user, following security policies defined in `/etc/sudoers`.

## **2. Key Features**
- Granular access control
- Command logging (`/var/log/auth.log`)
- Temporary privilege elevation
- Environment variable control

## **3. Basic Usage**
```bash
sudo command              # Run as root
sudo -u username command  # Run as specific user
sudo -i                   # Start interactive root shell
sudo -s                   # Start shell with preserved environment
```

## **4. Configuration (`/etc/sudoers`)**
**Always edit with `visudo` (validates syntax):**
```bash
sudo visudo
```

### **Common Directives**
```bash
# Allow user full root access
username ALL=(ALL:ALL) ALL

# Allow group admin access
%admin ALL=(ALL) ALL

# Allow specific commands
username ALL=(root) /usr/bin/apt,/usr/bin/systemctl

# Passwordless sudo
username ALL=(ALL) NOPASSWD: ALL

# Allow with restrictions
username ALL=(ALL) !/usr/bin/su, !/bin/bash
```

## **5. Advanced Features**
### **Command Aliases**
```bash
Cmnd_Alias NETWORKING = /sbin/route, /sbin/ifconfig
Cmnd_Alias SOFTWARE = /usr/bin/apt, /usr/bin/dpkg
```

### **Runas Aliases**
```bash
Runas_Alias ADMINS = root, admin1, admin2
```

### **Defaults Modifications**
```bash
Defaults timestamp_timeout=30  # Re-ask password after 30 mins
Defaults insults               # Fun when password is wrong
Defaults logfile=/var/log/sudo.log
```

## **6. Security Best Practices**
1. **Never give unrestricted sudo access**
2. **Use groups instead of individual users** (`%groupname`)
3. **Specify exact commands** instead of `ALL`
4. **Enable sudo logging** (default in most distros)
5. **Regularly audit sudo access**:
   ```bash
   sudo grep -E '^[^#]' /etc/sudoers /etc/sudoers.d/*
   ```

## **7. Troubleshooting**
- **"User not in sudoers file"**: Add user to sudo group:
  ```bash
  usermod -aG sudo username
  ```
- **"Sorry, try again"**: Check PAM authentication in `/etc/pam.d/sudo`
- **Command not allowed**: Verify exact path is in sudoers (use `which command`)

## **8. Alternatives to sudo**
- `su` - Switch user (requires root password)
- `doas` - Simpler alternative (OpenBSD)
- `pkexec` - PolicyKit-based execution

## **9. Real-World Examples**
### **Web Admin Scenario**
```bash
%webadmins ALL=(root) /usr/bin/systemctl restart apache2, /usr/bin/journalctl -u apache2
```

### **Developers with Docker**
```bash
%devs ALL=(root) /usr/bin/docker, /usr/bin/dockerd
```

### **Passwordless Backup Script**
```bash
backupuser ALL=(root) NOPASSWD: /usr/bin/rsync
```

## **10. Important Files**
- `/etc/sudoers` - Main configuration
- `/etc/sudoers.d/` - Additional policies
- `/var/log/auth.log` - Sudo attempts (Debian/Ubuntu)
- `/var/log/secure` - Sudo attempts (RHEL/CentOS)
