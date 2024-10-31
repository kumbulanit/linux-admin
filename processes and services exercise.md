
### **Exercise: Managing Processes and Services**

#### **Part 1: Basic Process Management (5 minutes)**

1. **Viewing Processes**:
   - Run the `ps aux` command. Identify the following details for any process:
     - The Process ID (PID)
     - CPU and memory usage
     - The command associated with the process

2. **Using `top`**:
   - Run the `top` command to monitor processes in real-time.
   - Sort the processes by CPU usage and identify the top three CPU-consuming processes.

3. **Running a Process in the Background**:
   - Execute the command `sleep 100 &` to start a process in the background.
   - Use `jobs` to confirm that the process is running.

#### **Part 2: Intermediate Process Control (5 minutes)**

1. **Killing a Process**:
   - Use the `kill` command to terminate the `sleep` process from Part 1. 
   - Confirm that it was terminated using `ps aux` or `jobs`.

2. **Renicing a Process**:
   - Start another background process, such as `sleep 200 &`.
   - Use the `ps aux | grep sleep` command to find the PID of this process.
   - Change the process’s priority using the `renice` command, setting it to a priority level of 10.
   - Verify the change by running `ps -o pid,ni,comm -p <PID>`, where `<PID>` is the process ID of your `sleep` command.

#### **Part 3: Service Management with Systemd (5 minutes)**

1. **Checking the Status of a Service**:
   - Run `systemctl status sshd` (or any common service like `cron` or `nginx` if available).
   - Take note of whether the service is active, inactive, or failed.

2. **Starting and Stopping a Service**:
   - Use `sudo systemctl stop sshd` to stop the service.
   - Confirm it has stopped with `systemctl status sshd`.
   - Restart the service using `sudo systemctl start sshd`.
   - Verify its status once more.

3. **Enabling and Disabling a Service**:
   - Use `sudo systemctl disable sshd` to disable the service from starting at boot.
   - Check by running `systemctl is-enabled sshd`.
   - Re-enable the service with `sudo systemctl enable sshd` and verify it is now enabled at boot.

#### **Part 4: Advanced Service Management and Troubleshooting (5 minutes)**

1. **Examining Service Logs**:
   - Run `journalctl -u sshd` to view logs for the `sshd` service.
   - Note any errors or warnings, if present.

2. **Creating a Custom Service**:
   - Create a simple bash script called `hello.sh` with the following content:
     ```bash
     #!/bin/bash
     echo "Hello, World!"
     ```
   - Make the script executable with `chmod +x hello.sh`.
   - Create a service file in `/etc/systemd/system/hello.service` with the following content:
     ```ini
     [Unit]
     Description=Hello World Service

     [Service]
     ExecStart=/path/to/your/hello.sh

     [Install]
     WantedBy=multi-user.target
     ```
   - Replace `/path/to/your/hello.sh` with the full path to your script.
   - Reload systemd with `sudo systemctl daemon-reload`.
   - Start the service with `sudo systemctl start hello`.
   - Check the output with `journalctl -u hello`.

3. **Automating the Service with a Timer**:
   - Create a timer file in `/etc/systemd/system/hello.timer` with the following content:
     ```ini
     [Unit]
     Description=Run Hello World Service Every 1 Minute

     [Timer]
     OnBootSec=1min
     OnUnitActiveSec=1min
     Unit=hello.service

     [Install]
     WantedBy=timers.target
     ```
   - Enable and start the timer with `sudo systemctl enable --now hello.timer`.
   - Confirm it’s working by checking the logs with `journalctl -u hello`.

---

### **Reflection Questions**
- How would you monitor memory usage across multiple processes at once?
- What would you do if a critical service repeatedly failed to start?
- Why might you use a `nice` value adjustment or a `cgroup` limit on a process?

