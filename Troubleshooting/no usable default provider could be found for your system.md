

# Resolving VirtualBox and Vagrant Issues on Rocky Linux 9.4

The errors you're encountering are primarily due to missing dependencies required to build VirtualBox kernel modules and VirtualBox not being properly recognized by Vagrant. Below is a comprehensive guide to resolve these issues and get `vagrant up` working smoothly on your Rocky Linux 9.4 system.

## 1. Install Required Build Dependencies

VirtualBox requires certain development tools and kernel headers to build its kernel modules. Follow these steps to install them:

### 1.1. Update Your System

First, ensure your system packages are up-to-date:

```bash
sudo dnf update -y
```

### 1.2. Install Development Tools and Kernel Headers

Install the necessary packages (`gcc`, `make`, `perl`, `kernel-devel`, and `kernel-headers`) that are required to build VirtualBox kernel modules:

```bash
sudo dnf install -y gcc make perl kernel-devel kernel-headers
```

**Note:** It's crucial that the `kernel-devel` and `kernel-headers` packages match your current running kernel version. Verify your kernel version with:

```bash
uname -r
```

If your current kernel version is, for example, `5.14.0-427.13.1.el9_4.x86_64`, ensure you install the corresponding `kernel-devel` package:

```bash
sudo dnf install -y kernel-devel-$(uname -r) kernel-headers-$(uname -r)
```

---

## 2. Rebuild VirtualBox Kernel Modules

Once the dependencies are installed, rebuild the VirtualBox kernel modules:

```bash
sudo /sbin/vboxconfig
```

**Possible Outcomes:**

- **Success:** You should see messages indicating that VirtualBox services are stopping, starting, and that kernel modules are being built successfully.
- **Failure:** If you encounter errors, especially related to Secure Boot, proceed to the next section.

---

## 3. Handle Secure Boot (If Applicable)

If your system uses **EFI Secure Boot**, it may prevent the loading of unsigned kernel modules like those required by VirtualBox. You have two options:

### 3.1. Disable Secure Boot

**Warning:** Disabling Secure Boot can reduce the security of your system. Proceed only if you're comfortable with this change.

1. **Reboot Your System:**
    
    ```bash
    sudo reboot
    ```
    
2. **Enter BIOS/UEFI Settings:**
    - Typically by pressing keys like `F2`, `F10`, `Del`, or `Esc` during boot (the exact key depends on your hardware).
3. **Navigate to the Secure Boot Settings:**
    - Look for **Secure Boot** in the security or boot sections.
4. **Disable Secure Boot:**
    - Change the setting to **Disabled**.
5. **Save and Exit:**
    - Save the changes and reboot your system.
6. **Rebuild VirtualBox Modules:**
    
    After rebooting, run:
    
    ```bash
    sudo /sbin/vboxconfig
    ```

### 3.2. Sign VirtualBox Kernel Modules

If you prefer to keep Secure Boot enabled, you'll need to sign the VirtualBox kernel modules. This process is more involved and requires generating your own signing keys and enrolling them into the system's MOK (Machine Owner Key). Detailed instructions can be found in the [VirtualBox documentation](https://www.virtualbox.org/manual/ch02.html#idm140487455925056).

**Note:** This is an advanced procedure and is beyond the scope of this guide. Disabling Secure Boot is generally simpler for development environments.

---

## 4. Verify VirtualBox Installation

Ensure that VirtualBox is correctly installed and its version is recognized:

```bash
VBoxManage --version
```

**Expected Output:**

A version number, e.g., `7.0.6r137139`, indicating VirtualBox is installed.

