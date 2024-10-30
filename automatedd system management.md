

### **1. Basic Task - Automated System Updates** (5 minutes)

1. **Objective**: Create a simple script to automate system updates.
   
2. **Instructions**:
   - Create a script file `auto_update.sh`.
   - Inside the script, add commands to update and upgrade the system.
   - Ensure the script logs each update attempt to a file for tracking.

3. **Script Example**:
   ```bash
   #!/bin/bash
   LOGFILE="/var/log/auto_update.log"
   
   echo "Updating system on $(date)" >> "$LOGFILE"
   sudo apt-get update -y && sudo apt-get upgrade -y >> "$LOGFILE" 2>&1
   echo "Update completed on $(date)" >> "$LOGFILE"
   ```

4. **Execution**:
   - Run the script with `sudo ./auto_update.sh`.
   - Check `/var/log/auto_update.log` for the update log.

---

### **2. Intermediate Task - Disk Space Monitoring** (5 minutes)**

1. **Objective**: Monitor disk usage and send an alert if it exceeds a threshold.

2. **Instructions**:
   - Create a script file `disk_monitor.sh`.
   - Set a disk usage threshold (e.g., 80%).
   - Use conditional logic to check if disk usage exceeds the threshold.
   - Print a warning message to the console or log it.

3. **Script Example**:
   ```bash
   #!/bin/bash
   THRESHOLD=80
   USAGE=$(df / | grep / | awk '{print $5}' | sed 's/%//')

   if [ "$USAGE" -gt "$THRESHOLD" ]; then
       echo "Disk usage is at ${USAGE}% on $(hostname) - Exceeds threshold!" | tee -a /var/log/disk_monitor.log
   else
       echo "Disk usage is within limit: ${USAGE}%"
   fi
   ```

4. **Execution**:
   - Run the script with `./disk_monitor.sh` to test it.
   - Review `/var/log/disk_monitor.log` if the usage exceeds the threshold.

---

### **3. Advanced Task - Scheduled Backups** (5 minutes)**

1. **Objective**: Automate the backup of a specific directory to a backup folder.

2. **Instructions**:
   - Create a script file `backup.sh`.
   - Use `rsync` to copy files from a source directory (e.g., `/home/user/docs`) to a backup directory (`/home/user/backup`).
   - Append a timestamp to each backup file for easy identification.

3. **Script Example**:
   ```bash
   #!/bin/bash
   SOURCE="/home/user/docs"
   DEST="/home/user/backup"
   TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
   
   mkdir -p "$DEST"
   rsync -av --delete "$SOURCE" "${DEST}/backup_${TIMESTAMP}" | tee -a /var/log/backup.log
   echo "Backup completed at ${TIMESTAMP}" >> /var/log/backup.log
   ```

4. **Execution**:
   - Run the script with `./backup.sh`.
   - Check `/var/log/backup.log` to ensure it logs each backup.

---

### **4. Advanced Task - Service Health Check and Auto-Restart** (5 minutes)**

1. **Objective**: Monitor the status of a service (e.g., `nginx`) and restart it if it’s not running.

2. **Instructions**:
   - Create a script file `service_monitor.sh`.
   - Check if the service is running; if not, restart it and log the action.
   
3. **Script Example**:
   ```bash
   #!/bin/bash
   SERVICE="nginx"
   LOGFILE="/var/log/service_monitor.log"

   if ! systemctl is-active --quiet "$SERVICE"; then
       echo "$SERVICE is down. Restarting at $(date)" | tee -a "$LOGFILE"
       sudo systemctl start "$SERVICE"
       echo "$SERVICE restarted successfully." >> "$LOGFILE"
   else
       echo "$SERVICE is running."
   fi
   ```

4. **Execution**:
   - Run the script with `./service_monitor.sh`.
   - Check `/var/log/service_monitor.log` to confirm if the service restarted when down.

---

### **Recap & Tips**

After completing the exercises, you should understand:
- Basic automation for updates and monitoring.
- Intermediate usage of conditional logic and logging.
- Advanced service management through monitoring and scheduled tasks.

---




---

### **Test Questions**

#### **1. System Updates**

1. **What command would you use to update the package list on an Ubuntu system?**
   - a) `sudo apt-get install`
   - b) `sudo apt-get update`
   - c) `sudo apt-get upgrade`
   - d) `sudo apt-cache search`

2. **In a Bash script, how would you redirect both standard output and standard error to a log file?**
   - a) `>> logfile`
   - b) `> logfile`
   - c) `>> logfile 2>&1`
   - d) `2>&1 >> logfile`

#### **2. Disk Space Monitoring**

3. **What command can be used in a script to check the disk space usage on a Linux system?**
   - a) `df -h`
   - b) `du -sh`
   - c) `ls -l`
   - d) `free -m`

4. **In a disk monitoring script, if a threshold is set at 80%, which conditional statement would check if usage is over the threshold?**
   - a) `[ "$USAGE" = "$THRESHOLD" ]`
   - b) `[ "$USAGE" -lt "$THRESHOLD" ]`
   - c) `[ "$USAGE" -gt "$THRESHOLD" ]`
   - d) `[ "$USAGE" -le "$THRESHOLD" ]`

#### **3. Scheduled Backups**

5. **Which command would you use in a backup script to copy files from one directory to another, while deleting files in the destination that no longer exist in the source?**
   - a) `cp -r`
   - b) `rsync -av`
   - c) `rsync -av --delete`
   - d) `mv -f`

6. **What command would you use to create a new directory in Linux if it doesn’t already exist?**
   - a) `mkdir -p`
   - b) `mkdir -f`
   - c) `mkdir -i`
   - d) `mkdir -r`

#### **4. Service Health Check and Auto-Restart**

7. **What command would you use to check if a service, like `nginx`, is currently active?**
   - a) `service nginx check`
   - b) `systemctl status nginx`
   - c) `service nginx status`
   - d) `systemctl is-active nginx`

8. **Which command in a Bash script would you use to start a service if it’s not running?**
   - a) `start-service nginx`
   - b) `systemctl start nginx`
   - c) `service start nginx`
   - d) `init start nginx`

#### **5. General Bash Scripting**

9. **What does the following line mean in a Bash script: `#!/bin/bash`?**
   - a) It defines the name of the script.
   - b) It specifies that the script should be run using the Bash shell.
   - c) It starts a comment in the script.
   - d) It defines the path to the log file.

10. **If you want to save the current date in the format `YYYYMMDD_HHMMSS` in a Bash variable called `TIMESTAMP`, which command would you use?**
    - a) `TIMESTAMP=$(date +'%Y%m%d %H:%M:%S')`
    - b) `TIMESTAMP=$(date +"%Y%m%d_%H%M%S")`
    - c) `TIMESTAMP=$(date '%Y-%m-%d')`
    - d) `TIMESTAMP=$(date +'%Y/%m/%d')`

---

### **Answer Key**

1. **b) `sudo apt-get update`**
2. **c) `>> logfile 2>&1`**
3. **a) `df -h`**
4. **c) `[ "$USAGE" -gt "$THRESHOLD" ]`**
5. **c) `rsync -av --delete`**
6. **a) `mkdir -p`**
7. **d) `systemctl is-active nginx`**
8. **b) `systemctl start nginx`**
9. **b) It specifies that the script should be run using the Bash shell.**
10. **b) `TIMESTAMP=$(date +"%Y%m%d_%H%M%S")`**

---

d