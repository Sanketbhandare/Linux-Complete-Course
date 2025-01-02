# Package, Service & Process Management | Day 4 | Complete Linux Course 

### **1. Package Management**

In Linux, **Package Management** is essential for installing, upgrading, and removing software. Red Hat-based systems (including RHEL, CentOS, and Fedora) use **`yum`** or **`dnf`** for package management.

#### **Installing Packages**

To install software packages from a repository, you can use the following commands:

- **Using `dnf`** (recommended for RHEL 8 and later):
  ```bash
  sudo dnf install <package_name>
  ```
  Example:
  ```bash
  sudo dnf install httpd
  ```

- **Using `yum`** (for RHEL 7 and earlier):
  ```bash
  sudo yum install <package_name>
  ```
  Example:
  ```bash
  sudo yum install nginx
  ```

#### **Removing Packages**

To remove a package from the system:

- **Using `dnf`**:
  ```bash
  sudo dnf remove <package_name>
  ```

- **Using `yum`**:
  ```bash
  sudo yum remove <package_name>
  ```

Example:
```bash
sudo dnf remove nginx
```

#### **Updating Packages**

- **Update all installed packages**:
  - **Using `dnf`**:
    ```bash
    sudo dnf update
    ```
  - **Using `yum`**:
    ```bash
    sudo yum update
    ```

- **Update a specific package**:
  - **Using `dnf`**:
    ```bash
    sudo dnf update <package_name>
    ```
  - **Using `yum`**:
    ```bash
    sudo yum update <package_name>
    ```

#### **Searching for Packages**

You can search for available packages in the repositories:

- **Using `dnf`**:
  ```bash
  sudo dnf search <package_name>
  ```

- **Using `yum`**:
  ```bash
  sudo yum search <package_name>
  ```

#### **Viewing Installed Packages**

To list installed packages:

- **Using `dnf`**:
  ```bash
  sudo dnf list installed
  ```

- **Using `yum`**:
  ```bash
  sudo yum list installed
  ```

---

### **2. Service Management**

Linux systems use **Systemd** for managing services, especially on RHEL 7 and later versions. Systemd provides tools like **`systemctl`** to control services, enable or disable services at boot, and check their statuses.

#### **Starting and Stopping Services**

To start a service:

```bash
sudo systemctl start <service_name>
```
Example (to start Apache web server):
```bash
sudo systemctl start httpd
```

To stop a service:

```bash
sudo systemctl stop <service_name>
```
Example (to stop Apache):
```bash
sudo systemctl stop httpd
```

#### **Enabling Services at Boot**

To enable a service to start automatically when the system boots, use:

```bash
sudo systemctl enable <service_name>
```
Example:
```bash
sudo systemctl enable httpd
```

To disable a service from starting at boot:

```bash
sudo systemctl disable <service_name>
```
Example:
```bash
sudo systemctl disable httpd
```

#### **Checking the Status of Services**

To check the status of a service:

```bash
sudo systemctl status <service_name>
```
Example:
```bash
sudo systemctl status httpd
```

This command shows whether the service is running or stopped and provides logs related to the service.

#### **Reloading and Restarting Services**

To reload the configuration of a service (without stopping it):

```bash
sudo systemctl reload <service_name>
```

To restart a service (to restart it completely):

```bash
sudo systemctl restart <service_name>
```
Example:
```bash
sudo systemctl restart httpd
```

#### **Viewing Logs of Services**

You can view logs for services managed by **systemd** using the `journalctl` command. To view logs for a specific service:

```bash
sudo journalctl -u <service_name>
```
Example:
```bash
sudo journalctl -u httpd
```

To follow logs in real-time:

```bash
sudo journalctl -u <service_name> -f
```

---

### **3. Process Management**

Linux process management involves monitoring and controlling the processes running on a system. The **`ps`**, **`top`**, **`kill`**, and **`nice`** commands are commonly used to manage processes.

#### **Viewing Running Processes**

To see a list of running processes, use **`ps`**:

- **List all processes**:
  ```bash
  ps aux
  ```
  - **`a`**: Shows processes for all users.
  - **`u`**: Displays processes in a user-oriented format.
  - **`x`**: Lists processes that do not have a terminal.

- **View processes by specific user**:
  ```bash
  ps -u <username>
  ```

#### **Using `top` to Monitor Processes**

The **`top`** command provides a dynamic view of the processes running on the system:

```bash
top
```

- **`top`** displays a list of processes, along with resource utilization like CPU and memory.
- You can press **`q`** to quit `top`.

#### **Killing Processes**

If a process becomes unresponsive, you can kill it using **`kill`**.

- Find the **PID** (Process ID) of the process:
  ```bash
  ps aux | grep <process_name>
  ```

- To kill a process by its PID:
  ```bash
  sudo kill <PID>
  ```

- To forcefully kill a process:
  ```bash
  sudo kill -9 <PID>
  ```

Example:
```bash
sudo kill -9 1234
```

#### **Renicing Processes**

You can change the priority of a process using **`nice`** and **`renice`** commands. This is useful in a **DevOps environment** when managing long-running processes like builds or backups.

- **`nice`**: Start a new process with a specified priority (lower priority number = higher priority).
  ```bash
  sudo nice -n 10 command
  ```

- **`renice`**: Change the priority of an existing process.
  ```bash
  sudo renice -n 10 -p <PID>
  ```

#### **Foreground and Background Processes**

- **Send a process to the background**:
  ```bash
  command &
  ```

- **Bring a background process to the foreground**:
  Press **`Ctrl + Z`** to suspend a process and then run:
  ```bash
  fg
  ```

- **View jobs**:
  ```bash
  jobs
  ```

---

### **Real-World Example in DevOps Project**

Let’s walk through a real-world example where we manage services, packages, and processes in a **DevOps project** involving an **Apache web server** and a **HTML Web Application application**.

#### **1. Install and Manage Packages**

- Install Apache: (ubuntu)
  ```bash
  sudo apt update
  sudo apt install apache2
  ```
  (CentOs/RedHat)
  ```bash
  sudo dnf install httpd
  ```

- Ensure the Apache service starts at boot:
  ```bash
  sudo systemctl enable apache2
  ```
  (CentOs/RedHat)
  ```bash
  sudo systemctl enable httpd
  ```

#### **2. Manage Apache Service**

- Start the Apache web server:
  ```bash
  sudo systemctl start apache2 # Use  httpd for CentOS / RedHat OS
  ```

- Check the status of the Apache service:
  ```bash
  sudo systemctl status apache2  # Use  httpd for CentOS / RedHat OS
  ```

- Reload Apache if its configuration has changed:
  ```bash
  sudo systemctl reload apache2  # Use  httpd for CentOS / RedHat OS
  ```

- Restart Apache if needed (e.g., after a crash):
  ```bash
  sudo systemctl restart apache2  # Use  httpd for CentOS / RedHat OS
  ```

#### **3. Process Management for Apache Web Application**

- Monitor the Apache application’s running process using `top` or `ps`:
  ```bash
  ps aux | grep apache
  ```

- Kill an unresponsive Apache process:
  ```bash
  sudo kill -9 <PID>
  ```

- Monitor real-time CPU and memory usage using **`top`**:
  ```bash
  top
  ```

---

### **Summary of Topics covered so far:**

**Package Management**, **Service Management**, and **Process Management** are foundational to Linux system administration. Here’s a summary of the key points:

- **Package Management**: Install, update, and remove software using `apt` on Ubuntu OS, `dnf` (RHEL 8+) or `yum` (RHEL 7). Search and list packages with `dnf search` and `dnf list installed`.
- **Service Management**: Use `systemctl` to start, stop, enable, and disable services like Apache (`httpd`). View logs with `journalctl`.
- **Process Management**: Monitor and manage running processes with `ps`, `top`, and `kill`. Use `nice` and `renice` to adjust process priorities.

These concepts are essential for managing Linux systems in a DevOps environment, ensuring the smooth operation and automation of software deployment, monitoring, and maintenance tasks.

----------------------------------------------------------------------------------------

## Real-Time Example: DevOps Project to Host a Website Using Apache Server on Ubuntu VM

Here is a complete, step-by-step example with the requested format to set up and test Apache on Ubuntu, create a feedback form, generate fake load, and monitor traffic using **Apache Bench**:

---

### **1. Install & Verify Apache Web Server to Host a Website (Package Management)**

First, let's install the Apache web server using the **apt package manager**.

```bash
sudo apt update
sudo apt install apache2
```

After installation, verify that Apache is installed correctly:

```bash
apache2 -v
```

This should display the version of Apache that was installed.

---

### **2. Start Apache Web Server (Service Management)**

Now, start the Apache service to begin serving the web pages:

```bash
sudo systemctl start apache2
```

To make sure Apache starts automatically upon boot, enable it:

```bash
sudo systemctl enable apache2
```

Verify that Apache is running:

```bash
sudo systemctl status apache2
```

If it's active, you're ready to continue.

---

### **3. Test Whether Apache Web Server is Accessible or Not**

Open a web browser and go to `http://localhost` (or the IP address of your server if it's on a different machine).

You should see the default Apache welcome page, indicating that the Apache server is running and accessible.

Alternatively, you can test accessibility directly in the terminal using `curl`:

```bash
curl http://localhost
```

If the page is accessible, you should see the HTML content of the Apache welcome page.

---

### **4. Monitor Apache Processes (Process Management)**

To monitor the running Apache processes, use the `ps` command:

```bash
ps aux | grep apache2
```

This shows all running Apache processes and their process IDs (PIDs). You can also use `top` for a dynamic view:

```bash
top
```

To see only Apache-related processes in `top`, press `Shift` + `L` and type `apache2`.

---

### **5. Create a Feedback Form for the Linux Course (HTML Page)**

Let's create a basic HTML feedback form for the **Linux course**. First, create the `index.html` file in the Apache web directory:

```bash
sudo vim /var/www/html/index.html
```

Add the following HTML code for the feedback form:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Linux Course Feedback</title>
    <style>
        /* Global Styles */
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            background: linear-gradient(135deg, #6f42c1, #ff7f50);
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .container {
            background: white;
            border-radius: 10px;
            padding: 30px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 600px;
            transition: transform 0.3s ease-in-out;
        }

        .container:hover {
            transform: scale(1.05);
        }

        h2 {
            text-align: center;
            color: #333;
            margin-bottom: 20px;
            font-size: 24px;
            font-weight: bold;
        }

        label {
            font-size: 16px;
            color: #555;
            margin-bottom: 10px;
            display: block;
        }

        input[type="text"], input[type="email"], textarea, select {
            width: 100%;
            padding: 12px;
            margin-bottom: 20px;
            border-radius: 8px;
            border: 1px solid #ddd;
            font-size: 14px;
            box-sizing: border-box;
            transition: border-color 0.3s ease;
        }

        input[type="text"]:focus, input[type="email"]:focus, textarea:focus, select:focus {
            border-color: #6f42c1;
            outline: none;
        }

        .rating {
            display: flex;
            justify-content: space-between;
            max-width: 350px;
            margin-bottom: 20px;
        }

        .rating input {
            margin-right: 5px;
        }

        .rating label {
            font-size: 18px;
            color: #666;
        }

        button {
            background-color: #6f42c1;
            color: white;
            padding: 12px 20px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            width: 100%;
            transition: background-color 0.3s ease;
        }

        button:hover {
            background-color: #5a34a4;
        }

        .textarea-container {
            display: flex;
            flex-direction: column;
        }

        /* Success Message */
        .success-message {
            text-align: center;
            padding: 20px;
            background-color: #4CAF50;
            color: white;
            font-size: 18px;
            border-radius: 8px;
            margin-top: 20px;
            display: none;
        }

        /* Responsive Design */
        @media (max-width: 600px) {
            .container {
                padding: 20px;
            }

            h2 {
                font-size: 22px;
            }

            .rating {
                flex-direction: column;
            }

            .rating input {
                margin-bottom: 5px;
            }

            button {
                padding: 10px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>Linux Course Feedback</h2>
        <form id="feedbackForm">
            <!-- Name and Email Fields -->
            <label for="name">Your Name:</label>
            <input type="text" id="name" name="name" placeholder="Enter your name" required>

            <label for="email">Your Email:</label>
            <input type="email" id="email" name="email" placeholder="Enter your email" required>

            <!-- Course Rating -->
            <label for="course_rating">How would you rate the course content?</label>
            <div class="rating">
                <label for="course_rating_1">1</label>
                <input type="radio" id="course_rating_1" name="course_rating" value="1" required>
                <label for="course_rating_2">2</label>
                <input type="radio" id="course_rating_2" name="course_rating" value="2">
                <label for="course_rating_3">3</label>
                <input type="radio" id="course_rating_3" name="course_rating" value="3">
                <label for="course_rating_4">4</label>
                <input type="radio" id="course_rating_4" name="course_rating" value="4">
                <label for="course_rating_5">5</label>
                <input type="radio" id="course_rating_5" name="course_rating" value="5">
            </div>

            <!-- Instructor Rating -->
            <label for="instructor_rating">How would you rate the instructor?</label>
            <div class="rating">
                <label for="instructor_rating_1">1</label>
                <input type="radio" id="instructor_rating_1" name="instructor_rating" value="1" required>
                <label for="instructor_rating_2">2</label>
                <input type="radio" id="instructor_rating_2" name="instructor_rating" value="2">
                <label for="instructor_rating_3">3</label>
                <input type="radio" id="instructor_rating_3" name="instructor_rating" value="3">
                <label for="instructor_rating_4">4</label>
                <input type="radio" id="instructor_rating_4" name="instructor_rating" value="4">
                <label for="instructor_rating_5">5</label>
                <input type="radio" id="instructor_rating_5" name="instructor_rating" value="5">
            </div>

            <!-- Additional Comments -->
            <div class="textarea-container">
                <label for="comments">Additional Comments:</label>
                <textarea id="comments" name="comments" rows="5" placeholder="Enter your feedback here..."></textarea>
            </div>

            <!-- Submit Button -->
            <button type="submit">Submit Feedback</button>
        </form>

        <!-- Success Message -->
        <div class="success-message" id="successMessage">
            Thank you for your feedback!
        </div>
    </div>

    <script>
        // Handle form submission
        document.getElementById("feedbackForm").addEventListener("submit", function(event) {
            event.preventDefault();  // Prevent page reload

            // Collecting form data
            const name = document.getElementById("name").value;
            const email = document.getElementById("email").value;
            const courseRating = document.querySelector('input[name="course_rating"]:checked')?.value;
            const instructorRating = document.querySelector('input[name="instructor_rating"]:checked')?.value;
            const comments = document.getElementById("comments").value;

            // Validation
            if (!courseRating || !instructorRating) {
                alert("Please rate the course and the instructor.");
                return;
            }

            // Display the success message
            document.getElementById("successMessage").style.display = "block";

            // Clear the form after submission
            document.getElementById("feedbackForm").reset();

            // Optionally, you could send the feedback to a server here using fetch() or XMLHttpRequest
            console.log({
                name: name,
                email: email,
                courseRating: courseRating,
                instructorRating: instructorRating,
                comments: comments
            });
        });
    </script>
</body>
</html>
```

Save the file and close the editor.

---

### **6. Performance Testing: Generate Fake Load & Monitor Traffic with Apache Bench**
Now, we will generate fake load using **Apache Bench** (`ab`) to simulate traffic on your Apache server.
Run the following command to simulate **1000 requests** with **10 concurrent connections**:

```bash
  ab -n 1000 -c 10 http://localhost/
```

This will send **1000 total requests** to the server with **10 concurrent connections**.

#### Monitor Server Performance

While generating traffic, you can monitor the server’s performance using the following tools:
**Check Apache's Access Logs:**
To monitor the requests being processed by Apache, you can tail the access log:

  tail -f /var/log/apache2/access.log

This will display each request as it's processed in real-time.

**Analyze Apache Bench Output**

After running the ab command, it will output various metrics such as:
 - Requests per second: How many requests Apache handled per second.
 - Time per request: The average time it took to handle each request.
 - Connection times: The minimum, maximum, and mean time taken for a connection.

For example, you might see:
```
  Requests per second:    50.00 [#/sec] (mean)
  Time per request:       200.000 [ms] (mean)
```
These numbers will give you an idea of how Apache is handling the simulated load.

---

### **7. Clean Up (Package Management)**

After testing, you can clean up by removing any unnecessary packages. 
If you installed Apache Bench and no longer need it, remove it using:

```bash
sudo apt remove apache2-utils
```

If you want to remove Apache entirely:
```bash
sudo apt remove apache2
```

To remove the configuration files as well:
```bash
sudo apt purge apache2
```

Finally, clean up unused dependencies:
```bash
sudo apt autoremove
```


**This example demonstrates the basic usage of Package Management, Service Management, and Process Management in Linux.**

<hr/>