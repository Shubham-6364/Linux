# **Firewall and SELinux in Linux: A Comprehensive Guide**

## **1. Linux Firewall Management**

### **I. Netfilter/iptables (Traditional)**
The legacy firewall framework using these components:
- **iptables** (IPv4)
- **ip6tables** (IPv6)
- **ebtables** (Ethernet bridge)

**Common Commands:**
```bash
sudo iptables -L -v -n              # List rules
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT  # Allow SSH
sudo iptables -A INPUT -j DROP      # Default deny
sudo iptables-save > /etc/iptables.rules  # Save rules
```

### **II. nftables (Modern Replacement)**
Replaces iptables with unified syntax and better performance.

**Basic Usage:**
```bash
sudo nft list ruleset               # Show current rules
sudo nft add table inet filter      # Create table
sudo nft add chain inet filter input { type filter hook input priority 0 \; }  # Create chain
sudo nft add rule inet filter input tcp dport 22 accept  # Allow SSH
```

### **III. Firewall Management Tools**
| Tool | Distros | Description |
|------|---------|-------------|
| **UFW** (Uncomplicated Firewall) | Ubuntu/Debian | Simple frontend |
| **firewalld** | RHEL/CentOS/Fedora | Dynamic daemon with zones |
| **iptables-persistent** | Debian/Ubuntu | Save iptables rules |

**UFW Examples:**
```bash
sudo ufw allow 22/tcp           # Allow SSH
sudo ufw allow from 192.168.1.0/24  # Allow subnet
sudo ufw enable                 # Activate firewall
```

**firewalld Examples:**
```bash
sudo firewall-cmd --add-port=80/tcp --permanent  # Allow HTTP
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --reload        # Apply changes
```

---

## **2. SELinux (Security-Enhanced Linux)**

### **I. Core Concepts**
- **Mandatory Access Control (MAC)** system
- Developed by NSA
- Three modes:
  - **Enforcing**: Blocks unauthorized actions
  - **Permissive**: Logs but doesn't block
  - **Disabled**: Completely off

### **II. Key Commands**
```bash
getenforce                         # Check current mode
sudo setenforce 0                  # Set Permissive
sudo setenforce 1                  # Set Enforcing
sestatus                           # Detailed status
```

### **III. Context Management**
SELinux uses security contexts (`user:role:type:level`)

**View Contexts:**
```bash
ls -Z /var/www/html                # File contexts
ps auxZ | grep nginx               # Process contexts
```

**Modify Contexts:**
```bash
sudo chcon -t httpd_sys_content_t /webcontent  # Change type
sudo restorecon -Rv /var/www/html  # Reset to default
```

### **IV. Policy Management**
```bash
sudo semanage port -l              # List port contexts
sudo semanage port -a -t http_port_t -p tcp 8080  # Add port
sudo audit2allow -a                # Generate policy from audit logs
```

### **V. Troubleshooting**
**Check Audit Logs:**
```bash
sudo ausearch -m AVC -ts recent    # View recent denials
sudo sealert -a /var/log/audit/audit.log  # Analyze with setroubleshoot
```

**Common Fixes:**
```bash
sudo setsebool -P httpd_can_network_connect 1  # Allow HTTPD network access
sudo semanage fcontext -a -t public_content_rw_t "/shared(/.*)?"  # Add file context
```

---

## **3. Firewall + SELinux Interaction**

### **I. Port Labeling**
SELinux controls whether services can bind to ports:
```bash
sudo semanage port -l | grep http   # View allowed HTTP ports
```

### **II. Network Controls**
SELinux can restrict network access independently of firewall:
```bash
getsebool -a | grep httpd          # View HTTP-related booleans
setsebool -P httpd_can_network_connect_db on  # Allow DB connections
```

---

## **4. Best Practices**

### **Firewall**
1. Default deny policy
2. Allow only necessary ports
3. Use zone-based configurations (firewalld)
4. Regularly audit rules
5. Log dropped packets:
   ```bash
   sudo iptables -A INPUT -j LOG --log-prefix "DROPPED: "
   ```

### **SELinux**
1. Keep in **Enforcing** mode
2. Use `audit2allow` instead of disabling protections
3. Document custom policies
4. Regularly check `sealert` reports
5. Test in Permissive mode before Enforcing

---

## **5. Troubleshooting Matrix**

| Issue | Firewall Check | SELinux Check |
|-------|----------------|---------------|
| Service not binding to port | `iptables -L` | `semanage port -l` |
| Connection refused | `telnet localhost PORT` | `ausearch -m AVC` |
| Permission denied | Check NAT/forwarding | `ls -Z` on files |
| Service starts but can't connect | Check OUTPUT rules | Network-related booleans |

---

## **6. Real-World Examples**

### **Web Server Deployment**
```bash
# Firewall
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# SELinux
sudo semanage port -a -t http_port_t -p tcp 8080
sudo setsebool -P httpd_can_network_connect=1
```

### **Database Server**
```bash
# firewalld
sudo firewall-cmd --add-service=mysql --permanent
sudo firewall-cmd --reload

# SELinux
sudo setsebool -P mysqld_connect_any=1
```

---

## **7. Key Files**

### **Firewall**
- `/etc/iptables/rules.v4` (iptables persistent rules)
- `/etc/ufw/` (UFW configuration)
- `/etc/firewalld/` (firewalld configurations)

### **SELinux**
- `/etc/selinux/config` (Main config file)
- `/var/log/audit/audit.log` (Denial messages)
- `/etc/selinux/targeted/modules/` (Custom policy modules)

---

## **Summary Cheat Sheet**

| Task | Firewall Command | SELinux Command |
|------|------------------|-----------------|
| Check status | `sudo ufw status` | `getenforce` |
| Allow service | `sudo firewall-cmd --add-service=http` | `setsebool httpd_can_network_connect=1` |
| Open port | `sudo ufw allow 22/tcp` | `semanage port -a -t ssh_port_t -p tcp 2222` |
| Troubleshoot | `journalctl -xe` | `sealert -a /var/log/audit/audit.log` |
| Persistent config | `iptables-save > /etc/iptables.rules` | `semanage export` |

**Remember:**  
- Firewall controls **network access**  
- SELinux controls **process/file interactions**  
- They work **together** for defense-in-depth security  
