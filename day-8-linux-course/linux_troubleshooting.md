# Linux System Troubleshooting | Day 8 | Complete Linux Course 

Troubleshooting is one of the key skills required for DevOps Engineers. DevOps candidates need to demonstrate the ability to diagnose system issues, manage logs effectively, and understand system diagnostics tools. Additionally, the ability to recover from system failures is a critical aspect of system administration. Below is a detailed overview of **System Logs**, **System Diagnostics**, and **Backup and Recovery** concepts for the DevOps Engineers.

---
### **1. System Logs**

System logs are vital for troubleshooting as they capture detailed information about the system’s operations, errors, and services. Linux systems store log files in the `/var/log/` directory.

#### **Key Log Files**:
- **/var/log/messages**: Contains general system activity logs. It logs most system-related messages, such as hardware errors, system startup, and shutdown messages, and service failures.
- **/var/log/dmesg**: Contains messages from the kernel. Useful for troubleshooting hardware issues, such as driver problems or hardware malfunctions.
- **/var/log/secure**: Logs security-related messages, including user login attempts, authentication failures, and sudo commands.
- **/var/log/cron**: Logs cron job executions, which helps diagnose issues with scheduled tasks.
- **/var/log/httpd/access_log** and **/var/log/httpd/error_log**: Store logs for Apache HTTP server, useful for debugging web server issues.
- **/var/log/boot.log**: Logs system boot messages. Useful for diagnosing boot-related problems.
  
#### **Managing System Logs**:
1. **View Log Files**:
   - To view logs in real-time, use `tail` or `less`:
     ```bash
     tail -f /var/log/messages
     less /var/log/secure
     ```
   
2. **Log Rotation**:
   - Use **logrotate** to manage the size of log files and ensure they are archived and compressed regularly.
   - Logrotate configuration is located in `/etc/logrotate.conf` or `/etc/logrotate.d/`.

3. **Syslog Configuration**:
   - **rsyslog** is the default service used to manage system logs. The configuration file for rsyslog is located at `/etc/rsyslog.conf`.
   - You can configure which log messages go to different files or even forward logs to remote systems.

4. **Troubleshooting with Logs**:
   - **Log File Example**:
     - To troubleshoot a service that fails to start, view the logs:
       ```bash
       journalctl -u httpd
       ```
     - Check for errors in `/var/log/messages` for issues with system services:
       ```bash
       grep -i "error" /var/log/messages
       ```

---

### **2. System Diagnostics**

System diagnostics tools help in identifying system performance issues, hardware failures, and overall system health.

#### **Key Diagnostic Tools**:

1. **`dmesg`**:
   - Displays messages from the kernel ring buffer. Useful for debugging hardware issues (e.g., disk failures, network interface errors).
   ```bash
   dmesg | less
   ```

2. **`top`**:
   - Real-time system monitoring tool that shows CPU, memory, and process usage. It's useful for identifying resource hogs.
   ```bash
   top
   ```
   - Use `htop` for a more user-friendly, interactive version of `top`.

3. **`vmstat`**:
   - Reports virtual memory statistics, CPU activity, and system performance. Useful for diagnosing system resource problems.
   ```bash
   vmstat 1
   ```

4. **`iostat`**:
   - Reports CPU statistics and input/output statistics for devices and partitions. This helps troubleshoot disk performance issues.
   ```bash
   iostat -x 1
   ```

5. **`free`**:
   - Displays memory usage information, including total, used, free, and swap memory.
   ```bash
   free -m
   ```

6. **`sar`**:
   - Collects, reports, and saves system activity information. It can be used for historical performance analysis.
   ```bash
   sar -u 1 5
   ```

7. **`netstat`**:
   - Displays active network connections and listening ports. Useful for diagnosing network issues.
   ```bash
   netstat -tuln
   ```

8. **`ss`**:
   - Similar to `netstat`, but faster and more detailed.
   ```bash
   ss -tuln
   ```

9. **`lsof`**:
   - Lists open files and the processes that opened them. Useful for identifying files that are being used by a process.
   ```bash
   lsof | grep <filename>
   ```

10. **`systemctl`**:
    - Use `systemctl` to check the status of system services, see logs, and troubleshoot service failures.
    ```bash
    systemctl status <service_name>
    journalctl -u <service_name>
    ```

---

### **3. Backup and Recovery**

Backup and recovery are essential aspects of system administration to ensure data safety and integrity. In case of hardware failures, system crashes, or accidental deletions, backups help in restoring the system to a working state.

#### **Backup Tools**:

1. **`rsync`**:
   - Used for efficient backups and syncing files locally or remotely.
   - Syntax:
     ```bash
     rsync -av /source/directory /destination/directory
     rsync -av /local/directory user@remote:/backup/directory
     ```

2. **`tar`**:
   - A common command used to create backups and compress directories.
   - Syntax for creating a backup:
     ```bash
     tar -czf backup.tar.gz /path/to/directory
     ```
   - Syntax for extracting a backup:
     ```bash
     tar -xzf backup.tar.gz -C /restore/path
     ```

3. **`cp` and `mv`**:
   - Use `cp` for simple file copying.
     ```bash
     cp -r /source /destination
     ```
   - Use `mv` for moving files between directories.

4. **Automating Backups with `cron`**:
   - Use cron jobs to schedule regular backups.
   - Example: Backup every day at 2:00 AM:
     ```bash
     0 2 * * * /usr/bin/rsync -av /home/ /backup/home/
     ```

#### **Restore and Recovery**:

1. **System Recovery from Backups**:
   - If a system crashes and you have a backup, you can restore it using `rsync` or `tar`:
     ```bash
     rsync -av /backup/location /restore/location
     ```

2. **Recovery from Boot Problems**:
   - If the system is unable to boot, use a live CD/USB and mount the root filesystem:
     ```bash
     mount /dev/sda1 /mnt
     chroot /mnt
     ```
   - Perform diagnostics or restore files from backups while the system is running from the live environment.

3. **Using System Snapshots**:
   - Red Hat-based systems can use **LVM snapshots** for creating backups of the filesystem.
     ```bash
     lvcreate --size 1G --snapshot --name backup_snap /dev/vol_group/vol_name
     ```

4. **GRUB Recovery**:
   - If the system fails to boot due to bootloader issues, you can use **GRUB rescue mode** to recover the boot process. From the GRUB prompt:
     ```bash
     grub> ls
     grub> set root=(hd0,1)
     grub> linux /boot/vmlinuz-<version> root=/dev/sda1
     grub> initrd /boot/initramfs-<version>.img
     grub> boot
     ```

---

### **Troubleshooting Example Scenarios**

#### **Scenario 1: Service Fails to Start After Update**
**Problem**: After performing a system update, a service (e.g., Apache HTTPD) fails to start. The error message is:
```
Failed to start httpd.service: Unit httpd.service not found.
```

**Solution**:
1. **Check the status of the service**:
   ```bash
   systemctl status httpd
   ```
2. **Check the logs for errors**:
   ```bash
   journalctl -u httpd
   ```
3. **Verify the service is installed**:
   ```bash
   rpm -q httpd
   ```
4. **Reinstall the package**:
   ```bash
   yum reinstall httpd
   ```
5. **Check for SELinux restrictions**:
   ```bash
   setenforce 0
   ```

#### **Scenario 2: Disk Full**
**Problem**: The system is running out of space, and applications are not functioning properly due to full disk space.

**Solution**:
1. **Check disk space usage**:
   ```bash
   df -h
   ```
2. **Check which directories are using the most space**:
   ```bash
   du -sh /var/* | sort -h
   ```
3. **Clean up log files** or remove old unnecessary files:
   ```bash
   find /var/log -type f -name "*.log" -mtime +30 -exec rm -f {} \;
   ```

---

Sure! Here’s a comprehensive **example of system troubleshooting** that covers a variety of aspects including system logs, diagnostics, backup, and recovery.

### **Real-World Scenario: Apache Service Failing to Start Due to Disk Space Issue**

#### **Problem:**
After an update, the Apache HTTPD service fails to start on a Red Hat-based Linux system. You attempt to start the service using `systemctl start httpd`, but the command fails, and you get the following error message:
```
Failed to start httpd.service: Unit httpd.service not found.
```
Upon further investigation, you find the system is running out of disk space, and various applications are affected.

#### **Steps to Troubleshoot and Resolve the Issue:**

---

### **Step 1: Check Disk Space Usage**

1. **Check Disk Space with `df` Command**:
   First, check the disk space on the system to see if there are any issues with full partitions. This will help identify if there is a storage issue.
   ```bash
   df -h
   ```
   Output:
   ```
   Filesystem      Size  Used Avail Use% Mounted on
   /dev/sda1        50G   50G     0 100% /
   /dev/sdb1       100G   50G  50G  50% /data
   ```
   The root filesystem (`/dev/sda1`) is completely full, which is likely causing issues with services like Apache.

2. **Investigate Which Directories Are Consuming Disk Space**:
   ```bash
   du -sh /var/* | sort -h
   ```
   Output:
   ```
   10M    /var/cache
   500M   /var/log
   10M    /var/tmp
   10G    /var/www
   ```
   The `/var/log` directory has consumed a significant portion of the disk space due to large log files. Apache logs (in `/var/log/httpd/`) might be particularly large.

---

### **Step 2: Resolve Disk Space Issue**

1. **Clean Up Old Log Files**:
   Since log files are taking up a lot of space, you can clean up older logs in `/var/log` directory. You can use the `find` command to delete logs older than 30 days:
   ```bash
   find /var/log -type f -name "*.log" -mtime +30 -exec rm -f {} \;
   ```

2. **Compress Log Files**:
   To retain logs but save space, you can compress old log files:
   ```bash
   gzip /var/log/httpd/access_log
   gzip /var/log/httpd/error_log
   ```

3. **Check Disk Space Again**:
   After cleaning up the logs, check the disk space again:
   ```bash
   df -h
   ```
   Output:
   ```
   Filesystem      Size  Used Avail Use% Mounted on
   /dev/sda1        50G   35G  10G  80% /
   /dev/sdb1       100G   50G  50G  50% /data
   ```

---

### **Step 3: Investigate the Apache Service Issue**

1. **Check Apache Status**:
   Run the `systemctl status httpd` command to get more details about the Apache service status:
   ```bash
   systemctl status httpd
   ```
   Output:
   ```
   httpd.service - The Apache HTTP Server
      Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
      Active: failed (Result: exit-code) since Fri 2024-12-21 10:15:24 UTC; 30min ago
    Main PID: 1234 (code=exited, status=1/FAILURE)
   ```

2. **Check Apache Logs**:
   Apache logs are stored in `/var/log/httpd/`. You can review the logs to find out why the service failed:
   ```bash
   tail -f /var/log/httpd/error_log
   ```
   Sample log output might show an error like:
   ```
   [Sat Dec 21 10:10:00.000000 2024] [core:notice] [pid 1234] AH00094: Command line: '/usr/sbin/httpd -D FOREGROUND'
   [Sat Dec 21 10:10:00.000000 2024] [core:warn] [pid 1234] AH00117: /var/log/httpd/access_log is not writable.
   ```

3. **Resolve Log Directory Permissions**:
   It seems that Apache is unable to write to the log files because of the disk space issue. After cleaning up the disk, ensure the correct permissions on the log directories:
   ```bash
   chown -R apache:apache /var/log/httpd/
   chmod -R 755 /var/log/httpd/
   ```

---

### **Step 4: Restart Apache and Verify the Service**

1. **Restart the Apache Service**:
   After resolving the disk space and fixing the permissions, restart the Apache service:
   ```bash
   systemctl restart httpd
   ```

2. **Verify Apache is Running**:
   Verify that the service started successfully:
   ```bash
   systemctl status httpd
   ```
   Output:
   ```
   httpd.service - The Apache HTTP Server
      Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
      Active: active (running) since Fri 2024-12-21 10:50:00 UTC; 5min ago
   ```

3. **Test Apache**:
   You can test if Apache is serving web pages by accessing the server from a browser using the server's IP address:
   ```
   http://<server_ip>
   ```

---

### **Step 5: Backup and Recovery (Optional)**

1. **Create a Backup**:
   After fixing the issue, create a backup of the system, including the Apache configuration and web content, to avoid future data loss:
   ```bash
   tar -czf /backup/apache_backup.tar.gz /etc/httpd /var/www
   ```

2. **Schedule Automatic Backups**:
   Schedule daily backups using `cron` to ensure regular backups:
   ```bash
   crontab -e
   ```
   Add the following line to back up Apache every day at 2:00 AM:
   ```
   0 2 * * * tar -czf /backup/apache_backup_$(date +\%F).tar.gz /etc/httpd /var/www
   ```

---

### **Step 6: System Diagnostics**

1. **Check System Resources**:
   If the issue was related to system resources (e.g., high CPU usage), use diagnostic tools to monitor the system:
   ```bash
   top
   ```
   - Look for any processes that are consuming too many resources. If a service like Apache or a background process is consuming high CPU or memory, it may indicate the need for optimization.

2. **Check Kernel Logs**:
   If there are hardware issues that may have contributed to the disk space problem (e.g., disk errors), check the kernel logs:
   ```bash
   dmesg | grep -i error
   ```

---

### **Step 7: Preventive Measures**

1. **Configure Disk Monitoring**:
   Set up alerts to monitor disk usage and send notifications when disk space is running low. You can use tools like **`monit`**, **`nagios`**, or **`zabbix`** for real-time monitoring.

2. **Automate Log Rotation**:
   Configure log rotation for Apache logs to ensure they don’t fill up the disk space again:
   - Edit the logrotate configuration file `/etc/logrotate.d/httpd`:
     ```bash
     /var/log/httpd/*.log {
         weekly
         rotate 4
         compress
         delaycompress
         notifempty
         create 640 apache apache
     }
     ```

---

### **Conclusion**:
This example demonstrates how to troubleshoot and resolve a common issue in a production environment. By following the systematic steps outlined above, you can:
- Identify disk space issues.
- Clean up old log files.
- Resolve permission issues for Apache.
- Restart the Apache service successfully.
- Set up backups and monitoring to prevent future issues.
<hr/>