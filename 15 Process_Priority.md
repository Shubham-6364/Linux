# **Process Priority in Linux**

## **1. Understanding Process Priority**
Linux uses priority values to determine CPU allocation. Two key metrics:

- **Nice Value (-20 to 19)**
  - Lower value = Higher priority (-20 is highest)
  - Default: 0
  - Regular users can only increase niceness (lower priority)

- **Real-time Priority (0-99)**
  - Higher value = Higher priority
  - Requires root privileges
  - Used for time-critical processes

## **2. Viewing Process Priorities**
```bash
top        # PR column shows priority, NI shows nice value
ps -eo pid,ni,comm,pri,rtprio | head
```

## **3. Managing Nice Values**
### **Launching Processes with Specific Nice Value**
```bash
nice -n 10 command      # Start with nice=10 (lower priority)
nice -n -5 command      # Requires root (higher priority)
```

### **Changing Priority of Running Processes**
```bash
renice 5 -p PID         # Change nice value
renice -n 10 -u username # Change for all user's processes
```

## **4. Real-time Priority (SCHED_FIFO/SCHED_RR)**
### **Set Real-time Priority**
```bash
chrt -f 99 command      # SCHED_FIFO (fixed priority)
chrt -r 50 command      # SCHED_RR (round-robin)
chrt -p PID             # Check current policy
```

### **Available Scheduling Policies**
| Policy | Value | Description |
|--------|-------|-------------|
| SCHED_OTHER | 0 | Default time-sharing |
| SCHED_FIFO | 1 | First-in-first-out (no timeslice) |
| SCHED_RR | 2 | Round-robin (with timeslice) |
| SCHED_BATCH | 3 | For batch processes |
| SCHED_IDLE | 5 | Very low priority |

## **5. Practical Examples**
### **High Priority Video Encoding**
```bash
nice -n -15 ffmpeg -i input.mp4 output.avi
```

### **Low Priority Backup**
```bash
nice -n 19 tar -czf backup.tar.gz /data
```

### **Critical Real-time Process**
```bash
sudo chrt -f 99 /usr/sbin/irqbalance
```

## **6. System Configuration**
### **Limit User Priorities**
Edit `/etc/security/limits.conf`:
```bash
username hard priority -10  # Max priority (min nice)
@developers soft nice 10   # Default nice for group
```

### **Adjust OOM Killer**
```bash
echo -15 > /proc/PID/oom_adj  # Less likely to be killed
echo 17 > /proc/PID/oom_adj   # More likely to be killed
```

## **7. Best Practices**
1. **Use nice for CPU-intensive batch jobs**
2. **Reserve real-time priority for critical systems**
3. **Monitor priority inversion** (when high-prio waits for low-prio)
4. **Balance priorities** to prevent starvation
5. **Consider cgroups** for advanced control

## **8. Troubleshooting**
### **Find High-Priority Processes**
```bash
ps -eo pid,ni,pri,cmd --sort=-pri | head
```

### **Fix Unresponsive System**
```bash
renice 19 -p PID_OF_PROBLEM_PROCESS
killall -STOP cpu_hog; nice -n 19 cpu_hog
```

## **9. Priority Inheritance**
For multithreaded apps, use:
```bash
pthread_mutexattr_setprotocol(&attr, PTHREAD_PRIO_INHERIT);
```

## **10. Graphical Tools**
- `htop` (F7/F8 to adjust nice)
- `gnome-system-monitor`
- `bpytop`
