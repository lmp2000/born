*This project has been created as part of the 42 curriculum by lude-jes.*

# Born2beroot

## Description
Born2beroot is a system administration project that serves as an introduction to virtualization and server configuration. The goal of this project is to create a secure, strictly configured virtual machine.

This exercise involves setting up a server (without a graphical interface), focusing on partitioning with LVM, implementing strict password policies, configuring a firewall, setting up SSH access, and managing user permissions.

## Instructions
Since this is a Virtual Machine project, the repository primarily contains the signature of the virtual disk.

1.  **Environment**: This project was built using **VirtualBox**.
2.  **Verification**:
    * Clone this repository.
    * Locate the `signature.txt` file.
    * Get the SHA1 signature of the provided `.vdi` (Virtual Disk Image) or machine snapshot.
    * Ensure the signature matches the content of `signature.txt`.
3.  **Running the Machine**:
    * Open VirtualBox.
    * Start the VM named `lude-jes42` (or the specific name provided during evaluation).
    * Log in using the non-root user credentials created during setup.

## Project Description & Design Choices

### 1. Operating System Choice: Debian
For this project, **Debian** was selected over Rocky Linux.

* **Why Debian?** Debian is known for its stability and its vast package manager (`apt`). It is widely used in the industry for servers and is generally considered more "user-friendly" for setting up personal servers compared to the enterprise-focused structure of RHEL/Rocky.
* **Design Choices**:
    * **Partitioning**: Manual partitioning was performed using **LVM (Logical Volume Manager)** to allow for flexible resizing of partitions. The structure includes separate partitions for `/home`, `/var`, `/srv`, `/tmp`, and `/var/log` to prevent system crashes if one partition fills up.
    * **Security Policies**: Strong password policies were enforced via `libpam-pwquality`.
    * **User Management**: A distinct root user and a standard user with sudo privileges were created.
    * **Services**: Only the essential services were installed (SSH server) to minimize the attack surface.

### 2. Comparisons

During the setup process, several technical choices had to be made. Below is a comparison of the available options:

#### Debian vs. Rocky Linux
| Feature | Debian | Rocky Linux |
| :--- | :--- | :--- |
| **Family** | Debian-based (`.deb`) | RHEL-based (`.rpm`) |
| **Package Manager** | `apt` / `dpkg` | `dnf` / `yum` |
| **Security Module** | AppArmor (Default) | SELinux (Default) |
| **Updates** | Known for stability (Slow release cycle) | Enterprise-grade stability (Clone of RHEL) |
| **Community** | Massive community support | Focused on enterprise replacements for CentOS |

#### AppArmor vs. SELinux
| Feature | AppArmor (Used) | SELinux |
| :--- | :--- | :--- |
| **Concept** | Path-based access control. | Inode/Label-based access control. |
| **Ease of Use** | Generally easier to configure and read. | Very complex and granular. |
| **Implementation** | Profiles are applied to specific binaries. | Policies are applied to file system labels. |

#### UFW vs. Firewalld
| Feature | UFW (Used) | Firewalld |
| :--- | :--- | :--- |
| **Name** | Uncomplicated Firewall. | Firewall Daemon. |
| **Interface** | Simple command-line interface for `iptables`. | Uses zones and services dynamically. |
| **Best for** | Simple server setups (Ubuntu/Debian standard). | Complex networking environments (RHEL standard). |

#### VirtualBox vs. UTM
| Feature | VirtualBox (Used) | UTM |
| :--- | :--- | :--- |
| **Architecture** | x86 virtualization (Type 2 Hypervisor). | QEMU-based (Great for Apple Silicon/ARM). |
| **Platform** | Cross-platform (Windows, Mac, Linux). | MacOS / iOS specific. |
| **Performance** | Industry standard for x86. | Better emulation on M1/M2 chips. |
| **Cost** | Free and Open Source (Oracle). | Free (Open Source), Paid version on AppStore. |

## Resources

### References
* **Primary Tutorial**: [Born2beroot Guide by noreply](https://noreply.gitbook.io/born2beroot/) - Used as a comprehensive guide for step-by-step configuration.
* [Debian Administrator's Handbook](https://debian-handbook.info/)
* [UFW Documentation](https://help.ubuntu.com/community/UFW)

### AI Usage
AI tools (ChatGPT/Claude) were used to:
1.  Clarify the differences between AppArmor and SELinux.
2.  Generate the comparison tables found in the README.
3.  Proofread the configuration scripts to ensure syntax accuracy.