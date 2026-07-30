The **df** command is used for viewing and monitoring disk space usage:
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