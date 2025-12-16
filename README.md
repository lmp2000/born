*This project has been created as part of the 42 curriculum by lude-jes.*

# Born2beroot

## Description
**Born2beroot** is a system administration project that serves as an introduction to virtualization and server configuration. The goal is to create a secure virtual machine from scratch using strict rules, without a graphical interface.

This project covers essential topics such as:
* **Virtualization:** Creating and managing a VM.
* **Partitioning:** implementing LVM (Logical Volume Manager).
* **Security:** Configuring firewalls (UFW), password policies, and SSH access.
* **User Management:** Implementing sudo privileges and strict group management.
* **Monitoring:** Creating a script to report system health.

## Instructions
This project involves setting up a virtual machine with specific configurations. Below is a high-level summary of the installation and execution process.

### 1. VM Creation
* **Software:** VirtualBox
* **OS:** Debian (latest stable version)
* **Resources:** * Disk: 30GB (Standard)
    * RAM: 4096MB (Recommended)
    * CPU: 2 vCPUs

### 2. Installation & Partitioning
* Launch the Debian ISO in VirtualBox.
* **Hostname:** `lude-jes42`
* **Partitioning:** Manual partitioning using LVM is required:
    * `/boot` (Primary, ~500MB)
    * LVM Group (`LVMGroup`) containing logical volumes for `/`, `/home`, `/var`, `/srv`, `/tmp`, and `/var/log`.

### 3. Setup & Configuration
Once the OS is installed, perform the following (via root or sudo):
1.  **Sudo:** Install `sudo` and configure the group `user42`.
2.  **SSH:** Install `openssh-server`, change port to `4242`, and disable root login (`PermitRootLogin no`).
3.  **UFW:** Install `ufw`, enable it, and allow port 4242.
4.  **Password Policy:** Configure strict rules in `/etc/login.defs` and `/etc/pam.d/common-password`.
5.  **Monitoring:** Place the `monitoring.sh` script in `/usr/local/bin/` and configure `crontab` to run it every 10 minutes.

### 4. Execution
To verify the project is running:
* Start the VM in VirtualBox.
* Log in with your created user.
* Run the monitoring script manually or wait for the cron job to broadcast the system info to all terminals.

## Resources
* **Main Tutorial:** [Born2beroot Guide (GitBook)](https://noreply.gitbook.io/born2beroot/)
* **Debian Documentation:** [https://www.debian.org/doc/](https://www.debian.org/doc/)
* **UFW Manual:** [https://wiki.ubuntu.com/UncomplicatedFirewall](https://wiki.ubuntu.com/UncomplicatedFirewall)

### AI Usage
AI tools were used to:
* **Generate Examples:** AI was prompted to provide standard configuration examples for LVM partitioning and SSH setup to cross-reference with official documentation.
* **Comparisons:** AI was used to synthesize key differences between Linux distributions and security modules for the project description below.

---

## Project Description & Technical Choices

### Operating System: Debian
For this project, **Debian** was chosen over Rocky Linux.
* **Pros:** Debian is renowned for its stability ("Debian Stable"), strict free software adherence, and massive package repository (`apt`). It has a huge community and documentation base, making it ideal for learning system administration.
* **Cons:** Software packages in the "Stable" branch can be older than in other distributions.

#### Design Choices
* **Partitioning:** LVM was used to allow for flexible resizing of partitions without downtime.
* **Security:**
    * **SSH:** Root login is disabled to prevent brute-force attacks on the superuser account. Port 4242 is used to avoid standard port 22 automated scripts.
    * **Password Policy:** Enforced complexity (uppercase, numbers, no username) and expiration to ensure credential hygiene.
* **User Management:** The `sudo` group limits administrative access to authorized users only, with all actions logged for auditing.

### Technical Comparisons

#### Debian vs Rocky Linux
| Feature | Debian | Rocky Linux |
| :--- | :--- | :--- |
| **Family** | Debian-based (`.deb`) | RHEL-based (`.rpm`) |
| **Package Manager** | `apt` (Advanced Package Tool) | `dnf` / `yum` |
| **Philosophy** | Community-driven, stability focused. | Enterprise-focused, binary-compatible with RHEL. |
| **Primary Use** | General purpose servers, desktops. | Enterprise servers, CentOS replacement. |

#### AppArmor vs SELinux
**Choice: AppArmor**
* **AppArmor (Application Armor):** Uses path-based profiles to restrict programs. It is generally considered easier to configure and learn for beginners. It allows specific applications to be restricted without requiring a complete system overhaul.
* **SELinux (Security-Enhanced Linux):** Uses a labeling system (inode-based) for Mandatory Access Control. It is extremely granular and powerful but has a steep learning curve and can be difficult to troubleshoot.

#### UFW vs firewalld
**Choice: UFW**
* **UFW (Uncomplicated Firewall):** A simplified interface for `iptables`. It is designed to be easy to use with simple commands (e.g., `ufw allow 4242`). Ideal for single-server setups where complex zoning isn't required.
* **firewalld:** A dynamic firewall manager with support for network "zones" (e.g., public, home, work). It allows changing rules without breaking existing connections but is more complex to configure than UFW.

#### VirtualBox vs UTM
**Choice: VirtualBox**
* **VirtualBox:** A mature, open-source x86 virtualization tool owned by Oracle. It is the industry standard for educational virtualization, offers snapshot features, and has broad compatibility across host OSs (Windows, Linux, macOS Intel).
* **UTM:** A virtualization tool designed specifically for macOS (especially Apple Silicon/M1/M2 chips). It is often used as an alternative when VirtualBox architecture compatibility is an issue on newer Macs.