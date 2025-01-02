### Linux Networking | Linux Complete Course

Networking is a core aspect of managing Linux servers. 
In this section, we will cover the essential Linux networking concepts and tools necessary for DevOps Engineers. 
These include network interfaces, configuration, troubleshooting, firewall management, network services, and routing.

### **1. Network Interface Configuration**

#### **Ubuntu:**
- **Checking Network Interfaces:**
  ```bash
  ip a       # or ifconfig (older command)
  ```

- **Configuring Network Interfaces (Netplan):**
  - Configuration files are located in `/etc/netplan/`.
  - Example static IP configuration:

    ```yaml
    network:
      version: 2
      renderer: networkd
      ethernets:
        eth0:
          dhcp4: no
          addresses:
            - 192.168.1.100/24
          gateway4: 192.168.1.1
          nameservers:
            addresses:
              - 8.8.8.8
              - 8.8.4.4
    ```

  - Apply configuration:

    ```bash
    sudo netplan apply
    ```

#### **Red Hat (RHEL/CentOS):**
- **Checking Network Interfaces:**
  ```bash
  ip a       # or ifconfig (older command)
  ```

- **Configuring Network Interfaces (ifcfg files):**
  - Files are located in `/etc/sysconfig/network-scripts/`.
  - Example static IP configuration for `eth0`:

    ```bash
    sudo vi /etc/sysconfig/network-scripts/ifcfg-eth0
    ```

    Configuration for static IP:

    ```bash
    TYPE=Ethernet
    BOOTPROTO=static
    NAME=eth0
    DEVICE=eth0
    ONBOOT=yes
    IPADDR=192.168.1.100
    NETMASK=255.255.255.0
    GATEWAY=192.168.1.1
    DNS1=8.8.8.8
    DNS2=8.8.4.4
    ```

  - Restart network service:

    ```bash
    sudo systemctl restart network
    ```

---

### **2. Network Configuration Files**

#### **Ubuntu:**
- **Static IP Configuration:** `/etc/netplan/*.yaml`
- **DNS Configuration:** `/etc/resolv.conf`

#### **Red Hat (RHEL/CentOS):**
- **Static IP Configuration:** `/etc/sysconfig/network-scripts/ifcfg-<interface_name>`
- **Global Network Configuration:** `/etc/sysconfig/network`
- **DNS Configuration:** `/etc/resolv.conf`

---

### **3. Network Troubleshooting Tools**

#### **Ubuntu & Red Hat:**

- **Ping:**
  ```bash
  ping google.com
  ```

- **Traceroute:**
  - **Ubuntu:**
    ```bash
    sudo apt install traceroute
    traceroute google.com
    ```

  - **Red Hat:**
    ```bash
    sudo yum install traceroute -y
    traceroute google.com
    ```

- **Netstat:**
  - **Ubuntu:**
    ```bash
    sudo apt install net-tools
    netstat -tuln
    ```

  - **Red Hat:**
    ```bash
    sudo yum install net-tools -y
    netstat -tuln
    ```

- **SS (Socket Stat):**
  ```bash
  ss -tuln
  ```

- **Dig (DNS Query):**
  - **Ubuntu:**
    ```bash
    sudo apt install dnsutils
    dig google.com
    ```

  - **Red Hat:**
    ```bash
    sudo yum install bind-utils -y
    dig google.com
    ```

---

### **4. Firewalls and Security**

#### **Ubuntu:**
- **UFW (Uncomplicated Firewall):**
  - **Check UFW status:**
    ```bash
    sudo ufw status
    ```

  - **Enable UFW:**
    ```bash
    sudo ufw enable
    ```

  - **Allow HTTP and HTTPS:**
    ```bash
    sudo ufw allow http
    sudo ufw allow https
    ```

  - **Allow custom port (e.g., 8080):**
    ```bash
    sudo ufw allow 8080
    ```

  - **Disable UFW:**
    ```bash
    sudo ufw disable
    ```

#### **Red Hat (RHEL/CentOS):**
- **Firewalld:**
  - **Start firewalld:**
    ```bash
    sudo systemctl start firewalld
    ```

  - **Enable firewalld at boot:**
    ```bash
    sudo systemctl enable firewalld
    ```

  - **Allow HTTP and HTTPS:**
    ```bash
    sudo firewall-cmd --zone=public --add-service=http --permanent
    sudo firewall-cmd --zone=public --add-service=https --permanent
    sudo firewall-cmd --reload
    ```

  - **Allow custom port (e.g., 8080):**
    ```bash
    sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent
    sudo firewall-cmd --reload
    ```

---

### **5. Routing and Network Address Translation (NAT)**

#### **Ubuntu & Red Hat:**

- **View Routing Table:**
  ```bash
  ip route
  ```

- **Configure Static Routes:**
  - **Ubuntu:**
    Add the route in the `/etc/netplan/*.yaml` configuration file or use the `ip` command:
    ```bash
    sudo ip route add 10.10.0.0/24 via 192.168.1.1
    ```

  - **Red Hat:**
    Add the route in the `/etc/sysconfig/network-scripts/route-<interface>` file:
    ```bash
    sudo vi /etc/sysconfig/network-scripts/route-eth0
    ```
    Add the route:
    ```bash
    10.10.0.0/24 via 192.168.1.1
    ```

    To apply it immediately, use the `ip` command as well:
    ```bash
    sudo ip route add 10.10.0.0/24 via 192.168.1.1
    ```

---

### **6. Network Services and Configuration**

#### **NFS (Network File System)**

- **Ubuntu & Red Hat:**

  1. **Install NFS client:**
     - **Ubuntu:**
       ```bash
       sudo apt install nfs-common
       ```

     - **Red Hat:**
       ```bash
       sudo yum install nfs-utils -y
       ```

  2. **Mount the NFS share:**
     ```bash
     sudo mount -t nfs <nfs-server-ip>:/path/to/share /mnt
     ```

  3. **Unmount the NFS share:**
     ```bash
     sudo umount /mnt
     ```

#### **Samba (SMB/CIFS)**

- **Ubuntu & Red Hat:**

  1. **Install Samba client:**
     - **Ubuntu:**
       ```bash
       sudo apt install cifs-utils
       ```

     - **Red Hat:**
       ```bash
       sudo yum install cifs-utils -y
       ```

  2. **Mount the SMB share:**
     ```bash
     sudo mount -t cifs //server/share /mnt -o username=user,password=pass
     ```

  3. **Unmount the SMB share:**
     ```bash
     sudo umount /mnt
     ```

---

### **7. Managing Network Services**

#### **Network Time Protocol (NTP)**

- **Ubuntu & Red Hat:**

  - **Install chrony (recommended for Ubuntu and RHEL 7+):**
    ```bash
    sudo apt install chrony     # Ubuntu
    sudo yum install chrony -y  # RHEL
    ```

  - **Start chrony service:**
    ```bash
    sudo systemctl start chronyd
    sudo systemctl enable chronyd
    ```

  - **Check synchronization status:**
    ```bash
    chronyc tracking
    ```

#### **DNS Configuration**

- **Ubuntu & Red Hat:**

  Edit the `/etc/resolv.conf` file to set DNS servers:

  ```bash
  sudo vi /etc/resolv.conf
  ```

  Example DNS configuration:

  ```bash
  nameserver 8.8.8.8
  nameserver 8.8.4.4
  ```

---

### **8. Network Performance Monitoring**

#### **Bandwidth Usage**

- **Ubuntu & Red Hat:**

  1. **Install `nload` for bandwidth monitoring:**
     - **Ubuntu:**
       ```bash
       sudo apt install nload
       ```

     - **Red Hat:**
       ```bash
       sudo yum install nload -y
       ```

  2. **Run `nload` to view live network traffic:**
     ```bash
     nload
     ```

  3. **Install `iftop` to monitor bandwidth usage by connection:**
     - **Ubuntu:**
       ```bash
       sudo apt install iftop
       ```

     - **Red Hat:**
       ```bash
       sudo yum install iftop -y
       ```

  4. **Run `iftop` to monitor live traffic:**
     ```bash
     sudo iftop
     ```

---

## Real-Time Example: Configuring and Monitoring Networking for a Web Application
<hr/>

In the context of **DevOpsNetworking**, focusing solely on **network configuration**, **firewall management**, **monitoring**, and **troubleshooting**, here’s how we can manage and troubleshoot networking aspects in a **real-time project** involving both **Ubuntu** and **Red Hat (RHEL)** systems. 


This example will focus on configuring networking settings, monitoring traffic, managing firewalls, and troubleshooting connectivity issues. We’ll consider the deployment of a **web application** and focus on networking tasks.

---

### **1. Network Interface Configuration**

#### **Ubuntu:**

1. **Checking Network Interfaces:**
   - To view all network interfaces:
     ```bash
     ip a
     ```

2. **Configuring Static IP Using Netplan:**
   - Netplan is the default network configuration tool in Ubuntu (starting from 18.04). The configuration files are stored in `/etc/netplan/`.
   - Example of a static IP configuration:
     ```yaml
     network:
       version: 2
       renderer: networkd
       ethernets:
         eth0:
           dhcp4: no
           addresses:
             - 192.168.1.100/24
           gateway4: 192.168.1.1
           nameservers:
             addresses:
               - 8.8.8.8
               - 8.8.4.4
     ```

   - Apply changes:
     ```bash
     sudo netplan apply
     ```

#### **Red Hat (RHEL/CentOS):**

1. **Checking Network Interfaces:**
   - To view all network interfaces:
     ```bash
     ip a
     ```

2. **Configuring Static IP Using Network-Scripts:**
   - Network interface configuration files are located in `/etc/sysconfig/network-scripts/`.
   - For example, configuring `eth0` for static IP:
     ```bash
     sudo vi /etc/sysconfig/network-scripts/ifcfg-eth0
     ```

     Example configuration:
     ```bash
     TYPE=Ethernet
     BOOTPROTO=static
     NAME=eth0
     DEVICE=eth0
     ONBOOT=yes
     IPADDR=192.168.1.100
     NETMASK=255.255.255.0
     GATEWAY=192.168.1.1
     DNS1=8.8.8.8
     DNS2=8.8.4.4
     ```

   - Restart network:
     ```bash
     sudo systemctl restart network
     ```

---

### **2. Firewall Configuration**

In a **DevOps** environment, firewall settings are crucial for ensuring that only specific traffic can access the servers, such as allowing only HTTP and HTTPS traffic for a web server like **Nginx**.

#### **Ubuntu:**

1. **Check UFW (Uncomplicated Firewall) Status:**
   ```bash
   sudo ufw status
   ```

2. **Allow HTTP and HTTPS Traffic:**
   - To allow **HTTP (80)** and **HTTPS (443)** traffic:
     ```bash
     sudo ufw allow 'Nginx Full'
     ```

3. **Enable UFW:**
   ```bash
   sudo ufw enable
   ```

4. **Deny All Other Traffic:**
   - To ensure that only allowed traffic is accepted:
     ```bash
     sudo ufw default deny incoming
     sudo ufw default allow outgoing
     ```

5. **Reload UFW to Apply Changes:**
   ```bash
   sudo ufw reload
   ```

#### **Red Hat (RHEL/CentOS):**

1. **Start and Enable Firewalld:**
   ```bash
   sudo systemctl start firewalld
   sudo systemctl enable firewalld
   ```

2. **Allow HTTP and HTTPS Traffic:**
   ```bash
   sudo firewall-cmd --zone=public --add-service=http --permanent
   sudo firewall-cmd --zone=public --add-service=https --permanent
   sudo firewall-cmd --reload
   ```

3. **Allow Custom Ports (e.g., port 8080):**
   ```bash
   sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent
   sudo firewall-cmd --reload
   ```

4. **Check Firewalld Status:**
   ```bash
   sudo firewall-cmd --state
   ```

---

### **3. Traffic Monitoring and Network Performance**

#### **Ubuntu & Red Hat:**

Monitoring network traffic is essential in a **DevOps** environment to ensure the web application is not overloaded, and resources are used efficiently.

1. **Using `nload` for Bandwidth Monitoring:**
   - **Install nload:**
     - **Ubuntu:**
       ```bash
       sudo apt install nload
       ```
     - **Red Hat:**
       ```bash
       sudo yum install nload -y
       ```

   - **Monitor Network Traffic:**
     ```bash
     nload
     ```

2. **Using `iftop` for Monitoring Active Connections:**
   - **Install iftop:**
     - **Ubuntu:**
       ```bash
       sudo apt install iftop
       ```
     - **Red Hat:**
       ```bash
       sudo yum install iftop -y
       ```

   - **Monitor Live Traffic:**
     ```bash
     sudo iftop
     ```

3. **Using `ss` for Socket Statistics:**
   - **View Listening Ports and Established Connections:**
     ```bash
     ss -tuln
     ```

4. **Check Current Network Usage:**
   - **Using `netstat`:**
     ```bash
     sudo netstat -tuln
     ```

5. **Monitoring Logs for Network Issues:**
   - Logs can be found in `/var/log/syslog` (Ubuntu) or `/var/log/messages` (Red Hat).
   - Example command to check logs:
     ```bash
     sudo tail -f /var/log/syslog    # For Ubuntu
     sudo tail -f /var/log/messages  # For Red Hat
     ```

---

### **4. Troubleshooting Network Issues**

Troubleshooting network connectivity is an essential skill in a **DevOps** environment. Here are some common troubleshooting techniques.

#### **Ubuntu & Red Hat:**

1. **Ping Test:**
   - Check connectivity to an external server (e.g., Google DNS):
     ```bash
     ping 8.8.8.8
     ```

2. **Traceroute:**
   - Track the path packets take to reach a remote server:
     - **Ubuntu:**
       ```bash
       sudo apt install traceroute
       traceroute google.com
       ```
     - **Red Hat:**
       ```bash
       sudo yum install traceroute -y
       traceroute google.com
       ```

3. **Check DNS Resolution:**
   - Verify if DNS is resolving correctly:
     ```bash
     dig google.com
     ```

4. **Check Routing Table:**
   - **Ubuntu & Red Hat:**
     ```bash
     ip route
     ```

5. **Check Network Interface Status:**
   - **Ubuntu & Red Hat:**
     ```bash
     ip link show
     ```

---

### **5. Configuring Network Routes**

In more complex **DevOps** environments, you may need to configure specific network routes for various applications and services.

#### **Ubuntu & Red Hat:**

1. **Adding Static Routes:**
   - **For Ubuntu (via Netplan)**:
     - Edit the `/etc/netplan/*.yaml` file to add a static route:
       ```yaml
       network:
         version: 2
         renderer: networkd
         ethernets:
           eth0:
             routes:
               - to: 10.10.0.0/24
                 via: 192.168.1.1
       ```
     - Apply changes:
       ```bash
       sudo netplan apply
       ```

   - **For Red Hat (via ifcfg file)**:
     - Edit the `/etc/sysconfig/network-scripts/route-eth0` file:
       ```bash
       10.10.0.0/24 via 192.168.1.1
       ```
     - Restart network service:
       ```bash
       sudo systemctl restart network
       ```


**With this We've covered all required Linux Networking concepts with practical aspect.**

<hr/>