
## Virtual Machine Configuration

The following virtual machines are configured in your Vagrant environment:

| System Name            | IP Address      | Ansible Role            |
| ---------------------- | --------------- | ----------------------- |
| `repo.ansi.example.com` | `192.168.56.199` | Ansible Repo Machine     |
| `control.ansi.example.com` | `192.168.56.200` | Ansible Control Node     |
| `node1.ansi.example.com` | `192.168.56.201` | Ansible Managed Node     |
| `node2.ansi.example.com` | `192.168.56.202` | Ansible Managed Node     |
| `node3.ansi.example.com` | `192.168.56.203` | Ansible Managed Node     |
| `node4.ansi.example.com` | `192.168.56.204` | Ansible Managed Node     |


# Setting up a Virtual Ansible Lab with Vagrant and VirtualBox

This guide will help you set up a virtual environment for practicing Ansible using Rocky Linux 9.4, Vagrant, and VirtualBox. By following the steps below, you'll spin up a control node and multiple managed nodes, all configured for Ansible automation.

## Prerequisites

Before getting started, ensure you have the following:

- **Operating System**: This article is based on Rocky Linux 9.4, any Redhat based OS will fit this Guide well
- **VirtualBox**: Version 7.0 (installed later in the guide)
- **Vagrant**: Latest version (installed later in the guide)
- **Hardware**: Minimum 8 GB of RAM for running multiple VMs

---

## Step 1: Setting up the Project Directory

Start by creating a directory for your project.

```bash
mkdir ~/ansible-rocky-linux
cd ~/ansible-rocky-linux
```

Clone the repository that contains the Vagrantfile and necessary configurations:

```bash
git clone https://github.com/k-javaman12/RHCE-lab.git
cd RHCE-lab
```

---

## Step 2: Installing VirtualBox 7.0

1. **Add the VirtualBox Repository**:

   Download and add the VirtualBox repository for your system:

   ```bash
   sudo wget -O /etc/yum.repos.d/virtualbox.repo https://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo
   ```

2. **Install Dependencies and VirtualBox**:

   Install the required dependencies and VirtualBox 7.0:

   ```bash
   sudo dnf install -y gcc make perl dkms kernel-devel kernel-headers
   sudo dnf install -y VirtualBox-7.0
   ```

3. **Rebuild the VirtualBox Kernel Modules**:

   After installation, rebuild the VirtualBox kernel modules to ensure they load properly:

   ```bash
   sudo /sbin/vboxconfig
   ```

4. **Verify VirtualBox Installation**:

   Check the VirtualBox version to ensure it's installed correctly:

   ```bash
   VBoxManage -version
   ```

5. **Install the VirtualBox Extension Pack (Optional)**:

   The VirtualBox Extension Pack provides additional functionality such as support for USB devices. Download it from the official VirtualBox website and install it.

   - [Download VirtualBox Extension Pack 7.0](https://www.virtualbox.org/wiki/Download_Old_Builds_7_0)

---

## Step 3: Installing Vagrant

1. **Add the HashiCorp Repository**:

   Add the official Vagrant repository provided by HashiCorp:

   ```bash
   sudo dnf install -y dnf-plugins-core
   sudo dnf config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
   ```

2. **Install Vagrant**:

   Now, install Vagrant using the newly added repository:

   ```bash
   sudo dnf install -y vagrant
   ```

3. **Verify Vagrant Installation**:

   Check if Vagrant has been installed correctly by running:

   ```bash
   vagrant --version
   ```

---

## Step 4: Initializing and Starting the Vagrant Environment

1. **Initialize the Vagrant Environment**:

   Inside the project directory (`~/ansible-rocky-linux/RHCE-lab`), initialize Vagrant:

   ```bash
   vagrant init
   ```

2. **Start the Virtual Machines**:

   Spin up the entire virtual environment with the following command:

   ```bash
   vagrant up
   ```

   This will download the necessary Rocky Linux 9.4 Vagrant box and start all virtual machines (control node and managed nodes).

3. **Access the Control Node**:

   After the machines are up, SSH into the control node:

   ```bash
   vagrant ssh control
   ```

---

## What's Next?

Once you're inside the **control node**, you can start configuring Ansible and writing playbooks to automate tasks across your managed nodes.

- [Getting Started with Ansible](https://docs.ansible.com/ansible/latest/user_guide/index.html)
- [Ansible Playbooks](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html)

---

## Conclusion

This guide helps you set up a complete virtual environment using **Vagrant**, **VirtualBox**, and **Rocky Linux 9.4**. With the control node and managed nodes in place, you're ready to dive into automating infrastructure tasks with Ansible.

## Detailed guide for this
https://medium.com/@kjavaman12/deploy-simple-html-webpage-with-rocky-linux-9-4-a-comprehensive-guide-93725ba29b72
