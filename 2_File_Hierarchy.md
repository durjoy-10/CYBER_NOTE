# Linux File Hierarchy

The Linux file system follows a hierarchical directory structure, known as the Filesystem Hierarchy Standard (FHS). Below is a list of the most important directories and their purposes:

## `/` - Root Directory
- The top-level directory in the Linux file system.
- All other directories and files are located under this directory.

## `/bin` - Essential User Binaries
- Contains essential command binaries (executable files) that are required for system booting and repair.
- Examples: `ls`, `cp`, `mv`, `cat`, etc.

## `/boot` - Boot Loader Files
- Contains files required for the boot process, such as the kernel, initramfs, and bootloader configuration files.

## `/dev` - Device Files
- Contains device files that represent hardware components (e.g., `/dev/sda` for a hard disk).

## `/etc` - Configuration Files
- Contains system-wide configuration files and scripts.
- Examples: `/etc/passwd`, `/etc/hosts`, `/etc/fstab`.

## `/home` - User Home Directories
- Contains personal directories for users (e.g., `/home/username`).
- Each user has their own subdirectory for storing personal files.

## `/lib` - Essential Shared Libraries
- Contains shared libraries required by the binaries in `/bin` and `/sbin`.

## `/media` - Removable Media Mount Points
- Used as a mount point for removable media like USB drives, CDs, and DVDs.

## `/mnt` - Temporary Mount Points
- Used for temporarily mounting file systems (e.g., network shares or external drives).

## `/opt` - Optional Software Packages
- Contains additional software packages installed by the user or third-party applications.

## `/proc` - Process and Kernel Information
- A virtual filesystem that provides information about running processes and system resources.
- Examples: `/proc/cpuinfo`, `/proc/meminfo`.

## `/root` - Root User's Home Directory
- The home directory for the root user (superuser).

## `/run` - Runtime Data
- Stores runtime data for processes and services since the last boot (e.g., PID files, socket files).

## `/sbin` - System Binaries
- Contains essential system administration binaries (e.g., `fdisk`, `ifconfig`, `init`).

## `/srv` - Service Data
- Contains data for services provided by the system (e.g., web server files, FTP files).

## `/tmp` - Temporary Files
- Used for storing temporary files. Files in this directory are often deleted upon reboot.

## `/usr` - User Utilities and Applications
- Contains user-installed software, libraries, and documentation.
- Subdirectories include:
  - `/usr/bin`: Non-essential user binaries.
  - `/usr/lib`: Libraries for user binaries.
  - `/usr/local`: Locally installed software.
  - `/usr/share`: Architecture-independent data (e.g., documentation, fonts).

## `/var` - Variable Data Files
- Contains files that are expected to grow in size (e.g., logs, databases, emails).
- Subdirectories include:
  - `/var/log`: System log files.
  - `/var/cache`: Application cache data.
  - `/var/lib`: Application state information.
  - `/var/spool`: Queued data (e.g., print jobs, mail).

## `/sys` - Kernel and System Information
- A virtual filesystem that provides information about the kernel, hardware, and system configuration.

## `/lost+found` - Recovered Files
- Used by the `fsck` utility to store recovered files after a file system check.

---

This hierarchy ensures a standardized and organized file system, making it easier to manage and navigate Linux systems.
