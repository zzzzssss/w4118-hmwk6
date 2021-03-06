Implementations of a GPS tagged ext3 file system.
==
Copyright (C) 2014 V. Atlidakis, G. Koloventzos, A. Papancea

COMS-W4118 Columbia University

## MODIFIED/ADDED FILES:

- flo-kernel/arch/arm/configs/flo_defconfig
  defined the CONFIG_GPS_TAGFS preprocessor variable

- flo-kernel/init/Kconfig
  defined the CONFIG_GPS_TAGFS preprocessor variable

- flo-kernel/arch/arm/include/asm/unistd.h
  defined the get_ and set_gps_location syscalls at positions 378 and 379

- flo-kernel/arch/arm/kernel/calls.S
  added get_ and set_gps_location syscalls to the syscall table
  at positions 378 and 379

- flo-kernel/include/linux/syscalls.h
  defined the get_ and set_gps_location syscalls at positions 378 and 379

- flo-kernel/include/linux/fs.h
  added vfs_get_ and vfs_set_gps_location generic function wrappers and
  added them as operations under struct inode_operations

- flo-kernel/include/linux/gps.h
  header file that defines the gps_location struct and some helper function
  definitions

- flo-kernel/include/linux/magic.h
  changed EXT3_SUPER_MAGIC from 0xEF53 to 0xEF54, in order to reflect our
  fs modifications

- flo-kernel/kernel/Makefile
  added the gps.o binary to the compilation

- flo-kernel/kernel/gps.c
  implementation for the two syscalls get_ and set_gps_location, as well as
  a helper function get_location to get the location from within other
  portions of the kernel

- flo-kernel/fs/ext3/gps.c
  implementation of the ext3_set_ and ext3_get_gps_location functions to
  set and get the gps information to and from the inodes

- flo-kernel/fs/namei.c
  added vfs_get_ and vfs_set_gps_location function implementations, which
  are used for getting and setting the gps location into the inode through
  the inode->i_op->get_ and set_gps_location function pointers

- flo-kernel/fs/ext3/ext3.h
  added the i_latitude, i_longitude, i_accuracy, i_coord_age fields in the
  inode representation on disk, under ext3_inode, and the same four fields
  for the inode representation in memory under ext3_inode_info

- flo-kernel/fs/ext3/inode.c
  setting and getting gps location to/from disk from/to memory

- flo-kernel/fs/ext3/Makefile
  added flo-kernel/fs/ext3/gps.o to the compilation

- flo-kernel/fs/ext3/acl.c
  updating gps location on inode create or modify

- flo-kernel/fs/ext3/file.c
  updating gps location on inode create or modify

- flo-kernel/fs/ext3/ialloc.c
  updating gps location on inode create or modify

- flo-kernel/fs/ext3/ioctl.c
  updating gps location on inode create or modify

- flo-kernel/fs/ext3/namei.c
  updating gps location on inode create or modify
  and defined get_ and set_gps_location operations
  for inode_dir and inode_special

- flo-kernel/fs/ext3/super.c
  updating gps location on inode create or modify

- flo-kernel/fs/ext3/symlink.c
  updating gps location on inode create or modify
  and defined get_ and set_gps_location operations
  for inode_symlink and inode_fast_symlink

- flo-kernel/fs/ext3/xattr.c
  updating gps location on inode create or modify
  and defined get_ and set_gps_location operations
  for inode_dir and inode_special

- userspace/e2fsprogs/lib/ext2fs/ext2_fs.h
  modified this to reflect the ext3 fs changes from the kernel to include
  gps location

- userspace/file_loc/file_loc.c
  file_loc utility that takes a file path and prints the location information
  associated with the file, if available, and an error otherwise

- userspace/file_loc/file_loc.h
  header file defining the gps_location struct in userspace
			
- userspace/file_loc/fs_loopback.sh
  helper bash script for the loopback operation; in the tablet under /data/misc/

- userspace/gpsd/gpsd.c
  daemon that polls the GPS location and stores it in memory

- userspace/gpsd/gpsd.h
  header file defining the gps_location struct in userspace  


## HMWK6.FS DETAILS:

The file system contains 3 seperate folders with one file, one soft
link, and one hard link each. The first folder is tagged with GPS
info @New Jersey, the second with GPS info @Columbia (Mudd), and the
third with GPS info @Morning-side drive.


## RESOURCES USED:

1. Linux Cross Reference
   http://lxr.free-electrons.com
