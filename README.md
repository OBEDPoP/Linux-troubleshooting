# **Linux Troubleshooting Guide**

## **Introduction**
This guide provides in-depth troubleshooting techniques for Linux-based environments. It covers common production issues, their replication, debugging steps, and resolutions, along with essential Linux commands used by system administrators.

---

## **1. Common Issues and Troubleshooting Techniques**

### **1.1 High CPU Usage**
#### **Symptoms:**
- Sluggish system performance
- Delayed response times
- High CPU usage in monitoring tools

#### **Diagnosis:**
```bash
htop            # Interactive process viewer
ps aux --sort=-%cpu | head -10  # List top 10 CPU-consuming processes
mpstat -P ALL 1 5  # Monitor CPU utilization per core
```

#### **Resolution:**
- Identify the process consuming high CPU and restart if necessary:
  ```bash
  kill -9 <PID>  # Force kill a process
  systemctl restart <service>  # Restart a service
  ```
- Optimize application code if a specific service is causing high load.
- Consider resource allocation tuning.

---

### **1.2 High Memory Usage**
#### **Symptoms:**
- Excessive swapping
- System slowdowns
- "Out of memory" (OOM) errors

#### **Diagnosis:**
```bash
free -m               # Check memory usage
vmstat 1 5           # View memory stats in real time
ps aux --sort=-%mem | head -10  # List top memory-consuming processes
```

#### **Resolution:**
- Identify and restart memory-heavy processes.
- Clear unused memory:
  ```bash
  echo 3 > /proc/sys/vm/drop_caches
  ```
- Optimize application memory usage.

---

### **1.3 Disk Space Full**
#### **Symptoms:**
- Unable to create or write files
- System warnings about low disk space

#### **Diagnosis:**
```bash
df -h      # Check disk usage per partition
du -sh /*  # Find large files/directories
find / -size +500M  # Locate files larger than 500MB
```

#### **Resolution:**
- Remove unnecessary files:
  ```bash
  rm -rf /var/log/*.gz  # Delete old logs
  ```
- Resize disk partitions or extend storage.

---

### **1.4 Network Issues**
#### **Symptoms:**
- Unable to connect to remote servers
- Slow network speeds

#### **Diagnosis:**
```bash
ping -c 4 google.com   # Check connectivity
traceroute google.com  # Identify network hops
netstat -tulnp         # View open ports and services
ss -tulnp             # Alternative to netstat
```

#### **Resolution:**
- Restart network services:
  ```bash
  systemctl restart networking
  ```
- Adjust firewall rules if necessary:
  ```bash
  iptables -L
  ```

---

### **1.5 Service Failures**
#### **Symptoms:**
- Applications not responding
- Service downtime

#### **Diagnosis:**
```bash
systemctl status <service>
journalctl -u <service> --no-pager | tail -20
```

#### **Resolution:**
- Restart the service:
  ```bash
  systemctl restart <service>
  ```
- Check logs and reconfigure if needed.

---

## **2. Advanced Debugging Techniques**

### **2.1 Checking System Logs**
```bash
dmesg | tail -20  # View last 20 system logs
journalctl -xe  # Show extended logs
```

### **2.2 Debugging Process Issues**
```bash
strace -p <PID>  # Trace system calls of a process
lsof -p <PID>    # List open files by a process
```

### **2.3 Finding and Killing Zombie Processes**
```bash
ps aux | grep Z  # Find zombie processes
kill -9 <PID>    # Kill zombie process
```

---

## **3. Preventive Maintenance Tips**
- **Automate log rotation:** Use `logrotate` to prevent logs from consuming disk space.
- **Monitor system performance:** Set up `top`, `htop`, and `prometheus` monitoring.
- **Apply security patches regularly:** Use `apt update && apt upgrade` or `yum update`.

---

## **Conclusion**
This guide provides a fundamental understanding of Linux troubleshooting with hands-on commands. Regular monitoring and proactive maintenance help prevent issues before they impact production.

For further learning, refer to Linux documentation and administrator best practices.

---

