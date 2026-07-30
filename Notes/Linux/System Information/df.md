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
