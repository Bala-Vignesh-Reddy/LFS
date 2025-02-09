# Linux from Scratch - My journey

Journey of creating **`techie-0.1`**

## Overview

This repo documents my journey in building a LFS system (12.2). I am using linux from last 3 years now.. and i loved using it.. tried various distros from ubuntu to fedora and the best experience using windows tilling manager (i3wm) it was very fast and i loved it.. 

I was amazed how the linux is working deep inside.. so i thought of giving it a try to learn and explore about it more... then i got to know about LFS(linux from scratch book) by Gerard Beekmans.. 

Started learning and it was my primary resource for this project.. book gives  a great overview of building the stuff.. 

## Why LFS?

- **Deep Learning:** Understanding every part of linux system
- **customization:** Building a system according to needs..
- **Control:** Learning how to complie and configure each component.

## Prerequisites and References

- [Software Building HOWTO](https://tldp.org/HOWTO/Software-Building-HOWTO.html)
- [LFS Errata](https://www.linuxfromscratch.org/lfs/errata/12.2/)
- [LFS Security Advisories](https://www.linuxfromscratch.org/lfs/advisories/)

---

## Strucutre 
- **PART-I:** Steps to proceed with lfs installation
- **PART-II:** tells how to perpare for the biulding process-making parition, downloading packages and compiling tools.. 
- **PART-III:** provides instructions for building the tools needed for constructing the final LFS system.
- **PART-IV:** building of the LFS system—compiling and installing all the packages one by one, setting up the boot scripts, and installing the kernel.


## Day-by-Day notes

### Day 0: Introduction and Preface

- **Introduction:**
    - Began with reading the preface of book, prerequisites, FHS(file hierarchy standard - 3.0), LSB (linux standard base - 5.0) 
    - Going through the docs and understanding tools
- **Resources:**
    The primary reference is the LFS book.

_Read more in [docs/day0-intro.md](docs/day0.md)._

 ---
 ### Day 1: Real work begins

 - **Setting the host environment:**
    - it is recommended to build in a virtual environment.. inside any linux distro.. 
    - But for me.. i dual booted my laptop installed Ubuntu 24.04 
- **Partitioning:** 
    - create a new parition inside the ubuntu (approx 20-30GB recommended) for lfs and  2gb for swap partition.. 
    - to do this task you can use the `cfdisk` or use the disk partition if you have access to the gui. 
    - format the parition to ext4


_Read more in [docs/day1-setup.md](docs/day1.md)._

---
### Day 2: Building and Compiling packages.. 
- Cross-compilation is the process of building executable code for a platform different from the one on which the compiler is running. It is essential for developing software for embedded systems or different architectures.

- **Important Steps**:
    1. Place all the source and patches inside the same directory.. for me it was `/mnt/lfs/sources`... 
    2. change to the `/mnt/lfs/sources` directory.. .
    3. for each package
        - use the `tar` cmd to extract the packages.. 

            `tar -xvJf <pacakage_name>` for .xz extension

            `tar -xzvf <package_name>` for .gz extension
        - Change the directory created when the package is extracted.. 
        - Follow the instructions for building the package.
        - Change back to the sources directory when build is complete
        - Delete the extracted source directory unless instructed otherwise..

---

### Day 3: Entering chroot and building temporary tools

- create an vritual file system.. 
- Applications running in userspace utilize various file systems created by the kernel to communicate with the kernel itself. These file systems are virtual: no disk space is used for them. The content of these file systems resides in memory. 

- Entering the chroot environment and creating all the necessary directories and symlinks 
- Installing all the tools and package inside this environment..

- **Time took for compiling**
    - This may varying according to the hardware you are working upon.. my laptop specs i5-6500U, 8gb of RAM, provided 3 cores for building..  took around 15+ hr to compile and install all the packages.. 
    - and approx.. 26hr+ to complete this project.. 

---

## Integrating LFS into a Triple-Boot GRUB Configuration
In my triple-boot setup, I use Ubuntu’s GRUB as the central boot loader to manage booting into three operating systems: Windows, Ubuntu, and my custom-built Linux From Scratch (LFS) system. Here’s how I did it:

### 1. Preparing the LFS Partition

- **Partitioning & Formatting:**  
  I created a dedicated partition for LFS (e.g., `/dev/sda6`) and formatted it with **ext4**. This partition houses the LFS root filesystem including its `/boot` directory, where the kernel image resides.

- **Kernel Installation:**  
  After building the system, I installed my custom kernel (named `vmlinuz-6.10.5-lfs-12.2`) into the `/boot` directory of the LFS partition.

### 2. Configuring Ubuntu’s GRUB for Triple Boot

Since Ubuntu is already installed on my machine and manages GRUB in UEFI mode, I added a custom entry for LFS to Ubuntu’s GRUB configuration. This centralizes boot management, allowing me to choose between Windows, Ubuntu, and LFS from one menu.

- **Creating the Custom GRUB Entry:**  
  I edited the file `/etc/grub.d/40_custom` in Ubuntu and added the following entry:

  ```bash
  menuentry "Linux From Scratch" {
      insmod ext2
      # In UEFI with GPT, (hd0,gpt6) represents /dev/sda6 – the LFS partition
      set root='(hd0,gpt6)'
      # Load the LFS kernel. The "ro" parameter mounts the root filesystem as read-only initially.
      linux /boot/vmlinuz-6.10.5-lfs-12.2 root=/dev/sda6 ro
  }
  ```

  **Explanation of Key Directives:**

  - **`insmod ext2`:**  
    Loads the module to read ext2/3/4 filesystems (ext4 is compatible via this module).

  - **`set root='(hd0,gpt6)'`:**  
    Tells GRUB to use the sixth partition on the first disk (GPT notation) as the root for LFS.

  - **`linux /boot/vmlinuz-6.10.5-lfs-12.2 root=/dev/sda6 ro`:**  
    Directs GRUB to load the LFS kernel from `/boot`, setting `/dev/sda6` as the root filesystem. The `ro` flag means that the kernel mounts the root filesystem as read-only initially—this is standard practice to allow for filesystem checks and safe transition to read-write mode later.


- **Updating GRUB:**  
  After adding the custom entry, I ran:

  ```bash
  sudo update-grub
  ```

  This command regenerated the GRUB configuration (`/boot/grub/grub.cfg`), incorporating the new LFS entry along with automatically detected entries for Ubuntu and Windows.

### 3. Finalizing the Build and Cleanup

- **Exiting the Build Environment:**  
  Once all packages were built and installed, I exited the chroot environment and unmounted all temporary virtual filesystems (like `/proc`, `/sys`, and `/dev`), as recommended by the LFS book. This cleanup ensured that the LFS partition was in a stable state before rebooting.

- **Rebooting the System:**  
  With everything configured and cleaned up, I rebooted the machine. On startup, Ubuntu’s GRUB menu appeared, displaying options for Windows, Ubuntu, and “Linux From Scratch.”

### 4. Boot Verification

- **Boot Process:**  
  Selecting the “Linux From Scratch” entry causes GRUB to:
  - Load the kernel from `/boot/vmlinuz-6.10.5-lfs-12.2` on `/dev/sda6`.
  - Pass the correct root parameter so that the kernel mounts the LFS filesystem.
  - Load the initial RAM disk (if configured) and proceed with the boot sequence.
  
- **Outcome:**  
  After these steps, the LFS system boots to the “login:” prompt, indicating a successful build and integration into the triple-boot setup.


---

### It was great experience building this.. and will continue with it if time premits.. we'll add some additional tools and try to make it crazyy.. 