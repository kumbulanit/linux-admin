File and directory management is foundational in Linux, as it allows you to create, organize, navigate, and manipulate files and folders (directories). Here’s an overview of essential commands and tools for managing files and directories:

---

### **1. Navigating the Filesystem**

- **`pwd`** (Print Working Directory): Displays the current directory path.
  ```bash
  pwd
  ```

- **`cd`** (Change Directory): Moves between directories.
  ```bash
  cd /path/to/directory    # Navigate to a specific directory
  cd ..                    # Move up one level
  cd ~                     # Move to the home directory
  ```

- **`ls`** (List): Lists files and directories in the current or specified directory.
  ```bash
  ls                       # List in current directory
  ls -l                    # Detailed list with permissions
  ls -a                    # Include hidden files
  ls -lh                   # Detailed, human-readable format
  ```

### **2. Creating Files and Directories**

- **`touch`**: Creates a new empty file or updates the timestamp of an existing file.
  ```bash
  touch filename.txt       # Create an empty file
  ```

- **`mkdir`** (Make Directory): Creates new directories.
  ```bash
  mkdir directory_name         # Create a single directory
  mkdir -p dir1/dir2/dir3      # Create nested directories
  ```

### **3. Viewing and Editing Files**

- **`cat`**: Displays the contents of a file.
  ```bash
  cat filename.txt            # View file contents
  ```

- **`less`**: Allows you to view large files page by page (quicker with large files than `cat`).
  ```bash
  less filename.txt
  ```

- **`head`** and **`tail`**: Shows the beginning or end of a file.
  ```bash
  head -n 10 filename.txt      # Show first 10 lines
  tail -n 10 filename.txt      # Show last 10 lines
  ```

### **4. Copying, Moving, and Renaming Files and Directories**

- **`cp`** (Copy): Copies files and directories.
  ```bash
  cp file1.txt file2.txt                # Copy file1.txt to file2.txt
  cp -r /source/directory /destination  # Copy a directory recursively
  ```

- **`mv`** (Move): Moves or renames files and directories.
  ```bash
  mv oldname.txt newname.txt            # Rename a file
  mv file.txt /new/location             # Move file to a different directory
  ```

### **5. Deleting Files and Directories**

- **`rm`** (Remove): Deletes files and directories.
  ```bash
  rm filename.txt                       # Delete a file
  rm -r directory_name                  # Delete a directory and its contents
  rm -i filename.txt                    # Prompt before deletion
  rm -rf directory_name                 # Force delete without prompt
  ```

### **6. File Permissions and Ownership**

Files and directories in Linux have three types of permissions: **read (r)**, **write (w)**, and **execute (x)**, which apply to three categories of users: **owner**, **group**, and **others**.

- **`chmod`** (Change Mode): Changes file and directory permissions.
  ```bash
  chmod 755 filename                # Set permissions using numeric format
  chmod u+x filename                # Add execute permission for the user (owner)
  ```

- **`chown`** (Change Owner): Changes the owner and group of a file or directory.
  ```bash
  sudo chown user:group filename    # Change owner and group
  sudo chown -R user:group directory_name  # Change ownership recursively
  ```

### **7. Searching for Files and Directories**

- **`find`**: Searches for files and directories within a specified path.
  ```bash
  find /path -name "filename.txt"    # Search by name
  find /path -type d -name "folder*" # Search for directories with 'folder' in name
  ```

- **`locate`**: Quickly finds files by name (based on an updated database).
  ```bash
  locate filename
  ```

- **`grep`**: Searches for text within files.
  ```bash
  grep "text_to_find" filename.txt   # Search for specific text within a file
  grep -r "text" /path/to/directory  # Search recursively within directory
  ```

### **8. File Links**

- **Hard Links**: Create an additional name for an existing file, both pointing to the same data blocks.
  ```bash
  ln file1.txt file2.txt             # Create a hard link
  ```

- **Symbolic (Soft) Links**: Like shortcuts, pointing to the file path rather than data blocks.
  ```bash
  ln -s /path/to/original file_link  # Create a symbolic link
  ```

### **9. Archiving and Compressing Files**

- **`tar`**: Archives files and directories. Common flags include `-c` (create), `-x` (extract), and `-z` (gzip compression).
  ```bash
  tar -czvf archive.tar.gz /path/to/directory    # Compress directory into tar.gz
  tar -xzvf archive.tar.gz                       # Extract tar.gz archive
  ```

- **gzip**: Compresses individual files.
  ```bash
  gzip filename                                 # Compress file
  gunzip filename.gz                            # Decompress file
  ```

- **zip/unzip**: Compresses and decompresses files into ZIP format.
  ```bash
  zip archive.zip file1 file2                   # Create a ZIP archive
  unzip archive.zip                             # Extract ZIP archive
  ```

### **10. Disk Usage and File Sizes**

- **`df`**: Displays information about disk space usage for file systems.
  ```bash
  df -h                                       # Human-readable format
  ```

- **`du`**: Shows disk usage for files and directories.
  ```bash
  du -h /path/to/directory                     # Disk usage for a directory
  du -sh /path/to/directory                    # Summary of a directory
  ```

### **11. Viewing and Manipulating File Content**

- **`cat`**: Displays the file content.
- **`tac`**: Displays file content in reverse order.
- **`sort`**: Sorts the content of a file.
  ```bash
  sort filename.txt                            # Sort lines alphabetically
  ```

- **`uniq`**: Removes duplicate lines (often combined with `sort`).
  ```bash
  sort filename.txt | uniq                     # Remove duplicates
  ```

- **`wc`**: Counts lines, words, and characters in a file.
  ```bash
  wc -l filename.txt                           # Count lines
  ```

---
