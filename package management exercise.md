

### **1. Basics: Updating and Searching Packages (5 mins)**

1. **Update Package Information**
   - Run the command to update the package list:
     ```bash
     sudo apt update
     ```

2. **Search for a Package**
   - Search for a package by name (e.g., `curl`):
     ```bash
     apt search curl
     ```

3. **View Package Details**
   - Display detailed information about the `curl` package:
     ```bash
     apt show curl
     ```

---

### **2. Installing and Removing Packages (10 mins)**

1. **Install a Simple Package**
   - Install the `curl` package:
     ```bash
     sudo apt install curl
     ```

2. **Install a Package with Dependencies**
   - Install `vim` with dependencies and observe the output:
     ```bash
     sudo apt install vim
     ```

3. **Remove a Package**
   - Remove `vim` but keep configuration files:
     ```bash
     sudo apt remove vim
     ```

4. **Purge a Package**
   - Completely remove `vim`, including configuration files:
     ```bash
     sudo apt purge vim
     ```

---

### **3. Handling Dependencies and Updates (10 mins)**

1. **Install a Package with Missing Dependencies**
   - Try installing the `libboost-all-dev` package (this may require additional libraries and dependencies).
     ```bash
     sudo apt install libboost-all-dev
     ```

2. **Check for Broken Dependencies**
   - Verify if any dependencies are broken:
     ```bash
     sudo apt --fix-broken install
     ```

3. **Update a Package**
   - If `curl` or another package has an update, install it:
     ```bash
     sudo apt upgrade curl
     ```

4. **Full System Upgrade**
   - Perform a full upgrade of all packages on the system:
     ```bash
     sudo apt full-upgrade
     ```

---

### **4. Advanced Package Management: Repositories and Manual Installation (10 mins)**

1. **Add a New Repository**
   - Add the Google Chrome repository to install Chrome (replace `<distro>` with your Ubuntu version, e.g., `stable`):
     ```bash
     echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" | sudo tee /etc/apt/sources.list.d/google-chrome.list
     ```

   - Add the Google signing key:
     ```bash
     wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
     ```

   - Update the package list and install `google-chrome-stable`:
     ```bash
     sudo apt update
     sudo apt install google-chrome-stable
     ```

2. **Remove a Repository**
   - Remove the Google Chrome repository:
     ```bash
     sudo rm /etc/apt/sources.list.d/google-chrome.list
     ```

---

### **5. Working with dpkg for Manual Package Management (5 mins)**

1. **Download a Package**
   - Download the `.deb` file for Slack (example package):
     ```bash
     wget https://downloads.slack-edge.com/linux_releases/slack-desktop-4.25.1-amd64.deb
     ```

2. **Install the Package Manually with dpkg**
   - Install the downloaded package with `dpkg`:
     ```bash
     sudo dpkg -i slack-desktop-4.25.1-amd64.deb
     ```

3. **Resolve Dependencies with APT**
   - Resolve any missing dependencies:
     ```bash
     sudo apt --fix-broken install
     ```

4. **Remove the Manually Installed Package**
   - Remove Slack using `dpkg`:
     ```bash
     sudo dpkg -r slack-desktop
     ```

---

### **6. Troubleshooting: Common Package Management Issues (5 mins)**

1. **Check for Held Packages**
   - View held packages (packages that cannot be upgraded):
     ```bash
     apt-mark showhold
     ```

2. **Unhold a Package**
   - Remove a hold from the `curl` package (if it was held):
     ```bash
     sudo apt-mark unhold curl
     ```

3. **Clean Up Unused Packages**
   - Remove unused packages and dependencies:
     ```bash
     sudo apt autoremove
     ```

4. **Clear Local Package Cache**
   - Free up disk space by clearing cached packages:
     ```bash
     sudo apt clean
     ```

---

### **Reflection and Review (5 mins)**

Take a few minutes to answer the following questions:
- Which command would you use to install a package and resolve any missing dependencies?
- How would you add and remove a repository on your system?
- How can you view held packages, and why might a package be held?

---
