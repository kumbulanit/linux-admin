---

# **Practical Exercise: Storage Configuration Showcase**

## **Objective**
To configure a storage system by performing disk partitioning, setting up file systems, managing storage using LVM, and implementing RAID. By the end of this exercise, participants will understand how to manage and optimize storage configurations effectively.

## **Prerequisites**
- A Linux environment (physical or virtual machine).
- Basic knowledge of Linux command-line operations.
- Sudo privileges to perform administrative tasks.

## **Exercise Overview**
Participants will complete the following tasks:
1. **Disk Partitioning**
2. **File System Setup**
3. **Logical Volume Management (LVM) Configuration**
4. **RAID Setup**
5. **NFS Configuration for Remote Access**

---

### **Part 1: Disk Partitioning**

1. **Identify Available Disks**
   - Use the command `lsblk` to list all block devices and identify a disk that can be used for this exercise (e.g., `/dev/sdb`).

2. **Partition the Disk**
   - Use `fdisk` or `parted` to create two partitions on the selected disk.
     - Create a primary partition (`/dev/sdb1`) and an extended partition (`/dev/sdb2`).
   - Example command for `fdisk`:
     ```bash
     sudo fdisk /dev/sdb
     ```

3. **Verify the Partitions**
   - Run `lsblk` again to ensure the partitions were created successfully.

---

### **Part 2: File System Setup**

1. **Create File Systems**
   - Format the partitions with the EXT4 file system using the `mkfs` command:
     ```bash
     sudo mkfs.ext4 /dev/sdb1
     sudo mkfs.ext4 /dev/sdb2
     ```

2. **Create Mount Points**
   - Create directories to mount the file systems:
     ```bash
     sudo mkdir /mnt/part1
     sudo mkdir /mnt/part2
     ```

3. **Mount the Partitions**
   - Mount the partitions to the created directories:
     ```bash
     sudo mount /dev/sdb1 /mnt/part1
     sudo mount /dev/sdb2 /mnt/part2
     ```

4. **Verify the Mounts**
   - Check the mounted partitions using `df -h`.

---

### **Part 3: Logical Volume Management (LVM) Configuration**

1. **Convert Physical Partitions to LVM**
   - Install LVM tools if not already installed:
     ```bash
     sudo apt install lvm2
     ```
   - Create physical volumes (PVs):
     ```bash
     sudo pvcreate /dev/sdb1 /dev/sdb2
     ```

2. **Create a Volume Group (VG)**
   - Create a volume group named `vg_storage`:
     ```bash
     sudo vgcreate vg_storage /dev/sdb1 /dev/sdb2
     ```

3. **Create Logical Volumes (LV)**
   - Create two logical volumes, `lv_data1` and `lv_data2`:
     ```bash
     sudo lvcreate -n lv_data1 -L 5G vg_storage
     sudo lvcreate -n lv_data2 -L 5G vg_storage
     ```

4. **Format and Mount Logical Volumes**
   - Format the logical volumes with EXT4 and mount them:
     ```bash
     sudo mkfs.ext4 /dev/vg_storage/lv_data1
     sudo mkfs.ext4 /dev/vg_storage/lv_data2
     sudo mkdir /mnt/lv_data1
     sudo mkdir /mnt/lv_data2
     sudo mount /dev/vg_storage/lv_data1 /mnt/lv_data1
     sudo mount /dev/vg_storage/lv_data2 /mnt/lv_data2
     ```

---

### **Part 4: RAID Setup**

1. **Install mdadm for RAID Management**
   - Install `mdadm` if not already installed:
     ```bash
     sudo apt install mdadm
     ```

2. **Create a RAID Array**
   - Create a RAID 1 array using two disks (make sure to use available disks, e.g., `/dev/sdc` and `/dev/sdd`):
     ```bash
     sudo mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sdc /dev/sdd
     ```

3. **Format and Mount RAID Array**
   - Format the RAID array with EXT4 and create a mount point:
     ```bash
     sudo mkfs.ext4 /dev/md0
     sudo mkdir /mnt/raid0
     sudo mount /dev/md0 /mnt/raid0
     ```

4. **Verify the RAID Array**
   - Check the status of the RAID array:
     ```bash
     cat /proc/mdstat
     sudo mdadm --detail /dev/md0
     ```

---

### **Part 5: NFS Configuration for Remote Access**

1. **Install NFS Server**
   - Install the NFS server package:
     ```bash
     sudo apt install nfs-kernel-server
     ```

2. **Configure NFS Exports**
   - Edit the `/etc/exports` file to share the `/mnt/lv_data1` directory:
     ```bash
     echo "/mnt/lv_data1 *(rw,sync,no_subtree_check)" | sudo tee -a /etc/exports
     ```

3. **Start NFS Service**
   - Export the shares and start the NFS service:
     ```bash
     sudo exportfs -a
     sudo systemctl restart nfs-kernel-server
     ```

4. **Test NFS from a Client Machine**
   - On a different Linux machine, mount the NFS share:
     ```bash
     sudo mount <server_ip>:/mnt/lv_data1 /mnt/nfs
     ```

5. **Verify Access**
   - Navigate to `/mnt/nfs` to ensure you can access the shared files.

---

