

### 1. Automated System Health Monitoring Script

This script monitors CPU usage, memory usage, disk usage, and network connectivity. If any metrics exceed specified thresholds, it logs the details and can notify the system administrator via email.

```bash
#!/bin/bash

# Configuration
LOG_FILE="/var/log/system_health.log"
EMAIL="admin@example.com"  # Change to your email address
CPU_THRESHOLD=80
MEMORY_THRESHOLD=80
DISK_THRESHOLD=90
INTERFACE="eth0"  # Change as needed

# Log function
log_message() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1" >> $LOG_FILE
}

# Check CPU usage
CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | sed "s/.*, *\([0-9.]*\)%* id.*/\1/" | awk '{print 100 - $1}')
if (( $(echo "$CPU_USAGE > $CPU_THRESHOLD" | bc -l) )); then
    log_message "High CPU Usage: ${CPU_USAGE}%"
    echo "High CPU Usage: ${CPU_USAGE}%" | mail -s "CPU Alert" "$EMAIL"
fi

# Check Memory usage
MEMORY_USAGE=$(free | awk '/Mem/{printf("%.2f"), $3/$2*100}')
if (( $(echo "$MEMORY_USAGE > $MEMORY_THRESHOLD" | bc -l) )); then
    log_message "High Memory Usage: ${MEMORY_USAGE}%"
    echo "High Memory Usage: ${MEMORY_USAGE}%" | mail -s "Memory Alert" "$EMAIL"
fi

# Check Disk usage
DISK_USAGE=$(df / | grep / | awk '{ print $5 }' | sed 's/%//g')
if [ "$DISK_USAGE" -gt "$DISK_THRESHOLD" ]; then
    log_message "High Disk Usage: ${DISK_USAGE}%"
    echo "High Disk Usage: ${DISK_USAGE}%" | mail -s "Disk Alert" "$EMAIL"
fi

# Check Network Status
ping -c 1 google.com > /dev/null
if [ $? -ne 0 ]; then
    log_message "Network is down!"
    echo "Network is down!" | mail -s "Network Alert" "$EMAIL"
fi

log_message "Health check completed."
```

### 2. Disk Space Monitoring and Cleanup Script

This script checks the disk space usage of specific directories and automatically cleans up old log files if disk usage exceeds a specified threshold.

```bash
#!/bin/bash

# Configuration
LOG_DIR="/var/log/myapp"  # Change to your log directory
LOG_FILE="/var/log/disk_cleanup.log"
THRESHOLD=90  # Disk usage percentage threshold
DAYS_TO_KEEP=30  # Number of days to keep logs

# Log function
log_message() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1" >> $LOG_FILE
}

# Check Disk usage
DISK_USAGE=$(df "$LOG_DIR" | grep / | awk '{ print $5 }' | sed 's/%//g')
if [ "$DISK_USAGE" -gt "$THRESHOLD" ]; then
    log_message "Disk usage is high: ${DISK_USAGE}% - cleaning up old logs."
    find "$LOG_DIR" -type f -mtime +$DAYS_TO_KEEP -exec rm -f {} \;
    log_message "Old logs cleaned up. Current disk usage: $(df "$LOG_DIR" | grep / | awk '{ print $5 }')"
else
    log_message "Disk usage is under control: ${DISK_USAGE}%."
fi
```

### 3. Automated System Updates Script

This script checks for available package updates and installs them automatically. It logs the update history and can send notifications if updates are applied.

```bash
#!/bin/bash

# Configuration
LOG_FILE="/var/log/apt_update.log"
EMAIL="admin@example.com"  # Change to your email address

# Log function
log_message() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1" >> $LOG_FILE
}

# Check for updates
UPDATES=$(apt list --upgradable 2> /dev/null | grep -v 'Listing...' | wc -l)

if [ "$UPDATES" -gt 0 ]; then
    log_message "$UPDATES updates available. Installing..."
    apt update && apt upgrade -y
    log_message "Updates installed."
    echo "System updated successfully. $UPDATES packages were upgraded." | mail -s "System Update Notification" "$EMAIL"
else
    log_message "No updates available."
fi
```

### 4. Hardware Monitoring Script

This script uses `lm-sensors` to monitor hardware temperatures and fan speeds. It logs the readings and can send alerts if temperatures exceed safe limits.

```bash
#!/bin/bash

# Configuration
LOG_FILE="/var/log/hardware_monitor.log"
EMAIL="admin@example.com"  # Change to your email address
TEMP_THRESHOLD=75  # Celsius

# Install lm-sensors if not already installed
if ! command -v sensors &> /dev/null; then
    apt install lm-sensors -y
    sensors-detect --auto
fi

# Log function
log_message() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1" >> $LOG_FILE
}

# Check hardware temperatures
sensors | while read -r line; do
    if echo "$line" | grep -q "°C"; then
        TEMP=$(echo "$line" | grep -o '[0-9]*\.[0-9]*\|[0-9]*' | tail -1)
        if [ "$TEMP" -gt "$TEMP_THRESHOLD" ]; then
            log_message "High temperature detected: ${TEMP}°C"
            echo "High temperature detected: ${TEMP}°C" | mail -s "Temperature Alert" "$EMAIL"
        fi
    fi
done

log_message "Hardware monitoring completed."
```

### 5. Automated Service Monitoring Script

This script checks the status of specified services and restarts them automatically if they are not running.

```bash
#!/bin/bash

# Configuration
SERVICES=("apache2" "mysql")  # Add your services here
LOG_FILE="/var/log/service_monitor.log"

# Log function
log_message() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1" >> $LOG_FILE
}

# Check each service
for SERVICE in "${SERVICES[@]}"; do
    if systemctl is-active --quiet $SERVICE; then
        log_message "$SERVICE is running."
    else
        log_message "$SERVICE is not running. Attempting to restart..."
        systemctl start $SERVICE
        if systemctl is-active --quiet $SERVICE; then
            log_message "$SERVICE restarted successfully."
        else
            log_message "Failed to restart $SERVICE."
        fi
    fi
done
```

### Scheduling Scripts with Cron

To ensure these scripts run automatically without human intervention, you can schedule them using cron:

1. Open the cron table for editing:
   ```bash
   crontab -e
   ```

2. Add entries for the scripts, specifying when you want them to run. Here’s an example of how you might schedule these scripts to run every hour:
   ```plaintext
   0 * * * * /path/to/system_health_monitor.sh
   0 * * * * /path/to/disk_cleanup.sh
   0 2 * * * /path/to/system_updates.sh
   */30 * * * * /path/to/hardware_monitor.sh
   */10 * * * * /path/to/service_monitor.sh
   ```

