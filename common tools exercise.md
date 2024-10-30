---

### **Exercise: Exploring Common Tools in Linux**

#### **Objective:**
Practice using essential Linux tools for text processing, system monitoring, and command-line efficiency. This exercise will require you to think critically about how different tools work together to solve problems.

---

### **Instructions:**

Complete each task in the Bash shell. Write down the commands you used and the resulting output for each task.

---

### **Tasks:**

#### **1. Text Processing (10 minutes)**

   - **Question 1**: You have a file called `names.txt` with a list of names, one per line. Display the names in alphabetical order.  
   *Hint*: Try using `sort`.

   - **Question 2**: Count the total number of unique names in `names.txt`.  
   *Hint*: Combine `sort` with `uniq` and `wc`.

   - **Question 3**: Extract only names that contain the letter "a" (case insensitive) from `names.txt` and save them to a new file called `names_with_a.txt`.  
   *Hint*: Use `grep` with a regular expression.

   - **Question 4**: In `names.txt`, some names are duplicated. Remove duplicates, keep only unique names, and save the output to a new file called `unique_names.txt`.  
   *Hint*: Try using `sort` and `uniq`.

---

#### **2. System Monitoring and Analysis (5 minutes)**

   - **Question 1**: Check the current CPU usage percentage of your system in real-time.  
   *Hint*: Use `top` or `htop` (if available) or try `mpstat` (part of `sysstat` package).

   - **Question 2**: Display the top 5 processes that are consuming the most memory on your system.  
   *Hint*: Use `ps` with sorting options and `head`.

   - **Question 3**: Find the total amount of free memory on your system in megabytes (MB).  
   *Hint*: Use `free` with the appropriate option.

---

#### **3. Advanced Command-Line Techniques (5 minutes)**

   - **Question 1**: Find all files in the `/var/log` directory that have been modified in the last 7 days. Display their names along with the modification date and time.  
   *Hint*: Use `find` with the `-mtime` and `-exec` options.

   - **Question 2**: There is a file called `access.log` in the `/var/log` directory. Extract only the lines that contain HTTP 404 errors and save them to a new file named `errors_404.log`.  
   *Hint*: Use `grep` with a pattern match.

   - **Question 3**: You have a file called `data.csv` that contains a large dataset. Extract and display the 10th field from each line, assuming fields are comma-separated.  
   *Hint*: Use `cut` with field options.

   - **Question 4**: Display the total disk space usage of files in the `/home` directory, sorted from largest to smallest.  
   *Hint*: Use `du` with sorting options.

---

### **Answers Guide**:

Here’s a guide to help you verify your answers after completing each task.

---

#### **Text Processing Answers**
   - **Answer 1**: `sort names.txt`
   - **Answer 2**: `sort names.txt | uniq | wc -l`
   - **Answer 3**: `grep -i "a" names.txt > names_with_a.txt`
   - **Answer 4**: `sort names.txt | uniq > unique_names.txt`

#### **System Monitoring and Analysis Answers**
   - **Answer 1**: `top` or `htop` (or `mpstat`)
   - **Answer 2**: `ps aux --sort=-%mem | head -n 5`
   - **Answer 3**: `free -m | grep "Mem:" | awk '{print $4}'`

#### **Advanced Command-Line Techniques Answers**
   - **Answer 1**: `find /var/log -mtime -7 -exec ls -l {} \;`
   - **Answer 2**: `grep "404" /var/log/access.log > errors_404.log`
   - **Answer 3**: `cut -d ',' -f 10 data.csv`
   - **Answer 4**: `du -h /home | sort -hr`

--- 

After completing each section, check your results with the answer guide. This exercise will help solidify your command-line skills and introduce you to more advanced Linux tools and techniques.  