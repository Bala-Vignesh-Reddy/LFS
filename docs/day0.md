# Day 0: Introduction to LFS Building Journey

## POSIX.1-2008
    POSIX.1-2008 (also known as POSIX.1-2017) defines a standard operating system interface and environment, including a command interpreter (or “shell”), and common utility programs to support applications portability at the source code level.

## FHS-3.0
    The Filesystem Hierarchy Standard (FHS) defines the directory structure and directory contents in Linux operating systems. Here are the main directories:

- `/`: The root directory.
- `/bin`: Essential command binaries.
- `/boot`: Static files of the boot loader.
- `/dev`: Device files.
- `/etc`: Host-specific system configuration.
- `/home`: User home directories.
- `/lib`: Essential shared libraries and kernel modules.
- `/media`: Mount point for removable media.
- `/mnt`: Mount point for temporarily mounted filesystems.
- `/opt`: Add-on application software packages.
- `/proc`: Virtual filesystem providing process and kernel information.
- `/root`: Home directory for the root user.
- `/run`: Runtime variable data.
- `/sbin`: Essential system binaries.
- `/srv`: Data for services provided by the system.
- `/sys`: Virtual filesystem providing system information.
- `/tmp`: Temporary files.
- `/usr`: Secondary hierarchy for read-only user data.
- `/var`: Variable data files.

## LSB 5.0
    The Linux Standard Base (LSB) is a joint project by several Linux distributions under the organizational structure of the Linux Foundation to standardize the software system structure, including the filesystem hierarchy used in the GNU/Linux operating system.

## Rationale for Packages Listed in the LFS Book
    some of the packages necessary to build lfs.. 


#### Bash
This package satisfies an LSB core requirement to provide a Bourne Shell interface to the system. It was chosen over other shell packages because of its common usage and extensive capabilities.

#### Binutils
This package supplies a linker, an assembler, and other tools for handling object files. The programs in this package are needed to compile most of the packages in an LFS system.

#### Coreutils
This package contains a number of essential programs for viewing and manipulating files and directories. These programs are needed for command line file management, and are necessary for the installation procedures of every package in LFS.

#### GCC
This is the Gnu Compiler Collection. It contains the C and C++ compilers as well as several others not built by LFS.

#### Glibc
This package contains the main C library. Linux programs will not run without it.

#### Linux Kernel
This package is the Operating System. It is the Linux in the GNU/Linux environment.

#### Make
This package contains a program for directing the building of packages. It is required by almost every package in LFS.

#### Perl
This package is an interpreter for the runtime language PERL. It is needed for the installation and test suites of several LFS packages.

#### Python 3
This package provides an interpreted language that has a design philosophy emphasizing code readability.

#### Sed
This package allows editing of text without opening it in a text editor. It is also needed by many LFS packages' configure scripts.

#### Tar
This package provides archiving and extraction capabilities of virtually all the packages used in LFS.

#### Texinfo
This package supplies programs for reading, writing, and converting info pages. It is used in the installation procedures of many LFS packages.

#### Util-linux
This package contains miscellaneous utility programs. Among them are utilities for handling file systems, consoles, partitions, and messages.

#### Vim
This package provides an editor. It was chosen because of its compatibility with the classic vi editor and its huge number of powerful capabilities. An editor is a very personal choice for many users. Any other editor can be substituted, if you wish.

#### Zlib
This package contains compression and decompression routines used by some programs.

[Back](../README.md)