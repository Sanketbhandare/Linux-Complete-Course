# Create a Linux VM in 1 Minute Using Vagrant

This guide will show you how to create a Linux VM in just one minute using Vagrant. Make sure to follow the steps below to get started quickly.

## Prerequisites:
- **Vagrant**: Ensure you have Vagrant installed.  
  [Download Vagrant](https://www.vagrantup.com/downloads)
- **VirtualBox** (or another supported provider): Make sure you have VirtualBox installed as the default provider.  
  [Download VirtualBox](https://www.virtualbox.org/)
- **VS Code** (or any other text editor): Used for editing the `Vagrantfile`.  
  [Download VS Code](https://code.visualstudio.com/Download)


## Steps to Create a Linux VM with Vagrant

### 1. Initialize a Vagrant Project:
   Open the terminal and run the following command:
   ```bash
   vagrant init
   ```
   This will create a `Vagrantfile` in the current directory.

### 2. Edit the Vagrantfile:
   Open the `Vagrantfile` in a text editor (e.g., VS Code) and specify a Linux box (e.g., `ubuntu/bionic64`):
   ```ruby
   Vagrant.configure("2") do |config|
     config.vm.box = "ubuntu/bionic64"  
   end
     # You can change to any  Linux distro, like "ubuntu/focal64"
     # config.vm.box = "centos/8"         # For CentOS 8
     # config.vm.box = "redhat/8"         # For RedHat 8
     # config.vm.box = "fedora/33"        # For Fedora 33
     # Any other distribution,  you can find it here.
     # https://portal.cloud.hashicorp.com/vagrant/discover

   end
   ```

### 5. Start the VM:
   In the same directory as the `Vagrantfile`, run:
   ```bash
   vagrant up
   ```
   This command will automatically download the Ubuntu box (if not already downloaded) and start the virtual machine.

### 6. Access the VM:
   Once the VM is up and running, SSH into it with:
   ```bash
   vagrant ssh
   ```
   Now you are connected to your Linux VM.

## Conclusion:
You’ve successfully created and connected to a Linux VM using Vagrant in under a minute. This method is quick and ideal for setting up development environments.
```

Now, **Steps 4 to 6** and the **Conclusion** are properly formatted in Markdown, including the appropriate code blocks for terminal commands and configuration.
