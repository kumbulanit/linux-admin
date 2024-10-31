

### **Exercise Outline**

---

#### **Part 1: Process Management Basics (10 minutes)**

1. **View and Monitor Processes**
   - **List processes for the current user**:
     ```bash
     ps
     ```
   - **List all processes**:
     ```bash
     ps aux
     ```
   - **Filter processes by name**:
     ```bash
     ps aux | grep <process_name>
     ```
   - **View live processes** with `top`:
     ```bash
     top
     ```

2. **Terminate Processes**
   - Use `kill` to stop a process by PID:
     ```bash
     kill <PID>
     ```
   - Use `pkill` to kill processes by name:
     ```bash
     pkill <process_name>
     ```

3. **View Specific Process Details**
   - Find the status of a specific process (e.g., `sshd`):
     ```bash
     ps -aux | grep sshd
     ```
   - **Question**: Which processes use the most CPU and memory?

---

#### **Part 2: Advanced Process Management (10 minutes)**

1. **Process Prioritization with `nice` and `renice`**
   - **Start a command with a low priority**:
     ```bash
     nice -n 10 <command>
     ```
   - **Change priority of an existing process**:
     ```bash
     sudo renice -5 <PID>
     ```

2. **System Resource Monitoring with `htop`**
   - **Install and open `htop`**:
     ```bash
     sudo apt install htop   # for Debian/Ubuntu
     sudo yum install htop   # for CentOS/RHEL
     htop
     ```
   - Identify the process using the most CPU or memory.

---

#### **Part 3: Working with Services - Basic (10 minutes)**

1. **List, Start, Stop, and Restart Services**
   - **View all active services**:
     ```bash
     systemctl list-units --type=service
     ```
   - **Start a service** (e.g., `apache2`):
     ```bash
     sudo systemctl start apache2
     ```
   - **Stop a service**:
     ```bash
     sudo systemctl stop apache2
     ```
   - **Restart a service**:
     ```bash
     sudo systemctl restart apache2
     ```

2. **Enable and Disable Services**
   - **Enable a service to start on boot**:
     ```bash
     sudo systemctl enable apache2
     ```
   - **Disable a service from starting on boot**:
     ```bash
     sudo systemctl disable apache2
     ```

3. **Check Service Status**
   - **Check if a service is running**:
     ```bash
     systemctl status apache2
     ```
   - **Question**: What’s the current status of `apache2`? Is it active?

---

#### **Part 4: Installing and Managing a New Service (10 minutes)**

1. **Install a New Service (e.g., Nginx)**
   - **Install Nginx**:
     ```bash
     sudo apt update
     sudo apt install nginx
     ```
   - **Verify the installation** by checking its status:
     ```bash
     systemctl status nginx
     ```

2. **Start and Enable the New Service**
   - **Start Nginx**:
     ```bash
     sudo systemctl start nginx
     ```
   - **Enable it to start on boot**:
     ```bash
     sudo systemctl enable nginx
     ```

3. **Accessing the Service**
   - **Verify that Nginx is running** by opening a browser and navigating to `http://localhost`.
   - **Stop and Restart Nginx**:
     ```bash
     sudo systemctl stop nginx
     sudo systemctl restart nginx
     ```

4. **Uninstall the Service** (optional)
   - **Uninstall Nginx**:
     ```bash
     sudo apt remove nginx
     ```

---

#### **Part 5: Creating and Managing a Custom Service (10-15 minutes)**

1. **Create a Simple Script**
   - Create a script in `/usr/local/bin` that will serve as the basis for your custom service:
     ```bash
     sudo nano /usr/local/bin/hello_service.sh
     ```
   - Add the following content:
     ```bash
     #!/bin/bash
     while true; do
       echo "Hello from the custom service!" >> /var/log/hello_service.log
       sleep 10
     done
     ```
   - **Make the script executable**:
     ```bash
     sudo chmod +x /usr/local/bin/hello_service.sh
     ```

2. **Create a Service Unit File**
   - **Create the unit file**:
     ```bash
     sudo nano /etc/systemd/system/hello.service
     ```
   - Add the following content:
     ```ini
     [Unit]
     Description=Hello Service
     After=network.target

     [Service]
     ExecStart=/usr/local/bin/hello_service.sh
     Restart=always

     [Install]
     WantedBy=multi-user.target
     ```

3. **Start and Enable the Custom Service**
   - **Reload systemd** to recognize the new service:
     ```bash
     sudo systemctl daemon-reload
     ```
   - **Start the custom service**:
     ```bash
     sudo systemctl start hello.service
     ```
   - **Enable the service on boot**:
     ```bash
     sudo systemctl enable hello.service
     ```

4. **Monitor and Manage the Custom Service**
   - **Check the service status**:
     ```bash
     systemctl status hello.service
     ```
   - **View the output** written to the log:
     ```bash
     tail -f /var/log/hello_service.log
     ```
   - **Stop the service**:
     ```bash
     sudo systemctl stop hello.service
     ```
   - **Disable the service**:
     ```bash
     sudo systemctl disable hello.service
     ```

5. **Remove the Custom Service** (optional)
   - **Delete the unit file**:
     ```bash
     sudo rm /etc/systemd/system/hello.service
     ```
   - **Reload systemd** to apply changes:
     ```bash
     sudo systemctl daemon-reload
     ```
   - **Remove the script**:
     ```bash
     sudo rm /usr/local/bin/hello_service.sh
     ```

---

