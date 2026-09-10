The **df** command (*disk free*) is used for viewing and monitoring disk space usage:
```bash
$ df
Filesystem     1K-blocks     Used Available Use% Mounted on
overlay         20971520   256724  20714796   2% /
tmpfs              65536        0     65536   0% /dev
shm                65536        0     65536   0% /dev/shm
/dev/nvme1n1   104792064 25661056  79131008  25% /etc/hosts
tmpfs             102400    51200     51200  50% /mnt/ramdisk
```
- **Filesystem**: name of device or partition.
- **1K-blocks**: shows the total size of the filesystem in blocks of 1 kilobyte.
- **Used**: how many 1K-blocks are used.
- **Available**: how many 1K-blocks are available.
- **Use%**: percentage of the filesystem used.
- **Mounted on**: where in the directory tree the filesystem is mounted.
- "overlay" is commonly used in containerized environments.
### Human-Readable Output (-h)
Using the **-h** option shows sizes in GB and MB, making the output easier to understand.
```bash
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
overlay          20G  251M   20G   2% /
tmpfs            64M     0   64M   0% /dev
shm              64M     0   64M   0% /dev/shm
/dev/nvme1n1    100G   20G   81G  20% /etc/hosts
tmpfs           100M   50M   50M  50% /mnt/ramdisk
```
### Filesystem Types (-T)
Using the **-T** option shows the type of each filesystem.
```bash
$ df -T
Filesystem     Type    1K-blocks     Used Available Use% Mounted on
overlay        overlay  20971520   256872  20714648   2% /
tmpfs          tmpfs       65536        0     65536   0% /dev
shm            tmpfs       65536        0     65536   0% /dev/shm
/dev/nvme1n1   xfs     104792064 20601256  84190808  20% /etc/hosts
tmpfs          tmpfs      102400    51200     51200  50% /mnt/ramdisk
```
A new column **Type** is added.
- **overlay** is a type of union filesystem often used in containerized environments.
- **tmpfs** is a temporary filesystem that resides in memory and/or swap partition.
- **xfs** is a high-performance journaling filesystem. Here it is used for the /etc/hosts mount.
### Inode Usage (-i)
In Linux, every file has an *inode*, which is a data structure that stores metadata about the file, such as permissions, ownership, and storage location. It's also possible to run out of inodes even when you have disk space available, preventing you from creating new files. We can monitor inode usage using the **-i** option:
```bash
$ df -i
Filesystem       Inodes  IUsed    IFree IUse% Mounted on
overlay        52428800 688585 51740215    2% /
tmpfs           1961283    213  1961070    1% /dev
shm             1961283      1  1961282    1% /dev/shm
/dev/nvme1n1   52428800 688585 51740215    2% /etc/hosts
tmpfs           1961283      2  1961281    1% /mnt/ramdisk
```
- **Inodes** shows the total number of inodes for each filesystem.
- **IUsed** shows the number of inodes currently in use.
- **IFree** shows the number of inodes not in use.
- **IUse%** shows the percentage of inodes used.
### Focusing on a Specific Filesystem
We can check the status of a specific filesystem by providing its mount point as an argument:
```bash
$ df -h /
Filesystem      Size  Used Avail Use% Mounted on
overlay          20G  251M   20G   2% /
```
Here we are checking the space on the root filesystem. 

We can also use **df -h /home** to check space in user home directories.
### Excluding Filesystem Types (-x)
To exclude certain filesystem types (e.g. temporary filesystems) we can use the **-x** option along with specifying the filesystem type as an argument:
```bash
$ df -h -x tmpfs
Filesystem      Size  Used Avail Use% Mounted on
overlay          20G  251M   20G   2% /
/dev/nvme1n1    100G   22G   78G  22% /etc/hosts
```
### Total Summary (--total)
We can display a total summary row using the **--total** option:
```bash
$ df -h --total
Filesystem      Size  Used Avail Use% Mounted on
overlay          20G  251M   20G   2% /
tmpfs            64M     0   64M   0% /dev
shm              64M     0   64M   0% /dev/shm
/dev/nvme1n1    100G   22G   78G  22% /etc/hosts
tmpfs           100M   50M   50M  50% /mnt/ramdisk
total           121G   23G   98G  19% -
```
The last line shows the total across all filesystems.
### Other Options
- **-a**: show all filesystems, including pseudo, duplicate, and inaccessible ones.
- **-k**: display all sizes in kilobytes (the default).
- **-m**: display all sizes in megabytes.
- **-P**: use POSIX output format.
- **--sync**: invoke sync before getting usage info.
- **-t**: limit listing to filesystems of a specific type.