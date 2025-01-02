# User & Group Management | Day 3 | Complete Linux Course 

Welcome back to Day 2 of our Complete Linux Series. Today, we are diving deep into User and Group Management in Linux. 

If you missed our previous session, make sure to check it out on our YouTube channel.

By the end of today’s lesson, you’ll have the skills to manage user accounts, control permissions, create secure environments, and automate processes to handle multiple users. 

## 1. Introduction
In Linux, users and groups control access to resources. There are different types of users in Linux. We have the root user — who is the system admin with full privileges. Then, there are system users, like apache or mysql, which are specific to certain services.

You also have regular users for day-to-day tasks, guest users for temporary access, and even locked users, who can't log in but might exist for system purposes.

In Linux, there are three types of users:  
- **Root user**: The superuser with unrestricted access.  
- **System users**: Used by services and applications (e.g., `apache`, `mysql`).  
- **Regular users**: Created for human operators.

## 2. User Management
#### **1. Adding Users**  

Let’s start with creating a user. We use the useradd command. It’s straightforward. 
Let's create a user named john_cena.

```bash
sudo useradd -m -s /bin/bash john_cena
sudo passwd john_cena
```
-m option creates a home directory, and -s specifies the shell.

Once the user is created, we set a password with the passwd command.


#### **2. Modifying Users**  
  
Now, let’s say John is moving departments, and his home directory needs to change.

Once the user is modified, existing home directory gets moved to the new directory.

```bash
sudo usermod -d /home/dev_john_cena -m john_cena
```

#### **3. Deleting Users**  

When an employee leaves, you might need to remove their account while keeping the data for auditing.

Once the user is deleted, we delete the home directory and mail spool for the user.

```bash
sudo userdel -r john_cena
```

## 3. Group Management
#### **1. Creating Groups**  
Let’s create a group for our developers.”  
```bash
sudo groupadd developers
```

#### **2. Adding Users to Groups**  
To make John part of the `developers` group:”  
```bash
sudo usermod -aG developers john_cena
```

#### **3. Primary and Secondary Groups**  
A user can have one primary group and multiple secondary groups. Let’s change John’s primary group to `devops`.”  
```bash
sudo usermod -g devops john_cena
```

#### **4. Deleting Groups**  
To delete a group no longer in use:”  
```bash
sudo groupdel developers
```

## Chapter 4: File Permissions in Linux 

#### **1. Understanding Ownership**
- **Owner**: User who created the file.  
- **Group**: Users sharing group permissions.  
- **Others**: All other users.  

#### **2. Changing Ownership**
Let’s change the ownership of a file.”  
```bash
sudo chown john_cena:developers project_file.txt
```

#### **3. Modifying Permissions**
To restrict access to the file so only the owner can read/write and group members can read:”  
    ```bash
    chmod 640 project_file.txt
    ```

    **Explanation:**  
    - `6 (rw-)`: Owner can read and write.  
    - `4 (r--)`: Group can only read.  
    - `0 (---)`: No permissions for others.


## Chapter 5: Advanced Permission Management 

#### **1. Access Control Lists (ACLs)**  
ACLs allow you to assign permissions to specific users or groups without affecting others. 
Let’s give `john_cena` write access to a file owned by someone else.”  

```bash
sudo setfacl -m u:john_cena:rw project_file.txt
getfacl project_file.txt
```

#### **2. Special Permissions**
- **SUID (Set User ID)**: Execute a program with the file owner’s privileges.
- **SGID (Set Group ID)**: Files inherit group ownership.  
- **Sticky Bit**: Users can only delete their files in shared directories.  

Let’s secure a shared directory with SGID and Sticky Bit.”  

```bash
sudo chmod 2775 /var/shared_dir
sudo chmod +t /var/shared_dir
```

- `2`: SGID ensures new files belong to the directory group.  
- `+t`: Sticky Bit prevents accidental deletion by others.


## Chapter 6: Managing Default User Environment 


#### **1. Purpose of `/etc/skel`**
When you create a user, their home directory is initialized with files from `/etc/skel`. 
Let’s customize it for our DevOps team.”  
```bash
sudo echo "export PATH=\$PATH:/usr/local/devops_tools" >> /etc/skel/.bashrc
```

#### **2. Testing Changes**
Now, create a new user and verify the default settings.”  
```bash
sudo useradd -m -s /bin/bash devops_user
cat /home/devops_user/.bashrc
```

## Chapter 7: Monitoring & Auditing Users

#### **1. Viewing Logged-In Users**  
```bash
who
```

#### **2. Checking Last Login Times**  
```bash
last john_cena
```

#### **3. Monitoring User Activity**  
```bash
w
```

These commands are vital for ensuring system security and auditing compliance.



## Example: Create a secure environment for a project team
**Below Requirements:**
#### 1. Create users: `alice`, `bob`, and `charlie`.  
#### 2. Add them to a group `project_team`.  
#### 3. Set up a shared directory `/project_data` with SGID and Sticky Bit.  
#### 4. Restrict access to the directory for the team only.”

**Commands:**  
```bash
sudo useradd -m -s /bin/bash alice
sudo useradd -m -s /bin/bash bob
sudo useradd -m -s /bin/bash charlie

sudo groupadd project_team
sudo usermod -aG project_team alice
sudo usermod -aG project_team bob
sudo usermod -aG project_team charlie

sudo mkdir /project_data
sudo chown :project_team /project_data
sudo chmod 2770 /project_data
sudo chmod +t /project_data
```

Today, we've mastered Linux user and group management with real-world scenarios. 

**These skills are essential for DevOps Engineers & any Linux administrator.**
<hr/>
