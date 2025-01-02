# Shell Scripting & Automation | Day 6 | Complete Linux Course 

Shell scripting is an essential skill for system administrators & DevOps Professionals and is critical in automation, maintenance tasks, and configuration management. For **DevOps  Engineers** , mastering shell scripting helps in managing and automating system tasks efficiently.

---

### **1. Shell Scripting Basics**

Shell scripting enables automation by executing a series of commands saved in a script. A script file is typically created with the `.sh` extension, and the most common shell used is `bash` (`/bin/bash`).

#### **Creating and Executing Shell Scripts**
1. **Creating a Script File:**
   Use a text editor to create a shell script, such as `myscript.sh`.

   ```bash
   vi myscript.sh
   ```

2. **Add the Shebang Line:**
   The script should start with the shebang (`#!/bin/bash`), which tells the system that this script should be interpreted by `bash`.

   ```bash
   #!/bin/bash
   echo "Hello, World!"
   ```

3. **Make the Script Executable:**
   After writing the script, make it executable using:

   ```bash
   chmod +x myscript.sh
   ```

4. **Run the Script:**

   ```bash
   ./myscript.sh
   ```

---

### **2. Debugging with `set`**

The `set` command in bash is used to control the behavior of the shell script, especially useful for debugging. In an RHCE environment, using the right debugging techniques can help identify errors quickly and automate tasks efficiently.

#### **Common `set` Options for Debugging:**

1. **`set -x`**: Trace execution of commands
   - This option prints each command with its arguments as they are executed. It is useful to debug by seeing the flow of the script.

   **Example:**
   ```bash
   #!/bin/bash
   set -x  # Enable tracing
   echo "Starting process..."
   cp /home/user/file1 /home/user/backup/
   echo "Process finished."
   ```

   This will output each command executed, showing exactly what's happening in the script.

2. **`set -e`**: Exit immediately if a command fails
   - This is useful in a script where you don't want subsequent commands to run if any command fails (non-zero exit status).

   **Example:**
   ```bash
   #!/bin/bash
   set -e  # Exit if any command fails
   echo "This will be executed."
   cp /nonexistent/file /home/user/backup/  # This will cause the script to exit
   echo "This will not be executed if the previous command fails."
   ```

3. **`set -u`**: Treat unset variables as an error
   - This option will cause the script to terminate with an error message if any variable is referenced before being set.

   **Example:**
   ```bash
   #!/bin/bash
   set -u  # Exit if any unset variable is referenced
   echo "The value of my_variable is: $my_variable"
   ```

   If `my_variable` is not set, the script will exit immediately.

4. **`set -o pipefail`**: Fail on any command in a pipeline
   - By default, only the last command in a pipeline's exit status is considered. With `pipefail`, it considers the exit status of all commands in a pipeline.

   **Example:**
   ```bash
   #!/bin/bash
   set -o pipefail  # Ensure the script fails on pipeline errors
   echo "Trying to run a non-existent command | grep something"
   non_existent_command | grep "something"
   ```

   Even if `grep` succeeds, the script will fail due to the non-existent command.

5. **`set +x`, `set +e`, `set +u`**: Disabling specific options
   - To disable any of the above debugging options, you can use `set +<option>`. For example, `set +x` will stop the command tracing.

   **Example:**
   ```bash
   #!/bin/bash
   set -x
   echo "Tracing enabled"
   set +x
   echo "Tracing disabled"
   ```

---

### **3. Shell Script Variables**

Variables are used to store data that can be reused throughout the script. 

- **Assigning Variables**:
  ```bash
  my_var="Hello, RHCE!"
  ```

- **Accessing Variables**:
  ```bash
  echo $my_var
  ```

- **Command-line Arguments**:
  - `$1`, `$2`, ... are positional parameters passed to the script.
  - `$#` gives the number of arguments passed to the script.
  - `$@` gives all arguments passed to the script.

---

### **4. Conditional Statements in Shell Scripts**

You can use conditional statements (e.g., `if`, `elif`, `else`) to control the flow based on conditions.

#### **Example: Using `if` in Shell Script**

```bash
#!/bin/bash
file="/var/www/html/index.html"
if [ -f "$file" ]; then
    echo "File exists."
else
    echo "File does not exist."
fi
```

---

### **5. Looping in Shell Scripts**

Loops like `for`, `while`, and `until` help automate repetitive tasks.

#### **Example: For Loop**

```bash
#!/bin/bash
for i in {1..5}
do
    echo "Number $i"
done
```

#### **Example: While Loop**

```bash
#!/bin/bash
counter=1
while [ $counter -le 5 ]
do
    echo "Counter: $counter"
    ((counter++))
done
```

---

### **6. Functions in Shell Scripts**

Functions allow you to modularize your scripts for reusability.

#### **Defining and Calling Functions**

```bash
#!/bin/bash
greet_user() {
    echo "Hello, $1!"
}

greet_user "RHCE"
```

---

### **7. Text Processing in Shell Scripts**

You’ll often need to process text files (e.g., logs, data files). Linux provides powerful tools for this, such as `grep`, `awk`, and `sed`.

#### **Using `grep`**

```bash
#!/bin/bash
grep "error" /var/log/syslog
```

#### **Using `awk`**

```bash
#!/bin/bash
awk -F: '{print $1, $3}' /etc/passwd
```

#### **Using `sed`**

```bash
#!/bin/bash
sed -i 's/old_word/new_word/g' /path/to/file
```

---

### **8. Automating Tasks with Cron Jobs**

Cron is used to schedule repetitive tasks. You can automate backups, log rotations, and other tasks using cron jobs.

#### **Editing Cron Jobs**

To edit cron jobs:

```bash
crontab -e
```

#### **Cron Syntax**

```
* * * * * command_to_run
| | | | |
| | | | +---- Day of the week (0-7)
| | | +------ Month (1-12)
| | +-------- Day of the month (1-31)
| +---------- Hour (0-23)
+------------ Minute (0-59)
```

#### **Example: Running a Backup Script Daily at 5 AM**

```bash
0 5 * * * /path/to/backup_script.sh
```

---

### **9. Debugging and Error Handling**

Proper error handling ensures that your scripts don't fail silently, and debugging techniques help identify issues quickly.

#### **Checking Command Exit Status**

After a command, you can check the exit status with `$?`.

```bash
#!/bin/bash
cp /data/file /backup/
if [ $? -eq 0 ]; then
    echo "Copy succeeded."
else
    echo "Copy failed."
    exit 1
fi
```

#### **Using `trap` for Error Handling**

The `trap` command catches errors and signals. You can use `trap` to handle cleanup or logging when the script encounters an error.

```bash
#!/bin/bash

# Define a cleanup function
cleanup() {
    echo "An error occurred, performing cleanup."
}

# Trap any errors
trap cleanup ERR

echo "Starting script execution"
cp /invalid/file /backup/
echo "This won't be executed if the script encounters an error."
```

---

### **10. Best Practices for Shell Scripting**

When writing shell scripts for system automation (e.g., as part of RHCE tasks), it's important to follow best practices to ensure reliability and maintainability.

1. **Use Descriptive Variable Names**: Avoid ambiguous variable names. For example, use `backup_dir` instead of `dir`.
2. **Use Comments**: Explain complex parts of your scripts.
3. **Error Handling**: Always check for errors using `$?` or `set -e` for immediate termination.
4. **Modularize with Functions**: Make your scripts reusable by defining functions for common tasks.
5. **Ensure Portability**: Use POSIX-compliant syntax wherever possible, especially if your script might run on different Linux distributions.

---

## **Conclusion**

As a DevOps Engineer, mastering **Shell Scripting & Automation** is crucial for efficiently managing systems. The use of **set options** like `-x`, `-e`, and `-u` can greatly enhance your ability to debug and maintain scripts. Automating tasks with **cron**, handling errors properly, and processing text with tools like `grep`, `awk`, and `sed` are also key skills for system administrators. By following best practices and leveraging shell scripting effectively, you can ensure your Linux systems are maintained in an automated, efficient, and error-free manner.

Let's dive deeper into **text processing** in shell scripting, which is an essential skill for Linux system administration, especially for **RHCE**. Text processing tools like `grep`, `sed`, `awk`, `cut`, `tr`, and others are critical for managing logs, configuration files, and other text-based data efficiently.

---

## **Text Processing in Shell Scripting**

In Linux based environment, text processing is one of the most powerful ways to manipulate data and automate system tasks. Most of the data you encounter in Linux, including logs, user configurations, and output from various commands, is in plain text. Therefore, having a solid understanding of text processing commands will help you manipulate, search, and format this data to achieve your goals.

Below are the core tools and techniques for text processing in shell scripting.

---

### **1. `grep` (Global Regular Expression Print)**

`grep` is used to search for specific patterns (strings or regular expressions) in files. It prints matching lines, making it a fundamental tool for parsing logs and configurations.

#### **Basic Usage**

```bash
grep "pattern" file.txt
```

This will search for the word "pattern" in `file.txt` and print all lines that contain it.

#### **Examples**:

- **Search for a word in a file**:
  
  ```bash
  grep "error" /var/log/syslog
  ```

  This will search for the word "error" in the system log.

- **Search for a pattern with case-insensitivity** (`-i`):

  ```bash
  grep -i "warning" /var/log/syslog
  ```

- **Search for lines that do not match** (`-v`):

  ```bash
  grep -v "pattern" file.txt
  ```

  This will print all lines that do **not** contain the word "pattern".

- **Count occurrences** (`-c`):

  ```bash
  grep -c "error" /var/log/syslog
  ```

  This will count how many times "error" appears in the file.

#### **Regular Expressions in `grep`**

`grep` supports regular expressions, making it even more powerful:

- **Match a word starting with "e"**:

  ```bash
  grep "^e" file.txt
  ```

- **Match a word ending with "ing"**:

  ```bash
  grep "ing$" file.txt
  ```

- **Match any word containing "test"**:

  ```bash
  grep "test" file.txt
  ```

---

### **2. `sed` (Stream Editor)**

`sed` is used for text transformations, such as searching, replacing, and deleting text in files. It operates line-by-line, making it very efficient for processing large files.

#### **Basic Usage**

```bash
sed 's/pattern/replacement/' file.txt
```

This will replace the first occurrence of "pattern" with "replacement" in each line.

#### **Examples**:

- **Simple substitution**:

  ```bash
  sed 's/oldword/newword/' file.txt
  ```

  This replaces the first occurrence of "oldword" with "newword" in each line.

- **Global substitution** (`g` flag):

  ```bash
  sed 's/oldword/newword/g' file.txt
  ```

  This replaces **all** occurrences of "oldword" with "newword" in each line.

- **In-place editing** (`-i` flag):

  ```bash
  sed -i 's/oldword/newword/g' file.txt
  ```

  This modifies the file directly without requiring output redirection.

- **Delete lines** containing a pattern:

  ```bash
  sed '/pattern/d' file.txt
  ```

  This deletes all lines containing "pattern" from the file.

- **Insert text before a line**:

  ```bash
  sed '/pattern/i\New text' file.txt
  ```

  This inserts "New text" before any line containing "pattern".

- **Replace text with variables** (useful in scripts):

  ```bash
  var="newword"
  sed "s/oldword/$var/g" file.txt
  ```

---

### **3. `awk` (Pattern Scanning and Processing Language)**

`awk` is a powerful text processing tool used for pattern scanning and processing. It's ideal for working with structured data (like CSV files or logs) and performing operations on specific columns.

#### **Basic Usage**

```bash
awk 'pattern {action}' file.txt
```

Here, `pattern` specifies the condition for selecting lines, and `action` specifies what to do with those lines.

#### **Examples**:

- **Print specific columns**:

  ```bash
  awk '{print $1, $3}' file.txt
  ```

  This prints the first and third columns from `file.txt`.

- **Using a delimiter** (e.g., CSV files):

  ```bash
  awk -F, '{print $1, $2}' file.csv
  ```

  This uses the comma `,` as the field separator to print the first two columns of a CSV file.

- **Sum of a column**:

  ```bash
  awk '{sum += $2} END {print sum}' file.txt
  ```

  This sums up the values in the second column of `file.txt`.

- **Match a pattern and perform an action**:

  ```bash
  awk '/pattern/ {print $1, $3}' file.txt
  ```

  This prints the first and third columns only from lines that match "pattern".

- **Using `awk` with variables**:

  ```bash
  var=100
  awk -v threshold="$var" '$2 > threshold {print $1}' file.txt
  ```

  This uses an external shell variable (`threshold`) in `awk` to compare values in the second column.

---

### **4. `cut` (Cut sections from each line of a file)**

The `cut` command is used to extract sections from each line of input, usually by fields or characters.

#### **Basic Usage**

```bash
cut -d 'delimiter' -f field_number file.txt
```

Here, `-d` specifies the delimiter (e.g., comma for CSV), and `-f` specifies which field to extract.

#### **Examples**:

- **Extract the first column (space-delimited)**:

  ```bash
  cut -d ' ' -f 1 file.txt
  ```

- **Extract multiple fields**:

  ```bash
  cut -d ':' -f 1,3 /etc/passwd
  ```

  This extracts the first and third fields (usernames and UID) from `/etc/passwd`.

- **Extract specific number of characters**:

  ```bash
  cut -c 1-5 file.txt
  ```

  This extracts the first 5 characters of each line in the file.

---

### **5. `tr` (Translate or delete characters)**

`tr` is used to translate characters or delete specific characters in text.

#### **Basic Usage**

```bash
echo "input_string" | tr 'old_chars' 'new_chars'
```

This command replaces occurrences of `old_chars` with `new_chars`.

#### **Examples**:

- **Convert lowercase to uppercase**:

  ```bash
  echo "hello world" | tr 'a-z' 'A-Z'
  ```

- **Delete specific characters**:

  ```bash
  echo "hello123" | tr -d '0-9'
  ```

  This removes all digits from the string.

- **Replace spaces with underscores**:

  ```bash
  echo "hello world" | tr ' ' '_'
  ```

---

### **6. Combining Text Processing Tools**

In real-world scripts, you often combine multiple text processing tools to get the desired result.

#### **Example: Parsing Logs and Generating Reports**

Suppose you want to extract the IP addresses from an Apache log file and count how many times each IP address appears.

```bash
#!/bin/bash
awk '{print $1}' /var/log/apache2/access.log | sort | uniq -c | sort -nr
```

This command does the following:
- `awk '{print $1}'` extracts the first column (IP address).
- `sort` sorts the IP addresses.
- `uniq -c` counts the unique occurrences.
- `sort -nr` sorts the output in reverse numerical order.

---

### **7. Scripting Example: Monitoring Disk Usage**

Here's an example script that combines `df`, `grep`, `awk`, and `sed` to monitor disk usage and send an alert if usage exceeds a threshold:

```bash
#!/bin/bash
THRESHOLD=80

# Get disk usage and check if it's above the threshold
usage=$(df -h | grep '/dev/sda1' | awk '{print $5}' | sed 's/%//')

if [ "$usage" -gt "$THRESHOLD" ]; then
  echo "Disk usage is above $THRESHOLD%! Usage is $usage%"
  # Send an alert (this could be an email or message)
  # mail -s "Disk Alert" user@example.com <<< "Disk usage has exceeded $THRESHOLD%. Current usage: $usage%"
else
  echo "Disk usage is below the threshold. Usage is $usage%"
fi
```

This script:
- Uses `df` to check disk usage.
- Uses `grep` to filter the specific disk partition.
- Uses `awk` to extract the percentage.
- Uses `sed` to remove the `%` symbol and convert it to an integer.
- Sends an alert if the usage exceeds the specified threshold.

---

### **Conclusion**

Text processing is one of the most powerful tools for **Linux system administration.** Mastering commands like **`grep`**, **`sed`**, **`awk`**, **`cut`**, and **`tr`** allows administrators to automate, parse, modify, and filter text data, such as logs, configuration files, and system outputs.

These tools are essential for debugging scripts, monitoring systems, and creating reports. Combining them in shell scripts enables efficient automation and problem-solving for daily system administration tasks.

----------------------------------------------------------------------------------------

### **Combined DevOps Shell Script**

**Real-world Example** of shell script that integrates several concepts, including **log monitoring, system health checks, backup creation, log cleanup, resource usage monitoring, and alerts**. This script can be scheduled to run periodically (e.g., via a cron job) to automate system administration tasks, improve efficiency, and ensure the health of the server.


#### **Objective**: 
The script will:
1. **Check Apache logs** for 404 errors.
2. **Monitor system resources** (CPU, Memory, Disk).
3. **Create backups** of configuration files.
4. **Clean up old log files**.
5. **Send alerts** for critical conditions.
6. **Log the actions** in a log file for audit purposes.

#### **Script: `devops_automation.sh`**

```bash
#!/bin/bash

# Set variables
LOG_FILE="/var/log/apache2/access.log"
ERROR_PATTERN="404 Not Found"
LOG_DIR="/var/log"
CONFIG_DIR="/etc"
BACKUP_DIR="/backup"
THRESHOLD=80
RETENTION_PERIOD=7
ALERT_EMAIL="admin@example.com"
TIMESTAMP=$(date +'%Y-%m-%d_%H-%M-%S')
ARCHIVE_NAME="config_backup_$TIMESTAMP.tar.gz"
CPU_USAGE=0
MEMORY_USAGE=0
DISK_USAGE=0

# Step 1: Check Apache Logs for 404 Errors
echo "Checking Apache logs for 404 errors..."
grep "$ERROR_PATTERN" $LOG_FILE > /tmp/404_errors.txt

if [ -s /tmp/404_errors.txt ]; then
    echo "There were 404 errors in Apache logs at $(date)" | mail -s "Apache Error Alert: 404 Not Found" $ALERT_EMAIL
    echo "$(date) - 404 errors found and alert sent" >> /var/log/devops_automation.log
else
    echo "No 404 errors found."
fi

# Step 2: Monitor System Resource Usage (CPU, Memory, Disk)
echo "Monitoring system resources..."

# Check CPU usage
CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | sed "s/.*, *\([0-9.]*\)%* id.*/\1/" | awk '{print 100 - $1}')
if [ $(echo "$CPU_USAGE > $THRESHOLD" | bc) -eq 1 ]; then
    echo "CPU usage is high: $CPU_USAGE%" | mail -s "CPU Usage Alert" $ALERT_EMAIL
    echo "$(date) - CPU usage alert sent: $CPU_USAGE%" >> /var/log/devops_automation.log
fi

# Check memory usage
MEMORY_USAGE=$(free | grep Mem | awk '{print $3/$2 * 100.0}')
if [ $(echo "$MEMORY_USAGE > $THRESHOLD" | bc) -eq 1 ]; then
    echo "Memory usage is high: $MEMORY_USAGE%" | mail -s "Memory Usage Alert" $ALERT_EMAIL
    echo "$(date) - Memory usage alert sent: $MEMORY_USAGE%" >> /var/log/devops_automation.log
fi

# Check disk usage
DISK_USAGE=$(df / | grep / | awk '{ print $5 }' | sed 's/%//g')
if [ $DISK_USAGE -gt $THRESHOLD ]; then
    echo "Disk usage is high: $DISK_USAGE%" | mail -s "Disk Usage Alert" $ALERT_EMAIL
    echo "$(date) - Disk usage alert sent: $DISK_USAGE%" >> /var/log/devops_automation.log
fi

# Step 3: Backup Configuration Files
echo "Creating backup of configuration files..."
tar -czf $BACKUP_DIR/$ARCHIVE_NAME -C $CONFIG_DIR .
echo "$(date) - Backup created: $ARCHIVE_NAME" >> /var/log/devops_automation.log

# Step 4: Clean Up Old Log Files (Older than 7 days)
echo "Cleaning up log files older than $RETENTION_PERIOD days..."
find $LOG_DIR -name "*.log" -type f -mtime +$RETENTION_PERIOD -exec rm -f {} \;
echo "$(date) - Cleaned up log files older than $RETENTION_PERIOD days." >> /var/log/devops_automation.log

# Step 5: Log the Health Status of the System
echo "Logging system health status..."
echo "$(date) - CPU Usage: $CPU_USAGE%, Memory Usage: $MEMORY_USAGE%, Disk Usage: $DISK_USAGE%" >> /var/log/devops_automation.log

# Step 6: Send Daily Health Summary
echo "Sending daily health summary..."
HEALTH_SUMMARY="System Health Status on $(date):\n
CPU Usage: $CPU_USAGE%\n
Memory Usage: $MEMORY_USAGE%\n
Disk Usage: $DISK_USAGE%\n
Backup Created: $ARCHIVE_NAME\n
Log Cleanup: Successful"

echo -e "$HEALTH_SUMMARY" | mail -s "Daily System Health Summary" $ALERT_EMAIL

echo "$(date) - Health summary sent to admin" >> /var/log/devops_automation.log

# End of Script
```

---

### **Explanation of the Script**:

1. **Log Monitoring (Apache 404 Errors)**:
   - The script checks Apache logs (`/var/log/apache2/access.log`) for `404 Not Found` errors using the **`grep`** command.
   - If errors are found, it sends an email alert to the administrator and logs the event.

2. **System Resource Monitoring (CPU, Memory, Disk Usage)**:
   - **CPU Usage**: It uses the `top` command to capture CPU usage and compares it with a predefined threshold.
   - **Memory Usage**: The `free` command is used to check memory usage, and an alert is sent if the usage exceeds the threshold.
   - **Disk Usage**: The `df` command checks the disk usage of the root directory (`/`) and triggers an alert if usage is high.

3. **Backup Configuration Files**:
   - Creates a backup of the `/etc/` directory, which is often where system configuration files reside, using the `tar` command with a timestamped filename (`config_backup_<timestamp>.tar.gz`).
   - The backup action is logged.

4. **Log File Cleanup**:
   - This step finds and deletes `.log` files in `/var/log` that are older than 7 days, helping to keep the system clean and free from excessive logs.
   - The script uses the **`find`** command to locate files older than 7 days and delete them.

5. **Logging Health Status**:
   - The script logs the current CPU, memory, and disk usage, as well as the backup and log cleanup status into `/var/log/devops_automation.log`.

6. **Sending Daily Health Summary**:
   - At the end of the script, it compiles a health summary (CPU, memory, disk usage, backup status, and cleanup status) and sends it via email to the administrator.

---

### **How to Automate the Script**:

You can schedule this script to run periodically using **cron**. For example, to run the script daily at 2:00 AM, add the following line to the cron job configuration:

```bash
0 2 * * * /path/to/devops_automation.sh
```

To edit the cron jobs, run:

```bash
crontab -e
```

---

### **Use Cases for DevOps Automation**:

This script represents the following **DevOps automation use cases**:
1. **Log Monitoring**: Automatically identifies and alerts on errors in web server logs (e.g., Apache).
2. **Resource Monitoring**: Continuously monitors system resources (CPU, memory, disk) and triggers alerts when thresholds are exceeded.
3. **Backup Creation**: Ensures system configuration files are backed up regularly.
4. **Log Cleanup**: Cleans up old log files to free up disk space and keep the system organized.
5. **Health Monitoring and Alerts**: Provides real-time feedback about system health, allowing the administrator to take proactive actions.


    This comprehensive script simplifies system administration tasks, reduces manual intervention, and ensures that the server remains optimized, secure, and well-maintained.
---

### **Conclusion: Real-World DevOps Use Cases for Shell Scripting**

In DevOps, **shell scripting** and **text processing** are essential for:
- **Automating repetitive tasks** like log monitoring, file backups, system health checks, etc.
- **Extracting meaningful insights** from log files and system outputs.
- **Sending alerts** based on specific conditions (e.g., disk usage, application errors).
- **Performing cleanups** of logs and temporary files to save disk space.
  
By mastering tools like **`grep`**, **`awk`**, **`sed`**, **`cut`**, and **`tr`**, DevOps engineers can automate and streamline various administrative and operational tasks, making systems more efficient and reliable.
<hr/>