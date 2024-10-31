

### **Part 1: Checking and Identifying Available Storage Devices (5 minutes)**

1. **List All Devices**:
   ```bash
   sudo lsblk
   ```
   - This command lists devices and partitions, showing available storage devices and mount points.
   
2. **Identify Additional Disks and Partition Information**:
   ```bash
   sudo fdisk -l
   ```
   - Note any new, unmounted disk (`/dev/sdb`).

---

### **Part 2: Creating and Managing Partitions (10 minutes)**

1. **Partition the Disk Using `fdisk`**:
   ```bash
   sudo fdisk /dev/sdb
   ```
   - Create primary or extended partitions depending on requirements.
   - Use `n` to create, `w` to write changes.

2. **Format Partitions with Different Filesystems**:
   - Format `/dev/sdb1` with `ext4`.
     ```bash
     sudo mkfs.ext4 /dev/sdb1
     ```
   - Optionally, try different filesystems for learning:
     - `xfs`: `sudo mkfs.xfs /dev/sdb2`
     - `btrfs`: `sudo mkfs.btrfs /dev/sdb3`

---

### **Part 3: Configuring RAID (if possible) (10 minutes)**

*If the system has multiple disks, RAID configuration can be introduced for redundancy and performance.*

1. **Install RAID Utilities**:
   ```bash
   sudo apt update && sudo apt install mdadm -y
   ```

2. **Create a RAID 1 Array** (mirroring) with two disks, e.g., `/dev/sdb` and `/dev/sdc`:
   ```bash
   sudo mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sdb /dev/sdc
   ```

3. **Format and Mount the RAID Array**:
   ```bash
   sudo mkfs.ext4 /dev/md0
   sudo mkdir /mnt/raid_storage
   sudo mount /dev/md0 /mnt/raid_storage
   ```
   - **Persist Mount**:
     ```bash
     echo '/dev/md0 /mnt/raid_storage ext4 defaults 0 2' | sudo tee -a /etc/fstab
     ```

---

### **Part 4: Enabling Filesystem Quotas (5 minutes)**

1. **Install Quota Package**:
   ```bash
   sudo apt install quota -y
   ```

2. **Enable Quotas on `/mnt/data`**:
   - Edit `/etc/fstab` and add `usrquota,grpquota` options to the mount:
     ```bash
     sudo nano /etc/fstab
     ```
     - Update line:
       ```
       /dev/sdb1 /mnt/data ext4 defaults,usrquota,grpquota 0 2
       ```
   - Remount to apply:
     ```bash
     sudo mount -o remount /mnt/data
     ```

3. **Initialize Quotas and Set a Test Quota**:
   ```bash
   sudo quotacheck -cum /mnt/data
   sudo quotaon /mnt/data
   sudo edquota -u username
   ```
   - Set a soft/hard limit for disk usage.

---

### **Part 5: Implementing Storage Encryption (10 minutes)**

1. **Install Encryption Tools**:
   ```bash
   sudo apt install cryptsetup -y
   ```

2. **Encrypt a Partition**:
   - Initialize encryption for `/dev/sdb1`.
     ```bash
     sudo cryptsetup luksFormat /dev/sdb1
     ```
   - Open and map the device:
     ```bash
     sudo cryptsetup luksOpen /dev/sdb1 encrypted_data
     ```
   - Format and mount the encrypted partition:
     ```bash
     sudo mkfs.ext4 /dev/mapper/encrypted_data
     sudo mkdir /mnt/encrypted_data
     sudo mount /dev/mapper/encrypted_data /mnt/encrypted_data
     ```

3. **Close the Encrypted Partition**:
   ```bash
   sudo umount /mnt/encrypted_data
   sudo cryptsetup luksClose encrypted_data
   ```

---

### **Part 6: Backup and Restore (5 minutes)**

1. **Create a Backup with `rsync`**:
   - Back up `/mnt/data` to `/mnt/backup`.
     ```bash
     sudo mkdir /mnt/backup
     sudo rsync -av /mnt/data/ /mnt/backup/
     ```

2. **Restore Backup**:
   - Restore files back to `/mnt/data`:
     ```bash
     sudo rsync -av /mnt/backup/ /mnt/data/
     ```

---

### **Part 7: Extending a Partition and Resizing Filesystems (5 minutes)**

1. **Extend Logical Volume (LVM)**:
   - If LVM is used and there's free space, extend it:
     ```bash
     sudo lvextend -L +500M /dev/vg_data/lv_storage
     sudo resize2fs /dev/vg_data/lv_storage
     ```

2. **Shrink (Optional)**:
   - Ensure the filesystem is not mounted and resize to a smaller size:
     ```bash
     sudo umount /dev/vg_data/lv_storage
     sudo resize2fs /dev/vg_data/lv_storage 400M
     sudo lvreduce -L 400M /dev/vg_data/lv_storage
     ```

---

### **Part 8: Storage Monitoring and Analysis (5 minutes)**

1. **Monitor Disk Usage with `df` and `du`**:
   - Check disk space:
     ```bash
     df -h
     ```
   - Check specific directory usage:
     ```bash
     du -sh /mnt/data
     ```

2. **Use `iotop` to Monitor Disk I/O**:
   ```bash
   sudo apt install iotop -y
   sudo iotop
   ```

3. **Enable Disk Usage Alerts (Optional)**:
   - Use a simple script to alert if disk usage exceeds a threshold (e.g., 90%).

   ```bash
   df -H | grep -vE '^Filesystem|tmpfs|cdrom' | awk '{ print $5 " " $1 }' | while read output;
   do
     usep=$(echo $output | awk '{ print $1}' | sed 's/%//g')
     partition=$(echo $output | awk '{ print $2 }')
     if [ $usep -ge 90 ]; then
       echo "Running out of space \"$partition ($usep%)\" on $(hostname) as on $(date)" >> /var/log/disk_alert.log
     fi
   done
   ```

---
