The **free** command is used to analyze the system's memory usage. The following is an example of its usage:
```bash
$ free
		   total        used        free      shared  buff/cache   available
Mem:    15690272     4140688     6574004       53700     4975580    10896868
Swap:          0           0           0
```
- **total** is the total amount of physical RAM and swap space.
- **used** is the amount of RAM currently in use.
- **free** is the amount of RAM not in use.
- **shared** is the amount of RAM used by tmpfs.
- **buff/cache** is the amount of RAM used for the buffer and cache.
- **available** is an estimation on the amount of RAM available for starting new applications.
- **Mem** pertains to physical RAM.
- **Swap** pertains to swap space (virtual memory).
- All values are in bytes by default.
### Human-Readable Format (-h)
Using the **-h** option makes the output cleaner and easier to read:
```bash
$ free -h
            total        used        free      shared  buff/cache   available
Mem:         14Gi       3.4Gi       6.9Gi        39Mi       4.7Gi        10Gi
Swap:          0B          0B          0B
```
- Here, the numbers are scaled and appropriate unit suffixes are added, making the output much easier to understand at a glance.
- Values are displayed in binary units (Gi = gibibutes, Mi = mebibytes, where 1Gi = 1024Mi)
### Displaying in Megabytes (-m)
The **-m** option is used to display memory in megabytes:
```bash
$ free -m
            total        used        free      shared  buff/cache   available
Mem:        15322        3857        6742          45        4722       10834
Swap:           0           0           0
```
Note that mebibytes (MiB = 1,048,576 bytes) are not the same as megabytes (MB = 1,000,000 bytes) and should not be used interchangeably.
### Continuous Monitoring (-s)
We can use the **-s N** option (seconds) to make the display update every N seconds. The **-c N** option can be used to stop after N updates:
```bash
$ free -h -s 3 -c 5
          total        used        free      shared  buff/cache   available
Mem:       14Gi       3.2Gi       7.3Gi        44Mi       4.5Gi        11Gi
Swap:        0B          0B          0B

	      total        used        free      shared  buff/cache   available
Mem:       14Gi       3.2Gi       7.3Gi        44Mi       4.5Gi        11Gi
Swap:        0B          0B          0B

           total        used        free      shared  buff/cache   available
Mem:        14Gi       3.2Gi       7.3Gi        44Mi       4.5Gi        11Gi
Swap:         0B          0B          0B

           total        used        free      shared  buff/cache   available
Mem:        14Gi       3.2Gi       7.3Gi        44Mi       4.5Gi        11Gi
Swap:         0B          0B          0B

           total        used        free      shared  buff/cache   available
Mem:        14Gi       3.2Gi       7.3Gi        44Mi       4.5Gi        11Gi
Swap:         0B          0B          0B
```
- **-s 3** sets the update interval to three seconds.
- **-c 5** stops after five updates.
### Total Memory Usage (-t)
By default, the free command shows memory usage with buffers and cache separated. To see total memory usage including buffers and cache, we can use the **-t** option:
```bash
$ free -h -t
            total        used        free      shared  buff/cache   available
Mem:         14Gi       3.2Gi       7.2Gi        47Mi       4.5Gi        11Gi
Swap:          0B          0B          0B
Total:       14Gi       3.2Gi       7.2Gi
```
