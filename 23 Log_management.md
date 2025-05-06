# **Log Management in Linux**

Logs are critical for system monitoring, troubleshooting, and security auditing. Linux provides robust logging facilities through various subsystems and tools.

---

## **1. Linux Logging Architecture**

### **Key Components**
- **systemd-journald**: Binary journal (replaces traditional syslog in many distros)
- **rsyslog/syslog-ng**: Traditional text-based logging daemons
- **klogd**: Kernel message logger
- **Application Logs**: Stored in `/var/log/` or journald

---

## **2. Main Log Locations**

| Log File | Purpose |
|----------|---------|
| `/var/log/syslog` | General system messages (Debian) |
| `/var/log/messages` | General system messages (RHEL) |
| `/var/log/auth.log` | Authentication logs (Debian) |
| `/var/log/secure` | Authentication logs (RHEL) |
| `/var/log/kern.log` | Kernel messages |
| `/var/log/boot.log` | Boot process messages |
| `/var/log/dmesg` | Kernel ring buffer messages |
| `/var/log/apt/` | Package manager logs |
| `/var/log/nginx/` | Web server logs |
| `/var/log/mysql/` | Database logs |

---

## **3. Essential Log Viewing Commands**

### **Traditional Logs**
```bash
tail -f /var/log/syslog       # Follow logs in real-time
grep "error" /var/log/syslog  # Search for errors
less /var/log/auth.log        # Page through logs
```

### **Journald (systemd)**
```bash
journalctl -xe                # View system logs
journalctl -u nginx -f        # Follow service logs
journalctl --since "1 hour ago"
journalctl -p err..alert      # Filter by priority
journalctl --disk-usage       # Check log size
```

### **Kernel Messages**
```bash
dmesg                         # View kernel buffer
dmesg -T                      # Human-readable timestamps
dmesg -l err,crit             # Filter by severity
```

---

## **4. Log Rotation**

Managed by **logrotate** (configs in `/etc/logrotate.conf` and `/etc/logrotate.d/`)

### **Example Configuration**
```bash
/var/log/nginx/*.log {
    daily
    missingok
    rotate 14
    compress
    delaycompress
    notifempty
    create 0640 www-data adm
    sharedscripts
    postrotate
        systemctl reload nginx
    endscript
}
```

### **Manual Rotation**
```bash
logrotate -vf /etc/logrotate.conf
```

---

## **5. Centralized Logging**

### **rsyslog Remote Logging**
```bash
# On client (/etc/rsyslog.conf)
*.* @192.168.1.100:514

# On server
module(load="imudp")
input(type="imudp" port="514")
```

### **ELK Stack (Elasticsearch, Logstash, Kibana)**
```bash
# Sample Logstash config
input {
  file {
    path => "/var/log/*.log"
  }
}
output {
  elasticsearch {
    hosts => ["localhost:9200"]
  }
}
```

### **Other Solutions**
- **Graylog**
- **Fluentd**
- **Loki (Grafana)**

---

## **6. Log Monitoring & Analysis**

### **Basic Tools**
```bash
grep -i "fail" /var/log/*     # Search across logs
awk '/error/{print $1}' file  # Extract fields
cut -d' ' -f1-5 /var/log/syslog # Show first 5 columns
```

### **Advanced Tools**
```bash
logwatch                     # Daily log summaries
swatch                       # Real-time log monitoring
goaccess /var/log/nginx/access.log # Web log analyzer
```

---

## **7. Log Security Best Practices**

1. **Set proper permissions**:
   ```bash
   chmod 640 /var/log/auth.log
   chown root:adm /var/log/syslog
   ```

2. **Enable log signing** (with auditd):
   ```bash
   sudo auditctl -w /var/log/ -p wa -k logs
   ```

3. **Regularly audit logs**:
   ```bash
   ausearch -k logs | aureport -f -i
   ```

4. **Implement log forwarding** to prevent tampering

5. **Monitor log file integrity**:
   ```bash
   sudo apt install aide
   sudo aideinit
   sudo aide --check
   ```

---

## **8. Troubleshooting Common Issues**

### **Missing Logs**
```bash
systemctl status rsyslog    # Check if logger is running
journalctl --verify         # Check journal corruption
df -h /var/log             # Check disk space
```

### **High Log Volume**
```bash
journalctl --vacuum-size=100M  # Limit journal size
find /var/log -name "*.log" -size +1G  # Find large logs
```

### **Time Synchronization**
```bash
timedatectl status          # Check time sync
sudo apt install chrony     # Install NTP client
```

---

## **9. Cloud Logging Solutions**

### **AWS CloudWatch**
```bash
sudo apt install amazon-cloudwatch-agent
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s
```

### **Azure Monitor**
```bash
sudo apt install azsec-monitor
sudo systemctl enable azsec-monitor
```

### **Google Cloud Logging**
```bash
curl -sSO https://dl.google.com/cloudagents/add-logging-agent-repo.sh
sudo bash add-logging-agent-repo.sh
sudo apt-get install google-fluentd
```

---

## **10. Automation & Alerting**

### **Log-based Alerts**
```bash
# Simple cron job example
*/5 * * * * root grep -q "CRITICAL" /var/log/app.log && mail -s "ALERT" admin@example.com
```

### **More Advanced**
- **Prometheus Alertmanager**
- **Nagios**
- **Zabbix**

---

## **Summary Cheat Sheet**

| Task | Command |
|------|---------|
| View system logs | `journalctl -xe` |
| Follow logs | `tail -f /var/log/syslog` |
| Search logs | `grep "error" /var/log/*` |
| Rotate logs | `logrotate -vf /etc/logrotate.conf` |
| Kernel logs | `dmesg -T` |
| Configure remote logging | Edit `/etc/rsyslog.conf` |
| Check log size | `journalctl --disk-usage` |
| Secure logs | `chmod 640 /var/log/auth.log` |

Effective log management is crucial for **security**, **compliance**, and **system health**.
