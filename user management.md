

#### **Task 1: Create Users and Groups (5 mins)**

1. **Create two groups**:
   - `devteam`: for developers with read-only access to certain directories.
   - `sysadmin`: for system administrators with broader permissions.
   
   ```bash
   sudo groupadd devteam
   sudo groupadd sysadmin
   ```

2. **Create three users**:
   - `alice` and `bob`, assigned to `devteam`.
   - `carol`, assigned to `sysadmin`.
   
   ```bash
   sudo useradd -m -G devteam alice
   sudo useradd -m -G devteam bob
   sudo useradd -m -G sysadmin carol
   ```

3. **Set passwords** for each user:
   ```bash
   sudo passwd alice
   sudo passwd bob
   sudo passwd carol
   ```

---

#### **Task 2: Configure Basic File Permissions (5 mins)**

1. **Create a shared directory** for collaboration:
   ```bash
   sudo mkdir /shared
   ```

2. **Set ownership** of `/shared` to `root` and assign the group to `devteam`:
   ```bash
   sudo chown root:devteam /shared
   ```

3. **Assign group permissions** to allow `devteam` members to read and execute files within `/shared`:
   ```bash
   sudo chmod 750 /shared
   ```

---

#### **Task 3: Advanced Permissions with ACLs (10 mins)**

1. **Give `sysadmin` group full access** to `/shared` using ACL:
   ```bash
   sudo setfacl -m g:sysadmin:rwx /shared
   ```

2. **Verify ACL settings** to ensure `sysadmin` has full access and `devteam` has only read and execute:
   ```bash
   getfacl /shared
   ```

3. **Add an ACL entry for `bob`** to provide read-only access to a specific file (create a test file if needed):
   ```bash
   sudo touch /shared/testfile.txt
   sudo setfacl -m u:bob:r /shared/testfile.txt
   ```

4. **Verify the ACL for `testfile.txt`** to ensure `bob` has read-only access:
   ```bash
   getfacl /shared/testfile.txt
   ```

---

#### **Task 4: Apply the Sticky Bit for Security (5 mins)**

1. **Apply the sticky bit** on `/shared` to prevent users from deleting files they don’t own:
   ```bash
   sudo chmod +t /shared
   ```

2. **Test the sticky bit functionality**:
   - As user `alice`, create a file in `/shared` (e.g., `alicefile.txt`).
     ```bash
     sudo -u alice touch /shared/alicefile.txt
     ```
   - Attempt to delete `alicefile.txt` as `bob` to verify that the sticky bit prevents deletion by non-owners.

---

#### **Task 5: Set Special Permissions for System Admins (5 mins)**

1. **Open the sudoers file** to give the `sysadmin` group sudo privileges:
   ```bash
   sudo visudo
   ```
   
2. **Add an entry** to allow members of the `sysadmin` group full sudo access:
   ```bash
   %sysadmin ALL=(ALL:ALL) ALL
   ```

3. **Test sudo access** by switching to `carol` (a `sysadmin` group member) and trying a command with `sudo`:
   ```bash
   sudo -u carol sudo ls /root
   ```

---
Here are additional exercises to build on user and permission management. These should add further complexity to permissions, ACLs, and user roles while deepening understanding of security management in Linux.

---



#### **Task 6: Create a New Developer Directory with Restricted Access (5 mins)**

1. **Create a directory** named `/devdocs` where only members of `devteam` can read, write, and execute files, but without modifying the parent folder.
   ```bash
   sudo mkdir /devdocs
   sudo chown :devteam /devdocs
   sudo chmod 770 /devdocs
   ```

2. **Test directory access**:
   - Log in as `alice` and create a file inside `/devdocs` to verify access.
   - Try to list or access the directory as `bob` (if `bob` is not in `devteam`, the command should fail).

---

#### **Task 7: Add User-Specific ACL Entries (5 mins)**

1. **Create a file** in `/devdocs` called `project_plan.txt`.
   ```bash
   sudo touch /devdocs/project_plan.txt
   ```

2. **Assign an ACL** to allow `bob` read and write access to this file only:
   ```bash
   sudo setfacl -m u:bob:rw /devdocs/project_plan.txt
   ```

3. **Verify ACL settings** to ensure only `bob` has this specific permission:
   ```bash
   getfacl /devdocs/project_plan.txt
   ```

4. **Test permissions**:
   - Try to edit `project_plan.txt` as `bob` and as `alice`.
   - `bob` should have access to edit, while `alice` should not (unless further permissions are set).

---

#### **Task 8: Use `umask` to Set Default Permissions (5 mins)**

1. **Set the `umask`** value to `027` for all users in the `devteam` group, ensuring new files created by these users have restricted permissions by default. This can be added in the `/etc/profile` or `/home/<username>/.bashrc` file for each user:
   ```bash
   echo "umask 027" | sudo tee -a /home/alice/.bashrc /home/bob/.bashrc
   ```

2. **Verify the default permissions**:
   - Log in as `alice` and create a new file in `/devdocs`.
   - Check the file’s permissions to ensure it matches the `umask` settings.

---

#### **Task 9: Configure `sudo` Access with Limited Permissions for Developers (5 mins)**

1. **Edit the sudoers file** to allow `devteam` members to run specific commands, such as restarting a service, but restrict other sudo commands:
   ```bash
   sudo visudo
   ```

2. **Add a rule** to allow `devteam` members to restart Apache (replace with any common service name on your VM):
   ```bash
   %devteam ALL=(ALL) /bin/systemctl restart apache2
   ```

3. **Test sudo access**:
   - Try to restart Apache as `alice` with `sudo systemctl restart apache2`.
   - Confirm that `alice` cannot perform other commands like `sudo su`.

---

#### **Task 10: Explore the `sticky bit` on User-Created Directories (5 mins)**

1. **Create a new directory** `/teamwork` where all users can create files but only the owners of those files can delete them:
   ```bash
   sudo mkdir /teamwork
   sudo chmod 1777 /teamwork
   ```

2. **Test the sticky bit**:
   - As `bob`, create a file called `bobfile.txt` in `/teamwork`.
   - Log in as `alice` and attempt to delete `bobfile.txt` (the operation should be blocked).

---

#### **Task 11: Advanced Group Permissions and Directory Ownership (5 mins)**

1. **Create a shared project directory** for both `devteam` and `sysadmin` groups, with `sysadmin` as the group owner and full permissions to read, write, and execute:
   ```bash
   sudo mkdir /project_shared
   sudo chown root:sysadmin /project_shared
   sudo chmod 2770 /project_shared
   ```

2. **Assign `devteam` read-only permissions** using ACLs:
   ```bash
   sudo setfacl -m g:devteam:rx /project_shared
   ```

3. **Verify permissions**:
   - As `alice`, attempt to create a file in `/project_shared` (it should fail).
   - As `carol`, try to create and delete files in `/project_shared` (it should succeed).

---

### **Review and Self-Check Questions**

1. **Describe the difference** between standard permissions and ACLs. Why might ACLs be more advantageous?
2. **Explain how `umask` settings** help manage security on a multi-user system.
3. **Discuss the purpose of the sticky bit** in shared directories.
4. **Reflect on the difference between sudoers group access and limited sudo permissions** for groups like `devteam`.

