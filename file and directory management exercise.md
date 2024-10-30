

### **Exercise: Mastering File and Directory Management**

#### **Objective:**
Learn essential commands for navigating, creating, modifying, and managing files and directories. This exercise starts simple and gradually gets more advanced.

---

### **Instructions:**

Complete each task in the Bash shell, record the commands used, and verify the output. Each task builds upon skills needed for effective file and directory management.

---

### **Tasks:**

---

#### **1. Basic Navigation and Directory Management (5 minutes)**

   - **Task 1**: Create a new directory called `project` in your home directory.
     *Hint*: Use `mkdir`.
   
   - **Task 2**: Change into the `project` directory and create three subdirectories: `src`, `data`, and `docs`.
     *Hint*: Use `mkdir` for each subdirectory or create them all at once.

   - **Task 3**: Within the `docs` directory, create an empty file called `README.md`.
     *Hint*: Use `touch` to create an empty file.

   - **Task 4**: Display the full path to your current working directory.
     *Hint*: Use `pwd` to display the path.

---

#### **2. File Creation and Basic Manipulation (5 minutes)**

   - **Task 1**: Inside the `src` directory, create a file called `main.py` and add the text `# Main Python Script` to the file.
     *Hint*: Use `echo` to add text to the file.

   - **Task 2**: Create a copy of `main.py` in the `src` directory and name it `backup_main.py`.
     *Hint*: Use `cp` to copy files.

   - **Task 3**: Move the file `backup_main.py` to the `data` directory.
     *Hint*: Use `mv` to move files.

   - **Task 4**: Rename the file `README.md` in the `docs` directory to `ProjectOverview.md`.
     *Hint*: Use `mv` to rename a file.

---

#### **3. Viewing and Searching Content (5 minutes)**

   - **Task 1**: Display the contents of `main.py` on the terminal.
     *Hint*: Use `cat` to view file contents.

   - **Task 2**: Add three lines to the `ProjectOverview.md` file in `docs`, such as:
      ```
      # Project Documentation
      ## Overview
      - This project is designed to... (add some text)
      ```
      *Hint*: Use `echo` with `>>` to append text to the file.

   - **Task 3**: Display the first line of `ProjectOverview.md`.
     *Hint*: Use `head` with line option.

   - **Task 4**: Search for the word "Project" within `ProjectOverview.md`.
     *Hint*: Use `grep` to search for text in a file.

---

#### **4. Advanced Directory Management and Cleanup (5 minutes)**

   - **Task 1**: Find all files within the `project` directory and its subdirectories, and display their paths.
     *Hint*: Use `find` to list files recursively.

   - **Task 2**: List all files in the `project` directory and its subdirectories, sorted by modification time.
     *Hint*: Use `ls` with sorting options.

   - **Task 3**: Set read, write, and execute permissions for the owner on the `src` directory, but only read and execute permissions for others.
     *Hint*: Use `chmod` with numeric mode.

   - **Task 4**: Delete the `data` directory and all its contents.
     *Hint*: Use `rm -r` to delete directories recursively.

---

### **Answers Guide:**

Here's a guide for checking your work after completing each task.

---

#### **1. Basic Navigation and Directory Management**
   - **Answer 1**: `mkdir ~/project`
   - **Answer 2**: `mkdir ~/project/src ~/project/data ~/project/docs`
   - **Answer 3**: `touch ~/project/docs/README.md`
   - **Answer 4**: `pwd`

#### **2. File Creation and Basic Manipulation**
   - **Answer 1**: `echo "# Main Python Script" > ~/project/src/main.py`
   - **Answer 2**: `cp ~/project/src/main.py ~/project/src/backup_main.py`
   - **Answer 3**: `mv ~/project/src/backup_main.py ~/project/data/`
   - **Answer 4**: `mv ~/project/docs/README.md ~/project/docs/ProjectOverview.md`

#### **3. Viewing and Searching Content**
   - **Answer 1**: `cat ~/project/src/main.py`
   - **Answer 2**:
     ```bash
     echo "# Project Documentation" >> ~/project/docs/ProjectOverview.md
     echo "## Overview" >> ~/project/docs/ProjectOverview.md
     echo "- This project is designed to..." >> ~/project/docs/ProjectOverview.md
     ```
   - **Answer 3**: `head -n 1 ~/project/docs/ProjectOverview.md`
   - **Answer 4**: `grep "Project" ~/project/docs/ProjectOverview.md`

#### **4. Advanced Directory Management and Cleanup**
   - **Answer 1**: `find ~/project -type f`
   - **Answer 2**: `ls -ltR ~/project`
   - **Answer 3**: `chmod 755 ~/project/src`
   - **Answer 4**: `rm -r ~/project/data`

---
