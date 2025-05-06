# **Linux Networking**

Linux provides powerful networking capabilities for both basic connectivity and advanced configurations. Here's a complete breakdown of essential networking concepts and tools.

---

## **1. Network Interfaces**
### **Viewing Interfaces**
```bash
ip link show          # List all network interfaces
ifconfig -a           # Legacy command (deprecated on many systems)
lshw -class network   # Detailed hardware info
```

### **Interface Naming Conventions**
- **eth0**: Traditional Ethernet
- **ens33**: Predictable naming (PCI bus order)
- **wlan0**: Wireless interface
- **lo**: Loopback (127.0.0.1)

---

## **2. IP Address Configuration**
### **Temporary Assignment**
```bash
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip link set eth0 up
```

### **Permanent Configuration**
Edit config files (distribution-specific):
- **Debian/Ubuntu**: `/etc/network/interfaces`
  ```bash
  auto eth0
  iface eth0 inet static
      address 192.168.1.100
      netmask 255.255.255.0
      gateway 192.168.1.1
  ```
- **RHEL/CentOS**: `/etc/sysconfig/network-scripts/ifcfg-eth0`
- **Netplan (Ubuntu 18.04+)**: `/etc/netplan/*.yaml`

---

## **3. DNS Configuration**
```bash
# Temporary DNS
sudo echo "nameserver 8.8.8.8" > /etc/resolv.conf

# Permanent DNS (distribution-specific)
# Debian/Ubuntu: /etc/resolvconf/resolv.conf.d/base
# RHEL/CentOS: /etc/sysconfig/network-scripts/ifcfg-eth0
```

---

## **4. Essential Network Commands**
| Command | Purpose |
|---------|---------|
| `ping google.com` | Test connectivity |
| `traceroute google.com` | Trace network path |
| `mtr google.com` | Advanced traceroute |
| `dig google.com` | DNS lookup |
| `nslookup google.com` | Legacy DNS lookup |
| `ss -tulnp` | Modern socket statistics |
| `netstat -tulnp` | Legacy network stats |
| `ip route show` | View routing table |
| `route -n` | Legacy routing table |
| `hostname -I` | Show all IP addresses |
| `iwconfig` | Wireless interface info |

---

## **5. Firewall Management**
### **iptables (Legacy)**
```bash
sudo iptables -L -v -n  # List rules
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT  # Allow SSH
```

### **nftables (Modern Replacement)**
```bash
sudo nft list ruleset
```

### **UFW (Ubuntu Firewall)**
```bash
sudo ufw allow 22/tcp
sudo ufw enable
```

### **firewalld (RHEL/CentOS)**
```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --reload
```

---

## **6. Network Troubleshooting**
### **Connectivity Issues**
```bash
ping -c 4 8.8.8.8          # Test basic connectivity
traceroute 8.8.8.8         # Find where connection fails
mtr 8.8.8.8                # Continuous traceroute
```

### **DNS Issues**
```bash
dig google.com             # Check DNS resolution
dig @8.8.8.8 google.com   # Test with specific DNS server
```

### **Port Testing**
```bash
telnet example.com 80      # Test TCP port
nc -zv example.com 443     # Test TCP port with netcat
nmap -p 80 example.com     # Advanced port scanning
```

---

## **7. Advanced Networking**
### **Bonding (Network Teaming)**
```bash
# Create bond interface
sudo ip link add bond0 type bond mode balance-rr
sudo ip link set eth0 master bond0
sudo ip link set eth1 master bond0
```

### **VLAN Configuration**
```bash
sudo ip link add link eth0 name eth0.100 type vlan id 100
sudo ip addr add 192.168.100.1/24 dev eth0.100
```

### **VPN Connections**
```bash
# OpenVPN
sudo openvpn --config client.ovpn

# WireGuard
sudo wg-quick up wg0
```

---

## **8. Network Monitoring**
```bash
iftop -i eth0              # Bandwidth usage per connection
nethogs                    # Bandwidth per process
bmon                       # Interface bandwidth monitor
vnstat -l -i eth0          # Traffic statistics
tcpdump -i eth0 -n -w capture.pcap  # Packet capture
```

---

## **9. Network Services**
### **SSH**
```bash
ssh user@host              # Basic connection
ssh -p 2222 user@host     # Custom port
scp file.txt user@host:/path  # Secure copy
```

### **Web Server**
```bash
# Nginx
sudo systemctl start nginx

# Apache
sudo systemctl start apache2
```

### **Database Access**
```bash
mysql -h db.example.com -u user -p
psql -h db.example.com -U user -d dbname
```

---

## **10. Cloud Networking**
### **AWS CLI**
```bash
aws ec2 describe-instances
aws s3 ls
```

### **Azure CLI**
```bash
az vm list
az network nsg list
```

---

## **Best Practices**
1. **Always use the most specific tools** (`ip` instead of `ifconfig`)
2. **Document network changes**
3. **Test configurations before applying permanently**
4. **Use SSH keys instead of passwords**
5. **Regularly update network services**
