# Linux-troubleshooting

Common production issues in Linux environments include:

**High CPU, memory, or disk usage** (e.g., performance bottlenecks)
**Service crashes or unresponsiveness**(e.g., web server down, database failure)
**Network connectivity issues** (e.g., unreachable hosts, slow response times)
**File system errors** (e.g., disk full, corrupted files)
**Permission issues** (e.g., unauthorized access, missing execute permissions)
**Dependency failures** (e.g., missing libraries, outdated packages)
**Security incidents** (e.g., unauthorized logins, malware)
Each of these issues has specific troubleshooting steps and Linux commands.

**1. Essential Linux Commands for Debugging**
Here are the most used commands categorized by their function:

**System Monitoring**
```
top / htop – View real-time CPU and memory usage.
vmstat 1 – Monitor CPU, memory, I/O, and system performance.
iostat -x 1 – Check disk I/O performance.
free -m – Check memory usage.
df -h – Check disk usage.
du -sh /var/log – Find the size of log directories.
uptime – See system load averages.
```
**Process Management**
```
ps aux --sort=-%cpu – List processes sorted by CPU usage.
ps aux --sort=-%mem – List processes sorted by memory usage.
pidstat -p <PID> – Show CPU and I/O usage of a specific process.
strace -p <PID> – Trace system calls of a process.
kill -9 <PID> – Forcefully kill a process.
pkill -f <process-name> – Kill all processes matching a name.
systemctl restart <service> – Restart a systemd-managed service.
```
**Networking**
```
ping <host> – Check if a host is reachable.
netstat -tulnp – List open ports and listening services.
ss -tulnp – Alternative to netstat with better performance.
curl -I <url> – Check HTTP response headers.
wget --spider <url> – Test if a webpage is accessible.
traceroute <host> – Trace network hops to a destination.
ip a – Show IP addresses and interfaces.
ip r – Show routing table.
iptables -L -n -v – Check firewall rules.
```
**Log Analysis**
```
journalctl -u <service> – View logs of a systemd service.
tail -f /var/log/syslog – Monitor system logs in real-time.
tail -f /var/log/nginx/error.log – Monitor web server logs.
grep "ERROR" /var/log/app.log – Find error messages in logs.
less /var/log/messages – View general system logs.
```
**Disk and File System**
```
ls -lh – List files with human-readable sizes.
stat <file> – Get detailed file information.
mount | column -t – Show mounted filesystems.
fsck /dev/sda1 – Check and repair a filesystem.
tune2fs -l /dev/sda1 – View filesystem parameters.
```
**User & Permissions**
```
whoami – Show the current user.
id <username> – Show user ID and group info.
groups <username> – Show group memberships.
chmod 755 <file> – Change file permissions.
chown user:group <file> – Change file owner.
sudo -l – List commands a user can run as sudo.
```
**2. Replicating and Troubleshooting Common Production Issues**
Now, let’s go through specific production issues and how to replicate, diagnose, and fix them.

**Issue 1: High CPU Usage**
Replication:
Run a CPU-intensive process:

```
stress --cpu 4 --timeout 60
or
yes > /dev/null &
```
Diagnosis:
Check top processes consuming CPU:
```
top
or
ps aux --sort=-%cpu | head -5
```
Fix:
Find and kill the process:

```
kill -9 <PID>
or
pkill -f "yes"
```
**Issue 2: Service is Down**
Replication:
Stop a service (example: Nginx):
```
sudo systemctl stop nginx
```
Diagnosis:
Check service status:
```
systemctl status nginx

or check logs:

journalctl -u nginx --no-pager | tail -20
```
Fix:
Restart the service:
```
sudo systemctl restart nginx
```
If it doesn’t start, check:
```
sudo nginx -t
```
for configuration errors.

**Issue 3: Disk Space Full**
Replication:
Fill up a disk:
```
dd if=/dev/zero of=/bigfile bs=1M count=100000
```
Diagnosis:
Check disk usage:

```
df -h
```
Find large files:
```
du -ah /var | sort -rh | head -10
```
Fix:
Delete large files:

```
rm -f /bigfile
```

**Issue 4: Network Connectivity Issue**
Replication:
Block traffic using firewall:

```
sudo iptables -A INPUT -s 8.8.8.8 -j DROP
```
Diagnosis:
Check network connectivity:

```
ping google.com
```
Check active connections:

```
netstat -tulnp
```
Fix:
Remove firewall rule:

```
sudo iptables -D INPUT -s 8.8.8.8 -j DROP
```
Issue 5: File Permission Denied
Replication:
Create a restricted file:

```
touch /tmp/secure-file
chmod 000 /tmp/secure-file
```
Diagnosis:
Try accessing it:

```
cat /tmp/secure-file
```
Fix:
Grant permissions:

```
chmod 644 /tmp/secure-file
```

**Issue 6: Outdated or Missing Dependencies**
Replication:
Try running a missing package:

```
python3 -m pip list | grep requests
```
Fix:
Install missing dependency:

```
sudo apt install python3-requests -y
```
or

```
pip install requests
```
**3. Best Practices for Debugging and Testing**
- Use staging environments to test fixes before applying in production.
- Automate monitoring with tools like Prometheus, Grafana, and ELK stack.
- Enable logging and analyze logs using tools like journalctl, grep, and awk.
- Keep backups before making major changes.
- Follow change management processes to ensure system stability.
- This is just a starting point. Do you want to focus on any particular type of issue in more depth?
