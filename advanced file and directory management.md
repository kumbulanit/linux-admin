

### **Exercise: Advanced File and Directory Management in Linux**

**Objective:** To assess your ability to manage files and directories efficiently in a Linux environment. Complete each task in the given order, documenting your commands and outputs.

---

#### **Part 1: Basic File and Directory Operations (10 Minutes)**

1. **Creating Directories:**
   - Create a directory named `Project` in your home directory.
   - Inside `Project`, create three subdirectories named `src`, `bin`, and `docs`.

   **Command:**
   ```bash
   mkdir -p ~/Project/{src,bin,docs}
   ```

2. **Creating and Modifying Files:**
   - Inside the `src` directory, create a text file named `readme.txt` and add the following content: "This is the project source directory."

   **Command:**
   ```bash
   echo "This is the project source directory." > ~/Project/src/readme.txt
   ```

3. **Listing Files:**
   - List the contents of the `Project` directory in a detailed format.

   **Command:**
   ```bash
   ls -l ~/Project
   ```

4. **Viewing File Content:**
   - Display the content of the `readme.txt` file.

   **Command:**
   ```bash
   cat ~/Project/src/readme.txt
   ```

---

#### **Part 2: Intermediate File Management (10 Minutes)**

5. **Copying and Moving Files:**
   - Copy the `readme.txt` file from the `src` directory to the `docs` directory.
   - Rename the copied `readme.txt` file in the `docs` directory to `info.txt`.

   **Commands:**
   ```bash
   cp ~/Project/src/readme.txt ~/Project/docs/
   mv ~/Project/docs/readme.txt ~/Project/docs/info.txt
   ```

6. **Creating a Directory with File:**
   - Create a directory named `old_files` within the `Project` directory and move the `info.txt` file into `old_files`.

   **Command:**
   ```bash
   mkdir ~/Project/old_files
   mv ~/Project/docs/info.txt ~/Project/old_files/
   ```

7. **Creating Multiple Files:**
   - Inside the `src` directory, create three text files: `data1.txt`, `data2.txt`, and `data3.txt`, each containing different sample text.

   **Command:**
   ```bash
   echo "Sample data 1" > ~/Project/src/data1.txt
   echo "Sample data 2" > ~/Project/src/data2.txt
   echo "Sample data 3" > ~/Project/src/data3.txt
   ```

---

#### **Part 3: Advanced File and Directory Management (10 Minutes)**

8. **Changing Permissions:**
   - Change the permissions of the `src` directory to allow only the owner to read and write.

   **Command:**
   ```bash
   chmod 700 ~/Project/src
   ```

9. **Creating Symbolic and Hard Links:**
   - Create a symbolic link to `info.txt` in the `bin` directory.
   - Create a hard link to `info.txt` in the `src` directory.

   **Commands:**
   ```bash
   ln -s ~/Project/docs/info.txt ~/Project/bin/info_link.txt
   ln ~/Project/docs/info.txt ~/Project/src/info_hard_link.txt
   ```

10. **Finding Files:**
    - Use the `find` command to search for all text files in the `Project` directory and its subdirectories.

    **Command:**
    ```bash
    find ~/Project -name "*.txt"
    ```

---

#### **Part 4: Cleanup and Automation (10 Minutes)**

11. **Archiving and Compressing:**
    - Create a compressed archive of the `Project` directory.

    **Command:**
    ```bash
    tar -czvf ~/Project.tar.gz ~/Project
    ```

12. **Automated File Cleanup:**
    - Write a one-line command to find and delete all `.txt` files in the `Project` directory older than 7 days (for simulation, create a test file and touch it).

    **Command:**
    ```bash
    touch ~/Project/docs/test.txt
    find ~/Project -name "*.txt" -type f -mtime +7 -exec rm {} \;
    ```

13. **Batch Renaming Files:**
    - Suppose you have several text files in the `old_files` directory (e.g., `data1.txt`, `data2.txt`, `data3.txt`). Write a command using `rename` to append `_backup` to each file's name.

    **Command:**
    ```bash
    rename 's/(.*)\.txt$/$1_backup.txt/' ~/Project/old_files/*.txt
    ```

---

### **Submission Requirements:**
- Document each command you used, along with the output for each step.
- Include explanations of what each command does and why it was used.
- If applicable, provide screenshots or terminal output as evidence of completed tasks.

---

### **Grading Criteria:**
- **Completeness**: All tasks completed according to instructions.
- **Clarity**: Clear documentation of commands and their outputs.
- **Understanding**: Explanations demonstrate a solid understanding of file and directory management concepts.
- **Creativity**: Effective use of advanced techniques, especially in automation and cleanup tasks.

---
