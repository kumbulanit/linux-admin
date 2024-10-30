

### **Exercise: System Management with Bash Shell**

#### **Objective:**
Gain hands-on experience in managing system processes, monitoring system performance, and managing users through essential Bash commands.

---

### **Instructions:**

Complete each task in the Bash shell. Record the commands you used and their results as you proceed.

---

### **Tasks:**

#### **1. Process Management (5 minutes)**  
   - **Question 1**: List all running processes.  
   - **Question 2**: Filter the list of processes to only show those related to the Bash shell.  
   - **Question 3**: Identify the process ID (PID) of a process that interests you (e.g., "bash" or "sshd").
   - **Question 4**: Terminate a process safely using the `kill` command with the PID you found.
   - **Question 5**: Verify that the process has terminated by checking the process list again.

---

#### **2. System Monitoring (5 minutes)**  
   - **Question 1**: Check system uptime to see how long the system has been running.
   - **Question 2**: Display current memory usage.
   - **Question 3**: Display disk usage for each mounted filesystem.
   - **Question 4**: View CPU load information over the last 1, 5, and 15 minutes.

---

#### **3. User Management (5 minutes)**  
   - **Question 1**: List all users currently logged in.
   - **Question 2**: Display user account details for your own username.
   - **Question 3**: Create a new user (requires `sudo` permissions).
   - **Question 4**: Set a password for the new user (also requires `sudo`).
   - **Question 5**: Remove the new user after confirming its creation.

---

#### **4. File Permissions and Ownership (5 minutes)**  
   - **Question 1**: Create a new file named `practice.txt`.
   - **Question 2**: Change the ownership of the file to a different user (e.g., `root`).
   - **Question 3**: Modify the file’s permissions to make it readable and writable only by the owner.
   - **Question 4**: Verify the permissions and ownership changes by listing the file details.

---

### **Answers Guide**:

Below is an answer guide to check your responses after completing the tasks. Try not to look at it until you finish each section!

---

#### **Process Management Answers**
   - **Answer 1**: `ps aux`
   - **Answer 2**: `ps aux | grep bash`
   - **Answer 3**: Command output will display PID in the second column.
   - **Answer 4**: `kill <PID>`
   - **Answer 5**: Run `ps aux` again to confirm.

#### **System Monitoring Answers**
   - **Answer 1**: `uptime`
   - **Answer 2**: `free -h`
   - **Answer 3**: `df -h`
   - **Answer 4**: `top` (exit by pressing `q`)

#### **User Management Answers**
   - **Answer 1**: `who`
   - **Answer 2**: `id <your_username>`
   - **Answer 3**: `sudo useradd testuser`
   - **Answer 4**: `sudo passwd testuser`
   - **Answer 5**: `sudo userdel testuser`

#### **File Permissions and Ownership Answers**
   - **Answer 1**: `touch practice.txt`
   - **Answer 2**: `sudo chown root practice.txt`
   - **Answer 3**: `chmod 600 practice.txt`
   - **Answer 4**: `ls -l practice.txt`

--- 
