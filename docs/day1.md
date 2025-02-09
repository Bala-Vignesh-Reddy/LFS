# Day 1: Real work 

## Summary

    On the first day of building my Linux From Scratch (LFS) system, I focused on setting up the environment and preparing the host system. 

### Creating the LFS Partition
    allocated around 30gb of space, ext4 format.. and 2gb for swap parition
used `cfdisk` cmd to do that.. 

### Mounting the Partition
    After creating the partition, I mounted it to the `/mnt/lfs` directory.
    `export LFS=/mnt/lfs` 
    LFS will be used


### Setting Up the Environment
    I configured the necessary environment variables and created the directory structure required for the build process. This includes setting the `LFS` variable to the mount point

### Version Check Script
    `version-check.sh` in scripts is used to verify that all required tools are available and meet the minimum version requirements. This script checks the versions of critical development tools such as `bash`, `gcc`, `make`, and others to ensure compatibility.

### Downloading the Source Packages
    I compiled a list of packages and patches with their download links `wget-list` inside packages directory..  This step is essential for downloading all the necessary source files and patches 

### Creating necessary directories and added group and user.. 
    /usr, /bin, /tools, and many others.. 
    created various scripts like .bashrc, .bash_profile and more.. 

Got to know about `nproc`.. it is used to find the number of cores available in our machine.. because we have to set it up before start installing and building packagaes. 

**SBU (standard built unit):** the time it takes to compile using one core is what we will refer to as the Standard Build Unit or SBU.

[Back](../README.md)